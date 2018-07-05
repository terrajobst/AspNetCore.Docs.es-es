---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework Scaffolding y migraciones | Microsoft Docs
author: rick-anderson
description: Si está familiarizado con los métodos de controlador de ASP.NET MVC 4 o ha completado la &quot;aplicaciones auxiliares, formularios y la validación&quot; laboratorio práctico, debe tener en cuenta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: a385ffd3f0067d4ac56d592b0f2bf151fc0f8dd4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394214"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="f30ce-103">ASP.NET MVC 4 Entity Framework Scaffolding y migraciones</span><span class="sxs-lookup"><span data-stu-id="f30ce-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>

<span data-ttu-id="f30ce-104">por [campamentos Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="f30ce-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="f30ce-105">Descargue el Kit de aprendizaje de campamentos de Web</span><span class="sxs-lookup"><span data-stu-id="f30ce-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="f30ce-106">Si está familiarizado con los métodos de controlador de ASP.NET MVC 4 o ha completado la &quot;aplicaciones auxiliares, formularios y la validación&quot; laboratorio práctico, debe tener en cuenta que muchos de la lógica para crear, actualizar, enumerar y quitar cualquier entidad de datos se repite entre la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f30ce-106">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="f30ce-107">Para que no se menciona, si el modelo tiene varias clases para manipular, será probablemente dedicar un tiempo considerable a escribir los métodos de acción POST y GET para cada operación de entidad, así como cada una de las vistas.</span><span class="sxs-lookup"><span data-stu-id="f30ce-107">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>

<span data-ttu-id="f30ce-108">En este laboratorio obtendrá información sobre cómo usar el scaffolding de ASP.NET MVC 4 para generar automáticamente la línea de base de CRUD la aplicación (creación, lectura, actualización y eliminación).</span><span class="sxs-lookup"><span data-stu-id="f30ce-108">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="f30ce-109">A partir de una clase de modelo simple y sin necesidad de escribir una sola línea de código, se creará un controlador que contendrá todas las operaciones de CRUD, así como la todas las necesarias vistas.</span><span class="sxs-lookup"><span data-stu-id="f30ce-109">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="f30ce-110">Después de compilar y ejecutar la solución más sencilla, tendrá la base de datos de aplicación generado, junto con la lógica MVC y vistas para la manipulación de datos.</span><span class="sxs-lookup"><span data-stu-id="f30ce-110">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>

<span data-ttu-id="f30ce-111">Además, obtendrá información sobre lo fácil que es usar migraciones de Entity Framework para realizar actualizaciones del modelo a lo largo de toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f30ce-111">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="f30ce-112">Migraciones de Entity Framework le permitirá modificar la base de datos después de que el modelo ha cambiado con pasos sencillos.</span><span class="sxs-lookup"><span data-stu-id="f30ce-112">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="f30ce-113">Con todo esto en mente, podrá crear y mantener las aplicaciones web de forma más eficaz, que aprovecha las características más recientes de ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="f30ce-113">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>

> [!NOTE]
> <span data-ttu-id="f30ce-114">Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web campamentos, disponible desde en [versiones de Microsoft de Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="f30ce-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="f30ce-115">Está disponible en el proyecto específico para este laboratorio [Entity Framework Scaffolding de ASP.NET MVC 4 y migraciones](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span><span class="sxs-lookup"><span data-stu-id="f30ce-115">The project specific to this lab is available at [ASP.NET MVC 4 Entity Framework Scaffolding and Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="f30ce-116">Objetivos</span><span class="sxs-lookup"><span data-stu-id="f30ce-116">Objectives</span></span>

<span data-ttu-id="f30ce-117">En este laboratorio práctico, aprenderá cómo:</span><span class="sxs-lookup"><span data-stu-id="f30ce-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="f30ce-118">Usar el scaffolding de ASP.NET para las operaciones CRUD en los controladores.</span><span class="sxs-lookup"><span data-stu-id="f30ce-118">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="f30ce-119">Cambiar el modelo de base de datos mediante migraciones de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f30ce-119">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="f30ce-120">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="f30ce-120">Prerequisites</span></span>

<span data-ttu-id="f30ce-121">Debe tener los elementos siguientes para completar esta práctica:</span><span class="sxs-lookup"><span data-stu-id="f30ce-121">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="f30ce-122">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (leer [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).</span><span class="sxs-lookup"><span data-stu-id="f30ce-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="f30ce-123">Programa de instalación</span><span class="sxs-lookup"><span data-stu-id="f30ce-123">Setup</span></span>

<span data-ttu-id="f30ce-124">**Instalación de fragmentos de código**</span><span class="sxs-lookup"><span data-stu-id="f30ce-124">**Installing Code Snippets**</span></span>

<span data-ttu-id="f30ce-125">Para mayor comodidad, gran parte del código que se va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f30ce-125">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="f30ce-126">Para instalar los fragmentos de código que se ejecute **.\Source\Setup\CodeSnippets.vsi** archivo.</span><span class="sxs-lookup"><span data-stu-id="f30ce-126">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="f30ce-127">Si no está familiarizado con los fragmentos de código de Visual Studio y desea aprender a usarlas, puede consultar el apéndice de este documento &quot; [Apéndice B: Using Code Snippets](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="f30ce-127">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="f30ce-128">Ejercicios</span><span class="sxs-lookup"><span data-stu-id="f30ce-128">Exercises</span></span>

<span data-ttu-id="f30ce-129">El ejercicio siguiente conforman este laboratorio práctico:</span><span class="sxs-lookup"><span data-stu-id="f30ce-129">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="f30ce-130">Uso de Scaffolding de ASP.NET MVC 4 con migraciones de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="f30ce-130">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="f30ce-131">Este ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debe obtener después de completar el ejercicio.</span><span class="sxs-lookup"><span data-stu-id="f30ce-131">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="f30ce-132">Puede usar esta solución como una guía si necesita ayuda adicional para trabajar por el ejercicio.</span><span class="sxs-lookup"><span data-stu-id="f30ce-132">You can use this solution as a guide if you need additional help working through the exercise.</span></span>


<span data-ttu-id="f30ce-133">Tiempo estimado para completar esta práctica: **30 minutos**</span><span class="sxs-lookup"><span data-stu-id="f30ce-133">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="f30ce-134">Ejercicio 1: Uso de Scaffolding de ASP.NET MVC 4 con migraciones de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="f30ce-134">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="f30ce-135">Scaffolding de ASP.NET MVC proporciona una forma rápida para generar las operaciones CRUD de manera estándar para crear la lógica necesaria que permite a las aplicaciones interactúan con la capa de base de datos.</span><span class="sxs-lookup"><span data-stu-id="f30ce-135">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="f30ce-136">En este ejercicio, obtendrá información sobre cómo usar el scaffolding de ASP.NET MVC 4 con código en primer lugar para crear los métodos CRUD.</span><span class="sxs-lookup"><span data-stu-id="f30ce-136">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="f30ce-137">A continuación, obtendrá información sobre cómo actualizar el modelo de aplicar los cambios en la base de datos mediante el uso de migraciones de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f30ce-137">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="f30ce-138">Proyecto de la tarea 1: crear una nueva versión de ASP.NET MVC 4 mediante Scaffolding</span><span class="sxs-lookup"><span data-stu-id="f30ce-138">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="f30ce-139">Si no está abierto, inicie **Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="f30ce-139">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="f30ce-140">Seleccione **archivo | Nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="f30ce-140">Select **File | New Project**.</span></span> <span data-ttu-id="f30ce-141">En el nuevo proyecto de cuadro de diálogo, en el **Visual C# | Web** sección, seleccione **aplicación Web de ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="f30ce-141">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="f30ce-142">Denomine el proyecto a **MVC4andEFMigrations** y establezca la ubicación en **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** carpeta de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="f30ce-142">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="f30ce-143">Establecer el **nombre de la solución** a **comenzar** y asegúrese de **crear directorio para la solución** está activada.</span><span class="sxs-lookup"><span data-stu-id="f30ce-143">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="f30ce-144">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="f30ce-144">Click **OK**.</span></span>

    <span data-ttu-id="f30ce-145">![Nuevo cuadro de diálogo de proyecto de ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "nuevo cuadro de diálogo de proyecto de ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="f30ce-145">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="f30ce-146">*Nuevo cuadro de diálogo de proyecto de ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="f30ce-146">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="f30ce-147">En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione el **aplicación de Internet** plantilla y asegúrese de que **Razor** está seleccionado **delmotordevistas**.</span><span class="sxs-lookup"><span data-stu-id="f30ce-147">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="f30ce-148">Haga clic en **Aceptar** para crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="f30ce-148">Click **OK** to create the project.</span></span>

    <span data-ttu-id="f30ce-149">![Nueva aplicación de Internet de ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "nueva aplicación de Internet de ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="f30ce-149">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="f30ce-150">*Nueva aplicación de Internet de ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="f30ce-150">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="f30ce-151">En el Explorador de soluciones, haga clic en **modelos** y seleccione **Add | Clase** para crear una persona de la clase simple (POCO).</span><span class="sxs-lookup"><span data-stu-id="f30ce-151">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="f30ce-152">Asígnele el nombre **persona** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="f30ce-152">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="f30ce-153">Abra la clase Person e inserte las siguientes propiedades.</span><span class="sxs-lookup"><span data-stu-id="f30ce-153">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="f30ce-154">(Código de fragmento de código - *ASP.NET MVC 4 y migraciones de Entity Framework - propiedades de persona Ex1*)</span><span class="sxs-lookup"><span data-stu-id="f30ce-154">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="f30ce-155">Haga clic en **compilar | Compilar solución** para guardar los cambios y compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="f30ce-155">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="f30ce-156">![Creación de la aplicación](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "compilar la aplicación")</span><span class="sxs-lookup"><span data-stu-id="f30ce-156">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="f30ce-157">*Compilación de la aplicación*</span><span class="sxs-lookup"><span data-stu-id="f30ce-157">*Building the Application*</span></span>
7. <span data-ttu-id="f30ce-158">En el Explorador de soluciones, haga clic en la carpeta controllers y seleccione **Add | Controlador**.</span><span class="sxs-lookup"><span data-stu-id="f30ce-158">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="f30ce-159">Asigne al controlador el *PersonController* y complete el **opciones de Scaffolding** con los valores siguientes.</span><span class="sxs-lookup"><span data-stu-id="f30ce-159">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

   1. <span data-ttu-id="f30ce-160">En el **plantilla** lista desplegable, seleccione el **controlador de MVC con acciones de lectura/escritura y vistas que usan Entity Framework** opción.</span><span class="sxs-lookup"><span data-stu-id="f30ce-160">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
   2. <span data-ttu-id="f30ce-161">En el **clase modelo** lista desplegable, seleccione el **persona** clase.</span><span class="sxs-lookup"><span data-stu-id="f30ce-161">In the **Model class** drop-down list, select the **Person** class.</span></span>
   3. <span data-ttu-id="f30ce-162">En el **clase de contexto de datos** lista, seleccione  **&lt;nuevo contexto de datos... &gt;**.</span><span class="sxs-lookup"><span data-stu-id="f30ce-162">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="f30ce-163">Elegir un nombre y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="f30ce-163">Choose any name and click **OK**.</span></span>
   4. <span data-ttu-id="f30ce-164">En el **vistas** desplegable lista, asegúrese de que **Razor** está seleccionada.</span><span class="sxs-lookup"><span data-stu-id="f30ce-164">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

      <span data-ttu-id="f30ce-165">![Agregar el controlador de la persona con la técnica scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "adición del controlador de la persona con la técnica scaffolding")</span><span class="sxs-lookup"><span data-stu-id="f30ce-165">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

      <span data-ttu-id="f30ce-166">*Agregar el controlador de la persona con la técnica scaffolding*</span><span class="sxs-lookup"><span data-stu-id="f30ce-166">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="f30ce-167">Haga clic en **agregar** para crear el nuevo controlador para la persona con la técnica scaffolding.</span><span class="sxs-lookup"><span data-stu-id="f30ce-167">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="f30ce-168">Ahora se ha generado las acciones de controlador, así como las vistas.</span><span class="sxs-lookup"><span data-stu-id="f30ce-168">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="f30ce-169">![Después de crear el controlador de la persona con la técnica scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "después de crear el controlador de la persona con la técnica scaffolding")</span><span class="sxs-lookup"><span data-stu-id="f30ce-169">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="f30ce-170">*Después de crear el controlador de la persona con la técnica scaffolding*</span><span class="sxs-lookup"><span data-stu-id="f30ce-170">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="f30ce-171">Abra **PersonController** clase.</span><span class="sxs-lookup"><span data-stu-id="f30ce-171">Open **PersonController** class.</span></span> <span data-ttu-id="f30ce-172">Tenga en cuenta que los métodos de acción CRUD completos se han generado automáticamente.</span><span class="sxs-lookup"><span data-stu-id="f30ce-172">Notice that the full CRUD action methods have been generated automatically.</span></span>

   <span data-ttu-id="f30ce-173">![En el controlador de persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "controlador dentro de la persona")</span><span class="sxs-lookup"><span data-stu-id="f30ce-173">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

   <span data-ttu-id="f30ce-174">*En el controlador de persona*</span><span class="sxs-lookup"><span data-stu-id="f30ce-174">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="f30ce-175">Tarea 2: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="f30ce-175">Task 2- Running the application</span></span>

<span data-ttu-id="f30ce-176">En este momento, no se ha creado la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f30ce-176">At this point, the database is not yet created.</span></span> <span data-ttu-id="f30ce-177">En esta tarea, ejecutará la aplicación por primera vez y probar las operaciones CRUD.</span><span class="sxs-lookup"><span data-stu-id="f30ce-177">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="f30ce-178">Se creará la base de datos sobre la marcha con Code First.</span><span class="sxs-lookup"><span data-stu-id="f30ce-178">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="f30ce-179">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f30ce-179">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="f30ce-180">En el explorador, agregue **/Person** a la dirección URL para abrir la página de la persona.</span><span class="sxs-lookup"><span data-stu-id="f30ce-180">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="f30ce-181">![Aplicación que se ejecuta por primera vez](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "primero la ejecución de la aplicación")</span><span class="sxs-lookup"><span data-stu-id="f30ce-181">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="f30ce-182">*Aplicación: primero ejecute*</span><span class="sxs-lookup"><span data-stu-id="f30ce-182">*Application: first run*</span></span>
3. <span data-ttu-id="f30ce-183">Ahora explorará las páginas de la persona y probar las operaciones CRUD.</span><span class="sxs-lookup"><span data-stu-id="f30ce-183">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="f30ce-184">Haga clic en **crear nuevo** para agregar una nueva persona.</span><span class="sxs-lookup"><span data-stu-id="f30ce-184">Click **Create New** to add a new person.</span></span> <span data-ttu-id="f30ce-185">Escriba un nombre y un apellido y haga clic en **crear**.</span><span class="sxs-lookup"><span data-stu-id="f30ce-185">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="f30ce-186">![Agregar una nueva persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "agregando una nueva persona")</span><span class="sxs-lookup"><span data-stu-id="f30ce-186">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="f30ce-187">*Agregar una nueva persona*</span><span class="sxs-lookup"><span data-stu-id="f30ce-187">*Adding a new person*</span></span>
    2. <span data-ttu-id="f30ce-188">En la lista de la persona, puede eliminar, editar o agregar elementos.</span><span class="sxs-lookup"><span data-stu-id="f30ce-188">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="f30ce-189">![lista de personas](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "lista de personas")</span><span class="sxs-lookup"><span data-stu-id="f30ce-189">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="f30ce-190">*Lista de personas*</span><span class="sxs-lookup"><span data-stu-id="f30ce-190">*Person list*</span></span>
    3. <span data-ttu-id="f30ce-191">Haga clic en **detalles** para abrir los detalles de la persona.</span><span class="sxs-lookup"><span data-stu-id="f30ce-191">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="f30ce-192">![Detalles de la persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "detalles de la persona.")</span><span class="sxs-lookup"><span data-stu-id="f30ce-192">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="f30ce-193">*Detalles de la persona.*</span><span class="sxs-lookup"><span data-stu-id="f30ce-193">*Person's details*</span></span>
4. <span data-ttu-id="f30ce-194">Cierre el explorador y vuelva a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f30ce-194">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="f30ce-195">Tenga en cuenta que ha creado el CRUD completo para la entidad person a lo largo de la aplicación - desde el modelo a las vistas, sin tener que escribir una sola línea de código.</span><span class="sxs-lookup"><span data-stu-id="f30ce-195">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="f30ce-196">Tarea 3: actualizando la base de datos mediante migraciones de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="f30ce-196">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="f30ce-197">En esta tarea se actualizará la base de datos mediante migraciones de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f30ce-197">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="f30ce-198">Descubrirá lo fácil que es cambiar el modelo y reflejar los cambios en las bases de datos mediante la característica de migraciones de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f30ce-198">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="f30ce-199">Abra la consola de administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="f30ce-199">Open the Package Manager Console.</span></span> <span data-ttu-id="f30ce-200">Seleccione **herramientas | Administrador de paquetes de biblioteca | Consola de administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="f30ce-200">Select **Tools | Library Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="f30ce-201">En la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="f30ce-201">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="f30ce-202">PMC</span><span class="sxs-lookup"><span data-stu-id="f30ce-202">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="f30ce-203">![Habilitar migraciones](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "habilitar migraciones")</span><span class="sxs-lookup"><span data-stu-id="f30ce-203">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="f30ce-204">*Habilitación de migraciones*</span><span class="sxs-lookup"><span data-stu-id="f30ce-204">*Enabling migrations*</span></span>

    <span data-ttu-id="f30ce-205">El comando de habilitación de la migración crea el **migraciones** carpeta, que contiene una secuencia de comandos para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f30ce-205">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="f30ce-206">![Carpeta de migraciones](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "carpeta migraciones")</span><span class="sxs-lookup"><span data-stu-id="f30ce-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="f30ce-207">*Carpeta de migraciones*</span><span class="sxs-lookup"><span data-stu-id="f30ce-207">*Migrations folder*</span></span>
3. <span data-ttu-id="f30ce-208">Abra el **Configuration.cs** archivo en la carpeta de migraciones.</span><span class="sxs-lookup"><span data-stu-id="f30ce-208">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="f30ce-209">Busque el constructor de clase y cambie el **AutomaticMigrationsEnabled** valor *true*.</span><span class="sxs-lookup"><span data-stu-id="f30ce-209">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="f30ce-210">Abra la clase Person y agregue un atributo para el nombre del medio.</span><span class="sxs-lookup"><span data-stu-id="f30ce-210">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="f30ce-211">Con este nuevo atributo, va a cambiar el modelo.</span><span class="sxs-lookup"><span data-stu-id="f30ce-211">With this new attribute, you are changing the model.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="f30ce-212">Seleccione **compilar | Compilar solución** en el menú para compilar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f30ce-212">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="f30ce-213">![Creación de la aplicación](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "compilar la aplicación")</span><span class="sxs-lookup"><span data-stu-id="f30ce-213">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="f30ce-214">*Compilar la aplicación*</span><span class="sxs-lookup"><span data-stu-id="f30ce-214">*Building the application*</span></span>
6. <span data-ttu-id="f30ce-215">En la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="f30ce-215">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="f30ce-216">PMC</span><span class="sxs-lookup"><span data-stu-id="f30ce-216">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="f30ce-217">Este comando busca cambios en los objetos de datos y, a continuación, agregará los comandos necesarios para modificar la base de datos en consecuencia.</span><span class="sxs-lookup"><span data-stu-id="f30ce-217">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="f30ce-218">![Agregar un segundo nombre](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "agregando un segundo nombre")</span><span class="sxs-lookup"><span data-stu-id="f30ce-218">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="f30ce-219">*Agregar un segundo nombre*</span><span class="sxs-lookup"><span data-stu-id="f30ce-219">*Adding a middle name*</span></span>
7. <span data-ttu-id="f30ce-220">(Opcional) Puede ejecutar el siguiente comando para generar un script SQL con la actualización diferencial.</span><span class="sxs-lookup"><span data-stu-id="f30ce-220">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="f30ce-221">Esto le permitirá actualizar manualmente la base de datos (en este caso no es necesario), o aplicar los cambios en otras bases de datos:</span><span class="sxs-lookup"><span data-stu-id="f30ce-221">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="f30ce-222">PMC</span><span class="sxs-lookup"><span data-stu-id="f30ce-222">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="f30ce-223">![Generar un script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "generar un script SQL")</span><span class="sxs-lookup"><span data-stu-id="f30ce-223">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="f30ce-224">*Generar un script SQL*</span><span class="sxs-lookup"><span data-stu-id="f30ce-224">*Generating a SQL script*</span></span>

    <span data-ttu-id="f30ce-225">![Actualización de secuencia de comandos SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "actualización de secuencia de comandos SQL")</span><span class="sxs-lookup"><span data-stu-id="f30ce-225">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="f30ce-226">*Actualización de secuencia de comandos SQL*</span><span class="sxs-lookup"><span data-stu-id="f30ce-226">*SQL Script update*</span></span>
8. <span data-ttu-id="f30ce-227">En la consola de administrador de paquetes, escriba el siguiente comando para actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="f30ce-227">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="f30ce-228">PMC</span><span class="sxs-lookup"><span data-stu-id="f30ce-228">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="f30ce-229">![Actualizando la base de datos](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "actualizar la base de datos")</span><span class="sxs-lookup"><span data-stu-id="f30ce-229">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="f30ce-230">*Actualizando la base de datos*</span><span class="sxs-lookup"><span data-stu-id="f30ce-230">*Updating the Database*</span></span>

    <span data-ttu-id="f30ce-231">Esto agregará la **MiddleName** columna en el **personas** tabla para que coincida con la definición actual de la **persona** clase.</span><span class="sxs-lookup"><span data-stu-id="f30ce-231">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="f30ce-232">Una vez que se actualiza la base de datos, haga clic en la carpeta de controlador y seleccione **Add | Controlador** para agregar el controlador de persona nuevo (terminar con los mismos valores).</span><span class="sxs-lookup"><span data-stu-id="f30ce-232">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="f30ce-233">Esto actualizará los métodos existentes y agregar el nuevo atributo de vistas.</span><span class="sxs-lookup"><span data-stu-id="f30ce-233">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="f30ce-234">![Adición de una actualización del controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "adición de una actualización del controlador")</span><span class="sxs-lookup"><span data-stu-id="f30ce-234">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="f30ce-235">*Actualizar el controlador*</span><span class="sxs-lookup"><span data-stu-id="f30ce-235">*Updating the controller*</span></span>
10. <span data-ttu-id="f30ce-236">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="f30ce-236">Click **Add**.</span></span> <span data-ttu-id="f30ce-237">A continuación, seleccione los valores **sobrescribir PersonController.cs** y **sobrescritura vistas asociadas** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="f30ce-237">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

   ![Agregar una sobrescritura de controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   <span data-ttu-id="f30ce-239">*Actualizar el controlador*</span><span class="sxs-lookup"><span data-stu-id="f30ce-239">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="f30ce-240">Task4: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="f30ce-240">Task4- Running the application</span></span>

1. <span data-ttu-id="f30ce-241">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f30ce-241">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="f30ce-242">Abra **/Person**.</span><span class="sxs-lookup"><span data-stu-id="f30ce-242">Open **/Person**.</span></span> <span data-ttu-id="f30ce-243">Tenga en cuenta que se conservan los datos, mientras que se ha agregado la columna middle name.</span><span class="sxs-lookup"><span data-stu-id="f30ce-243">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="f30ce-244">![El segundo apellido agregado](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "segundo nombre agregado")</span><span class="sxs-lookup"><span data-stu-id="f30ce-244">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="f30ce-245">*Segundo apellido agregado*</span><span class="sxs-lookup"><span data-stu-id="f30ce-245">*Middle Name added*</span></span>
3. <span data-ttu-id="f30ce-246">Si hace clic en **editar**, podrá agregar un segundo nombre a la persona actual.</span><span class="sxs-lookup"><span data-stu-id="f30ce-246">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="f30ce-247">![El segundo apellido edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "edition del segundo nombre")</span><span class="sxs-lookup"><span data-stu-id="f30ce-247">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="f30ce-248">Resumen</span><span class="sxs-lookup"><span data-stu-id="f30ce-248">Summary</span></span>

<span data-ttu-id="f30ce-249">En este laboratorio práctico, ha aprendido sencillos pasos para crear operaciones CRUD con Scaffolding de ASP.NET MVC 4 mediante cualquier clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="f30ce-249">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="f30ce-250">A continuación, ha aprendido a realizar una actualización de extremo a extremo en su aplicación - desde la base de datos a las vistas - mediante migraciones de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f30ce-250">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="f30ce-251">Apéndice A: instalación de Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="f30ce-251">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="f30ce-252">Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión utilizando el **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="f30ce-252">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="f30ce-253">Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* mediante *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="f30ce-253">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="f30ce-254">Vaya a [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="f30ce-254">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="f30ce-255">Como alternativa, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con SDK de Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="f30ce-255">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="f30ce-256">Haga clic en **instalar ahora**.</span><span class="sxs-lookup"><span data-stu-id="f30ce-256">Click on **Install Now**.</span></span> <span data-ttu-id="f30ce-257">Si no tienes **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="f30ce-257">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="f30ce-258">Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.</span><span class="sxs-lookup"><span data-stu-id="f30ce-258">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="f30ce-259">![Instale Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "instale Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="f30ce-259">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="f30ce-260">*Instale Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="f30ce-260">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="f30ce-261">Leer todos los productos las licencias y los términos y haga clic en **acepto** para continuar.</span><span class="sxs-lookup"><span data-stu-id="f30ce-261">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Acepte los términos de licencia](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="f30ce-263">*Acepte los términos de licencia*</span><span class="sxs-lookup"><span data-stu-id="f30ce-263">*Accepting the license terms*</span></span>
5. <span data-ttu-id="f30ce-264">Espere hasta que finalice el proceso de descarga e instalación.</span><span class="sxs-lookup"><span data-stu-id="f30ce-264">Wait until the downloading and installation process completes.</span></span>

    ![Progreso de la instalación](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="f30ce-266">*Progreso de la instalación*</span><span class="sxs-lookup"><span data-stu-id="f30ce-266">*Installation progress*</span></span>
6. <span data-ttu-id="f30ce-267">Cuando se complete la instalación, haga clic en **finalizar**.</span><span class="sxs-lookup"><span data-stu-id="f30ce-267">When the installation completes, click **Finish**.</span></span>

    ![Instalación completada](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="f30ce-269">*Instalación completada*</span><span class="sxs-lookup"><span data-stu-id="f30ce-269">*Installation completed*</span></span>
7. <span data-ttu-id="f30ce-270">Haga clic en **Exit** para cerrar el instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="f30ce-270">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="f30ce-271">Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **VS Express**&quot;, a continuación, haga clic en el **VS Express para Web** icono.</span><span class="sxs-lookup"><span data-stu-id="f30ce-271">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para el icono de Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="f30ce-273">*VS Express para el icono de Web*</span><span class="sxs-lookup"><span data-stu-id="f30ce-273">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="f30ce-274">Apéndice B: uso de fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="f30ce-274">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="f30ce-275">Con fragmentos de código, tiene todo el código que necesita a su alcance.</span><span class="sxs-lookup"><span data-stu-id="f30ce-275">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="f30ce-276">El documento de laboratorio le dirá exactamente cuándo se pueden utilizar, como se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="f30ce-276">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="f30ce-277">![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "fragmentos de código en Visual Studio para insertar código en el proyecto")</span><span class="sxs-lookup"><span data-stu-id="f30ce-277">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="f30ce-278">*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*</span><span class="sxs-lookup"><span data-stu-id="f30ce-278">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="f30ce-279">***Para agregar un fragmento de código mediante el teclado (solo C#)***</span><span class="sxs-lookup"><span data-stu-id="f30ce-279">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="f30ce-280">Coloque el cursor donde desea insertar el código.</span><span class="sxs-lookup"><span data-stu-id="f30ce-280">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="f30ce-281">Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).</span><span class="sxs-lookup"><span data-stu-id="f30ce-281">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="f30ce-282">Observe cómo IntelliSense muestra los nombres de fragmentos de código coincidentes.</span><span class="sxs-lookup"><span data-stu-id="f30ce-282">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="f30ce-283">Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).</span><span class="sxs-lookup"><span data-stu-id="f30ce-283">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="f30ce-284">Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.</span><span class="sxs-lookup"><span data-stu-id="f30ce-284">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="f30ce-285">![Comience a escribir el nombre del fragmento](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "comience a escribir el nombre del fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="f30ce-285">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="f30ce-286">*Comience a escribir el nombre del fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="f30ce-286">*Start typing the snippet name*</span></span>

<span data-ttu-id="f30ce-287">![Presione la tecla Tab para seleccionar el fragmento de código resaltada](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "presione Tab para seleccionar el fragmento de código resaltada")</span><span class="sxs-lookup"><span data-stu-id="f30ce-287">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="f30ce-288">*Presione la tecla Tab para seleccionar el fragmento de código resaltada*</span><span class="sxs-lookup"><span data-stu-id="f30ce-288">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="f30ce-289">![Vuelva a presionar Tab y el fragmento de código se expandirán](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "vuelva a presionar Tab y el fragmento de código se expandirán")</span><span class="sxs-lookup"><span data-stu-id="f30ce-289">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="f30ce-290">*Vuelva a presionar Tab y el fragmento de código se expandirán*</span><span class="sxs-lookup"><span data-stu-id="f30ce-290">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="f30ce-291">***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="f30ce-291">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="f30ce-292">Haga doble clic en el desea insertar el fragmento de código.</span><span class="sxs-lookup"><span data-stu-id="f30ce-292">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="f30ce-293">Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.</span><span class="sxs-lookup"><span data-stu-id="f30ce-293">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="f30ce-294">Seleccione el fragmento de código relevante en la lista, haciendo clic en él.</span><span class="sxs-lookup"><span data-stu-id="f30ce-294">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="f30ce-295">![Con el botón secundario donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="f30ce-295">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="f30ce-296">*Haga doble clic donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="f30ce-296">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="f30ce-297">![Elegir el fragmento de código relevante en la lista, hacer clic en ella](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "elegir el fragmento de código relevante en la lista, hacer clic en ella")</span><span class="sxs-lookup"><span data-stu-id="f30ce-297">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="f30ce-298">*Elegir el fragmento de código relevante en la lista, hacer clic en ella*</span><span class="sxs-lookup"><span data-stu-id="f30ce-298">*Pick the relevant snippet from the list, by clicking on it*</span></span>
