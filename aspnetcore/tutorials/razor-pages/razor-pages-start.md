---
title: Introducción a las páginas de Razor en ASP.NET Core
author: rick-anderson
monikerRange: '>= aspnetcore-2.2'
description: Esta serie de tutoriales muestra cómo usar Razor Pages en ASP.NET Core. Obtenga información sobre cómo crear un modelo, generar código para Razor Pages, usar Entity Framework Core y SQL Server para el acceso a datos, agregar la funcionalidad de búsqueda, agregar validación de entrada y usar migraciones para actualizar el modelo.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1152ebfcee48a46ecd28c941fce32d3fc1e05c41
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861633"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="f4155-104">Tutorial: Introducción a Razor Pages en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f4155-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="f4155-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f4155-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f4155-106">En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web de páginas de Razor de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f4155-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

<span data-ttu-id="f4155-107">La aplicación administra una base de datos de títulos de películas.</span><span class="sxs-lookup"><span data-stu-id="f4155-107">The app manages a database of movie titles.</span></span> <span data-ttu-id="f4155-108">Aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="f4155-108">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f4155-109">Crear una aplicación web de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="f4155-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="f4155-110">Agregar un modelo y aplicarle scaffolding.</span><span class="sxs-lookup"><span data-stu-id="f4155-110">Add and scaffold a model.</span></span>
> * <span data-ttu-id="f4155-111">Trabajar con una base de datos.</span><span class="sxs-lookup"><span data-stu-id="f4155-111">Work with a database.</span></span>
> * <span data-ttu-id="f4155-112">Agregar búsqueda y validación.</span><span class="sxs-lookup"><span data-stu-id="f4155-112">Add search and validation.</span></span>

<span data-ttu-id="f4155-113">Al final, tendrá una aplicación que le permitirá administrar y mostrar los elementos de los títulos de películas.</span><span class="sxs-lookup"><span data-stu-id="f4155-113">At the end, you have an app that can manage and display movie titles items.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="f4155-114">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="f4155-114">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="f4155-115">Creación de una aplicación web de Razor</span><span class="sxs-lookup"><span data-stu-id="f4155-115">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f4155-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f4155-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f4155-117">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="f4155-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f4155-118">Cree una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f4155-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="f4155-119">Asigne al proyecto el nombre **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="f4155-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="f4155-120">Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="f4155-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="f4155-121">![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="f4155-121">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>

* <span data-ttu-id="f4155-122">Seleccione **ASP.NET Core 2.2** en la lista desplegable y, luego, **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="f4155-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="f4155-124">Se crea el proyecto de inicio siguiente:</span><span class="sxs-lookup"><span data-stu-id="f4155-124">The following starter project is created:</span></span>

  ![Explorador de soluciones](razor-pages-start/_static/se2.2.png)

* <span data-ttu-id="f4155-126">Presione **Ctrl+F5** para ejecutar sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="f4155-126">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="f4155-127">Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f4155-127">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="f4155-128">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="f4155-128">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="f4155-129">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="f4155-129">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="f4155-130">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="f4155-130">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="f4155-131">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="f4155-131">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="f4155-132">En la imagen anterior, el número de puerto es 5001.</span><span class="sxs-lookup"><span data-stu-id="f4155-132">In the preceding image, the port number is 5001.</span></span> <span data-ttu-id="f4155-133">Al ejecutar la aplicación verá otro puerto distinto.</span><span class="sxs-lookup"><span data-stu-id="f4155-133">When you run the app, you'll see a different port number.</span></span>

  <span data-ttu-id="f4155-134">Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="f4155-134">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="f4155-135">Muchos desarrolladores prefieren usar el modo de no depuración para actualizar la página y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="f4155-135">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f4155-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f4155-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f4155-137">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="f4155-137">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="f4155-138">Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.</span><span class="sxs-lookup"><span data-stu-id="f4155-138">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="f4155-139">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="f4155-139">Run the following command:</span></span>

   ```console
   dotnet new webapp -o RazorPagesMovie
   code -r RazorPagesMovie
   ```

  * <span data-ttu-id="f4155-140">Se muestra un cuadro de diálogo con el texto **Required assets to build and debug are missing from "RazorPagesMovie". Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="f4155-140">A dialog box appears with **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span>  <span data-ttu-id="f4155-141">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="f4155-141">Select **Yes**</span></span>

  * <span data-ttu-id="f4155-142">`dotnet new webapp -o RazorPagesMovie`: crea un nuevo proyecto de Razor Pages en la carpeta *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="f4155-142">`dotnet new webapp -o RazorPagesMovie`: creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="f4155-143">`code -r RazorPagesMovie`: carga el archivo del proyecto *RazorPagesMovie.csproj* en Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="f4155-143">`code -r RazorPagesMovie`: Loads the *RazorPagesMovie.csproj* project file in Visual Studio Code.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="f4155-144">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="f4155-144">Launch the app</span></span>

* <span data-ttu-id="f4155-145">Presione **Ctrl+F5** para ejecutar sin el depurador.</span><span class="sxs-lookup"><span data-stu-id="f4155-145">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="f4155-146">Visual Studio Code inicia [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplaza hasta `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="f4155-146">Visual Studio Code starts starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="f4155-147">En la barra de direcciones aparece `localhost:port:5001` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="f4155-147">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="f4155-148">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="f4155-148">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="f4155-149">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="f4155-149">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="f4155-150">Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="f4155-150">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="f4155-151">Muchos desarrolladores prefieren usar el modo de no depuración para actualizar la página y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="f4155-151">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f4155-152">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="f4155-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f4155-153">Desde un terminal, ejecute estos comandos:</span><span class="sxs-lookup"><span data-stu-id="f4155-153">From a terminal, run the following commands:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="f4155-154">Los comandos anteriores usan el [CLI de .NET Core](/dotnet/core/tools/dotnet) para crear y ejecutar un proyecto de páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="f4155-154">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="f4155-155">Abra http://localhost:5000 en un explorador para ver la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f4155-155">Open a browser to http://localhost:5000 to view the application.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="f4155-156">Abrir el proyecto</span><span class="sxs-lookup"><span data-stu-id="f4155-156">Open the project</span></span>

<span data-ttu-id="f4155-157">Presione CTRL+C para cerrar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f4155-157">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="f4155-158">En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="f4155-158">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="f4155-159">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="f4155-159">Launch the app</span></span>

<span data-ttu-id="f4155-160">Seleccione **Ejecutar > Iniciar sin depurar** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f4155-160">Select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="f4155-161">Visual Studio iniciará [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplazará a `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="f4155-161">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

* <span data-ttu-id="f4155-162">Seleccione **Aceptar** para dar su consentimiento al seguimiento.</span><span class="sxs-lookup"><span data-stu-id="f4155-162">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="f4155-163">Esta aplicación no lleva un seguimiento de la información personal.</span><span class="sxs-lookup"><span data-stu-id="f4155-163">This app doesn't track personal information.</span></span> <span data-ttu-id="f4155-164">El código generado con plantilla incluye activos que sirven para cumplir el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="f4155-164">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="f4155-166">En la siguiente imagen se muestra la aplicación tras haber aceptado el seguimiento:</span><span class="sxs-lookup"><span data-stu-id="f4155-166">The following image shows the app after accepting tracking:</span></span>

  ![Página Inicio o Índice](razor-pages-start/_static/home2.2.png)

## <a name="project-files-and-folders"></a><span data-ttu-id="f4155-168">Archivos y carpetas del proyecto</span><span class="sxs-lookup"><span data-stu-id="f4155-168">Project files and folders</span></span>

<span data-ttu-id="f4155-169">En la tabla siguiente se enumeran los archivos y las carpetas del proyecto.</span><span class="sxs-lookup"><span data-stu-id="f4155-169">The following table lists the files and folders in the project.</span></span> <span data-ttu-id="f4155-170">En este punto del tutorial, el archivo *Startup.cs* es el más importante.</span><span class="sxs-lookup"><span data-stu-id="f4155-170">At this point in the tutorial, the *Startup.cs* file is the most important to understand.</span></span> <span data-ttu-id="f4155-171">No es necesario revisar todos los vínculos siguientes.</span><span class="sxs-lookup"><span data-stu-id="f4155-171">You don't need to review each link provided below.</span></span> <span data-ttu-id="f4155-172">Los vínculos se proporcionan como referencia para cuando necesite más información sobre un archivo o una carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="f4155-172">The links are provided as a reference when you need more information on a file or folder in the project.</span></span>

| <span data-ttu-id="f4155-173">Archivo o carpeta</span><span class="sxs-lookup"><span data-stu-id="f4155-173">File or folder</span></span>              | <span data-ttu-id="f4155-174">Propósito</span><span class="sxs-lookup"><span data-stu-id="f4155-174">Purpose</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="f4155-175">*wwwroot*</span><span class="sxs-lookup"><span data-stu-id="f4155-175">*wwwroot*</span></span> | <span data-ttu-id="f4155-176">Contiene archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="f4155-176">Contains static files.</span></span> <span data-ttu-id="f4155-177">Vea [Archivos estáticos](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="f4155-177">See [Static files](xref:fundamentals/static-files).</span></span> |
| <span data-ttu-id="f4155-178">*Páginas*</span><span class="sxs-lookup"><span data-stu-id="f4155-178">*Pages*</span></span> | <span data-ttu-id="f4155-179">Carpeta para [páginas de Razor](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="f4155-179">Folder for [Razor Pages](xref:razor-pages/index).</span></span> |
| <span data-ttu-id="f4155-180">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="f4155-180">*appsettings.json*</span></span> | [<span data-ttu-id="f4155-181">Configuración</span><span class="sxs-lookup"><span data-stu-id="f4155-181">Configuration</span></span>](xref:fundamentals/configuration/index) |
| <span data-ttu-id="f4155-182">*Program.cs*</span><span class="sxs-lookup"><span data-stu-id="f4155-182">*Program.cs*</span></span> | <span data-ttu-id="f4155-183">[Aloja](xref:fundamentals/host/index) la aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f4155-183">[Hosts](xref:fundamentals/host/index) the ASP.NET Core app.</span></span>|
| <span data-ttu-id="f4155-184">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="f4155-184">*Startup.cs*</span></span> | <span data-ttu-id="f4155-185">Configura los servicios y la canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="f4155-185">Configures services and the request pipeline.</span></span> <span data-ttu-id="f4155-186">Consulte [Inicio](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="f4155-186">See [Startup](xref:fundamentals/startup).</span></span>|

### <a name="the-pages-folder"></a><span data-ttu-id="f4155-187">Carpeta Páginas</span><span class="sxs-lookup"><span data-stu-id="f4155-187">The Pages folder</span></span>

<span data-ttu-id="f4155-188">El archivo *_Layout.cshtml* contiene elementos HTML comunes (scripts y hojas de estilos) y establece el diseño de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f4155-188">The *_Layout.cshtml* file contains common HTML elements (scripts and stylesheets) and sets the layout for the application.</span></span> <span data-ttu-id="f4155-189">Por ejemplo, al hacer clic en **RazorPagesMovie**, **Inicio** o **Privacidad**, verá los mismos elementos.</span><span class="sxs-lookup"><span data-stu-id="f4155-189">For example, when you click on **RazorPagesMovie**, **Home**, or **Privacy**, you see the same elements.</span></span> <span data-ttu-id="f4155-190">Los elementos comunes incluyen el menú de navegación de la parte superior y el encabezado de la parte inferior de la ventana.</span><span class="sxs-lookup"><span data-stu-id="f4155-190">The common elements include the navigation menu on the top and the header on the bottom of the window.</span></span> <span data-ttu-id="f4155-191">Vea [Layout](xref:mvc/views/layout) (Diseño) para más información.</span><span class="sxs-lookup"><span data-stu-id="f4155-191">See [Layout](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="f4155-192">El archivo *_ViewImports.cshtml* contiene directivas de Razor que se importan en cada página de Razor.</span><span class="sxs-lookup"><span data-stu-id="f4155-192">The *_ViewImports.cshtml* file contains Razor directives that are imported into each Razor Page.</span></span> <span data-ttu-id="f4155-193">Consulte [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives) (Importar directivas compartidas) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="f4155-193">See [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives) for more information.</span></span>

<span data-ttu-id="f4155-194">El archivo *_ViewStart.cshtml* establece la propiedad `Layout` de las páginas de Razor que para usar el archivo *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f4155-194">The *_ViewStart.cshtml* sets the Razor Pages `Layout` property to use the *_Layout.cshtml* file.</span></span> <span data-ttu-id="f4155-195">Vea [Layout](xref:mvc/views/layout) (Diseño) para más información.</span><span class="sxs-lookup"><span data-stu-id="f4155-195">See [Layout](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="f4155-196">El archivo *_ValidationScriptsPartial.cshtml* proporciona una referencia de los scripts de validación de [jQuery](https://jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="f4155-196">The *_ValidationScriptsPartial.cshtml* file provides a reference to [jQuery](https://jquery.com/) validation scripts.</span></span> <span data-ttu-id="f4155-197">Cuando las páginas `Create` y `Edit` se agreguen más adelante en el tutorial, se usará el archivo *_ValidationScriptsPartial.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f4155-197">When the `Create` and `Edit` pages are added later in the tutorial, the *_ValidationScriptsPartial.cshtml* file will be used.</span></span>

<span data-ttu-id="f4155-198">Las páginas `Index`, `Error` y `Privacy` sirven para:</span><span class="sxs-lookup"><span data-stu-id="f4155-198">`Index`, `Error`, and `Privacy` pages are provided to:</span></span>

* <span data-ttu-id="f4155-199">`Index`: iniciar una aplicación.</span><span class="sxs-lookup"><span data-stu-id="f4155-199">`Index`: Start an app.</span></span>
* <span data-ttu-id="f4155-200">`Error`: mostrar información sobre errores.</span><span class="sxs-lookup"><span data-stu-id="f4155-200">`Error`: Display error information.</span></span>
* <span data-ttu-id="f4155-201">`Privacy`: especificar los detalles de la directiva de privacidad del sitio web.</span><span class="sxs-lookup"><span data-stu-id="f4155-201">`Privacy`: Specify details about the site's privacy policy.</span></span>

<span data-ttu-id="f4155-202">En este tutorial, las páginas anteriores no se utilizan.</span><span class="sxs-lookup"><span data-stu-id="f4155-202">For this tutorial, the preceding pages are not used.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f4155-203">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f4155-203">Visual Studio</span></span>](#tab/visual-studio)

<a name="f7"></a>
### <a name="use-f7-to-toggle-between-a-razor-page-and-the-pagemodel"></a><span data-ttu-id="f4155-204">Use F7 para alternar entre una página de Razor y la clase PageModel</span><span class="sxs-lookup"><span data-stu-id="f4155-204">Use F7 to toggle between a Razor Page and the PageModel</span></span>

<span data-ttu-id="f4155-205">F7 alterna entre una página de Razor (archivo *\*.cshtml*) y el archivo de C# (*\*.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="f4155-205">F7 toggles between a Razor Page (*\*.cshtml* file) and the C# file (*\*.cshtml.cs*).</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f4155-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f4155-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- TODO review  Need something in these tabs -->

<span data-ttu-id="f4155-207">Por convención, la página de Razor (archivo *\*.cshtml*) y el elemento `PageModel` asociado tienen el mismo nombre de archivo raíz.</span><span class="sxs-lookup"><span data-stu-id="f4155-207">By convention, the Razor Page (*\*.cshtml* file) and the associated `PageModel` have the same root file name.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f4155-208">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="f4155-208">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f4155-209">Por convención, la página de Razor (archivo *\*.cshtml*) y el elemento `PageModel` asociado tienen el mismo nombre de archivo raíz.</span><span class="sxs-lookup"><span data-stu-id="f4155-209">By convention, the Razor Page (*\*.cshtml* file) and the associated `PageModel` have the same root file name.</span></span>

---

> [!div class="step-by-step"]
> [<span data-ttu-id="f4155-210">Siguiente: Adición de un modelo</span><span class="sxs-lookup"><span data-stu-id="f4155-210">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)