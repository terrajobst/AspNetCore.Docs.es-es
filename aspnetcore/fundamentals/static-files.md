---
title: "Trabajar con archivos estáticos en ASP.NET Core"
author: rick-anderson
description: "Obtenga información acerca de cómo trabajar con archivos estáticos en ASP.NET Core."
keywords: "Núcleo de ASP.NET, archivos estáticos, activos estáticos, HTML, CSS, JavaScript"
ms.author: riande
manager: wpickett
ms.date: 4/07/2017
ms.topic: article
ms.assetid: e32245c7-4eee-4831-bd2e-915dbf9f5f70
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/static-files
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c0751576a1391f26f045c3f8c42ea39c0ff6e5d9
ms.sourcegitcommit: e4fb6b13be56a0fb2f2778623740a047d6489227
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2017
---
# <a name="working-with-static-files-in-aspnet-core"></a><span data-ttu-id="24741-104">Trabajar con archivos estáticos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="24741-104">Working with static files in ASP.NET Core</span></span>

<a name="fundamentals-static-files"></a>

<span data-ttu-id="24741-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="24741-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="24741-106">Los archivos estáticos, como HTML, CSS, imágenes y JavaScript, están activos que una aplicación de ASP.NET Core puede actuar directamente a los clientes.</span><span class="sxs-lookup"><span data-stu-id="24741-106">Static files, such as HTML, CSS, image, and JavaScript, are assets that an ASP.NET Core app can serve directly to clients.</span></span>

<span data-ttu-id="24741-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="24741-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="serving-static-files"></a><span data-ttu-id="24741-108">Enviar archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="24741-108">Serving static files</span></span>

<span data-ttu-id="24741-109">Archivos estáticos suelen encontrarse en la `web root` (*\<contenido raíz > / wwwroot*) carpeta.</span><span class="sxs-lookup"><span data-stu-id="24741-109">Static files are typically located in the `web root` (*\<content-root>/wwwroot*) folder.</span></span> <span data-ttu-id="24741-110">Vea [raíz del contenido](xref:fundamentals/index#content-root) y [raíz Web](xref:fundamentals/index#web-root) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="24741-110">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span> <span data-ttu-id="24741-111">En general establecida la raíz del contenido es el directorio actual para que el proyecto `web root` se encontrarán en el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="24741-111">You generally set the content root to be the current directory so that your project's `web root` will be found while in development.</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]

<span data-ttu-id="24741-112">Archivos estáticos pueden almacenarse en cualquier carpeta bajo la `web root` y obtener acceso a una ruta de acceso relativa a esa raíz.</span><span class="sxs-lookup"><span data-stu-id="24741-112">Static files can be stored in any folder under the `web root` and accessed with a relative path to that root.</span></span> <span data-ttu-id="24741-113">Por ejemplo, cuando se crea un proyecto de aplicación Web predeterminada con Visual Studio, hay varias carpetas que creó en el *wwwroot* carpeta - *css*, *imágenes*, y *js*.</span><span class="sxs-lookup"><span data-stu-id="24741-113">For example, when you create a default Web application project using Visual Studio, there are several folders created within the *wwwroot*  folder - *css*, *images*, and *js*.</span></span> <span data-ttu-id="24741-114">El URI para tener acceso a una imagen en el *imágenes* subcarpeta:</span><span class="sxs-lookup"><span data-stu-id="24741-114">The URI to access an image in the *images* subfolder:</span></span>

* `http://<app>/images/<imageFileName>`
* `http://localhost:9189/images/banner3.svg`

<span data-ttu-id="24741-115">Para los archivos estáticos que se sirvan, debe configurar el [Middleware](middleware.md) para agregar archivos estáticos a la canalización.</span><span class="sxs-lookup"><span data-stu-id="24741-115">In order for static files to be served, you must configure the [Middleware](middleware.md) to add static files to the pipeline.</span></span> <span data-ttu-id="24741-116">El middleware de archivos estáticos puede configurarse mediante la adición de una dependencia en el *Microsoft.AspNetCore.StaticFiles* paquete a su proyecto y, a continuación, llamar a la `UseStaticFiles` método de extensión de `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="24741-116">The static file middleware can be configured by adding a dependency on the *Microsoft.AspNetCore.StaticFiles* package to your project and then calling the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]

<span data-ttu-id="24741-117">`app.UseStaticFiles();`hace que los archivos en `web root` (*wwwroot* de forma predeterminada) servable.</span><span class="sxs-lookup"><span data-stu-id="24741-117">`app.UseStaticFiles();` makes the files in `web root` (*wwwroot* by default) servable.</span></span> <span data-ttu-id="24741-118">Más adelante voy a explicar cómo hacer servable con otro contenido del directorio `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="24741-118">Later I'll show how to make other directory contents servable with `UseStaticFiles`.</span></span>

<span data-ttu-id="24741-119">Debe incluir el paquete NuGet "Microsoft.AspNetCore.StaticFiles".</span><span class="sxs-lookup"><span data-stu-id="24741-119">You must include the NuGet package "Microsoft.AspNetCore.StaticFiles".</span></span>

> [!NOTE]
> <span data-ttu-id="24741-120">`web root`tiene como valor predeterminado el *wwwroot* directorio, pero puede establecer el `web root` directorio con `UseWebRoot`.</span><span class="sxs-lookup"><span data-stu-id="24741-120">`web root` defaults to the *wwwroot* directory, but you can set the `web root` directory with `UseWebRoot`.</span></span>

<span data-ttu-id="24741-121">Suponga que tiene una jerarquía de proyectos donde están los archivos estáticos que se va a actuar fuera de la `web root`.</span><span class="sxs-lookup"><span data-stu-id="24741-121">Suppose you have a project hierarchy where the static files you wish to serve are outside the `web root`.</span></span> <span data-ttu-id="24741-122">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="24741-122">For example:</span></span>

* <span data-ttu-id="24741-123">wwwroot</span><span class="sxs-lookup"><span data-stu-id="24741-123">wwwroot</span></span>
  * <span data-ttu-id="24741-124">css</span><span class="sxs-lookup"><span data-stu-id="24741-124">css</span></span>
  * <span data-ttu-id="24741-125">imágenes</span><span class="sxs-lookup"><span data-stu-id="24741-125">images</span></span>
  * <span data-ttu-id="24741-126">...</span><span class="sxs-lookup"><span data-stu-id="24741-126">...</span></span>
* <span data-ttu-id="24741-127">MyStaticFiles</span><span class="sxs-lookup"><span data-stu-id="24741-127">MyStaticFiles</span></span>
  * <span data-ttu-id="24741-128">Test.png</span><span class="sxs-lookup"><span data-stu-id="24741-128">test.png</span></span>

<span data-ttu-id="24741-129">Para una solicitud obtener acceso a *test.png*, configurar el middleware de archivos estáticos como sigue:</span><span class="sxs-lookup"><span data-stu-id="24741-129">For a request to access *test.png*, configure the static files middleware as follows:</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]

<span data-ttu-id="24741-130">Una solicitud para `http://<app>/StaticFiles/test.png` dará servicio a la *test.png* archivo.</span><span class="sxs-lookup"><span data-stu-id="24741-130">A request to `http://<app>/StaticFiles/test.png` will serve the *test.png* file.</span></span>

<span data-ttu-id="24741-131">`StaticFileOptions()`puede establecer encabezados de respuesta.</span><span class="sxs-lookup"><span data-stu-id="24741-131">`StaticFileOptions()` can set response headers.</span></span> <span data-ttu-id="24741-132">Por ejemplo, el código siguiente configura estático file servers de la *wwwroot* carpeta y establece la `Cache-Control` encabezado para hacerlos públicamente almacenable en caché durante 10 minutos (600 segundos):</span><span class="sxs-lookup"><span data-stu-id="24741-132">For example, the code below sets up static file serving from the *wwwroot* folder and sets the `Cache-Control` header to make them publicly cacheable for 10 minutes (600 seconds):</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]

<span data-ttu-id="24741-133">El [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) método está disponible desde el [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paquete.</span><span class="sxs-lookup"><span data-stu-id="24741-133">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method is available from the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span> <span data-ttu-id="24741-134">Agregar `using Microsoft.AspNetCore.Http;` a su *csharp* archivo si el método no está disponible.</span><span class="sxs-lookup"><span data-stu-id="24741-134">Add `using Microsoft.AspNetCore.Http;` to your *csharp* file if the method is unavailable.</span></span>

![Encabezados de respuesta que muestra el encabezado Cache-Control se ha agregado](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="24741-136">Autorización de archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="24741-136">Static file authorization</span></span>

<span data-ttu-id="24741-137">El módulo de archivo estático proporciona **sin** comprobaciones de autorización.</span><span class="sxs-lookup"><span data-stu-id="24741-137">The static file module provides **no** authorization checks.</span></span> <span data-ttu-id="24741-138">Los archivos atendido por él, las de incluidas *wwwroot* estén disponibles públicamente.</span><span class="sxs-lookup"><span data-stu-id="24741-138">Any files served by it, including those under *wwwroot* are publicly available.</span></span> <span data-ttu-id="24741-139">Para servir archivos según su autorización:</span><span class="sxs-lookup"><span data-stu-id="24741-139">To serve files based on authorization:</span></span>

* <span data-ttu-id="24741-140">Almacenarlos fuera de *wwwroot* y cualquier directorio accesible para el middleware de archivos estáticos **y**</span><span class="sxs-lookup"><span data-stu-id="24741-140">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>

* <span data-ttu-id="24741-141">Atender a ellos a través de una acción de controlador, devolver un `FileResult` donde se aplica la autorización</span><span class="sxs-lookup"><span data-stu-id="24741-141">Serve them through a controller action, returning a `FileResult` where authorization is applied</span></span>

## <a name="enabling-directory-browsing"></a><span data-ttu-id="24741-142">Habilitar examen de directorios</span><span class="sxs-lookup"><span data-stu-id="24741-142">Enabling directory browsing</span></span>

<span data-ttu-id="24741-143">Examen de directorios permite al usuario de la aplicación web ver una lista de directorios y archivos contenidos en un directorio especificado.</span><span class="sxs-lookup"><span data-stu-id="24741-143">Directory browsing allows the user of your web app to see a list of directories and files within a specified directory.</span></span> <span data-ttu-id="24741-144">Examen de directorios está deshabilitado de forma predeterminada por motivos de seguridad (consulte [consideraciones](#considerations)).</span><span class="sxs-lookup"><span data-stu-id="24741-144">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="24741-145">Para habilitar el examen de directorios, llame a la `UseDirectoryBrowser` método de extensión de `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="24741-145">To enable directory browsing, call the `UseDirectoryBrowser` extension method from  `Startup.Configure`:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet1)]

<span data-ttu-id="24741-146">Y agregue los servicios necesarios mediante una llamada a `AddDirectoryBrowser` método de extensión de `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="24741-146">And add required services by calling `AddDirectoryBrowser` extension method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet2)]

<span data-ttu-id="24741-147">El código anterior permite la exploración de directorios de la *wwwroot/imágenes* carpeta mediante la dirección URL http://\<aplicación > / MyImages, con vínculos a cada archivo y carpeta:</span><span class="sxs-lookup"><span data-stu-id="24741-147">The code above allows directory browsing of the *wwwroot/images* folder using the URL http://\<app>/MyImages, with links to each file and folder:</span></span>

![examen de directorios](static-files/_static/dir-browse.png)

<span data-ttu-id="24741-149">Vea [consideraciones](#considerations) sobre los riesgos de seguridad al habilitar la exploración.</span><span class="sxs-lookup"><span data-stu-id="24741-149">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="24741-150">Tenga en cuenta los dos `app.UseStaticFiles` llamadas.</span><span class="sxs-lookup"><span data-stu-id="24741-150">Note the two `app.UseStaticFiles` calls.</span></span> <span data-ttu-id="24741-151">La primera de ellas es requerida para atender la CSS, imágenes y JavaScript en el *wwwroot* carpeta y la segunda llamada de examen de directorios de la *wwwroot/imágenes* carpeta mediante la dirección URL http://\<aplicación > / MyImages:</span><span class="sxs-lookup"><span data-stu-id="24741-151">The first one is required to serve the CSS, images and JavaScript in the *wwwroot* folder, and the second call for directory browsing of the *wwwroot/images* folder using the URL http://\<app>/MyImages:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]

## <a name="serving-a-default-document"></a><span data-ttu-id="24741-152">Servir un documento predeterminado</span><span class="sxs-lookup"><span data-stu-id="24741-152">Serving a default document</span></span>

<span data-ttu-id="24741-153">Establecer una página principal predeterminada, proporciona a los visitantes del sitio un punto de partida cuando se visita el sitio.</span><span class="sxs-lookup"><span data-stu-id="24741-153">Setting a default home page gives site visitors a place to start when visiting your site.</span></span> <span data-ttu-id="24741-154">En el orden de la aplicación Web servir una página predeterminada sin que el usuario tener que calificar totalmente el URI, llame a la `UseDefaultFiles` método de extensión de `Startup.Configure` como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="24741-154">In order for your Web app to serve a default page without the user having to fully qualify the URI, call the `UseDefaultFiles` extension method from `Startup.Configure` as follows.</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]

> [!NOTE]
> <span data-ttu-id="24741-155">`UseDefaultFiles`debe llamarse antes `UseStaticFiles` para servir el archivo predeterminado.</span><span class="sxs-lookup"><span data-stu-id="24741-155">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="24741-156">`UseDefaultFiles`es un escritor de nuevo de dirección URL que no sirva realmente el archivo.</span><span class="sxs-lookup"><span data-stu-id="24741-156">`UseDefaultFiles` is a URL re-writer that doesn't actually serve the file.</span></span> <span data-ttu-id="24741-157">Debe habilitar el middleware de archivos estáticos (`UseStaticFiles`) para servir el archivo.</span><span class="sxs-lookup"><span data-stu-id="24741-157">You must enable the static file middleware (`UseStaticFiles`) to serve the file.</span></span>

<span data-ttu-id="24741-158">Con `UseDefaultFiles`, buscará las solicitudes a una carpeta:</span><span class="sxs-lookup"><span data-stu-id="24741-158">With `UseDefaultFiles`, requests to a folder will search for:</span></span>

* <span data-ttu-id="24741-159">default.htm</span><span class="sxs-lookup"><span data-stu-id="24741-159">default.htm</span></span>
* <span data-ttu-id="24741-160">default.html</span><span class="sxs-lookup"><span data-stu-id="24741-160">default.html</span></span>
* <span data-ttu-id="24741-161">index.htm</span><span class="sxs-lookup"><span data-stu-id="24741-161">index.htm</span></span>
* <span data-ttu-id="24741-162">index.html</span><span class="sxs-lookup"><span data-stu-id="24741-162">index.html</span></span>

<span data-ttu-id="24741-163">El primer archivo que se encuentra en la lista se servirá como si la solicitud no era el URI completo (aunque se seguirá la dirección URL del explorador mostrar el identificador URI solicitado).</span><span class="sxs-lookup"><span data-stu-id="24741-163">The first file found from the list will be served as if the request was the fully qualified URI (although the browser URL will continue to show the URI requested).</span></span>

<span data-ttu-id="24741-164">El código siguiente muestra cómo cambiar el nombre de archivo predeterminado que *mydefault.html*.</span><span class="sxs-lookup"><span data-stu-id="24741-164">The following code shows how to change the default file name to *mydefault.html*.</span></span>

[!code-csharp[Main](static-files/sample/StartupDefault.cs?name=snippet1)]

## <a name="usefileserver"></a><span data-ttu-id="24741-165">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="24741-165">UseFileServer</span></span>

<span data-ttu-id="24741-166">`UseFileServer`combina la funcionalidad de `UseStaticFiles`, `UseDefaultFiles`, y `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="24741-166">`UseFileServer` combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="24741-167">El código siguiente permite archivos estáticos y el archivo predeterminado que se sirvan, pero no permite la exploración de directorios:</span><span class="sxs-lookup"><span data-stu-id="24741-167">The following code enables static files and the default file to be served, but does not allow directory browsing:</span></span>

```csharp
app.UseFileServer();
   ```

<span data-ttu-id="24741-168">El código siguiente permite archivos estáticos, los archivos de forma predeterminada y examen de directorios:</span><span class="sxs-lookup"><span data-stu-id="24741-168">The following code enables static files, default files and  directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
   ```

<span data-ttu-id="24741-169">Vea [consideraciones](#considerations) sobre los riesgos de seguridad al habilitar la exploración.</span><span class="sxs-lookup"><span data-stu-id="24741-169">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span> <span data-ttu-id="24741-170">Al igual que con `UseStaticFiles`, `UseDefaultFiles`, y `UseDirectoryBrowser`, si desea servir archivos que existen fuera de la `web root`, crear una instancia y configurar un `FileServerOptions` objeto que se pasa como un parámetro a `UseFileServer`.</span><span class="sxs-lookup"><span data-stu-id="24741-170">As with `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`, if you wish to serve files that exist outside the `web root`, you instantiate and configure an `FileServerOptions` object that you pass as a parameter to `UseFileServer`.</span></span> <span data-ttu-id="24741-171">Por ejemplo, dada la siguiente jerarquía de directorios de la aplicación Web:</span><span class="sxs-lookup"><span data-stu-id="24741-171">For example, given the following directory hierarchy in your Web app:</span></span>

* <span data-ttu-id="24741-172">wwwroot</span><span class="sxs-lookup"><span data-stu-id="24741-172">wwwroot</span></span>

  * <span data-ttu-id="24741-173">css</span><span class="sxs-lookup"><span data-stu-id="24741-173">css</span></span>

  * <span data-ttu-id="24741-174">imágenes</span><span class="sxs-lookup"><span data-stu-id="24741-174">images</span></span>

  * <span data-ttu-id="24741-175">...</span><span class="sxs-lookup"><span data-stu-id="24741-175">...</span></span>

* <span data-ttu-id="24741-176">MyStaticFiles</span><span class="sxs-lookup"><span data-stu-id="24741-176">MyStaticFiles</span></span>

  * <span data-ttu-id="24741-177">Test.png</span><span class="sxs-lookup"><span data-stu-id="24741-177">test.png</span></span>

  * <span data-ttu-id="24741-178">default.html</span><span class="sxs-lookup"><span data-stu-id="24741-178">default.html</span></span>

<span data-ttu-id="24741-179">Utilizando el ejemplo de jerarquía anterior, puede que los archivos estáticos, archivos de forma predeterminada y exploración de la `MyStaticFiles` directory.</span><span class="sxs-lookup"><span data-stu-id="24741-179">Using the hierarchy example above, you might want to enable static files, default files, and browsing for the `MyStaticFiles` directory.</span></span> <span data-ttu-id="24741-180">En el siguiente fragmento de código, que se logra con una sola llamada a `FileServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="24741-180">In the following code snippet, that is accomplished with a single call to `FileServerOptions`.</span></span>

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]

<span data-ttu-id="24741-181">Si `enableDirectoryBrowsing` está establecido en `true` , debe llamar a `AddDirectoryBrowser` método de extensión de `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="24741-181">If `enableDirectoryBrowsing` is set to `true` you are required to call `AddDirectoryBrowser` extension method from  `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?name=snippet2)]

<span data-ttu-id="24741-182">Uso de la jerarquía de archivos y el código anterior:</span><span class="sxs-lookup"><span data-stu-id="24741-182">Using the file hierarchy and code above:</span></span>

| <span data-ttu-id="24741-183">Identificador URI</span><span class="sxs-lookup"><span data-stu-id="24741-183">URI</span></span>            |                             <span data-ttu-id="24741-184">Respuesta</span><span class="sxs-lookup"><span data-stu-id="24741-184">Response</span></span>  |
| ------- | ------|
| `http://<app>/StaticFiles/test.png`    |      <span data-ttu-id="24741-185">MyStaticFiles/test.png</span><span class="sxs-lookup"><span data-stu-id="24741-185">MyStaticFiles/test.png</span></span> |
| `http://<app>/StaticFiles`              |     <span data-ttu-id="24741-186">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="24741-186">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="24741-187">Si no tiene valor predeterminado con el nombre de archivos que se encuentran en el *MyStaticFiles* directorio, http://\<aplicación > / StaticFiles devuelve la lista con vínculos activos de directorios:</span><span class="sxs-lookup"><span data-stu-id="24741-187">If no default named files are in the *MyStaticFiles* directory, http://\<app>/StaticFiles returns the directory listing with clickable links:</span></span>

![Lista de archivos estáticos](static-files/_static/db2.PNG)

> [!NOTE]
> <span data-ttu-id="24741-189">`UseDefaultFiles`y `UseDirectoryBrowser` le llevará a la dirección url http://\<aplicación > / StaticFiles sin la barra diagonal final y causa un cliente redirigir a http://\<aplicación > /StaticFiles/ (adición de la barra diagonal final).</span><span class="sxs-lookup"><span data-stu-id="24741-189">`UseDefaultFiles` and `UseDirectoryBrowser` will take the url http://\<app>/StaticFiles without the trailing slash and cause a client side redirect to http://\<app>/StaticFiles/ (adding the trailing slash).</span></span> <span data-ttu-id="24741-190">Sin la barra diagonal final relativa direcciones URL dentro de los documentos sería incorrectas.</span><span class="sxs-lookup"><span data-stu-id="24741-190">Without the trailing slash relative URLs within the documents would be incorrect.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="24741-191">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="24741-191">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="24741-192">La `FileExtensionContentTypeProvider` clase contiene una colección que asigna las extensiones de archivo para tipos de contenido MIME.</span><span class="sxs-lookup"><span data-stu-id="24741-192">The `FileExtensionContentTypeProvider` class contains a  collection that maps file extensions to MIME content types.</span></span> <span data-ttu-id="24741-193">En el ejemplo siguiente, se registran varias extensiones de archivo para el tipo MIME conocido, se reemplaza ".rtf" y ". mp4" se quita.</span><span class="sxs-lookup"><span data-stu-id="24741-193">In the following sample, several file extensions are registered to known MIME types, the ".rtf" is replaced, and ".mp4" is removed.</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]

<span data-ttu-id="24741-194">Vea [tipos de contenido MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).</span><span class="sxs-lookup"><span data-stu-id="24741-194">See   [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="24741-195">Tipos de contenido no estándares</span><span class="sxs-lookup"><span data-stu-id="24741-195">Non-standard content types</span></span>

<span data-ttu-id="24741-196">El middleware de archivos estáticos ASP.NET entiende casi 400 tipos de contenido de archivo conocidos.</span><span class="sxs-lookup"><span data-stu-id="24741-196">The ASP.NET static file middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="24741-197">Si el usuario solicita un archivo de un tipo de archivo desconocido, el middleware de archivos estáticos devuelve una respuesta HTTP 404 (no encontrado).</span><span class="sxs-lookup"><span data-stu-id="24741-197">If the user requests a file of an unknown file type, the static file middleware returns a HTTP 404 (Not found) response.</span></span> <span data-ttu-id="24741-198">Si se habilita el examen de directorios, se mostrará un vínculo al archivo, pero el URI devolverá un error HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="24741-198">If directory browsing is enabled, a link to the file will be displayed, but the URI will return an HTTP 404 error.</span></span>

<span data-ttu-id="24741-199">El código siguiente permite que sirve al tipo desconocido y representará el archivo desconocido como una imagen.</span><span class="sxs-lookup"><span data-stu-id="24741-199">The following code enables serving unknown types and will render the unknown file as an image.</span></span>

[!code-csharp[Main](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]

<span data-ttu-id="24741-200">Con el código anterior, se devolverá una solicitud para un archivo con un tipo de contenido desconocido como una imagen.</span><span class="sxs-lookup"><span data-stu-id="24741-200">With the code above, a request for a file with an unknown content type will be returned as an image.</span></span>

>[!WARNING]
> <span data-ttu-id="24741-201">Habilitar `ServeUnknownFileTypes` es un riesgo de seguridad y no se recomienda utilizarlo.</span><span class="sxs-lookup"><span data-stu-id="24741-201">Enabling `ServeUnknownFileTypes` is a security risk and using it is discouraged.</span></span>  <span data-ttu-id="24741-202">`FileExtensionContentTypeProvider`(explicados con anterioridad) proporciona una alternativa más segura a ofrecer servicio a archivos con extensiones no estándar.</span><span class="sxs-lookup"><span data-stu-id="24741-202">`FileExtensionContentTypeProvider`  (explained above) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="24741-203">Consideraciones</span><span class="sxs-lookup"><span data-stu-id="24741-203">Considerations</span></span>

>[!WARNING]
> <span data-ttu-id="24741-204">`UseDirectoryBrowser`y `UseStaticFiles` puede producir la pérdida de información confidencial.</span><span class="sxs-lookup"><span data-stu-id="24741-204">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="24741-205">Se recomienda **no** Habilitar examen de directorios en producción.</span><span class="sxs-lookup"><span data-stu-id="24741-205">We recommend that you **not** enable directory browsing in production.</span></span> <span data-ttu-id="24741-206">Tenga cuidado acerca de los directorios que se habilita con `UseStaticFiles` o `UseDirectoryBrowser` como todo el directorio y todos los subdirectorios será accesibles.</span><span class="sxs-lookup"><span data-stu-id="24741-206">Be careful about which directories you enable with `UseStaticFiles` or `UseDirectoryBrowser` as the entire directory and all sub-directories will be accessible.</span></span> <span data-ttu-id="24741-207">Se recomienda mantener contenido público en su propio directorio como  *\<contenido raíz > / wwwroot*, lejos de vistas de la aplicación, archivos de configuración, etcetera.</span><span class="sxs-lookup"><span data-stu-id="24741-207">We recommend keeping public content in its own directory such as *\<content root>/wwwroot*, away from application views, configuration files, etc.</span></span>

* <span data-ttu-id="24741-208">Las direcciones URL para el contenido que se exponen a través de `UseDirectoryBrowser` y `UseStaticFiles` están sujetos a las mayúsculas y minúsculas y restricciones de caracteres de su sistema de archivos subyacente.</span><span class="sxs-lookup"><span data-stu-id="24741-208">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of their underlying file system.</span></span> <span data-ttu-id="24741-209">Por ejemplo, Windows distingue mayúsculas de minúsculas, pero no son Mac y Linux.</span><span class="sxs-lookup"><span data-stu-id="24741-209">For example, Windows is case insensitive, but Mac and Linux are not.</span></span>

* <span data-ttu-id="24741-210">Las aplicaciones de ASP.NET Core hospedadas en IIS usar el módulo principal de ASP.NET para reenviar todas las solicitudes a la aplicación incluidas las solicitudes de archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="24741-210">ASP.NET Core applications hosted in IIS use the ASP.NET Core Module to forward all requests to the application including requests for static files.</span></span> <span data-ttu-id="24741-211">No se usa el controlador de archivos estáticos de IIS porque no puede ser una oportunidad para controlar las solicitudes antes de que están controlados por el módulo principal de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="24741-211">The IIS static file handler is not used because it doesn't get a chance to handle requests before they are handled by the ASP.NET Core Module.</span></span>

* <span data-ttu-id="24741-212">Para quitar el controlador de archivos estáticos de IIS (en el nivel de servidor o un sitio Web):</span><span class="sxs-lookup"><span data-stu-id="24741-212">To remove the IIS static file handler (at the server or website level):</span></span>

     * <span data-ttu-id="24741-213">Navegue hasta la **módulos** característica</span><span class="sxs-lookup"><span data-stu-id="24741-213">Navigate to the **Modules** feature</span></span>

     * <span data-ttu-id="24741-214">Seleccione **StaticFileModule** en la lista</span><span class="sxs-lookup"><span data-stu-id="24741-214">Select **StaticFileModule** in the list</span></span>

     * <span data-ttu-id="24741-215">Pulse **quitar** en el **acciones** sidebar</span><span class="sxs-lookup"><span data-stu-id="24741-215">Tap **Remove** in the **Actions** sidebar</span></span>

>[!WARNING]
> <span data-ttu-id="24741-216">Si está habilitado el controlador de archivos estáticos de IIS **y** el módulo de núcleo de ASP.NET (ANCM) no está configurado correctamente (por ejemplo si *web.config* no se implementó), se servirá archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="24741-216">If the IIS static file handler is enabled **and** the ASP.NET Core Module (ANCM) is not correctly configured (for example if *web.config* was not deployed), static files will be served.</span></span>

* <span data-ttu-id="24741-217">Archivos de código (como c# y Razor) deben colocarse fuera del proyecto de aplicación `web root` (*wwwroot* de forma predeterminada).</span><span class="sxs-lookup"><span data-stu-id="24741-217">Code files (including c# and Razor) should be placed outside of the app project's `web root` (*wwwroot* by default).</span></span> <span data-ttu-id="24741-218">Esto crea una separación clara entre el contenido del lado de la aplicación cliente y código de origen del lado servidor, lo que impide que se filtren código de servidor.</span><span class="sxs-lookup"><span data-stu-id="24741-218">This creates a clean separation between your app's client side content and server side source code, which prevents server side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="24741-219">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="24741-219">Additional Resources</span></span>

* [<span data-ttu-id="24741-220">Middleware</span><span class="sxs-lookup"><span data-stu-id="24741-220">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="24741-221">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="24741-221">Introduction to ASP.NET Core</span></span>](../index.md)
