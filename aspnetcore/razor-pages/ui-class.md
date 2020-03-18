---
title: Interfaz de usuario de Razor reutilizable en bibliotecas de clases con ASP.NET Core
author: Rick-Anderson
description: En este artículo se explica cómo crear una interfaz de usuario de Razor reutilizable mediante el uso de vistas parciales en una biblioteca de clases en ASP.NET Core.
ms.author: riande
ms.date: 01/25/2020
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: f24dc62eba345a8a3d35143805b4966cb51832fa
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78650987"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="ae53f-103">Creación de una interfaz de usuario reutilizable con el proyecto de biblioteca de clases de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ae53f-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="ae53f-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ae53f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ae53f-105">Las vistas, las páginas, los controladores, los modelos de página, los [componentes](xref:blazor/class-libraries), los modelos de datos y los [componentes de vista](xref:mvc/views/view-components) de Razor se pueden integrar en una biblioteca de clases de Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="ae53f-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="ae53f-106">Las RCL se pueden empaquetar y reutilizar.</span><span class="sxs-lookup"><span data-stu-id="ae53f-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="ae53f-107">Las aplicaciones pueden incluir la RCL y reemplazar las vistas y páginas que contienen.</span><span class="sxs-lookup"><span data-stu-id="ae53f-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="ae53f-108">Cuando existe una vista, vista parcial o página de Razor tanto en la aplicación web como en la RCL, tendrá prioridad el marcado de Razor (archivo *.cshtml*) de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="ae53f-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="ae53f-109">[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ae53f-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="ae53f-110">Crear una biblioteca de clases que contenga interfaz de usuario de Razor</span><span class="sxs-lookup"><span data-stu-id="ae53f-110">Create a class library containing Razor UI</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="ae53f-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ae53f-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ae53f-112">En Visual Studio, seleccione **Crear un proyecto nuevo**.</span><span class="sxs-lookup"><span data-stu-id="ae53f-112">From Visual Studio select **Create new a new project**.</span></span>
* <span data-ttu-id="ae53f-113">Seleccione **Biblioteca de clases de Razor** > **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="ae53f-113">Select **Razor Class Library** > **Next**.</span></span>
* <span data-ttu-id="ae53f-114">Asigne un nombre a la biblioteca (por ejemplo, "RazorClassLib") > **Crear**.</span><span class="sxs-lookup"><span data-stu-id="ae53f-114">Name the library (for example, "RazorClassLib"), > **Create**.</span></span> <span data-ttu-id="ae53f-115">Para evitar un conflicto de nombres de archivo con la biblioteca de vistas generada, asegúrese de que el nombre de la biblioteca no acaba en `.Views`.</span><span class="sxs-lookup"><span data-stu-id="ae53f-115">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="ae53f-116">Seleccione **Admitir páginas y vistas** si necesita admitir vistas.</span><span class="sxs-lookup"><span data-stu-id="ae53f-116">Select **Support pages and views** if you need to support views.</span></span> <span data-ttu-id="ae53f-117">De forma predeterminada, solo se admiten Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="ae53f-117">By default, only Razor Pages are supported.</span></span> <span data-ttu-id="ae53f-118">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="ae53f-118">Select **Create**.</span></span>

<span data-ttu-id="ae53f-119">De forma predeterminada, la plantilla de la biblioteca de clases de Razor (RCL) utiliza el desarrollo de componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="ae53f-119">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="ae53f-120">La opción **Admitir páginas y vistas** proporciona compatibilidad con páginas y vistas.</span><span class="sxs-lookup"><span data-stu-id="ae53f-120">The **Support pages and views** option supports pages and views.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="ae53f-121">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="ae53f-121">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ae53f-122">Ejecute `dotnet new razorclasslib` desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="ae53f-122">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="ae53f-123">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ae53f-123">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="ae53f-124">De forma predeterminada, la plantilla de la biblioteca de clases de Razor (RCL) utiliza el desarrollo de componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="ae53f-124">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="ae53f-125">Pase la opción `--support-pages-and-views` (`dotnet new razorclasslib --support-pages-and-views`) para proporcionar compatibilidad con páginas y vistas.</span><span class="sxs-lookup"><span data-stu-id="ae53f-125">Pass the `--support-pages-and-views` option (`dotnet new razorclasslib --support-pages-and-views`) to provide support for pages and views.</span></span>

<span data-ttu-id="ae53f-126">Para más información, vea [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="ae53f-126">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="ae53f-127">Para evitar un conflicto de nombres de archivo con la biblioteca de vistas generada, asegúrese de que el nombre de la biblioteca no acaba en `.Views`.</span><span class="sxs-lookup"><span data-stu-id="ae53f-127">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="ae53f-128">Agregue archivos de Razor a la RCL.</span><span class="sxs-lookup"><span data-stu-id="ae53f-128">Add Razor files to the RCL.</span></span>

<span data-ttu-id="ae53f-129">Las plantillas de ASP.NET Core dan por sentado que el contenido de la RCL se encuentra en la carpeta *Áreas*.</span><span class="sxs-lookup"><span data-stu-id="ae53f-129">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="ae53f-130">Consulte la sección [Diseño de páginas de RCL](#rcl-pages-layout) para crear una RCL que exponga contenido en `~/Pages` y no en `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="ae53f-130">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="ae53f-131">Referencias al contenido de la RCL</span><span class="sxs-lookup"><span data-stu-id="ae53f-131">Reference RCL content</span></span>

<span data-ttu-id="ae53f-132">Los siguientes elementos pueden hacer referencia a la RCL:</span><span class="sxs-lookup"><span data-stu-id="ae53f-132">The RCL can be referenced by:</span></span>

* <span data-ttu-id="ae53f-133">Paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="ae53f-133">NuGet package.</span></span> <span data-ttu-id="ae53f-134">Vea [Creación de paquetes NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) y [Creación y publicación de un paquete NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="ae53f-134">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="ae53f-135">*{NombreDeProyecto}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="ae53f-135">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="ae53f-136">Vea [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="ae53f-136">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="ae53f-137">Reemplazar vistas, vistas parciales y páginas</span><span class="sxs-lookup"><span data-stu-id="ae53f-137">Override views, partial views, and pages</span></span>

<span data-ttu-id="ae53f-138">Cuando existe una vista, vista parcial o página de Razor tanto en la aplicación web como en la RCL, tendrá prioridad el marcado de Razor (archivo *.cshtml*) de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="ae53f-138">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="ae53f-139">Por ejemplo, si agrega *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* a WebApp1, Page1 en WebApp1 prevalecerá sobre Page1 en la biblioteca de clases de Razor.</span><span class="sxs-lookup"><span data-stu-id="ae53f-139">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="ae53f-140">En la descarga de ejemplo, cambie el nombre *WebApp1/Areas/MyFeature2* por *WebApp1/Areas/MyFeature* para comprobar la prioridad.</span><span class="sxs-lookup"><span data-stu-id="ae53f-140">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="ae53f-141">Copie la vista parcial *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* en *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ae53f-141">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="ae53f-142">Actualice el marcado para señalar la nueva ubicación.</span><span class="sxs-lookup"><span data-stu-id="ae53f-142">Update the markup to indicate the new location.</span></span> <span data-ttu-id="ae53f-143">Compile y ejecute la aplicación para comprobar si se está usando la versión de la vista parcial de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ae53f-143">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="ae53f-144">Diseño de páginas de RCL</span><span class="sxs-lookup"><span data-stu-id="ae53f-144">RCL Pages layout</span></span>

<span data-ttu-id="ae53f-145">Para hacer referencia al contenido de la RCL como si formara parte de la carpeta *Pages* de la aplicación web, cree el proyecto RCL con la siguiente estructura de archivos:</span><span class="sxs-lookup"><span data-stu-id="ae53f-145">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="ae53f-146">*RazorUIClassLib/Pages*</span><span class="sxs-lookup"><span data-stu-id="ae53f-146">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="ae53f-147">*RazorUIClassLib/Pages/Shared*</span><span class="sxs-lookup"><span data-stu-id="ae53f-147">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="ae53f-148">Imagine que *RazorUIClassLib/Pages/Shared* contiene dos archivos parciales: *_Header.cshtml* y *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ae53f-148">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="ae53f-149">En ese caso, se podrían agregar etiquetas `<partial>` al archivo *_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ae53f-149">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="ae53f-150">Creación de una RCL con recursos estáticos</span><span class="sxs-lookup"><span data-stu-id="ae53f-150">Create an RCL with static assets</span></span>

<span data-ttu-id="ae53f-151">Una RCL puede requerir recursos estáticos complementarios a los que puede hacer referencia la RCL o la aplicación de consumo de la RCL.</span><span class="sxs-lookup"><span data-stu-id="ae53f-151">An RCL may require companion static assets that can be referenced by either the RCL or the consuming app of the RCL.</span></span> <span data-ttu-id="ae53f-152">ASP.NET Core permite crear bibliotecas de clases de Razor que incluyan recursos estáticos que estén disponibles para una aplicación de consumo.</span><span class="sxs-lookup"><span data-stu-id="ae53f-152">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="ae53f-153">Para incluir los recursos complementarios como parte de una RCL, cree una carpeta *wwwroot* en la biblioteca de clases e incluya en ella los archivos necesarios.</span><span class="sxs-lookup"><span data-stu-id="ae53f-153">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="ae53f-154">Al empaquetar una RCL, todos los recursos complementarios de la carpeta *wwwroot* se incluyen automáticamente en el paquete.</span><span class="sxs-lookup"><span data-stu-id="ae53f-154">When packing an RCL, all companion assets in the *wwwroot* folder are automatically included in the package.</span></span>

### <a name="exclude-static-assets"></a><span data-ttu-id="ae53f-155">Exclusión de recursos estáticos</span><span class="sxs-lookup"><span data-stu-id="ae53f-155">Exclude static assets</span></span>

<span data-ttu-id="ae53f-156">Para excluir recursos estáticos, agregue la ruta de exclusión deseada al grupo de propiedades `$(DefaultItemExcludes)` en el archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="ae53f-156">To exclude static assets, add the desired exclusion path to the `$(DefaultItemExcludes)` property group in the project file.</span></span> <span data-ttu-id="ae53f-157">Separe las entradas con un punto y coma (`;`).</span><span class="sxs-lookup"><span data-stu-id="ae53f-157">Separate entries with a semicolon (`;`).</span></span>

<span data-ttu-id="ae53f-158">En el ejemplo siguiente, la hoja de estilos *lib.css* de la carpeta *wwwroot* no se considera un recurso estático y no se incluye en la RCL publicada:</span><span class="sxs-lookup"><span data-stu-id="ae53f-158">In the following example, the *lib.css* stylesheet in the *wwwroot* folder isn't considered a static asset and isn't included in the published RCL:</span></span>

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a><span data-ttu-id="ae53f-159">Integración de TypeScript</span><span class="sxs-lookup"><span data-stu-id="ae53f-159">Typescript integration</span></span>

<span data-ttu-id="ae53f-160">Para incluir archivos TypeScript en una RCL:</span><span class="sxs-lookup"><span data-stu-id="ae53f-160">To include TypeScript files in an RCL:</span></span>

1. <span data-ttu-id="ae53f-161">Coloque los archivos TypeScript ( *.ts*) fuera de la carpeta *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="ae53f-161">Place the TypeScript files (*.ts*) outside of the *wwwroot* folder.</span></span> <span data-ttu-id="ae53f-162">Por ejemplo, coloque los archivos en una carpeta de *cliente*.</span><span class="sxs-lookup"><span data-stu-id="ae53f-162">For example, place the files in a *Client* folder.</span></span>

1. <span data-ttu-id="ae53f-163">Configure la salida de la compilación de TypeScript para la carpeta *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="ae53f-163">Configure the TypeScript build output for the *wwwroot* folder.</span></span> <span data-ttu-id="ae53f-164">Establezca la propiedad `TypescriptOutDir` dentro de un `PropertyGroup` en el archivo del proyecto:</span><span class="sxs-lookup"><span data-stu-id="ae53f-164">Set the `TypescriptOutDir` property inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. <span data-ttu-id="ae53f-165">Incluya el destino de TypeScript como una dependencia del destino `ResolveCurrentProjectStaticWebAssets`; para ello, agregue el siguiente destino dentro de un objeto `PropertyGroup` en el archivo del proyecto:</span><span class="sxs-lookup"><span data-stu-id="ae53f-165">Include the TypeScript target as a dependency of the `ResolveCurrentProjectStaticWebAssets` target by adding the following target inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     CompileTypeScript;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="ae53f-166">Consumo de contenido de una RCL a la que se hace referencia</span><span class="sxs-lookup"><span data-stu-id="ae53f-166">Consume content from a referenced RCL</span></span>

<span data-ttu-id="ae53f-167">Los archivos incluidos en la carpeta *wwwroot* de la RCL se exponen a la RCL o a la aplicación de consumo en el prefijo `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="ae53f-167">The files included in the *wwwroot* folder of the RCL are exposed to either the RCL or the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="ae53f-168">Por ejemplo, una biblioteca denominada *Razor.Class.Lib* produce una ruta de acceso al contenido estático en `_content/Razor.Class.Lib/`.</span><span class="sxs-lookup"><span data-stu-id="ae53f-168">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span> <span data-ttu-id="ae53f-169">Al generar un paquete NuGet, si el nombre del ensamblado no es igual que el del identificador del paquete, use el identificador de paquete para `{LIBRARY NAME}`.</span><span class="sxs-lookup"><span data-stu-id="ae53f-169">When producing a NuGet package and the assembly name isn't the same as the package ID, use the package ID for `{LIBRARY NAME}`.</span></span>

<span data-ttu-id="ae53f-170">La aplicación de consumo hace referencia a los recursos estáticos proporcionados por la biblioteca con `<script>`, `<style>`, `<img>` y otras etiquetas HTML.</span><span class="sxs-lookup"><span data-stu-id="ae53f-170">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="ae53f-171">La aplicación de consumo debe tener habilitada la [compatibilidad con archivos estáticos](xref:fundamentals/static-files) en `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="ae53f-171">The consuming app must have [static file support](xref:fundamentals/static-files) enabled in `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="ae53f-172">Al ejecutar la aplicación de consumo desde la salida de la compilación (`dotnet run`), los activos web estáticos están habilitados de forma predeterminada en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="ae53f-172">When running the consuming app from build output (`dotnet run`), static web assets are enabled by default in the Development environment.</span></span> <span data-ttu-id="ae53f-173">Para admitir recursos en otros entornos al ejecutar la aplicación desde la salida de la compilación, llame a `UseStaticWebAssets` en el generador de hosts en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="ae53f-173">To support assets in other environments when running from build output, call `UseStaticWebAssets` on the host builder in *Program.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStaticWebAssets();
                webBuilder.UseStartup<Startup>();
            });
}
```

<span data-ttu-id="ae53f-174">No es necesario llamar a `UseStaticWebAssets` si se ejecuta una aplicación desde la salida publicada (`dotnet publish`).</span><span class="sxs-lookup"><span data-stu-id="ae53f-174">Calling `UseStaticWebAssets` isn't required when running an app from published output (`dotnet publish`).</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="ae53f-175">Flujo de desarrollo de varios proyectos</span><span class="sxs-lookup"><span data-stu-id="ae53f-175">Multi-project development flow</span></span>

<span data-ttu-id="ae53f-176">Al ejecutar la aplicación de consumo:</span><span class="sxs-lookup"><span data-stu-id="ae53f-176">When the consuming app runs:</span></span>

* <span data-ttu-id="ae53f-177">Los recursos de la biblioteca de clases de Razor permanecen en sus carpetas originales.</span><span class="sxs-lookup"><span data-stu-id="ae53f-177">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="ae53f-178">Los recursos no se mueven a la aplicación de consumo.</span><span class="sxs-lookup"><span data-stu-id="ae53f-178">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="ae53f-179">Cualquier cambio dentro de la carpeta *wwwroot* de la RCL se refleja en la aplicación de consumo después de que se vuelva a compilar la RCL y sin tener que recompilar la aplicación de consumo.</span><span class="sxs-lookup"><span data-stu-id="ae53f-179">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="ae53f-180">Cuando se compila la biblioteca de clases de Razor, se genera un manifiesto que describe las ubicaciones de los recursos web estáticos.</span><span class="sxs-lookup"><span data-stu-id="ae53f-180">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="ae53f-181">La aplicación de consumo lee el manifiesto en tiempo de ejecución para consumir los recursos de los proyectos y paquetes a los que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="ae53f-181">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="ae53f-182">Cuando se agrega un nuevo recurso a una RCL, esta se debe recompilar para actualizar el manifiesto antes de que una aplicación de consumo pueda acceder al nuevo recurso.</span><span class="sxs-lookup"><span data-stu-id="ae53f-182">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="ae53f-183">Publicar</span><span class="sxs-lookup"><span data-stu-id="ae53f-183">Publish</span></span>

<span data-ttu-id="ae53f-184">Cuando se publica la aplicación, los recursos complementarios de todos los proyectos y paquetes a los que se hace referencia se copian en la carpeta *wwwroot* de la aplicación publicada en `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="ae53f-184">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ae53f-185">Las vistas, las páginas, los controladores, los modelos de página, los [componentes](xref:blazor/class-libraries), los modelos de datos y los [componentes de vista](xref:mvc/views/view-components) de Razor se pueden integrar en una biblioteca de clases de Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="ae53f-185">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="ae53f-186">Las RCL se pueden empaquetar y reutilizar.</span><span class="sxs-lookup"><span data-stu-id="ae53f-186">The RCL can be packaged and reused.</span></span> <span data-ttu-id="ae53f-187">Las aplicaciones pueden incluir la RCL y reemplazar las vistas y páginas que contienen.</span><span class="sxs-lookup"><span data-stu-id="ae53f-187">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="ae53f-188">Cuando existe una vista, vista parcial o página de Razor tanto en la aplicación web como en la RCL, tendrá prioridad el marcado de Razor (archivo *.cshtml*) de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="ae53f-188">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="ae53f-189">[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ae53f-189">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="ae53f-190">Crear una biblioteca de clases que contenga interfaz de usuario de Razor</span><span class="sxs-lookup"><span data-stu-id="ae53f-190">Create a class library containing Razor UI</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="ae53f-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ae53f-191">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ae53f-192">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="ae53f-192">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="ae53f-193">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ae53f-193">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="ae53f-194">Asigne un nombre a la biblioteca (por ejemplo, "RazorClassLib") > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ae53f-194">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="ae53f-195">Para evitar un conflicto de nombres de archivo con la biblioteca de vistas generada, asegúrese de que el nombre de la biblioteca no acaba en `.Views`.</span><span class="sxs-lookup"><span data-stu-id="ae53f-195">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="ae53f-196">Confirme que **ASP.NET Core 2.1** o una versión posterior está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="ae53f-196">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="ae53f-197">Seleccione **Biblioteca de clases de Razor** > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ae53f-197">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="ae53f-198">Una RCL tiene el siguiente archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="ae53f-198">An RCL has the following project file:</span></span>

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-cli"></a>[<span data-ttu-id="ae53f-199">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="ae53f-199">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ae53f-200">Ejecute `dotnet new razorclasslib` desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="ae53f-200">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="ae53f-201">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ae53f-201">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="ae53f-202">Para más información, vea [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="ae53f-202">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="ae53f-203">Para evitar un conflicto de nombres de archivo con la biblioteca de vistas generada, asegúrese de que el nombre de la biblioteca no acaba en `.Views`.</span><span class="sxs-lookup"><span data-stu-id="ae53f-203">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="ae53f-204">Agregue archivos de Razor a la RCL.</span><span class="sxs-lookup"><span data-stu-id="ae53f-204">Add Razor files to the RCL.</span></span>

<span data-ttu-id="ae53f-205">Las plantillas de ASP.NET Core dan por sentado que el contenido de la RCL se encuentra en la carpeta *Áreas*.</span><span class="sxs-lookup"><span data-stu-id="ae53f-205">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="ae53f-206">Consulte la sección [Diseño de páginas de RCL](#rcl-pages-layout) para crear una RCL que exponga contenido en `~/Pages` y no en `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="ae53f-206">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="ae53f-207">Referencias al contenido de la RCL</span><span class="sxs-lookup"><span data-stu-id="ae53f-207">Reference RCL content</span></span>

<span data-ttu-id="ae53f-208">Los siguientes elementos pueden hacer referencia a la RCL:</span><span class="sxs-lookup"><span data-stu-id="ae53f-208">The RCL can be referenced by:</span></span>

* <span data-ttu-id="ae53f-209">Paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="ae53f-209">NuGet package.</span></span> <span data-ttu-id="ae53f-210">Vea [Creación de paquetes NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) y [Creación y publicación de un paquete NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="ae53f-210">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="ae53f-211">*{NombreDeProyecto}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="ae53f-211">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="ae53f-212">Vea [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="ae53f-212">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="ae53f-213">Tutorial: Creación de un proyecto de RCL y uso de este desde un proyecto de Razor Pages</span><span class="sxs-lookup"><span data-stu-id="ae53f-213">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="ae53f-214">En lugar de crearlo, puede descargar el [proyecto completo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) y comprobarlo.</span><span class="sxs-lookup"><span data-stu-id="ae53f-214">You can download the [complete project](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="ae53f-215">La descarga de ejemplo contiene más código y vínculos que hacen que el proyecto sea fácil de comprobar.</span><span class="sxs-lookup"><span data-stu-id="ae53f-215">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="ae53f-216">Puede dejar sus comentarios en [este problema de GitHub](https://github.com/dotnet/AspNetCore.Docs/issues/6098) sobre las descargas de ejemplo en comparación con las instrucciones paso a paso.</span><span class="sxs-lookup"><span data-stu-id="ae53f-216">You can leave feedback in [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="ae53f-217">Comprobar la aplicación de descarga</span><span class="sxs-lookup"><span data-stu-id="ae53f-217">Test the download app</span></span>

<span data-ttu-id="ae53f-218">Si no ha descargado la aplicación final y prefiere crear el proyecto de tutorial, omita este paso y vaya a la [siguiente sección](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="ae53f-218">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="ae53f-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ae53f-219">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ae53f-220">Abra el archivo *.sln* en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ae53f-220">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="ae53f-221">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ae53f-221">Run the app.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="ae53f-222">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="ae53f-222">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ae53f-223">Desde un símbolo del sistema en el directorio *cli*, cree la RCL y la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="ae53f-223">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="ae53f-224">Vaya al directorio *WebApp1* y ejecute la aplicación:</span><span class="sxs-lookup"><span data-stu-id="ae53f-224">Move to the *WebApp1* directory and run the app:</span></span>

```dotnetcli
dotnet run
```

---

<span data-ttu-id="ae53f-225">Siga las instrucciones de [Probar WebApp1](#test-webapp1).</span><span class="sxs-lookup"><span data-stu-id="ae53f-225">Follow the instructions in [Test WebApp1](#test-webapp1)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="ae53f-226">Creación de una RCL</span><span class="sxs-lookup"><span data-stu-id="ae53f-226">Create an RCL</span></span>

<span data-ttu-id="ae53f-227">En esta sección, se creará una RCL</span><span class="sxs-lookup"><span data-stu-id="ae53f-227">In this section, an RCL is created.</span></span> <span data-ttu-id="ae53f-228">y se agregarán a ella archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="ae53f-228">Razor files are added to the RCL.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="ae53f-229">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ae53f-229">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ae53f-230">Cree el proyecto de RCL:</span><span class="sxs-lookup"><span data-stu-id="ae53f-230">Create the RCL project:</span></span>

* <span data-ttu-id="ae53f-231">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="ae53f-231">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="ae53f-232">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ae53f-232">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="ae53f-233">Asigne a la aplicación el nombre **RazorUIClassLib** > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ae53f-233">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="ae53f-234">Confirme que **ASP.NET Core 2.1** o una versión posterior está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="ae53f-234">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="ae53f-235">Seleccione **Biblioteca de clases de Razor** > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ae53f-235">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="ae53f-236">Agregue un archivo de vista parcial de Razor denominado *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ae53f-236">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="ae53f-237">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="ae53f-237">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ae53f-238">Ejecute lo siguiente desde la línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="ae53f-238">From the command line, run the following:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="ae53f-239">Los comandos anteriores:</span><span class="sxs-lookup"><span data-stu-id="ae53f-239">The preceding commands:</span></span>

* <span data-ttu-id="ae53f-240">Crean la biblioteca de clases de Razor `RazorUIClassLib`.</span><span class="sxs-lookup"><span data-stu-id="ae53f-240">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="ae53f-241">Crean una página _Message de Razor y la agrega a la RCL.</span><span class="sxs-lookup"><span data-stu-id="ae53f-241">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="ae53f-242">El parámetro `-np` crea la página sin un `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="ae53f-242">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="ae53f-243">Crean un archivo [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) y lo agregan a la RCL.</span><span class="sxs-lookup"><span data-stu-id="ae53f-243">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="ae53f-244">El archivo *_ViewStart.cshtml* es necesario para poder usar el diseño del proyecto de Razor Pages (que agregaremos en la siguiente sección).</span><span class="sxs-lookup"><span data-stu-id="ae53f-244">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="ae53f-245">Adición de archivos y carpetas de Razor al proyecto</span><span class="sxs-lookup"><span data-stu-id="ae53f-245">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="ae53f-246">Reemplace el marcado de *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="ae53f-246">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="ae53f-247">Reemplace el marcado de *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="ae53f-247">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  <span data-ttu-id="ae53f-248">Se necesita `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` para usar la vista parcial (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="ae53f-248">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="ae53f-249">En lugar de incluir la directiva `@addTagHelper`, puede agregar un archivo *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ae53f-249">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="ae53f-250">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ae53f-250">For example:</span></span>

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  <span data-ttu-id="ae53f-251">Para obtener más información sobre *_ViewImports.cshtml*, consulte la sección [Importar directivas compartidas](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="ae53f-251">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="ae53f-252">Compile la biblioteca de clases para confirmar que no hay ningún error de compilador:</span><span class="sxs-lookup"><span data-stu-id="ae53f-252">Build the class library to verify there are no compiler errors:</span></span>

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

<span data-ttu-id="ae53f-253">El resultado de la compilación contiene *RazorUIClassLib.dll* y *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="ae53f-253">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="ae53f-254">*RazorUIClassLib.Views.dll* incluye el contenido de Razor compilado.</span><span class="sxs-lookup"><span data-stu-id="ae53f-254">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="ae53f-255">Usar la biblioteca de interfaz de usuario de Razor desde un proyecto de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="ae53f-255">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="ae53f-256">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ae53f-256">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ae53f-257">Cree la aplicación web de páginas de Razor:</span><span class="sxs-lookup"><span data-stu-id="ae53f-257">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="ae53f-258">En el **Explorador de soluciones**, haga clic con el botón derecho en la solución > **Agregar** > **Nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="ae53f-258">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="ae53f-259">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ae53f-259">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="ae53f-260">Denomine la aplicación **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="ae53f-260">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="ae53f-261">Confirme que **ASP.NET Core 2.1** o una versión posterior está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="ae53f-261">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="ae53f-262">Seleccione **Aplicación web** > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ae53f-262">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="ae53f-263">En el **Explorador de soluciones**, haga clic con el botón derecho en **WebApp1** y seleccione **Establecer como proyecto de inicio**.</span><span class="sxs-lookup"><span data-stu-id="ae53f-263">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="ae53f-264">En el **Explorador de soluciones**, haga clic con el botón derecho en **WebApp1** y seleccione **Dependencias de compilación** > **Dependencias del proyecto**.</span><span class="sxs-lookup"><span data-stu-id="ae53f-264">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="ae53f-265">Marque **RazorUIClassLib** como dependencia de **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="ae53f-265">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="ae53f-266">En el **Explorador de soluciones**, haga clic con el botón derecho en **WebApp1** y seleccione **Agregar** > **Referencia**.</span><span class="sxs-lookup"><span data-stu-id="ae53f-266">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="ae53f-267">En el cuadro de diálogo **Administrador de referencias**, marque la casilla **RazorUIClassLib** > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ae53f-267">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="ae53f-268">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ae53f-268">Run the app.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="ae53f-269">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="ae53f-269">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ae53f-270">Cree una aplicación web de Razor Pages y un archivo de solución que contenga dicha aplicación y la biblioteca de clases de Razor:</span><span class="sxs-lookup"><span data-stu-id="ae53f-270">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="ae53f-271">Compile y ejecute la aplicación web:</span><span class="sxs-lookup"><span data-stu-id="ae53f-271">Build and run the web app:</span></span>

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a><span data-ttu-id="ae53f-272">Probar WebApp1</span><span class="sxs-lookup"><span data-stu-id="ae53f-272">Test WebApp1</span></span>

<span data-ttu-id="ae53f-273">Vaya a `/MyFeature/Page1` para comprobar que la biblioteca de clases de la interfaz de usuario de Razor esté en uso.</span><span class="sxs-lookup"><span data-stu-id="ae53f-273">Browse to `/MyFeature/Page1` to verify that the Razor UI class library is in use.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="ae53f-274">Reemplazar vistas, vistas parciales y páginas</span><span class="sxs-lookup"><span data-stu-id="ae53f-274">Override views, partial views, and pages</span></span>

<span data-ttu-id="ae53f-275">Cuando existe una vista, vista parcial o página de Razor tanto en la aplicación web como en la RCL, tendrá prioridad el marcado de Razor (archivo *.cshtml*) de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="ae53f-275">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="ae53f-276">Por ejemplo, si agrega *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* a WebApp1, Page1 en WebApp1 prevalecerá sobre Page1 en la biblioteca de clases de Razor.</span><span class="sxs-lookup"><span data-stu-id="ae53f-276">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="ae53f-277">En la descarga de ejemplo, cambie el nombre *WebApp1/Areas/MyFeature2* por *WebApp1/Areas/MyFeature* para comprobar la prioridad.</span><span class="sxs-lookup"><span data-stu-id="ae53f-277">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="ae53f-278">Copie la vista parcial *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* en *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ae53f-278">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="ae53f-279">Actualice el marcado para señalar la nueva ubicación.</span><span class="sxs-lookup"><span data-stu-id="ae53f-279">Update the markup to indicate the new location.</span></span> <span data-ttu-id="ae53f-280">Compile y ejecute la aplicación para comprobar si se está usando la versión de la vista parcial de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ae53f-280">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="ae53f-281">Diseño de páginas de RCL</span><span class="sxs-lookup"><span data-stu-id="ae53f-281">RCL Pages layout</span></span>

<span data-ttu-id="ae53f-282">Para hacer referencia al contenido de la RCL como si formara parte de la carpeta *Pages* de la aplicación web, cree el proyecto RCL con la siguiente estructura de archivos:</span><span class="sxs-lookup"><span data-stu-id="ae53f-282">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="ae53f-283">*RazorUIClassLib/Pages*</span><span class="sxs-lookup"><span data-stu-id="ae53f-283">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="ae53f-284">*RazorUIClassLib/Pages/Shared*</span><span class="sxs-lookup"><span data-stu-id="ae53f-284">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="ae53f-285">Imagine que *RazorUIClassLib/Pages/Shared* contiene dos archivos parciales: *_Header.cshtml* y *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ae53f-285">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="ae53f-286">En ese caso, se podrían agregar etiquetas `<partial>` al archivo *_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ae53f-286">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="ae53f-287">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ae53f-287">Additional resources</span></span>

* <xref:blazor/class-libraries>
