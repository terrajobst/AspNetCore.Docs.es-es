---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: "Simulación de Entity Framework cuando ASP.NET Web API 2 de pruebas unitarias | Documentos de Microsoft"
author: tfitzmac
description: "Esta guía y la aplicación muestran cómo crear pruebas unitarias para la aplicación de API Web 2 que usa Entity Framework. Muestra cómo modificar el..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: abfde7edec85812de3560f4edefb110c3e374580
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2018
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a><span data-ttu-id="736df-104">Simulación de Entity Framework cuando ASP.NET Web API 2 de pruebas unitarias</span><span class="sxs-lookup"><span data-stu-id="736df-104">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="736df-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="736df-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="736df-106">Descargar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="736df-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> <span data-ttu-id="736df-107">Esta guía y la aplicación muestran cómo crear pruebas unitarias para la aplicación de API Web 2 que usa Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="736df-107">This guidance and application demonstrate how to create unit tests for your Web API 2 application that uses the Entity Framework.</span></span> <span data-ttu-id="736df-108">Muestra cómo modificar el controlador con scaffolding para permitir pasar un objeto de contexto para las pruebas y cómo crear objetos de prueba que funcionan con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="736df-108">It shows how to modify the scaffolded controller to enable passing a context object for testing, and how to create test objects that work with Entity Framework.</span></span>
> 
> <span data-ttu-id="736df-109">Para obtener una introducción a las pruebas unitarias con ASP.NET Web API, consulte [pruebas unitarias con ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="736df-109">For an introduction to unit testing with ASP.NET Web API, see [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span></span>
> 
> <span data-ttu-id="736df-110">Este tutorial se da por supuesto que está familiarizado con los conceptos básicos de ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="736df-110">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="736df-111">Para un tutorial introductorio, vea [Introducción a ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="736df-111">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="736df-112">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="736df-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="736df-113">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="736df-113">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="736df-114">Web API 2</span><span class="sxs-lookup"><span data-stu-id="736df-114">Web API 2</span></span>


## <a name="in-this-topic"></a><span data-ttu-id="736df-115">En este tema</span><span class="sxs-lookup"><span data-stu-id="736df-115">In this topic</span></span>

<span data-ttu-id="736df-116">Este tema contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="736df-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="736df-117">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="736df-117">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="736df-118">Descargar código</span><span class="sxs-lookup"><span data-stu-id="736df-118">Download code</span></span>](#download)
- [<span data-ttu-id="736df-119">Crear aplicaciones con el proyecto de prueba unitaria</span><span class="sxs-lookup"><span data-stu-id="736df-119">Create application with unit test project</span></span>](#appwithunittest)
- [<span data-ttu-id="736df-120">Crear la clase del modelo</span><span class="sxs-lookup"><span data-stu-id="736df-120">Create the model class</span></span>](#modelclass)
- [<span data-ttu-id="736df-121">Agregar el controlador</span><span class="sxs-lookup"><span data-stu-id="736df-121">Add the controller</span></span>](#controller)
- [<span data-ttu-id="736df-122">Agregar la inyección de dependencia</span><span class="sxs-lookup"><span data-stu-id="736df-122">Add dependency injection</span></span>](#dependency)
- [<span data-ttu-id="736df-123">Instalar paquetes de NuGet en el proyecto de prueba</span><span class="sxs-lookup"><span data-stu-id="736df-123">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="736df-124">Crear el contexto de prueba</span><span class="sxs-lookup"><span data-stu-id="736df-124">Create test context</span></span>](#testcontext)
- [<span data-ttu-id="736df-125">Crear pruebas</span><span class="sxs-lookup"><span data-stu-id="736df-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="736df-126">Ejecutar pruebas</span><span class="sxs-lookup"><span data-stu-id="736df-126">Run tests</span></span>](#runtests)

<span data-ttu-id="736df-127">Si ya ha completado los pasos descritos en [pruebas unitarias con ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), puede ir a la sección [agregar el controlador](#controller).</span><span class="sxs-lookup"><span data-stu-id="736df-127">If you have already completed the steps in [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), you can skip to the section [Add the controller](#controller).</span></span>

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="736df-128">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="736df-128">Prerequisites</span></span>

<span data-ttu-id="736df-129">Visual Studio 2017 Community, Professional o Enterprise.</span><span class="sxs-lookup"><span data-stu-id="736df-129">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="736df-130">Descargar código</span><span class="sxs-lookup"><span data-stu-id="736df-130">Download code</span></span>

<span data-ttu-id="736df-131">Descargue el [proyecto completado](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="736df-131">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="736df-132">El proyecto descargable incluye código de prueba de unidad de este tema y la [unidad pruebas ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) tema.</span><span class="sxs-lookup"><span data-stu-id="736df-132">The downloadable project includes unit test code for this topic and for the [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="736df-133">Crear aplicaciones con el proyecto de prueba unitaria</span><span class="sxs-lookup"><span data-stu-id="736df-133">Create application with unit test project</span></span>

<span data-ttu-id="736df-134">Puede crear un proyecto de prueba unitaria al crear la aplicación o agregar un proyecto de prueba unitaria a una aplicación existente.</span><span class="sxs-lookup"><span data-stu-id="736df-134">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="736df-135">Este tutorial muestra cómo crear un proyecto de prueba unitaria al crear la aplicación.</span><span class="sxs-lookup"><span data-stu-id="736df-135">This tutorial shows creating a unit test project when creating the application.</span></span>

<span data-ttu-id="736df-136">Crear una nueva aplicación Web de ASP.NET denominada **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="736df-136">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

<span data-ttu-id="736df-137">En la ventana nuevo proyecto ASP.NET, seleccione la **vacía** plantilla y agregar carpetas y principales referencias de API Web.</span><span class="sxs-lookup"><span data-stu-id="736df-137">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="736df-138">Seleccione el **agregar pruebas unitarias** opción.</span><span class="sxs-lookup"><span data-stu-id="736df-138">Select the **Add unit tests** option.</span></span> <span data-ttu-id="736df-139">El proyecto de prueba unitaria se denomina automáticamente **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="736df-139">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="736df-140">Puede conservar este nombre.</span><span class="sxs-lookup"><span data-stu-id="736df-140">You can keep this name.</span></span>

![Crear proyecto de prueba unitaria](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

<span data-ttu-id="736df-142">Después de crear la aplicación, verá contiene dos proyectos: **StoreApp** y **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="736df-142">After creating the application, you will see it contains two projects - **StoreApp** and **StoreApp.Tests**.</span></span>

<a id="modelclass"></a>
## <a name="create-the-model-class"></a><span data-ttu-id="736df-143">Crear la clase del modelo</span><span class="sxs-lookup"><span data-stu-id="736df-143">Create the model class</span></span>

<span data-ttu-id="736df-144">En el proyecto StoreApp, agregue un archivo de clase para la **modelos** carpeta denominada **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="736df-144">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="736df-145">Reemplace el contenido del archivo con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="736df-145">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

<span data-ttu-id="736df-146">Compile la solución.</span><span class="sxs-lookup"><span data-stu-id="736df-146">Build the solution.</span></span>

<a id="controller"></a>
## <a name="add-the-controller"></a><span data-ttu-id="736df-147">Agregar el controlador</span><span class="sxs-lookup"><span data-stu-id="736df-147">Add the controller</span></span>

<span data-ttu-id="736df-148">Haga clic en la carpeta Controllers y seleccione **agregar** y **nuevo elemento de scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="736df-148">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="736df-149">Seleccione el controlador de Web API 2 con acciones que usan Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="736df-149">Select Web API 2 Controller with actions, using Entity Framework.</span></span>

![Agregar nuevo controlador](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

<span data-ttu-id="736df-151">Establezca los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="736df-151">Set the following values:</span></span>

- <span data-ttu-id="736df-152">Nombre del controlador: **ProductController**</span><span class="sxs-lookup"><span data-stu-id="736df-152">Controller name: **ProductController**</span></span>
- <span data-ttu-id="736df-153">Clase de modelo: **producto**</span><span class="sxs-lookup"><span data-stu-id="736df-153">Model class: **Product**</span></span>
- <span data-ttu-id="736df-154">Clase de contexto de datos: [seleccionar **nuevo contexto de datos** botón que rellena los valores que se muestra a continuación]</span><span class="sxs-lookup"><span data-stu-id="736df-154">Data context class: [Select **New data context** button which fills in the values seen below]</span></span>

![especificar controlador](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

<span data-ttu-id="736df-156">Haga clic en **agregar** para crear el controlador con el código generado automáticamente.</span><span class="sxs-lookup"><span data-stu-id="736df-156">Click **Add** to create the controller with automatically-generated code.</span></span> <span data-ttu-id="736df-157">El código incluye métodos para crear, recuperar, actualizar y eliminar instancias de la clase de producto.</span><span class="sxs-lookup"><span data-stu-id="736df-157">The code includes methods for creating, retrieving, updating and deleting instances of the Product class.</span></span> <span data-ttu-id="736df-158">El código siguiente muestra el método para agregar un producto.</span><span class="sxs-lookup"><span data-stu-id="736df-158">The following code shows the method for add a Product.</span></span> <span data-ttu-id="736df-159">Tenga en cuenta que el método devuelve una instancia de **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="736df-159">Notice that the method returns an instance of **IHttpActionResult**.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

<span data-ttu-id="736df-160">IHttpActionResult es una de las nuevas características de API Web 2, y simplifica el desarrollo de pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="736df-160">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span>

<span data-ttu-id="736df-161">En la sección siguiente, va a personalizar el código generado para facilitar la transferencia de objetos de prueba al controlador.</span><span class="sxs-lookup"><span data-stu-id="736df-161">In the next section, you will customize the generated code to facilitate passing test objects to the controller.</span></span>

<a id="dependency"></a>
## <a name="add-dependency-injection"></a><span data-ttu-id="736df-162">Agregar la inyección de dependencia</span><span class="sxs-lookup"><span data-stu-id="736df-162">Add dependency injection</span></span>

<span data-ttu-id="736df-163">Actualmente, la clase ProductController está codificado de forma rígida para utilizar una instancia de la clase StoreAppContext.</span><span class="sxs-lookup"><span data-stu-id="736df-163">Currently, the ProductController class is hard-coded to use an instance of the StoreAppContext class.</span></span> <span data-ttu-id="736df-164">Utilizará un patrón que se denomina inyección de dependencia para modificar la aplicación y quitar esa dependencia codificada de forma rígida.</span><span class="sxs-lookup"><span data-stu-id="736df-164">You will use a pattern called dependency injection to modify your application and remove that hard-coded dependency.</span></span> <span data-ttu-id="736df-165">Al romper con esta dependencia, puede pasar en un objeto ficticio al probar.</span><span class="sxs-lookup"><span data-stu-id="736df-165">By breaking this dependency, you can pass in a mock object when testing.</span></span>

<span data-ttu-id="736df-166">Haga clic en el **modelos** carpeta y agregue una nueva interfaz denominada **IStoreAppContext**.</span><span class="sxs-lookup"><span data-stu-id="736df-166">Right-click the **Models** folder, and add a new interface named **IStoreAppContext**.</span></span>

<span data-ttu-id="736df-167">Reemplace el código por el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="736df-167">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

<span data-ttu-id="736df-168">Abra el archivo StoreAppContext.cs y realizar los siguientes cambios resaltados.</span><span class="sxs-lookup"><span data-stu-id="736df-168">Open the StoreAppContext.cs file and make the following highlighted changes.</span></span> <span data-ttu-id="736df-169">Los cambios importantes a tener en cuenta son:</span><span class="sxs-lookup"><span data-stu-id="736df-169">The important changes to note are:</span></span>

- <span data-ttu-id="736df-170">Clase StoreAppContext implementa la interfaz de IStoreAppContext</span><span class="sxs-lookup"><span data-stu-id="736df-170">StoreAppContext class implements IStoreAppContext interface</span></span>
- <span data-ttu-id="736df-171">Se implementa MarkAsModified (método)</span><span class="sxs-lookup"><span data-stu-id="736df-171">MarkAsModified method is implemented</span></span>


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

<span data-ttu-id="736df-172">Abra el archivo ProductController.cs.</span><span class="sxs-lookup"><span data-stu-id="736df-172">Open the ProductController.cs file.</span></span> <span data-ttu-id="736df-173">Cambie el código existente para que coincida con el código que aparece resaltado.</span><span class="sxs-lookup"><span data-stu-id="736df-173">Change the existing code to match the highlighted code.</span></span> <span data-ttu-id="736df-174">Estos cambios interrumpa la dependencia en StoreAppContext y permiten a otras clases pasar un objeto diferente de la clase de contexto.</span><span class="sxs-lookup"><span data-stu-id="736df-174">These changes break the dependency on StoreAppContext and enable other classes to pass in a different object for the context class.</span></span> <span data-ttu-id="736df-175">Este cambio le permitirá pasar en un contexto de prueba durante las pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="736df-175">This change will enable you to pass in a test context during unit tests.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

<span data-ttu-id="736df-176">Hay un cambio más que debe realizar en ProductController.</span><span class="sxs-lookup"><span data-stu-id="736df-176">There is one more change you must make in ProductController.</span></span> <span data-ttu-id="736df-177">En el **PutProduct**  /método siguiente, reemplace la línea que establece el estado de entidad para modifica con una llamada al método MarkAsModified.</span><span class="sxs-lookup"><span data-stu-id="736df-177">In the **PutProduct** method, replace the line that sets the entity state to modified with a call to the MarkAsModified method.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

<span data-ttu-id="736df-178">Compile la solución.</span><span class="sxs-lookup"><span data-stu-id="736df-178">Build the solution.</span></span>

<span data-ttu-id="736df-179">Ahora está listo para configurar el proyecto de prueba.</span><span class="sxs-lookup"><span data-stu-id="736df-179">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="736df-180">Instalar paquetes de NuGet en el proyecto de prueba</span><span class="sxs-lookup"><span data-stu-id="736df-180">Install NuGet packages in test project</span></span>

<span data-ttu-id="736df-181">Cuando usas la plantilla vacía para crear una aplicación, el proyecto de prueba unitaria (StoreApp.Tests) no incluye los paquetes de NuGet instalados.</span><span class="sxs-lookup"><span data-stu-id="736df-181">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="736df-182">Otras plantillas, como la plantilla API Web, incluyen algunos paquetes de NuGet en el proyecto de prueba unitaria.</span><span class="sxs-lookup"><span data-stu-id="736df-182">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="736df-183">Para este tutorial, debe incluir el paquete de Entity Framework y el paquete de Microsoft ASP.NET Web API 2 principal para el proyecto de prueba.</span><span class="sxs-lookup"><span data-stu-id="736df-183">For this tutorial, you must include the Entity Framework packge and the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="736df-184">Haga clic en el proyecto StoreApp.Tests y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="736df-184">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="736df-185">Debe seleccionar el proyecto StoreApp.Tests para agregar los paquetes a ese proyecto.</span><span class="sxs-lookup"><span data-stu-id="736df-185">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![Administrar paquetes](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

<span data-ttu-id="736df-187">De los paquetes en línea, buscar e instalar el paquete de Entity Framework (versión 6.0 o posterior).</span><span class="sxs-lookup"><span data-stu-id="736df-187">From the Online packages, find and install the EntityFramework package (version 6.0 or later).</span></span> <span data-ttu-id="736df-188">Si parece que ya está instalado el paquete de Entity Framework, es posible que ha seleccionado el proyecto StoreApp en lugar del proyecto StoreApp.Tests.</span><span class="sxs-lookup"><span data-stu-id="736df-188">If it appears that the EntityFramework package is already installed, you may have selected the StoreApp project instead of the StoreApp.Tests project.</span></span>

![Agregar Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

<span data-ttu-id="736df-190">Buscar e instalar el paquete de Microsoft ASP.NET Web API 2 Core.</span><span class="sxs-lookup"><span data-stu-id="736df-190">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![instalar el paquete de web api core](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

<span data-ttu-id="736df-192">Cierre la ventana Administrar paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="736df-192">Close the Manage NuGet Packages window.</span></span>

<a id="testcontext"></a>
## <a name="create-test-context"></a><span data-ttu-id="736df-193">Crear el contexto de prueba</span><span class="sxs-lookup"><span data-stu-id="736df-193">Create test context</span></span>

<span data-ttu-id="736df-194">Agregue una clase denominada **TestDbSet** al proyecto de prueba.</span><span class="sxs-lookup"><span data-stu-id="736df-194">Add a class named **TestDbSet** to the test project.</span></span> <span data-ttu-id="736df-195">Esta clase actúa como clase base para el conjunto de datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="736df-195">This class serves as the base class for your test data set.</span></span> <span data-ttu-id="736df-196">Reemplace el código por el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="736df-196">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

<span data-ttu-id="736df-197">Agregue una clase denominada **TestProductDbSet** al proyecto de prueba que contiene el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="736df-197">Add a class named **TestProductDbSet** to the test project which contains the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

<span data-ttu-id="736df-198">Agregue una clase denominada **TestStoreAppContext** y reemplace el código existente por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="736df-198">Add a class named **TestStoreAppContext** and replace the existing code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="736df-199">Crear pruebas</span><span class="sxs-lookup"><span data-stu-id="736df-199">Create tests</span></span>

<span data-ttu-id="736df-200">De forma predeterminada, el proyecto de prueba incluye un archivo de prueba vacío denominado **UnitTest1.cs**.</span><span class="sxs-lookup"><span data-stu-id="736df-200">By default, your test project includes an empty test file named **UnitTest1.cs**.</span></span> <span data-ttu-id="736df-201">Este archivo muestra los atributos que se utiliza para crear métodos de prueba.</span><span class="sxs-lookup"><span data-stu-id="736df-201">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="736df-202">Para este tutorial, puede eliminar este archivo porque se agregará una nueva clase de prueba.</span><span class="sxs-lookup"><span data-stu-id="736df-202">For this tutorial, you can delete this file because you will add a new test class.</span></span>

<span data-ttu-id="736df-203">Agregue una clase denominada **TestProductController** al proyecto de prueba.</span><span class="sxs-lookup"><span data-stu-id="736df-203">Add a class named **TestProductController** to the test project.</span></span> <span data-ttu-id="736df-204">Reemplace el código por el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="736df-204">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="736df-205">Ejecutar pruebas</span><span class="sxs-lookup"><span data-stu-id="736df-205">Run tests</span></span>

<span data-ttu-id="736df-206">Ahora está listo para ejecutar las pruebas.</span><span class="sxs-lookup"><span data-stu-id="736df-206">You are now ready to run the tests.</span></span> <span data-ttu-id="736df-207">Todos los del método que se marcan con la **TestMethod** se probará el atributo.</span><span class="sxs-lookup"><span data-stu-id="736df-207">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="736df-208">Desde el **prueba** elemento de menú, ejecutar las pruebas.</span><span class="sxs-lookup"><span data-stu-id="736df-208">From the **Test** menu item, run the tests.</span></span>

![ejecución de pruebas](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

<span data-ttu-id="736df-210">Abra la **Explorador de pruebas** ventana y observe los resultados de las pruebas.</span><span class="sxs-lookup"><span data-stu-id="736df-210">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![resultados de pruebas](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
