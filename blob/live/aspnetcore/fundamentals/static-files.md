---
title: "Trabajar con archivos estáticos en ASP.NET Core"
author: rick-anderson
description: "Aprenda a servir y proteger archivos estáticos y configurar archivos estáticos hospedaje comportamientos de middleware en una aplicación web de ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: 60b205bf0a45e2965f9dab46f46956947ae513fd
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="work-with-static-files-in-aspnet-core"></a><span data-ttu-id="803a0-103">Trabajar con archivos estáticos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="803a0-103">Work with static files in ASP.NET Core</span></span>

<span data-ttu-id="803a0-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="803a0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="803a0-105">Los archivos estáticos, como HTML, CSS, imágenes y JavaScript, están activos de que una aplicación de ASP.NET Core sirve directamente a los clientes.</span><span class="sxs-lookup"><span data-stu-id="803a0-105">Static files, such as HTML, CSS, images, and JavaScript, are assets an ASP.NET Core app serves directly to clients.</span></span> <span data-ttu-id="803a0-106">Alguna configuración es necesaria para habilitar el servicio de estos archivos.</span><span class="sxs-lookup"><span data-stu-id="803a0-106">Some configuration is required to enable to serving of these files.</span></span>

<span data-ttu-id="803a0-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="803a0-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="803a0-108">Servir archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="803a0-108">Serve static files</span></span>

<span data-ttu-id="803a0-109">Archivos estáticos se almacenan en el directorio del proyecto web raíz.</span><span class="sxs-lookup"><span data-stu-id="803a0-109">Static files are stored within your project's web root directory.</span></span> <span data-ttu-id="803a0-110">El directorio predeterminado es  *\<content_root > / wwwroot*, pero puede cambiarse a través de la [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) método.</span><span class="sxs-lookup"><span data-stu-id="803a0-110">The default directory is *\<content_root>/wwwroot*, but it can be changed via the [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) method.</span></span> <span data-ttu-id="803a0-111">Vea [raíz del contenido](xref:fundamentals/index#content-root) y [raíz Web](xref:fundamentals/index#web-root) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="803a0-111">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span>

<span data-ttu-id="803a0-112">Host de la aplicación web debe se conocen la existencia del directorio raíz del contenido.</span><span class="sxs-lookup"><span data-stu-id="803a0-112">The app's web host must be made aware of the content root directory.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="803a0-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="803a0-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="803a0-114">El `WebHost.CreateDefaultBuilder` método establece la raíz de contenido en el directorio actual:</span><span class="sxs-lookup"><span data-stu-id="803a0-114">The `WebHost.CreateDefaultBuilder` method sets the content root to the current directory:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="803a0-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="803a0-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="803a0-116">Establece el directorio raíz contenida en el directorio actual mediante la invocación de [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) dentro de `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="803a0-116">Set the content root to the current directory by invoking [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) inside of `Program.Main`:</span></span>

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

<span data-ttu-id="803a0-117">Archivos estáticos son accesibles a través de una ruta de acceso relativa a la raíz de web.</span><span class="sxs-lookup"><span data-stu-id="803a0-117">Static files are accessible via a path relative to the web root.</span></span> <span data-ttu-id="803a0-118">Por ejemplo, el **aplicación Web** plantilla de proyecto contiene varias carpetas dentro de la *wwwroot* carpeta:</span><span class="sxs-lookup"><span data-stu-id="803a0-118">For example, the **Web Application** project template contains several folders within the *wwwroot* folder:</span></span>

* <span data-ttu-id="803a0-119">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="803a0-119">**wwwroot**</span></span>
  * <span data-ttu-id="803a0-120">**css**</span><span class="sxs-lookup"><span data-stu-id="803a0-120">**css**</span></span>
  * <span data-ttu-id="803a0-121">**images**</span><span class="sxs-lookup"><span data-stu-id="803a0-121">**images**</span></span>
  * <span data-ttu-id="803a0-122">**js**</span><span class="sxs-lookup"><span data-stu-id="803a0-122">**js**</span></span>

<span data-ttu-id="803a0-123">El formato del URI para tener acceso a un archivo en el *imágenes* subcarpeta es *http://\<server_address >/images /\<Nombre_archivo_imágenes >*.</span><span class="sxs-lookup"><span data-stu-id="803a0-123">The URI format to access a file in the *images* subfolder is *http://\<server_address>/images/\<image_file_name>*.</span></span> <span data-ttu-id="803a0-124">Por ejemplo, *http://localhost:9189/images/banner3.svg*.</span><span class="sxs-lookup"><span data-stu-id="803a0-124">For example, *http://localhost:9189/images/banner3.svg*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="803a0-125">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="803a0-125">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="803a0-126">Si el destino es .NET Framework, agregue el [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) paquete al proyecto.</span><span class="sxs-lookup"><span data-stu-id="803a0-126">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span> <span data-ttu-id="803a0-127">Si el destino es .NET Core, el [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) incluye este paquete.</span><span class="sxs-lookup"><span data-stu-id="803a0-127">If targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) includes this package.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="803a0-128">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="803a0-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="803a0-129">Agregar el [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) paquete al proyecto.</span><span class="sxs-lookup"><span data-stu-id="803a0-129">Add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span>

---

<span data-ttu-id="803a0-130">Configurar la [middleware](xref:fundamentals/middleware) lo que permite a los servidores de archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="803a0-130">Configure the [middleware](xref:fundamentals/middleware) which enables the serving of static files.</span></span>

### <a name="serve-files-inside-of-web-root"></a><span data-ttu-id="803a0-131">Servir archivos dentro de la raíz de web</span><span class="sxs-lookup"><span data-stu-id="803a0-131">Serve files inside of web root</span></span>

<span data-ttu-id="803a0-132">Invocar la [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) método dentro de `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="803a0-132">Invoke the [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method within `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

<span data-ttu-id="803a0-133">Sin parámetros `UseStaticFiles` sobrecarga del método marca los archivos en la raíz web como servable.</span><span class="sxs-lookup"><span data-stu-id="803a0-133">The parameterless `UseStaticFiles` method overload marks the files in web root as servable.</span></span> <span data-ttu-id="803a0-134">Las siguientes referencias de marcado *wwwroot/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="803a0-134">The following markup references *wwwroot/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a><span data-ttu-id="803a0-135">Servir archivos fuera de la raíz de web</span><span class="sxs-lookup"><span data-stu-id="803a0-135">Serve files outside of web root</span></span>

<span data-ttu-id="803a0-136">Considere la posibilidad de una jerarquía de directorios en el que residen los archivos estáticos que se sirvan fuera de la raíz de web:</span><span class="sxs-lookup"><span data-stu-id="803a0-136">Consider a directory hierarchy in which the static files to be served reside outside of the web root:</span></span>

* <span data-ttu-id="803a0-137">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="803a0-137">**wwwroot**</span></span>
  * <span data-ttu-id="803a0-138">**css**</span><span class="sxs-lookup"><span data-stu-id="803a0-138">**css**</span></span>
  * <span data-ttu-id="803a0-139">**images**</span><span class="sxs-lookup"><span data-stu-id="803a0-139">**images**</span></span>
  * <span data-ttu-id="803a0-140">**js**</span><span class="sxs-lookup"><span data-stu-id="803a0-140">**js**</span></span>
* <span data-ttu-id="803a0-141">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="803a0-141">**MyStaticFiles**</span></span>
  * <span data-ttu-id="803a0-142">**images**</span><span class="sxs-lookup"><span data-stu-id="803a0-142">**images**</span></span>
      * <span data-ttu-id="803a0-143">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="803a0-143">*banner1.svg*</span></span>

<span data-ttu-id="803a0-144">Puede tener acceso una solicitud de la *banner1.svg* archivo configurando el middleware de archivos estáticos como sigue:</span><span class="sxs-lookup"><span data-stu-id="803a0-144">A request can access the *banner1.svg* file by configuring the static file middleware as follows:</span></span>

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

<span data-ttu-id="803a0-145">En el código anterior, el *MyStaticFiles* jerarquía de directorios se expone públicamente a través de la *StaticFiles* segmento de URI.</span><span class="sxs-lookup"><span data-stu-id="803a0-145">In the preceding code, the *MyStaticFiles* directory hierarchy is exposed publicly via the *StaticFiles* URI segment.</span></span> <span data-ttu-id="803a0-146">Una solicitud para *http://\<server_address > /StaticFiles/images/banner1.svg* sirve la *banner1.svg* archivo.</span><span class="sxs-lookup"><span data-stu-id="803a0-146">A request to *http://\<server_address>/StaticFiles/images/banner1.svg* serves the *banner1.svg* file.</span></span>

<span data-ttu-id="803a0-147">Las siguientes referencias de marcado *MyStaticFiles/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="803a0-147">The following markup references *MyStaticFiles/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a><span data-ttu-id="803a0-148">Establecer encabezados de respuesta HTTP</span><span class="sxs-lookup"><span data-stu-id="803a0-148">Set HTTP response headers</span></span>

<span data-ttu-id="803a0-149">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) objeto puede utilizarse para establecer encabezados de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="803a0-149">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) object can be used to set HTTP response headers.</span></span> <span data-ttu-id="803a0-150">Además de configurar los servicios de archivos estáticos desde la raíz de web, el código siguiente establece el `Cache-Control` encabezado:</span><span class="sxs-lookup"><span data-stu-id="803a0-150">In addition to configuring static file serving from the web root, the following code sets the `Cache-Control` header:</span></span>

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="803a0-151">El [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) método existe en el [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paquete.</span><span class="sxs-lookup"><span data-stu-id="803a0-151">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method exists in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

<span data-ttu-id="803a0-152">Los archivos se han realizado públicamente almacenable en caché durante 10 minutos (600 segundos):</span><span class="sxs-lookup"><span data-stu-id="803a0-152">The files have been made publicly cacheable for 10 minutes (600 seconds):</span></span>

![Encabezados de respuesta que muestra el encabezado Cache-Control se ha agregado](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="803a0-154">Autorización de archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="803a0-154">Static file authorization</span></span>

<span data-ttu-id="803a0-155">El middleware de archivos estáticos no proporciona comprobaciones de autorización.</span><span class="sxs-lookup"><span data-stu-id="803a0-155">The static file middleware doesn't provide authorization checks.</span></span> <span data-ttu-id="803a0-156">Los archivos atendido por él, las de incluidas *wwwroot*, sean accesibles públicamente.</span><span class="sxs-lookup"><span data-stu-id="803a0-156">Any files served by it, including those under *wwwroot*, are publicly accessible.</span></span> <span data-ttu-id="803a0-157">Para servir archivos según su autorización:</span><span class="sxs-lookup"><span data-stu-id="803a0-157">To serve files based on authorization:</span></span>

* <span data-ttu-id="803a0-158">Almacenarlos fuera de *wwwroot* y cualquier directorio accesible para el middleware de archivos estáticos **y**</span><span class="sxs-lookup"><span data-stu-id="803a0-158">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>
* <span data-ttu-id="803a0-159">Servir ellos a través de un método de acción al que se aplica la autorización.</span><span class="sxs-lookup"><span data-stu-id="803a0-159">Serve them via an action method to which authorization is applied.</span></span> <span data-ttu-id="803a0-160">Devolver un [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) objeto:</span><span class="sxs-lookup"><span data-stu-id="803a0-160">Return a [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) object:</span></span>

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a><span data-ttu-id="803a0-161">Habilitar examen de directorios</span><span class="sxs-lookup"><span data-stu-id="803a0-161">Enable directory browsing</span></span>

<span data-ttu-id="803a0-162">Examen de directorios permite a los usuarios de su aplicación web ver una lista de directorios y archivos contenidos en un directorio especificado.</span><span class="sxs-lookup"><span data-stu-id="803a0-162">Directory browsing allows users of your web app to see a directory listing and files within a specified directory.</span></span> <span data-ttu-id="803a0-163">Examen de directorios está deshabilitado de forma predeterminada por motivos de seguridad (consulte [consideraciones](#considerations)).</span><span class="sxs-lookup"><span data-stu-id="803a0-163">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="803a0-164">Exploración mediante la invocación de directorios que habilitar la [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) método `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="803a0-164">Enable directory browsing by invoking the [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) method in `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

<span data-ttu-id="803a0-165">Agregar servicios requeridos invocando la [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) método desde `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="803a0-165">Add required services by invoking the [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

<span data-ttu-id="803a0-166">El código anterior permite la exploración de directorios de la *wwwroot/imágenes* utilizando la dirección URL de carpeta *http://\<server_address > / MyImages*, con vínculos a cada archivo y carpeta:</span><span class="sxs-lookup"><span data-stu-id="803a0-166">The preceding code allows directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*, with links to each file and folder:</span></span>

![examen de directorios](static-files/_static/dir-browse.png)

<span data-ttu-id="803a0-168">Vea [consideraciones](#considerations) sobre los riesgos de seguridad al habilitar la exploración.</span><span class="sxs-lookup"><span data-stu-id="803a0-168">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="803a0-169">Tenga en cuenta los dos `UseStaticFiles` llama en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="803a0-169">Note the two `UseStaticFiles` calls in the following example.</span></span> <span data-ttu-id="803a0-170">La primera llamada permite que el servicio de archivos estáticos en la *wwwroot* carpeta.</span><span class="sxs-lookup"><span data-stu-id="803a0-170">The first call enables the serving of static files in the *wwwroot* folder.</span></span> <span data-ttu-id="803a0-171">La segunda llamada habilita el examen de directorios de la *wwwroot/imágenes* utilizando la dirección URL de carpeta *http://\<server_address > / MyImages*:</span><span class="sxs-lookup"><span data-stu-id="803a0-171">The second call enables directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a><span data-ttu-id="803a0-172">Proporcione un documento predeterminado</span><span class="sxs-lookup"><span data-stu-id="803a0-172">Serve a default document</span></span>

<span data-ttu-id="803a0-173">Establecer una página principal predeterminada proporciona a los visitantes un punto de partida lógico al visitar el sitio.</span><span class="sxs-lookup"><span data-stu-id="803a0-173">Setting a default home page provides visitors a logical starting point when visiting your site.</span></span> <span data-ttu-id="803a0-174">Para dar servicio a una página predeterminada sin que el usuario calificar por completo el URI, llame a la [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) método desde `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="803a0-174">To serve a default page without the user fully qualifying the URI, call the [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method from `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> <span data-ttu-id="803a0-175">`UseDefaultFiles`debe llamarse antes `UseStaticFiles` para servir el archivo predeterminado.</span><span class="sxs-lookup"><span data-stu-id="803a0-175">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="803a0-176">`UseDefaultFiles`es un sistema de reescritura de dirección URL que no sirva realmente el archivo.</span><span class="sxs-lookup"><span data-stu-id="803a0-176">`UseDefaultFiles` is a URL rewriter that doesn't actually serve the file.</span></span> <span data-ttu-id="803a0-177">Habilitar el middleware de archivos estáticos a través de `UseStaticFiles` para servir el archivo.</span><span class="sxs-lookup"><span data-stu-id="803a0-177">Enable the static file middleware via `UseStaticFiles` to serve the file.</span></span>

<span data-ttu-id="803a0-178">Con `UseDefaultFiles`, las solicitudes a una búsqueda de la carpeta para:</span><span class="sxs-lookup"><span data-stu-id="803a0-178">With `UseDefaultFiles`, requests to a folder search for:</span></span>

* <span data-ttu-id="803a0-179">*default.htm*</span><span class="sxs-lookup"><span data-stu-id="803a0-179">*default.htm*</span></span>
* <span data-ttu-id="803a0-180">*default.html*</span><span class="sxs-lookup"><span data-stu-id="803a0-180">*default.html*</span></span>
* <span data-ttu-id="803a0-181">*index.htm*</span><span class="sxs-lookup"><span data-stu-id="803a0-181">*index.htm*</span></span>
* <span data-ttu-id="803a0-182">*index.html*</span><span class="sxs-lookup"><span data-stu-id="803a0-182">*index.html*</span></span>

<span data-ttu-id="803a0-183">El primer archivo que se encuentra en la lista se sirve como si el URI completo de fuera de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="803a0-183">The first file found from the list is served as though the request were the fully qualified URI.</span></span> <span data-ttu-id="803a0-184">La dirección URL del explorador continúa reflejar el identificador URI solicitado.</span><span class="sxs-lookup"><span data-stu-id="803a0-184">The browser URL continues to reflect the URI requested.</span></span>

<span data-ttu-id="803a0-185">El código siguiente cambia el nombre de archivo predeterminado que *mydefault.html*:</span><span class="sxs-lookup"><span data-stu-id="803a0-185">The following code changes the default file name to *mydefault.html*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a><span data-ttu-id="803a0-186">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="803a0-186">UseFileServer</span></span>

<span data-ttu-id="803a0-187">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combina la funcionalidad de `UseStaticFiles`, `UseDefaultFiles`, y `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="803a0-187">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="803a0-188">El código siguiente permite a los servidores de archivos estáticos y el archivo predeterminado.</span><span class="sxs-lookup"><span data-stu-id="803a0-188">The following code enables the serving of static files and the default file.</span></span> <span data-ttu-id="803a0-189">Examen de directorios no está habilitado.</span><span class="sxs-lookup"><span data-stu-id="803a0-189">Directory browsing isn't enabled.</span></span>

```csharp
app.UseFileServer();
```

<span data-ttu-id="803a0-190">El código siguiente se basa en la sobrecarga sin parámetros habilitando el examen de directorios:</span><span class="sxs-lookup"><span data-stu-id="803a0-190">The following code builds upon the parameterless overload by enabling directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

<span data-ttu-id="803a0-191">Tenga en cuenta la siguiente jerarquía de directorios:</span><span class="sxs-lookup"><span data-stu-id="803a0-191">Consider the following directory hierarchy:</span></span>

* <span data-ttu-id="803a0-192">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="803a0-192">**wwwroot**</span></span>
  * <span data-ttu-id="803a0-193">**css**</span><span class="sxs-lookup"><span data-stu-id="803a0-193">**css**</span></span>
  * <span data-ttu-id="803a0-194">**images**</span><span class="sxs-lookup"><span data-stu-id="803a0-194">**images**</span></span>
  * <span data-ttu-id="803a0-195">**js**</span><span class="sxs-lookup"><span data-stu-id="803a0-195">**js**</span></span>
* <span data-ttu-id="803a0-196">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="803a0-196">**MyStaticFiles**</span></span>
  * <span data-ttu-id="803a0-197">**images**</span><span class="sxs-lookup"><span data-stu-id="803a0-197">**images**</span></span>
      * <span data-ttu-id="803a0-198">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="803a0-198">*banner1.svg*</span></span>
  * <span data-ttu-id="803a0-199">*default.html*</span><span class="sxs-lookup"><span data-stu-id="803a0-199">*default.html*</span></span>

<span data-ttu-id="803a0-200">El código siguiente permite archivos estáticos, archivos de forma predeterminada y examen de directorios de `MyStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="803a0-200">The following code enables static files, default files, and directory browsing of `MyStaticFiles`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

<span data-ttu-id="803a0-201">`AddDirectoryBrowser`se debe llamar cuando la `EnableDirectoryBrowsing` es el valor de la propiedad `true`:</span><span class="sxs-lookup"><span data-stu-id="803a0-201">`AddDirectoryBrowser` must be called when the `EnableDirectoryBrowsing` property value is `true`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

<span data-ttu-id="803a0-202">De código mediante la jerarquía de archivos y anteriores, para resolver las direcciones URL como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="803a0-202">Using the file hierarchy and preceding code, URLs resolve as follows:</span></span>

| <span data-ttu-id="803a0-203">Identificador URI</span><span class="sxs-lookup"><span data-stu-id="803a0-203">URI</span></span>            |                             <span data-ttu-id="803a0-204">Respuesta</span><span class="sxs-lookup"><span data-stu-id="803a0-204">Response</span></span>  |
| ------- | ------|
| <span data-ttu-id="803a0-205">*http://\<server_address>/StaticFiles/images/banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="803a0-205">*http://\<server_address>/StaticFiles/images/banner1.svg*</span></span>    |      <span data-ttu-id="803a0-206">MyStaticFiles/images/banner1.svg</span><span class="sxs-lookup"><span data-stu-id="803a0-206">MyStaticFiles/images/banner1.svg</span></span> |
| <span data-ttu-id="803a0-207">*http://\<server_address>/StaticFiles*</span><span class="sxs-lookup"><span data-stu-id="803a0-207">*http://\<server_address>/StaticFiles*</span></span>             |     <span data-ttu-id="803a0-208">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="803a0-208">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="803a0-209">Si no existe ningún archivo con el nombre predeterminado en el *MyStaticFiles* directorio, *http://\<server_address > / StaticFiles* devuelve la lista con vínculos activos de directorios:</span><span class="sxs-lookup"><span data-stu-id="803a0-209">If no default-named file exists in the *MyStaticFiles* directory, *http://\<server_address>/StaticFiles* returns the directory listing with clickable links:</span></span>

![Lista de archivos estáticos](static-files/_static/db2.png)

> [!NOTE]
> <span data-ttu-id="803a0-211">`UseDefaultFiles`y `UseDirectoryBrowser` utilizar la dirección URL *http://\<server_address > / StaticFiles* sin la barra diagonal final para desencadenar un cliente redirigir a *http://\<server_address > / StaticFiles /*.</span><span class="sxs-lookup"><span data-stu-id="803a0-211">`UseDefaultFiles` and `UseDirectoryBrowser` use the URL *http://\<server_address>/StaticFiles* without the trailing slash to trigger a client-side redirect to *http://\<server_address>/StaticFiles/*.</span></span> <span data-ttu-id="803a0-212">Tenga en cuenta la adición de la barra diagonal final.</span><span class="sxs-lookup"><span data-stu-id="803a0-212">Notice the addition of the trailing slash.</span></span> <span data-ttu-id="803a0-213">Direcciones URL relativas dentro de los documentos se consideran no válidas sin una barra diagonal final.</span><span class="sxs-lookup"><span data-stu-id="803a0-213">Relative URLs within the documents are deemed invalid without a trailing slash.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="803a0-214">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="803a0-214">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="803a0-215">El [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) clase contiene un `Mappings` propiedad que actúa como una asignación de extensiones de archivo para tipos de contenido MIME.</span><span class="sxs-lookup"><span data-stu-id="803a0-215">The [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) class contains a `Mappings` property serving as a mapping of file extensions to MIME content types.</span></span> <span data-ttu-id="803a0-216">En el ejemplo siguiente, se registran varias extensiones de archivo a los tipos MIME conocidos.</span><span class="sxs-lookup"><span data-stu-id="803a0-216">In the following sample, several file extensions are registered to known MIME types.</span></span> <span data-ttu-id="803a0-217">El *.rtf* se reemplaza la extensión, y *. mp4* se quita.</span><span class="sxs-lookup"><span data-stu-id="803a0-217">The *.rtf* extension is replaced, and *.mp4* is removed.</span></span>

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

<span data-ttu-id="803a0-218">Vea [tipos de contenido MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).</span><span class="sxs-lookup"><span data-stu-id="803a0-218">See [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="803a0-219">Tipos de contenido no estándares</span><span class="sxs-lookup"><span data-stu-id="803a0-219">Non-standard content types</span></span>

<span data-ttu-id="803a0-220">El middleware de archivos estáticos entiende casi 400 tipos de contenido de archivo conocidos.</span><span class="sxs-lookup"><span data-stu-id="803a0-220">The static file middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="803a0-221">Si el usuario solicita un archivo de un tipo de archivo desconocido, el middleware de archivos estáticos devuelve una respuesta HTTP 404 (no encontrado).</span><span class="sxs-lookup"><span data-stu-id="803a0-221">If the user requests a file of an unknown file type, the static file middleware returns a HTTP 404 (Not Found) response.</span></span> <span data-ttu-id="803a0-222">Si se habilita el examen de directorios, se muestra un vínculo al archivo.</span><span class="sxs-lookup"><span data-stu-id="803a0-222">If directory browsing is enabled, a link to the file is displayed.</span></span> <span data-ttu-id="803a0-223">El URI devuelve un error HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="803a0-223">The URI returns an HTTP 404 error.</span></span>

<span data-ttu-id="803a0-224">El código siguiente permite que sirve al tipo desconocido y procesa el archivo como una imagen desconocido:</span><span class="sxs-lookup"><span data-stu-id="803a0-224">The following code enables serving unknown types and renders the unknown file as an image:</span></span>

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="803a0-225">Con el código anterior, una solicitud para un archivo con un tipo de contenido desconocido se devuelve como una imagen.</span><span class="sxs-lookup"><span data-stu-id="803a0-225">With the preceding code, a request for a file with an unknown content type is returned as an image.</span></span>

> [!WARNING]
> <span data-ttu-id="803a0-226">Habilitar [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) supone un riesgo para la seguridad.</span><span class="sxs-lookup"><span data-stu-id="803a0-226">Enabling [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) is a security risk.</span></span> <span data-ttu-id="803a0-227">Está deshabilitado de forma predeterminada y no se recomienda su uso.</span><span class="sxs-lookup"><span data-stu-id="803a0-227">It's disabled by default, and its use is discouraged.</span></span> <span data-ttu-id="803a0-228">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) proporciona una alternativa más segura a ofrecer servicio a archivos con extensiones no estándar.</span><span class="sxs-lookup"><span data-stu-id="803a0-228">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="803a0-229">Consideraciones</span><span class="sxs-lookup"><span data-stu-id="803a0-229">Considerations</span></span>

> [!WARNING]
> <span data-ttu-id="803a0-230">`UseDirectoryBrowser`y `UseStaticFiles` puede producir la pérdida de información confidencial.</span><span class="sxs-lookup"><span data-stu-id="803a0-230">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="803a0-231">Se recomienda deshabilitar examen de directorios en producción.</span><span class="sxs-lookup"><span data-stu-id="803a0-231">Disabling directory browsing in production is highly recommended.</span></span> <span data-ttu-id="803a0-232">Revise cuidadosamente los directorios que se habilitan mediante `UseStaticFiles` o `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="803a0-232">Carefully review which directories are enabled via `UseStaticFiles` or `UseDirectoryBrowser`.</span></span> <span data-ttu-id="803a0-233">Todo el directorio y sus subdirectorios dejan de ser accesibles públicamente.</span><span class="sxs-lookup"><span data-stu-id="803a0-233">The entire directory and its sub-directories become publicly accessible.</span></span> <span data-ttu-id="803a0-234">Almacén de archivos adecuado para servir al público en un directorio dedicado, como  *\<content_root > / wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="803a0-234">Store files suitable for serving to the public in a dedicated directory, such as *\<content_root>/wwwroot*.</span></span> <span data-ttu-id="803a0-235">Separar estos archivos de las vistas de MVC, las páginas de Razor (solo 2.x), archivos de configuración, etcetera.</span><span class="sxs-lookup"><span data-stu-id="803a0-235">Separate these files from MVC views, Razor Pages (2.x only), configuration files, etc.</span></span>

* <span data-ttu-id="803a0-236">Las direcciones URL para el contenido que se exponen a través de `UseDirectoryBrowser` y `UseStaticFiles` están sujetos a las mayúsculas y minúsculas y restricciones de caracteres del sistema de archivos subyacente.</span><span class="sxs-lookup"><span data-stu-id="803a0-236">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of the underlying file system.</span></span> <span data-ttu-id="803a0-237">Por ejemplo, distingue mayúsculas de minúsculas Windows&mdash;no Mac y Linux.</span><span class="sxs-lookup"><span data-stu-id="803a0-237">For example, Windows is case insensitive&mdash;Mac and Linux aren't.</span></span>

* <span data-ttu-id="803a0-238">Aplicaciones de ASP.NET Core hospedadas en IIS uso el [módulo de núcleo de ASP.NET (ANCM)](xref:fundamentals/servers/aspnet-core-module) para reenviar todas las solicitudes a la aplicación, incluidas las solicitudes de archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="803a0-238">ASP.NET Core apps hosted in IIS use the [ASP.NET Core Module (ANCM)](xref:fundamentals/servers/aspnet-core-module) to forward all requests to the app, including static file requests.</span></span> <span data-ttu-id="803a0-239">No se usa el controlador de archivos estáticos de IIS.</span><span class="sxs-lookup"><span data-stu-id="803a0-239">The IIS static file handler isn't used.</span></span> <span data-ttu-id="803a0-240">No tiene ninguna posibilidad de controlar las solicitudes antes de que está controlando el ANCM.</span><span class="sxs-lookup"><span data-stu-id="803a0-240">It has no chance to handle requests before they're handled by the ANCM.</span></span>

* <span data-ttu-id="803a0-241">Complete los pasos siguientes en el Administrador de IIS para quitar el controlador de archivos estáticos de IIS en el nivel de servidor o un sitio Web:</span><span class="sxs-lookup"><span data-stu-id="803a0-241">Complete the following steps in IIS Manager to remove the IIS static file handler at the server or website level:</span></span>
    1. <span data-ttu-id="803a0-242">Navegue hasta la **módulos** característica.</span><span class="sxs-lookup"><span data-stu-id="803a0-242">Navigate to the **Modules** feature.</span></span>
    1. <span data-ttu-id="803a0-243">Seleccione **StaticFileModule** en la lista.</span><span class="sxs-lookup"><span data-stu-id="803a0-243">Select **StaticFileModule** in the list.</span></span>
    1. <span data-ttu-id="803a0-244">Haga clic en **quitar** en el **acciones** sidebar.</span><span class="sxs-lookup"><span data-stu-id="803a0-244">Click **Remove** in the **Actions** sidebar.</span></span>

> [!WARNING]
> <span data-ttu-id="803a0-245">Si está habilitado el controlador de archivos estáticos de IIS **y** la ANCM está configurada incorrectamente, se sirven archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="803a0-245">If the IIS static file handler is enabled **and** the ANCM is configured incorrectly, static files are served.</span></span> <span data-ttu-id="803a0-246">Esto sucede, por ejemplo, si la *web.config* archivo no está implementado.</span><span class="sxs-lookup"><span data-stu-id="803a0-246">This happens, for example, if the *web.config* file isn't deployed.</span></span>

* <span data-ttu-id="803a0-247">Coloque los archivos de código (incluidas *.cs* y *.cshtml*) fuera de la aplicación raíz del proyecto web.</span><span class="sxs-lookup"><span data-stu-id="803a0-247">Place code files (including *.cs* and *.cshtml*) outside of the app project's web root.</span></span> <span data-ttu-id="803a0-248">Por lo tanto, se crea una separación lógica entre la aplicación de cliente contenido y código de servidor.</span><span class="sxs-lookup"><span data-stu-id="803a0-248">A logical separation is therefore created between the app's client-side content and server-based code.</span></span> <span data-ttu-id="803a0-249">Esto impide que se filtren código del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="803a0-249">This prevents server-side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="803a0-250">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="803a0-250">Additional resources</span></span>

* [<span data-ttu-id="803a0-251">Middleware</span><span class="sxs-lookup"><span data-stu-id="803a0-251">Middleware</span></span>](xref:fundamentals/middleware)

* [<span data-ttu-id="803a0-252">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="803a0-252">Introduction to ASP.NET Core</span></span>](xref:index)
