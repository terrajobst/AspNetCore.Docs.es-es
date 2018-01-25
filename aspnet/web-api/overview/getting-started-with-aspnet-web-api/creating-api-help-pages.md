---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: "Crear páginas de ayuda para ASP.NET Web API | Documentos de Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2013
ms.topic: article
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 37fd26ebaea192cb540c443eff8a07343ab8c15b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="1d8da-102">Crear páginas de ayuda para ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="1d8da-102">Creating Help Pages for ASP.NET Web API</span></span>
====================
<span data-ttu-id="1d8da-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1d8da-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="1d8da-104">Cuando se crea una API web, a menudo resulta útil crear una página de ayuda, para que otros desarrolladores sepan cómo llamar a la API.</span><span class="sxs-lookup"><span data-stu-id="1d8da-104">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="1d8da-105">Puede crear toda la documentación de forma manual, pero es mejor para generar automáticamente tanto como sea posible.</span><span class="sxs-lookup"><span data-stu-id="1d8da-105">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span>

<span data-ttu-id="1d8da-106">Para facilitar esta tarea, ASP.NET Web API proporciona una biblioteca para generar automáticamente las páginas de ayuda en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="1d8da-106">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="1d8da-107">Crear páginas de Ayuda de API</span><span class="sxs-lookup"><span data-stu-id="1d8da-107">Creating API Help Pages</span></span>

<span data-ttu-id="1d8da-108">Instalar [Web ASP.NET y herramientas de actualización 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="1d8da-108">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="1d8da-109">Esta actualización integra en las páginas de ayuda en la plantilla de proyecto de API Web.</span><span class="sxs-lookup"><span data-stu-id="1d8da-109">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="1d8da-110">A continuación, cree un nuevo proyecto de ASP.NET MVC 4 y seleccione la plantilla de proyecto de API Web.</span><span class="sxs-lookup"><span data-stu-id="1d8da-110">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="1d8da-111">La plantilla de proyecto crea un controlador de API de ejemplo denominado `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="1d8da-111">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="1d8da-112">La plantilla también crea las páginas de Ayuda de API.</span><span class="sxs-lookup"><span data-stu-id="1d8da-112">The template also creates the API help pages.</span></span> <span data-ttu-id="1d8da-113">Todos los archivos de código para la página de ayuda se colocan en la carpeta de áreas del proyecto.</span><span class="sxs-lookup"><span data-stu-id="1d8da-113">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="1d8da-114">Al ejecutar la aplicación, la página principal contiene un vínculo a la página de Ayuda de API.</span><span class="sxs-lookup"><span data-stu-id="1d8da-114">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="1d8da-115">En la página principal, la ruta de acceso relativa es/Help.</span><span class="sxs-lookup"><span data-stu-id="1d8da-115">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="1d8da-116">Este vínculo le lleva a una página de resumen de la API.</span><span class="sxs-lookup"><span data-stu-id="1d8da-116">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="1d8da-117">La vista MVC de esta página se define en Areas/HelpPage/Views/Help/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="1d8da-117">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="1d8da-118">Puede editar esta página para modificar el diseño, introducción, título, estilos y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="1d8da-118">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="1d8da-119">La parte principal de la página es una tabla de API, agrupadas por el controlador.</span><span class="sxs-lookup"><span data-stu-id="1d8da-119">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="1d8da-120">Las entradas de tabla se generan de forma dinámica, mediante la **IApiExplorer** interfaz.</span><span class="sxs-lookup"><span data-stu-id="1d8da-120">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="1d8da-121">(Hablaremos más acerca de esta interfaz más adelante.) Si agrega un nuevo controlador de API, la tabla se actualiza automáticamente en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="1d8da-121">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="1d8da-122">La columna "API" muestra el método HTTP y el URI relativo.</span><span class="sxs-lookup"><span data-stu-id="1d8da-122">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="1d8da-123">La columna "Descripción" contiene documentación para cada API.</span><span class="sxs-lookup"><span data-stu-id="1d8da-123">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="1d8da-124">Inicialmente, la documentación es simplemente texto de marcador de posición.</span><span class="sxs-lookup"><span data-stu-id="1d8da-124">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="1d8da-125">En la sección siguiente, mostraré cómo agregar la documentación de los comentarios XML.</span><span class="sxs-lookup"><span data-stu-id="1d8da-125">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="1d8da-126">Cada API incluye un vínculo a una página con información más detallada, incluidos los cuerpos de solicitud y respuesta de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="1d8da-126">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="1d8da-127">Agregar páginas de ayuda a un proyecto existente</span><span class="sxs-lookup"><span data-stu-id="1d8da-127">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="1d8da-128">Puede agregar páginas de ayuda a un proyecto existente de la API Web mediante el Administrador de paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="1d8da-128">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="1d8da-129">Esta opción es útil que empezar desde una plantilla de proyecto diferente de la plantilla de "API Web".</span><span class="sxs-lookup"><span data-stu-id="1d8da-129">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="1d8da-130">Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**y, a continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="1d8da-130">From the **Tools** menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="1d8da-131">En el [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) ventana, escriba uno de los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="1d8da-131">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="1d8da-132">Para una **C#** aplicación:`Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="1d8da-132">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="1d8da-133">Para una **Visual Basic** aplicación:`Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="1d8da-133">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="1d8da-134">Hay dos paquetes, uno para C# y otro para Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="1d8da-134">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="1d8da-135">Asegúrese de utilizar uno que se adapte al proyecto.</span><span class="sxs-lookup"><span data-stu-id="1d8da-135">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="1d8da-136">Este comando instala a los ensamblados necesarios y agrega las vistas MVC para las páginas de ayuda (que se encuentra en la carpeta áreas/HelpPage).</span><span class="sxs-lookup"><span data-stu-id="1d8da-136">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="1d8da-137">Debe agregar manualmente un vínculo a la página de ayuda.</span><span class="sxs-lookup"><span data-stu-id="1d8da-137">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="1d8da-138">El URI es/Help.</span><span class="sxs-lookup"><span data-stu-id="1d8da-138">The URI is /Help.</span></span> <span data-ttu-id="1d8da-139">Para crear un vínculo en una vista razor, agregue lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1d8da-139">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="1d8da-140">Además, asegúrese de registrar las áreas.</span><span class="sxs-lookup"><span data-stu-id="1d8da-140">Also, make sure to register areas.</span></span> <span data-ttu-id="1d8da-141">En el archivo Global.asax, agregue el código siguiente a la **aplicación\_iniciar** método, si no está allí ya:</span><span class="sxs-lookup"><span data-stu-id="1d8da-141">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="1d8da-142">Agregar la documentación de API</span><span class="sxs-lookup"><span data-stu-id="1d8da-142">Adding API Documentation</span></span>

<span data-ttu-id="1d8da-143">De forma predeterminada, la Ayuda de las páginas tienen cadenas de marcador de posición para la documentación.</span><span class="sxs-lookup"><span data-stu-id="1d8da-143">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="1d8da-144">Puede usar [comentarios de documentación XML](https://msdn.microsoft.com/library/b2s063f7.aspx) para crear la documentación.</span><span class="sxs-lookup"><span data-stu-id="1d8da-144">You can use [XML documentation comments](https://msdn.microsoft.com/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="1d8da-145">Para habilitar esta característica, abra el archivo áreas/HelpPage/aplicación\_Start/HelpPageConfig.cs y elimine la línea siguiente:</span><span class="sxs-lookup"><span data-stu-id="1d8da-145">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="1d8da-146">Ahora habilitar la documentación XML.</span><span class="sxs-lookup"><span data-stu-id="1d8da-146">Now enable XML documentation.</span></span> <span data-ttu-id="1d8da-147">En el Explorador de soluciones, haga clic en el proyecto y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="1d8da-147">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="1d8da-148">Seleccione el **generar** página.</span><span class="sxs-lookup"><span data-stu-id="1d8da-148">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="1d8da-149">En **salida**, comprobar **archivo de documentación XML**.</span><span class="sxs-lookup"><span data-stu-id="1d8da-149">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="1d8da-150">En el cuadro de edición, escriba "aplicación\_Data/XmlDocument.xml".</span><span class="sxs-lookup"><span data-stu-id="1d8da-150">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="1d8da-151">A continuación, abra el código para el `ValuesController` controlador de API, que se define en /Controllers/ValuesControler.cs.</span><span class="sxs-lookup"><span data-stu-id="1d8da-151">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesControler.cs.</span></span> <span data-ttu-id="1d8da-152">Agregar algunos comentarios de documentación a los métodos de controlador.</span><span class="sxs-lookup"><span data-stu-id="1d8da-152">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="1d8da-153">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1d8da-153">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="1d8da-154">Sugerencia: Si coloca el símbolo de intercalación en la línea encima del método y escribe tres barras diagonales, Visual Studio inserta automáticamente los elementos XML.</span><span class="sxs-lookup"><span data-stu-id="1d8da-154">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="1d8da-155">A continuación, puede rellenar los espacios en blanco.</span><span class="sxs-lookup"><span data-stu-id="1d8da-155">Then you can fill in the blanks.</span></span>


<span data-ttu-id="1d8da-156">Ahora compilar y ejecutar la aplicación de nuevo y vaya a las páginas de ayuda.</span><span class="sxs-lookup"><span data-stu-id="1d8da-156">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="1d8da-157">Las cadenas de documentación deben aparecer en la tabla de la API.</span><span class="sxs-lookup"><span data-stu-id="1d8da-157">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="1d8da-158">La página de ayuda lee las cadenas desde el archivo XML en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="1d8da-158">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="1d8da-159">(Cuando se implementa la aplicación, asegúrese de que implementar el archivo XML.)</span><span class="sxs-lookup"><span data-stu-id="1d8da-159">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="1d8da-160">En segundo plano</span><span class="sxs-lookup"><span data-stu-id="1d8da-160">Under the Hood</span></span>

<span data-ttu-id="1d8da-161">Las páginas de ayuda se crean en la parte superior de la **ApiExplorer** (clase), que forma parte del marco Web API.</span><span class="sxs-lookup"><span data-stu-id="1d8da-161">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="1d8da-162">El **ApiExplorer** clase proporciona las materias primas para la creación de una página de ayuda.</span><span class="sxs-lookup"><span data-stu-id="1d8da-162">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="1d8da-163">Para cada API **ApiExplorer** contiene un **ApiDescription** que describe la API.</span><span class="sxs-lookup"><span data-stu-id="1d8da-163">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="1d8da-164">Para ello, una "API" se define como la combinación del método HTTP y URI relativo.</span><span class="sxs-lookup"><span data-stu-id="1d8da-164">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="1d8da-165">Por ejemplo, incluimos algunas API distinto:</span><span class="sxs-lookup"><span data-stu-id="1d8da-165">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="1d8da-166">OBTENER /api/Products</span><span class="sxs-lookup"><span data-stu-id="1d8da-166">GET /api/Products</span></span>
- <span data-ttu-id="1d8da-167">OBTENER/API/Products / {id}</span><span class="sxs-lookup"><span data-stu-id="1d8da-167">GET /api/Products/{id}</span></span>
- <span data-ttu-id="1d8da-168">REGISTRAR/api/productos</span><span class="sxs-lookup"><span data-stu-id="1d8da-168">POST /api/Products</span></span>

<span data-ttu-id="1d8da-169">Si una acción de controlador es compatible con varios métodos HTTP, el **ApiExplorer** trata cada método como una API distinta.</span><span class="sxs-lookup"><span data-stu-id="1d8da-169">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="1d8da-170">Para ocultar una API de la **ApiExplorer**, agregue el **ApiExplorerSettings** atribuir a la acción y el conjunto *IgnoreApi* en true.</span><span class="sxs-lookup"><span data-stu-id="1d8da-170">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="1d8da-171">También puede agregar este atributo al controlador, para excluir el controlador de todo.</span><span class="sxs-lookup"><span data-stu-id="1d8da-171">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="1d8da-172">La clase ApiExplorer Obtiene las cadenas de documentación de la **IDocumentationProvider** interfaz.</span><span class="sxs-lookup"><span data-stu-id="1d8da-172">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="1d8da-173">Como se vio anteriormente, la biblioteca de páginas de Ayuda proporciona un **IDocumentationProvider** que obtiene la documentación de las cadenas de documentación XML.</span><span class="sxs-lookup"><span data-stu-id="1d8da-173">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="1d8da-174">El código se encuentra en /Areas/HelpPage/XmlDocumentationProvider.cs.</span><span class="sxs-lookup"><span data-stu-id="1d8da-174">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="1d8da-175">Puede obtener documentación de otro origen escribiendo su propio **IDocumentationProvider**.</span><span class="sxs-lookup"><span data-stu-id="1d8da-175">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="1d8da-176">Para vincularla, llame a la **SetDocumentationProvider** método de extensión, definido en **HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="1d8da-176">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="1d8da-177">**ApiExplorer** llama automáticamente a la **IDocumentationProvider** interfaz para obtener cadenas de documentación para cada API.</span><span class="sxs-lookup"><span data-stu-id="1d8da-177">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="1d8da-178">Almacena en la **documentación** propiedad de la **ApiDescription** y **ApiParameterDescription** objetos.</span><span class="sxs-lookup"><span data-stu-id="1d8da-178">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1d8da-179">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="1d8da-179">Next Steps</span></span>

<span data-ttu-id="1d8da-180">No se limita a las páginas de ayuda que se muestra aquí.</span><span class="sxs-lookup"><span data-stu-id="1d8da-180">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="1d8da-181">De hecho, **ApiExplorer** no se limita a la creación de páginas de ayuda.</span><span class="sxs-lookup"><span data-stu-id="1d8da-181">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="1d8da-182">Yao Huang Lin ha escrito que las entradas de algunos blog excelente para que pueda pensar de fábrica:</span><span class="sxs-lookup"><span data-stu-id="1d8da-182">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="1d8da-183">Agregar a un cliente de prueba simple a la página de Ayuda de ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="1d8da-183">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="1d8da-184">Para que la página de Ayuda de API Web de ASP.NET se funcionen en los servicios autohospedados</span><span class="sxs-lookup"><span data-stu-id="1d8da-184">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="1d8da-185">Generación de tiempo de diseño de página de ayuda (o cliente) para ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="1d8da-185">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="1d8da-186">Personalizaciones avanzadas de la página de ayuda</span><span class="sxs-lookup"><span data-stu-id="1d8da-186">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
