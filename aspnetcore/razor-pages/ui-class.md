---
title: Interfaz de usuario de Razor reutilizable en bibliotecas de clases con ASP.NET Core
author: Rick-Anderson
description: Explica cómo crear la interfaz de usuario Razor reutilizable con las vistas parciales en una biblioteca de clases en ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 10/08/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: d656e924033f1b217cdd8c86f7d00411c5d71beb
ms.sourcegitcommit: 73a451e9a58ac7102f90b608d661d8c23dd9bbaf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72037589"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="0c46d-103">Crear una interfaz de usuario reutilizable mediante el proyecto de biblioteca de clases de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c46d-103">Create reusable UI using the Razor class library project in ASP.NET Core</span></span>

<span data-ttu-id="0c46d-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0c46d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0c46d-105">Las vistas, páginas, controladores, modelos de páginas, [componentes de Razor](xref:blazor/class-libraries), componentes de [vista](xref:mvc/views/view-components)y modelos de datos de Razor se pueden integrar en una biblioteca de clases de Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="0c46d-105">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="0c46d-106">Las RCL se pueden empaquetar y reutilizar.</span><span class="sxs-lookup"><span data-stu-id="0c46d-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="0c46d-107">Las aplicaciones pueden incluir la RCL y reemplazar las vistas y páginas que contienen.</span><span class="sxs-lookup"><span data-stu-id="0c46d-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="0c46d-108">Cuando existe una vista, vista parcial o página de Razor tanto en la aplicación web como en la RCL, tendrá prioridad el marcado de Razor (archivo *.cshtml*) de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="0c46d-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="0c46d-109">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0c46d-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="0c46d-110">Crear una biblioteca de clases que contenga interfaz de usuario de Razor</span><span class="sxs-lookup"><span data-stu-id="0c46d-110">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0c46d-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0c46d-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0c46d-112">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="0c46d-112">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="0c46d-113">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="0c46d-113">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="0c46d-114">Asigne un nombre a la biblioteca (por ejemplo, "RazorClassLib") > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="0c46d-114">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="0c46d-115">Para evitar un conflicto de nombres de archivo con la biblioteca de vistas generada, asegúrese de que el nombre de la biblioteca no acaba en `.Views`.</span><span class="sxs-lookup"><span data-stu-id="0c46d-115">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="0c46d-116">Compruebe que se ha seleccionado **ASP.NET Core 3,0** o una versión posterior.</span><span class="sxs-lookup"><span data-stu-id="0c46d-116">Verify **ASP.NET Core 3.0** or later is selected.</span></span>
* <span data-ttu-id="0c46d-117">Seleccione **biblioteca de clases de Razor** > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="0c46d-117">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="0c46d-118">De forma predeterminada, la plantilla de la biblioteca de clases de Razor (RCL) utiliza el desarrollo de componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="0c46d-118">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="0c46d-119">Una opción de plantilla en Visual Studio proporciona compatibilidad con plantillas para páginas y vistas.</span><span class="sxs-lookup"><span data-stu-id="0c46d-119">A template option in Visual Studio provides template support for pages and views.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0c46d-120">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="0c46d-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="0c46d-121">Ejecute `dotnet new razorclasslib` desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="0c46d-121">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="0c46d-122">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0c46d-122">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="0c46d-123">De forma predeterminada, la plantilla de la biblioteca de clases de Razor (RCL) utiliza el desarrollo de componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="0c46d-123">The Razor class library (RCL) template defaults to Razor component development by default.</span></span> <span data-ttu-id="0c46d-124">Pase la opción `--support-pages-and-views` (`dotnet new razorclasslib --support-pages-and-views`) para proporcionar compatibilidad con páginas y vistas.</span><span class="sxs-lookup"><span data-stu-id="0c46d-124">Pass the `--support-pages-and-views` option (`dotnet new razorclasslib --support-pages-and-views`) to provide support for pages and views.</span></span>

<span data-ttu-id="0c46d-125">Para más información, vea [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="0c46d-125">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="0c46d-126">Para evitar un conflicto de nombres de archivo con la biblioteca de vistas generada, asegúrese de que el nombre de la biblioteca no acaba en `.Views`.</span><span class="sxs-lookup"><span data-stu-id="0c46d-126">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="0c46d-127">Agregue archivos de Razor a la RCL.</span><span class="sxs-lookup"><span data-stu-id="0c46d-127">Add Razor files to the RCL.</span></span>

<span data-ttu-id="0c46d-128">Las plantillas de ASP.NET Core se suponen que el contenido RCL está en el *áreas* carpeta.</span><span class="sxs-lookup"><span data-stu-id="0c46d-128">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="0c46d-129">Consulte [diseño de páginas de RCL](#rcl-pages-layout) para crear un RCL que exponga contenido en `~/Pages` en lugar de `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="0c46d-129">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="0c46d-130">Contenido de RCL de referencia</span><span class="sxs-lookup"><span data-stu-id="0c46d-130">Reference RCL content</span></span>

<span data-ttu-id="0c46d-131">Los siguientes elementos pueden hacer referencia a la RCL:</span><span class="sxs-lookup"><span data-stu-id="0c46d-131">The RCL can be referenced by:</span></span>

* <span data-ttu-id="0c46d-132">Paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="0c46d-132">NuGet package.</span></span> <span data-ttu-id="0c46d-133">Vea [Creación de paquetes NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) y [Creación y publicación de un paquete NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="0c46d-133">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="0c46d-134">*{NombreDeProyecto}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="0c46d-134">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="0c46d-135">Vea [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="0c46d-135">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="0c46d-136">Reemplazar vistas, vistas parciales y páginas</span><span class="sxs-lookup"><span data-stu-id="0c46d-136">Override views, partial views, and pages</span></span>

<span data-ttu-id="0c46d-137">Cuando existe una vista, vista parcial o página de Razor tanto en la aplicación web como en la RCL, tendrá prioridad el marcado de Razor (archivo *.cshtml*) de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="0c46d-137">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="0c46d-138">Por ejemplo, agregue *WebApp1/areas/mi Feature/pages/Page1. cshtml* a WebApp1 y Page1 en WebApp1 tendrá prioridad sobre PÁGINA1 en RCL.</span><span class="sxs-lookup"><span data-stu-id="0c46d-138">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="0c46d-139">En la descarga de ejemplo, cambie el nombre *WebApp1/Areas/MyFeature2* por *WebApp1/Areas/MyFeature* para comprobar la prioridad.</span><span class="sxs-lookup"><span data-stu-id="0c46d-139">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="0c46d-140">Copie la vista parcial *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* en *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0c46d-140">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="0c46d-141">Actualice el marcado para señalar la nueva ubicación.</span><span class="sxs-lookup"><span data-stu-id="0c46d-141">Update the markup to indicate the new location.</span></span> <span data-ttu-id="0c46d-142">Compile y ejecute la aplicación para comprobar si se está usando la versión de la vista parcial de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0c46d-142">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="0c46d-143">Diseño de páginas RCL</span><span class="sxs-lookup"><span data-stu-id="0c46d-143">RCL Pages layout</span></span>

<span data-ttu-id="0c46d-144">Para hacer referencia RCL contenido como si formara parte de la aplicación web a *páginas* carpeta, cree el proyecto RCL con la siguiente estructura de archivos:</span><span class="sxs-lookup"><span data-stu-id="0c46d-144">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="0c46d-145">*Páginas/RazorUIClassLib*</span><span class="sxs-lookup"><span data-stu-id="0c46d-145">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="0c46d-146">*RazorUIClassLib/páginas/Shared*</span><span class="sxs-lookup"><span data-stu-id="0c46d-146">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="0c46d-147">Supongamos que *RazorUIClassLib, compartidos o páginas* contiene dos archivos parciales: *_Header.cshtml* y *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0c46d-147">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="0c46d-148">El `<partial>` se puede agregar etiquetas a *_Layout.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="0c46d-148">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a><span data-ttu-id="0c46d-149">Creación de un RCL con recursos estáticos</span><span class="sxs-lookup"><span data-stu-id="0c46d-149">Create an RCL with static assets</span></span>

<span data-ttu-id="0c46d-150">Una RCL puede requerir recursos estáticos complementarios a los que puede hacer referencia la aplicación de consumo de RCL.</span><span class="sxs-lookup"><span data-stu-id="0c46d-150">An RCL may require companion static assets that can be referenced by the consuming app of the RCL.</span></span> <span data-ttu-id="0c46d-151">ASP.NET Core permite crear RCLs que incluyen recursos estáticos que están disponibles para una aplicación de consumo.</span><span class="sxs-lookup"><span data-stu-id="0c46d-151">ASP.NET Core allows creating RCLs that include static assets that are available to a consuming app.</span></span>

<span data-ttu-id="0c46d-152">Para incluir los recursos complementarios como parte de un RCL, cree una carpeta *wwwroot* en la biblioteca de clases e incluya los archivos necesarios en esa carpeta.</span><span class="sxs-lookup"><span data-stu-id="0c46d-152">To include companion assets as part of an RCL, create a *wwwroot* folder in the class library and include any required files in that folder.</span></span>

<span data-ttu-id="0c46d-153">Al empaquetar un RCL, todos los recursos complementarios de la carpeta *wwwroot* se incluyen automáticamente en el paquete.</span><span class="sxs-lookup"><span data-stu-id="0c46d-153">When packing an RCL, all companion assets in the *wwwroot* folder are automatically included in the package.</span></span>

### <a name="exclude-static-assets"></a><span data-ttu-id="0c46d-154">Excluir recursos estáticos</span><span class="sxs-lookup"><span data-stu-id="0c46d-154">Exclude static assets</span></span>

<span data-ttu-id="0c46d-155">Para excluir recursos estáticos, agregue la ruta de exclusión deseada al grupo de propiedades `$(DefaultItemExcludes)` en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="0c46d-155">To exclude static assets, add the desired exclusion path to the `$(DefaultItemExcludes)` property group in the project file.</span></span> <span data-ttu-id="0c46d-156">Separe las entradas con un punto y coma (`;`).</span><span class="sxs-lookup"><span data-stu-id="0c46d-156">Separate entries with a semicolon (`;`).</span></span>

<span data-ttu-id="0c46d-157">En el ejemplo siguiente, la hoja de estilos *lib. CSS* de la carpeta *wwwroot* no se considera un recurso estático y no se incluye en el RCL publicado:</span><span class="sxs-lookup"><span data-stu-id="0c46d-157">In the following example, the *lib.css* stylesheet in the *wwwroot* folder isn't considered a static asset and isn't included in the published RCL:</span></span>

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a><span data-ttu-id="0c46d-158">Integración de Typescript</span><span class="sxs-lookup"><span data-stu-id="0c46d-158">Typescript integration</span></span>

<span data-ttu-id="0c46d-159">Para incluir los archivos TypeScript en un RCL:</span><span class="sxs-lookup"><span data-stu-id="0c46d-159">To include TypeScript files in an RCL:</span></span>

1. <span data-ttu-id="0c46d-160">Coloque los archivos TypeScript ( *. ts*) fuera de la carpeta *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="0c46d-160">Place the TypeScript files (*.ts*) outside of the *wwwroot* folder.</span></span> <span data-ttu-id="0c46d-161">Por ejemplo, coloque los archivos en una carpeta de *cliente* .</span><span class="sxs-lookup"><span data-stu-id="0c46d-161">For example, place the files in a *Client* folder.</span></span>

1. <span data-ttu-id="0c46d-162">Configure el resultado de la compilación de TypeScript para la carpeta *wwwroot* .</span><span class="sxs-lookup"><span data-stu-id="0c46d-162">Configure the TypeScript build output for the *wwwroot* folder.</span></span> <span data-ttu-id="0c46d-163">Establezca la propiedad `TypescriptOutDir` dentro de un `PropertyGroup` en el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="0c46d-163">Set the `TypescriptOutDir` property inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. <span data-ttu-id="0c46d-164">Incluya el destino de TypeScript como una dependencia del destino `ResolveCurrentProjectStaticWebAssets` agregando el siguiente destino dentro de un `PropertyGroup` en el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="0c46d-164">Include the TypeScript target as a dependency of the `ResolveCurrentProjectStaticWebAssets` target by adding the following target inside of a `PropertyGroup` in the project file:</span></span>

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     TypeScriptCompile;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a><span data-ttu-id="0c46d-165">Consumo de contenido de una RCL a la que se hace referencia</span><span class="sxs-lookup"><span data-stu-id="0c46d-165">Consume content from a referenced RCL</span></span>

<span data-ttu-id="0c46d-166">Los archivos incluidos en la carpeta *wwwroot* del RCL se exponen a la aplicación que lo consume bajo el prefijo `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="0c46d-166">The files included in the *wwwroot* folder of the RCL are exposed to the consuming app under the prefix `_content/{LIBRARY NAME}/`.</span></span> <span data-ttu-id="0c46d-167">Por ejemplo, una biblioteca denominada *Razor. class. lib* da como resultado una ruta de acceso al contenido estático en `_content/Razor.Class.Lib/`.</span><span class="sxs-lookup"><span data-stu-id="0c46d-167">For example, a library named *Razor.Class.Lib* results in a path to static content at `_content/Razor.Class.Lib/`.</span></span>

<span data-ttu-id="0c46d-168">La aplicación de consumo hace referencia a los recursos estáticos proporcionados por la biblioteca con `<script>`, `<style>`, `<img>` y otras etiquetas HTML.</span><span class="sxs-lookup"><span data-stu-id="0c46d-168">The consuming app references static assets provided by the library with `<script>`, `<style>`, `<img>`, and other HTML tags.</span></span> <span data-ttu-id="0c46d-169">La aplicación de consumo debe tener habilitada la [compatibilidad con archivos estáticos](xref:fundamentals/static-files) en `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="0c46d-169">The consuming app must have [static file support](xref:fundamentals/static-files) enabled in `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="0c46d-170">Al ejecutar la aplicación de consumo desde la salida de la compilación (`dotnet run`), los activos web estáticos están habilitados de forma predeterminada en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="0c46d-170">When running the consuming app from build output (`dotnet run`), static web assets are enabled by default in the Development environment.</span></span> <span data-ttu-id="0c46d-171">Para admitir recursos en otros entornos cuando se ejecutan desde la salida de la compilación, llame a `UseStaticWebAssets` en el generador de host en *Program.CS*:</span><span class="sxs-lookup"><span data-stu-id="0c46d-171">To support assets in other environments when running from build output, call `UseStaticWebAssets` on the host builder in *Program.cs*:</span></span>

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

<span data-ttu-id="0c46d-172">No es necesario llamar a `UseStaticWebAssets` cuando se ejecuta una aplicación desde la salida publicada (`dotnet publish`).</span><span class="sxs-lookup"><span data-stu-id="0c46d-172">Calling `UseStaticWebAssets` isn't required when running an app from published output (`dotnet publish`).</span></span>

### <a name="multi-project-development-flow"></a><span data-ttu-id="0c46d-173">Flujo de desarrollo de varios proyectos</span><span class="sxs-lookup"><span data-stu-id="0c46d-173">Multi-project development flow</span></span>

<span data-ttu-id="0c46d-174">Cuando se ejecuta la aplicación de consumo:</span><span class="sxs-lookup"><span data-stu-id="0c46d-174">When the consuming app runs:</span></span>

* <span data-ttu-id="0c46d-175">Los recursos de RCL permanecen en sus carpetas originales.</span><span class="sxs-lookup"><span data-stu-id="0c46d-175">The assets in the RCL stay in their original folders.</span></span> <span data-ttu-id="0c46d-176">Los recursos no se mueven a la aplicación de consumo.</span><span class="sxs-lookup"><span data-stu-id="0c46d-176">The assets aren't moved to the consuming app.</span></span>
* <span data-ttu-id="0c46d-177">Cualquier cambio dentro de la carpeta *wwwroot* de RCL se refleja en la aplicación de consumo después de que se vuelva a generar RCL y sin volver a generar la aplicación de consumo.</span><span class="sxs-lookup"><span data-stu-id="0c46d-177">Any change within the RCL's *wwwroot* folder is reflected in the consuming app after the RCL is rebuilt and without rebuilding the consuming app.</span></span>

<span data-ttu-id="0c46d-178">Cuando se compila el RCL, se genera un manifiesto que describe las ubicaciones de los recursos web estáticos.</span><span class="sxs-lookup"><span data-stu-id="0c46d-178">When the RCL is built, a manifest is produced that describes the static web asset locations.</span></span> <span data-ttu-id="0c46d-179">La aplicación consumidora lee el manifiesto en tiempo de ejecución para consumir los recursos de los proyectos y paquetes a los que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="0c46d-179">The consuming app reads the manifest at runtime to consume the assets from referenced projects and packages.</span></span> <span data-ttu-id="0c46d-180">Cuando se agrega un nuevo recurso a un RCL, se debe volver a generar el RCL para actualizar el manifiesto antes de que una aplicación de consumo pueda acceder al nuevo recurso.</span><span class="sxs-lookup"><span data-stu-id="0c46d-180">When a new asset is added to an RCL, the RCL must be rebuilt to update its manifest before a consuming app can access the new asset.</span></span>

### <a name="publish"></a><span data-ttu-id="0c46d-181">Publicar</span><span class="sxs-lookup"><span data-stu-id="0c46d-181">Publish</span></span>

<span data-ttu-id="0c46d-182">Cuando se publica la aplicación, los recursos complementarios de todos los proyectos y paquetes a los que se hace referencia se copian en la carpeta *wwwroot* de la aplicación publicada en `_content/{LIBRARY NAME}/`.</span><span class="sxs-lookup"><span data-stu-id="0c46d-182">When the app is published, the companion assets from all referenced projects and packages are copied into the *wwwroot* folder of the published app under `_content/{LIBRARY NAME}/`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0c46d-183">Las vistas, páginas, controladores, modelos de páginas, [componentes de Razor](xref:blazor/class-libraries), componentes de [vista](xref:mvc/views/view-components)y modelos de datos de Razor se pueden integrar en una biblioteca de clases de Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="0c46d-183">Razor views, pages, controllers, page models, [Razor components](xref:blazor/class-libraries), [View components](xref:mvc/views/view-components), and data models can be built into a Razor class library (RCL).</span></span> <span data-ttu-id="0c46d-184">Las RCL se pueden empaquetar y reutilizar.</span><span class="sxs-lookup"><span data-stu-id="0c46d-184">The RCL can be packaged and reused.</span></span> <span data-ttu-id="0c46d-185">Las aplicaciones pueden incluir la RCL y reemplazar las vistas y páginas que contienen.</span><span class="sxs-lookup"><span data-stu-id="0c46d-185">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="0c46d-186">Cuando existe una vista, vista parcial o página de Razor tanto en la aplicación web como en la RCL, tendrá prioridad el marcado de Razor (archivo *.cshtml*) de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="0c46d-186">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="0c46d-187">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0c46d-187">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="0c46d-188">Crear una biblioteca de clases que contenga interfaz de usuario de Razor</span><span class="sxs-lookup"><span data-stu-id="0c46d-188">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0c46d-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0c46d-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0c46d-190">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="0c46d-190">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="0c46d-191">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="0c46d-191">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="0c46d-192">Asigne un nombre a la biblioteca (por ejemplo, "RazorClassLib") > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="0c46d-192">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="0c46d-193">Para evitar un conflicto de nombres de archivo con la biblioteca de vistas generada, asegúrese de que el nombre de la biblioteca no acaba en `.Views`.</span><span class="sxs-lookup"><span data-stu-id="0c46d-193">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="0c46d-194">Confirme que **ASP.NET Core 2.1** o una versión posterior está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="0c46d-194">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="0c46d-195">Seleccione **biblioteca de clases de Razor** > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="0c46d-195">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="0c46d-196">Un RCL tiene el siguiente archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="0c46d-196">An RCL has the following project file:</span></span>

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0c46d-197">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="0c46d-197">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="0c46d-198">Ejecute `dotnet new razorclasslib` desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="0c46d-198">From the command line, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="0c46d-199">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0c46d-199">For example:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="0c46d-200">Para más información, vea [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="0c46d-200">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="0c46d-201">Para evitar un conflicto de nombres de archivo con la biblioteca de vistas generada, asegúrese de que el nombre de la biblioteca no acaba en `.Views`.</span><span class="sxs-lookup"><span data-stu-id="0c46d-201">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

---

<span data-ttu-id="0c46d-202">Agregue archivos de Razor a la RCL.</span><span class="sxs-lookup"><span data-stu-id="0c46d-202">Add Razor files to the RCL.</span></span>

<span data-ttu-id="0c46d-203">Las plantillas de ASP.NET Core se suponen que el contenido RCL está en el *áreas* carpeta.</span><span class="sxs-lookup"><span data-stu-id="0c46d-203">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="0c46d-204">Consulte [diseño de páginas de RCL](#rcl-pages-layout) para crear un RCL que exponga contenido en `~/Pages` en lugar de `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="0c46d-204">See [RCL Pages layout](#rcl-pages-layout) to create an RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="reference-rcl-content"></a><span data-ttu-id="0c46d-205">Contenido de RCL de referencia</span><span class="sxs-lookup"><span data-stu-id="0c46d-205">Reference RCL content</span></span>

<span data-ttu-id="0c46d-206">Los siguientes elementos pueden hacer referencia a la RCL:</span><span class="sxs-lookup"><span data-stu-id="0c46d-206">The RCL can be referenced by:</span></span>

* <span data-ttu-id="0c46d-207">Paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="0c46d-207">NuGet package.</span></span> <span data-ttu-id="0c46d-208">Vea [Creación de paquetes NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) y [Creación y publicación de un paquete NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="0c46d-208">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="0c46d-209">*{NombreDeProyecto}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="0c46d-209">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="0c46d-210">Vea [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="0c46d-210">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="0c46d-211">Tutorial: Creación de un proyecto de RCL y uso de un proyecto de Razor Pages</span><span class="sxs-lookup"><span data-stu-id="0c46d-211">Walkthrough: Create an RCL project and use from a Razor Pages project</span></span>

<span data-ttu-id="0c46d-212">En lugar de crearlo, puede descargar el [proyecto completo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) y comprobarlo.</span><span class="sxs-lookup"><span data-stu-id="0c46d-212">You can download the [complete project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="0c46d-213">La descarga de ejemplo contiene más código y vínculos que hacen que el proyecto sea fácil de comprobar.</span><span class="sxs-lookup"><span data-stu-id="0c46d-213">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="0c46d-214">Puede dejar sus comentarios en [este problema de GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6098) sobre las descargas de ejemplo en comparación con las instrucciones paso a paso.</span><span class="sxs-lookup"><span data-stu-id="0c46d-214">You can leave feedback in [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="0c46d-215">Comprobar la aplicación de descarga</span><span class="sxs-lookup"><span data-stu-id="0c46d-215">Test the download app</span></span>

<span data-ttu-id="0c46d-216">Si no ha descargado la aplicación final y prefiere crear el proyecto de tutorial, omita este paso y vaya a la [siguiente sección](#create-an-rcl).</span><span class="sxs-lookup"><span data-stu-id="0c46d-216">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-an-rcl).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0c46d-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0c46d-217">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0c46d-218">Abra el archivo *.sln* en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0c46d-218">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="0c46d-219">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0c46d-219">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0c46d-220">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="0c46d-220">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="0c46d-221">Desde un símbolo del sistema en el directorio *cli*, cree la RCL y la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="0c46d-221">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="0c46d-222">Vaya al directorio *WebApp1* y ejecute la aplicación:</span><span class="sxs-lookup"><span data-stu-id="0c46d-222">Move to the *WebApp1* directory and run the app:</span></span>

```dotnetcli
dotnet run
```

---

<span data-ttu-id="0c46d-223">Siga las instrucciones de [Probar WebApp1](#test-webapp1).</span><span class="sxs-lookup"><span data-stu-id="0c46d-223">Follow the instructions in [Test WebApp1](#test-webapp1)</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="0c46d-224">Creación de un RCL</span><span class="sxs-lookup"><span data-stu-id="0c46d-224">Create an RCL</span></span>

<span data-ttu-id="0c46d-225">En esta sección, se crea un RCL.</span><span class="sxs-lookup"><span data-stu-id="0c46d-225">In this section, an RCL is created.</span></span> <span data-ttu-id="0c46d-226">y se agregarán a ella archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="0c46d-226">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0c46d-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0c46d-227">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0c46d-228">Cree el proyecto de RCL:</span><span class="sxs-lookup"><span data-stu-id="0c46d-228">Create the RCL project:</span></span>

* <span data-ttu-id="0c46d-229">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="0c46d-229">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="0c46d-230">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="0c46d-230">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="0c46d-231">Asigne a la aplicación el nombre **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="0c46d-231">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="0c46d-232">Confirme que **ASP.NET Core 2.1** o una versión posterior está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="0c46d-232">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="0c46d-233">Seleccione **biblioteca de clases de Razor** > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="0c46d-233">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="0c46d-234">Agregue un archivo de vista parcial de Razor denominado *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0c46d-234">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0c46d-235">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="0c46d-235">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="0c46d-236">Ejecute lo siguiente desde la línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="0c46d-236">From the command line, run the following:</span></span>

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="0c46d-237">Los comandos anteriores:</span><span class="sxs-lookup"><span data-stu-id="0c46d-237">The preceding commands:</span></span>

* <span data-ttu-id="0c46d-238">Crea el `RazorUIClassLib` RCL.</span><span class="sxs-lookup"><span data-stu-id="0c46d-238">Creates the `RazorUIClassLib` RCL.</span></span>
* <span data-ttu-id="0c46d-239">Crean una página _Message de Razor y la agrega a la RCL.</span><span class="sxs-lookup"><span data-stu-id="0c46d-239">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="0c46d-240">El parámetro `-np` crea la página sin un `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="0c46d-240">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="0c46d-241">Crea un [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) de archivo y lo agrega a la RCL.</span><span class="sxs-lookup"><span data-stu-id="0c46d-241">Creates a [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="0c46d-242">El *_ViewStart.cshtml* archivo es necesario para usar el diseño del proyecto de páginas de Razor (que se agrega en la sección siguiente).</span><span class="sxs-lookup"><span data-stu-id="0c46d-242">The *_ViewStart.cshtml* file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

---

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="0c46d-243">Agregar archivos de Razor y carpetas al proyecto</span><span class="sxs-lookup"><span data-stu-id="0c46d-243">Add Razor files and folders to the project</span></span>

* <span data-ttu-id="0c46d-244">Reemplace el marcado de *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="0c46d-244">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="0c46d-245">Reemplace el marcado de *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="0c46d-245">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  <span data-ttu-id="0c46d-246">Se necesita `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` para usar la vista parcial (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="0c46d-246">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="0c46d-247">En lugar de incluir la directiva `@addTagHelper`, puede agregar un archivo *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0c46d-247">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="0c46d-248">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0c46d-248">For example:</span></span>

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  <span data-ttu-id="0c46d-249">Para obtener más información sobre *_ViewImports.cshtml*, consulte [importar directivas compartidas](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="0c46d-249">For more information on *_ViewImports.cshtml*, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="0c46d-250">Compile la biblioteca de clases para confirmar que no hay ningún error de compilador:</span><span class="sxs-lookup"><span data-stu-id="0c46d-250">Build the class library to verify there are no compiler errors:</span></span>

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

<span data-ttu-id="0c46d-251">El resultado de la compilación contiene *RazorUIClassLib.dll* y *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="0c46d-251">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="0c46d-252">*RazorUIClassLib.Views.dll* incluye el contenido de Razor compilado.</span><span class="sxs-lookup"><span data-stu-id="0c46d-252">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="0c46d-253">Usar la biblioteca de interfaz de usuario de Razor desde un proyecto de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="0c46d-253">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0c46d-254">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0c46d-254">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0c46d-255">Cree la aplicación web de páginas de Razor:</span><span class="sxs-lookup"><span data-stu-id="0c46d-255">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="0c46d-256">En **Explorador de soluciones**, haga clic con el botón derecho en la solución > **Agregar** **proyecto nuevo**de @no__t 2.</span><span class="sxs-lookup"><span data-stu-id="0c46d-256">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="0c46d-257">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="0c46d-257">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="0c46d-258">Denomine la aplicación **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="0c46d-258">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="0c46d-259">Confirme que **ASP.NET Core 2.1** o una versión posterior está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="0c46d-259">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="0c46d-260">Seleccione **aplicación Web** > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="0c46d-260">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="0c46d-261">En el **Explorador de soluciones**, haga clic con el botón derecho en **WebApp1** y seleccione **Establecer como proyecto de inicio**.</span><span class="sxs-lookup"><span data-stu-id="0c46d-261">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="0c46d-262">En **Explorador de soluciones**, haga clic con el botón derecho en **WebApp1** y seleccione las **dependencias de compilación** > **dependencias del proyecto**.</span><span class="sxs-lookup"><span data-stu-id="0c46d-262">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="0c46d-263">Marque **RazorUIClassLib** como dependencia de **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="0c46d-263">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="0c46d-264">En **Explorador de soluciones**, haga clic con el botón derecho en **WebApp1** y seleccione **Agregar** **referencia**de >.</span><span class="sxs-lookup"><span data-stu-id="0c46d-264">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="0c46d-265">En el cuadro de diálogo **Administrador de referencias** , active **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="0c46d-265">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="0c46d-266">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0c46d-266">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0c46d-267">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="0c46d-267">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="0c46d-268">Cree una aplicación Web de Razor Pages y un archivo de solución que contenga la aplicación Razor Pages y RCL:</span><span class="sxs-lookup"><span data-stu-id="0c46d-268">Create a Razor Pages web app and a solution file containing the Razor Pages app and the RCL:</span></span>

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="0c46d-269">Compile y ejecute la aplicación web:</span><span class="sxs-lookup"><span data-stu-id="0c46d-269">Build and run the web app:</span></span>

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a><span data-ttu-id="0c46d-270">Probar WebApp1</span><span class="sxs-lookup"><span data-stu-id="0c46d-270">Test WebApp1</span></span>

<span data-ttu-id="0c46d-271">Vaya a `/MyFeature/Page1` para comprobar que la biblioteca de clases de la interfaz de usuario de Razor está en uso.</span><span class="sxs-lookup"><span data-stu-id="0c46d-271">Browse to `/MyFeature/Page1` to verify that the Razor UI class library is in use.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="0c46d-272">Reemplazar vistas, vistas parciales y páginas</span><span class="sxs-lookup"><span data-stu-id="0c46d-272">Override views, partial views, and pages</span></span>

<span data-ttu-id="0c46d-273">Cuando existe una vista, vista parcial o página de Razor tanto en la aplicación web como en la RCL, tendrá prioridad el marcado de Razor (archivo *.cshtml*) de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="0c46d-273">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="0c46d-274">Por ejemplo, agregue *WebApp1/areas/mi Feature/pages/Page1. cshtml* a WebApp1 y Page1 en WebApp1 tendrá prioridad sobre PÁGINA1 en RCL.</span><span class="sxs-lookup"><span data-stu-id="0c46d-274">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1 in the RCL.</span></span>

<span data-ttu-id="0c46d-275">En la descarga de ejemplo, cambie el nombre *WebApp1/Areas/MyFeature2* por *WebApp1/Areas/MyFeature* para comprobar la prioridad.</span><span class="sxs-lookup"><span data-stu-id="0c46d-275">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="0c46d-276">Copie la vista parcial *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* en *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0c46d-276">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="0c46d-277">Actualice el marcado para señalar la nueva ubicación.</span><span class="sxs-lookup"><span data-stu-id="0c46d-277">Update the markup to indicate the new location.</span></span> <span data-ttu-id="0c46d-278">Compile y ejecute la aplicación para comprobar si se está usando la versión de la vista parcial de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0c46d-278">Build and run the app to verify the app's version of the partial is being used.</span></span>

### <a name="rcl-pages-layout"></a><span data-ttu-id="0c46d-279">Diseño de páginas RCL</span><span class="sxs-lookup"><span data-stu-id="0c46d-279">RCL Pages layout</span></span>

<span data-ttu-id="0c46d-280">Para hacer referencia RCL contenido como si formara parte de la aplicación web a *páginas* carpeta, cree el proyecto RCL con la siguiente estructura de archivos:</span><span class="sxs-lookup"><span data-stu-id="0c46d-280">To reference RCL content as though it is part of the web app's *Pages* folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="0c46d-281">*Páginas/RazorUIClassLib*</span><span class="sxs-lookup"><span data-stu-id="0c46d-281">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="0c46d-282">*RazorUIClassLib/páginas/Shared*</span><span class="sxs-lookup"><span data-stu-id="0c46d-282">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="0c46d-283">Supongamos que *RazorUIClassLib, compartidos o páginas* contiene dos archivos parciales: *_Header.cshtml* y *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0c46d-283">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files: *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="0c46d-284">El `<partial>` se puede agregar etiquetas a *_Layout.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="0c46d-284">The `<partial>` tags could be added to *_Layout.cshtml* file:</span></span>

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end
