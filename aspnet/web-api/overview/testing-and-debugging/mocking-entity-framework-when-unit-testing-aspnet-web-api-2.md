---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Simular Entity Framework cuando ASP.NET Web API 2 de pruebas unitarias | Microsoft Docs
author: Rick-Anderson
description: Esta guía y la aplicación muestran cómo crear pruebas unitarias para la aplicación Web API 2 que utiliza Entity Framework. Muestra cómo modificar el...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: f1799b3f9d698053c397e57da3f33ff900ec4013
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021032"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a><span data-ttu-id="3de2f-104">Simular Entity Framework cuando ASP.NET Web API 2 de pruebas unitarias</span><span class="sxs-lookup"><span data-stu-id="3de2f-104">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="3de2f-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3de2f-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="3de2f-106">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="3de2f-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> <span data-ttu-id="3de2f-107">Esta guía y la aplicación muestran cómo crear pruebas unitarias para la aplicación Web API 2 que utiliza Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3de2f-107">This guidance and application demonstrate how to create unit tests for your Web API 2 application that uses the Entity Framework.</span></span> <span data-ttu-id="3de2f-108">Muestra cómo modificar el controlador con scaffold para permitir pasar un objeto de contexto para las pruebas y cómo crear objetos de prueba que funcionan con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3de2f-108">It shows how to modify the scaffolded controller to enable passing a context object for testing, and how to create test objects that work with Entity Framework.</span></span>
>
> <span data-ttu-id="3de2f-109">Para obtener una introducción a las pruebas unitarias con ASP.NET Web API, consulte [Unit Testing con ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="3de2f-109">For an introduction to unit testing with ASP.NET Web API, see [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span></span>
>
> <span data-ttu-id="3de2f-110">En este tutorial se da por supuesto que está familiarizado con los conceptos básicos de ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="3de2f-110">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="3de2f-111">Para ver un tutorial introductorio, vea [Introducción a ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="3de2f-111">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3de2f-112">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="3de2f-112">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="3de2f-113">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="3de2f-113">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="3de2f-114">Web API 2</span><span class="sxs-lookup"><span data-stu-id="3de2f-114">Web API 2</span></span>

## <a name="in-this-topic"></a><span data-ttu-id="3de2f-115">En este tema</span><span class="sxs-lookup"><span data-stu-id="3de2f-115">In this topic</span></span>

<span data-ttu-id="3de2f-116">Este tema contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="3de2f-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="3de2f-117">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="3de2f-117">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="3de2f-118">Descargue el código</span><span class="sxs-lookup"><span data-stu-id="3de2f-118">Download code</span></span>](#download)
- [<span data-ttu-id="3de2f-119">Crear la aplicación con el proyecto de prueba unitaria</span><span class="sxs-lookup"><span data-stu-id="3de2f-119">Create application with unit test project</span></span>](#appwithunittest)
- [<span data-ttu-id="3de2f-120">Crear la clase del modelo</span><span class="sxs-lookup"><span data-stu-id="3de2f-120">Create the model class</span></span>](#modelclass)
- [<span data-ttu-id="3de2f-121">Agregar el controlador</span><span class="sxs-lookup"><span data-stu-id="3de2f-121">Add the controller</span></span>](#controller)
- [<span data-ttu-id="3de2f-122">Agregar la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="3de2f-122">Add dependency injection</span></span>](#dependency)
- [<span data-ttu-id="3de2f-123">Instalar paquetes de NuGet en el proyecto de prueba</span><span class="sxs-lookup"><span data-stu-id="3de2f-123">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="3de2f-124">Crear el contexto de la prueba</span><span class="sxs-lookup"><span data-stu-id="3de2f-124">Create test context</span></span>](#testcontext)
- [<span data-ttu-id="3de2f-125">Crear pruebas</span><span class="sxs-lookup"><span data-stu-id="3de2f-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="3de2f-126">Ejecutar pruebas</span><span class="sxs-lookup"><span data-stu-id="3de2f-126">Run tests</span></span>](#runtests)

<span data-ttu-id="3de2f-127">Si ya ha completado los pasos descritos en [Unit Testing con ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), puede ir a la sección [agregar el controlador](#controller).</span><span class="sxs-lookup"><span data-stu-id="3de2f-127">If you have already completed the steps in [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), you can skip to the section [Add the controller](#controller).</span></span>

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="3de2f-128">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="3de2f-128">Prerequisites</span></span>

<span data-ttu-id="3de2f-129">Visual Studio 2017 Community, Professional o Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="3de2f-129">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="3de2f-130">Descargue el código</span><span class="sxs-lookup"><span data-stu-id="3de2f-130">Download code</span></span>

<span data-ttu-id="3de2f-131">Descargue el [proyecto completado](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="3de2f-131">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="3de2f-132">El proyecto descargable incluye código de prueba de unidad para este tema y la [unidad pruebas ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) tema.</span><span class="sxs-lookup"><span data-stu-id="3de2f-132">The downloadable project includes unit test code for this topic and for the [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="3de2f-133">Crear la aplicación con el proyecto de prueba unitaria</span><span class="sxs-lookup"><span data-stu-id="3de2f-133">Create application with unit test project</span></span>

<span data-ttu-id="3de2f-134">Puede crear un proyecto de prueba unitaria al crear la aplicación o agregar un proyecto de prueba unitaria a una aplicación existente.</span><span class="sxs-lookup"><span data-stu-id="3de2f-134">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="3de2f-135">Este tutorial muestra cómo crear un proyecto de prueba unitaria al crear la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3de2f-135">This tutorial shows creating a unit test project when creating the application.</span></span>

<span data-ttu-id="3de2f-136">Crear una nueva aplicación Web de ASP.NET denominada **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="3de2f-136">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

<span data-ttu-id="3de2f-137">En las ventanas de nuevo proyecto ASP.NET, seleccione la **vacía** plantilla y agregar carpetas y referencias centrales para API Web.</span><span class="sxs-lookup"><span data-stu-id="3de2f-137">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="3de2f-138">Seleccione el **agregar pruebas unitarias** opción.</span><span class="sxs-lookup"><span data-stu-id="3de2f-138">Select the **Add unit tests** option.</span></span> <span data-ttu-id="3de2f-139">El proyecto de prueba unitaria se denomina automáticamente **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="3de2f-139">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="3de2f-140">Puede conservar este nombre.</span><span class="sxs-lookup"><span data-stu-id="3de2f-140">You can keep this name.</span></span>

![Crear proyecto de prueba unitaria](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

<span data-ttu-id="3de2f-142">Después de crear la aplicación, verá contiene dos proyectos: **StoreApp** y **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="3de2f-142">After creating the application, you will see it contains two projects - **StoreApp** and **StoreApp.Tests**.</span></span>

<a id="modelclass"></a>
## <a name="create-the-model-class"></a><span data-ttu-id="3de2f-143">Crear la clase del modelo</span><span class="sxs-lookup"><span data-stu-id="3de2f-143">Create the model class</span></span>

<span data-ttu-id="3de2f-144">En el proyecto StoreApp, agregue un archivo de clase para el **modelos** carpeta denominada **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="3de2f-144">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="3de2f-145">Reemplace el contenido del archivo con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="3de2f-145">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

<span data-ttu-id="3de2f-146">Compila la solución.</span><span class="sxs-lookup"><span data-stu-id="3de2f-146">Build the solution.</span></span>

<a id="controller"></a>
## <a name="add-the-controller"></a><span data-ttu-id="3de2f-147">Agregar el controlador</span><span class="sxs-lookup"><span data-stu-id="3de2f-147">Add the controller</span></span>

<span data-ttu-id="3de2f-148">Haga clic en la carpeta Controllers y seleccione **agregar** y **nuevo elemento de scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="3de2f-148">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="3de2f-149">Seleccione el controlador de Web API 2 con acciones que usan Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3de2f-149">Select Web API 2 Controller with actions, using Entity Framework.</span></span>

![Agregar nuevo controlador](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

<span data-ttu-id="3de2f-151">Establezca los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="3de2f-151">Set the following values:</span></span>

- <span data-ttu-id="3de2f-152">Nombre del controlador: **ProductController**</span><span class="sxs-lookup"><span data-stu-id="3de2f-152">Controller name: **ProductController**</span></span>
- <span data-ttu-id="3de2f-153">Clase de modelo: **producto**</span><span class="sxs-lookup"><span data-stu-id="3de2f-153">Model class: **Product**</span></span>
- <span data-ttu-id="3de2f-154">Clase de contexto de datos: [seleccione **nuevo contexto de datos** botón que rellena los valores que se muestra a continuación]</span><span class="sxs-lookup"><span data-stu-id="3de2f-154">Data context class: [Select **New data context** button which fills in the values seen below]</span></span>

![Especifique el controlador](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

<span data-ttu-id="3de2f-156">Haga clic en **agregar** para crear el controlador con el código generado automáticamente.</span><span class="sxs-lookup"><span data-stu-id="3de2f-156">Click **Add** to create the controller with automatically-generated code.</span></span> <span data-ttu-id="3de2f-157">El código incluye métodos para crear, recuperar, actualizar y eliminar instancias de la clase de producto.</span><span class="sxs-lookup"><span data-stu-id="3de2f-157">The code includes methods for creating, retrieving, updating and deleting instances of the Product class.</span></span> <span data-ttu-id="3de2f-158">El código siguiente muestra el método para agregar un producto.</span><span class="sxs-lookup"><span data-stu-id="3de2f-158">The following code shows the method for add a Product.</span></span> <span data-ttu-id="3de2f-159">Tenga en cuenta que el método devuelve una instancia de **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="3de2f-159">Notice that the method returns an instance of **IHttpActionResult**.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

<span data-ttu-id="3de2f-160">IHttpActionResult es una de las nuevas características de Web API 2, y simplifica el desarrollo de pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="3de2f-160">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span>

<span data-ttu-id="3de2f-161">En la sección siguiente, personalizará el código generado para facilitar la transferencia de objetos de prueba al controlador.</span><span class="sxs-lookup"><span data-stu-id="3de2f-161">In the next section, you will customize the generated code to facilitate passing test objects to the controller.</span></span>

<a id="dependency"></a>
## <a name="add-dependency-injection"></a><span data-ttu-id="3de2f-162">Agregar la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="3de2f-162">Add dependency injection</span></span>

<span data-ttu-id="3de2f-163">Actualmente, la clase ProductController está codificado de forma rígida para utilizar una instancia de la clase StoreAppContext.</span><span class="sxs-lookup"><span data-stu-id="3de2f-163">Currently, the ProductController class is hard-coded to use an instance of the StoreAppContext class.</span></span> <span data-ttu-id="3de2f-164">Utilizará un patrón de inserción de dependencias para modificar la aplicación y quitar esa dependencia codificada de forma rígida.</span><span class="sxs-lookup"><span data-stu-id="3de2f-164">You will use a pattern called dependency injection to modify your application and remove that hard-coded dependency.</span></span> <span data-ttu-id="3de2f-165">Al romper esta dependencia, puede pasar al realizar pruebas en un objeto ficticio.</span><span class="sxs-lookup"><span data-stu-id="3de2f-165">By breaking this dependency, you can pass in a mock object when testing.</span></span>

<span data-ttu-id="3de2f-166">Haga clic en el **modelos** carpeta y agregue una nueva interfaz denominada **IStoreAppContext**.</span><span class="sxs-lookup"><span data-stu-id="3de2f-166">Right-click the **Models** folder, and add a new interface named **IStoreAppContext**.</span></span>

<span data-ttu-id="3de2f-167">Reemplace el código por el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="3de2f-167">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

<span data-ttu-id="3de2f-168">Abra el archivo StoreAppContext.cs y realice los siguientes cambios resaltados.</span><span class="sxs-lookup"><span data-stu-id="3de2f-168">Open the StoreAppContext.cs file and make the following highlighted changes.</span></span> <span data-ttu-id="3de2f-169">Los cambios importantes a tener en cuenta son:</span><span class="sxs-lookup"><span data-stu-id="3de2f-169">The important changes to note are:</span></span>

- <span data-ttu-id="3de2f-170">Clase StoreAppContext implementa la interfaz IStoreAppContext</span><span class="sxs-lookup"><span data-stu-id="3de2f-170">StoreAppContext class implements IStoreAppContext interface</span></span>
- <span data-ttu-id="3de2f-171">Se implementa el método MarkAsModified</span><span class="sxs-lookup"><span data-stu-id="3de2f-171">MarkAsModified method is implemented</span></span>


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

<span data-ttu-id="3de2f-172">Abra el archivo ProductController.cs.</span><span class="sxs-lookup"><span data-stu-id="3de2f-172">Open the ProductController.cs file.</span></span> <span data-ttu-id="3de2f-173">Cambie el código existente para que coincida con el código resaltado.</span><span class="sxs-lookup"><span data-stu-id="3de2f-173">Change the existing code to match the highlighted code.</span></span> <span data-ttu-id="3de2f-174">Estos cambios romper la dependencia en StoreAppContext y permiten a otras clases pasar un objeto diferente de la clase de contexto.</span><span class="sxs-lookup"><span data-stu-id="3de2f-174">These changes break the dependency on StoreAppContext and enable other classes to pass in a different object for the context class.</span></span> <span data-ttu-id="3de2f-175">Este cambio permitirá pasar en un contexto de prueba durante las pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="3de2f-175">This change will enable you to pass in a test context during unit tests.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

<span data-ttu-id="3de2f-176">Hay un cambio más que debe realizar en ProductController.</span><span class="sxs-lookup"><span data-stu-id="3de2f-176">There is one more change you must make in ProductController.</span></span> <span data-ttu-id="3de2f-177">En el **PutProduct** método, reemplace la línea que establece el estado de la entidad a se modifica con una llamada al método MarkAsModified.</span><span class="sxs-lookup"><span data-stu-id="3de2f-177">In the **PutProduct** method, replace the line that sets the entity state to modified with a call to the MarkAsModified method.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

<span data-ttu-id="3de2f-178">Compila la solución.</span><span class="sxs-lookup"><span data-stu-id="3de2f-178">Build the solution.</span></span>

<span data-ttu-id="3de2f-179">Ahora está listo para configurar el proyecto de prueba.</span><span class="sxs-lookup"><span data-stu-id="3de2f-179">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="3de2f-180">Instalar paquetes de NuGet en el proyecto de prueba</span><span class="sxs-lookup"><span data-stu-id="3de2f-180">Install NuGet packages in test project</span></span>

<span data-ttu-id="3de2f-181">Cuando usas la plantilla vacía para crear una aplicación, el proyecto de prueba unitaria (StoreApp.Tests) no incluye los paquetes de NuGet instalados.</span><span class="sxs-lookup"><span data-stu-id="3de2f-181">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="3de2f-182">Otras plantillas, como la plantilla Web API, incluyen algunos paquetes de NuGet en el proyecto de prueba unitaria.</span><span class="sxs-lookup"><span data-stu-id="3de2f-182">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="3de2f-183">Para este tutorial, debe incluir el paquete de Entity Framework y el paquete de Microsoft ASP.NET Web API 2 Core al proyecto de prueba.</span><span class="sxs-lookup"><span data-stu-id="3de2f-183">For this tutorial, you must include the Entity Framework packge and the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="3de2f-184">Haga clic en el proyecto StoreApp.Tests y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3de2f-184">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="3de2f-185">Debe seleccionar el proyecto StoreApp.Tests para agregar los paquetes a ese proyecto.</span><span class="sxs-lookup"><span data-stu-id="3de2f-185">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![administración de paquetes](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

<span data-ttu-id="3de2f-187">De los paquetes en línea, buscar e instalar el paquete de Entity Framework (versión 6.0 o posterior).</span><span class="sxs-lookup"><span data-stu-id="3de2f-187">From the Online packages, find and install the EntityFramework package (version 6.0 or later).</span></span> <span data-ttu-id="3de2f-188">Si parece que ya está instalado el paquete de Entity Framework, es posible que ha seleccionado el proyecto StoreApp en lugar del proyecto StoreApp.Tests.</span><span class="sxs-lookup"><span data-stu-id="3de2f-188">If it appears that the EntityFramework package is already installed, you may have selected the StoreApp project instead of the StoreApp.Tests project.</span></span>

![Agregar Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

<span data-ttu-id="3de2f-190">Buscar e instalar el paquete de Microsoft ASP.NET Web API 2 núcleos.</span><span class="sxs-lookup"><span data-stu-id="3de2f-190">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![instalar el paquete de core web api](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

<span data-ttu-id="3de2f-192">Cierre la ventana Administrar paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="3de2f-192">Close the Manage NuGet Packages window.</span></span>

<a id="testcontext"></a>
## <a name="create-test-context"></a><span data-ttu-id="3de2f-193">Crear el contexto de la prueba</span><span class="sxs-lookup"><span data-stu-id="3de2f-193">Create test context</span></span>

<span data-ttu-id="3de2f-194">Agregue una clase denominada **TestDbSet** al proyecto de prueba.</span><span class="sxs-lookup"><span data-stu-id="3de2f-194">Add a class named **TestDbSet** to the test project.</span></span> <span data-ttu-id="3de2f-195">Esta clase actúa como clase base para el conjunto de datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="3de2f-195">This class serves as the base class for your test data set.</span></span> <span data-ttu-id="3de2f-196">Reemplace el código por el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="3de2f-196">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

<span data-ttu-id="3de2f-197">Agregue una clase denominada **TestProductDbSet** al proyecto de prueba que contiene el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="3de2f-197">Add a class named **TestProductDbSet** to the test project which contains the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

<span data-ttu-id="3de2f-198">Agregue una clase denominada **TestStoreAppContext** y reemplace el código existente por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="3de2f-198">Add a class named **TestStoreAppContext** and replace the existing code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="3de2f-199">Crear pruebas</span><span class="sxs-lookup"><span data-stu-id="3de2f-199">Create tests</span></span>

<span data-ttu-id="3de2f-200">De forma predeterminada, el proyecto de prueba incluye un archivo de prueba vacío denominado **UnitTest1.cs**.</span><span class="sxs-lookup"><span data-stu-id="3de2f-200">By default, your test project includes an empty test file named **UnitTest1.cs**.</span></span> <span data-ttu-id="3de2f-201">Este archivo muestra los atributos que se utiliza para crear métodos de prueba.</span><span class="sxs-lookup"><span data-stu-id="3de2f-201">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="3de2f-202">Para este tutorial, puede eliminar este archivo, puesto que agregará una nueva clase de prueba.</span><span class="sxs-lookup"><span data-stu-id="3de2f-202">For this tutorial, you can delete this file because you will add a new test class.</span></span>

<span data-ttu-id="3de2f-203">Agregue una clase denominada **TestProductController** al proyecto de prueba.</span><span class="sxs-lookup"><span data-stu-id="3de2f-203">Add a class named **TestProductController** to the test project.</span></span> <span data-ttu-id="3de2f-204">Reemplace el código por el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="3de2f-204">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="3de2f-205">Ejecutar pruebas</span><span class="sxs-lookup"><span data-stu-id="3de2f-205">Run tests</span></span>

<span data-ttu-id="3de2f-206">Ahora está listo para ejecutar las pruebas.</span><span class="sxs-lookup"><span data-stu-id="3de2f-206">You are now ready to run the tests.</span></span> <span data-ttu-id="3de2f-207">Todo el método que se marcan con el **TestMethod** se probará el atributo.</span><span class="sxs-lookup"><span data-stu-id="3de2f-207">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="3de2f-208">Desde el **prueba** elemento de menú, ejecutar las pruebas.</span><span class="sxs-lookup"><span data-stu-id="3de2f-208">From the **Test** menu item, run the tests.</span></span>

![ejecución de pruebas](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

<span data-ttu-id="3de2f-210">Abra el **Explorador de pruebas** ventana y observe los resultados de las pruebas.</span><span class="sxs-lookup"><span data-stu-id="3de2f-210">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![resultados de pruebas](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
