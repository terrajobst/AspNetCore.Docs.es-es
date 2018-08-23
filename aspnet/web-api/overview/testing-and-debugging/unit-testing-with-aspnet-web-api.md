---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: ASP.NET Web API 2 de pruebas unitarias | Microsoft Docs
author: tfitzmac
description: Esta guía y la aplicación muestran cómo crear pruebas unitarias simple para la aplicación Web API 2. Este tutorial muestra cómo incluir un proj de prueba unitaria...
ms.author: riande
ms.date: 06/05/2014
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 0a3f9ddde725995359de85c5e0783106f5de02bd
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838011"
---
<a name="unit-testing-aspnet-web-api-2"></a><span data-ttu-id="387e1-104">ASP.NET Web API 2 de pruebas unitarias</span><span class="sxs-lookup"><span data-stu-id="387e1-104">Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="387e1-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="387e1-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="387e1-106">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="387e1-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> <span data-ttu-id="387e1-107">Esta guía y la aplicación muestran cómo crear pruebas unitarias simple para la aplicación Web API 2.</span><span class="sxs-lookup"><span data-stu-id="387e1-107">This guidance and application demonstrate how to create simple unit tests for your Web API 2 application.</span></span> <span data-ttu-id="387e1-108">Este tutorial muestra cómo incluir un proyecto de prueba unitaria en la solución y escribir métodos de prueba que comprueban los valores devueltos desde un método de controlador.</span><span class="sxs-lookup"><span data-stu-id="387e1-108">This tutorial shows how to include a unit test project in your solution, and write test methods that check the returned values from a controller method.</span></span>
> 
> <span data-ttu-id="387e1-109">En este tutorial se da por supuesto que está familiarizado con los conceptos básicos de ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="387e1-109">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="387e1-110">Para ver un tutorial introductorio, vea [Introducción a ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="387e1-110">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
> 
> <span data-ttu-id="387e1-111">Las pruebas unitarias en este tema se limita deliberadamente a escenarios de datos simple.</span><span class="sxs-lookup"><span data-stu-id="387e1-111">The unit tests in this topic are intentionally limited to simple data scenarios.</span></span> <span data-ttu-id="387e1-112">Para escenarios más avanzados de datos de pruebas unitarias, vea [simulación Entity Framework cuando unidad pruebas ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="387e1-112">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="387e1-113">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="387e1-113">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="387e1-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="387e1-114">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="387e1-115">Web API 2</span><span class="sxs-lookup"><span data-stu-id="387e1-115">Web API 2</span></span>


## <a name="in-this-topic"></a><span data-ttu-id="387e1-116">En este tema</span><span class="sxs-lookup"><span data-stu-id="387e1-116">In this topic</span></span>

<span data-ttu-id="387e1-117">Este tema contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="387e1-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="387e1-118">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="387e1-118">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="387e1-119">Descargue el código</span><span class="sxs-lookup"><span data-stu-id="387e1-119">Download code</span></span>](#download)
- [<span data-ttu-id="387e1-120">Crear la aplicación con el proyecto de prueba unitaria</span><span class="sxs-lookup"><span data-stu-id="387e1-120">Create application with unit test project</span></span>](#appwithunittest)

    - [<span data-ttu-id="387e1-121">Agregar proyecto de prueba unitaria al crear la aplicación</span><span class="sxs-lookup"><span data-stu-id="387e1-121">Add unit test project when creating the application</span></span>](#whencreate)
    - [<span data-ttu-id="387e1-122">Agregar proyecto de prueba unitaria a una aplicación existente</span><span class="sxs-lookup"><span data-stu-id="387e1-122">Add unit test project to an existing application</span></span>](#addtoexisting)
- [<span data-ttu-id="387e1-123">Configurar la aplicación Web API 2</span><span class="sxs-lookup"><span data-stu-id="387e1-123">Set up the Web API 2 application</span></span>](#setupproject)
- [<span data-ttu-id="387e1-124">Instalar paquetes de NuGet en el proyecto de prueba</span><span class="sxs-lookup"><span data-stu-id="387e1-124">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="387e1-125">Crear pruebas</span><span class="sxs-lookup"><span data-stu-id="387e1-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="387e1-126">Ejecutar pruebas</span><span class="sxs-lookup"><span data-stu-id="387e1-126">Run tests</span></span>](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="387e1-127">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="387e1-127">Prerequisites</span></span>

<span data-ttu-id="387e1-128">Visual Studio 2017 Community, Professional o Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="387e1-128">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="387e1-129">Descargue el código</span><span class="sxs-lookup"><span data-stu-id="387e1-129">Download code</span></span>

<span data-ttu-id="387e1-130">Descargue el [proyecto completado](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="387e1-130">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="387e1-131">El proyecto descargable incluye código de prueba de unidad para este tema y la [simulación Entity Framework cuando ASP.NET Web API de pruebas de unidad](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) tema.</span><span class="sxs-lookup"><span data-stu-id="387e1-131">The downloadable project includes unit test code for this topic and for the [Mocking Entity Framework when Unit Testing ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="387e1-132">Crear la aplicación con el proyecto de prueba unitaria</span><span class="sxs-lookup"><span data-stu-id="387e1-132">Create application with unit test project</span></span>

<span data-ttu-id="387e1-133">Puede crear un proyecto de prueba unitaria al crear la aplicación o agregar un proyecto de prueba unitaria a una aplicación existente.</span><span class="sxs-lookup"><span data-stu-id="387e1-133">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="387e1-134">En este tutorial se muestra ambos métodos para crear un proyecto de prueba unitaria.</span><span class="sxs-lookup"><span data-stu-id="387e1-134">This tutorial shows both methods for creating a unit test project.</span></span> <span data-ttu-id="387e1-135">Para seguir este tutorial, puede utilizar cualquier enfoque.</span><span class="sxs-lookup"><span data-stu-id="387e1-135">To follow this tutorial, you can use either approach.</span></span>

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a><span data-ttu-id="387e1-136">Agregar proyecto de prueba unitaria al crear la aplicación</span><span class="sxs-lookup"><span data-stu-id="387e1-136">Add unit test project when creating the application</span></span>

<span data-ttu-id="387e1-137">Crear una nueva aplicación Web de ASP.NET denominada **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="387e1-137">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

![Crear proyecto](unit-testing-with-aspnet-web-api/_static/image1.png)

<span data-ttu-id="387e1-139">En las ventanas de nuevo proyecto ASP.NET, seleccione la **vacía** plantilla y agregar carpetas y referencias centrales para API Web.</span><span class="sxs-lookup"><span data-stu-id="387e1-139">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="387e1-140">Seleccione el **agregar pruebas unitarias** opción.</span><span class="sxs-lookup"><span data-stu-id="387e1-140">Select the **Add unit tests** option.</span></span> <span data-ttu-id="387e1-141">El proyecto de prueba unitaria se denomina automáticamente **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="387e1-141">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="387e1-142">Puede conservar este nombre.</span><span class="sxs-lookup"><span data-stu-id="387e1-142">You can keep this name.</span></span>

![Crear proyecto de prueba unitaria](unit-testing-with-aspnet-web-api/_static/image2.png)

<span data-ttu-id="387e1-144">Después de crear la aplicación, verá que contiene dos proyectos.</span><span class="sxs-lookup"><span data-stu-id="387e1-144">After creating the application, you will see it contains two projects.</span></span>

![dos proyectos](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a><span data-ttu-id="387e1-146">Agregar proyecto de prueba unitaria a una aplicación existente</span><span class="sxs-lookup"><span data-stu-id="387e1-146">Add unit test project to an existing application</span></span>

<span data-ttu-id="387e1-147">Si no ha creado el proyecto de prueba unitaria cuando creó la aplicación, puede agregar uno en cualquier momento.</span><span class="sxs-lookup"><span data-stu-id="387e1-147">If you did not create the unit test project when you created your application, you can add one at any time.</span></span> <span data-ttu-id="387e1-148">Por ejemplo, supongamos que ya tiene una aplicación denominada StoreApp, y desea agregar pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="387e1-148">For example, suppose you already have an application named StoreApp, and you want to add unit tests.</span></span> <span data-ttu-id="387e1-149">Para agregar un proyecto de prueba unitaria, haga clic en la solución y seleccione **agregar** y **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="387e1-149">To add a unit test project, right-click your solution and select **Add** and **New Project**.</span></span>

![Agregar nuevo proyecto a la solución](unit-testing-with-aspnet-web-api/_static/image4.png)

<span data-ttu-id="387e1-151">Seleccione **prueba** en el panel izquierdo y seleccione **proyecto de prueba unitaria** para el tipo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="387e1-151">Select **Test** in the left pane, and select **Unit Test Project** for the project type.</span></span> <span data-ttu-id="387e1-152">Denomine el proyecto **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="387e1-152">Name the project **StoreApp.Tests**.</span></span>

![Agregar proyecto de prueba unitaria](unit-testing-with-aspnet-web-api/_static/image5.png)

<span data-ttu-id="387e1-154">Verá el proyecto de prueba unitaria de la solución.</span><span class="sxs-lookup"><span data-stu-id="387e1-154">You will see the unit test project in your solution.</span></span>

<span data-ttu-id="387e1-155">En el proyecto de prueba unitaria, agregue una referencia de proyecto al proyecto original.</span><span class="sxs-lookup"><span data-stu-id="387e1-155">In the unit test project, add a project reference to the original project.</span></span>

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a><span data-ttu-id="387e1-156">Configurar la aplicación Web API 2</span><span class="sxs-lookup"><span data-stu-id="387e1-156">Set up the Web API 2 application</span></span>

<span data-ttu-id="387e1-157">En el proyecto StoreApp, agregue un archivo de clase para el **modelos** carpeta denominada **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="387e1-157">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="387e1-158">Reemplace el contenido del archivo con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="387e1-158">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="387e1-159">Compile la solución.</span><span class="sxs-lookup"><span data-stu-id="387e1-159">Build the solution.</span></span>

<span data-ttu-id="387e1-160">Haga clic en la carpeta Controllers y seleccione **agregar** y **nuevo elemento de scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="387e1-160">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="387e1-161">Seleccione **controlador - Web API 2 blanco**.</span><span class="sxs-lookup"><span data-stu-id="387e1-161">Select **Web API 2 Controller - Empty**.</span></span>

![Agregar nuevo controlador](unit-testing-with-aspnet-web-api/_static/image6.png)

<span data-ttu-id="387e1-163">Establece el nombre del controlador en **SimpleProductController**y haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="387e1-163">Set the controller name to **SimpleProductController**, and click **Add**.</span></span>

![Especifique el controlador](unit-testing-with-aspnet-web-api/_static/image7.png)

<span data-ttu-id="387e1-165">Reemplace el código existente por el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="387e1-165">Replace the existing code with the following code.</span></span> <span data-ttu-id="387e1-166">Para simplificar este ejemplo, los datos se almacenan en una lista en lugar de una base de datos.</span><span class="sxs-lookup"><span data-stu-id="387e1-166">To simplify this example, the data is stored in a list rather than a database.</span></span> <span data-ttu-id="387e1-167">La lista definida en esta clase representa los datos de producción.</span><span class="sxs-lookup"><span data-stu-id="387e1-167">The list defined in this class represents the production data.</span></span> <span data-ttu-id="387e1-168">Tenga en cuenta que el controlador incluye un constructor que toma como parámetro una lista de objetos Product.</span><span class="sxs-lookup"><span data-stu-id="387e1-168">Notice that the controller includes a constructor that takes as a parameter a list of Product objects.</span></span> <span data-ttu-id="387e1-169">Este constructor le permite pasar datos de prueba cuando las pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="387e1-169">This constructor enables you to pass test data when unit testing.</span></span> <span data-ttu-id="387e1-170">El controlador también incluye dos **async** métodos para ilustrar los métodos asincrónicos de pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="387e1-170">The controller also includes two **async** methods to illustrate unit testing asynchronous methods.</span></span> <span data-ttu-id="387e1-171">Estos métodos asincrónicos se implementan mediante una llamada a **Task.FromResult** minimizar el código extraño, pero normalmente los métodos incluiría las operaciones que consumen muchos recursos.</span><span class="sxs-lookup"><span data-stu-id="387e1-171">These async methods were implemented by calling **Task.FromResult** to minimize extraneous code, but normally the methods would include resource-intensive operations.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="387e1-172">El método GetProduct devuelve una instancia de la **IHttpActionResult** interfaz.</span><span class="sxs-lookup"><span data-stu-id="387e1-172">The GetProduct method returns an instance of the **IHttpActionResult** interface.</span></span> <span data-ttu-id="387e1-173">IHttpActionResult es una de las nuevas características de Web API 2, y simplifica el desarrollo de pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="387e1-173">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span> <span data-ttu-id="387e1-174">Las clases que implementan la interfaz IHttpActionResult se encuentran en el [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="387e1-174">Classes that implement the IHttpActionResult interface are found in the [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) namespace.</span></span> <span data-ttu-id="387e1-175">Estas clases representan las posibles respuestas de una solicitud de acción y corresponden a los códigos de estado HTTP.</span><span class="sxs-lookup"><span data-stu-id="387e1-175">These classes represent possible responses from an action request, and they correspond to HTTP status codes.</span></span>

<span data-ttu-id="387e1-176">Compile la solución.</span><span class="sxs-lookup"><span data-stu-id="387e1-176">Build the solution.</span></span>

<span data-ttu-id="387e1-177">Ahora está listo para configurar el proyecto de prueba.</span><span class="sxs-lookup"><span data-stu-id="387e1-177">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="387e1-178">Instalar paquetes de NuGet en el proyecto de prueba</span><span class="sxs-lookup"><span data-stu-id="387e1-178">Install NuGet packages in test project</span></span>

<span data-ttu-id="387e1-179">Cuando usas la plantilla vacía para crear una aplicación, el proyecto de prueba unitaria (StoreApp.Tests) no incluye los paquetes de NuGet instalados.</span><span class="sxs-lookup"><span data-stu-id="387e1-179">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="387e1-180">Otras plantillas, como la plantilla Web API, incluyen algunos paquetes de NuGet en el proyecto de prueba unitaria.</span><span class="sxs-lookup"><span data-stu-id="387e1-180">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="387e1-181">Para este tutorial, debe incluir el paquete de Microsoft ASP.NET Web API 2 Core al proyecto de prueba.</span><span class="sxs-lookup"><span data-stu-id="387e1-181">For this tutorial, you must include the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="387e1-182">Haga clic en el proyecto StoreApp.Tests y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="387e1-182">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="387e1-183">Debe seleccionar el proyecto StoreApp.Tests para agregar los paquetes a ese proyecto.</span><span class="sxs-lookup"><span data-stu-id="387e1-183">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![administración de paquetes](unit-testing-with-aspnet-web-api/_static/image8.png)

<span data-ttu-id="387e1-185">Buscar e instalar el paquete de Microsoft ASP.NET Web API 2 núcleos.</span><span class="sxs-lookup"><span data-stu-id="387e1-185">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![instalar el paquete de core web api](unit-testing-with-aspnet-web-api/_static/image9.png)

<span data-ttu-id="387e1-187">Cierre la ventana Administrar paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="387e1-187">Close the Manage NuGet Packages window.</span></span>

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="387e1-188">Crear pruebas</span><span class="sxs-lookup"><span data-stu-id="387e1-188">Create tests</span></span>

<span data-ttu-id="387e1-189">De forma predeterminada, el proyecto de prueba incluye un archivo de prueba vacío denominado UnitTest1.cs.</span><span class="sxs-lookup"><span data-stu-id="387e1-189">By default, your test project includes an empty test file named UnitTest1.cs.</span></span> <span data-ttu-id="387e1-190">Este archivo muestra los atributos que se utiliza para crear métodos de prueba.</span><span class="sxs-lookup"><span data-stu-id="387e1-190">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="387e1-191">Para las pruebas unitarias, puede usar este archivo o crear su propio archivo.</span><span class="sxs-lookup"><span data-stu-id="387e1-191">For your unit tests, you can either use this file or create your own file.</span></span>

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

<span data-ttu-id="387e1-193">Para este tutorial, creará su propia clase de prueba.</span><span class="sxs-lookup"><span data-stu-id="387e1-193">For this tutorial, you will create your own test class.</span></span> <span data-ttu-id="387e1-194">Puede eliminar el archivo UnitTest1.cs.</span><span class="sxs-lookup"><span data-stu-id="387e1-194">You can delete the UnitTest1.cs file.</span></span> <span data-ttu-id="387e1-195">Agregue una clase denominada **TestSimpleProductController.cs**y reemplace el código con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="387e1-195">Add a class named **TestSimpleProductController.cs**, and replace the code with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="387e1-196">Ejecutar pruebas</span><span class="sxs-lookup"><span data-stu-id="387e1-196">Run tests</span></span>

<span data-ttu-id="387e1-197">Ahora está listo para ejecutar las pruebas.</span><span class="sxs-lookup"><span data-stu-id="387e1-197">You are now ready to run the tests.</span></span> <span data-ttu-id="387e1-198">Todo el método que se marcan con el **TestMethod** se probará el atributo.</span><span class="sxs-lookup"><span data-stu-id="387e1-198">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="387e1-199">Desde el **prueba** elemento de menú, ejecutar las pruebas.</span><span class="sxs-lookup"><span data-stu-id="387e1-199">From the **Test** menu item, run the tests.</span></span>

![ejecución de pruebas](unit-testing-with-aspnet-web-api/_static/image11.png)

<span data-ttu-id="387e1-201">Abra el **Explorador de pruebas** ventana y observe los resultados de las pruebas.</span><span class="sxs-lookup"><span data-stu-id="387e1-201">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![resultados de pruebas](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a><span data-ttu-id="387e1-203">Resumen</span><span class="sxs-lookup"><span data-stu-id="387e1-203">Summary</span></span>

<span data-ttu-id="387e1-204">Ha completado este tutorial.</span><span class="sxs-lookup"><span data-stu-id="387e1-204">You have completed this tutorial.</span></span> <span data-ttu-id="387e1-205">Los datos en este tutorial se ha simplificado intencionadamente para centrarse en condiciones de prueba unitaria.</span><span class="sxs-lookup"><span data-stu-id="387e1-205">The data in this tutorial was intentionally simplified to focus on unit testing conditions.</span></span> <span data-ttu-id="387e1-206">Para escenarios más avanzados de datos de pruebas unitarias, vea [simulación Entity Framework cuando unidad pruebas ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="387e1-206">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
