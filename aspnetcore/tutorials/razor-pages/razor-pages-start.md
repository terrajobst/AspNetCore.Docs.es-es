---
title: 'Tutorial: Introducción a Razor Pages en ASP.NET Core'
author: rick-anderson
description: Esta serie de tutoriales muestra cómo usar Razor Pages en ASP.NET Core. Obtenga información sobre cómo crear un modelo, generar código para Razor Pages, usar Entity Framework Core y SQL Server para el acceso a datos, agregar la funcionalidad de búsqueda, agregar validación de entrada y usar migraciones para actualizar el modelo.
ms.author: riande
ms.date: 07/25/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 57a10895c718c539ece280afcb27cb4033c7fb45
ms.sourcegitcommit: 979dbfc5e9ce09b9470789989cddfcfb57079d94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682793"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="1c42e-104">Tutorial: Introducción a Razor Pages en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1c42e-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="1c42e-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1c42e-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="1c42e-106">Este es el primer tutorial de una serie de varios que enseña los aspectos básicos de la compilación de una aplicación web de Razor Pages de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c42e-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="1c42e-107">Al final de la serie, tendrá una aplicación que puede administrar una base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="1c42e-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="1c42e-108">En este tutorial, hizo lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1c42e-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1c42e-109">Crear una aplicación web de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="1c42e-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="1c42e-110">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1c42e-110">Run the app.</span></span>
> * <span data-ttu-id="1c42e-111">Examinar los archivos de proyecto.</span><span class="sxs-lookup"><span data-stu-id="1c42e-111">Examine the project files.</span></span>

<span data-ttu-id="1c42e-112">Al final de este tutorial, tendrá una aplicación web de Razor Pages que compilará en los tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="1c42e-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="1c42e-114">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="1c42e-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c42e-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c42e-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1c42e-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1c42e-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1c42e-117">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="1c42e-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="1c42e-118">Creación de una aplicación web de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="1c42e-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c42e-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c42e-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1c42e-120">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="1c42e-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="1c42e-121">Cree una nueva aplicación web de ASP.NET Core y seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="1c42e-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="1c42e-122">![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="1c42e-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="1c42e-123">Asigne al proyecto el nombre **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="1c42e-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="1c42e-124">Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="1c42e-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="1c42e-125">![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="1c42e-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="1c42e-126">Seleccione **ASP.NET Core 3.0** en la lista desplegable, después **Aplicación web** y, por último, **Crear**.</span><span class="sxs-lookup"><span data-stu-id="1c42e-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="1c42e-128">Se crea el proyecto de inicio siguiente:</span><span class="sxs-lookup"><span data-stu-id="1c42e-128">The following starter project is created:</span></span>

  ![Explorador de soluciones](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1c42e-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1c42e-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="1c42e-131">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="1c42e-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="1c42e-132">Cambie al directorio (`cd`) que contiene el proyecto.</span><span class="sxs-lookup"><span data-stu-id="1c42e-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="1c42e-133">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="1c42e-133">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="1c42e-134">El comando `dotnet new` crea un proyecto de Razor Pages en la carpeta *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="1c42e-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="1c42e-135">El comando `code` abre la carpeta *RazorPagesMovie* en la instancia actual de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="1c42e-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="1c42e-136">Cuando el icono de llama de OmniSharp de la barra de estado se ponga verde, aparecerá un cuadro de diálogo que pregunta **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="1c42e-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="1c42e-137">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="1c42e-137">Select **Yes**.</span></span>

  <span data-ttu-id="1c42e-138">Un directorio *.vscode*, que contiene archivos *launch.json* y *tasks.json*, se agrega al directorio raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="1c42e-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1c42e-139">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="1c42e-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="1c42e-140">Desde un terminal, ejecute este comando:</span><span class="sxs-lookup"><span data-stu-id="1c42e-140">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="1c42e-141">Los comandos anteriores utilizan la [CLI de .NET Core](/dotnet/core/tools/dotnet) para crear un proyecto de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="1c42e-141">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="1c42e-142">Abrir el proyecto</span><span class="sxs-lookup"><span data-stu-id="1c42e-142">Open the project</span></span>

<span data-ttu-id="1c42e-143">En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="1c42e-143">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="1c42e-144">Ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="1c42e-144">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c42e-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c42e-145">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1c42e-146">Presione Ctrl+F5 para ejecutarla sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="1c42e-146">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="1c42e-147">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1c42e-147">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="1c42e-148">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="1c42e-148">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="1c42e-149">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="1c42e-149">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="1c42e-150">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="1c42e-150">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="1c42e-151">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="1c42e-151">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1c42e-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1c42e-152">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="1c42e-153">Presione **Ctrl+F5** para ejecutar sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="1c42e-153">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="1c42e-154">Visual Studio Code inicia [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplaza hasta `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="1c42e-154">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="1c42e-155">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="1c42e-155">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="1c42e-156">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="1c42e-156">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="1c42e-157">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="1c42e-157">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1c42e-158">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="1c42e-158">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="1c42e-159">Presione **Alt-Cmd-Entrar** para realizar la ejecución sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="1c42e-159">Press **Alt-Cmd-Enter** to run without the debugger.</span></span> <span data-ttu-id="1c42e-160">O bien, en la barra de menús, vaya a Ejecutar > Iniciar sin depurar.</span><span class="sxs-lookup"><span data-stu-id="1c42e-160">Alternatively, navigate to the menu bar and go to Run>Start Without Debugging.</span></span>

  <span data-ttu-id="1c42e-161">Visual Studio iniciará [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplazará a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="1c42e-161">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="1c42e-162">Examen de los archivo del proyecto</span><span class="sxs-lookup"><span data-stu-id="1c42e-162">Examine the project files</span></span>

<span data-ttu-id="1c42e-163">He aquí un resumen de las principales carpetas y archivos del proyecto con los que va a trabajar en los próximos tutoriales.</span><span class="sxs-lookup"><span data-stu-id="1c42e-163">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="1c42e-164">Carpeta Pages</span><span class="sxs-lookup"><span data-stu-id="1c42e-164">Pages folder</span></span>

<span data-ttu-id="1c42e-165">Contiene Razor Pages y los archivos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="1c42e-165">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="1c42e-166">Cada página de Razor se compone de un par de archivos:</span><span class="sxs-lookup"><span data-stu-id="1c42e-166">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="1c42e-167">Archivo *.cshtml* que contiene el marcado HTML con código C# que usa la sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="1c42e-167">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="1c42e-168">Archivo *. cshtml.cs* que contiene C# código que controla los eventos de página.</span><span class="sxs-lookup"><span data-stu-id="1c42e-168">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="1c42e-169">Los archivos auxiliares tienen nombres que comienzan con un carácter de subrayado.</span><span class="sxs-lookup"><span data-stu-id="1c42e-169">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="1c42e-170">Por ejemplo, el archivo *_Layout.cshtml* configura los elementos de la interfaz de usuario comunes a todas las páginas.</span><span class="sxs-lookup"><span data-stu-id="1c42e-170">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="1c42e-171">Este archivo configura el menú de navegación de la parte superior de la página y el aviso de copyright de la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="1c42e-171">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="1c42e-172">Para más información, consulte <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="1c42e-172">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="1c42e-173">Carpeta wwwroot</span><span class="sxs-lookup"><span data-stu-id="1c42e-173">wwwroot folder</span></span>

<span data-ttu-id="1c42e-174">Contiene los archivos estáticos, como los archivos HTML, los archivos de JavaScript y los archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="1c42e-174">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="1c42e-175">Para más información, consulte <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="1c42e-175">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="1c42e-176">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="1c42e-176">appSettings.json</span></span>

<span data-ttu-id="1c42e-177">Contiene los datos de configuración, como las cadenas de conexión.</span><span class="sxs-lookup"><span data-stu-id="1c42e-177">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="1c42e-178">Para más información, consulte <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="1c42e-178">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="1c42e-179">Program.cs</span><span class="sxs-lookup"><span data-stu-id="1c42e-179">Program.cs</span></span>

<span data-ttu-id="1c42e-180">Contiene el punto de entrada del programa.</span><span class="sxs-lookup"><span data-stu-id="1c42e-180">Contains the entry point for the program.</span></span> <span data-ttu-id="1c42e-181">Para más información, consulte <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="1c42e-181">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="1c42e-182">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="1c42e-182">Startup.cs</span></span>

<span data-ttu-id="1c42e-183">Contiene código que configura el comportamiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1c42e-183">Contains code that configures app behavior.</span></span> <span data-ttu-id="1c42e-184">Para más información, consulte <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="1c42e-184">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1c42e-185">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="1c42e-185">Next steps</span></span>

<span data-ttu-id="1c42e-186">Pase al siguiente tutorial de la serie:</span><span class="sxs-lookup"><span data-stu-id="1c42e-186">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="1c42e-187">Agregar un modelo</span><span class="sxs-lookup"><span data-stu-id="1c42e-187">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1c42e-188">Este es el primer tutorial de una serie.</span><span class="sxs-lookup"><span data-stu-id="1c42e-188">This is the first tutorial of a series.</span></span> <span data-ttu-id="1c42e-189">En la [serie](xref:tutorials/razor-pages/index) se enseñan los conceptos básicos de la compilación de una aplicación web de Razor Pages en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c42e-189">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="1c42e-190">Al final de la serie, tendrá una aplicación que puede administrar una base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="1c42e-190">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="1c42e-191">En este tutorial, hizo lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1c42e-191">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1c42e-192">Crear una aplicación web de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="1c42e-192">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="1c42e-193">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1c42e-193">Run the app.</span></span>
> * <span data-ttu-id="1c42e-194">Examinar los archivos de proyecto.</span><span class="sxs-lookup"><span data-stu-id="1c42e-194">Examine the project files.</span></span>

<span data-ttu-id="1c42e-195">Al final de este tutorial, tendrá una aplicación web de Razor Pages que compilará en los tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="1c42e-195">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="1c42e-197">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="1c42e-197">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c42e-198">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c42e-198">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1c42e-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1c42e-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1c42e-200">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="1c42e-200">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="1c42e-201">Creación de una aplicación web de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="1c42e-201">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c42e-202">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c42e-202">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1c42e-203">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="1c42e-203">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="1c42e-204">Cree una nueva aplicación web de ASP.NET Core y seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="1c42e-204">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="1c42e-206">Asigne al proyecto el nombre **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="1c42e-206">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="1c42e-207">Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="1c42e-207">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="1c42e-209">Seleccione **ASP.NET Core 2.2** en la lista desplegable, después **Aplicación web** y, por último, **Crear**.</span><span class="sxs-lookup"><span data-stu-id="1c42e-209">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="1c42e-211">Se crea el proyecto de inicio siguiente:</span><span class="sxs-lookup"><span data-stu-id="1c42e-211">The following starter project is created:</span></span>

  ![Explorador de soluciones](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1c42e-213">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1c42e-213">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="1c42e-214">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="1c42e-214">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="1c42e-215">Cambie al directorio (`cd`) que contiene el proyecto.</span><span class="sxs-lookup"><span data-stu-id="1c42e-215">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="1c42e-216">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="1c42e-216">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="1c42e-217">El comando `dotnet new` crea un proyecto de Razor Pages en la carpeta *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="1c42e-217">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="1c42e-218">El comando `code` abre la carpeta *RazorPagesMovie* en la instancia actual de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="1c42e-218">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="1c42e-219">Cuando el icono de llama de OmniSharp de la barra de estado se ponga verde, aparecerá un cuadro de diálogo que pregunta **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="1c42e-219">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="1c42e-220">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="1c42e-220">Select **Yes**.</span></span>

  <span data-ttu-id="1c42e-221">Un directorio *.vscode*, que contiene archivos *launch.json* y *tasks.json*, se agrega al directorio raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="1c42e-221">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1c42e-222">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="1c42e-222">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="1c42e-223">Desde un terminal, ejecute este comando:</span><span class="sxs-lookup"><span data-stu-id="1c42e-223">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="1c42e-224">Los comandos anteriores utilizan la [CLI de .NET Core](/dotnet/core/tools/dotnet) para crear un proyecto de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="1c42e-224">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="1c42e-225">Abrir el proyecto</span><span class="sxs-lookup"><span data-stu-id="1c42e-225">Open the project</span></span>

<span data-ttu-id="1c42e-226">En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="1c42e-226">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="1c42e-227">Ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="1c42e-227">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c42e-228">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c42e-228">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1c42e-229">Presione Ctrl+F5 para ejecutarla sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="1c42e-229">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="1c42e-230">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1c42e-230">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="1c42e-231">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="1c42e-231">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="1c42e-232">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="1c42e-232">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="1c42e-233">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="1c42e-233">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="1c42e-234">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="1c42e-234">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="1c42e-235">En la página principal de la aplicación, seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="1c42e-235">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="1c42e-236">Esta aplicación no realiza un seguimiento de la información personal, pero la plantilla del proyecto incluye la función de consentimiento en caso de que sea necesaria para cumplir con el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr) de la Unión Europea.</span><span class="sxs-lookup"><span data-stu-id="1c42e-236">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="1c42e-238">En la siguiente imagen se muestra la aplicación tras haber dado su consentimiento al seguimiento:</span><span class="sxs-lookup"><span data-stu-id="1c42e-238">The following image shows the app after you give consent to tracking:</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1c42e-240">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1c42e-240">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="1c42e-241">Presione **Ctrl+F5** para ejecutar sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="1c42e-241">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="1c42e-242">Visual Studio Code inicia [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplaza hasta `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="1c42e-242">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="1c42e-243">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="1c42e-243">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="1c42e-244">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="1c42e-244">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="1c42e-245">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="1c42e-245">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="1c42e-246">En la página principal de la aplicación, seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="1c42e-246">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="1c42e-247">Esta aplicación no realiza un seguimiento de la información personal, pero la plantilla del proyecto incluye la función de consentimiento en caso de que sea necesaria para cumplir con el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr) de la Unión Europea.</span><span class="sxs-lookup"><span data-stu-id="1c42e-247">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="1c42e-249">En la siguiente imagen se muestra la aplicación tras haber dado su consentimiento al seguimiento:</span><span class="sxs-lookup"><span data-stu-id="1c42e-249">The following image shows the app after you give consent to tracking:</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1c42e-251">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="1c42e-251">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="1c42e-252">Presione **Cmd-Opt-F5** para realizar la ejecución sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="1c42e-252">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="1c42e-253">Visual Studio iniciará [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplazará a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="1c42e-253">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="1c42e-254">En la página principal de la aplicación, seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="1c42e-254">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="1c42e-255">Esta aplicación no realiza un seguimiento de la información personal, pero la plantilla del proyecto incluye la función de consentimiento en caso de que sea necesaria para cumplir con el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr) de la Unión Europea.</span><span class="sxs-lookup"><span data-stu-id="1c42e-255">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="1c42e-257">En la siguiente imagen se muestra la aplicación tras haber dado su consentimiento al seguimiento:</span><span class="sxs-lookup"><span data-stu-id="1c42e-257">The following image shows the app after you give consent to tracking:</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="1c42e-259">Examen de los archivo del proyecto</span><span class="sxs-lookup"><span data-stu-id="1c42e-259">Examine the project files</span></span>

<span data-ttu-id="1c42e-260">He aquí un resumen de las principales carpetas y archivos del proyecto con los que va a trabajar en los próximos tutoriales.</span><span class="sxs-lookup"><span data-stu-id="1c42e-260">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="1c42e-261">Carpeta Pages</span><span class="sxs-lookup"><span data-stu-id="1c42e-261">Pages folder</span></span>

<span data-ttu-id="1c42e-262">Contiene Razor Pages y los archivos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="1c42e-262">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="1c42e-263">Cada página de Razor se compone de un par de archivos:</span><span class="sxs-lookup"><span data-stu-id="1c42e-263">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="1c42e-264">Archivo *.cshtml* que contiene el marcado HTML con código C# que usa la sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="1c42e-264">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="1c42e-265">Archivo *. cshtml.cs* que contiene C# código que controla los eventos de página.</span><span class="sxs-lookup"><span data-stu-id="1c42e-265">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="1c42e-266">Los archivos auxiliares tienen nombres que comienzan con un carácter de subrayado.</span><span class="sxs-lookup"><span data-stu-id="1c42e-266">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="1c42e-267">Por ejemplo, el archivo *_Layout.cshtml* configura los elementos de la interfaz de usuario comunes a todas las páginas.</span><span class="sxs-lookup"><span data-stu-id="1c42e-267">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="1c42e-268">Este archivo configura el menú de navegación de la parte superior de la página y el aviso de copyright de la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="1c42e-268">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="1c42e-269">Para más información, consulte <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="1c42e-269">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="1c42e-270">Carpeta wwwroot</span><span class="sxs-lookup"><span data-stu-id="1c42e-270">wwwroot folder</span></span>

<span data-ttu-id="1c42e-271">Contiene los archivos estáticos, como los archivos HTML, los archivos de JavaScript y los archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="1c42e-271">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="1c42e-272">Para más información, consulte <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="1c42e-272">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="1c42e-273">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="1c42e-273">appSettings.json</span></span>

<span data-ttu-id="1c42e-274">Contiene los datos de configuración, como las cadenas de conexión.</span><span class="sxs-lookup"><span data-stu-id="1c42e-274">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="1c42e-275">Para más información, consulte <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="1c42e-275">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="1c42e-276">Program.cs</span><span class="sxs-lookup"><span data-stu-id="1c42e-276">Program.cs</span></span>

<span data-ttu-id="1c42e-277">Contiene el punto de entrada del programa.</span><span class="sxs-lookup"><span data-stu-id="1c42e-277">Contains the entry point for the program.</span></span> <span data-ttu-id="1c42e-278">Para más información, consulte <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="1c42e-278">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="1c42e-279">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="1c42e-279">Startup.cs</span></span>

<span data-ttu-id="1c42e-280">Contiene código que configura el comportamiento de la aplicación, como, por ejemplo, si se requiere consentimiento para las cookies.</span><span class="sxs-lookup"><span data-stu-id="1c42e-280">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="1c42e-281">Para más información, consulte <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="1c42e-281">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1c42e-282">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="1c42e-282">Additional resources</span></span>

* [<span data-ttu-id="1c42e-283">Versión en YouTube de este tutorial</span><span class="sxs-lookup"><span data-stu-id="1c42e-283">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="1c42e-284">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="1c42e-284">Next steps</span></span>

<span data-ttu-id="1c42e-285">Pase al siguiente tutorial de la serie:</span><span class="sxs-lookup"><span data-stu-id="1c42e-285">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="1c42e-286">Agregar un modelo</span><span class="sxs-lookup"><span data-stu-id="1c42e-286">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
