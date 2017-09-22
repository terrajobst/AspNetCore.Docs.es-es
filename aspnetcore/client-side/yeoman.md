---
title: Compilar proyectos con Yeoman en ASP.NET Core
author: spboyer
description: "Este artículo le guía por la creación de un núcleo de ASP.NET aplicación web mediante la Yeoman generador en macOS."
keywords: "Núcleo de ASP.NET, Yeoman, multiplataforma, yo aspnet"
ms.author: spboyer
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: fda0c2a8-1743-4505-be1a-7f8ceeef8647
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/yeoman
ms.openlocfilehash: d7411c1635e9fef2857f9a03e7310224ee8d7344
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-building-projects-with-yeoman-in-aspnet-core"></a><span data-ttu-id="73058-104">Introducción a la creación de proyectos con Yeoman en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="73058-104">Introduction to building projects with Yeoman in ASP.NET Core</span></span>

<span data-ttu-id="73058-105">[Yeoman](http://yeoman.io/) es un sistema de scaffolding de proyecto para crear muchos tipos de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="73058-105">[Yeoman](http://yeoman.io/) is a project scaffolding system for creating many kinds of applications.</span></span> <span data-ttu-id="73058-106">El Yeoman generador para ASP.NET Core contiene una variedad de plantillas de proyecto para iniciar un nuevo sitio web, MVC o aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="73058-106">The Yeoman generator for ASP.NET Core contains a variety of project templates for starting a new web, MVC, or console application.</span></span>

## <a name="install-nodejs-npm-and-yeoman"></a><span data-ttu-id="73058-107">Instalar Node.js y npm, Yeoman</span><span class="sxs-lookup"><span data-stu-id="73058-107">Install Node.js, npm, and Yeoman</span></span>

### <a name="prerequisites"></a><span data-ttu-id="73058-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="73058-108">Prerequisites</span></span>

<span data-ttu-id="73058-109">Se requiere para Yeoman Node.js y npm.</span><span class="sxs-lookup"><span data-stu-id="73058-109">Node.js and npm are required for Yeoman.</span></span> <span data-ttu-id="73058-110">Descargar desde [Node.js](https://nodejs.org/).</span><span class="sxs-lookup"><span data-stu-id="73058-110">Download from [Node.js](https://nodejs.org/).</span></span> <span data-ttu-id="73058-111">El instalador incluye [Node.js](https://nodejs.org/) y [npm](https://www.npmjs.com/).</span><span class="sxs-lookup"><span data-stu-id="73058-111">The installer includes [Node.js](https://nodejs.org/) and [npm](https://www.npmjs.com/).</span></span> <span data-ttu-id="73058-112">Bower también es necesario para la instalación de marcos de interfaz de usuario como de arranque.</span><span class="sxs-lookup"><span data-stu-id="73058-112">Bower is also required for installing UI frameworks like Bootstrap.</span></span>

<span data-ttu-id="73058-113">Para instalar Yeoman y Bower, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="73058-113">To install Yeoman and Bower, run the following command:</span></span>

```console
npm install -g yo bower
```

>[!Note]
><span data-ttu-id="73058-114">Si se produce un error `npm ERR! Please try running this command again as root/Administrator.` en macOS, ejecute el siguiente comando mediante [sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html):`sudo npm install -g yo bower`</span><span class="sxs-lookup"><span data-stu-id="73058-114">If you get the error `npm ERR! Please try running this command again as root/Administrator.` on macOS, run the following command using [sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html): `sudo npm install -g yo bower`</span></span>

<span data-ttu-id="73058-115">Desde un símbolo del sistema, instalar el generador ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="73058-115">From a command prompt, install the ASP.NET generator:</span></span>

```console
npm install -g generator-aspnet
```

> [!NOTE]
> <span data-ttu-id="73058-116">Si recibe un error de permiso, ejecute el comando en `sudo` tal y como se ha descrito anteriormente.</span><span class="sxs-lookup"><span data-stu-id="73058-116">If you get a permission error, run the command under `sudo` as described above.</span></span>

<span data-ttu-id="73058-117">El `–g` marca instala el generador globalmente, de modo que se puede utilizar en cualquier ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="73058-117">The `–g` flag installs the generator globally, so that it can be used from any path.</span></span>

## <a name="create-an-aspnet-app"></a><span data-ttu-id="73058-118">Crear una aplicación ASP.NET</span><span class="sxs-lookup"><span data-stu-id="73058-118">Create an ASP.NET app</span></span>

<span data-ttu-id="73058-119">Ejecutar el generador de ASP.NET basado en Yeoman:</span><span class="sxs-lookup"><span data-stu-id="73058-119">Run the Yeoman-based ASP.NET generator:</span></span>

```console
yo aspnet
```

<span data-ttu-id="73058-120">El generador de muestra un menú.</span><span class="sxs-lookup"><span data-stu-id="73058-120">The generator displays a menu.</span></span> <span data-ttu-id="73058-121">Flecha hacia abajo hasta la **básicos de aplicación Web [sin pertenencia y autorización]** del proyecto y pulse **ENTRAR**:</span><span class="sxs-lookup"><span data-stu-id="73058-121">Arrow down to the **Web Application Basic [without Membership and Authorization]** project and tap **Enter**:</span></span>

![Ventana de comandos: ¿qué tipo de aplicación desea crear?](yeoman/_static/yeoman-yo-aspnet.png)

<span data-ttu-id="73058-124">Seleccione Bootstrap como el marco de interfaz de usuario y pulse **ENTRAR**.</span><span class="sxs-lookup"><span data-stu-id="73058-124">Select Bootstrap as the UI Framework and tap **Enter**.</span></span>

<span data-ttu-id="73058-125">Use "**MyWebApp**" para el nombre de la aplicación y a continuación, puntee **ENTRAR**.</span><span class="sxs-lookup"><span data-stu-id="73058-125">Use "**MyWebApp**" for the app name and then tap **Enter**.</span></span>

<span data-ttu-id="73058-126">Yeoman se aplique la técnica scaffolding al proyecto y sus archivos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="73058-126">Yeoman will scaffold the project and its supporting files.</span></span> <span data-ttu-id="73058-127">También se proporcionan los pasos siguientes sugeridos en forma de comandos.</span><span class="sxs-lookup"><span data-stu-id="73058-127">Suggested next steps are also provided in the form of commands.</span></span>

![Ventana de comandos: ¿cuál es el nombre de la aplicación ASP.NET?](yeoman/_static/yeoman-yo-aspnet-created.png)

<span data-ttu-id="73058-130">El [generador ASP.NET](https://www.npmjs.com/package/generator-aspnet) crea ASP.NET Core proyectos que se pueden cargar en el código de Visual Studio, Visual Studio, o ejecutar desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="73058-130">The [ASP.NET generator](https://www.npmjs.com/package/generator-aspnet) creates ASP.NET Core projects that can be loaded into Visual Studio Code, Visual Studio, or run from the command line.</span></span>

## <a name="restore-build-and-run"></a><span data-ttu-id="73058-131">Restaurar, compilar y ejecutar</span><span class="sxs-lookup"><span data-stu-id="73058-131">Restore, build, and run</span></span>

<span data-ttu-id="73058-132">Siga los comandos sugeridos, cambie los directorios a la `MyWebApp` directory.</span><span class="sxs-lookup"><span data-stu-id="73058-132">Follow the suggested commands by changing directories to the `MyWebApp` directory.</span></span> <span data-ttu-id="73058-133">A continuación, ejecute `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="73058-133">Then run `dotnet restore`.</span></span>

![Comandos (ventana)](yeoman/_static/dotnet-restore.png)

<span data-ttu-id="73058-135">Compilar y ejecutar la aplicación con `dotnet build` y `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="73058-135">Build and run the app using `dotnet build` and `dotnet run`:</span></span>

![Comandos (ventana)](yeoman/_static/dotnet-build-run.png)

<span data-ttu-id="73058-137">En este punto, puede navegar a la dirección URL se muestra para probar la aplicación de ASP.NET Core recién creado.</span><span class="sxs-lookup"><span data-stu-id="73058-137">At this point, you can navigate to the URL shown to test the newly-created ASP.NET Core app.</span></span>

## <a name="client-side-packages"></a><span data-ttu-id="73058-138">Paquetes de cliente</span><span class="sxs-lookup"><span data-stu-id="73058-138">Client-side packages</span></span>

<span data-ttu-id="73058-139">Los recursos de front-end se proporcionan con las plantillas de la Yeoman generador utilizando el [Bower](xref:client-side/bower) Administrador de paquetes de cliente, agregar *bower.json* y *bowerrc* archivos para restaurar los paquetes de cliente mediante Bower.</span><span class="sxs-lookup"><span data-stu-id="73058-139">The front-end resources are provided by the templates from the Yeoman generator using the [Bower](xref:client-side/bower) client-side package manager, adding *bower.json* and *.bowerrc* files to restore client-side packages using Bower.</span></span>

<span data-ttu-id="73058-140">El [BundlerMinifier](xref:client-side/bundling-and-minification) componente también se incluye de forma predeterminada para facilitar la concatenación (unión) y minificación de CSS, JavaScript y HTML.</span><span class="sxs-lookup"><span data-stu-id="73058-140">The [BundlerMinifier](xref:client-side/bundling-and-minification) component is also included by default for ease of concatenation (bundling) and minification of CSS, JavaScript, and HTML.</span></span>

## <a name="building-and-running-from-visual-studio"></a><span data-ttu-id="73058-141">Compilar y ejecutar desde Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73058-141">Building and running from Visual Studio</span></span>

<span data-ttu-id="73058-142">Puede cargar el proyecto web de ASP.NET Core generado directamente en Visual Studio, a continuación, compile y ejecute el proyecto a partir de ahí.</span><span class="sxs-lookup"><span data-stu-id="73058-142">You can load your generated ASP.NET Core web project directly into Visual Studio, then build and run your project from there.</span></span> <span data-ttu-id="73058-143">Siga las instrucciones anteriores para aplicar una nueva aplicación de ASP.NET Core mediante Yeoman la técnica scaffolding.</span><span class="sxs-lookup"><span data-stu-id="73058-143">Follow the instructions above to scaffold a new ASP.NET Core app using Yeoman.</span></span> <span data-ttu-id="73058-144">Esta vez, se selecciona **aplicación Web** en el menú y el nombre de la aplicación `MyWebApp`.</span><span class="sxs-lookup"><span data-stu-id="73058-144">This time, choose **Web Application** from the menu and name the app `MyWebApp`.</span></span>

<span data-ttu-id="73058-145">Abra Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73058-145">Open Visual Studio.</span></span> <span data-ttu-id="73058-146">En el menú archivo, seleccione ‣ Abrir proyecto o solución.</span><span class="sxs-lookup"><span data-stu-id="73058-146">From the File menu, select Open ‣ Project/Solution.</span></span>

<span data-ttu-id="73058-147">En el cuadro de diálogo Abrir proyecto, vaya a la *.csproj* de archivo, selecciónelo y haga clic en el **abiertos** botón.</span><span class="sxs-lookup"><span data-stu-id="73058-147">In the Open Project dialog, navigate to the *.csproj* file, select it, and click the **Open** button.</span></span> <span data-ttu-id="73058-148">En el Explorador de soluciones, el proyecto debe ser similar a la captura de pantalla siguiente.</span><span class="sxs-lookup"><span data-stu-id="73058-148">In the Solution Explorer, the project should look something like the screenshot below.</span></span>

![Archivos y carpetas de un nuevo proyecto en el Explorador de soluciones](yeoman/_static/yeoman-solution.png)

<span data-ttu-id="73058-150">Yeoman scaffolds una aplicación web MVC, compatibilidad de compilación de servidor y cliente junto con ambos.</span><span class="sxs-lookup"><span data-stu-id="73058-150">Yeoman scaffolds a MVC web application, complete with both server- and client-side build support.</span></span> <span data-ttu-id="73058-151">Dependencias de servidor aparecen en la **dependencias/NuGet** nodo y las dependencias del lado cliente en el **dependencias/Bower** nodo del explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="73058-151">Server-side dependencies are listed under the **Dependencies/NuGet** node, and client-side dependencies in the **Dependencies/Bower** node of Solution Explorer.</span></span> <span data-ttu-id="73058-152">Las dependencias se restauran automáticamente cuando se carga el proyecto.</span><span class="sxs-lookup"><span data-stu-id="73058-152">Dependencies are restored automatically when the project is loaded.</span></span>

![En el nodo dependencias en la vista de árbol del explorador de soluciones, la carpeta de Bower está abierta enumerar sus dependencias.](yeoman/_static/yeoman-loading-dependencies.png)

<span data-ttu-id="73058-154">Cuando se restauran todas las dependencias, presione **F5** para ejecutar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="73058-154">When all the dependencies are restored, press **F5** to run the project.</span></span> <span data-ttu-id="73058-155">La página principal predeterminada se muestra en el explorador.</span><span class="sxs-lookup"><span data-stu-id="73058-155">The default home page displays in the browser.</span></span>

![Aplicación Web abierta en Microsoft Edge](yeoman/_static/yeoman-home-page.png)

## <a name="restoring-building-and-hosting-from-a-command-line"></a><span data-ttu-id="73058-157">Restauración, creación y hospedaje desde una línea de comandos</span><span class="sxs-lookup"><span data-stu-id="73058-157">Restoring, building, and hosting from a command line</span></span>

<span data-ttu-id="73058-158">Puede preparar y hospedar la aplicación web mediante la CLI de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="73058-158">You can prepare and host your web application using the .NET Core CLI.</span></span>

<span data-ttu-id="73058-159">En un símbolo del sistema, cambie el directorio actual a la carpeta que contiene el proyecto (es decir, la carpeta que contiene el *.csproj* archivo):</span><span class="sxs-lookup"><span data-stu-id="73058-159">At a command prompt, change the current directory to the folder containing the project (that is, the folder containing the *.csproj* file):</span></span>

```console
cd src\MyWebApp
```

<span data-ttu-id="73058-160">Restaurar las dependencias de paquetes de NuGet del proyecto:</span><span class="sxs-lookup"><span data-stu-id="73058-160">Restore the project's NuGet package dependencies:</span></span>

```console
dotnet restore
```

<span data-ttu-id="73058-161">Ejecute la aplicación:</span><span class="sxs-lookup"><span data-stu-id="73058-161">Run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="73058-162">La multiplataforma [Kestrel](xref:fundamentals/servers/kestrel) servidor web iniciará escuchando en el puerto 5000.</span><span class="sxs-lookup"><span data-stu-id="73058-162">The cross-platform [Kestrel](xref:fundamentals/servers/kestrel) web server will begin listening on port 5000.</span></span>

<span data-ttu-id="73058-163">Abra un explorador web y navegue hasta `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="73058-163">Open a web browser, and navigate to `http://localhost:5000`.</span></span>

![Aplicación Web abierta en Microsoft Edge](yeoman/_static/yeoman-home-page_5000.png)

## <a name="adding-to-your-project-with-sub-generators"></a><span data-ttu-id="73058-165">Agregar a un proyecto con los generadores de sub</span><span class="sxs-lookup"><span data-stu-id="73058-165">Adding to your project with sub generators</span></span>

<span data-ttu-id="73058-166">Usando Yeoman [sub generadores](https://github.com/omnisharp/generator-aspnet), puede agregar un `nuget.config` o `web.config` después de crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="73058-166">Using Yeoman [sub generators](https://github.com/omnisharp/generator-aspnet), you can add either a `nuget.config` or a `web.config` after the project is created.</span></span> <span data-ttu-id="73058-167">Por ejemplo, ejecute el siguiente comando desde el directorio en el que se debe crear el archivo:</span><span class="sxs-lookup"><span data-stu-id="73058-167">For example, execute the following command from the directory in which the file should be created:</span></span>

```console
yo aspnet:nugetconfig
```

<span data-ttu-id="73058-168">El resultado es un archivo de configuración de NuGet denominado `nuget.config` con el siguiente contenido:</span><span class="sxs-lookup"><span data-stu-id="73058-168">The result is a NuGet configuration file named `nuget.config` with the following content:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
 <packageSources>
    <!--To inherit the global NuGet package sources remove the <clear/> line below -->
    <clear />
 </packageSources>
</configuration>
```

## <a name="additional-resources"></a><span data-ttu-id="73058-169">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="73058-169">Additional resources</span></span>

* [<span data-ttu-id="73058-170">Servidores (Kestrel y WebListener)</span><span class="sxs-lookup"><span data-stu-id="73058-170">Servers (Kestrel and WebListener)</span></span>](xref:fundamentals/servers/index)
* [<span data-ttu-id="73058-171">Aspectos básicos</span><span class="sxs-lookup"><span data-stu-id="73058-171">Fundamentals</span></span>](xref:fundamentals/index)
