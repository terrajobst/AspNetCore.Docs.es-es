---
title: Uso de Bower en ASP.NET Core
author: rick-anderson
description: Administrar paquetes de cliente con Bower.
keywords: ASP.NET Core,bower
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: df7c43da-280e-4df6-86cb-eecec8f12bfc
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/bower
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 183e748cbf87b1973941eacb3fb1008f4041bb2a
ms.sourcegitcommit: 532a323f99a37c4d7894c95cee3f7a04b594dcec
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2017
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a><span data-ttu-id="bf1f8-104">Administrar paquetes de cliente con Bower en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bf1f8-104">Manage client-side packages with Bower in ASP.NET Core</span></span>

<span data-ttu-id="bf1f8-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel arroz](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), y [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="bf1f8-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), and [Scott Addie](https://scottaddie.com)</span></span> 

<span data-ttu-id="bf1f8-106">[Bower](https://bower.io/) llama a sí mismo "Un administrador de paquetes para la web."</span><span class="sxs-lookup"><span data-stu-id="bf1f8-106">[Bower](https://bower.io/) calls itself "A package manager for the web."</span></span> <span data-ttu-id="bf1f8-107">Dentro del ecosistema de. NET, que se llena el espacio vacío a la izquierda incapacidad de NuGet para entregar archivos de contenido estático.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-107">Within the .NET ecosystem, it fills the void left by NuGet’s inability to deliver static content files.</span></span> <span data-ttu-id="bf1f8-108">Para los proyectos de ASP.NET Core, estos archivos estáticos son inherentes a las bibliotecas de cliente como [jQuery](http://jquery.com/) y [arranque](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="bf1f8-108">For ASP.NET Core projects, these static files are inherent to client-side libraries like [jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="bf1f8-109">Para las bibliotecas. NET, seguir usando [NuGet](https://www.nuget.org/) Administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-109">For .NET libraries, you still use [NuGet](https://www.nuget.org/) package manager.</span></span>

<span data-ttu-id="bf1f8-110">Proceso de compilación de proyectos nuevos creados con las plantillas de proyecto de ASP.NET Core configurar el cliente.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-110">New projects created with the ASP.NET Core project templates set up the client-side build process.</span></span> <span data-ttu-id="bf1f8-111">[jQuery](http://jquery.com/) y [arranque](http://getbootstrap.com/) están instalados, y se admite Bower.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-111">[jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/) are installed, and Bower is supported.</span></span>

<span data-ttu-id="bf1f8-112">Paquetes de cliente se muestran en la *bower.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-112">Client-side packages are listed in the *bower.json* file.</span></span> <span data-ttu-id="bf1f8-113">Configura las plantillas de proyecto de ASP.NET Core *bower.json* con jQuery, validación de jQuery y arranque.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-113">The ASP.NET Core project templates configures *bower.json* with jQuery, jQuery validation, and Bootstrap.</span></span>

<span data-ttu-id="bf1f8-114">En este tutorial, vamos a agregar compatibilidad para [fuente Maravilla](http://fontawesome.io).</span><span class="sxs-lookup"><span data-stu-id="bf1f8-114">In this tutorial, we'll add support for [Font Awesome](http://fontawesome.io).</span></span> <span data-ttu-id="bf1f8-115">Se pueden instalar paquetes de bower con el **administrar paquetes de Bower** interfaz de usuario o manualmente en el *bower.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-115">Bower packages can be installed with the **Manage Bower Packages** UI or manually in the *bower.json* file.</span></span>

### <a name="installation-via-manage-bower-packages-ui"></a><span data-ttu-id="bf1f8-116">Instalación mediante la opción administrar paquetes de Bower interfaz de usuario</span><span class="sxs-lookup"><span data-stu-id="bf1f8-116">Installation via Manage Bower Packages UI</span></span>

* <span data-ttu-id="bf1f8-117">Crear una nueva aplicación Web de ASP.NET Core con el **aplicación Web de ASP.NET Core (.NET Core)** plantilla.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-117">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="bf1f8-118">Seleccione **aplicación Web** y **sin autenticación**.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-118">Select **Web Application** and **No Authentication**.</span></span>

* <span data-ttu-id="bf1f8-119">Haga clic en el proyecto en el Explorador de soluciones y seleccione **administrar paquetes de Bower** (o bien en el menú principal, **proyecto** > **administrar paquetes de Bower**).</span><span class="sxs-lookup"><span data-stu-id="bf1f8-119">Right-click the project in Solution Explorer and select **Manage Bower Packages** (alternatively from the main menu, **Project** > **Manage Bower Packages**).</span></span>

* <span data-ttu-id="bf1f8-120">En el **Bower: \<nombre del proyecto\>**  ventana, haga clic en la ficha "Examinar" y, a continuación, filtre la lista de paquetes especificando `font-awesome` en el cuadro de búsqueda:</span><span class="sxs-lookup"><span data-stu-id="bf1f8-120">In the **Bower: \<project name\>** window, click the "Browse" tab, and then filter the packages list by entering `font-awesome` in the search box:</span></span>

 ![Administrar paquetes de bower](bower/_static/manage-bower-packages.png)

* <span data-ttu-id="bf1f8-122">Confirme que la "guardar los cambios en *bower.json*" casilla de verificación está activada.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-122">Confirm that the "Save changes to *bower.json*" checkbox is checked.</span></span> <span data-ttu-id="bf1f8-123">Seleccione una versión de la lista desplegable y haga clic en el **instalar** botón.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-123">Select a version from the drop-down list and click the **Install** button.</span></span> <span data-ttu-id="bf1f8-124">El **salida** ventana muestra los detalles de instalación.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-124">The **Output** window shows the installation details.</span></span>

### <a name="manual-installation-in-bowerjson"></a><span data-ttu-id="bf1f8-125">Instalación manual en bower.json</span><span class="sxs-lookup"><span data-stu-id="bf1f8-125">Manual installation in bower.json</span></span>

<span data-ttu-id="bf1f8-126">Abra la *bower.json* de archivos y agregar "fuente maravillosa" a las dependencias.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-126">Open the *bower.json* file and add "font-awesome" to the dependencies.</span></span> <span data-ttu-id="bf1f8-127">IntelliSense muestra los paquetes disponibles.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-127">IntelliSense shows the available packages.</span></span> <span data-ttu-id="bf1f8-128">Cuando se selecciona un paquete, se muestran las versiones disponibles.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-128">When a package is selected, the available versions are displayed.</span></span> <span data-ttu-id="bf1f8-129">Las siguientes imágenes anteriores y no coincidirá con lo que se ve.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-129">The images below are older and will not match what you see.</span></span>

![IntelliSense de explorador de paquetes de bower](bower/_static/add-package.png)

![versión de bower IntelliSense](bower/_static/version-intelliSense.png)

<span data-ttu-id="bf1f8-132">Usos de bower [control de versiones semántico](http://semver.org/) para organizar las dependencias.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-132">Bower uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="bf1f8-133">Control de versiones semántico, también conocido como SemVer, identifica los paquetes con el esquema de numeración \<principal >.\< secundaria >. \<revisión >.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-133">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="bf1f8-134">IntelliSense simplifica el control de versiones semántico presentando unas cuantas opciones comunes.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-134">IntelliSense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="bf1f8-135">El elemento superior en la lista de IntelliSense (4.6.3 en el ejemplo anterior) se considera la versión estable más reciente del paquete.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-135">The top item in the IntelliSense list (4.6.3 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="bf1f8-136">El símbolo de intercalación (^) coincide con la versión principal más reciente y la tilde (~) coincide con la versión secundaria más reciente.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-136">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span>

<span data-ttu-id="bf1f8-137">Guardar el *bower.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-137">Save the *bower.json* file.</span></span> <span data-ttu-id="bf1f8-138">Visual Studio busca la *bower.json* archivo para los cambios.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-138">Visual Studio watches the *bower.json* file for changes.</span></span> <span data-ttu-id="bf1f8-139">Al guardar, el *bower install* se ejecuta el comando.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-139">Upon saving, the *bower install* command is executed.</span></span> <span data-ttu-id="bf1f8-140">Vea la ventana de salida **npm/Bower** vista para el comando exacto que se ejecuta.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-140">See the Output window's **Bower/npm** view for the exact command executed.</span></span>

<span data-ttu-id="bf1f8-141">Abra la *bowerrc* de archivos en *bower.json*.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-141">Open the *.bowerrc* file under *bower.json*.</span></span> <span data-ttu-id="bf1f8-142">El `directory` propiedad está establecida en *wwwroot/lib* que indica la ubicación Bower instalará los activos de paquete.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-142">The `directory` property is set to *wwwroot/lib* which indicates the location Bower will install the package assets.</span></span>

```json
{
 "directory": "wwwroot/lib"
}
```

<span data-ttu-id="bf1f8-143">Puede usar el cuadro de búsqueda en el Explorador de soluciones para buscar y mostrar el paquete de fuente Maravilla.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-143">You can use the search box in Solution Explorer to find and display the font-awesome package.</span></span>

<span data-ttu-id="bf1f8-144">Abra la *Views\Shared\_Layout.cshtml* de archivos y agregar el archivo CSS de fuente Maravilla en el entorno de [etiqueta auxiliar](xref:mvc/views/tag-helpers/intro) para `Development`.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-144">Open the *Views\Shared\_Layout.cshtml* file and add the font-awesome CSS file to the environment [Tag Helper](xref:mvc/views/tag-helpers/intro) for `Development`.</span></span> <span data-ttu-id="bf1f8-145">En el Explorador de soluciones, arrastre y coloque *fuente awesome.css* dentro de la `<environment names="Development">` elemento.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-145">From Solution Explorer, drag and drop *font-awesome.css* inside the `<environment names="Development">` element.</span></span>

[!code-html[Main](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

<span data-ttu-id="bf1f8-146">En una aplicación de producción agregaría *fuente awesome.min.css* a la aplicación auxiliar de etiquetas de entorno para `Staging,Production`.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-146">In a production app you would add *font-awesome.min.css* to the environment tag helper for `Staging,Production`.</span></span>

<span data-ttu-id="bf1f8-147">Reemplace el contenido de la *Views\Home\About.cshtml* archivo Razor con el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="bf1f8-147">Replace the contents of the *Views\Home\About.cshtml* Razor file with the following markup:</span></span>

[!code-html[Main](bower/sample/About.cshtml)]

<span data-ttu-id="bf1f8-148">Ejecutar la aplicación y navegue hasta la vista acerca para comprobar el funciona de Maravilla de fuente del paquete.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-148">Run the app and navigate to the About view to verify the font-awesome package works.</span></span>

## <a name="exploring-the-client-side-build-process"></a><span data-ttu-id="bf1f8-149">Explorar el proceso de compilación de cliente</span><span class="sxs-lookup"><span data-stu-id="bf1f8-149">Exploring the client-side build process</span></span>

<span data-ttu-id="bf1f8-150">Plantillas de proyecto de ASP.NET Core mayoría ya están configuradas para usar Bower.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-150">Most ASP.NET Core project templates are already configured to use Bower.</span></span> <span data-ttu-id="bf1f8-151">En este tutorial siguiente comienza con un proyecto vacío de ASP.NET Core y agrega cada pieza manualmente, por lo que puede hacerse una idea de cómo se usa Bower en un proyecto.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-151">This next walkthrough starts with an empty ASP.NET Core project and adds each piece manually, so you can get a feel for how Bower is used in a project.</span></span> <span data-ttu-id="bf1f8-152">Puede ver lo que ocurre con la estructura del proyecto y el tiempo de ejecución como resultado que se realiza cada cambio de configuración.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-152">You can see what happens to the project structure and the runtime output as each configuration change is made.</span></span>

<span data-ttu-id="bf1f8-153">Los pasos generales para usar el proceso de compilación de cliente con Bower son:</span><span class="sxs-lookup"><span data-stu-id="bf1f8-153">The general steps to use the client-side build process with Bower are:</span></span>

* <span data-ttu-id="bf1f8-154">Definir los paquetes utilizados en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-154">Define packages used in your project.</span></span> <!-- once defined, you don't need to download them, VS does -->
* <span data-ttu-id="bf1f8-155">Paquetes de referencia desde las páginas web.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-155">Reference packages from your web pages.</span></span>

### <a name="define-packages"></a><span data-ttu-id="bf1f8-156">Definir paquetes</span><span class="sxs-lookup"><span data-stu-id="bf1f8-156">Define packages</span></span>

<span data-ttu-id="bf1f8-157">Una vez que enumerar los paquetes en el *bower.json* archivo, Visual Studio, descargará.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-157">Once you list packages in the *bower.json* file, Visual Studio will download them.</span></span> <span data-ttu-id="bf1f8-158">En el ejemplo siguiente se utiliza Bower para cargar jQuery y arranque a la *wwwroot* carpeta.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-158">The following example uses Bower to load jQuery and Bootstrap to the *wwwroot* folder.</span></span>

* <span data-ttu-id="bf1f8-159">Crear una nueva aplicación Web de ASP.NET Core con el **aplicación Web de ASP.NET Core (.NET Core)** plantilla.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-159">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="bf1f8-160">Seleccione el **vacía** plantilla de proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-160">Select the **Empty** project template and click **OK**.</span></span>

* <span data-ttu-id="bf1f8-161">En el Explorador de soluciones, haga clic en el proyecto > **Agregar nuevo elemento** y seleccione **archivo de configuración de Bower**.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-161">In Solution Explorer, right-click the project > **Add New Item** and select **Bower Configuration File**.</span></span> <span data-ttu-id="bf1f8-162">Nota: Una *bowerrc* también se agrega el archivo.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-162">Note: A *.bowerrc* file is also added.</span></span>

* <span data-ttu-id="bf1f8-163">Abra *bower.json*y agregar jquery y arrancar en el `dependencies` sección.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-163">Open *bower.json*, and add jquery and bootstrap to the `dependencies` section.</span></span> <span data-ttu-id="bf1f8-164">Resultante *bower.json* archivo tendrá un aspecto similar al ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-164">The resulting *bower.json* file will look like the following example.</span></span> <span data-ttu-id="bf1f8-165">Las versiones cambiarán con el tiempo y no pueden coincidir con la imagen siguiente.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-165">The versions will change over time and may not match the image below.</span></span>

[!code-json[Main](bower/sample/bower.json?highlight=5,6)]

* <span data-ttu-id="bf1f8-166">Guardar el *bower.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-166">Save the *bower.json* file.</span></span>

 <span data-ttu-id="bf1f8-167">Compruebe que el proyecto incluye la *arranque* y *jQuery* directorios en *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-167">Verify the project includes the *bootstrap* and *jQuery* directories in *wwwroot/lib*.</span></span> <span data-ttu-id="bf1f8-168">Bower utiliza el *bowerrc* archivo para instalar los activos en *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-168">Bower uses the *.bowerrc* file to install the assets in *wwwroot/lib*.</span></span>

 <span data-ttu-id="bf1f8-169">Nota: La interfaz de usuario "Administrar paquetes de Bower" proporciona una alternativa a modificar el archivo manualmente.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-169">Note: The "Manage Bower Packages" UI provides an alternative to manual file editing.</span></span>

### <a name="enable-static-files"></a><span data-ttu-id="bf1f8-170">Habilitar archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="bf1f8-170">Enable static files</span></span>

* <span data-ttu-id="bf1f8-171">Agregar el `Microsoft.AspNetCore.StaticFiles` paquete NuGet para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-171">Add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project.</span></span>
* <span data-ttu-id="bf1f8-172">Habilitar archivos estáticos que se sirvan con el [middleware de archivos estáticos](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions).</span><span class="sxs-lookup"><span data-stu-id="bf1f8-172">Enable static files to be served with the [Static file middleware](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions).</span></span> <span data-ttu-id="bf1f8-173">Agregue una llamada a [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) a la `Configure` método `Startup`.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-173">Add a call to [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) to the `Configure` method of `Startup`.</span></span>

[!code-csharp[Main](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a><span data-ttu-id="bf1f8-174">Paquetes de referencia</span><span class="sxs-lookup"><span data-stu-id="bf1f8-174">Reference packages</span></span>

<span data-ttu-id="bf1f8-175">En esta sección, creará una página HTML para comprobar puede tener acceso a los paquetes implementados.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-175">In this section, you will create an HTML page to verify it can access the deployed packages.</span></span>

* <span data-ttu-id="bf1f8-176">Agregar una nueva página HTML denominada *Index.html* a la *wwwroot* carpeta.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-176">Add a new HTML page named *Index.html* to the *wwwroot* folder.</span></span> <span data-ttu-id="bf1f8-177">Nota: Debe agregar el archivo HTML para la *wwwroot* carpeta.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-177">Note: You must add the HTML file to the *wwwroot* folder.</span></span> <span data-ttu-id="bf1f8-178">De forma predeterminada, no se pueda servir contenido estático fuera *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-178">By default, static content cannot be served outside *wwwroot*.</span></span> <span data-ttu-id="bf1f8-179">Vea [trabajar con archivos estáticos](xref:fundamentals/static-files) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-179">See [Working with static files](xref:fundamentals/static-files) for more information.</span></span>

 <span data-ttu-id="bf1f8-180">Reemplace el contenido de *Index.html* con el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="bf1f8-180">Replace the contents of *Index.html* with the following markup:</span></span>

[!code-html[Main](bower/sample/Index.html)]

* <span data-ttu-id="bf1f8-181">Ejecute la aplicación y vaya a `http://localhost:<port>/Index.html`.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-181">Run the app and navigate to `http://localhost:<port>/Index.html`.</span></span> <span data-ttu-id="bf1f8-182">O bien, con *Index.html* abierto, presione `Ctrl+Shift+W`.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-182">Alternatively, with *Index.html* opened, press `Ctrl+Shift+W`.</span></span> <span data-ttu-id="bf1f8-183">Compruebe que se aplica el estilo jumbotron, el código de jQuery responde cuando se hace clic en el botón y que el botón de arranque cambia el estado.</span><span class="sxs-lookup"><span data-stu-id="bf1f8-183">Verify that the jumbotron styling is applied, the jQuery code responds when the button is clicked, and that the Bootstrap button changes state.</span></span>

 ![estilo de Jumbotron aplicado](bower/_static/jumbotron.png)
