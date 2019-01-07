---
title: Archivos estáticos en ASP.NET Core
author: rick-anderson
description: Aprenda a proporcionar y proteger los archivos estáticos y a configurar los comportamientos de middleware de hospedaje de archivos estáticos en una aplicación web de ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: fundamentals/static-files
ms.openlocfilehash: 4c08d65cc1f658ef08a9b4b362ac7f8a3a243557
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637786"
---
# <a name="static-files-in-aspnet-core"></a><span data-ttu-id="2f0de-103">Archivos estáticos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2f0de-103">Static files in ASP.NET Core</span></span>

<span data-ttu-id="2f0de-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="2f0de-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="2f0de-105">Los archivos estáticos, como HTML, CSS, imágenes y JavaScript, son activos que una aplicación de ASP.NET Core proporciona directamente a los clientes.</span><span class="sxs-lookup"><span data-stu-id="2f0de-105">Static files, such as HTML, CSS, images, and JavaScript, are assets an ASP.NET Core app serves directly to clients.</span></span> <span data-ttu-id="2f0de-106">Se necesita alguna configuración para habilitar el servicio de estos archivos.</span><span class="sxs-lookup"><span data-stu-id="2f0de-106">Some configuration is required to enable serving of these files.</span></span>

<span data-ttu-id="2f0de-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2f0de-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="2f0de-108">Proporcionar archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="2f0de-108">Serve static files</span></span>

<span data-ttu-id="2f0de-109">Los archivos estáticos se almacenan en el directorio raíz de la Web del proyecto.</span><span class="sxs-lookup"><span data-stu-id="2f0de-109">Static files are stored within your project's web root directory.</span></span> <span data-ttu-id="2f0de-110">El directorio predeterminado es *\<content_root>/wwwroot*, pero puede cambiarse a través del método [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_).</span><span class="sxs-lookup"><span data-stu-id="2f0de-110">The default directory is *\<content_root>/wwwroot*, but it can be changed via the [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) method.</span></span> <span data-ttu-id="2f0de-111">Vea [Raíz del contenido](xref:fundamentals/index#content-root) y [Raíz web](xref:fundamentals/index#web-root-webroot) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="2f0de-111">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root-webroot) for more information.</span></span>

<span data-ttu-id="2f0de-112">El host de web de la aplicación debe tener conocimiento del directorio raíz del contenido.</span><span class="sxs-lookup"><span data-stu-id="2f0de-112">The app's web host must be made aware of the content root directory.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2f0de-113">El método `WebHost.CreateDefaultBuilder` establece la raíz de contenido en el directorio actual:</span><span class="sxs-lookup"><span data-stu-id="2f0de-113">The `WebHost.CreateDefaultBuilder` method sets the content root to the current directory:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2f0de-114">Establezca la raíz de contenido en el directorio actual invocando a [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) dentro de `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="2f0de-114">Set the content root to the current directory by invoking [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) inside of `Program.Main`:</span></span>

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

::: moniker-end

<span data-ttu-id="2f0de-115">Se puede acceder a los archivos estáticos a través de una ruta de acceso relativa a la raíz web.</span><span class="sxs-lookup"><span data-stu-id="2f0de-115">Static files are accessible via a path relative to the web root.</span></span> <span data-ttu-id="2f0de-116">Por ejemplo, la plantilla de proyecto **Aplicación web** contiene varias carpetas dentro de la carpeta *wwwroot*:</span><span class="sxs-lookup"><span data-stu-id="2f0de-116">For example, the **Web Application** project template contains several folders within the *wwwroot* folder:</span></span>

* <span data-ttu-id="2f0de-117">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="2f0de-117">**wwwroot**</span></span>
  * <span data-ttu-id="2f0de-118">**css**</span><span class="sxs-lookup"><span data-stu-id="2f0de-118">**css**</span></span>
  * <span data-ttu-id="2f0de-119">**images**</span><span class="sxs-lookup"><span data-stu-id="2f0de-119">**images**</span></span>
  * <span data-ttu-id="2f0de-120">**js**</span><span class="sxs-lookup"><span data-stu-id="2f0de-120">**js**</span></span>

<span data-ttu-id="2f0de-121">El formato de URI para acceder a un archivo en la subcarpeta *images* es *http://\<dirección_servidor>/images/\<nombre_archivo_imagen>*.</span><span class="sxs-lookup"><span data-stu-id="2f0de-121">The URI format to access a file in the *images* subfolder is *http://\<server_address>/images/\<image_file_name>*.</span></span> <span data-ttu-id="2f0de-122">Por ejemplo, *http://localhost:9189/images/banner3.svg*.</span><span class="sxs-lookup"><span data-stu-id="2f0de-122">For example, *http://localhost:9189/images/banner3.svg*.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2f0de-123">Si el destino es .NET Framework, agregue el paquete [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) al proyecto.</span><span class="sxs-lookup"><span data-stu-id="2f0de-123">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span> <span data-ttu-id="2f0de-124">Si el destino es .NET Core, el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) incluye este paquete.</span><span class="sxs-lookup"><span data-stu-id="2f0de-124">If targeting .NET Core, the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) includes this package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="2f0de-125">Si el destino es .NET Framework, agregue el paquete [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) al proyecto.</span><span class="sxs-lookup"><span data-stu-id="2f0de-125">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span> <span data-ttu-id="2f0de-126">Si el destino es .NET Core, el [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage) incluye este paquete.</span><span class="sxs-lookup"><span data-stu-id="2f0de-126">If targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) includes this package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2f0de-127">Agregue el paquete [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) al proyecto.</span><span class="sxs-lookup"><span data-stu-id="2f0de-127">Add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span>

::: moniker-end

<span data-ttu-id="2f0de-128">Configure el [middleware](xref:fundamentals/middleware/index), que permite proporcionar archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="2f0de-128">Configure the [middleware](xref:fundamentals/middleware/index) which enables the serving of static files.</span></span>

### <a name="serve-files-inside-of-web-root"></a><span data-ttu-id="2f0de-129">Proporcionar archivos dentro de la raíz web</span><span class="sxs-lookup"><span data-stu-id="2f0de-129">Serve files inside of web root</span></span>

<span data-ttu-id="2f0de-130">Invoque el método [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) dentro de `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="2f0de-130">Invoke the [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method within `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

<span data-ttu-id="2f0de-131">La sobrecarga del método sin parámetros `UseStaticFiles` marca los archivos en la raíz web como que se pueden proporcionar.</span><span class="sxs-lookup"><span data-stu-id="2f0de-131">The parameterless `UseStaticFiles` method overload marks the files in web root as servable.</span></span> <span data-ttu-id="2f0de-132">El siguiente marcado hace referencia a *wwwroot/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="2f0de-132">The following markup references *wwwroot/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

<span data-ttu-id="2f0de-133">En el código anterior, el carácter de tilde de la ñ `~/` apunta a la raíz web.</span><span class="sxs-lookup"><span data-stu-id="2f0de-133">In the preceding code, the tilde character `~/` points to webroot.</span></span> <span data-ttu-id="2f0de-134">Para obtener más información, vea [Raíz web](xref:fundamentals/index#web-root-webroot).</span><span class="sxs-lookup"><span data-stu-id="2f0de-134">For more information, see [Web root](xref:fundamentals/index#web-root-webroot).</span></span>

### <a name="serve-files-outside-of-web-root"></a><span data-ttu-id="2f0de-135">Proporcionar archivos fuera de la raíz web</span><span class="sxs-lookup"><span data-stu-id="2f0de-135">Serve files outside of web root</span></span>

<span data-ttu-id="2f0de-136">Considere una jerarquía de directorios en la que residen fuera de la raíz web los archivos estáticos que se van a proporcionar:</span><span class="sxs-lookup"><span data-stu-id="2f0de-136">Consider a directory hierarchy in which the static files to be served reside outside of the web root:</span></span>

* <span data-ttu-id="2f0de-137">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="2f0de-137">**wwwroot**</span></span>
  * <span data-ttu-id="2f0de-138">**css**</span><span class="sxs-lookup"><span data-stu-id="2f0de-138">**css**</span></span>
  * <span data-ttu-id="2f0de-139">**images**</span><span class="sxs-lookup"><span data-stu-id="2f0de-139">**images**</span></span>
  * <span data-ttu-id="2f0de-140">**js**</span><span class="sxs-lookup"><span data-stu-id="2f0de-140">**js**</span></span>
* <span data-ttu-id="2f0de-141">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="2f0de-141">**MyStaticFiles**</span></span>
  * <span data-ttu-id="2f0de-142">**images**</span><span class="sxs-lookup"><span data-stu-id="2f0de-142">**images**</span></span>
      * <span data-ttu-id="2f0de-143">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="2f0de-143">*banner1.svg*</span></span>

<span data-ttu-id="2f0de-144">Una solicitud puede acceder al archivo *banner1.svg* configurando el middleware de archivos estáticos como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="2f0de-144">A request can access the *banner1.svg* file by configuring the Static File Middleware as follows:</span></span>

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

<span data-ttu-id="2f0de-145">En el código anterior, la jerarquía del directorio *MyStaticFiles* se expone públicamente a través del segmento de URI *StaticFiles*.</span><span class="sxs-lookup"><span data-stu-id="2f0de-145">In the preceding code, the *MyStaticFiles* directory hierarchy is exposed publicly via the *StaticFiles* URI segment.</span></span> <span data-ttu-id="2f0de-146">Una solicitud a *http://\<dirección_servidor>/StaticFiles/images/banner1.svg* proporciona el archivo *banner1.svg*.</span><span class="sxs-lookup"><span data-stu-id="2f0de-146">A request to *http://\<server_address>/StaticFiles/images/banner1.svg* serves the *banner1.svg* file.</span></span>

<span data-ttu-id="2f0de-147">El siguiente marcado hace referencia a *MyStaticFiles/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="2f0de-147">The following markup references *MyStaticFiles/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a><span data-ttu-id="2f0de-148">Establecer encabezados de respuesta HTTP</span><span class="sxs-lookup"><span data-stu-id="2f0de-148">Set HTTP response headers</span></span>

<span data-ttu-id="2f0de-149">Se puede usar un objeto [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) para establecer encabezados de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="2f0de-149">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) object can be used to set HTTP response headers.</span></span> <span data-ttu-id="2f0de-150">Además de configurar el servicio de archivos estáticos desde la raíz web, el código siguiente establece el encabezado `Cache-Control`:</span><span class="sxs-lookup"><span data-stu-id="2f0de-150">In addition to configuring static file serving from the web root, the following code sets the `Cache-Control` header:</span></span>

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="2f0de-151">El método [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) existe en el paquete [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/).</span><span class="sxs-lookup"><span data-stu-id="2f0de-151">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method exists in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

<span data-ttu-id="2f0de-152">Los archivos se han hecho públicamente almacenables en caché durante 10 minutos (600 segundos) en el entorno de desarrollo:</span><span class="sxs-lookup"><span data-stu-id="2f0de-152">The files have been made publicly cacheable for 10 minutes (600 seconds) in the Development environment:</span></span>

![Se han agregado encabezados de respuesta que muestran el encabezado Cache-Control](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="2f0de-154">Autorización de archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="2f0de-154">Static file authorization</span></span>

<span data-ttu-id="2f0de-155">El middleware de archivos estáticos no proporciona comprobaciones de autorización.</span><span class="sxs-lookup"><span data-stu-id="2f0de-155">The Static File Middleware doesn't provide authorization checks.</span></span> <span data-ttu-id="2f0de-156">Los archivos que proporciona, incluidos los de *wwwroot*, están accesibles de forma pública.</span><span class="sxs-lookup"><span data-stu-id="2f0de-156">Any files served by it, including those under *wwwroot*, are publicly accessible.</span></span> <span data-ttu-id="2f0de-157">Para proporcionar archivos según su autorización:</span><span class="sxs-lookup"><span data-stu-id="2f0de-157">To serve files based on authorization:</span></span>

* <span data-ttu-id="2f0de-158">Almacénelos fuera de *wwwroot* y cualquier directorio al que el middleware de archivos estáticos tenga acceso.</span><span class="sxs-lookup"><span data-stu-id="2f0de-158">Store them outside of *wwwroot* and any directory accessible to the Static File Middleware.</span></span>
* <span data-ttu-id="2f0de-159">Proporciónelos a través de un método de acción al que se aplica la autorización.</span><span class="sxs-lookup"><span data-stu-id="2f0de-159">Serve them via an action method to which authorization is applied.</span></span> <span data-ttu-id="2f0de-160">Devuelva un objeto [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult):</span><span class="sxs-lookup"><span data-stu-id="2f0de-160">Return a [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) object:</span></span>

  [!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a><span data-ttu-id="2f0de-161">Habilite el examen de directorios</span><span class="sxs-lookup"><span data-stu-id="2f0de-161">Enable directory browsing</span></span>

<span data-ttu-id="2f0de-162">El examen de directorios permite a los usuarios de su aplicación web ver una lista de directorios y archivos contenidos en un directorio especificado.</span><span class="sxs-lookup"><span data-stu-id="2f0de-162">Directory browsing allows users of your web app to see a directory listing and files within a specified directory.</span></span> <span data-ttu-id="2f0de-163">Por motivos de seguridad, el examen de directorios está deshabilitado de forma predeterminada (consulte [Consideraciones](#considerations)).</span><span class="sxs-lookup"><span data-stu-id="2f0de-163">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="2f0de-164">Habilitar el examen de directorios invocando el método [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) en `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="2f0de-164">Enable directory browsing by invoking the [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) method in `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

<span data-ttu-id="2f0de-165">Agregar servicios requeridos invocando el método [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) desde `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="2f0de-165">Add required services by invoking the [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

<span data-ttu-id="2f0de-166">El código anterior permite el examen de directorios de la carpeta *wwwroot/images* usando la dirección URL *http://\<dirección_servidor>/MyImages*, con vínculos a cada archivo y carpeta:</span><span class="sxs-lookup"><span data-stu-id="2f0de-166">The preceding code allows directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*, with links to each file and folder:</span></span>

![examen de directorios](static-files/_static/dir-browse.png)

<span data-ttu-id="2f0de-168">Vea [consideraciones](#considerations) sobre los riesgos de seguridad al habilitar el examen.</span><span class="sxs-lookup"><span data-stu-id="2f0de-168">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="2f0de-169">Tenga en cuenta las dos llamadas a `UseStaticFiles` en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="2f0de-169">Note the two `UseStaticFiles` calls in the following example.</span></span> <span data-ttu-id="2f0de-170">La primera llamada permite proporcionar archivos estáticos en la carpeta *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="2f0de-170">The first call enables the serving of static files in the *wwwroot* folder.</span></span> <span data-ttu-id="2f0de-171">La segunda llamada habilita el examen de directorios de la carpeta *wwwroot/images* usando la dirección URL *http://\<dirección_servidor>/MyImages*:</span><span class="sxs-lookup"><span data-stu-id="2f0de-171">The second call enables directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a><span data-ttu-id="2f0de-172">Proporcionar un documento predeterminado</span><span class="sxs-lookup"><span data-stu-id="2f0de-172">Serve a default document</span></span>

<span data-ttu-id="2f0de-173">Establecer una página principal predeterminada proporciona a los visitantes un punto de partida lógico cuando visitan su sitio.</span><span class="sxs-lookup"><span data-stu-id="2f0de-173">Setting a default home page provides visitors a logical starting point when visiting your site.</span></span> <span data-ttu-id="2f0de-174">Para proporcionar una página predeterminada sin que el usuario cumpla por completo los requisitos del URI, llame al método [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) desde `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="2f0de-174">To serve a default page without the user fully qualifying the URI, call the [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method from `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> <span data-ttu-id="2f0de-175">Debe llamarse a `UseDefaultFiles` antes de a `UseStaticFiles` para proporcionar el archivo predeterminado.</span><span class="sxs-lookup"><span data-stu-id="2f0de-175">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="2f0de-176">`UseDefaultFiles` es un sistema de reescritura de direcciones URL que no proporciona realmente el archivo.</span><span class="sxs-lookup"><span data-stu-id="2f0de-176">`UseDefaultFiles` is a URL rewriter that doesn't actually serve the file.</span></span> <span data-ttu-id="2f0de-177">Habilite el middleware de archivos estáticos a través de `UseStaticFiles` para proporcionar el archivo.</span><span class="sxs-lookup"><span data-stu-id="2f0de-177">Enable Static File Middleware via `UseStaticFiles` to serve the file.</span></span>

<span data-ttu-id="2f0de-178">Con `UseDefaultFiles`, las solicitudes a una carpeta buscan:</span><span class="sxs-lookup"><span data-stu-id="2f0de-178">With `UseDefaultFiles`, requests to a folder search for:</span></span>

* <span data-ttu-id="2f0de-179">*default.htm*</span><span class="sxs-lookup"><span data-stu-id="2f0de-179">*default.htm*</span></span>
* <span data-ttu-id="2f0de-180">*default.html*</span><span class="sxs-lookup"><span data-stu-id="2f0de-180">*default.html*</span></span>
* <span data-ttu-id="2f0de-181">*index.htm*</span><span class="sxs-lookup"><span data-stu-id="2f0de-181">*index.htm*</span></span>
* <span data-ttu-id="2f0de-182">*index.html*</span><span class="sxs-lookup"><span data-stu-id="2f0de-182">*index.html*</span></span>

<span data-ttu-id="2f0de-183">El primer archivo que se encuentra en la lista se proporciona como si la solicitud fuera el URI completo.</span><span class="sxs-lookup"><span data-stu-id="2f0de-183">The first file found from the list is served as though the request were the fully qualified URI.</span></span> <span data-ttu-id="2f0de-184">La dirección URL del explorador sigue reflejando el URI solicitado.</span><span class="sxs-lookup"><span data-stu-id="2f0de-184">The browser URL continues to reflect the URI requested.</span></span>

<span data-ttu-id="2f0de-185">El código siguiente cambia el nombre de archivo predeterminado a *mydefault.html*:</span><span class="sxs-lookup"><span data-stu-id="2f0de-185">The following code changes the default file name to *mydefault.html*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a><span data-ttu-id="2f0de-186">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="2f0de-186">UseFileServer</span></span>

<span data-ttu-id="2f0de-187">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combina la funcionalidad de `UseStaticFiles`, `UseDefaultFiles` y `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="2f0de-187">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="2f0de-188">El código siguiente permite proporcionar archivos estáticos y el archivo predeterminado.</span><span class="sxs-lookup"><span data-stu-id="2f0de-188">The following code enables the serving of static files and the default file.</span></span> <span data-ttu-id="2f0de-189">El examen de directorios no está habilitado.</span><span class="sxs-lookup"><span data-stu-id="2f0de-189">Directory browsing isn't enabled.</span></span>

```csharp
app.UseFileServer();
```

<span data-ttu-id="2f0de-190">El código siguiente refuerza la sobrecarga sin parámetros habilitando el examen de directorios:</span><span class="sxs-lookup"><span data-stu-id="2f0de-190">The following code builds upon the parameterless overload by enabling directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

<span data-ttu-id="2f0de-191">Tenga en cuenta la siguiente jerarquía de directorios:</span><span class="sxs-lookup"><span data-stu-id="2f0de-191">Consider the following directory hierarchy:</span></span>

* <span data-ttu-id="2f0de-192">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="2f0de-192">**wwwroot**</span></span>
  * <span data-ttu-id="2f0de-193">**css**</span><span class="sxs-lookup"><span data-stu-id="2f0de-193">**css**</span></span>
  * <span data-ttu-id="2f0de-194">**images**</span><span class="sxs-lookup"><span data-stu-id="2f0de-194">**images**</span></span>
  * <span data-ttu-id="2f0de-195">**js**</span><span class="sxs-lookup"><span data-stu-id="2f0de-195">**js**</span></span>
* <span data-ttu-id="2f0de-196">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="2f0de-196">**MyStaticFiles**</span></span>
  * <span data-ttu-id="2f0de-197">**images**</span><span class="sxs-lookup"><span data-stu-id="2f0de-197">**images**</span></span>
      * <span data-ttu-id="2f0de-198">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="2f0de-198">*banner1.svg*</span></span>
  * <span data-ttu-id="2f0de-199">*default.html*</span><span class="sxs-lookup"><span data-stu-id="2f0de-199">*default.html*</span></span>

<span data-ttu-id="2f0de-200">El código siguiente permite los archivos estáticos, los archivos predeterminados y el examen de directorios de `MyStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="2f0de-200">The following code enables static files, default files, and directory browsing of `MyStaticFiles`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

<span data-ttu-id="2f0de-201">Se debe llamar a `AddDirectoryBrowser` cuando el valor de la propiedad `EnableDirectoryBrowsing` es `true`:</span><span class="sxs-lookup"><span data-stu-id="2f0de-201">`AddDirectoryBrowser` must be called when the `EnableDirectoryBrowsing` property value is `true`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

<span data-ttu-id="2f0de-202">Al usar la jerarquía de archivos y el código anterior, las direcciones URL se resuelven como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="2f0de-202">Using the file hierarchy and preceding code, URLs resolve as follows:</span></span>

| <span data-ttu-id="2f0de-203">Identificador URI</span><span class="sxs-lookup"><span data-stu-id="2f0de-203">URI</span></span>            |                             <span data-ttu-id="2f0de-204">Respuesta</span><span class="sxs-lookup"><span data-stu-id="2f0de-204">Response</span></span>  |
| ------- | ------|
| <span data-ttu-id="2f0de-205">*http://\<dirección_servidor>/StaticFiles/images/banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="2f0de-205">*http://\<server_address>/StaticFiles/images/banner1.svg*</span></span>    |      <span data-ttu-id="2f0de-206">MyStaticFiles/images/banner1.svg</span><span class="sxs-lookup"><span data-stu-id="2f0de-206">MyStaticFiles/images/banner1.svg</span></span> |
| <span data-ttu-id="2f0de-207">*http://\<dirección_servidor>/StaticFiles*</span><span class="sxs-lookup"><span data-stu-id="2f0de-207">*http://\<server_address>/StaticFiles*</span></span>             |     <span data-ttu-id="2f0de-208">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="2f0de-208">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="2f0de-209">Si no existe ningún archivo con el nombre predeterminado en el directorio *MyStaticFiles*, *http://\<dirección_servidor>/StaticFiles* devuelve la lista de directorios con vínculos activos:</span><span class="sxs-lookup"><span data-stu-id="2f0de-209">If no default-named file exists in the *MyStaticFiles* directory, *http://\<server_address>/StaticFiles* returns the directory listing with clickable links:</span></span>

![Lista de archivos estáticos](static-files/_static/db2.png)

> [!NOTE]
> <span data-ttu-id="2f0de-211">`UseDefaultFiles` y `UseDirectoryBrowser` usan la dirección URL *http://\<dirección_servidor>/StaticFiles* sin la barra diagonal final para desencadenar un redireccionamiento del lado cliente a *http://\<dirección_servidor>/StaticFiles/*.</span><span class="sxs-lookup"><span data-stu-id="2f0de-211">`UseDefaultFiles` and `UseDirectoryBrowser` use the URL *http://\<server_address>/StaticFiles* without the trailing slash to trigger a client-side redirect to *http://\<server_address>/StaticFiles/*.</span></span> <span data-ttu-id="2f0de-212">Tenga en cuenta la adición de la barra diagonal final.</span><span class="sxs-lookup"><span data-stu-id="2f0de-212">Notice the addition of the trailing slash.</span></span> <span data-ttu-id="2f0de-213">Las direcciones URL relativas dentro de los documentos se consideran no válidas sin una barra diagonal final.</span><span class="sxs-lookup"><span data-stu-id="2f0de-213">Relative URLs within the documents are deemed invalid without a trailing slash.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="2f0de-214">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="2f0de-214">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="2f0de-215">La clase [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) contiene una propiedad `Mappings` que actúa como una asignación de extensiones de archivo para tipos de contenido MIME.</span><span class="sxs-lookup"><span data-stu-id="2f0de-215">The [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) class contains a `Mappings` property serving as a mapping of file extensions to MIME content types.</span></span> <span data-ttu-id="2f0de-216">En el ejemplo siguiente, se registran varias extensiones de archivo a los tipos MIME conocidos.</span><span class="sxs-lookup"><span data-stu-id="2f0de-216">In the following sample, several file extensions are registered to known MIME types.</span></span> <span data-ttu-id="2f0de-217">Se reemplaza la extensión *.rtf* y se quita *.mp4*.</span><span class="sxs-lookup"><span data-stu-id="2f0de-217">The *.rtf* extension is replaced, and *.mp4* is removed.</span></span>

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

<span data-ttu-id="2f0de-218">Vea [Tipos de contenido MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).</span><span class="sxs-lookup"><span data-stu-id="2f0de-218">See [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="2f0de-219">Tipos de contenido no estándar</span><span class="sxs-lookup"><span data-stu-id="2f0de-219">Non-standard content types</span></span>

<span data-ttu-id="2f0de-220">El middleware de archivos estáticos entiende casi 400 tipos de contenido de archivo conocidos.</span><span class="sxs-lookup"><span data-stu-id="2f0de-220">Static File Middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="2f0de-221">Si el usuario solicita un archivo con un tipo de archivo desconocido, el middleware de archivos estáticos pasa la solicitud al siguiente middleware de la canalización.</span><span class="sxs-lookup"><span data-stu-id="2f0de-221">If the user requests a file with an unknown file type, Static File Middleware passes the request to the next middleware in the pipeline.</span></span> <span data-ttu-id="2f0de-222">Si ningún middleware se ocupa de la solicitud, se devuelve una respuesta *404 No encontrado*.</span><span class="sxs-lookup"><span data-stu-id="2f0de-222">If no middleware handles the request, a *404 Not Found* response is returned.</span></span> <span data-ttu-id="2f0de-223">Si se habilita la exploración de directorios, se muestra un vínculo al archivo en una lista de directorios.</span><span class="sxs-lookup"><span data-stu-id="2f0de-223">If directory browsing is enabled, a link to the file is displayed in a directory listing.</span></span>

<span data-ttu-id="2f0de-224">El código siguiente permite proporcionar tipos desconocidos y procesa el archivo desconocido como una imagen:</span><span class="sxs-lookup"><span data-stu-id="2f0de-224">The following code enables serving unknown types and renders the unknown file as an image:</span></span>

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="2f0de-225">Con el código anterior, una solicitud para un archivo con un tipo de contenido desconocido se devuelve como una imagen.</span><span class="sxs-lookup"><span data-stu-id="2f0de-225">With the preceding code, a request for a file with an unknown content type is returned as an image.</span></span>

> [!WARNING]
> <span data-ttu-id="2f0de-226">Habilitar [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) supone un riesgo para la seguridad.</span><span class="sxs-lookup"><span data-stu-id="2f0de-226">Enabling [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) is a security risk.</span></span> <span data-ttu-id="2f0de-227">Está deshabilitado de forma predeterminada y no se recomienda su uso.</span><span class="sxs-lookup"><span data-stu-id="2f0de-227">It's disabled by default, and its use is discouraged.</span></span> <span data-ttu-id="2f0de-228">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) proporciona una alternativa más segura a ofrecer archivos con extensiones no estándar.</span><span class="sxs-lookup"><span data-stu-id="2f0de-228">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="2f0de-229">Consideraciones</span><span class="sxs-lookup"><span data-stu-id="2f0de-229">Considerations</span></span>

> [!WARNING]
> <span data-ttu-id="2f0de-230">`UseDirectoryBrowser` y `UseStaticFiles` pueden producir pérdidas de información confidencial.</span><span class="sxs-lookup"><span data-stu-id="2f0de-230">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="2f0de-231">Se recomienda deshabilitar el examen de directorios en producción.</span><span class="sxs-lookup"><span data-stu-id="2f0de-231">Disabling directory browsing in production is highly recommended.</span></span> <span data-ttu-id="2f0de-232">Revise cuidadosamente los directorios que se habilitan mediante `UseStaticFiles` o `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="2f0de-232">Carefully review which directories are enabled via `UseStaticFiles` or `UseDirectoryBrowser`.</span></span> <span data-ttu-id="2f0de-233">Todo el directorio y sus subdirectorios pasan a ser accesibles públicamente.</span><span class="sxs-lookup"><span data-stu-id="2f0de-233">The entire directory and its sub-directories become publicly accessible.</span></span> <span data-ttu-id="2f0de-234">Almacene los archivos adecuados para proporcionarlos al público en un directorio dedicado, como *\<raíz_contenido>/wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="2f0de-234">Store files suitable for serving to the public in a dedicated directory, such as *\<content_root>/wwwroot*.</span></span> <span data-ttu-id="2f0de-235">Separe estos archivos de las vistas MVC, las páginas de Razor (solo 2.x), los archivos de configuración, etc.</span><span class="sxs-lookup"><span data-stu-id="2f0de-235">Separate these files from MVC views, Razor Pages (2.x only), configuration files, etc.</span></span>

* <span data-ttu-id="2f0de-236">Las direcciones URL para el contenido que se expone a través de `UseDirectoryBrowser` y `UseStaticFiles` están sujetas a la distinción entre mayúsculas y minúsculas, y a restricciones de caracteres del sistema de archivos subyacente.</span><span class="sxs-lookup"><span data-stu-id="2f0de-236">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of the underlying file system.</span></span> <span data-ttu-id="2f0de-237">Por ejemplo, Windows no distingue entre mayúsculas y minúsculas, pero macOS y Linux sí.</span><span class="sxs-lookup"><span data-stu-id="2f0de-237">For example, Windows is case insensitive&mdash;macOS and Linux aren't.</span></span>

* <span data-ttu-id="2f0de-238">Las aplicaciones de ASP.NET Core hospedadas en IIS usan el [módulo de ASP.NET Core](xref:host-and-deploy/aspnet-core-module) para reenviar todas las solicitudes a la aplicación, incluidas las solicitudes de archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="2f0de-238">ASP.NET Core apps hosted in IIS use the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to forward all requests to the app, including static file requests.</span></span> <span data-ttu-id="2f0de-239">No se usa el controlador de archivos estáticos de IIS.</span><span class="sxs-lookup"><span data-stu-id="2f0de-239">The IIS static file handler isn't used.</span></span> <span data-ttu-id="2f0de-240">No tiene ninguna posibilidad de controlar las solicitudes antes de que las controle el módulo.</span><span class="sxs-lookup"><span data-stu-id="2f0de-240">It has no chance to handle requests before they're handled by the module.</span></span>

* <span data-ttu-id="2f0de-241">Complete los pasos siguientes en el Administrador de IIS para quitar el controlador de archivos estáticos de IIS en el nivel de servidor o de sitio web:</span><span class="sxs-lookup"><span data-stu-id="2f0de-241">Complete the following steps in IIS Manager to remove the IIS static file handler at the server or website level:</span></span>
    1. <span data-ttu-id="2f0de-242">Navegue hasta la característica **Módulos**.</span><span class="sxs-lookup"><span data-stu-id="2f0de-242">Navigate to the **Modules** feature.</span></span>
    1. <span data-ttu-id="2f0de-243">En la lista, seleccione **StaticFileModule**.</span><span class="sxs-lookup"><span data-stu-id="2f0de-243">Select **StaticFileModule** in the list.</span></span>
    1. <span data-ttu-id="2f0de-244">Haga clic en **Quitar** en la barra lateral **Acciones**.</span><span class="sxs-lookup"><span data-stu-id="2f0de-244">Click **Remove** in the **Actions** sidebar.</span></span>

> [!WARNING]
> <span data-ttu-id="2f0de-245">Si el controlador de archivos estáticos de IIS está habilitado **y** el módulo de ASP.NET Core no está configurado correctamente, se proporcionan archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="2f0de-245">If the IIS static file handler is enabled **and** the ASP.NET Core Module is configured incorrectly, static files are served.</span></span> <span data-ttu-id="2f0de-246">Esto sucede, por ejemplo, si el archivo *web.config* no está implementado.</span><span class="sxs-lookup"><span data-stu-id="2f0de-246">This happens, for example, if the *web.config* file isn't deployed.</span></span>

* <span data-ttu-id="2f0de-247">Coloque los archivos de código (incluidos *.cs* y *.cshtml*) fuera de la raíz web del proyecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2f0de-247">Place code files (including *.cs* and *.cshtml*) outside of the app project's web root.</span></span> <span data-ttu-id="2f0de-248">Por lo tanto, se crea una separación lógica entre el contenido del lado cliente de la aplicación y el código basado en servidor.</span><span class="sxs-lookup"><span data-stu-id="2f0de-248">A logical separation is therefore created between the app's client-side content and server-based code.</span></span> <span data-ttu-id="2f0de-249">Esto impide que se filtre el código del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="2f0de-249">This prevents server-side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2f0de-250">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="2f0de-250">Additional resources</span></span>

* [<span data-ttu-id="2f0de-251">Middleware</span><span class="sxs-lookup"><span data-stu-id="2f0de-251">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="2f0de-252">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2f0de-252">Introduction to ASP.NET Core</span></span>](xref:index)
