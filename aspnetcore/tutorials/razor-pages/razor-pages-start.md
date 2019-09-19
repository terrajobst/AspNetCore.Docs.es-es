---
title: 'Tutorial: Introducción a Razor Pages en ASP.NET Core'
author: rick-anderson
description: Esta serie de tutoriales muestra cómo usar Razor Pages en ASP.NET Core. Obtenga información sobre cómo crear un modelo, generar código para Razor Pages, usar Entity Framework Core y SQL Server para el acceso a datos, agregar la funcionalidad de búsqueda, agregar validación de entrada y usar migraciones para actualizar el modelo.
ms.author: riande
ms.date: 07/25/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 0cc00cb85b6054752417b82c783cfd4c306aeda5
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082574"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="adf90-104">Tutorial: Introducción a Razor Pages en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="adf90-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="adf90-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="adf90-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="adf90-106">Este es el primer tutorial de una serie de varios que enseña los aspectos básicos de la compilación de una aplicación web de Razor Pages de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="adf90-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="adf90-107">Al final de la serie, tendrá una aplicación que puede administrar una base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="adf90-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="adf90-108">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="adf90-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="adf90-109">Crear una aplicación web de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="adf90-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="adf90-110">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="adf90-110">Run the app.</span></span>
> * <span data-ttu-id="adf90-111">Examinar los archivos de proyecto.</span><span class="sxs-lookup"><span data-stu-id="adf90-111">Examine the project files.</span></span>

<span data-ttu-id="adf90-112">Al final de este tutorial, tendrá una aplicación web de Razor Pages que compilará en los tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="adf90-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="adf90-114">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="adf90-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="adf90-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="adf90-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="adf90-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="adf90-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="adf90-117">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="adf90-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="adf90-118">Creación de una aplicación web de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="adf90-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="adf90-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="adf90-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="adf90-120">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="adf90-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="adf90-121">Cree una nueva aplicación web de ASP.NET Core y seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="adf90-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="adf90-122">![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="adf90-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="adf90-123">Asigne al proyecto el nombre **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="adf90-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="adf90-124">Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="adf90-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="adf90-125">![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="adf90-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="adf90-126">Seleccione **ASP.NET Core 3.0** en la lista desplegable, después **Aplicación web** y, por último, **Crear**.</span><span class="sxs-lookup"><span data-stu-id="adf90-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="adf90-128">Se crea el proyecto de inicio siguiente:</span><span class="sxs-lookup"><span data-stu-id="adf90-128">The following starter project is created:</span></span>

  ![Explorador de soluciones](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="adf90-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="adf90-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="adf90-131">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="adf90-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="adf90-132">Cambie al directorio (`cd`) que contiene el proyecto.</span><span class="sxs-lookup"><span data-stu-id="adf90-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="adf90-133">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="adf90-133">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="adf90-134">El comando `dotnet new` crea un proyecto de Razor Pages en la carpeta *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="adf90-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="adf90-135">El comando `code` abre la carpeta *RazorPagesMovie* en la instancia actual de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="adf90-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="adf90-136">Cuando el icono de llama de OmniSharp de la barra de estado se ponga verde, aparecerá un cuadro de diálogo que pregunta **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="adf90-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="adf90-137">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="adf90-137">Select **Yes**.</span></span>

  <span data-ttu-id="adf90-138">Un directorio *.vscode*, que contiene archivos *launch.json* y *tasks.json*, se agrega al directorio raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="adf90-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="adf90-139">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="adf90-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="adf90-140">Seleccione **Archivo** > **Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="adf90-140">Select **File** > **New Solution**.</span></span>

![macOS: Nueva solución](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="adf90-142">Seleccione **.NET Core** > **Aplicación** > **Aplicación web** > **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="adf90-142">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![Cuadro de diálogo de nuevo proyecto de macOS](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="adf90-144">En el cuadro de diálogo **Configurar la nueva API web de ASP.NET Core**, establezca la **plataforma de destino** en **.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="adf90-144">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** to **.NET Core 3.0**.</span></span>

  ![Selección de .NET Core 3.0 de macOS](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="adf90-146">Asigne el nombre **RazorPagesMovie** al proyecto y, después, seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="adf90-146">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)


## <a name="open-the-project"></a><span data-ttu-id="adf90-148">Abrir el proyecto</span><span class="sxs-lookup"><span data-stu-id="adf90-148">Open the project</span></span>

<span data-ttu-id="adf90-149">En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="adf90-149">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="adf90-150">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="adf90-150">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="adf90-151">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="adf90-151">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="adf90-152">Presione Ctrl+F5 para ejecutarla sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="adf90-152">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="adf90-153">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="adf90-153">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="adf90-154">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="adf90-154">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="adf90-155">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="adf90-155">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="adf90-156">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="adf90-156">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="adf90-157">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="adf90-157">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="adf90-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="adf90-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="adf90-159">Presione **Ctrl+F5** para ejecutar sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="adf90-159">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="adf90-160">Visual Studio Code inicia [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplaza hasta `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="adf90-160">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="adf90-161">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="adf90-161">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="adf90-162">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="adf90-162">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="adf90-163">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="adf90-163">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="adf90-164">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="adf90-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="adf90-165">Presione **Alt-Cmd-Entrar** para realizar la ejecución sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="adf90-165">Press **Alt-Cmd-Enter** to run without the debugger.</span></span> <span data-ttu-id="adf90-166">O bien, en la barra de menús, vaya a Ejecutar > Iniciar sin depurar.</span><span class="sxs-lookup"><span data-stu-id="adf90-166">Alternatively, navigate to the menu bar and go to Run>Start Without Debugging.</span></span>

  <span data-ttu-id="adf90-167">Visual Studio iniciará [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplazará a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="adf90-167">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="adf90-168">Examen de los archivo del proyecto</span><span class="sxs-lookup"><span data-stu-id="adf90-168">Examine the project files</span></span>

<span data-ttu-id="adf90-169">He aquí un resumen de las principales carpetas y archivos del proyecto con los que va a trabajar en los próximos tutoriales.</span><span class="sxs-lookup"><span data-stu-id="adf90-169">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="adf90-170">Carpeta Pages</span><span class="sxs-lookup"><span data-stu-id="adf90-170">Pages folder</span></span>

<span data-ttu-id="adf90-171">Contiene Razor Pages y los archivos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="adf90-171">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="adf90-172">Cada página de Razor se compone de un par de archivos:</span><span class="sxs-lookup"><span data-stu-id="adf90-172">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="adf90-173">Archivo *.cshtml* que contiene el marcado HTML con código C# que usa la sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="adf90-173">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="adf90-174">Archivo *. cshtml.cs* que contiene C# código que controla los eventos de página.</span><span class="sxs-lookup"><span data-stu-id="adf90-174">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="adf90-175">Los archivos auxiliares tienen nombres que comienzan con un carácter de subrayado.</span><span class="sxs-lookup"><span data-stu-id="adf90-175">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="adf90-176">Por ejemplo, el archivo *_Layout.cshtml* configura los elementos de la interfaz de usuario comunes a todas las páginas.</span><span class="sxs-lookup"><span data-stu-id="adf90-176">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="adf90-177">Este archivo configura el menú de navegación de la parte superior de la página y el aviso de copyright de la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="adf90-177">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="adf90-178">Para más información, consulte <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="adf90-178">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="adf90-179">Carpeta wwwroot</span><span class="sxs-lookup"><span data-stu-id="adf90-179">wwwroot folder</span></span>

<span data-ttu-id="adf90-180">Contiene los archivos estáticos, como los archivos HTML, los archivos de JavaScript y los archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="adf90-180">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="adf90-181">Para más información, consulte <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="adf90-181">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="adf90-182">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="adf90-182">appSettings.json</span></span>

<span data-ttu-id="adf90-183">Contiene los datos de configuración, como las cadenas de conexión.</span><span class="sxs-lookup"><span data-stu-id="adf90-183">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="adf90-184">Para más información, consulte <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="adf90-184">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="adf90-185">Program.cs</span><span class="sxs-lookup"><span data-stu-id="adf90-185">Program.cs</span></span>

<span data-ttu-id="adf90-186">Contiene el punto de entrada del programa.</span><span class="sxs-lookup"><span data-stu-id="adf90-186">Contains the entry point for the program.</span></span> <span data-ttu-id="adf90-187">Para más información, consulte <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="adf90-187">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="adf90-188">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="adf90-188">Startup.cs</span></span>

<span data-ttu-id="adf90-189">Contiene código que configura el comportamiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="adf90-189">Contains code that configures app behavior.</span></span> <span data-ttu-id="adf90-190">Para más información, consulte <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="adf90-190">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="adf90-191">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="adf90-191">Next steps</span></span>

<span data-ttu-id="adf90-192">Pase al siguiente tutorial de la serie:</span><span class="sxs-lookup"><span data-stu-id="adf90-192">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="adf90-193">Agregar un modelo</span><span class="sxs-lookup"><span data-stu-id="adf90-193">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="adf90-194">Este es el primer tutorial de una serie.</span><span class="sxs-lookup"><span data-stu-id="adf90-194">This is the first tutorial of a series.</span></span> <span data-ttu-id="adf90-195">En la [serie](xref:tutorials/razor-pages/index) se enseñan los conceptos básicos de la compilación de una aplicación web de Razor Pages en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="adf90-195">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="adf90-196">Al final de la serie, tendrá una aplicación que puede administrar una base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="adf90-196">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="adf90-197">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="adf90-197">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="adf90-198">Crear una aplicación web de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="adf90-198">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="adf90-199">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="adf90-199">Run the app.</span></span>
> * <span data-ttu-id="adf90-200">Examinar los archivos de proyecto.</span><span class="sxs-lookup"><span data-stu-id="adf90-200">Examine the project files.</span></span>

<span data-ttu-id="adf90-201">Al final de este tutorial, tendrá una aplicación web de Razor Pages que compilará en los tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="adf90-201">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="adf90-203">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="adf90-203">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="adf90-204">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="adf90-204">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="adf90-205">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="adf90-205">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="adf90-206">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="adf90-206">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="adf90-207">Creación de una aplicación web de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="adf90-207">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="adf90-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="adf90-208">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="adf90-209">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="adf90-209">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="adf90-210">Cree una nueva aplicación web de ASP.NET Core y seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="adf90-210">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="adf90-212">Asigne al proyecto el nombre **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="adf90-212">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="adf90-213">Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="adf90-213">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="adf90-215">Seleccione **ASP.NET Core 2.2** en la lista desplegable, después **Aplicación web** y, por último, **Crear**.</span><span class="sxs-lookup"><span data-stu-id="adf90-215">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="adf90-217">Se crea el proyecto de inicio siguiente:</span><span class="sxs-lookup"><span data-stu-id="adf90-217">The following starter project is created:</span></span>

  ![Explorador de soluciones](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="adf90-219">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="adf90-219">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="adf90-220">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="adf90-220">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="adf90-221">Cambie al directorio (`cd`) que contiene el proyecto.</span><span class="sxs-lookup"><span data-stu-id="adf90-221">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="adf90-222">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="adf90-222">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="adf90-223">El comando `dotnet new` crea un proyecto de Razor Pages en la carpeta *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="adf90-223">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="adf90-224">El comando `code` abre la carpeta *RazorPagesMovie* en la instancia actual de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="adf90-224">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="adf90-225">Cuando el icono de llama de OmniSharp de la barra de estado se ponga verde, aparecerá un cuadro de diálogo que pregunta **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="adf90-225">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="adf90-226">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="adf90-226">Select **Yes**.</span></span>

  <span data-ttu-id="adf90-227">Un directorio *.vscode*, que contiene archivos *launch.json* y *tasks.json*, se agrega al directorio raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="adf90-227">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="adf90-228">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="adf90-228">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="adf90-229">Desde un terminal, ejecute este comando:</span><span class="sxs-lookup"><span data-stu-id="adf90-229">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```dotnetcli
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="adf90-230">Los comandos anteriores utilizan la [CLI de .NET Core](/dotnet/core/tools/dotnet) para crear un proyecto de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="adf90-230">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="adf90-231">Abrir el proyecto</span><span class="sxs-lookup"><span data-stu-id="adf90-231">Open the project</span></span>

<span data-ttu-id="adf90-232">En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="adf90-232">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="adf90-233">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="adf90-233">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="adf90-234">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="adf90-234">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="adf90-235">Presione Ctrl+F5 para ejecutarla sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="adf90-235">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="adf90-236">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="adf90-236">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="adf90-237">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="adf90-237">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="adf90-238">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="adf90-238">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="adf90-239">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="adf90-239">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="adf90-240">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="adf90-240">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="adf90-241">En la página principal de la aplicación, seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="adf90-241">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="adf90-242">Esta aplicación no realiza un seguimiento de la información personal, pero la plantilla del proyecto incluye la función de consentimiento en caso de que sea necesaria para cumplir con el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr) de la Unión Europea.</span><span class="sxs-lookup"><span data-stu-id="adf90-242">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="adf90-244">En la siguiente imagen se muestra la aplicación tras haber dado su consentimiento al seguimiento:</span><span class="sxs-lookup"><span data-stu-id="adf90-244">The following image shows the app after you give consent to tracking:</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="adf90-246">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="adf90-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="adf90-247">Presione **Ctrl+F5** para ejecutar sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="adf90-247">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="adf90-248">Visual Studio Code inicia [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplaza hasta `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="adf90-248">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="adf90-249">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="adf90-249">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="adf90-250">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="adf90-250">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="adf90-251">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="adf90-251">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="adf90-252">En la página principal de la aplicación, seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="adf90-252">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="adf90-253">Esta aplicación no realiza un seguimiento de la información personal, pero la plantilla del proyecto incluye la función de consentimiento en caso de que sea necesaria para cumplir con el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr) de la Unión Europea.</span><span class="sxs-lookup"><span data-stu-id="adf90-253">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="adf90-255">En la siguiente imagen se muestra la aplicación tras haber dado su consentimiento al seguimiento:</span><span class="sxs-lookup"><span data-stu-id="adf90-255">The following image shows the app after you give consent to tracking:</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="adf90-257">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="adf90-257">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="adf90-258">Presione **Cmd-Opt-F5** para realizar la ejecución sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="adf90-258">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="adf90-259">Visual Studio iniciará [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplazará a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="adf90-259">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="adf90-260">En la página principal de la aplicación, seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="adf90-260">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="adf90-261">Esta aplicación no realiza un seguimiento de la información personal, pero la plantilla del proyecto incluye la función de consentimiento en caso de que sea necesaria para cumplir con el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr) de la Unión Europea.</span><span class="sxs-lookup"><span data-stu-id="adf90-261">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="adf90-263">En la siguiente imagen se muestra la aplicación tras haber dado su consentimiento al seguimiento:</span><span class="sxs-lookup"><span data-stu-id="adf90-263">The following image shows the app after you give consent to tracking:</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="adf90-265">Examen de los archivo del proyecto</span><span class="sxs-lookup"><span data-stu-id="adf90-265">Examine the project files</span></span>

<span data-ttu-id="adf90-266">He aquí un resumen de las principales carpetas y archivos del proyecto con los que va a trabajar en los próximos tutoriales.</span><span class="sxs-lookup"><span data-stu-id="adf90-266">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="adf90-267">Carpeta Pages</span><span class="sxs-lookup"><span data-stu-id="adf90-267">Pages folder</span></span>

<span data-ttu-id="adf90-268">Contiene Razor Pages y los archivos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="adf90-268">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="adf90-269">Cada página de Razor se compone de un par de archivos:</span><span class="sxs-lookup"><span data-stu-id="adf90-269">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="adf90-270">Archivo *.cshtml* que contiene el marcado HTML con código C# que usa la sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="adf90-270">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="adf90-271">Archivo *. cshtml.cs* que contiene C# código que controla los eventos de página.</span><span class="sxs-lookup"><span data-stu-id="adf90-271">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="adf90-272">Los archivos auxiliares tienen nombres que comienzan con un carácter de subrayado.</span><span class="sxs-lookup"><span data-stu-id="adf90-272">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="adf90-273">Por ejemplo, el archivo *_Layout.cshtml* configura los elementos de la interfaz de usuario comunes a todas las páginas.</span><span class="sxs-lookup"><span data-stu-id="adf90-273">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="adf90-274">Este archivo configura el menú de navegación de la parte superior de la página y el aviso de copyright de la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="adf90-274">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="adf90-275">Para más información, consulte <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="adf90-275">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="adf90-276">Carpeta wwwroot</span><span class="sxs-lookup"><span data-stu-id="adf90-276">wwwroot folder</span></span>

<span data-ttu-id="adf90-277">Contiene los archivos estáticos, como los archivos HTML, los archivos de JavaScript y los archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="adf90-277">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="adf90-278">Para más información, consulte <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="adf90-278">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="adf90-279">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="adf90-279">appSettings.json</span></span>

<span data-ttu-id="adf90-280">Contiene los datos de configuración, como las cadenas de conexión.</span><span class="sxs-lookup"><span data-stu-id="adf90-280">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="adf90-281">Para más información, consulte <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="adf90-281">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="adf90-282">Program.cs</span><span class="sxs-lookup"><span data-stu-id="adf90-282">Program.cs</span></span>

<span data-ttu-id="adf90-283">Contiene el punto de entrada del programa.</span><span class="sxs-lookup"><span data-stu-id="adf90-283">Contains the entry point for the program.</span></span> <span data-ttu-id="adf90-284">Para más información, consulte <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="adf90-284">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="adf90-285">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="adf90-285">Startup.cs</span></span>

<span data-ttu-id="adf90-286">Contiene código que configura el comportamiento de la aplicación, como, por ejemplo, si se requiere consentimiento para las cookies.</span><span class="sxs-lookup"><span data-stu-id="adf90-286">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="adf90-287">Para más información, consulte <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="adf90-287">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="adf90-288">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="adf90-288">Additional resources</span></span>

* [<span data-ttu-id="adf90-289">Versión en YouTube de este tutorial</span><span class="sxs-lookup"><span data-stu-id="adf90-289">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="adf90-290">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="adf90-290">Next steps</span></span>

<span data-ttu-id="adf90-291">Pase al siguiente tutorial de la serie:</span><span class="sxs-lookup"><span data-stu-id="adf90-291">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="adf90-292">Agregar un modelo</span><span class="sxs-lookup"><span data-stu-id="adf90-292">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
