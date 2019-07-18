---
title: 'Tutorial: Introducción a Razor Pages en ASP.NET Core'
author: rick-anderson
description: Esta serie de tutoriales muestra cómo usar Razor Pages en ASP.NET Core. Obtenga información sobre cómo crear un modelo, generar código para Razor Pages, usar Entity Framework Core y SQL Server para el acceso a datos, agregar la funcionalidad de búsqueda, agregar validación de entrada y usar migraciones para actualizar el modelo.
ms.author: riande
ms.date: 06/03/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 7e228c99b4d55c14cea9c915cf06a7fbbbd5af44
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2019
ms.locfileid: "67855736"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="881c0-104">Tutorial: Introducción a Razor Pages en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="881c0-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="881c0-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="881c0-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="881c0-106">Este es el primer tutorial de una serie.</span><span class="sxs-lookup"><span data-stu-id="881c0-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="881c0-107">En la [serie](xref:tutorials/razor-pages/index) se enseñan los conceptos básicos de la compilación de una aplicación web de Razor Pages en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="881c0-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="881c0-108">Al final de la serie, tendrá una aplicación que puede administrar una base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="881c0-108">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="881c0-109">En este tutorial, hizo lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="881c0-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="881c0-110">Crear una aplicación web de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="881c0-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="881c0-111">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="881c0-111">Run the app.</span></span>
> * <span data-ttu-id="881c0-112">Examinar los archivos de proyecto.</span><span class="sxs-lookup"><span data-stu-id="881c0-112">Examine the project files.</span></span>

<span data-ttu-id="881c0-113">Al final de este tutorial, tendrá una aplicación web de Razor Pages que compilará en los tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="881c0-113">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="881c0-115">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="881c0-115">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="881c0-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="881c0-116">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="881c0-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="881c0-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="881c0-118">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="881c0-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="881c0-119">Creación de una aplicación web de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="881c0-119">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="881c0-120">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="881c0-120">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="881c0-121">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="881c0-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="881c0-122">Cree una nueva aplicación web de ASP.NET Core y seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="881c0-122">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="881c0-124">Asigne al proyecto el nombre **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="881c0-124">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="881c0-125">Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="881c0-125">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="881c0-127">Seleccione **ASP.NET Core 2.2** en la lista desplegable, después **Aplicación web** y, por último, **Crear**.</span><span class="sxs-lookup"><span data-stu-id="881c0-127">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="881c0-129">Se crea el proyecto de inicio siguiente:</span><span class="sxs-lookup"><span data-stu-id="881c0-129">The following starter project is created:</span></span>

  ![Explorador de soluciones](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="881c0-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="881c0-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="881c0-132">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="881c0-132">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="881c0-133">Cambie al directorio (`cd`) que contiene el proyecto.</span><span class="sxs-lookup"><span data-stu-id="881c0-133">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="881c0-134">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="881c0-134">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="881c0-135">El comando `dotnet new` crea un proyecto de Razor Pages en la carpeta *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="881c0-135">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="881c0-136">El comando `code` abre la carpeta *RazorPagesMovie* en la instancia actual de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="881c0-136">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="881c0-137">Cuando el icono de llama de OmniSharp de la barra de estado se ponga verde, aparecerá un cuadro de diálogo que pregunta **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="881c0-137">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="881c0-138">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="881c0-138">Select **Yes**.</span></span>

  <span data-ttu-id="881c0-139">Un directorio *.vscode*, que contiene archivos *launch.json* y *tasks.json*, se agrega al directorio raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="881c0-139">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="881c0-140">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="881c0-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="881c0-141">Desde un terminal, ejecute este comando:</span><span class="sxs-lookup"><span data-stu-id="881c0-141">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="881c0-142">Los comandos anteriores utilizan la [CLI de .NET Core](/dotnet/core/tools/dotnet) para crear un proyecto de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="881c0-142">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="881c0-143">Abrir el proyecto</span><span class="sxs-lookup"><span data-stu-id="881c0-143">Open the project</span></span>

<span data-ttu-id="881c0-144">En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="881c0-144">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="881c0-145">Ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="881c0-145">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="881c0-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="881c0-146">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="881c0-147">Presione Ctrl+F5 para ejecutarla sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="881c0-147">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="881c0-148">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="881c0-148">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="881c0-149">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="881c0-149">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="881c0-150">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="881c0-150">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="881c0-151">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="881c0-151">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="881c0-152">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="881c0-152">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="881c0-153">En la página principal de la aplicación, seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="881c0-153">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="881c0-154">Esta aplicación no realiza un seguimiento de la información personal, pero la plantilla del proyecto incluye la función de consentimiento en caso de que sea necesaria para cumplir con el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr) de la Unión Europea.</span><span class="sxs-lookup"><span data-stu-id="881c0-154">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="881c0-156">En la siguiente imagen se muestra la aplicación tras haber dado su consentimiento al seguimiento:</span><span class="sxs-lookup"><span data-stu-id="881c0-156">The following image shows the app after you give consent to tracking:</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="881c0-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="881c0-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="881c0-159">Presione **Ctrl+F5** para ejecutar sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="881c0-159">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="881c0-160">Visual Studio Code inicia [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplaza hasta `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="881c0-160">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="881c0-161">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="881c0-161">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="881c0-162">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="881c0-162">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="881c0-163">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="881c0-163">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="881c0-164">En la página principal de la aplicación, seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="881c0-164">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="881c0-165">Esta aplicación no realiza un seguimiento de la información personal, pero la plantilla del proyecto incluye la función de consentimiento en caso de que sea necesaria para cumplir con el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr) de la Unión Europea.</span><span class="sxs-lookup"><span data-stu-id="881c0-165">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="881c0-167">En la siguiente imagen se muestra la aplicación tras haber dado su consentimiento al seguimiento:</span><span class="sxs-lookup"><span data-stu-id="881c0-167">The following image shows the app after you give consent to tracking:</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="881c0-169">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="881c0-169">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="881c0-170">Presione **Cmd-Opt-F5** para realizar la ejecución sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="881c0-170">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="881c0-171">Visual Studio iniciará [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplazará a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="881c0-171">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="881c0-172">En la página principal de la aplicación, seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="881c0-172">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="881c0-173">Esta aplicación no realiza un seguimiento de la información personal, pero la plantilla del proyecto incluye la función de consentimiento en caso de que sea necesaria para cumplir con el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr) de la Unión Europea.</span><span class="sxs-lookup"><span data-stu-id="881c0-173">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="881c0-175">En la siguiente imagen se muestra la aplicación tras haber dado su consentimiento al seguimiento:</span><span class="sxs-lookup"><span data-stu-id="881c0-175">The following image shows the app after you give consent to tracking:</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="881c0-177">Examen de los archivo del proyecto</span><span class="sxs-lookup"><span data-stu-id="881c0-177">Examine the project files</span></span>

<span data-ttu-id="881c0-178">He aquí un resumen de las principales carpetas y archivos del proyecto con los que va a trabajar en los próximos tutoriales.</span><span class="sxs-lookup"><span data-stu-id="881c0-178">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="881c0-179">Carpeta Pages</span><span class="sxs-lookup"><span data-stu-id="881c0-179">Pages folder</span></span>

<span data-ttu-id="881c0-180">Contiene Razor Pages y los archivos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="881c0-180">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="881c0-181">Cada página de Razor se compone de un par de archivos:</span><span class="sxs-lookup"><span data-stu-id="881c0-181">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="881c0-182">Archivo *.cshtml* que contiene el marcado HTML con código C# que usa la sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="881c0-182">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="881c0-183">Archivo *. cshtml.cs* que contiene C# código que controla los eventos de página.</span><span class="sxs-lookup"><span data-stu-id="881c0-183">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="881c0-184">Los archivos auxiliares tienen nombres que comienzan con un carácter de subrayado.</span><span class="sxs-lookup"><span data-stu-id="881c0-184">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="881c0-185">Por ejemplo, el archivo *_Layout.cshtml* configura los elementos de la interfaz de usuario comunes a todas las páginas.</span><span class="sxs-lookup"><span data-stu-id="881c0-185">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="881c0-186">Este archivo configura el menú de navegación de la parte superior de la página y el aviso de copyright de la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="881c0-186">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="881c0-187">Para más información, consulte <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="881c0-187">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="881c0-188">Carpeta wwwroot</span><span class="sxs-lookup"><span data-stu-id="881c0-188">wwwroot folder</span></span>

<span data-ttu-id="881c0-189">Contiene los archivos estáticos, como los archivos HTML, los archivos de JavaScript y los archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="881c0-189">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="881c0-190">Para más información, consulte <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="881c0-190">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="881c0-191">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="881c0-191">appSettings.json</span></span>

<span data-ttu-id="881c0-192">Contiene los datos de configuración, como las cadenas de conexión.</span><span class="sxs-lookup"><span data-stu-id="881c0-192">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="881c0-193">Para más información, consulte <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="881c0-193">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="881c0-194">Program.cs</span><span class="sxs-lookup"><span data-stu-id="881c0-194">Program.cs</span></span>

<span data-ttu-id="881c0-195">Contiene el punto de entrada del programa.</span><span class="sxs-lookup"><span data-stu-id="881c0-195">Contains the entry point for the program.</span></span> <span data-ttu-id="881c0-196">Para más información, consulte <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="881c0-196">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="881c0-197">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="881c0-197">Startup.cs</span></span>

<span data-ttu-id="881c0-198">Contiene código que configura el comportamiento de la aplicación, como, por ejemplo, si se requiere consentimiento para las cookies.</span><span class="sxs-lookup"><span data-stu-id="881c0-198">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="881c0-199">Para más información, consulte <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="881c0-199">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="881c0-200">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="881c0-200">Additional resources</span></span>

* [<span data-ttu-id="881c0-201">Versión en YouTube de este tutorial</span><span class="sxs-lookup"><span data-stu-id="881c0-201">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="881c0-202">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="881c0-202">Next steps</span></span>

<span data-ttu-id="881c0-203">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="881c0-203">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="881c0-204">Creado una aplicación web de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="881c0-204">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="881c0-205">Ejecutado la aplicación.</span><span class="sxs-lookup"><span data-stu-id="881c0-205">Ran the app.</span></span>
> * <span data-ttu-id="881c0-206">Examinado los archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="881c0-206">Examined the project files.</span></span>

<span data-ttu-id="881c0-207">Pase al siguiente tutorial de la serie:</span><span class="sxs-lookup"><span data-stu-id="881c0-207">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="881c0-208">Agregar un modelo</span><span class="sxs-lookup"><span data-stu-id="881c0-208">Add a model</span></span>](xref:tutorials/razor-pages/model)
