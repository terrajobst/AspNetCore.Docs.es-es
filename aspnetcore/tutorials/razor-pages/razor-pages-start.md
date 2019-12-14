---
title: 'Tutorial: Introducción a Razor Pages en ASP.NET Core'
author: rick-anderson
description: Esta serie de tutoriales muestra cómo usar Razor Pages en ASP.NET Core. Obtenga información sobre cómo crear un modelo, generar código para Razor Pages, usar Entity Framework Core y SQL Server para el acceso a datos, agregar la funcionalidad de búsqueda, agregar validación de entrada y usar migraciones para actualizar el modelo.
ms.author: riande
ms.date: 11/12/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: b651437b698d01310f90c5f14832616c1896e6c0
ms.sourcegitcommit: 4e3edff24ba6e43a103fee1b126c9826241bb37b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2019
ms.locfileid: "74959104"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="96ac3-104">Tutorial: Introducción a Razor Pages en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="96ac3-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="96ac3-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="96ac3-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="96ac3-106">Este es el primer tutorial de una serie de varios que enseña los aspectos básicos de la compilación de una aplicación web de Razor Pages de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96ac3-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="96ac3-107">Al final de la serie, tendrá una aplicación que puede administrar una base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="96ac3-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="96ac3-108">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="96ac3-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="96ac3-109">Crear una aplicación web de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="96ac3-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="96ac3-110">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96ac3-110">Run the app.</span></span>
> * <span data-ttu-id="96ac3-111">Examinar los archivos de proyecto.</span><span class="sxs-lookup"><span data-stu-id="96ac3-111">Examine the project files.</span></span>

<span data-ttu-id="96ac3-112">Al final de este tutorial, tendrá una aplicación web de Razor Pages que compilará en los tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="96ac3-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="96ac3-114">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="96ac3-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="96ac3-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96ac3-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="96ac3-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="96ac3-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="96ac3-117">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="96ac3-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="96ac3-118">Creación de una aplicación web de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="96ac3-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="96ac3-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96ac3-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="96ac3-120">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="96ac3-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="96ac3-121">Cree una nueva aplicación web de ASP.NET Core y seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="96ac3-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="96ac3-122">![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="96ac3-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="96ac3-123">Asigne al proyecto el nombre **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="96ac3-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="96ac3-124">Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="96ac3-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="96ac3-125">![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="96ac3-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="96ac3-126">Seleccione **ASP.NET Core 3.1** en la lista desplegable, después **Aplicación web** y, por último, **Crear**.</span><span class="sxs-lookup"><span data-stu-id="96ac3-126">Select **ASP.NET Core 3.1** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="96ac3-128">Se crea el proyecto de inicio siguiente:</span><span class="sxs-lookup"><span data-stu-id="96ac3-128">The following starter project is created:</span></span>

  ![Explorador de soluciones](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="96ac3-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="96ac3-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="96ac3-131">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="96ac3-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="96ac3-132">Cambie al directorio (`cd`) que contiene el proyecto.</span><span class="sxs-lookup"><span data-stu-id="96ac3-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="96ac3-133">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="96ac3-133">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="96ac3-134">El comando `dotnet new` crea un proyecto de Razor Pages en la carpeta *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="96ac3-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="96ac3-135">El comando `code` abre la carpeta *RazorPagesMovie* en la instancia actual de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="96ac3-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="96ac3-136">Cuando el icono de llama de OmniSharp de la barra de estado se ponga verde, aparecerá un cuadro de diálogo que pregunta **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="96ac3-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="96ac3-137">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="96ac3-137">Select **Yes**.</span></span>

  <span data-ttu-id="96ac3-138">Un directorio *.vscode*, que contiene archivos *launch.json* y *tasks.json*, se agrega al directorio raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="96ac3-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="96ac3-139">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="96ac3-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="96ac3-140">Seleccione **Archivo** > **Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="96ac3-140">Select **File** > **New Solution**.</span></span>

![macOS: Nueva solución](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="96ac3-142">Seleccione **.NET Core** > **Aplicación** > **Aplicación web** > **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="96ac3-142">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![Cuadro de diálogo de nuevo proyecto de macOS](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="96ac3-144">En el cuadro de diálogo **Configure your new ASP.NET Core Web API** (Configurar la nueva API web de ASP.NET Core), establezca la **plataforma de destino** en **.NET Core 3.1**.</span><span class="sxs-lookup"><span data-stu-id="96ac3-144">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** to **.NET Core 3.1**.</span></span>

  ![Selección de .NET Core 3.0 de macOS](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="96ac3-146">Asigne el nombre **RazorPagesMovie** al proyecto y, después, seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="96ac3-146">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)


## <a name="open-the-project"></a><span data-ttu-id="96ac3-148">Abrir el proyecto</span><span class="sxs-lookup"><span data-stu-id="96ac3-148">Open the project</span></span>

<span data-ttu-id="96ac3-149">En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="96ac3-149">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="96ac3-150">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="96ac3-150">Run the app</span></span>

  [!INCLUDE[](~/includes/run-the-app.md)]

## <a name="examine-the-project-files"></a><span data-ttu-id="96ac3-151">Examen de los archivo del proyecto</span><span class="sxs-lookup"><span data-stu-id="96ac3-151">Examine the project files</span></span>

<span data-ttu-id="96ac3-152">He aquí un resumen de las principales carpetas y archivos del proyecto con los que va a trabajar en los próximos tutoriales.</span><span class="sxs-lookup"><span data-stu-id="96ac3-152">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="96ac3-153">Carpeta Pages</span><span class="sxs-lookup"><span data-stu-id="96ac3-153">Pages folder</span></span>

<span data-ttu-id="96ac3-154">Contiene Razor Pages y los archivos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="96ac3-154">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="96ac3-155">Cada página de Razor se compone de un par de archivos:</span><span class="sxs-lookup"><span data-stu-id="96ac3-155">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="96ac3-156">Archivo *.cshtml* que contiene el marcado HTML con código C# que usa la sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="96ac3-156">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="96ac3-157">Archivo *. cshtml.cs* que contiene C# código que controla los eventos de página.</span><span class="sxs-lookup"><span data-stu-id="96ac3-157">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="96ac3-158">Los archivos auxiliares tienen nombres que comienzan con un carácter de subrayado.</span><span class="sxs-lookup"><span data-stu-id="96ac3-158">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="96ac3-159">Por ejemplo, el archivo *_Layout.cshtml* configura los elementos de la interfaz de usuario comunes a todas las páginas.</span><span class="sxs-lookup"><span data-stu-id="96ac3-159">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="96ac3-160">Este archivo configura el menú de navegación de la parte superior de la página y el aviso de copyright de la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="96ac3-160">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="96ac3-161">Para más información, consulte <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="96ac3-161">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="96ac3-162">Carpeta wwwroot</span><span class="sxs-lookup"><span data-stu-id="96ac3-162">wwwroot folder</span></span>

<span data-ttu-id="96ac3-163">Contiene los archivos estáticos, como los archivos HTML, los archivos de JavaScript y los archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="96ac3-163">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="96ac3-164">Para más información, consulte <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="96ac3-164">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="96ac3-165">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="96ac3-165">appSettings.json</span></span>

<span data-ttu-id="96ac3-166">Contiene los datos de configuración, como las cadenas de conexión.</span><span class="sxs-lookup"><span data-stu-id="96ac3-166">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="96ac3-167">Para más información, consulte <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="96ac3-167">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="96ac3-168">Program.cs</span><span class="sxs-lookup"><span data-stu-id="96ac3-168">Program.cs</span></span>

<span data-ttu-id="96ac3-169">Contiene el punto de entrada del programa.</span><span class="sxs-lookup"><span data-stu-id="96ac3-169">Contains the entry point for the program.</span></span> <span data-ttu-id="96ac3-170">Para más información, consulte <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="96ac3-170">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="96ac3-171">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="96ac3-171">Startup.cs</span></span>

<span data-ttu-id="96ac3-172">Contiene código que configura el comportamiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96ac3-172">Contains code that configures app behavior.</span></span> <span data-ttu-id="96ac3-173">Para más información, consulte <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="96ac3-173">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="96ac3-174">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="96ac3-174">Next steps</span></span>

<span data-ttu-id="96ac3-175">Pase al siguiente tutorial de la serie:</span><span class="sxs-lookup"><span data-stu-id="96ac3-175">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="96ac3-176">Agregar un modelo</span><span class="sxs-lookup"><span data-stu-id="96ac3-176">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="96ac3-177">Este es el primer tutorial de una serie.</span><span class="sxs-lookup"><span data-stu-id="96ac3-177">This is the first tutorial of a series.</span></span> <span data-ttu-id="96ac3-178">En la [serie](xref:tutorials/razor-pages/index) se enseñan los conceptos básicos de la compilación de una aplicación web de Razor Pages en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96ac3-178">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="96ac3-179">Al final de la serie, tendrá una aplicación que puede administrar una base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="96ac3-179">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="96ac3-180">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="96ac3-180">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="96ac3-181">Crear una aplicación web de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="96ac3-181">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="96ac3-182">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96ac3-182">Run the app.</span></span>
> * <span data-ttu-id="96ac3-183">Examinar los archivos de proyecto.</span><span class="sxs-lookup"><span data-stu-id="96ac3-183">Examine the project files.</span></span>

<span data-ttu-id="96ac3-184">Al final de este tutorial, tendrá una aplicación web de Razor Pages que compilará en los tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="96ac3-184">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="96ac3-186">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="96ac3-186">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="96ac3-187">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96ac3-187">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="96ac3-188">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="96ac3-188">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="96ac3-189">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="96ac3-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="96ac3-190">Creación de una aplicación web de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="96ac3-190">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="96ac3-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96ac3-191">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="96ac3-192">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="96ac3-192">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="96ac3-193">Cree una nueva aplicación web de ASP.NET Core y seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="96ac3-193">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="96ac3-195">Asigne al proyecto el nombre **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="96ac3-195">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="96ac3-196">Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="96ac3-196">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="96ac3-198">Seleccione **ASP.NET Core 2.2** en la lista desplegable, después **Aplicación web** y, por último, **Crear**.</span><span class="sxs-lookup"><span data-stu-id="96ac3-198">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="96ac3-200">Se crea el proyecto de inicio siguiente:</span><span class="sxs-lookup"><span data-stu-id="96ac3-200">The following starter project is created:</span></span>

  ![Explorador de soluciones](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="96ac3-202">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="96ac3-202">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="96ac3-203">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="96ac3-203">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="96ac3-204">Cambie al directorio (`cd`) que contiene el proyecto.</span><span class="sxs-lookup"><span data-stu-id="96ac3-204">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="96ac3-205">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="96ac3-205">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="96ac3-206">El comando `dotnet new` crea un proyecto de Razor Pages en la carpeta *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="96ac3-206">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="96ac3-207">El comando `code` abre la carpeta *RazorPagesMovie* en la instancia actual de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="96ac3-207">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="96ac3-208">Cuando el icono de llama de OmniSharp de la barra de estado se ponga verde, aparecerá un cuadro de diálogo que pregunta **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="96ac3-208">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="96ac3-209">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="96ac3-209">Select **Yes**.</span></span>

  <span data-ttu-id="96ac3-210">Un directorio *.vscode*, que contiene archivos *launch.json* y *tasks.json*, se agrega al directorio raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="96ac3-210">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="96ac3-211">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="96ac3-211">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="96ac3-212">Desde un terminal, ejecute este comando:</span><span class="sxs-lookup"><span data-stu-id="96ac3-212">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```dotnetcli
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="96ac3-213">Los comandos anteriores utilizan la [CLI de .NET Core](/dotnet/core/tools/dotnet) para crear un proyecto de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="96ac3-213">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="96ac3-214">Abrir el proyecto</span><span class="sxs-lookup"><span data-stu-id="96ac3-214">Open the project</span></span>

<span data-ttu-id="96ac3-215">En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="96ac3-215">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="96ac3-216">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="96ac3-216">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="96ac3-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96ac3-217">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="96ac3-218">Presione Ctrl+F5 para ejecutarla sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="96ac3-218">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="96ac3-219">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96ac3-219">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="96ac3-220">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="96ac3-220">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="96ac3-221">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="96ac3-221">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="96ac3-222">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="96ac3-222">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="96ac3-223">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="96ac3-223">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="96ac3-224">En la página principal de la aplicación, seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="96ac3-224">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="96ac3-225">Esta aplicación no realiza un seguimiento de la información personal, pero la plantilla del proyecto incluye la función de consentimiento en caso de que sea necesaria para cumplir con el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr) de la Unión Europea.</span><span class="sxs-lookup"><span data-stu-id="96ac3-225">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="96ac3-227">En la siguiente imagen se muestra la aplicación tras haber dado su consentimiento al seguimiento:</span><span class="sxs-lookup"><span data-stu-id="96ac3-227">The following image shows the app after you give consent to tracking:</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="96ac3-229">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="96ac3-229">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="96ac3-230">Presione **Ctrl+F5** para ejecutar sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="96ac3-230">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="96ac3-231">Visual Studio Code inicia [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplaza hasta `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="96ac3-231">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="96ac3-232">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="96ac3-232">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="96ac3-233">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="96ac3-233">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="96ac3-234">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="96ac3-234">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="96ac3-235">En la página principal de la aplicación, seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="96ac3-235">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="96ac3-236">Esta aplicación no realiza un seguimiento de la información personal, pero la plantilla del proyecto incluye la función de consentimiento en caso de que sea necesaria para cumplir con el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr) de la Unión Europea.</span><span class="sxs-lookup"><span data-stu-id="96ac3-236">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="96ac3-238">En la siguiente imagen se muestra la aplicación tras haber dado su consentimiento al seguimiento:</span><span class="sxs-lookup"><span data-stu-id="96ac3-238">The following image shows the app after you give consent to tracking:</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="96ac3-240">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="96ac3-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="96ac3-241">Presione **Cmd-Opt-F5** para realizar la ejecución sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="96ac3-241">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="96ac3-242">Visual Studio iniciará [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplazará a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="96ac3-242">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="96ac3-243">En la página principal de la aplicación, seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="96ac3-243">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="96ac3-244">Esta aplicación no realiza un seguimiento de la información personal, pero la plantilla del proyecto incluye la función de consentimiento en caso de que sea necesaria para cumplir con el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr) de la Unión Europea.</span><span class="sxs-lookup"><span data-stu-id="96ac3-244">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="96ac3-246">En la siguiente imagen se muestra la aplicación tras haber dado su consentimiento al seguimiento:</span><span class="sxs-lookup"><span data-stu-id="96ac3-246">The following image shows the app after you give consent to tracking:</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="96ac3-248">Examen de los archivo del proyecto</span><span class="sxs-lookup"><span data-stu-id="96ac3-248">Examine the project files</span></span>

<span data-ttu-id="96ac3-249">He aquí un resumen de las principales carpetas y archivos del proyecto con los que va a trabajar en los próximos tutoriales.</span><span class="sxs-lookup"><span data-stu-id="96ac3-249">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="96ac3-250">Carpeta Pages</span><span class="sxs-lookup"><span data-stu-id="96ac3-250">Pages folder</span></span>

<span data-ttu-id="96ac3-251">Contiene Razor Pages y los archivos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="96ac3-251">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="96ac3-252">Cada página de Razor se compone de un par de archivos:</span><span class="sxs-lookup"><span data-stu-id="96ac3-252">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="96ac3-253">Archivo *.cshtml* que contiene el marcado HTML con código C# que usa la sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="96ac3-253">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="96ac3-254">Archivo *. cshtml.cs* que contiene C# código que controla los eventos de página.</span><span class="sxs-lookup"><span data-stu-id="96ac3-254">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="96ac3-255">Los archivos auxiliares tienen nombres que comienzan con un carácter de subrayado.</span><span class="sxs-lookup"><span data-stu-id="96ac3-255">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="96ac3-256">Por ejemplo, el archivo *_Layout.cshtml* configura los elementos de la interfaz de usuario comunes a todas las páginas.</span><span class="sxs-lookup"><span data-stu-id="96ac3-256">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="96ac3-257">Este archivo configura el menú de navegación de la parte superior de la página y el aviso de copyright de la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="96ac3-257">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="96ac3-258">Para más información, consulte <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="96ac3-258">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="96ac3-259">Carpeta wwwroot</span><span class="sxs-lookup"><span data-stu-id="96ac3-259">wwwroot folder</span></span>

<span data-ttu-id="96ac3-260">Contiene los archivos estáticos, como los archivos HTML, los archivos de JavaScript y los archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="96ac3-260">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="96ac3-261">Para más información, consulte <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="96ac3-261">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="96ac3-262">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="96ac3-262">appSettings.json</span></span>

<span data-ttu-id="96ac3-263">Contiene los datos de configuración, como las cadenas de conexión.</span><span class="sxs-lookup"><span data-stu-id="96ac3-263">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="96ac3-264">Para más información, consulte <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="96ac3-264">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="96ac3-265">Program.cs</span><span class="sxs-lookup"><span data-stu-id="96ac3-265">Program.cs</span></span>

<span data-ttu-id="96ac3-266">Contiene el punto de entrada del programa.</span><span class="sxs-lookup"><span data-stu-id="96ac3-266">Contains the entry point for the program.</span></span> <span data-ttu-id="96ac3-267">Para más información, consulte <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="96ac3-267">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="96ac3-268">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="96ac3-268">Startup.cs</span></span>

<span data-ttu-id="96ac3-269">Contiene código que configura el comportamiento de la aplicación, como, por ejemplo, si se requiere consentimiento para las cookies.</span><span class="sxs-lookup"><span data-stu-id="96ac3-269">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="96ac3-270">Para más información, consulte <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="96ac3-270">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="96ac3-271">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="96ac3-271">Additional resources</span></span>

* [<span data-ttu-id="96ac3-272">Versión en YouTube de este tutorial</span><span class="sxs-lookup"><span data-stu-id="96ac3-272">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="96ac3-273">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="96ac3-273">Next steps</span></span>

<span data-ttu-id="96ac3-274">Pase al siguiente tutorial de la serie:</span><span class="sxs-lookup"><span data-stu-id="96ac3-274">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="96ac3-275">Agregar un modelo</span><span class="sxs-lookup"><span data-stu-id="96ac3-275">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
