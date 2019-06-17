---
title: 'Tutorial: Introducción a Razor Pages en ASP.NET Core'
author: rick-anderson
description: Esta serie de tutoriales muestra cómo usar Razor Pages en ASP.NET Core. Obtenga información sobre cómo crear un modelo, generar código para Razor Pages, usar Entity Framework Core y SQL Server para el acceso a datos, agregar la funcionalidad de búsqueda, agregar validación de entrada y usar migraciones para actualizar el modelo.
ms.author: riande
ms.date: 6/3/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: e7d0312bd4b54586f4a3d403f464ded1aa49bcac
ms.sourcegitcommit: 9691b742134563b662948b0ed63f54ef7186801e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/10/2019
ms.locfileid: "66824704"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="1e356-104">Tutorial: Introducción a Razor Pages en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1e356-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="1e356-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1e356-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1e356-106">Este es el primer tutorial de una serie.</span><span class="sxs-lookup"><span data-stu-id="1e356-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="1e356-107">En la [serie](xref:tutorials/razor-pages/index) se enseñan los conceptos básicos de la compilación de una aplicación web de Razor Pages en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1e356-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="1e356-108">Al final de la serie, tendrá una aplicación que puede administrar una base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="1e356-108">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="1e356-109">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="1e356-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1e356-110">Crear una aplicación web de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="1e356-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="1e356-111">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1e356-111">Run the app.</span></span>
> * <span data-ttu-id="1e356-112">Examinar los archivos de proyecto.</span><span class="sxs-lookup"><span data-stu-id="1e356-112">Examine the project files.</span></span>

<span data-ttu-id="1e356-113">Al final de este tutorial, tendrá una aplicación web de Razor Pages que compilará en los tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="1e356-113">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="1e356-115">Creación de una aplicación web de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="1e356-115">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1e356-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1e356-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1e356-117">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="1e356-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="1e356-118">Cree una nueva aplicación web de ASP.NET Core y seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="1e356-118">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="1e356-120">Asigne al proyecto el nombre **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="1e356-120">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="1e356-121">Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="1e356-121">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="1e356-123">Seleccione **ASP.NET Core 2.2** en la lista desplegable, después **Aplicación web** y, por último, **Crear**.</span><span class="sxs-lookup"><span data-stu-id="1e356-123">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="1e356-125">Se crea el proyecto de inicio siguiente:</span><span class="sxs-lookup"><span data-stu-id="1e356-125">The following starter project is created:</span></span>

  ![Explorador de soluciones](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1e356-127">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1e356-127">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="1e356-128">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="1e356-128">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="1e356-129">Cambie al directorio (`cd`) que contiene el proyecto.</span><span class="sxs-lookup"><span data-stu-id="1e356-129">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="1e356-130">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="1e356-130">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="1e356-131">El comando `dotnet new` crea un proyecto de Razor Pages en la carpeta *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="1e356-131">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="1e356-132">El comando `code` abre la carpeta *RazorPagesMovie* en la instancia actual de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="1e356-132">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="1e356-133">Cuando el icono de llama de OmniSharp de la barra de estado se ponga verde, aparecerá un cuadro de diálogo que pregunta **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="1e356-133">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="1e356-134">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="1e356-134">Select **Yes**.</span></span>

  <span data-ttu-id="1e356-135">Un directorio *.vscode*, que contiene archivos *launch.json* y *tasks.json*, se agrega al directorio raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="1e356-135">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1e356-136">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="1e356-136">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="1e356-137">Desde un terminal, ejecute este comando:</span><span class="sxs-lookup"><span data-stu-id="1e356-137">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="1e356-138">Los comandos anteriores utilizan la [CLI de .NET Core](/dotnet/core/tools/dotnet) para crear un proyecto de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="1e356-138">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="1e356-139">Abrir el proyecto</span><span class="sxs-lookup"><span data-stu-id="1e356-139">Open the project</span></span>

<span data-ttu-id="1e356-140">En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="1e356-140">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="1e356-141">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="1e356-141">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1e356-142">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1e356-142">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1e356-143">Presione Ctrl+F5 para ejecutarla sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="1e356-143">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="1e356-144">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1e356-144">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="1e356-145">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="1e356-145">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="1e356-146">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="1e356-146">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="1e356-147">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="1e356-147">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="1e356-148">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="1e356-148">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="1e356-149">En la página principal de la aplicación, seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="1e356-149">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="1e356-150">Esta aplicación no realiza un seguimiento de la información personal, pero la plantilla del proyecto incluye la función de consentimiento en caso de que sea necesaria para cumplir con el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr) de la Unión Europea.</span><span class="sxs-lookup"><span data-stu-id="1e356-150">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="1e356-152">En la siguiente imagen se muestra la aplicación tras haber dado su consentimiento al seguimiento:</span><span class="sxs-lookup"><span data-stu-id="1e356-152">The following image shows the app after you give consent to tracking:</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1e356-154">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1e356-154">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="1e356-155">Presione **Ctrl+F5** para ejecutar sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="1e356-155">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="1e356-156">Visual Studio Code inicia [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplaza hasta `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="1e356-156">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="1e356-157">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="1e356-157">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="1e356-158">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="1e356-158">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="1e356-159">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="1e356-159">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="1e356-160">En la página principal de la aplicación, seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="1e356-160">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="1e356-161">Esta aplicación no realiza un seguimiento de la información personal, pero la plantilla del proyecto incluye la función de consentimiento en caso de que sea necesaria para cumplir con el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr) de la Unión Europea.</span><span class="sxs-lookup"><span data-stu-id="1e356-161">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="1e356-163">En la siguiente imagen se muestra la aplicación tras haber dado su consentimiento al seguimiento:</span><span class="sxs-lookup"><span data-stu-id="1e356-163">The following image shows the app after you give consent to tracking:</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1e356-165">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="1e356-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="1e356-166">Presione **Cmd-Opt-F5** para realizar la ejecución sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="1e356-166">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="1e356-167">Visual Studio iniciará [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplazará a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="1e356-167">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="1e356-168">En la página principal de la aplicación, seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="1e356-168">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="1e356-169">Esta aplicación no realiza un seguimiento de la información personal, pero la plantilla del proyecto incluye la función de consentimiento en caso de que sea necesaria para cumplir con el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr) de la Unión Europea.</span><span class="sxs-lookup"><span data-stu-id="1e356-169">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="1e356-171">En la siguiente imagen se muestra la aplicación tras haber dado su consentimiento al seguimiento:</span><span class="sxs-lookup"><span data-stu-id="1e356-171">The following image shows the app after you give consent to tracking:</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="1e356-173">Examen de los archivo del proyecto</span><span class="sxs-lookup"><span data-stu-id="1e356-173">Examine the project files</span></span>

<span data-ttu-id="1e356-174">He aquí un resumen de las principales carpetas y archivos del proyecto con los que va a trabajar en los próximos tutoriales.</span><span class="sxs-lookup"><span data-stu-id="1e356-174">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="1e356-175">Carpeta Pages</span><span class="sxs-lookup"><span data-stu-id="1e356-175">Pages folder</span></span>

<span data-ttu-id="1e356-176">Contiene Razor Pages y los archivos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="1e356-176">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="1e356-177">Cada página de Razor se compone de un par de archivos:</span><span class="sxs-lookup"><span data-stu-id="1e356-177">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="1e356-178">Archivo *.cshtml* que contiene el marcado HTML con código C# que usa la sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="1e356-178">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="1e356-179">Archivo *. cshtml.cs* que contiene C# código que controla los eventos de página.</span><span class="sxs-lookup"><span data-stu-id="1e356-179">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="1e356-180">Los archivos auxiliares tienen nombres que comienzan con un carácter de subrayado.</span><span class="sxs-lookup"><span data-stu-id="1e356-180">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="1e356-181">Por ejemplo, el archivo *_Layout.cshtml* configura los elementos de la interfaz de usuario comunes a todas las páginas.</span><span class="sxs-lookup"><span data-stu-id="1e356-181">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="1e356-182">Este archivo configura el menú de navegación de la parte superior de la página y el aviso de copyright de la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="1e356-182">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="1e356-183">Para obtener más información, vea <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="1e356-183">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="1e356-184">Carpeta wwwroot</span><span class="sxs-lookup"><span data-stu-id="1e356-184">wwwroot folder</span></span>

<span data-ttu-id="1e356-185">Contiene los archivos estáticos, como los archivos HTML, los archivos de JavaScript y los archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="1e356-185">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="1e356-186">Para obtener más información, vea <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="1e356-186">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="1e356-187">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="1e356-187">appSettings.json</span></span>

<span data-ttu-id="1e356-188">Contiene los datos de configuración, como las cadenas de conexión.</span><span class="sxs-lookup"><span data-stu-id="1e356-188">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="1e356-189">Para obtener más información, vea <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="1e356-189">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="1e356-190">Program.cs</span><span class="sxs-lookup"><span data-stu-id="1e356-190">Program.cs</span></span>

<span data-ttu-id="1e356-191">Contiene el punto de entrada del programa.</span><span class="sxs-lookup"><span data-stu-id="1e356-191">Contains the entry point for the program.</span></span> <span data-ttu-id="1e356-192">Para obtener más información, vea <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="1e356-192">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="1e356-193">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="1e356-193">Startup.cs</span></span>

<span data-ttu-id="1e356-194">Contiene código que configura el comportamiento de la aplicación, como, por ejemplo, si se requiere consentimiento para las cookies.</span><span class="sxs-lookup"><span data-stu-id="1e356-194">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="1e356-195">Para obtener más información, vea <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="1e356-195">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1e356-196">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="1e356-196">Additional resources</span></span>

* [<span data-ttu-id="1e356-197">Versión en YouTube de este tutorial</span><span class="sxs-lookup"><span data-stu-id="1e356-197">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="1e356-198">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="1e356-198">Next steps</span></span>

<span data-ttu-id="1e356-199">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="1e356-199">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1e356-200">Creado una aplicación web de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="1e356-200">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="1e356-201">Ejecutado la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1e356-201">Ran the app.</span></span>
> * <span data-ttu-id="1e356-202">Examinado los archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="1e356-202">Examined the project files.</span></span>

<span data-ttu-id="1e356-203">Pase al siguiente tutorial de la serie:</span><span class="sxs-lookup"><span data-stu-id="1e356-203">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="1e356-204">Agregar un modelo</span><span class="sxs-lookup"><span data-stu-id="1e356-204">Add a model</span></span>](xref:tutorials/razor-pages/model)
