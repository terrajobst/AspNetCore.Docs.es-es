---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: Scaffolding de ASP.NET MVC 4 Entity Framework y migraciones | Documentos de Microsoft
author: rick-anderson
description: Si está familiarizado con los métodos de controlador de ASP.NET MVC 4 o ha completado la &quot;aplicaciones auxiliares, formularios y la validación&quot; laboratorio práctico, debe tener en cuenta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 42a12ee39223a06054382dbe9b4784196a706216
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/18/2018
ms.locfileid: "34306850"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="5774a-103">Las migraciones y Scaffolding de ASP.NET MVC 4 Entity Framework</span><span class="sxs-lookup"><span data-stu-id="5774a-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>

<span data-ttu-id="5774a-104">Por [Web colonias equipo](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="5774a-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="5774a-105">Descargar el Kit de aprendizaje de colonias de Web</span><span class="sxs-lookup"><span data-stu-id="5774a-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="5774a-106">Si está familiarizado con los métodos de controlador de ASP.NET MVC 4 o ha completado la &quot;aplicaciones auxiliares, formularios y la validación&quot; laboratorio práctico, deben tenerse en cuenta que muchos de la lógica para crear, actualizar, enumerar y quitar cualquier entidad de datos se repite entre la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5774a-106">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="5774a-107">No se mencione que, si el modelo tiene varias clases para manipular, es posible que pueda dedicar un tiempo considerable escribir los métodos de acción POST y GET para cada operación de entidad, así como cada una de las vistas.</span><span class="sxs-lookup"><span data-stu-id="5774a-107">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>

<span data-ttu-id="5774a-108">En este laboratorio, aprenderá cómo utilizar el scaffolding de ASP.NET MVC 4 para generar automáticamente la línea de base de CRUD la aplicación (crear, leer, actualizar y eliminar).</span><span class="sxs-lookup"><span data-stu-id="5774a-108">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="5774a-109">A partir de una clase de modelo simple y, sin escribir una sola línea de código, se creará un controlador que contendrá todas las operaciones CRUD, así como el todas las necesarias vistas.</span><span class="sxs-lookup"><span data-stu-id="5774a-109">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="5774a-110">Después de compilar y ejecutar la solución más fácil, tendrá la base de datos de aplicación generado, junto con la lógica MVC y vistas para la manipulación de datos.</span><span class="sxs-lookup"><span data-stu-id="5774a-110">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>

<span data-ttu-id="5774a-111">Además, obtendrá información sobre lo fácil que es usar migraciones de Entity Framework para realizar actualizaciones del modelo a lo largo de toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5774a-111">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="5774a-112">Migraciones de Entity Framework permiten modificar la base de datos después de que el modelo ha cambiado con pasos sencillos.</span><span class="sxs-lookup"><span data-stu-id="5774a-112">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="5774a-113">Con todo esto en cuenta, podrá crear y mantener las aplicaciones web de forma más eficaz, sacar partido de las últimas características de ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="5774a-113">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>

> [!NOTE]
> <span data-ttu-id="5774a-114">Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web colonias, disponible desde en [versiones de Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="5774a-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="5774a-115">Está disponible en el proyecto específico para este laboratorio [Entity Framework Scaffolding de ASP.NET MVC 4 y migraciones](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span><span class="sxs-lookup"><span data-stu-id="5774a-115">The project specific to this lab is available at [ASP.NET MVC 4 Entity Framework Scaffolding and Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="5774a-116">Objetivos</span><span class="sxs-lookup"><span data-stu-id="5774a-116">Objectives</span></span>

<span data-ttu-id="5774a-117">En este laboratorio práctico, aprenderá cómo:</span><span class="sxs-lookup"><span data-stu-id="5774a-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="5774a-118">Usar el scaffolding de ASP.NET para las operaciones CRUD en los controladores.</span><span class="sxs-lookup"><span data-stu-id="5774a-118">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="5774a-119">Cambiar el modelo de base de datos usando migraciones de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5774a-119">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="5774a-120">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="5774a-120">Prerequisites</span></span>

<span data-ttu-id="5774a-121">Debe tener los elementos siguientes para completar esta práctica:</span><span class="sxs-lookup"><span data-stu-id="5774a-121">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="5774a-122">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (leer [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).</span><span class="sxs-lookup"><span data-stu-id="5774a-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="5774a-123">Programa de instalación</span><span class="sxs-lookup"><span data-stu-id="5774a-123">Setup</span></span>

<span data-ttu-id="5774a-124">**Instalación de fragmentos de código**</span><span class="sxs-lookup"><span data-stu-id="5774a-124">**Installing Code Snippets**</span></span>

<span data-ttu-id="5774a-125">Para mayor comodidad, gran parte del código que se va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5774a-125">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="5774a-126">Para instalar los fragmentos de código que se ejecute **.\Source\Setup\CodeSnippets.vsi** archivo.</span><span class="sxs-lookup"><span data-stu-id="5774a-126">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="5774a-127">Si no está familiarizado con los fragmentos de código de Visual Studio y desea obtener información sobre cómo utilizarlas, puede consultar el apéndice de este documento &quot; [Apéndice B: Using Code Snippets](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="5774a-127">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="5774a-128">Ejercicios</span><span class="sxs-lookup"><span data-stu-id="5774a-128">Exercises</span></span>

<span data-ttu-id="5774a-129">El siguiente ejercicio conforman este laboratorio práctico:</span><span class="sxs-lookup"><span data-stu-id="5774a-129">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="5774a-130">Usar el Scaffolding de ASP.NET MVC 4 con migraciones de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="5774a-130">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="5774a-131">Este ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debería obtener después de completar el ejercicio.</span><span class="sxs-lookup"><span data-stu-id="5774a-131">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="5774a-132">Puede usar esta solución como una guía si necesita ayuda adicional para trabajar en el ejercicio.</span><span class="sxs-lookup"><span data-stu-id="5774a-132">You can use this solution as a guide if you need additional help working through the exercise.</span></span>


<span data-ttu-id="5774a-133">Tiempo estimado para completar esta práctica: **30 minutos**</span><span class="sxs-lookup"><span data-stu-id="5774a-133">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="5774a-134">Ejercicio 1: Usar el Scaffolding de ASP.NET MVC 4 con migraciones de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="5774a-134">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="5774a-135">Scaffolding de ASP.NET MVC proporciona una manera rápida de generar las operaciones CRUD de una manera normalizada, crear la lógica necesaria que permite a las aplicaciones interactúan con el nivel de base de datos.</span><span class="sxs-lookup"><span data-stu-id="5774a-135">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="5774a-136">En este ejercicio, aprenderá a usar el scaffolding de ASP.NET MVC 4 con código en primer lugar para crear los métodos CRUD.</span><span class="sxs-lookup"><span data-stu-id="5774a-136">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="5774a-137">A continuación, obtendrá información sobre cómo actualizar el modelo de aplicar los cambios en la base de datos mediante el uso de las migraciones de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5774a-137">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="5774a-138">Proyecto de la tarea 1: crear una nueva versión de ASP.NET MVC 4 mediante Scaffolding</span><span class="sxs-lookup"><span data-stu-id="5774a-138">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="5774a-139">Si no está abierto, inicie **Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="5774a-139">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="5774a-140">Seleccione **archivo | Nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="5774a-140">Select **File | New Project**.</span></span> <span data-ttu-id="5774a-141">En el icono nuevo proyecto del cuadro de diálogo, en la **Visual C# | Web** sección, seleccione **aplicación Web de ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="5774a-141">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="5774a-142">Denomine el proyecto a **MVC4andEFMigrations** y establezca la ubicación en **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** carpeta de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="5774a-142">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="5774a-143">Establecer el **nombre de la solución** a **comenzar** y asegúrese de **crear directorio para la solución** está activada.</span><span class="sxs-lookup"><span data-stu-id="5774a-143">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="5774a-144">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="5774a-144">Click **OK**.</span></span>

    <span data-ttu-id="5774a-145">![Nuevo cuadro de diálogo de proyecto de MVC de ASP.NET 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "nuevo cuadro de diálogo de proyecto de MVC de ASP.NET 4")</span><span class="sxs-lookup"><span data-stu-id="5774a-145">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="5774a-146">*Nuevo cuadro de diálogo de proyecto de MVC de ASP.NET 4*</span><span class="sxs-lookup"><span data-stu-id="5774a-146">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="5774a-147">En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione la **aplicación de Internet** plantilla y asegúrese de que **Razor** está seleccionado **motor de vista**.</span><span class="sxs-lookup"><span data-stu-id="5774a-147">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="5774a-148">Haga clic en **Aceptar** para crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="5774a-148">Click **OK** to create the project.</span></span>

    <span data-ttu-id="5774a-149">![Nueva aplicación de Internet de ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "nueva aplicación de Internet de ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="5774a-149">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="5774a-150">*Nueva aplicación de Internet de ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="5774a-150">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="5774a-151">En el Explorador de soluciones, haga clic en **modelos** y seleccione **agregar | Clase** para crear una persona de clases simple (POCO).</span><span class="sxs-lookup"><span data-stu-id="5774a-151">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="5774a-152">Asígnele el nombre **persona** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="5774a-152">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="5774a-153">Abra la clase de persona e inserte las siguientes propiedades.</span><span class="sxs-lookup"><span data-stu-id="5774a-153">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="5774a-154">(Código de fragmento de código: *MVC de ASP.NET 4 y migraciones de Entity Framework - Ex1 persona propiedades*)</span><span class="sxs-lookup"><span data-stu-id="5774a-154">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="5774a-155">Haga clic en **compilar | Compilar solución** para guardar los cambios y compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="5774a-155">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="5774a-156">![Compilar la aplicación](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "compilar la aplicación")</span><span class="sxs-lookup"><span data-stu-id="5774a-156">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="5774a-157">*Compilación de la aplicación*</span><span class="sxs-lookup"><span data-stu-id="5774a-157">*Building the Application*</span></span>
7. <span data-ttu-id="5774a-158">En el Explorador de soluciones, haga clic en la carpeta controllers y seleccione **agregar | Controlador**.</span><span class="sxs-lookup"><span data-stu-id="5774a-158">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="5774a-159">Nombre del controlador *PersonController* y completar la **opciones de Scaffolding** con los valores siguientes.</span><span class="sxs-lookup"><span data-stu-id="5774a-159">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

   1. <span data-ttu-id="5774a-160">En el **plantilla** lista desplegable, seleccione la **controlador de MVC con acciones de lectura/escritura y vistas, mediante Entity Framework** opción.</span><span class="sxs-lookup"><span data-stu-id="5774a-160">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
   2. <span data-ttu-id="5774a-161">En el **clase modelo** lista desplegable, seleccione la **persona** clase.</span><span class="sxs-lookup"><span data-stu-id="5774a-161">In the **Model class** drop-down list, select the **Person** class.</span></span>
   3. <span data-ttu-id="5774a-162">En el **clase de contexto de datos** lista, seleccione  **&lt;nuevo contexto de datos... &gt;**.</span><span class="sxs-lookup"><span data-stu-id="5774a-162">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="5774a-163">Elegir un nombre y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="5774a-163">Choose any name and click **OK**.</span></span>
   4. <span data-ttu-id="5774a-164">En el **vistas** lista desplegable lista, asegúrese de que **Razor** está seleccionada.</span><span class="sxs-lookup"><span data-stu-id="5774a-164">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

      <span data-ttu-id="5774a-165">![Agregar el controlador de la persona con la técnica scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "agregar el controlador de la persona con la técnica scaffolding")</span><span class="sxs-lookup"><span data-stu-id="5774a-165">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

      <span data-ttu-id="5774a-166">*Agregar el controlador de la persona con la técnica scaffolding*</span><span class="sxs-lookup"><span data-stu-id="5774a-166">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="5774a-167">Haga clic en **agregar** para crear el nuevo controlador de la persona con la técnica scaffolding.</span><span class="sxs-lookup"><span data-stu-id="5774a-167">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="5774a-168">Ahora se ha generado las acciones de controlador, así como las vistas.</span><span class="sxs-lookup"><span data-stu-id="5774a-168">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="5774a-169">![Después de crear el controlador de la persona con la técnica scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "después de crear el controlador de la persona con la técnica scaffolding")</span><span class="sxs-lookup"><span data-stu-id="5774a-169">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="5774a-170">*Después de crear el controlador de la persona con la técnica scaffolding*</span><span class="sxs-lookup"><span data-stu-id="5774a-170">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="5774a-171">Abra **PersonController** clase.</span><span class="sxs-lookup"><span data-stu-id="5774a-171">Open **PersonController** class.</span></span> <span data-ttu-id="5774a-172">Tenga en cuenta que los métodos de acción CRUD completos se han generado automáticamente.</span><span class="sxs-lookup"><span data-stu-id="5774a-172">Notice that the full CRUD action methods have been generated automatically.</span></span>

   <span data-ttu-id="5774a-173">![En el controlador de la persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "controlador dentro de la persona")</span><span class="sxs-lookup"><span data-stu-id="5774a-173">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

   <span data-ttu-id="5774a-174">*En el controlador de persona*</span><span class="sxs-lookup"><span data-stu-id="5774a-174">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="5774a-175">Tarea 2: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="5774a-175">Task 2- Running the application</span></span>

<span data-ttu-id="5774a-176">En este momento, no se ha creado la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5774a-176">At this point, the database is not yet created.</span></span> <span data-ttu-id="5774a-177">En esta tarea, ejecutará la aplicación por primera vez y probar las operaciones CRUD.</span><span class="sxs-lookup"><span data-stu-id="5774a-177">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="5774a-178">Se creará la base de datos sobre la marcha con Code First.</span><span class="sxs-lookup"><span data-stu-id="5774a-178">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="5774a-179">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5774a-179">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="5774a-180">En el explorador, agregue **/Person** a la dirección URL para abrir la página de la persona.</span><span class="sxs-lookup"><span data-stu-id="5774a-180">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="5774a-181">![Aplicación que se ejecuta por primera vez](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "aplicación ejecuta por primera vez")</span><span class="sxs-lookup"><span data-stu-id="5774a-181">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="5774a-182">*Aplicación: primero ejecute*</span><span class="sxs-lookup"><span data-stu-id="5774a-182">*Application: first run*</span></span>
3. <span data-ttu-id="5774a-183">Ahora podrá consultar las páginas de la persona y probar las operaciones CRUD.</span><span class="sxs-lookup"><span data-stu-id="5774a-183">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="5774a-184">Haga clic en **crear nuevo** para agregar una nueva persona.</span><span class="sxs-lookup"><span data-stu-id="5774a-184">Click **Create New** to add a new person.</span></span> <span data-ttu-id="5774a-185">Escriba un nombre y un apellido y haga clic en **crear**.</span><span class="sxs-lookup"><span data-stu-id="5774a-185">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="5774a-186">![Agregar una nueva persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "agregar una nueva persona")</span><span class="sxs-lookup"><span data-stu-id="5774a-186">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="5774a-187">*Agregar una nueva persona*</span><span class="sxs-lookup"><span data-stu-id="5774a-187">*Adding a new person*</span></span>
    2. <span data-ttu-id="5774a-188">En la lista de la persona, puede eliminar, editar o agregar elementos.</span><span class="sxs-lookup"><span data-stu-id="5774a-188">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="5774a-189">![lista de personas](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "lista de personas")</span><span class="sxs-lookup"><span data-stu-id="5774a-189">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="5774a-190">*Lista de personas*</span><span class="sxs-lookup"><span data-stu-id="5774a-190">*Person list*</span></span>
    3. <span data-ttu-id="5774a-191">Haga clic en **detalles** para abrir detalles de la persona.</span><span class="sxs-lookup"><span data-stu-id="5774a-191">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="5774a-192">![Detalles de la persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "detalles de la persona")</span><span class="sxs-lookup"><span data-stu-id="5774a-192">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="5774a-193">*Detalles de la persona*</span><span class="sxs-lookup"><span data-stu-id="5774a-193">*Person's details*</span></span>
4. <span data-ttu-id="5774a-194">Cierre el explorador y vuelva a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5774a-194">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="5774a-195">Tenga en cuenta que se ha creado el CRUD completa para la entidad person a lo largo de su aplicación - desde el modelo a las vistas, sin tener que escribir una sola línea de código.</span><span class="sxs-lookup"><span data-stu-id="5774a-195">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="5774a-196">Tarea 3: actualizar la base de datos usando migraciones de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="5774a-196">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="5774a-197">En esta tarea se actualizará la base de datos usando migraciones de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5774a-197">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="5774a-198">Descubrirá lo fácil que es cambiar el modelo y reflejar los cambios en las bases de datos mediante la característica de migraciones de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5774a-198">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="5774a-199">Abra la consola de administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="5774a-199">Open the Package Manager Console.</span></span> <span data-ttu-id="5774a-200">Seleccione **herramientas | Administrador de paquetes de biblioteca | Consola de administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="5774a-200">Select **Tools | Library Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="5774a-201">En la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="5774a-201">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="5774a-202">PMC</span><span class="sxs-lookup"><span data-stu-id="5774a-202">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="5774a-203">![Habilitar migraciones](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "habilitar migraciones")</span><span class="sxs-lookup"><span data-stu-id="5774a-203">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="5774a-204">*Habilitar las migraciones*</span><span class="sxs-lookup"><span data-stu-id="5774a-204">*Enabling migrations*</span></span>

    <span data-ttu-id="5774a-205">El comando Enable-migración crea el **migraciones** carpeta, que contiene una secuencia de comandos para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5774a-205">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="5774a-206">![Carpeta Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "carpeta Migrations")</span><span class="sxs-lookup"><span data-stu-id="5774a-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="5774a-207">*Carpeta de migraciones*</span><span class="sxs-lookup"><span data-stu-id="5774a-207">*Migrations folder*</span></span>
3. <span data-ttu-id="5774a-208">Abra la **archivo Configuration.cs que** archivos en la carpeta Migrations.</span><span class="sxs-lookup"><span data-stu-id="5774a-208">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="5774a-209">Busque el constructor de clase y cambie la **AutomaticMigrationsEnabled** valor *true*.</span><span class="sxs-lookup"><span data-stu-id="5774a-209">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="5774a-210">Abra la clase de persona y agregue un atributo para el segundo nombre de la persona.</span><span class="sxs-lookup"><span data-stu-id="5774a-210">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="5774a-211">Con este nuevo atributo, va a cambiar el modelo.</span><span class="sxs-lookup"><span data-stu-id="5774a-211">With this new attribute, you are changing the model.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="5774a-212">Seleccione **compilar | Compilar solución** en el menú para compilar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5774a-212">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="5774a-213">![Compilar la aplicación](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "compilar la aplicación")</span><span class="sxs-lookup"><span data-stu-id="5774a-213">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="5774a-214">*Compilar la aplicación*</span><span class="sxs-lookup"><span data-stu-id="5774a-214">*Building the application*</span></span>
6. <span data-ttu-id="5774a-215">En la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="5774a-215">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="5774a-216">PMC</span><span class="sxs-lookup"><span data-stu-id="5774a-216">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="5774a-217">Este comando busca cambios en los objetos de datos y, a continuación, agregará los comandos necesarios para modificar la base de datos en consecuencia.</span><span class="sxs-lookup"><span data-stu-id="5774a-217">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="5774a-218">![Agregar un segundo nombre](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "adición de un nombre de medio")</span><span class="sxs-lookup"><span data-stu-id="5774a-218">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="5774a-219">*Adición de un nombre de medio*</span><span class="sxs-lookup"><span data-stu-id="5774a-219">*Adding a middle name*</span></span>
7. <span data-ttu-id="5774a-220">(Opcional) Puede ejecutar el comando siguiente para generar un script SQL con la actualización diferencial.</span><span class="sxs-lookup"><span data-stu-id="5774a-220">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="5774a-221">Esto le permitirá actualizar manualmente la base de datos (en este caso no es necesario), o aplicar los cambios en otras bases de datos:</span><span class="sxs-lookup"><span data-stu-id="5774a-221">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="5774a-222">PMC</span><span class="sxs-lookup"><span data-stu-id="5774a-222">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="5774a-223">![Generar una secuencia de comandos SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "generar un script SQL")</span><span class="sxs-lookup"><span data-stu-id="5774a-223">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="5774a-224">*Generar un script SQL*</span><span class="sxs-lookup"><span data-stu-id="5774a-224">*Generating a SQL script*</span></span>

    <span data-ttu-id="5774a-225">![Actualización de secuencia de comandos SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "actualización de secuencia de comandos SQL")</span><span class="sxs-lookup"><span data-stu-id="5774a-225">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="5774a-226">*Actualización de secuencia de comandos SQL*</span><span class="sxs-lookup"><span data-stu-id="5774a-226">*SQL Script update*</span></span>
8. <span data-ttu-id="5774a-227">En la consola de administrador de paquetes, escriba el siguiente comando para actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="5774a-227">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="5774a-228">PMC</span><span class="sxs-lookup"><span data-stu-id="5774a-228">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="5774a-229">![Actualizar la base de datos](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "actualizar la base de datos")</span><span class="sxs-lookup"><span data-stu-id="5774a-229">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="5774a-230">*Actualizar la base de datos*</span><span class="sxs-lookup"><span data-stu-id="5774a-230">*Updating the Database*</span></span>

    <span data-ttu-id="5774a-231">Esto agregará la **MiddleName** columna en el **personas** tabla para que coincida con la definición actual de la **persona** clase.</span><span class="sxs-lookup"><span data-stu-id="5774a-231">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="5774a-232">Una vez que se actualiza la base de datos, haga clic en la carpeta de controlador y seleccione **agregar | Controlador** para agregar el controlador de persona nuevo (completada con los mismos valores).</span><span class="sxs-lookup"><span data-stu-id="5774a-232">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="5774a-233">Esto actualizará los métodos existentes y las vistas de agregar el nuevo atributo.</span><span class="sxs-lookup"><span data-stu-id="5774a-233">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="5774a-234">![Agregar una actualización de controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "agregar una actualización de controlador")</span><span class="sxs-lookup"><span data-stu-id="5774a-234">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="5774a-235">*Actualizar el controlador*</span><span class="sxs-lookup"><span data-stu-id="5774a-235">*Updating the controller*</span></span>
10. <span data-ttu-id="5774a-236">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="5774a-236">Click **Add**.</span></span> <span data-ttu-id="5774a-237">A continuación, seleccione los valores **sobrescribir PersonController.cs** y **sobrescribir vistas asociadas** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="5774a-237">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

   ![Agregar una sobrescritura de controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   <span data-ttu-id="5774a-239">*Actualizar el controlador*</span><span class="sxs-lookup"><span data-stu-id="5774a-239">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="5774a-240">Task4 - ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="5774a-240">Task4- Running the application</span></span>

1. <span data-ttu-id="5774a-241">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5774a-241">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="5774a-242">Abra **/Person**.</span><span class="sxs-lookup"><span data-stu-id="5774a-242">Open **/Person**.</span></span> <span data-ttu-id="5774a-243">Tenga en cuenta que se conservan los datos, mientras que se ha agregado la columna apellido.</span><span class="sxs-lookup"><span data-stu-id="5774a-243">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="5774a-244">![Agrega el segundo apellido](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "agrega el segundo apellido")</span><span class="sxs-lookup"><span data-stu-id="5774a-244">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="5774a-245">*Agrega el segundo apellido*</span><span class="sxs-lookup"><span data-stu-id="5774a-245">*Middle Name added*</span></span>
3. <span data-ttu-id="5774a-246">Si hace clic en **editar**, podrá agregar un segundo nombre a la persona actual.</span><span class="sxs-lookup"><span data-stu-id="5774a-246">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="5774a-247">![El segundo apellido edición](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "edición del segundo nombre")</span><span class="sxs-lookup"><span data-stu-id="5774a-247">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="5774a-248">Resumen</span><span class="sxs-lookup"><span data-stu-id="5774a-248">Summary</span></span>

<span data-ttu-id="5774a-249">En este laboratorio práctico, ha aprendido sencillos pasos para crear operaciones CRUD con ASP.NET MVC 4 Scaffolding usando cualquier clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="5774a-249">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="5774a-250">A continuación, ha aprendido cómo realizar una actualización de extremo a extremo en su aplicación - desde la base de datos a las vistas - mediante el uso de las migraciones de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5774a-250">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="5774a-251">Apéndice A: instalación de Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="5774a-251">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="5774a-252">Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión usando la **[instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="5774a-252">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="5774a-253">Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* con *instalador de plataforma Web de Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="5774a-253">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="5774a-254">Vaya a [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="5774a-254">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="5774a-255">O bien, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con SDK de Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="5774a-255">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="5774a-256">Haga clic en **instalar ahora**.</span><span class="sxs-lookup"><span data-stu-id="5774a-256">Click on **Install Now**.</span></span> <span data-ttu-id="5774a-257">Si no tiene **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo primero.</span><span class="sxs-lookup"><span data-stu-id="5774a-257">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="5774a-258">Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.</span><span class="sxs-lookup"><span data-stu-id="5774a-258">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="5774a-259">![Instale Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "instale Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="5774a-259">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="5774a-260">*Instale Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="5774a-260">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="5774a-261">Lea los términos y licencias de todos los productos y haga clic en **acepto** para continuar.</span><span class="sxs-lookup"><span data-stu-id="5774a-261">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Acepte los términos de licencia](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="5774a-263">*Acepte los términos de licencia*</span><span class="sxs-lookup"><span data-stu-id="5774a-263">*Accepting the license terms*</span></span>
5. <span data-ttu-id="5774a-264">Espere hasta que finalice el proceso de descarga e instalación.</span><span class="sxs-lookup"><span data-stu-id="5774a-264">Wait until the downloading and installation process completes.</span></span>

    ![Progreso de la instalación](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="5774a-266">*Progreso de la instalación*</span><span class="sxs-lookup"><span data-stu-id="5774a-266">*Installation progress*</span></span>
6. <span data-ttu-id="5774a-267">Cuando se complete la instalación, haga clic en **finalizar**.</span><span class="sxs-lookup"><span data-stu-id="5774a-267">When the installation completes, click **Finish**.</span></span>

    ![Se completó la instalación](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="5774a-269">*Se completó la instalación*</span><span class="sxs-lookup"><span data-stu-id="5774a-269">*Installation completed*</span></span>
7. <span data-ttu-id="5774a-270">Haga clic en **Exit** para cerrar el instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="5774a-270">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="5774a-271">Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **Express frente a**&quot;, a continuación, haga clic en el **VS Express para Web** colocar en mosaico.</span><span class="sxs-lookup"><span data-stu-id="5774a-271">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Express de VS para icono Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="5774a-273">*Express de VS para icono Web*</span><span class="sxs-lookup"><span data-stu-id="5774a-273">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="5774a-274">Apéndice B: usar fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="5774a-274">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="5774a-275">Con fragmentos de código, tiene todo el código que necesita a su alcance.</span><span class="sxs-lookup"><span data-stu-id="5774a-275">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="5774a-276">El documento de laboratorio le indicará exactamente cuándo utilizarlas, tal como se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="5774a-276">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="5774a-277">![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "fragmentos de código en Visual Studio para insertar código en el proyecto")</span><span class="sxs-lookup"><span data-stu-id="5774a-277">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="5774a-278">*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*</span><span class="sxs-lookup"><span data-stu-id="5774a-278">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="5774a-279">***Para agregar un fragmento de código mediante el teclado (solo C#)***</span><span class="sxs-lookup"><span data-stu-id="5774a-279">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="5774a-280">Coloque el cursor donde desea insertar el código.</span><span class="sxs-lookup"><span data-stu-id="5774a-280">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="5774a-281">Comience a escribir el nombre del fragmento (sin espacios ni guiones).</span><span class="sxs-lookup"><span data-stu-id="5774a-281">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="5774a-282">Observe como coincidencia de nombres de fragmentos de código de muestra de IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="5774a-282">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="5774a-283">Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).</span><span class="sxs-lookup"><span data-stu-id="5774a-283">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="5774a-284">Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.</span><span class="sxs-lookup"><span data-stu-id="5774a-284">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="5774a-285">![Comience a escribir el nombre del fragmento](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "comience a escribir el nombre del fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="5774a-285">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="5774a-286">*Comience a escribir el nombre del fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="5774a-286">*Start typing the snippet name*</span></span>

<span data-ttu-id="5774a-287">![Presione la tecla Tab para seleccionar el fragmento de código resaltada](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "presione Tab para seleccionar el fragmento de código resaltada")</span><span class="sxs-lookup"><span data-stu-id="5774a-287">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="5774a-288">*Presione la tecla Tab para seleccionar el fragmento de código resaltada*</span><span class="sxs-lookup"><span data-stu-id="5774a-288">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="5774a-289">![Vuelva a presionar Tab y el fragmento de código se expandirán](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "vuelva a presionar Tab y el fragmento de código se expandirán")</span><span class="sxs-lookup"><span data-stu-id="5774a-289">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="5774a-290">*Vuelva a presionar Tab y el fragmento de código se expandirán*</span><span class="sxs-lookup"><span data-stu-id="5774a-290">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="5774a-291">***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="5774a-291">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="5774a-292">Haga clic en donde desea insertar el fragmento de código.</span><span class="sxs-lookup"><span data-stu-id="5774a-292">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="5774a-293">Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.</span><span class="sxs-lookup"><span data-stu-id="5774a-293">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="5774a-294">Seleccione el fragmento de código relevante de la lista haciendo clic en él.</span><span class="sxs-lookup"><span data-stu-id="5774a-294">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="5774a-295">![Menú contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="5774a-295">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="5774a-296">*Haga clic en donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="5774a-296">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="5774a-297">![Seleccione el fragmento de código relevante de la lista haciendo clic en él](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "elegir el fragmento de código relevante de la lista haciendo clic en él")</span><span class="sxs-lookup"><span data-stu-id="5774a-297">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="5774a-298">*Seleccione el fragmento de código relevante de la lista haciendo clic en él*</span><span class="sxs-lookup"><span data-stu-id="5774a-298">*Pick the relevant snippet from the list, by clicking on it*</span></span>
