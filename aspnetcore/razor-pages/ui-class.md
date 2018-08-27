---
title: Interfaz de usuario de Razor reutilizable en bibliotecas de clases con ASP.NET Core
author: Rick-Anderson
description: Se explica cómo crear una interfaz de usuario de Razor reutilizable en una biblioteca de clases.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/21/2018
uid: razor-pages/ui-class
ms.openlocfilehash: 1f0ef59ce3f3294d6a3bde015ca34800770b1be4
ms.sourcegitcommit: e955a722c05ce2e5e21b4219f7d94fb878e255a6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2018
ms.locfileid: "42909535"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="8af6f-103">Cree una interfaz de usuario reutilizable con el proyecto de biblioteca de clases de Razor en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8af6f-103">Create reusable UI using the Razor Class Library project in ASP.NET Core.</span></span>

<span data-ttu-id="8af6f-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8af6f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8af6f-105">Las vistas, las páginas, los controladores, los modelos de página, los modelos de datos y los [componentes de vista](xref:mvc/views/view-components) de Razor se pueden integrar en una biblioteca de clases de Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="8af6f-105">Razor views, pages, controllers, page models, [View components](xref:mvc/views/view-components), and data models can be built into a Razor Class Library (RCL).</span></span> <span data-ttu-id="8af6f-106">Las RCL se pueden empaquetar y reutilizar.</span><span class="sxs-lookup"><span data-stu-id="8af6f-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="8af6f-107">Las aplicaciones pueden incluir la RCL y reemplazar las vistas y páginas que contienen.</span><span class="sxs-lookup"><span data-stu-id="8af6f-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="8af6f-108">Cuando existe una vista, vista parcial o página de Razor tanto en la aplicación web como en la RCL, tendrá prioridad el marcado de Razor (archivo *.cshtml*) de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="8af6f-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="8af6f-109">Esta característica requiere [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="8af6f-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="8af6f-110">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8af6f-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="8af6f-111">Crear una biblioteca de clases que contenga interfaz de usuario de Razor</span><span class="sxs-lookup"><span data-stu-id="8af6f-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8af6f-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8af6f-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8af6f-113">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="8af6f-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="8af6f-114">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8af6f-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="8af6f-115">Asigne un nombre a la biblioteca (por ejemplo, "RazorClassLib") > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8af6f-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="8af6f-116">Para evitar un conflicto de nombres de archivo con la biblioteca de vistas generada, asegúrese de que el nombre de la biblioteca no acaba en `.Views`.</span><span class="sxs-lookup"><span data-stu-id="8af6f-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="8af6f-117">Confirme que **ASP.NET Core 2.1** o una versión posterior está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="8af6f-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="8af6f-118">Seleccione **Razor Class Library** (Biblioteca de clases de Razor) > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8af6f-118">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="8af6f-119">Una biblioteca de clases de Razor tiene el siguiente archivo del proyecto:</span><span class="sxs-lookup"><span data-stu-id="8af6f-119">A Razor Class Library has the following project file:</span></span>

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8af6f-120">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="8af6f-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="8af6f-121">Ejecute `dotnet new razorclasslib` desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="8af6f-121">From the commandline, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="8af6f-122">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8af6f-122">For example:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="8af6f-123">Para más información, vea [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="8af6f-123">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="8af6f-124">Para evitar un conflicto de nombres de archivo con la biblioteca de vistas generada, asegúrese de que el nombre de la biblioteca no acaba en `.Views`.</span><span class="sxs-lookup"><span data-stu-id="8af6f-124">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

------
<span data-ttu-id="8af6f-125">Agregue archivos de Razor a la RCL.</span><span class="sxs-lookup"><span data-stu-id="8af6f-125">Add Razor files to the RCL.</span></span>

<span data-ttu-id="8af6f-126">Las plantillas de ASP.NET Core se suponen que el contenido RCL está en el *áreas* carpeta.</span><span class="sxs-lookup"><span data-stu-id="8af6f-126">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="8af6f-127">Consulte [diseño de páginas RCL](#afs) para crear contenido en un RCL que expone `~/Pages` lugar `~/Areas/Pages`.</span><span class="sxs-lookup"><span data-stu-id="8af6f-127">See [RCL Pages layout](#afs) to create a RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="referencing-razor-class-library-content"></a><span data-ttu-id="8af6f-128">Hacer referencia a contenido de la biblioteca de clases de Razor</span><span class="sxs-lookup"><span data-stu-id="8af6f-128">Referencing Razor Class Library content</span></span>

<span data-ttu-id="8af6f-129">Los siguientes elementos pueden hacer referencia a la RCL:</span><span class="sxs-lookup"><span data-stu-id="8af6f-129">The RCL can be referenced by:</span></span>

* <span data-ttu-id="8af6f-130">Paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="8af6f-130">NuGet package.</span></span> <span data-ttu-id="8af6f-131">Vea [Creación de paquetes NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) y [Creación y publicación de un paquete NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="8af6f-131">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="8af6f-132">*{NombreDeProyecto}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="8af6f-132">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="8af6f-133">Vea [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="8af6f-133">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="8af6f-134">Tutorial: Crear un proyecto de biblioteca de clases de Razor y usar desde un proyecto de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="8af6f-134">Walkthrough: Create a Razor Class Library project and use from a Razor Pages project</span></span>

<span data-ttu-id="8af6f-135">En lugar de crearlo, puede descargar el [proyecto completo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) y comprobarlo.</span><span class="sxs-lookup"><span data-stu-id="8af6f-135">You can download the [complete project](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="8af6f-136">La descarga de ejemplo contiene más código y vínculos que hacen que el proyecto sea fácil de comprobar.</span><span class="sxs-lookup"><span data-stu-id="8af6f-136">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="8af6f-137">Puede dejar sus comentarios en [este problema de GitHub](https://github.com/aspnet/Docs/issues/6098) sobre las descargas de ejemplo en comparación con las instrucciones paso a paso.</span><span class="sxs-lookup"><span data-stu-id="8af6f-137">You can leave feedback in [this GitHub issue](https://github.com/aspnet/Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="8af6f-138">Comprobar la aplicación de descarga</span><span class="sxs-lookup"><span data-stu-id="8af6f-138">Test the download app</span></span>

<span data-ttu-id="8af6f-139">Si no ha descargado la aplicación final y prefiere crear el proyecto de tutorial, omita este paso y vaya a la [siguiente sección](#create-a-razor-class-library).</span><span class="sxs-lookup"><span data-stu-id="8af6f-139">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-a-razor-class-library).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8af6f-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8af6f-140">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8af6f-141">Abra el archivo *.sln* en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8af6f-141">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="8af6f-142">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8af6f-142">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8af6f-143">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="8af6f-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="8af6f-144">Desde un símbolo del sistema en el directorio *cli*, cree la RCL y la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="8af6f-144">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

``` CLI
dotnet build
```

<span data-ttu-id="8af6f-145">Vaya al directorio *WebApp1* y ejecute la aplicación:</span><span class="sxs-lookup"><span data-stu-id="8af6f-145">Move to the *WebApp1* directory and run the app:</span></span>

``` CLI
dotnet run
```
------

<span data-ttu-id="8af6f-146">Siga las instrucciones de [Probar WebApp1](#test).</span><span class="sxs-lookup"><span data-stu-id="8af6f-146">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="8af6f-147">Crear una biblioteca de clases de Razor</span><span class="sxs-lookup"><span data-stu-id="8af6f-147">Create a Razor Class Library</span></span>

<span data-ttu-id="8af6f-148">En esta sección, crearemos una biblioteca de clases de Razor (RCL)</span><span class="sxs-lookup"><span data-stu-id="8af6f-148">In this section, a Razor Class Library (RCL) is created.</span></span> <span data-ttu-id="8af6f-149">y se agregarán a ella archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="8af6f-149">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8af6f-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8af6f-150">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8af6f-151">Cree el proyecto de RCL:</span><span class="sxs-lookup"><span data-stu-id="8af6f-151">Create the RCL project:</span></span>

* <span data-ttu-id="8af6f-152">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="8af6f-152">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="8af6f-153">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8af6f-153">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="8af6f-154">Nombre de la aplicación **RazorUIClassLib** > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8af6f-154">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="8af6f-155">Confirme que **ASP.NET Core 2.1** o una versión posterior está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="8af6f-155">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="8af6f-156">Seleccione **Razor Class Library** (Biblioteca de clases de Razor) > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8af6f-156">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="8af6f-157">Agregue un archivo de vista parcial de Razor denominado *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8af6f-157">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8af6f-158">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="8af6f-158">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="8af6f-159">Ejecute lo siguiente desde la línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="8af6f-159">From the command line, run the following:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="8af6f-160">Los comandos anteriores:</span><span class="sxs-lookup"><span data-stu-id="8af6f-160">The preceding commands:</span></span>

* <span data-ttu-id="8af6f-161">Crean la biblioteca de clases de Razor (RCL) `RazorUIClassLib`.</span><span class="sxs-lookup"><span data-stu-id="8af6f-161">Creates the `RazorUIClassLib` Razor Class Library (RCL).</span></span>
* <span data-ttu-id="8af6f-162">Crean una página _Message de Razor y la agrega a la RCL.</span><span class="sxs-lookup"><span data-stu-id="8af6f-162">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="8af6f-163">El parámetro `-np` crea la página sin un `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="8af6f-163">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="8af6f-164">Crean un archivo [viewstart](xref:mvc/views/layout#running-code-before-each-view) y lo agregan a la RCL.</span><span class="sxs-lookup"><span data-stu-id="8af6f-164">Creates a [viewstart](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="8af6f-165">El archivo viewstart es necesario para poder usar el diseño del proyecto de páginas de Razor (que agregaremos en la siguiente sección).</span><span class="sxs-lookup"><span data-stu-id="8af6f-165">The viewstart file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

------

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="8af6f-166">Agregue archivos y carpetas de Razor al proyecto.</span><span class="sxs-lookup"><span data-stu-id="8af6f-166">Add Razor files and folders to the project.</span></span>

* <span data-ttu-id="8af6f-167">Reemplace el marcado de *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="8af6f-167">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="8af6f-168">Reemplace el marcado de *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="8af6f-168">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="8af6f-169">Se necesita `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` para usar la vista parcial (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="8af6f-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="8af6f-170">En lugar de incluir la directiva `@addTagHelper`, puede agregar un archivo *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8af6f-170">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="8af6f-171">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8af6f-171">For example:</span></span>

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="8af6f-172">Para más información sobre viewimports, vea [Importar directivas compartidas](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="8af6f-172">For more information on viewimports, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="8af6f-173">Compile la biblioteca de clases para confirmar que no hay ningún error de compilador:</span><span class="sxs-lookup"><span data-stu-id="8af6f-173">Build the class library to verify there are no compiler errors:</span></span>

``` CLI
dotnet build RazorUIClassLib
```

<span data-ttu-id="8af6f-174">El resultado de la compilación contiene *RazorUIClassLib.dll* y *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="8af6f-174">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="8af6f-175">*RazorUIClassLib.Views.dll* incluye el contenido de Razor compilado.</span><span class="sxs-lookup"><span data-stu-id="8af6f-175">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="8af6f-176">Usar la biblioteca de interfaz de usuario de Razor desde un proyecto de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="8af6f-176">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8af6f-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8af6f-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8af6f-178">Cree la aplicación web de páginas de Razor:</span><span class="sxs-lookup"><span data-stu-id="8af6f-178">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="8af6f-179">En el **Explorador de soluciones**, haga clic con el botón derecho en la solución > **Agregar** >  **Nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="8af6f-179">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="8af6f-180">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8af6f-180">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="8af6f-181">Denomine la aplicación **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="8af6f-181">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="8af6f-182">Confirme que **ASP.NET Core 2.1** o una versión posterior está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="8af6f-182">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="8af6f-183">Seleccione **Aplicación web** > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8af6f-183">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="8af6f-184">En el **Explorador de soluciones**, haga clic con el botón derecho en **WebApp1** y seleccione **Establecer como proyecto de inicio**.</span><span class="sxs-lookup"><span data-stu-id="8af6f-184">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="8af6f-185">En el **Explorador de soluciones**, haga clic con el botón derecho en **WebApp1** y seleccione **Dependencias de compilación** > **Dependencias del proyecto**.</span><span class="sxs-lookup"><span data-stu-id="8af6f-185">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="8af6f-186">Marque **RazorUIClassLib** como dependencia de **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="8af6f-186">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="8af6f-187">En el **Explorador de soluciones**, haga clic con el botón derecho en **WebApp1** y seleccione **Agregar** > **Referencia**.</span><span class="sxs-lookup"><span data-stu-id="8af6f-187">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="8af6f-188">En el cuadro de diálogo **Administrador de referencias**, marque **RazorUIClassLib** > **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8af6f-188">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="8af6f-189">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8af6f-189">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8af6f-190">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="8af6f-190">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="8af6f-191">Cree una aplicación web de páginas de Razor y un archivo de solución que contenga dicha aplicación y la biblioteca de clases de Razor:</span><span class="sxs-lookup"><span data-stu-id="8af6f-191">Create a Razor Pages web app and a solution file containing the Razor Pages app and the Razor Class Library:</span></span>

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

<span data-ttu-id="8af6f-192">Compile y ejecute la aplicación web:</span><span class="sxs-lookup"><span data-stu-id="8af6f-192">Build and run the web app:</span></span>

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="8af6f-193">Probar WebApp1</span><span class="sxs-lookup"><span data-stu-id="8af6f-193">Test WebApp1</span></span>

<span data-ttu-id="8af6f-194">Compruebe si la biblioteca de clases de interfaz de usuario de Razor se está usando.</span><span class="sxs-lookup"><span data-stu-id="8af6f-194">Verify the Razor UI class library is being used.</span></span>

* <span data-ttu-id="8af6f-195">Vaya a `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="8af6f-195">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="8af6f-196">Reemplazar vistas, vistas parciales y páginas</span><span class="sxs-lookup"><span data-stu-id="8af6f-196">Override views, partial views, and pages</span></span>

<span data-ttu-id="8af6f-197">Cuando existe una vista, vista parcial o página de Razor tanto en la aplicación web como en la biblioteca de clases de Razor, tendrá prioridad el marcado de Razor (archivo *.cshtml*) de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="8af6f-197">When a view, partial view, or Razor Page is found in both the web app and the Razor Class Library, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="8af6f-198">Por ejemplo, si agrega *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* a WebApp1, Page1 en WebApp1 prevalecerá sobre Page1 en la biblioteca de clases de Razor.</span><span class="sxs-lookup"><span data-stu-id="8af6f-198">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1in the Razor Class Library.</span></span>

<span data-ttu-id="8af6f-199">En la descarga de ejemplo, cambie el nombre *WebApp1/Areas/MyFeature2* por *WebApp1/Areas/MyFeature* para comprobar la prioridad.</span><span class="sxs-lookup"><span data-stu-id="8af6f-199">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="8af6f-200">Copie la vista parcial *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* en *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8af6f-200">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="8af6f-201">Actualice el marcado para señalar la nueva ubicación.</span><span class="sxs-lookup"><span data-stu-id="8af6f-201">Update the markup to indicate the new location.</span></span> <span data-ttu-id="8af6f-202">Compile y ejecute la aplicación para comprobar si se está usando la versión de la vista parcial de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8af6f-202">Build and run the app to verify the app's version of the partial is being used.</span></span>

<a name="afs"></a>

### <a name="rcl-pages-layout"></a><span data-ttu-id="8af6f-203">Diseño de páginas RCL</span><span class="sxs-lookup"><span data-stu-id="8af6f-203">RCL Pages layout</span></span>

<span data-ttu-id="8af6f-204">Para hacer referencia a contenido RCL como si formara parte de la carpeta de la aplicación web, cree el proyecto RCL con la siguiente estructura de archivos:</span><span class="sxs-lookup"><span data-stu-id="8af6f-204">To reference RCL content as though it is part of the web app's Pages folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="8af6f-205">*Páginas/RazorUIClassLib*</span><span class="sxs-lookup"><span data-stu-id="8af6f-205">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="8af6f-206">*RazorUIClassLib/páginas/Shared*</span><span class="sxs-lookup"><span data-stu-id="8af6f-206">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="8af6f-207">Supongamos que *RazorUIClassLib, compartidos o páginas* contiene dos archivos parciales, *_Header.cshtml* y *_Footer.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8af6f-207">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files, *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="8af6f-208">El <partial> se puede agregar etiquetas a *_Layout.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="8af6f-208">The <partial> tags could be added to *_Layout.cshtml* file:</span></span> 
  
```
  <body>
    <partial name="_Header">
    @RenderBody()
    <partial name="_Footer">
  </body>
```
