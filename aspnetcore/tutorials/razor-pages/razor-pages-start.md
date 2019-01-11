---
title: 'Tutorial: Introducción a las páginas de Razor en ASP.NET Core'
author: rick-anderson
description: Esta serie de tutoriales muestra cómo usar Razor Pages en ASP.NET Core. Obtenga información sobre cómo crear un modelo, generar código para Razor Pages, usar Entity Framework Core y SQL Server para el acceso a datos, agregar la funcionalidad de búsqueda, agregar validación de entrada y usar migraciones para actualizar el modelo.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 929bc72b16e302a5018038bc449704b7078dd33a
ms.sourcegitcommit: 6548c19f345850ee22b50f7ef9fca732895d9e08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53425086"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="41d62-104">Tutorial: Introducción a las páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="41d62-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="41d62-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="41d62-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="41d62-106">Este es el primer tutorial de una serie.</span><span class="sxs-lookup"><span data-stu-id="41d62-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="41d62-107">En la [serie](xref:tutorials/razor-pages/index) se enseñan los conceptos básicos de la compilación de una aplicación web de Razor Pages en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="41d62-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="41d62-108">Al final de la serie, tendrá una aplicación que puede administrar una base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="41d62-108">At the end of the series you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="41d62-109">En este tutorial va a:</span><span class="sxs-lookup"><span data-stu-id="41d62-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="41d62-110">Crear una aplicación web de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="41d62-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="41d62-111">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="41d62-111">Run the app.</span></span>
> * <span data-ttu-id="41d62-112">Examinar los archivos de proyecto.</span><span class="sxs-lookup"><span data-stu-id="41d62-112">Examine the project files.</span></span>

<span data-ttu-id="41d62-113">Al final de este tutorial, tendrá una aplicación web de Razor Pages que compilará en los tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="41d62-113">At the end of this tutorial you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

[<span data-ttu-id="41d62-114">Página Inicio o Índice</span><span class="sxs-lookup"><span data-stu-id="41d62-114">Home or Index page</span></span>](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="41d62-115">Creación de una aplicación web de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="41d62-115">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="41d62-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="41d62-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="41d62-117">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="41d62-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="41d62-118">Cree una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="41d62-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="41d62-119">Asigne al proyecto el nombre **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="41d62-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="41d62-120">Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="41d62-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="41d62-122">Seleccione **ASP.NET Core 2.2** en la lista desplegable y, luego, **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="41d62-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="41d62-124">Se crea el proyecto de inicio siguiente:</span><span class="sxs-lookup"><span data-stu-id="41d62-124">The following starter project is created:</span></span>

  ![Explorador de soluciones](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="41d62-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="41d62-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="41d62-127">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="41d62-127">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="41d62-128">Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.</span><span class="sxs-lookup"><span data-stu-id="41d62-128">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="41d62-129">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="41d62-129">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="41d62-130">El comando `dotnet new` crea un proyecto de Razor Pages en la carpeta *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="41d62-130">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="41d62-131">El comando `code` abre la carpeta *RazorPagesMovie* en una nueva instancia de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="41d62-131">The `code` command opens the *RazorPagesMovie* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="41d62-132">Se muestra un cuadro de diálogo con el texto **Required assets to build and debug are missing from "RazorPagesMovie". Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="41d62-132">A dialog box appears with **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span>

* <span data-ttu-id="41d62-133">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="41d62-133">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="41d62-134">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="41d62-134">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="41d62-135">Desde un terminal, ejecute estos comandos:</span><span class="sxs-lookup"><span data-stu-id="41d62-135">From a terminal, run the following commands:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
```

<span data-ttu-id="41d62-136">Los comandos anteriores utilizan la [CLI de .NET Core](/dotnet/core/tools/dotnet) para crear un proyecto de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="41d62-136">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="41d62-137">Abrir el proyecto</span><span class="sxs-lookup"><span data-stu-id="41d62-137">Open the project</span></span>

<span data-ttu-id="41d62-138">En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="41d62-138">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-web-app"></a><span data-ttu-id="41d62-139">Ejecutar la aplicación web</span><span class="sxs-lookup"><span data-stu-id="41d62-139">Run the web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="41d62-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="41d62-140">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="41d62-141">Presione Ctrl+F5 para ejecutarla sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="41d62-141">Press Ctrl+F5 to run without the debugger.</span></span>

  <span data-ttu-id="41d62-142">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="41d62-142">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="41d62-143">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="41d62-143">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="41d62-144">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="41d62-144">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="41d62-145">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="41d62-145">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="41d62-146">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="41d62-146">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="41d62-147">En la imagen anterior, el número de puerto es 5001.</span><span class="sxs-lookup"><span data-stu-id="41d62-147">In the preceding image, the port number is 5001.</span></span> <span data-ttu-id="41d62-148">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="41d62-148">When you run the app, you'll see a different port number.</span></span>
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="41d62-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="41d62-149">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="41d62-150">Presione **Ctrl+F5** para ejecutar sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="41d62-150">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="41d62-151">Visual Studio Code inicia [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplaza hasta `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="41d62-151">Visual Studio Code starts starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="41d62-152">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="41d62-152">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="41d62-153">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="41d62-153">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="41d62-154">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="41d62-154">Localhost only serves web requests from the local computer.</span></span>
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="41d62-155">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="41d62-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="41d62-156">Seleccione **Ejecutar > Iniciar sin depurar** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="41d62-156">Select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="41d62-157">Visual Studio iniciará [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplazará a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="41d62-157">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

* <span data-ttu-id="41d62-158">En la página principal de la aplicación, seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="41d62-158">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="41d62-159">Esta aplicación no realiza un seguimiento de la información personal, pero la plantilla del proyecto incluye la función de consentimiento en caso de que sea necesaria para cumplir con el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr) de la Unión Europea.</span><span class="sxs-lookup"><span data-stu-id="41d62-159">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="41d62-161">En la siguiente imagen se muestra la aplicación tras haber dado su consentimiento al seguimiento:</span><span class="sxs-lookup"><span data-stu-id="41d62-161">The following image shows the app after you give consent to tracking:</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)

## <a name="examine-the-project-files"></a><span data-ttu-id="41d62-163">Examen de los archivo del proyecto</span><span class="sxs-lookup"><span data-stu-id="41d62-163">Examine the project files</span></span>

<span data-ttu-id="41d62-164">He aquí un resumen de las principales carpetas y archivos del proyecto con los que va a trabajar en los próximos tutoriales.</span><span class="sxs-lookup"><span data-stu-id="41d62-164">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="41d62-165">Carpeta Pages</span><span class="sxs-lookup"><span data-stu-id="41d62-165">Pages folder</span></span>

<span data-ttu-id="41d62-166">Contiene Razor Pages y los archivos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="41d62-166">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="41d62-167">Cada página de Razor se compone de un par de archivos:</span><span class="sxs-lookup"><span data-stu-id="41d62-167">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="41d62-168">Archivo *.cshtml* que contiene el marcado HTML con código C# que usa la sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="41d62-168">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="41d62-169">Archivo *. cshtml.cs* que contiene C# código que controla los eventos de página.</span><span class="sxs-lookup"><span data-stu-id="41d62-169">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="41d62-170">Los archivos auxiliares tienen nombres que comienzan con un carácter de subrayado.</span><span class="sxs-lookup"><span data-stu-id="41d62-170">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="41d62-171">Por ejemplo, el archivo *_Layout.cshtml* configura los elementos de la interfaz de usuario comunes a todas las páginas.</span><span class="sxs-lookup"><span data-stu-id="41d62-171">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="41d62-172">Este archivo configura el menú de navegación de la parte superior de la página y el aviso de copyright de la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="41d62-172">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="41d62-173">Para obtener más información, vea <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="41d62-173">For more information, see <xref:mvc/views/layout>.</span></span>


### <a name="wwwroot-folder"></a><span data-ttu-id="41d62-174">Carpeta wwwroot</span><span class="sxs-lookup"><span data-stu-id="41d62-174">wwwroot folder</span></span>

<span data-ttu-id="41d62-175">Contiene los archivos estáticos, como los archivos HTML, los archivos de JavaScript y los archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="41d62-175">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="41d62-176">Para obtener más información, vea <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="41d62-176">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="41d62-177">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="41d62-177">appSettings.json</span></span>

<span data-ttu-id="41d62-178">Contiene los datos de configuración, como las cadenas de conexión.</span><span class="sxs-lookup"><span data-stu-id="41d62-178">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="41d62-179">Para obtener más información, vea <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="41d62-179">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="41d62-180">Program.cs</span><span class="sxs-lookup"><span data-stu-id="41d62-180">Program.cs</span></span>

<span data-ttu-id="41d62-181">Contiene el punto de entrada del programa.</span><span class="sxs-lookup"><span data-stu-id="41d62-181">Contains the entry point for the program.</span></span> <span data-ttu-id="41d62-182">Para obtener más información, vea <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="41d62-182">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="41d62-183">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="41d62-183">Startup.cs</span></span>

<span data-ttu-id="41d62-184">Contiene código que configura el comportamiento de la aplicación, como, por ejemplo, si se requiere consentimiento para las cookies.</span><span class="sxs-lookup"><span data-stu-id="41d62-184">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="41d62-185">Para obtener más información, vea <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="41d62-185">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="41d62-186">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="41d62-186">Next steps</span></span>

<span data-ttu-id="41d62-187">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="41d62-187">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="41d62-188">Creado una aplicación web de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="41d62-188">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="41d62-189">Ejecutado la aplicación.</span><span class="sxs-lookup"><span data-stu-id="41d62-189">Ran the app.</span></span>
> * <span data-ttu-id="41d62-190">Examinado los archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="41d62-190">Examined the project files.</span></span>

<span data-ttu-id="41d62-191">Pase al siguiente tutorial de la serie:</span><span class="sxs-lookup"><span data-stu-id="41d62-191">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="41d62-192">Agregar un modelo</span><span class="sxs-lookup"><span data-stu-id="41d62-192">Add a model</span></span>](xref:tutorials/razor-pages/model)
