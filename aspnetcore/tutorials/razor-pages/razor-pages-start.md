---
title: 'Tutorial: Introducción a Razor Pages en ASP.NET Core'
author: rick-anderson
description: Esta serie de tutoriales muestra cómo usar Razor Pages en ASP.NET Core. Obtenga información sobre cómo crear un modelo, generar código para Razor Pages, usar Entity Framework Core y SQL Server para el acceso a datos, agregar la funcionalidad de búsqueda, agregar validación de entrada y usar migraciones para actualizar el modelo.
ms.author: riande
ms.date: 07/25/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1605197188d97f27a884739a72400da2d5818b1a
ms.sourcegitcommit: 849af69ee3c94cdb9fd8fa1f1bb8f5a5dda7b9eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/22/2019
ms.locfileid: "68372006"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="241fd-104">Tutorial: Introducción a Razor Pages en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="241fd-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="241fd-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="241fd-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="241fd-106">Este es el primer tutorial de una serie de varios que enseña los aspectos básicos de la compilación de una aplicación web de Razor Pages de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="241fd-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="241fd-107">Al final de la serie, tendrá una aplicación que puede administrar una base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="241fd-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="241fd-108">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="241fd-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="241fd-109">Crear una aplicación web de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="241fd-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="241fd-110">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="241fd-110">Run the app.</span></span>
> * <span data-ttu-id="241fd-111">Examinar los archivos de proyecto.</span><span class="sxs-lookup"><span data-stu-id="241fd-111">Examine the project files.</span></span>

<span data-ttu-id="241fd-112">Al final de este tutorial, tendrá una aplicación web de Razor Pages que compilará en los tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="241fd-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="241fd-114">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="241fd-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="241fd-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="241fd-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="241fd-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="241fd-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="241fd-117">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="241fd-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="241fd-118">Creación de una aplicación web de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="241fd-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="241fd-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="241fd-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="241fd-120">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="241fd-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="241fd-121">Cree una nueva aplicación web de ASP.NET Core y seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="241fd-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="241fd-122">![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="241fd-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="241fd-123">Asigne al proyecto el nombre **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="241fd-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="241fd-124">Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="241fd-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="241fd-125">![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="241fd-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="241fd-126">Seleccione **ASP.NET Core 3.0** en la lista desplegable, después **Aplicación web** y, por último, **Crear**.</span><span class="sxs-lookup"><span data-stu-id="241fd-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="241fd-128">Se crea el proyecto de inicio siguiente:</span><span class="sxs-lookup"><span data-stu-id="241fd-128">The following starter project is created:</span></span>

  ![Explorador de soluciones](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="241fd-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="241fd-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="241fd-131">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="241fd-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="241fd-132">Cambie al directorio (`cd`) que contiene el proyecto.</span><span class="sxs-lookup"><span data-stu-id="241fd-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="241fd-133">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="241fd-133">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="241fd-134">El comando `dotnet new` crea un proyecto de Razor Pages en la carpeta *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="241fd-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="241fd-135">El comando `code` abre la carpeta *RazorPagesMovie* en la instancia actual de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="241fd-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="241fd-136">Cuando el icono de llama de OmniSharp de la barra de estado se ponga verde, aparecerá un cuadro de diálogo que pregunta **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="241fd-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="241fd-137">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="241fd-137">Select **Yes**.</span></span>

  <span data-ttu-id="241fd-138">Un directorio *.vscode*, que contiene archivos *launch.json* y *tasks.json*, se agrega al directorio raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="241fd-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="241fd-139">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="241fd-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="241fd-140">Desde un terminal, ejecute este comando:</span><span class="sxs-lookup"><span data-stu-id="241fd-140">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="241fd-141">Los comandos anteriores utilizan la [CLI de .NET Core](/dotnet/core/tools/dotnet) para crear un proyecto de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="241fd-141">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="241fd-142">Abrir el proyecto</span><span class="sxs-lookup"><span data-stu-id="241fd-142">Open the project</span></span>

<span data-ttu-id="241fd-143">En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="241fd-143">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="241fd-144">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="241fd-144">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="241fd-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="241fd-145">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="241fd-146">Presione Ctrl+F5 para ejecutarla sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="241fd-146">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="241fd-147">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="241fd-147">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="241fd-148">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="241fd-148">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="241fd-149">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="241fd-149">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="241fd-150">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="241fd-150">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="241fd-151">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="241fd-151">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="241fd-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="241fd-152">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="241fd-153">Presione **Ctrl+F5** para ejecutar sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="241fd-153">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="241fd-154">Visual Studio Code inicia [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplaza hasta `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="241fd-154">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="241fd-155">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="241fd-155">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="241fd-156">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="241fd-156">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="241fd-157">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="241fd-157">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="241fd-158">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="241fd-158">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="241fd-159">Presione **Cmd-Opt-F5** para realizar la ejecución sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="241fd-159">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="241fd-160">Visual Studio iniciará [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplazará a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="241fd-160">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="241fd-161">Examen de los archivo del proyecto</span><span class="sxs-lookup"><span data-stu-id="241fd-161">Examine the project files</span></span>

<span data-ttu-id="241fd-162">He aquí un resumen de las principales carpetas y archivos del proyecto con los que va a trabajar en los próximos tutoriales.</span><span class="sxs-lookup"><span data-stu-id="241fd-162">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="241fd-163">Carpeta Pages</span><span class="sxs-lookup"><span data-stu-id="241fd-163">Pages folder</span></span>

<span data-ttu-id="241fd-164">Contiene Razor Pages y los archivos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="241fd-164">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="241fd-165">Cada página de Razor se compone de un par de archivos:</span><span class="sxs-lookup"><span data-stu-id="241fd-165">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="241fd-166">Archivo *.cshtml* que contiene el marcado HTML con código C# que usa la sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="241fd-166">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="241fd-167">Archivo *. cshtml.cs* que contiene C# código que controla los eventos de página.</span><span class="sxs-lookup"><span data-stu-id="241fd-167">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="241fd-168">Los archivos auxiliares tienen nombres que comienzan con un carácter de subrayado.</span><span class="sxs-lookup"><span data-stu-id="241fd-168">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="241fd-169">Por ejemplo, el archivo *_Layout.cshtml* configura los elementos de la interfaz de usuario comunes a todas las páginas.</span><span class="sxs-lookup"><span data-stu-id="241fd-169">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="241fd-170">Este archivo configura el menú de navegación de la parte superior de la página y el aviso de copyright de la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="241fd-170">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="241fd-171">Para más información, consulte <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="241fd-171">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="241fd-172">Carpeta wwwroot</span><span class="sxs-lookup"><span data-stu-id="241fd-172">wwwroot folder</span></span>

<span data-ttu-id="241fd-173">Contiene los archivos estáticos, como los archivos HTML, los archivos de JavaScript y los archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="241fd-173">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="241fd-174">Para más información, consulte <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="241fd-174">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="241fd-175">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="241fd-175">appSettings.json</span></span>

<span data-ttu-id="241fd-176">Contiene los datos de configuración, como las cadenas de conexión.</span><span class="sxs-lookup"><span data-stu-id="241fd-176">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="241fd-177">Para más información, consulte <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="241fd-177">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="241fd-178">Program.cs</span><span class="sxs-lookup"><span data-stu-id="241fd-178">Program.cs</span></span>

<span data-ttu-id="241fd-179">Contiene el punto de entrada del programa.</span><span class="sxs-lookup"><span data-stu-id="241fd-179">Contains the entry point for the program.</span></span> <span data-ttu-id="241fd-180">Para más información, consulte <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="241fd-180">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="241fd-181">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="241fd-181">Startup.cs</span></span>

<span data-ttu-id="241fd-182">Contiene código que configura el comportamiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="241fd-182">Contains code that configures app behavior.</span></span> <span data-ttu-id="241fd-183">Para más información, consulte <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="241fd-183">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="241fd-184">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="241fd-184">Next steps</span></span>

<span data-ttu-id="241fd-185">Pase al siguiente tutorial de la serie:</span><span class="sxs-lookup"><span data-stu-id="241fd-185">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="241fd-186">Agregar un modelo</span><span class="sxs-lookup"><span data-stu-id="241fd-186">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="241fd-187">Este es el primer tutorial de una serie.</span><span class="sxs-lookup"><span data-stu-id="241fd-187">This is the first tutorial of a series.</span></span> <span data-ttu-id="241fd-188">En la [serie](xref:tutorials/razor-pages/index) se enseñan los conceptos básicos de la compilación de una aplicación web de Razor Pages en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="241fd-188">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="241fd-189">Al final de la serie, tendrá una aplicación que puede administrar una base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="241fd-189">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="241fd-190">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="241fd-190">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="241fd-191">Crear una aplicación web de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="241fd-191">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="241fd-192">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="241fd-192">Run the app.</span></span>
> * <span data-ttu-id="241fd-193">Examinar los archivos de proyecto.</span><span class="sxs-lookup"><span data-stu-id="241fd-193">Examine the project files.</span></span>

<span data-ttu-id="241fd-194">Al final de este tutorial, tendrá una aplicación web de Razor Pages que compilará en los tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="241fd-194">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="241fd-196">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="241fd-196">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="241fd-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="241fd-197">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="241fd-198">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="241fd-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="241fd-199">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="241fd-199">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="241fd-200">Creación de una aplicación web de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="241fd-200">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="241fd-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="241fd-201">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="241fd-202">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="241fd-202">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="241fd-203">Cree una nueva aplicación web de ASP.NET Core y seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="241fd-203">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="241fd-205">Asigne al proyecto el nombre **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="241fd-205">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="241fd-206">Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="241fd-206">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="241fd-208">Seleccione **ASP.NET Core 2.2** en la lista desplegable, después **Aplicación web** y, por último, **Crear**.</span><span class="sxs-lookup"><span data-stu-id="241fd-208">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="241fd-210">Se crea el proyecto de inicio siguiente:</span><span class="sxs-lookup"><span data-stu-id="241fd-210">The following starter project is created:</span></span>

  ![Explorador de soluciones](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="241fd-212">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="241fd-212">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="241fd-213">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="241fd-213">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="241fd-214">Cambie al directorio (`cd`) que contiene el proyecto.</span><span class="sxs-lookup"><span data-stu-id="241fd-214">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="241fd-215">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="241fd-215">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="241fd-216">El comando `dotnet new` crea un proyecto de Razor Pages en la carpeta *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="241fd-216">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="241fd-217">El comando `code` abre la carpeta *RazorPagesMovie* en la instancia actual de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="241fd-217">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="241fd-218">Cuando el icono de llama de OmniSharp de la barra de estado se ponga verde, aparecerá un cuadro de diálogo que pregunta **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="241fd-218">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="241fd-219">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="241fd-219">Select **Yes**.</span></span>

  <span data-ttu-id="241fd-220">Un directorio *.vscode*, que contiene archivos *launch.json* y *tasks.json*, se agrega al directorio raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="241fd-220">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="241fd-221">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="241fd-221">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="241fd-222">Desde un terminal, ejecute este comando:</span><span class="sxs-lookup"><span data-stu-id="241fd-222">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="241fd-223">Los comandos anteriores utilizan la [CLI de .NET Core](/dotnet/core/tools/dotnet) para crear un proyecto de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="241fd-223">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="241fd-224">Abrir el proyecto</span><span class="sxs-lookup"><span data-stu-id="241fd-224">Open the project</span></span>

<span data-ttu-id="241fd-225">En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="241fd-225">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="241fd-226">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="241fd-226">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="241fd-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="241fd-227">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="241fd-228">Presione Ctrl+F5 para ejecutarla sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="241fd-228">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="241fd-229">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="241fd-229">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="241fd-230">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="241fd-230">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="241fd-231">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="241fd-231">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="241fd-232">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="241fd-232">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="241fd-233">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="241fd-233">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="241fd-234">En la página principal de la aplicación, seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="241fd-234">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="241fd-235">Esta aplicación no realiza un seguimiento de la información personal, pero la plantilla del proyecto incluye la función de consentimiento en caso de que sea necesaria para cumplir con el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr) de la Unión Europea.</span><span class="sxs-lookup"><span data-stu-id="241fd-235">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="241fd-237">En la siguiente imagen se muestra la aplicación tras haber dado su consentimiento al seguimiento:</span><span class="sxs-lookup"><span data-stu-id="241fd-237">The following image shows the app after you give consent to tracking:</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="241fd-239">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="241fd-239">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="241fd-240">Presione **Ctrl+F5** para ejecutar sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="241fd-240">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="241fd-241">Visual Studio Code inicia [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplaza hasta `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="241fd-241">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="241fd-242">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="241fd-242">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="241fd-243">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="241fd-243">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="241fd-244">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="241fd-244">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="241fd-245">En la página principal de la aplicación, seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="241fd-245">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="241fd-246">Esta aplicación no realiza un seguimiento de la información personal, pero la plantilla del proyecto incluye la función de consentimiento en caso de que sea necesaria para cumplir con el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr) de la Unión Europea.</span><span class="sxs-lookup"><span data-stu-id="241fd-246">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="241fd-248">En la siguiente imagen se muestra la aplicación tras haber dado su consentimiento al seguimiento:</span><span class="sxs-lookup"><span data-stu-id="241fd-248">The following image shows the app after you give consent to tracking:</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="241fd-250">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="241fd-250">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="241fd-251">Presione **Cmd-Opt-F5** para realizar la ejecución sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="241fd-251">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="241fd-252">Visual Studio iniciará [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplazará a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="241fd-252">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="241fd-253">En la página principal de la aplicación, seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="241fd-253">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="241fd-254">Esta aplicación no realiza un seguimiento de la información personal, pero la plantilla del proyecto incluye la función de consentimiento en caso de que sea necesaria para cumplir con el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr) de la Unión Europea.</span><span class="sxs-lookup"><span data-stu-id="241fd-254">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="241fd-256">En la siguiente imagen se muestra la aplicación tras haber dado su consentimiento al seguimiento:</span><span class="sxs-lookup"><span data-stu-id="241fd-256">The following image shows the app after you give consent to tracking:</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="241fd-258">Examen de los archivo del proyecto</span><span class="sxs-lookup"><span data-stu-id="241fd-258">Examine the project files</span></span>

<span data-ttu-id="241fd-259">He aquí un resumen de las principales carpetas y archivos del proyecto con los que va a trabajar en los próximos tutoriales.</span><span class="sxs-lookup"><span data-stu-id="241fd-259">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="241fd-260">Carpeta Pages</span><span class="sxs-lookup"><span data-stu-id="241fd-260">Pages folder</span></span>

<span data-ttu-id="241fd-261">Contiene Razor Pages y los archivos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="241fd-261">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="241fd-262">Cada página de Razor se compone de un par de archivos:</span><span class="sxs-lookup"><span data-stu-id="241fd-262">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="241fd-263">Archivo *.cshtml* que contiene el marcado HTML con código C# que usa la sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="241fd-263">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="241fd-264">Archivo *. cshtml.cs* que contiene C# código que controla los eventos de página.</span><span class="sxs-lookup"><span data-stu-id="241fd-264">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="241fd-265">Los archivos auxiliares tienen nombres que comienzan con un carácter de subrayado.</span><span class="sxs-lookup"><span data-stu-id="241fd-265">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="241fd-266">Por ejemplo, el archivo *_Layout.cshtml* configura los elementos de la interfaz de usuario comunes a todas las páginas.</span><span class="sxs-lookup"><span data-stu-id="241fd-266">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="241fd-267">Este archivo configura el menú de navegación de la parte superior de la página y el aviso de copyright de la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="241fd-267">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="241fd-268">Para más información, consulte <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="241fd-268">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="241fd-269">Carpeta wwwroot</span><span class="sxs-lookup"><span data-stu-id="241fd-269">wwwroot folder</span></span>

<span data-ttu-id="241fd-270">Contiene los archivos estáticos, como los archivos HTML, los archivos de JavaScript y los archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="241fd-270">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="241fd-271">Para más información, consulte <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="241fd-271">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="241fd-272">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="241fd-272">appSettings.json</span></span>

<span data-ttu-id="241fd-273">Contiene los datos de configuración, como las cadenas de conexión.</span><span class="sxs-lookup"><span data-stu-id="241fd-273">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="241fd-274">Para más información, consulte <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="241fd-274">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="241fd-275">Program.cs</span><span class="sxs-lookup"><span data-stu-id="241fd-275">Program.cs</span></span>

<span data-ttu-id="241fd-276">Contiene el punto de entrada del programa.</span><span class="sxs-lookup"><span data-stu-id="241fd-276">Contains the entry point for the program.</span></span> <span data-ttu-id="241fd-277">Para más información, consulte <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="241fd-277">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="241fd-278">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="241fd-278">Startup.cs</span></span>

<span data-ttu-id="241fd-279">Contiene código que configura el comportamiento de la aplicación, como, por ejemplo, si se requiere consentimiento para las cookies.</span><span class="sxs-lookup"><span data-stu-id="241fd-279">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="241fd-280">Para más información, consulte <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="241fd-280">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="241fd-281">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="241fd-281">Additional resources</span></span>

* [<span data-ttu-id="241fd-282">Versión en YouTube de este tutorial</span><span class="sxs-lookup"><span data-stu-id="241fd-282">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="241fd-283">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="241fd-283">Next steps</span></span>

<span data-ttu-id="241fd-284">Pase al siguiente tutorial de la serie:</span><span class="sxs-lookup"><span data-stu-id="241fd-284">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="241fd-285">Agregar un modelo</span><span class="sxs-lookup"><span data-stu-id="241fd-285">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end