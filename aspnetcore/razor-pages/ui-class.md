---
title: Interfaz de usuario de Razor reutilizable en bibliotecas de clases con ASP.NET Core
author: Rick-Anderson
description: Explica cómo crear la interfaz de usuario Razor reutilizable con las vistas parciales en una biblioteca de clases en ASP.NET Core.
ms.author: riande
ms.date: 01/25/2020
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 8813244ea6d00b170d9f95d12743e9fee38bf810
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172647"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="dedea-103">Crear una interfaz de usuario reutilizable mediante el proyecto de biblioteca de clases de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dedea-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="dedea-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dedea-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="dedea-105">Las vistas, páginas, controladores, modelos de páginas, [componentes de Razor](xref:blazor/class-libraries), componentes de [vista](xref:mvc/views/view-components)y modelos de datos de Razor se pueden integrar en una biblioteca de clases de Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="dedea-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="dedea-106">Las RCL se pueden empaquetar y reutilizar.</span><span class="sxs-lookup"><span data-stu-id="dedea-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="dedea-107">Las aplicaciones pueden incluir la RCL y reemplazar las vistas y páginas que contienen.</span><span class="sxs-lookup"><span data-stu-id="dedea-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="dedea-108">Cuando existe una vista, vista parcial o página de Razor tanto en la aplicación web como en la RCL, tendrá prioridad el marcado de Razor (archivo *.cshtml*) de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="dedea-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="dedea-109">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dedea-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="dedea-110">Crear una biblioteca de clases que contenga interfaz de usuario de Razor</span><span class="sxs-lookup"><span data-stu-id="dedea-110">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dedea-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dedea-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="dedea-112">En Visual Studio, seleccione **crear nuevo proyecto nuevo**.</span><span class="sxs-lookup"><span data-stu-id="dedea-112">From Visual Studio select **Create new a new project**.</span></span>
* <span data-ttu-id="dedea-113">Seleccione **biblioteca de clases de Razor** > **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="dedea-113">Select **Razor Class Library** > **Next**.</span></span>
* <span data-ttu-id="dedea-114">Asigne a la biblioteca el nombre (por ejemplo, "RazorClassLib"), > **crear**.</span><span class="sxs-lookup"><span data-stu-id="dedea-114">Name the library (for example, "RazorClassLib"), > **Create**.</span></span> <span data-ttu-id="dedea-115">Para evitar un conflicto de nombres de archivo con la biblioteca de vistas generada, asegúrese de que el nombre de la biblioteca no acaba en `.Views`.</span><span class="sxs-lookup"><span data-stu-id="dedea-115">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="dedea-116">Seleccione **páginas y vistas de soporte técnico** si necesita admitir vistas.</span><span class="sxs-lookup"><span data-stu-id="dedea-116">Select **Support pages and views** if you need to support views.</span></span> <span data-ttu-id="dedea-117">De forma predeterminada, solo se admiten Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="dedea-117">By default, only Razor Pages are supported.</span></span> <span data-ttu-id="dedea-118">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="dedea-118">Select **Create**.</span></span>

<span data-ttu-id="dedea-119">De forma predeterminada, la plantilla de la biblioteca de clases de Razor (RCL) utiliza el desarrollo de componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="dedea-119">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="dedea-120">La opción **páginas y vistas de soporte técnico** admite páginas y vistas.</span><span class="sxs-lookup"><span data-stu-id="dedea-120">The **Support pages and views** option supports pages and views.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="dedea-121">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="dedea-121">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="dedea-122">Ejecute `dotnet new razorclasslib` desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="dedea-122">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="dedea-123">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="dedea-123">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="dedea-124">De forma predeterminada, la plantilla de la biblioteca de clases de Razor (RCL) utiliza el desarrollo de componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="dedea-124">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="dedea-125">Pase la opción de `--support-pages-and-views` (`dotnet new razorclasslib --support-pages-and-views`) para proporcionar compatibilidad con páginas y vistas.</span><span class="sxs-lookup"><span data-stu-id="dedea-125">Pass the `--support-pages-and-views` option (`dotnet new razorclasslib --support-pages-and-views`) to provide support for pages and views.</span></span>

<span data-ttu-id="dedea-126">Para más información, vea [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="dedea-126">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="dedea-127">Para evitar un conflicto de nombres de archivo con la biblioteca de vistas generada, asegúrese de que el nombre de la biblioteca no acaba en `.Views`.</span><span class="sxs-lookup"><span data-stu-id="dedea-127">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="dedea-128">Agregue archivos de Razor a la RCL.</span><span class="sxs-lookup"><span data-stu-id="dedea-128">Add Razor files to the RCL.</span></span>

<span data-ttu-id="dedea-129">Las plantillas ASP.NET Core suponen que el contenido de RCL se encuentra en la carpeta *áreas* .</span><span class="sxs-lookup"><span data-stu-id="dedea-129">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="dedea-130">Consulte [diseño de páginas de RCL](#rcl-pages-layout) para crear un RCL que exponga contenido en `~/Pages` en lugar de `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="dedea-130">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="dedea-131">Contenido de RCL de referencia</span><span class="sxs-lookup"><span data-stu-id="dedea-131">Reference RCL content</span></span>

<span data-ttu-id="dedea-132">Los siguientes elementos pueden hacer referencia a la RCL:</span><span class="sxs-lookup"><span data-stu-id="dedea-132">The RCL can be referenced by:</span></span>

* <span data-ttu-id="dedea-133">Paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="dedea-133">NuGet package.</span></span> <span data-ttu-id="dedea-134">Vea [Creación de paquetes NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) y [Creación y publicación de un paquete NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="dedea-134">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="dedea-135">*{NombreDeProyecto}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="dedea-135">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="dedea-136">Vea [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="dedea-136">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="dedea-137">Reemplazar vistas, vistas parciales y páginas</span><span class="sxs-lookup"><span data-stu-id="dedea-137">Override views, partial views, and pages</span></span>

<span data-ttu-id="dedea-138">Cuando existe una vista, vista parcial o página de Razor tanto en la aplicación web como en la RCL, tendrá prioridad el marcado de Razor (archivo *.cshtml*) de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="dedea-138">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="dedea-139">Por ejemplo, agregue *WebApp1/areas/mi Feature/pages/Page1. cshtml* a WebApp1 y Page1 en WebApp1 tendrá prioridad sobre PÁGINA1 en RCL.</span><span class="sxs-lookup"><span data-stu-id="dedea-139">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="dedea-140">En la descarga de ejemplo, cambie el nombre *WebApp1/Areas/MyFeature2* por *WebApp1/Areas/MyFeature* para comprobar la prioridad.</span><span class="sxs-lookup"><span data-stu-id="dedea-140">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="dedea-141">Copie la vista parcial *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* en *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dedea-141">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="dedea-142">Actualice el marcado para señalar la nueva ubicación.</span><span class="sxs-lookup"><span data-stu-id="dedea-142">Update the markup to indicate the new location.</span></span> <span data-ttu-id="dedea-143">Compile y ejecute la aplicación para comprobar si se está usando la versión de la vista parcial de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dedea-143">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="dedea-144">Diseño de páginas RCL</span><span class="sxs-lookup"><span data-stu-id="dedea-144">RCL Pages layout</span></span>

<span data-ttu-id="dedea-145">Para hacer referencia al contenido de RCL como si formara parte de la carpeta *páginas* de la aplicación Web, cree el proyecto RCL con la siguiente estructura de archivos:</span><span class="sxs-lookup"><span data-stu-id="dedea-145">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="dedea-146">*RazorUIClassLib/páginas*</span><span class="sxs-lookup"><span data-stu-id="dedea-146">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="dedea-147">*RazorUIClassLib/páginas/compartido*</span><span class="sxs-lookup"><span data-stu-id="dedea-147">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="dedea-148">Supongamos que *RazorUIClassLib/pages/Shared* contiene dos archivos parciales: *_Header. cshtml* y *_Footer. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dedea-148">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="dedea-149">Las etiquetas de `<partial>` se pueden agregar al archivo *_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="dedea-149">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="dedea-150">Creación de un RCL con recursos estáticos</span><span class="sxs-lookup"><span data-stu-id="dedea-150">Create an RCL with static assets</span></span>

<span data-ttu-id="dedea-151">Una RCL puede requerir recursos estáticos complementarios a los que puede hacer referencia el RCL o la aplicación de consumo de RCL.</span><span class="sxs-lookup"><span data-stu-id="dedea-151">An RCL may require companion static assets that can be referenced by either the RCL or the consuming app of the RCL.</span></span> <span data-ttu-id="dedea-152">ASP.NET Core permite crear RCLs que incluyen recursos estáticos que están disponibles para una aplicación de consumo.</span><span class="sxs-lookup"><span data-stu-id="dedea-152">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="dedea-153">Para incluir los recursos complementarios como parte de un RCL, cree una carpeta *wwwroot* en la biblioteca de clases e incluya los archivos necesarios en esa carpeta.</span><span class="sxs-lookup"><span data-stu-id="dedea-153">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="dedea-154">Al empaquetar un RCL, todos los recursos complementarios de la carpeta *wwwroot* se incluyen automáticamente en el paquete.</span><span class="sxs-lookup"><span data-stu-id="dedea-154">When packing an RCL, all companion assets in the *wwwroot* folder are automatically included in the package.</span></span>

### <a name="exclude-static-assets"></a><span data-ttu-id="dedea-155">Excluir recursos estáticos</span><span class="sxs-lookup"><span data-stu-id="dedea-155">Exclude static assets</span></span>

<span data-ttu-id="dedea-156">Para excluir recursos estáticos, agregue la ruta de exclusión deseada al grupo de propiedades `$(DefaultItemExcludes)` en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="dedea-156">To exclude static assets, add the desired exclusion path to the `$(DefaultItemExcludes)` property group in the project file.</span></span> <span data-ttu-id="dedea-157">Separe las entradas con un punto y coma (`;`).</span><span class="sxs-lookup"><span data-stu-id="dedea-157">Separate entries with a semicolon (`;`).</span></span>

<span data-ttu-id="dedea-158">En el ejemplo siguiente, la hoja de estilos *lib. CSS* de la carpeta *wwwroot* no se considera un recurso estático y no se incluye en el RCL publicado:</span><span class="sxs-lookup"><span data-stu-id="dedea-158">In the following example, the *lib.css* stylesheet in the *wwwroot* folder isn't considered a static asset and isn't included in the published RCL:</span></span>

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a><span data-ttu-id="dedea-159">Integración de Typescript</span><span class="sxs-lookup"><span data-stu-id="dedea-159">Typescript integration</span></span>

<span data-ttu-id="dedea-160">Para incluir los archivos TypeScript en un RCL:</span><span class="sxs-lookup"><span data-stu-id="dedea-160">To include TypeScript files in an RCL:</span></span>

1. <span data-ttu-id="dedea-161">Coloque los archivos TypeScript ( *. ts*) fuera de la carpeta *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="dedea-161">Place the TypeScript files (*.ts*) outside of the *wwwroot* folder.</span></span> <span data-ttu-id="dedea-162">Por ejemplo, coloque los archivos en una carpeta de *cliente* .</span><span class="sxs-lookup"><span data-stu-id="dedea-162">For example, place the files in a *Client* folder.</span></span>

1. <span data-ttu-id="dedea-163">Configure el resultado de la compilación de TypeScript para la carpeta *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="dedea-163">Configure the TypeScript build output for the *wwwroot* folder.</span></span> <span data-ttu-id="dedea-164">Establezca la propiedad `TypescriptOutDir` dentro de un `PropertyGroup` en el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="dedea-164">Set the `TypescriptOutDir` property inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. <span data-ttu-id="dedea-165">Incluya el destino de TypeScript como una dependencia del destino de `ResolveCurrentProjectStaticWebAssets` agregando el siguiente destino dentro de un `PropertyGroup` en el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="dedea-165">Include the TypeScript target as a dependency of the `ResolveCurrentProjectStaticWebAssets` target by adding the following target inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     CompileTypeScript;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="dedea-166">Consumo de contenido de una RCL a la que se hace referencia</span><span class="sxs-lookup"><span data-stu-id="dedea-166">Consume content from a referenced RCL</span></span>

<span data-ttu-id="dedea-167">Los archivos incluidos en la carpeta *wwwroot* de RCL se exponen a RCL o a la aplicación de consumo en el prefijo `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="dedea-167">The files included in the *wwwroot* folder of the RCL are exposed to either the RCL or the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="dedea-168">Por ejemplo, una biblioteca denominada *Razor. class. lib* da como resultado una ruta de acceso al contenido estático en `_content/Razor.Class.Lib/`.</span><span class="sxs-lookup"><span data-stu-id="dedea-168">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span> <span data-ttu-id="dedea-169">Al generar un paquete de NuGet y el nombre de ensamblado no es el mismo que el identificador de paquete, use el identificador de paquete para `{LIBRARY NAME}`.</span><span class="sxs-lookup"><span data-stu-id="dedea-169">When producing a NuGet package and the assembly name isn't the same as the package ID, use the package ID for `{LIBRARY NAME}`.</span></span>

<span data-ttu-id="dedea-170">La aplicación de consumo hace referencia a los recursos estáticos proporcionados por la biblioteca con `<script>`, `<style>`, `<img>`y otras etiquetas HTML.</span><span class="sxs-lookup"><span data-stu-id="dedea-170">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="dedea-171">La aplicación de consumo debe tener habilitada la [compatibilidad con archivos estáticos](xref:fundamentals/static-files) en `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="dedea-171">The consuming app must have [static file support](xref:fundamentals/static-files) enabled in `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="dedea-172">Al ejecutar la aplicación de consumo desde la salida de la compilación (`dotnet run`), los activos web estáticos están habilitados de forma predeterminada en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="dedea-172">When running the consuming app from build output (`dotnet run`), static web assets are enabled by default in the Development environment.</span></span> <span data-ttu-id="dedea-173">Para admitir recursos en otros entornos cuando se ejecutan desde la salida de la compilación, llame a `UseStaticWebAssets` en el generador de hosts en *Program.CS*:</span><span class="sxs-lookup"><span data-stu-id="dedea-173">To support assets in other environments when running from build output, call `UseStaticWebAssets` on the host builder in *Program.cs*:</span></span>

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

<span data-ttu-id="dedea-174">No es necesario llamar a `UseStaticWebAssets` cuando se ejecuta una aplicación desde la salida publicada (`dotnet publish`).</span><span class="sxs-lookup"><span data-stu-id="dedea-174">Calling `UseStaticWebAssets` isn't required when running an app from published output (`dotnet publish`).</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="dedea-175">Flujo de desarrollo de varios proyectos</span><span class="sxs-lookup"><span data-stu-id="dedea-175">Multi-project development flow</span></span>

<span data-ttu-id="dedea-176">Cuando se ejecuta la aplicación de consumo:</span><span class="sxs-lookup"><span data-stu-id="dedea-176">When the consuming app runs:</span></span>

* <span data-ttu-id="dedea-177">Los recursos de RCL permanecen en sus carpetas originales.</span><span class="sxs-lookup"><span data-stu-id="dedea-177">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="dedea-178">Los recursos no se mueven a la aplicación de consumo.</span><span class="sxs-lookup"><span data-stu-id="dedea-178">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="dedea-179">Cualquier cambio dentro de la carpeta *wwwroot* de RCL se refleja en la aplicación de consumo después de que se vuelva a generar RCL y sin volver a generar la aplicación de consumo.</span><span class="sxs-lookup"><span data-stu-id="dedea-179">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="dedea-180">Cuando se compila el RCL, se genera un manifiesto que describe las ubicaciones de los recursos web estáticos.</span><span class="sxs-lookup"><span data-stu-id="dedea-180">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="dedea-181">La aplicación consumidora lee el manifiesto en tiempo de ejecución para consumir los recursos de los proyectos y paquetes a los que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="dedea-181">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="dedea-182">Cuando se agrega un nuevo recurso a un RCL, se debe volver a generar el RCL para actualizar el manifiesto antes de que una aplicación de consumo pueda acceder al nuevo recurso.</span><span class="sxs-lookup"><span data-stu-id="dedea-182">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="dedea-183">Publicar</span><span class="sxs-lookup"><span data-stu-id="dedea-183">Publish</span></span>

<span data-ttu-id="dedea-184">Cuando se publica la aplicación, los recursos complementarios de todos los proyectos y paquetes a los que se hace referencia se copian en la carpeta *wwwroot* de la aplicación publicada en `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="dedea-184">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="dedea-185">Las vistas, páginas, controladores, modelos de páginas, [componentes de Razor](xref:blazor/class-libraries), componentes de [vista](xref:mvc/views/view-components)y modelos de datos de Razor se pueden integrar en una biblioteca de clases de Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="dedea-185">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="dedea-186">Las RCL se pueden empaquetar y reutilizar.</span><span class="sxs-lookup"><span data-stu-id="dedea-186">The RCL can be packaged and reused.</span></span> <span data-ttu-id="dedea-187">Las aplicaciones pueden incluir la RCL y reemplazar las vistas y páginas que contienen.</span><span class="sxs-lookup"><span data-stu-id="dedea-187">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="dedea-188">Cuando existe una vista, vista parcial o página de Razor tanto en la aplicación web como en la RCL, tendrá prioridad el marcado de Razor (archivo *.cshtml*) de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="dedea-188">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="dedea-189">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dedea-189">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="dedea-190">Crear una biblioteca de clases que contenga interfaz de usuario de Razor</span><span class="sxs-lookup"><span data-stu-id="dedea-190">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dedea-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dedea-191">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="dedea-192">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="dedea-192">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="dedea-193">Seleccione **Aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="dedea-193">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="dedea-194">Asigne un nombre a la biblioteca (por ejemplo, "RazorClassLib") > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="dedea-194">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="dedea-195">Para evitar un conflicto de nombres de archivo con la biblioteca de vistas generada, asegúrese de que el nombre de la biblioteca no acaba en `.Views`.</span><span class="sxs-lookup"><span data-stu-id="dedea-195">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="dedea-196">Confirme que **ASP.NET Core 2.1** o una versión posterior está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="dedea-196">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="dedea-197">Seleccione **biblioteca de clases de Razor** > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="dedea-197">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="dedea-198">Un RCL tiene el siguiente archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="dedea-198">An RCL has the following project file:</span></span>

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="dedea-199">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="dedea-199">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="dedea-200">Ejecute `dotnet new razorclasslib` desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="dedea-200">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="dedea-201">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="dedea-201">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="dedea-202">Para más información, vea [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="dedea-202">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="dedea-203">Para evitar un conflicto de nombres de archivo con la biblioteca de vistas generada, asegúrese de que el nombre de la biblioteca no acaba en `.Views`.</span><span class="sxs-lookup"><span data-stu-id="dedea-203">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="dedea-204">Agregue archivos de Razor a la RCL.</span><span class="sxs-lookup"><span data-stu-id="dedea-204">Add Razor files to the RCL.</span></span>

<span data-ttu-id="dedea-205">Las plantillas ASP.NET Core suponen que el contenido de RCL se encuentra en la carpeta *áreas* .</span><span class="sxs-lookup"><span data-stu-id="dedea-205">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="dedea-206">Consulte [diseño de páginas de RCL](#rcl-pages-layout) para crear un RCL que exponga contenido en `~/Pages` en lugar de `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="dedea-206">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="dedea-207">Contenido de RCL de referencia</span><span class="sxs-lookup"><span data-stu-id="dedea-207">Reference RCL content</span></span>

<span data-ttu-id="dedea-208">Los siguientes elementos pueden hacer referencia a la RCL:</span><span class="sxs-lookup"><span data-stu-id="dedea-208">The RCL can be referenced by:</span></span>

* <span data-ttu-id="dedea-209">Paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="dedea-209">NuGet package.</span></span> <span data-ttu-id="dedea-210">Vea [Creación de paquetes NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) y [Creación y publicación de un paquete NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="dedea-210">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="dedea-211">*{NombreDeProyecto}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="dedea-211">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="dedea-212">Vea [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="dedea-212">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="dedea-213">Tutorial: creación de un proyecto de RCL y uso de un proyecto de Razor Pages</span><span class="sxs-lookup"><span data-stu-id="dedea-213">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="dedea-214">En lugar de crearlo, puede descargar el [proyecto completo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) y comprobarlo.</span><span class="sxs-lookup"><span data-stu-id="dedea-214">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="dedea-215">La descarga de ejemplo contiene más código y vínculos que hacen que el proyecto sea fácil de comprobar.</span><span class="sxs-lookup"><span data-stu-id="dedea-215">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="dedea-216">Puede dejar sus comentarios en [este problema de GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6098) sobre las descargas de ejemplo en comparación con las instrucciones paso a paso.</span><span class="sxs-lookup"><span data-stu-id="dedea-216">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="dedea-217">Comprobar la aplicación de descarga</span><span class="sxs-lookup"><span data-stu-id="dedea-217">Test the download app</span></span>

<span data-ttu-id="dedea-218">Si no ha descargado la aplicación final y prefiere crear el proyecto de tutorial, omita este paso y vaya a la [siguiente sección](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="dedea-218">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dedea-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dedea-219">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dedea-220">Abra el archivo *.sln* en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dedea-220">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="dedea-221">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dedea-221">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="dedea-222">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="dedea-222">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="dedea-223">Desde un símbolo del sistema en el directorio *cli*, cree la RCL y la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="dedea-223">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="dedea-224">Vaya al directorio *WebApp1* y ejecute la aplicación:</span><span class="sxs-lookup"><span data-stu-id="dedea-224">Move to the *WebApp1* directory and run the app:</span></span>

```dotnetcli
dotnet run
```

---

<span data-ttu-id="dedea-225">Siga las instrucciones de [Probar WebApp1](#test-webapp1).</span><span class="sxs-lookup"><span data-stu-id="dedea-225">Follow the instructions in [Test WebApp1](#test-webapp1)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="dedea-226">Creación de un RCL</span><span class="sxs-lookup"><span data-stu-id="dedea-226">Create an RCL</span></span>

<span data-ttu-id="dedea-227">En esta sección, se crea un RCL.</span><span class="sxs-lookup"><span data-stu-id="dedea-227">In this section, an RCL is created.</span></span> <span data-ttu-id="dedea-228">y se agregarán a ella archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="dedea-228">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dedea-229">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dedea-229">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dedea-230">Cree el proyecto de RCL:</span><span class="sxs-lookup"><span data-stu-id="dedea-230">Create the RCL project:</span></span>

* <span data-ttu-id="dedea-231">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="dedea-231">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="dedea-232">Seleccione **Aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="dedea-232">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="dedea-233">Asigne a la aplicación el nombre **RazorUIClassLib** > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="dedea-233">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="dedea-234">Confirme que **ASP.NET Core 2.1** o una versión posterior está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="dedea-234">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="dedea-235">Seleccione **biblioteca de clases de Razor** > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="dedea-235">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="dedea-236">Agregue un archivo de vista parcial de Razor denominado *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dedea-236">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="dedea-237">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="dedea-237">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="dedea-238">Ejecute lo siguiente desde la línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="dedea-238">From the command line, run the following:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="dedea-239">Los comandos anteriores:</span><span class="sxs-lookup"><span data-stu-id="dedea-239">The preceding commands:</span></span>

* <span data-ttu-id="dedea-240">Crea el `RazorUIClassLib` RCL.</span><span class="sxs-lookup"><span data-stu-id="dedea-240">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="dedea-241">Crean una página _Message de Razor y la agrega a la RCL.</span><span class="sxs-lookup"><span data-stu-id="dedea-241">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="dedea-242">El parámetro `-np` crea la página sin un `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="dedea-242">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="dedea-243">Crea un archivo [_ViewStart. cshtml](xref:mvc/views/layout#running-code-before-each-view) y lo agrega a RCL.</span><span class="sxs-lookup"><span data-stu-id="dedea-243">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="dedea-244">El archivo *_ViewStart. cshtml* es necesario para usar el diseño del proyecto Razor Pages (que se agrega en la sección siguiente).</span><span class="sxs-lookup"><span data-stu-id="dedea-244">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="dedea-245">Agregar archivos de Razor y carpetas al proyecto</span><span class="sxs-lookup"><span data-stu-id="dedea-245">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="dedea-246">Reemplace el marcado de *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="dedea-246">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="dedea-247">Reemplace el marcado de *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="dedea-247">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  <span data-ttu-id="dedea-248">Se necesita `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` para usar la vista parcial (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="dedea-248">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="dedea-249">En lugar de incluir la directiva `@addTagHelper`, puede agregar un archivo *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dedea-249">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="dedea-250">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="dedea-250">For example:</span></span>

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  <span data-ttu-id="dedea-251">Para obtener más información sobre *_ViewImports. cshtml*, vea [importar directivas compartidas](xref:mvc/views/layout#importing-shared-directives) .</span><span class="sxs-lookup"><span data-stu-id="dedea-251">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="dedea-252">Compile la biblioteca de clases para confirmar que no hay ningún error de compilador:</span><span class="sxs-lookup"><span data-stu-id="dedea-252">Build the class library to verify there are no compiler errors:</span></span>

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

<span data-ttu-id="dedea-253">El resultado de la compilación contiene *RazorUIClassLib.dll* y *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="dedea-253">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="dedea-254">*RazorUIClassLib.Views.dll* incluye el contenido de Razor compilado.</span><span class="sxs-lookup"><span data-stu-id="dedea-254">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="dedea-255">Usar la biblioteca de interfaz de usuario de Razor desde un proyecto de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="dedea-255">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dedea-256">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dedea-256">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dedea-257">Cree la aplicación web de páginas de Razor:</span><span class="sxs-lookup"><span data-stu-id="dedea-257">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="dedea-258">En **Explorador de soluciones**, haga clic con el botón secundario en la solución > **Agregar** >**nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="dedea-258">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="dedea-259">Seleccione **Aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="dedea-259">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="dedea-260">Denomine la aplicación **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="dedea-260">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="dedea-261">Confirme que **ASP.NET Core 2.1** o una versión posterior está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="dedea-261">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="dedea-262">Seleccione **aplicación Web** > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="dedea-262">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="dedea-263">En el **Explorador de soluciones**, haga clic con el botón derecho en **WebApp1** y seleccione **Establecer como proyecto de inicio**.</span><span class="sxs-lookup"><span data-stu-id="dedea-263">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="dedea-264">En **Explorador de soluciones**, haga clic con el botón derecho en **WebApp1** y seleccione **dependencias de compilación** > **dependencias del proyecto**.</span><span class="sxs-lookup"><span data-stu-id="dedea-264">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="dedea-265">Marque **RazorUIClassLib** como dependencia de **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="dedea-265">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="dedea-266">En **Explorador de soluciones**, haga clic con el botón derecho en **WebApp1** y seleccione **Agregar** **referencia**de >.</span><span class="sxs-lookup"><span data-stu-id="dedea-266">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="dedea-267">En el cuadro de diálogo **Administrador de referencias** , active **RazorUIClassLib** > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="dedea-267">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="dedea-268">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dedea-268">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="dedea-269">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="dedea-269">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="dedea-270">Cree una aplicación Web de Razor Pages y un archivo de solución que contenga la aplicación Razor Pages y RCL:</span><span class="sxs-lookup"><span data-stu-id="dedea-270">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="dedea-271">Compile y ejecute la aplicación web:</span><span class="sxs-lookup"><span data-stu-id="dedea-271">Build and run the web app:</span></span>

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a><span data-ttu-id="dedea-272">Probar WebApp1</span><span class="sxs-lookup"><span data-stu-id="dedea-272">Test WebApp1</span></span>

<span data-ttu-id="dedea-273">Vaya a `/MyFeature/Page1` para comprobar que la biblioteca de clases de la interfaz de usuario de Razor está en uso.</span><span class="sxs-lookup"><span data-stu-id="dedea-273">Browse to `/MyFeature/Page1` to verify that the Razor UI class library is in use.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="dedea-274">Reemplazar vistas, vistas parciales y páginas</span><span class="sxs-lookup"><span data-stu-id="dedea-274">Override views, partial views, and pages</span></span>

<span data-ttu-id="dedea-275">Cuando existe una vista, vista parcial o página de Razor tanto en la aplicación web como en la RCL, tendrá prioridad el marcado de Razor (archivo *.cshtml*) de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="dedea-275">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="dedea-276">Por ejemplo, agregue *WebApp1/areas/mi Feature/pages/Page1. cshtml* a WebApp1 y Page1 en WebApp1 tendrá prioridad sobre PÁGINA1 en RCL.</span><span class="sxs-lookup"><span data-stu-id="dedea-276">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="dedea-277">En la descarga de ejemplo, cambie el nombre *WebApp1/Areas/MyFeature2* por *WebApp1/Areas/MyFeature* para comprobar la prioridad.</span><span class="sxs-lookup"><span data-stu-id="dedea-277">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="dedea-278">Copie la vista parcial *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* en *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dedea-278">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="dedea-279">Actualice el marcado para señalar la nueva ubicación.</span><span class="sxs-lookup"><span data-stu-id="dedea-279">Update the markup to indicate the new location.</span></span> <span data-ttu-id="dedea-280">Compile y ejecute la aplicación para comprobar si se está usando la versión de la vista parcial de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dedea-280">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="dedea-281">Diseño de páginas RCL</span><span class="sxs-lookup"><span data-stu-id="dedea-281">RCL Pages layout</span></span>

<span data-ttu-id="dedea-282">Para hacer referencia al contenido de RCL como si formara parte de la carpeta *páginas* de la aplicación Web, cree el proyecto RCL con la siguiente estructura de archivos:</span><span class="sxs-lookup"><span data-stu-id="dedea-282">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="dedea-283">*RazorUIClassLib/páginas*</span><span class="sxs-lookup"><span data-stu-id="dedea-283">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="dedea-284">*RazorUIClassLib/páginas/compartido*</span><span class="sxs-lookup"><span data-stu-id="dedea-284">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="dedea-285">Supongamos que *RazorUIClassLib/pages/Shared* contiene dos archivos parciales: *_Header. cshtml* y *_Footer. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dedea-285">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="dedea-286">Las etiquetas de `<partial>` se pueden agregar al archivo *_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="dedea-286">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="dedea-287">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="dedea-287">Additional resources</span></span>

* <xref:blazor/class-libraries>
