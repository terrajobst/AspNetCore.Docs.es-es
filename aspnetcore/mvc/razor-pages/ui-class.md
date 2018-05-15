---
title: Interfaz de usuario de Razor reutilizable en bibliotecas de clases con ASP.NET Core
author: Rick-Anderson
description: Se explica cómo crear una interfaz de usuario de Razor reutilizable en una biblioteca de clases.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: advanced
uid: mvc/razor-pages/ui-class
ms.openlocfilehash: 731d37a8f4983b18ded114f05470f8a408deb7cd
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="84939-103">Cree una interfaz de usuario reutilizable con el proyecto de biblioteca de clases de Razor en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="84939-103">Create reusable UI using the Razor Class Library project in ASP.NET Core.</span></span>

<span data-ttu-id="84939-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="84939-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="84939-105">Las vistas, páginas, controladores, modelos de página y modelos de datos de Razor se pueden integrar en una biblioteca de clases de Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="84939-105">Razor views, pages, controllers, page models, and data models can be built into a Razor Class Library(RCL).</span></span> <span data-ttu-id="84939-106">Las RCL se pueden empaquetar y reutilizar.</span><span class="sxs-lookup"><span data-stu-id="84939-106">The RCL can be and packaged and reused.</span></span> <span data-ttu-id="84939-107">Las aplicaciones pueden incluir la RCL y reemplazar las vistas y páginas que contienen.</span><span class="sxs-lookup"><span data-stu-id="84939-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="84939-108">Cuando existe una vista, vista parcial o página de Razor tanto en la aplicación web como en la RCL, tendrá prioridad el marcado de Razor (archivo *.cshtml*) de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="84939-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="84939-109">Esta característica requiere [!INCLUDE[](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="84939-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

[!INCLUDE[](~/includes/2.1.md)]

<span data-ttu-id="84939-110">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="84939-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="84939-111">Crear una biblioteca de clases que contenga interfaz de usuario de Razor</span><span class="sxs-lookup"><span data-stu-id="84939-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="84939-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="84939-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="84939-113">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="84939-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="84939-114">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="84939-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="84939-115">Confirme que **ASP.NET Core 2.1** o una versión posterior está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="84939-115">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="84939-116">Seleccione **Razor Class Library** (Biblioteca de clases de Razor) > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="84939-116">Select **Razor Class Library** > **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="84939-117">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="84939-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="84939-118">Ejecute `dotnet new razorclasslib` desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="84939-118">From the commandline, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="84939-119">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="84939-119">For example:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="84939-120">Para más información, vea [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="84939-120">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span>

------
<span data-ttu-id="84939-121">Agregue archivos de Razor a la RCL.</span><span class="sxs-lookup"><span data-stu-id="84939-121">Add Razor files to the RCL.</span></span>

<span data-ttu-id="84939-122">Es aconsejable que el contenido de RCL esté en la carpeta *Areas*.</span><span class="sxs-lookup"><span data-stu-id="84939-122">We recommend RCL content go in the *Areas* folder.</span></span> 


## <a name="referencing-razor-class-library-content"></a><span data-ttu-id="84939-123">Hacer referencia a contenido de la biblioteca de clases de Razor</span><span class="sxs-lookup"><span data-stu-id="84939-123">Referencing Razor Class Library content</span></span>

<span data-ttu-id="84939-124">Los siguientes elementos pueden hacer referencia a la RCL:</span><span class="sxs-lookup"><span data-stu-id="84939-124">The RCL can be referenced by:</span></span>

* <span data-ttu-id="84939-125">Paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="84939-125">NuGet package.</span></span> <span data-ttu-id="84939-126">Vea [Creación de paquetes NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) y [Creación y publicación de un paquete NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="84939-126">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="84939-127">*{NombreDeProyecto}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="84939-127">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="84939-128">Vea [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="84939-128">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

### <a name="partial-files-access-in-the-rcl"></a><span data-ttu-id="84939-129">Acceso a archivos parciales en la RCL</span><span class="sxs-lookup"><span data-stu-id="84939-129">Partial files access in the RCL</span></span>

<span data-ttu-id="84939-130">Para el contenido que hay fuera de la RCL, el tiempo de ejecución de ASP.NET Core no busca archivos parciales en la RCL.</span><span class="sxs-lookup"><span data-stu-id="84939-130">For content outside the RCL, the ASP.NET Core runtime does not search for partial files in the RCL.</span></span>

<span data-ttu-id="84939-131">Por ejemplo, en la descarga de ejemplo **no** se puede hacer referencia a la vista parcial *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* en *WebApp1\Pages\About.cshtml* .</span><span class="sxs-lookup"><span data-stu-id="84939-131">For example, in the sample download, the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view can **not** be referenced in *WebApp1\Pages\About.cshtml*.</span></span> <span data-ttu-id="84939-132">Pero las páginas en la RCL (*RazorUIClassLib/* **sí pueden** tener acceso a *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="84939-132">However, pages in the RCL ( *RazorUIClassLib/* **can** access *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="84939-133">Tutorial: Crear un proyecto de biblioteca de clases de Razor y usar desde un proyecto de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="84939-133">Walkthrough: Create a Razor Class Library project and use from a Razor Pages project</span></span>

<span data-ttu-id="84939-134">En lugar de crearlo, puede descargar el [proyecto completo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) y comprobarlo.</span><span class="sxs-lookup"><span data-stu-id="84939-134">You can download the [complete project](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="84939-135">La descarga de ejemplo contiene más código y vínculos que hacen que el proyecto sea fácil de comprobar.</span><span class="sxs-lookup"><span data-stu-id="84939-135">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="84939-136">Puede dejar sus comentarios en [este problema de GitHub](https://github.com/aspnet/Docs/issues/6098) sobre las descargas de ejemplo en comparación con las instrucciones paso a paso.</span><span class="sxs-lookup"><span data-stu-id="84939-136">You can leave feedback in [this GitHub issue](https://github.com/aspnet/Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="84939-137">Comprobar la aplicación de descarga</span><span class="sxs-lookup"><span data-stu-id="84939-137">Test the download app</span></span>

<span data-ttu-id="84939-138">Si no ha descargado la aplicación final y prefiere crear el proyecto de tutorial, omita este paso y vaya a la [siguiente sección](#create-a-razor-class-library).</span><span class="sxs-lookup"><span data-stu-id="84939-138">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-a-razor-class-library).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="84939-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="84939-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="84939-140">Abra el archivo *.sln* en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="84939-140">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="84939-141">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="84939-141">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="84939-142">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="84939-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="84939-143">Desde un símbolo del sistema en el directorio *cli*, cree la RCL y la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="84939-143">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

``` CLI
dotnet build
```

<span data-ttu-id="84939-144">Vaya al directorio *WebApp1* y ejecute la aplicación:</span><span class="sxs-lookup"><span data-stu-id="84939-144">Move to the *WebApp1* directory and run the app:</span></span>

``` CLI
dotnet run
```
------

<span data-ttu-id="84939-145">Siga las instrucciones de [Probar WebApp1](#test).</span><span class="sxs-lookup"><span data-stu-id="84939-145">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="84939-146">Crear una biblioteca de clases de Razor</span><span class="sxs-lookup"><span data-stu-id="84939-146">Create a Razor Class Library</span></span>

<span data-ttu-id="84939-147">En esta sección, crearemos una biblioteca de clases de Razor (RCL)</span><span class="sxs-lookup"><span data-stu-id="84939-147">In this section, a Razor Class Library (RCL) is created.</span></span> <span data-ttu-id="84939-148">y se agregarán a ella archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="84939-148">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="84939-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="84939-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="84939-150">Cree el proyecto de RCL:</span><span class="sxs-lookup"><span data-stu-id="84939-150">Create the RCL project:</span></span>

* <span data-ttu-id="84939-151">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="84939-151">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="84939-152">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="84939-152">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="84939-153">Denomine la aplicación **RazorUIClassLib**.</span><span class="sxs-lookup"><span data-stu-id="84939-153">Name the app **RazorUIClassLib**.</span></span>
* <span data-ttu-id="84939-154">Confirme que **ASP.NET Core 2.1** o una versión posterior está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="84939-154">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="84939-155">Seleccione **Razor Class Library** (Biblioteca de clases de Razor) > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="84939-155">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="84939-156">Cree la aplicación web de páginas de Razor:</span><span class="sxs-lookup"><span data-stu-id="84939-156">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="84939-157">En el **Explorador de soluciones**, haga clic con el botón derecho en la solución > **Agregar** >  **Nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="84939-157">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="84939-158">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="84939-158">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="84939-159">Denomine la aplicación **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="84939-159">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="84939-160">Confirme que **ASP.NET Core 2.1** o una versión posterior está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="84939-160">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="84939-161">Seleccione **Aplicación web** > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="84939-161">Select **Web Application** > **OK**.</span></span>

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="84939-162">Agregue archivos y carpetas de Razor al proyecto.</span><span class="sxs-lookup"><span data-stu-id="84939-162">Add Razor files and folders to the project.</span></span>

* <span data-ttu-id="84939-163">Agregue un archivo de vista parcial de Razor denominado *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="84939-163">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>
* <span data-ttu-id="84939-164">Reemplace el marcado de *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="84939-164">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="84939-165">Copie el archivo *_ViewStart.cshtml* del proyecto WebApp1 en *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="84939-165">Copy the *_ViewStart.cshtml* file from the WebApp1 project to  *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.</span></span>

  <span data-ttu-id="84939-166">El archivo [viewstart](xref:mvc/views/layout#running-code-before-each-view) es necesario para poder usar el diseño del proyecto de páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="84939-166">The [viewstart](xref:mvc/views/layout#running-code-before-each-view) file is required to use the layout of the Razor Pages project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="84939-167">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="84939-167">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="84939-168">Ejecute lo siguiente desde la línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="84939-168">From the command line, run the following:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="84939-169">Los comandos anteriores:</span><span class="sxs-lookup"><span data-stu-id="84939-169">The preceding commands:</span></span>

* <span data-ttu-id="84939-170">Crean la biblioteca de clases de Razor (RCL) `RazorUIClassLib`.</span><span class="sxs-lookup"><span data-stu-id="84939-170">Creates the `RazorUIClassLib` Razor Class Library (RCL).</span></span>
* <span data-ttu-id="84939-171">Crean una página _Message de Razor y la agrega a la RCL.</span><span class="sxs-lookup"><span data-stu-id="84939-171">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="84939-172">El parámetro `-np` crea la página sin un `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="84939-172">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="84939-173">Crean un archivo [viewstart](xref:mvc/views/layout#running-code-before-each-view) y lo agregan a la RCL.</span><span class="sxs-lookup"><span data-stu-id="84939-173">Creates a [viewstart](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="84939-174">El archivo viewstart es necesario para poder usar el diseño del proyecto de páginas de Razor (que agregaremos en la siguiente sección).</span><span class="sxs-lookup"><span data-stu-id="84939-174">The viewstart file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

<span data-ttu-id="84939-175">Actualice las páginas de Razor:</span><span class="sxs-lookup"><span data-stu-id="84939-175">Update the Razor Pages:</span></span>

* <span data-ttu-id="84939-176">Reemplace el marcado de *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="84939-176">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="84939-177">Reemplace el marcado de *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="84939-177">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="84939-178">Se necesita `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` para usar la vista parcial (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="84939-178">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="84939-179">En lugar de incluir la directiva `@addTagHelper`, puede agregar un archivo *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="84939-179">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="84939-180">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="84939-180">For example:</span></span>

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="84939-181">Para más información sobre viewimports, vea [Importar directivas compartidas](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="84939-181">For more information on viewimports, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="84939-182">Compile la biblioteca de clases para confirmar que no hay ningún error de compilador:</span><span class="sxs-lookup"><span data-stu-id="84939-182">Build the class library to verify there are no compiler errors:</span></span>

``` CLI
dotnet build RazorUIClassLib
```

<span data-ttu-id="84939-183">El resultado de la compilación contiene *RazorUIClassLib.dll* y *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="84939-183">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="84939-184">*RazorUIClassLib.Views.dll* incluye el contenido de Razor compilado.</span><span class="sxs-lookup"><span data-stu-id="84939-184">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

------

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="84939-185">Usar la biblioteca de interfaz de usuario de Razor desde un proyecto de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="84939-185">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="84939-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="84939-186">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="84939-187">En el **Explorador de soluciones**, haga clic con el botón derecho en **WebApp1** y seleccione **Establecer como proyecto de inicio**.</span><span class="sxs-lookup"><span data-stu-id="84939-187">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="84939-188">En el **Explorador de soluciones**, haga clic con el botón derecho en **WebApp1** y seleccione **Dependencias de compilación** > **Dependencias del proyecto**.</span><span class="sxs-lookup"><span data-stu-id="84939-188">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="84939-189">Marque **RazorUIClassLib** como dependencia de **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="84939-189">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="84939-190">En el **Explorador de soluciones**, haga clic con el botón derecho en **WebApp1** y seleccione **Agregar** > **Referencia**.</span><span class="sxs-lookup"><span data-stu-id="84939-190">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="84939-191">En el cuadro de diálogo **Administrador de referencias**, marque **RazorUIClassLib** > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="84939-191">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="84939-192">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="84939-192">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="84939-193">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="84939-193">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="84939-194">Cree una aplicación web de páginas de Razor y un archivo de solución que contenga dicha aplicación y la biblioteca de clases de Razor:</span><span class="sxs-lookup"><span data-stu-id="84939-194">Create a Razor Pages web app and a solution file containing the Razor Pages app and the Razor Class Library:</span></span>

``` CLI
dotnet new razor -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="84939-195">Compile y ejecute la aplicación web:</span><span class="sxs-lookup"><span data-stu-id="84939-195">Build and run the web app:</span></span>

``` CLI
cd WebApp1
dotnet run
```

------

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="84939-196">Probar WebApp1</span><span class="sxs-lookup"><span data-stu-id="84939-196">Test WebApp1</span></span>

<span data-ttu-id="84939-197">Compruebe si la biblioteca de clases de interfaz de usuario de Razor se está usando.</span><span class="sxs-lookup"><span data-stu-id="84939-197">Verify the Razor UI class library is being used.</span></span>

* <span data-ttu-id="84939-198">Vaya a `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="84939-198">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="84939-199">Reemplazar vistas, vistas parciales y páginas</span><span class="sxs-lookup"><span data-stu-id="84939-199">Override views, partial views, and pages</span></span>

<span data-ttu-id="84939-200">Cuando existe una vista, vista parcial o página de Razor tanto en la aplicación web como en la biblioteca de clases de Razor, tendrá prioridad el marcado de Razor (archivo *.cshtml*) de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="84939-200">When a view, partial view, or Razor Page is found in both the web app and the Razor Class Library, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="84939-201">Por ejemplo, si agrega *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* a WebApp1, Page1 en WebApp1 prevalecerá sobre Page1 en la biblioteca de clases de Razor.</span><span class="sxs-lookup"><span data-stu-id="84939-201">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1in the Razor Class Library.</span></span>

<span data-ttu-id="84939-202">En la descarga de ejemplo, cambie el nombre *WebApp1/Areas/MyFeature2* por *WebApp1/Areas/MyFeature* para comprobar la prioridad.</span><span class="sxs-lookup"><span data-stu-id="84939-202">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="84939-203">Copie la vista parcial *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* en *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="84939-203">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="84939-204">Actualice el marcado para señalar la nueva ubicación.</span><span class="sxs-lookup"><span data-stu-id="84939-204">Update the markup to indicate the new location.</span></span> <span data-ttu-id="84939-205">Compile y ejecute la aplicación para comprobar si se está usando la versión de la vista parcial de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="84939-205">Build and run the app to verify the app's version of the partial is being used.</span></span>
