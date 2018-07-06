---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: Las aplicaciones auxiliares de ASP.NET MVC 4, formularios y la validación | Microsoft Docs
author: rick-anderson
description: En los modelos de ASP.NET MVC 4 y laboratorios prácticos de acceso de datos, ha estado cargar y mostrar los datos de la base de datos. En este laboratorio práctico, agregará a la...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: a84e35695fa08ac1bd4834d2803d2be76f863e5b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815926"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="096e5-104">Validación, formularios y aplicaciones auxiliares de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="096e5-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>

<span data-ttu-id="096e5-105">por [campamentos Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="096e5-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="096e5-106">Descargue el Kit de aprendizaje de campamentos de Web</span><span class="sxs-lookup"><span data-stu-id="096e5-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="096e5-107">En **acceso a datos y modelos de ASP.NET MVC 4** laboratorios prácticos, han sido cargar y mostrar los datos de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="096e5-107">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="096e5-108">En este laboratorio práctico, agregará el **Music Store** la capacidad de editar los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="096e5-108">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>

<span data-ttu-id="096e5-109">Con ese objetivo en mente, creará primero el controlador que será compatible con las acciones de creación, lectura, actualización y eliminación (CRUD) de álbumes.</span><span class="sxs-lookup"><span data-stu-id="096e5-109">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="096e5-110">Se generará una plantilla de vista de índice que aprovecha la característica de scaffolding de ASP.NET MVC para mostrar las propiedades de los álbumes en una tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="096e5-110">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="096e5-111">Para mejorar esa vista, deberá agregar una aplicación auxiliar HTML personalizada que se truncará descripciones largas.</span><span class="sxs-lookup"><span data-stu-id="096e5-111">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>

<span data-ttu-id="096e5-112">Después, agregará la edición y crear vistas que se pueden modificar los álbumes en la base de datos, con la Ayuda de los elementos de formulario como listas desplegables.</span><span class="sxs-lookup"><span data-stu-id="096e5-112">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>

<span data-ttu-id="096e5-113">Por último, permitirá a los usuarios eliminar un álbum y también se les impedirá introducir datos incorrectos al validar su entrada.</span><span class="sxs-lookup"><span data-stu-id="096e5-113">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>

<span data-ttu-id="096e5-114">Esta práctica se da por supuesto que tiene conocimientos básicos de **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="096e5-114">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="096e5-115">Si no ha usado **ASP.NET MVC** antes, le recomendamos que repase **aspectos básicos de ASP.NET MVC** laboratorios prácticos.</span><span class="sxs-lookup"><span data-stu-id="096e5-115">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="096e5-116">Esta práctica le guiará a través de las mejoras y nuevas características descritas anteriormente aplicando cambios menores a una aplicación Web de ejemplo proporcionada en la carpeta de origen.</span><span class="sxs-lookup"><span data-stu-id="096e5-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="096e5-117">Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de entrenamiento campamentos de Web, que está disponible en [versiones de Microsoft de Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="096e5-117">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="096e5-118">Está disponible en el proyecto específico para este laboratorio [las aplicaciones auxiliares de ASP.NET MVC 4, formularios y la validación](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span><span class="sxs-lookup"><span data-stu-id="096e5-118">The project specific to this lab is available at [ASP.NET MVC 4 Helpers, Forms and Validation](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="096e5-119">Objetivos</span><span class="sxs-lookup"><span data-stu-id="096e5-119">Objectives</span></span>

<span data-ttu-id="096e5-120">En este laboratorio práctico, aprenderá cómo:</span><span class="sxs-lookup"><span data-stu-id="096e5-120">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="096e5-121">Crear un controlador para que admita operaciones CRUD</span><span class="sxs-lookup"><span data-stu-id="096e5-121">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="096e5-122">Generar una vista de índice para mostrar las propiedades de entidad en una tabla HTML</span><span class="sxs-lookup"><span data-stu-id="096e5-122">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="096e5-123">Agregar una aplicación auxiliar HTML personalizada</span><span class="sxs-lookup"><span data-stu-id="096e5-123">Add a custom HTML helper</span></span>
- <span data-ttu-id="096e5-124">Crear y personalizar una vista de edición</span><span class="sxs-lookup"><span data-stu-id="096e5-124">Create and customize an Edit View</span></span>
- <span data-ttu-id="096e5-125">Diferenciar entre los métodos de acción que reaccionan a HTTP-GET o las llamadas HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="096e5-125">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="096e5-126">Agregar y personalizar una instrucción Create View</span><span class="sxs-lookup"><span data-stu-id="096e5-126">Add and customize a Create View</span></span>
- <span data-ttu-id="096e5-127">Identificador de la eliminación de una entidad</span><span class="sxs-lookup"><span data-stu-id="096e5-127">Handle the deletion of an entity</span></span>
- <span data-ttu-id="096e5-128">Validar entrada de usuario</span><span class="sxs-lookup"><span data-stu-id="096e5-128">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="096e5-129">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="096e5-129">Prerequisites</span></span>

<span data-ttu-id="096e5-130">Debe tener los elementos siguientes para completar esta práctica:</span><span class="sxs-lookup"><span data-stu-id="096e5-130">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="096e5-131">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (leer [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).</span><span class="sxs-lookup"><span data-stu-id="096e5-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="096e5-132">Programa de instalación</span><span class="sxs-lookup"><span data-stu-id="096e5-132">Setup</span></span>

<span data-ttu-id="096e5-133">**Instalación de fragmentos de código**</span><span class="sxs-lookup"><span data-stu-id="096e5-133">**Installing Code Snippets**</span></span>

<span data-ttu-id="096e5-134">Para mayor comodidad, gran parte del código que se va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="096e5-134">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="096e5-135">Para instalar los fragmentos de código que se ejecute **.\Source\Setup\CodeSnippets.vsi** archivo.</span><span class="sxs-lookup"><span data-stu-id="096e5-135">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="096e5-136">Si no está familiarizado con los fragmentos de código de Visual Studio y desea aprender a usarlas, puede consultar el apéndice de este documento &quot; [Apéndice B: Using Code Snippets](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="096e5-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="096e5-137">Ejercicios</span><span class="sxs-lookup"><span data-stu-id="096e5-137">Exercises</span></span>

<span data-ttu-id="096e5-138">Los ejercicios siguientes constituyen este laboratorio práctico:</span><span class="sxs-lookup"><span data-stu-id="096e5-138">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="096e5-139">Crear el controlador de Store Manager y su vista de índice</span><span class="sxs-lookup"><span data-stu-id="096e5-139">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="096e5-140">Agregar una aplicación auxiliar HTML</span><span class="sxs-lookup"><span data-stu-id="096e5-140">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="096e5-141">Creación de la vista de edición</span><span class="sxs-lookup"><span data-stu-id="096e5-141">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="096e5-142">Agregar una vista Create</span><span class="sxs-lookup"><span data-stu-id="096e5-142">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="096e5-143">Eliminación de control</span><span class="sxs-lookup"><span data-stu-id="096e5-143">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="096e5-144">Agregar una validación</span><span class="sxs-lookup"><span data-stu-id="096e5-144">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="096e5-145">Uso de jQuery discreta en el lado cliente</span><span class="sxs-lookup"><span data-stu-id="096e5-145">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="096e5-146">Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debe obtener después de completar los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="096e5-146">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="096e5-147">Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="096e5-147">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="096e5-148">Tiempo estimado para completar esta práctica: **60 minutos**</span><span class="sxs-lookup"><span data-stu-id="096e5-148">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="096e5-149">Ejercicio 1: Crear el controlador de Store Manager y su vista de índice</span><span class="sxs-lookup"><span data-stu-id="096e5-149">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="096e5-150">En este ejercicio, obtendrá información sobre cómo crear un nuevo controlador para admitir operaciones CRUD, personalizar su método de acción de índice para devolver una lista de álbumes de la base de datos y, por último, generar una plantilla de vista de índice que aprovecha el scaffolding de ASP.NET MVC función para mostrar las propiedades de los álbumes en una tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="096e5-150">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="096e5-151">Tarea 1: crear el StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="096e5-151">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="096e5-152">En esta tarea, creará un nuevo controlador denominado **StoreManagerController** para admitir las operaciones CRUD.</span><span class="sxs-lookup"><span data-stu-id="096e5-152">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="096e5-153">Abra el **comenzar** solución ubicado en **origen/Ex1-CreatingTheStoreManagerController/inicio/** carpeta.</span><span class="sxs-lookup"><span data-stu-id="096e5-153">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

   1. <span data-ttu-id="096e5-154">Deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="096e5-154">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="096e5-155">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="096e5-155">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="096e5-156">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="096e5-156">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="096e5-157">Por último, compile la solución haciendo **compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="096e5-157">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="096e5-158">Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="096e5-158">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="096e5-159">Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="096e5-159">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="096e5-160">Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="096e5-160">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="096e5-161">Agregar un nuevo controlador.</span><span class="sxs-lookup"><span data-stu-id="096e5-161">Add a new controller.</span></span> <span data-ttu-id="096e5-162">Para ello, haga clic en el **controladores** carpeta en el Explorador de soluciones, seleccione **agregar** y, a continuación, el **controlador** comando.</span><span class="sxs-lookup"><span data-stu-id="096e5-162">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="096e5-163">Cambiar el **controlador** **nombre** a **StoreManagerController** y asegúrese de que la opción **controlador de MVC con acciones de lectura/escritura vacías**está seleccionada.</span><span class="sxs-lookup"><span data-stu-id="096e5-163">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="096e5-164">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="096e5-164">Click **Add**.</span></span>

    <span data-ttu-id="096e5-165">![Cuadro de diálogo Agregar controlador](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "cuadro de diálogo Agregar controlador")</span><span class="sxs-lookup"><span data-stu-id="096e5-165">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="096e5-166">*Agregar cuadro de diálogo de controlador*</span><span class="sxs-lookup"><span data-stu-id="096e5-166">*Add Controller Dialog*</span></span>

    <span data-ttu-id="096e5-167">Se genera una nueva clase de controlador.</span><span class="sxs-lookup"><span data-stu-id="096e5-167">A new Controller class is generated.</span></span> <span data-ttu-id="096e5-168">Puesto que indican para agregar acciones de lectura/escritura, los métodos auxiliares para los usuarios, acciones comunes de CRUD se crean con comentarios TODO rellenados, preguntar para incluir la lógica específica de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="096e5-168">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="096e5-169">Tarea 2: personalizar el índice StoreManager</span><span class="sxs-lookup"><span data-stu-id="096e5-169">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="096e5-170">En esta tarea, personalizará el método de acción del índice StoreManager para devolver una vista con la lista de álbumes de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="096e5-170">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="096e5-171">En la clase StoreManagerController, agregue la siguiente *mediante* directivas.</span><span class="sxs-lookup"><span data-stu-id="096e5-171">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="096e5-172">(Código de fragmento de código - *las aplicaciones auxiliares de ASP.NET MVC 4 y formularios y validación: Ex1 mediante MvcMusicStore*)</span><span class="sxs-lookup"><span data-stu-id="096e5-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="096e5-173">Agregar un campo a la **StoreManagerController** para contener una instancia de **MusicStoreEntities.**</span><span class="sxs-lookup"><span data-stu-id="096e5-173">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="096e5-174">(Código de fragmento de código - *aplicaciones auxiliares de ASP.NET MVC 4 y formularios y validación: Ex1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="096e5-174">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="096e5-175">Implementar la acción del índice de StoreManagerController para devolver una vista con la lista de álbumes.</span><span class="sxs-lookup"><span data-stu-id="096e5-175">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="096e5-176">La lógica de la acción de controlador será muy similar a la acción del índice del StoreController escrito anteriormente.</span><span class="sxs-lookup"><span data-stu-id="096e5-176">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="096e5-177">Usar LINQ para recuperar todos los álbumes, incluida la información de género y el intérprete para su presentación.</span><span class="sxs-lookup"><span data-stu-id="096e5-177">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="096e5-178">(Código de fragmento de código - *aplicaciones auxiliares de ASP.NET MVC 4 y formularios y validación: Ex1 StoreManagerController índice*)</span><span class="sxs-lookup"><span data-stu-id="096e5-178">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="096e5-179">Tarea 3: creación de la vista de índice</span><span class="sxs-lookup"><span data-stu-id="096e5-179">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="096e5-180">En esta tarea, creará la plantilla de vista de índice para mostrar la lista de álbumes devuelto por la **StoreManager** controlador.</span><span class="sxs-lookup"><span data-stu-id="096e5-180">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="096e5-181">Antes de crear la nueva plantilla de vista, debe compilar el proyecto para que el **Agregar cuadro de diálogo de vista** conoce la **álbum** clase se debe utilizar.</span><span class="sxs-lookup"><span data-stu-id="096e5-181">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="096e5-182">Seleccione **compilar | Compilar MvcMusicStore** para compilar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="096e5-182">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="096e5-183">Haga clic en el **índice** método de acción y seleccione **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="096e5-183">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="096e5-184">Esto le llevará a la **agregar vista** cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="096e5-184">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="096e5-185">![Agregar vista](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "agregar vista")</span><span class="sxs-lookup"><span data-stu-id="096e5-185">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="096e5-186">*Agregar una vista desde dentro del método de índice*</span><span class="sxs-lookup"><span data-stu-id="096e5-186">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="096e5-187">En el cuadro de diálogo Agregar vista, compruebe que el nombre de vista es **índice**.</span><span class="sxs-lookup"><span data-stu-id="096e5-187">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="096e5-188">Seleccione el **crear una vista fuertemente tipada** opción y seleccione **álbum (MvcMusicStore.Models)** desde el **clase modelo** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="096e5-188">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="096e5-189">Seleccione **lista** desde el **plantilla Scaffold** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="096e5-189">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="096e5-190">Deje el **motor de vista** a **Razor** y los demás campos con su valor predeterminado de valor y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="096e5-190">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="096e5-191">![Agregar una vista de índice](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "agregar una vista de índice")</span><span class="sxs-lookup"><span data-stu-id="096e5-191">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="096e5-192">*Agregar una vista de índice*</span><span class="sxs-lookup"><span data-stu-id="096e5-192">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="096e5-193">Tarea 4: personalizar las plantillas scaffold de la vista de índice</span><span class="sxs-lookup"><span data-stu-id="096e5-193">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="096e5-194">En esta tarea, se ajustará la plantilla de vista simple creada con la característica de scaffolding de ASP.NET MVC para que muestre los campos que desee.</span><span class="sxs-lookup"><span data-stu-id="096e5-194">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="096e5-195">El **scaffolding** compatibilidad dentro de ASP.NET MVC genera una plantilla de vista simple que enumera todos los campos en el modelo de álbum.</span><span class="sxs-lookup"><span data-stu-id="096e5-195">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="096e5-196">**Scaffolding** proporciona una forma rápida de empezar a trabajar en una vista fuertemente tipada: en lugar de tener que escribir manualmente la plantilla de vista, scaffolding rápidamente genera una plantilla predeterminada y, a continuación, puede modificar el código generado.</span><span class="sxs-lookup"><span data-stu-id="096e5-196">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>


1. <span data-ttu-id="096e5-197">Revise el código creado.</span><span class="sxs-lookup"><span data-stu-id="096e5-197">Review the code created.</span></span> <span data-ttu-id="096e5-198">La lista de campos generada formarán parte de los siguientes valores de tabla HTML que **Scaffolding** usa para mostrar datos tabulares.</span><span class="sxs-lookup"><span data-stu-id="096e5-198">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="096e5-199">Reemplace el **&lt;tabla&gt;** código con el código siguiente para mostrar solamente el **género**, **artista**, **título del álbum**, y **precio** campos.</span><span class="sxs-lookup"><span data-stu-id="096e5-199">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="096e5-200">Esto elimina la **AlbumId** y **URL carátulas de álbum** columnas.</span><span class="sxs-lookup"><span data-stu-id="096e5-200">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="096e5-201">Además, cambia GenreId y ArtistId columnas para mostrar sus propiedades de la clase vinculado del **Artist.Name** y **Genre.Name**y quita el **detalles** vínculo.</span><span class="sxs-lookup"><span data-stu-id="096e5-201">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="096e5-202">Cambie las siguientes descripciones.</span><span class="sxs-lookup"><span data-stu-id="096e5-202">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="096e5-203">Tarea 5: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="096e5-203">Task 5 - Running the Application</span></span>

<span data-ttu-id="096e5-204">En esta tarea, probará que la **StoreManager** **índice** plantilla de vista muestra una lista de álbumes de acuerdo con el diseño de los pasos anteriores.</span><span class="sxs-lookup"><span data-stu-id="096e5-204">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="096e5-205">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="096e5-205">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="096e5-206">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="096e5-206">The project starts in the Home page.</span></span> <span data-ttu-id="096e5-207">Cambiar la dirección URL de **/StoreManager** para comprobar que se muestra una lista de álbumes, que muestra su **título**, **artista** y **género**.</span><span class="sxs-lookup"><span data-stu-id="096e5-207">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="096e5-208">![Examinar la lista de álbumes](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "examinar la lista de álbumes")</span><span class="sxs-lookup"><span data-stu-id="096e5-208">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="096e5-209">*Examinar la lista de álbumes*</span><span class="sxs-lookup"><span data-stu-id="096e5-209">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="096e5-210">Ejercicio 2: Agregar una aplicación auxiliar HTML</span><span class="sxs-lookup"><span data-stu-id="096e5-210">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="096e5-211">La página de índice StoreManager tiene un problema potencial: propiedades de título y el nombre del intérprete tanto pueden ser lo suficientemente largo para deshacerse del formato de tabla.</span><span class="sxs-lookup"><span data-stu-id="096e5-211">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="096e5-212">En este ejercicio obtendrá información sobre cómo agregar una aplicación auxiliar HTML personalizada para truncar ese texto.</span><span class="sxs-lookup"><span data-stu-id="096e5-212">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="096e5-213">En la ilustración siguiente, puede ver cómo se modifica el formato debido a la longitud del texto cuando se usa un tamaño pequeño de explorador.</span><span class="sxs-lookup"><span data-stu-id="096e5-213">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="096e5-214">![Examinar la lista de álbumes con texto truncado no](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "no examinar la lista de álbumes con texto truncado")</span><span class="sxs-lookup"><span data-stu-id="096e5-214">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="096e5-215">*No examinar la lista de álbumes con texto truncado*</span><span class="sxs-lookup"><span data-stu-id="096e5-215">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="096e5-216">Tarea 1: Extender la aplicación auxiliar HTML</span><span class="sxs-lookup"><span data-stu-id="096e5-216">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="096e5-217">En esta tarea, agregará un nuevo método **Truncate** a la **HTML** objeto expuesto en las vistas de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="096e5-217">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="096e5-218">Para ello, implementará un **método de extensión** a la integrada **System.Web.Mvc.HtmlHelper** clase proporcionada por ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="096e5-218">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="096e5-219">Para obtener más información acerca de **métodos de extensión**, visite este artículo de msdn.</span><span class="sxs-lookup"><span data-stu-id="096e5-219">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="096e5-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="096e5-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>


1. <span data-ttu-id="096e5-221">Abra el **comenzar** solución ubicado en **origen/Ex2-AddingAnHTMLHelper/inicio/** carpeta.</span><span class="sxs-lookup"><span data-stu-id="096e5-221">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="096e5-222">En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="096e5-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="096e5-223">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="096e5-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="096e5-224">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="096e5-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="096e5-225">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="096e5-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="096e5-226">Por último, compile la solución haciendo **compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="096e5-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="096e5-227">Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="096e5-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="096e5-228">Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="096e5-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="096e5-229">Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="096e5-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="096e5-230">Abra la vista de índice del StoreManager.</span><span class="sxs-lookup"><span data-stu-id="096e5-230">Open StoreManager's Index View.</span></span> <span data-ttu-id="096e5-231">Para ello, en el Explorador de soluciones, expanda el **vistas** carpeta, el **StoreManager** y abra el **Index.cshtml** archivo.</span><span class="sxs-lookup"><span data-stu-id="096e5-231">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="096e5-232">Agregue el código siguiente el <strong>@model</strong> directiva para definir el <strong>Truncate</strong> método auxiliar.</span><span class="sxs-lookup"><span data-stu-id="096e5-232">Add the following code below the <strong>@model</strong> directive to define the <strong>Truncate</strong> helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="096e5-233">Tarea 2 - truncar el texto de la página</span><span class="sxs-lookup"><span data-stu-id="096e5-233">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="096e5-234">En esta tarea, utilizará el **Truncate** método truncar el texto en la plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="096e5-234">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="096e5-235">Abra la vista de índice del StoreManager.</span><span class="sxs-lookup"><span data-stu-id="096e5-235">Open StoreManager's Index View.</span></span> <span data-ttu-id="096e5-236">Para ello, en el Explorador de soluciones, expanda el **vistas** carpeta, el **StoreManager** y abra el **Index.cshtml** archivo.</span><span class="sxs-lookup"><span data-stu-id="096e5-236">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="096e5-237">Reemplace las líneas que muestran la **el nombre del intérprete** y del álbum **título**.</span><span class="sxs-lookup"><span data-stu-id="096e5-237">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="096e5-238">Para ello, reemplace las líneas siguientes.</span><span class="sxs-lookup"><span data-stu-id="096e5-238">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="096e5-239">Tarea 3: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="096e5-239">Task 3 - Running the Application</span></span>

<span data-ttu-id="096e5-240">En esta tarea, probará que la **StoreManager** **índice** ver plantilla trunca el título y el nombre del intérprete del álbum.</span><span class="sxs-lookup"><span data-stu-id="096e5-240">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="096e5-241">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="096e5-241">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="096e5-242">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="096e5-242">The project starts in the Home page.</span></span> <span data-ttu-id="096e5-243">Cambiar la dirección URL de **/StoreManager** para comprobar ese tiempo textos en el **título** y **artista** columna se truncan.</span><span class="sxs-lookup"><span data-stu-id="096e5-243">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="096e5-244">![Trunca los nombres de los títulos y los artistas](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "trunca los nombres de los títulos y los artistas")</span><span class="sxs-lookup"><span data-stu-id="096e5-244">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="096e5-245">*Nombres de intérpretes y títulos truncados*</span><span class="sxs-lookup"><span data-stu-id="096e5-245">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="096e5-246">Ejercicio 3: Creación de la vista de edición</span><span class="sxs-lookup"><span data-stu-id="096e5-246">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="096e5-247">En este ejercicio, obtendrá información sobre cómo crear un formulario para permitir que los responsables de tienda editar un álbum.</span><span class="sxs-lookup"><span data-stu-id="096e5-247">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="096e5-248">Examinará el **/StoreManager/Edit/id** URL (**id** está el identificador único del álbum que desea editar), lo que hace que una llamada HTTP-GET en el servidor.</span><span class="sxs-lookup"><span data-stu-id="096e5-248">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="096e5-249">El método de acción Edit de controlador se recupere el álbum adecuado de la base de datos, cree un **StoreManagerViewModel** objeto encapsularlo (junto con una lista de los artistas y géneros) y, a continuación, pasarlos para una plantilla de vista representar la página HTML al usuario.</span><span class="sxs-lookup"><span data-stu-id="096e5-249">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="096e5-250">Esta página contiene un **&lt;formulario&gt;** elemento con los cuadros de texto y listas desplegables para editar las propiedades del álbum.</span><span class="sxs-lookup"><span data-stu-id="096e5-250">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="096e5-251">Una vez que el usuario actualiza los valores de formulario de álbum y hace clic en el **guardar** botón, los cambios se envían a través de devolución de llamada HTTP-POST a **/StoreManager/Edit/id**. Aunque la dirección URL sigue siendo el mismo que en la última llamada, ASP.NET MVC que identifica este tiempo es una solicitud HTTP-POST y, por tanto, se ejecuta un método de acción Edit distintos (uno decorado con **[HttpPost]**).</span><span class="sxs-lookup"><span data-stu-id="096e5-251">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="096e5-252">Tarea 1: implementar el método de acción Edit de HTTP-GET</span><span class="sxs-lookup"><span data-stu-id="096e5-252">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="096e5-253">En esta tarea, implementará la versión de HTTP-GET del método de acción de edición para recuperar el álbum adecuado de la base de datos, así como una lista de todos los géneros y artistas.</span><span class="sxs-lookup"><span data-stu-id="096e5-253">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="096e5-254">Empaquetará estos datos de en el **StoreManagerViewModel** definido en el último paso, que, a continuación, se pasará a una plantilla de vista para representar la respuesta con el objeto.</span><span class="sxs-lookup"><span data-stu-id="096e5-254">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="096e5-255">Abra el **comenzar** solución ubicado en **origen/Ex3-CreatingTheEditView/inicio/** carpeta.</span><span class="sxs-lookup"><span data-stu-id="096e5-255">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="096e5-256">En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="096e5-256">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="096e5-257">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="096e5-257">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="096e5-258">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="096e5-258">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="096e5-259">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="096e5-259">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="096e5-260">Por último, compile la solución haciendo **compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="096e5-260">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="096e5-261">Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="096e5-261">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="096e5-262">Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="096e5-262">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="096e5-263">Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="096e5-263">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="096e5-264">Abra el **StoreManagerController** clase.</span><span class="sxs-lookup"><span data-stu-id="096e5-264">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="096e5-265">Para ello, expanda el **controladores** carpeta y haga doble clic en **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="096e5-265">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="096e5-266">Reemplace el **HTTP-GET editar** método de acción con el código siguiente para recuperar la correspondiente **álbum** , así como la **géneros** y **artistas**enumera.</span><span class="sxs-lookup"><span data-stu-id="096e5-266">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="096e5-267">(Código de fragmento de código - *las aplicaciones auxiliares de ASP.NET MVC 4 y formularios y validación: Ex3 StoreManagerController HTTP-GET Editar acción*)</span><span class="sxs-lookup"><span data-stu-id="096e5-267">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="096e5-268">Usa **System.Web.Mvc** **SelectList** para artistas y géneros en lugar de la **System.Collections.Generic** lista.</span><span class="sxs-lookup"><span data-stu-id="096e5-268">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="096e5-269">**SelectList** es una forma más limpia para rellenar las listas desplegables HTML y administran cosas como la selección actual.</span><span class="sxs-lookup"><span data-stu-id="096e5-269">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="096e5-270">Creación de instancias y configuración posterior de estos objetos de ViewModel en la acción de controlador hará que el escenario de formulario de edición más limpio.</span><span class="sxs-lookup"><span data-stu-id="096e5-270">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="096e5-271">Tarea 2: crear la vista de edición</span><span class="sxs-lookup"><span data-stu-id="096e5-271">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="096e5-272">En esta tarea, creará una plantilla de vista de edición que más adelante se mostrará las propiedades del álbum.</span><span class="sxs-lookup"><span data-stu-id="096e5-272">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="096e5-273">Crear la vista de edición.</span><span class="sxs-lookup"><span data-stu-id="096e5-273">Create the Edit View.</span></span> <span data-ttu-id="096e5-274">Para ello, haga clic en el **editar** método de acción y seleccione **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="096e5-274">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="096e5-275">En el cuadro de diálogo Agregar vista, compruebe que el nombre de vista es **editar**.</span><span class="sxs-lookup"><span data-stu-id="096e5-275">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="096e5-276">Compruebe el **crear una vista fuertemente tipada** casilla y seleccione **álbum (MvcMusicStore.Models)** desde el **Ver clase de datos** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="096e5-276">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="096e5-277">Seleccione **editar** desde el **plantilla Scaffold** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="096e5-277">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="096e5-278">Deje los demás campos con sus valores predeterminados y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="096e5-278">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="096e5-279">![Agregar una vista de edición](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "agregar una vista de edición")</span><span class="sxs-lookup"><span data-stu-id="096e5-279">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="096e5-280">*Agregar una vista de edición*</span><span class="sxs-lookup"><span data-stu-id="096e5-280">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="096e5-281">Tarea 3: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="096e5-281">Task 3 - Running the Application</span></span>

<span data-ttu-id="096e5-282">En esta tarea, probará que la **StoreManager** **editar** página de vista muestra los valores de las propiedades del álbum pasado como parámetro.</span><span class="sxs-lookup"><span data-stu-id="096e5-282">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="096e5-283">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="096e5-283">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="096e5-284">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="096e5-284">The project starts in the Home page.</span></span> <span data-ttu-id="096e5-285">Cambiar la dirección URL de **/StoreManager/Edit/1** para comprobar que se muestran los valores de las propiedades del álbum pasado.</span><span class="sxs-lookup"><span data-stu-id="096e5-285">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="096e5-286">![Exploración de la vista de edición del álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "exploración Editar vista del álbum")</span><span class="sxs-lookup"><span data-stu-id="096e5-286">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="096e5-287">*Exploración de la vista de edición del álbum*</span><span class="sxs-lookup"><span data-stu-id="096e5-287">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="096e5-288">Tarea 4: implementación de listas desplegables en la plantilla del Editor de álbum</span><span class="sxs-lookup"><span data-stu-id="096e5-288">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="096e5-289">En esta tarea, agregará listas desplegables a la plantilla de vista que se creó en la última tarea, para que el usuario puede seleccionar entre una lista de los artistas y géneros.</span><span class="sxs-lookup"><span data-stu-id="096e5-289">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="096e5-290">Reemplazar todo el **álbum** fieldset código con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="096e5-290">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="096e5-291">Un **Html.DropDownList** auxiliar se agregó para representar listas desplegables para elegir los artistas y géneros.</span><span class="sxs-lookup"><span data-stu-id="096e5-291">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="096e5-292">Los parámetros pasados a **Html.DropDownList** son:</span><span class="sxs-lookup"><span data-stu-id="096e5-292">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="096e5-293">El nombre del campo de formulario (**&quot;ArtistId&quot;**).</span><span class="sxs-lookup"><span data-stu-id="096e5-293">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="096e5-294">El **SelectList** de valores de la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="096e5-294">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="096e5-295">Tarea 5: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="096e5-295">Task 5 - Running the Application</span></span>

<span data-ttu-id="096e5-296">En esta tarea, probará que la **StoreManager** **editar** ver página muestra listas desplegables en lugar de campos de texto del artista y el Id. de género.</span><span class="sxs-lookup"><span data-stu-id="096e5-296">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="096e5-297">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="096e5-297">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="096e5-298">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="096e5-298">The project starts in the Home page.</span></span> <span data-ttu-id="096e5-299">Cambiar la dirección URL de **/StoreManager/Edit/1** para comprobar que muestra en lugar de campos de texto del artista y el Id. de género listas desplegables.</span><span class="sxs-lookup"><span data-stu-id="096e5-299">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="096e5-300">![Exploración de la vista de edición del álbum con listas desplegables](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "vista de edición del álbum de exploración con listas desplegables")</span><span class="sxs-lookup"><span data-stu-id="096e5-300">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="096e5-301">*Exploración de la vista de edición del álbum, esta vez con listas desplegables*</span><span class="sxs-lookup"><span data-stu-id="096e5-301">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="096e5-302">Tarea 6: implementar el método de acción Edit de HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="096e5-302">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="096e5-303">Ahora que la vista de edición se muestra según lo esperado, deberá implementar el método de acción de editar HTTP-POST para guardar los cambios realizados en el álbum.</span><span class="sxs-lookup"><span data-stu-id="096e5-303">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="096e5-304">Cierre el explorador si es necesario, para volver a la ventana de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="096e5-304">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="096e5-305">Abra **StoreManagerController** desde el **controladores** carpeta.</span><span class="sxs-lookup"><span data-stu-id="096e5-305">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="096e5-306">Reemplace **HTTP-POST editar** código del método de acción con los siguientes (tenga en cuenta que el método que se debe reemplazar es una versión sobrecargada que recibe dos parámetros):</span><span class="sxs-lookup"><span data-stu-id="096e5-306">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="096e5-307">(Código de fragmento de código - *las aplicaciones auxiliares de ASP.NET MVC 4 y formularios y validación: Ex3 StoreManagerController HTTP-POST Editar acción*)</span><span class="sxs-lookup"><span data-stu-id="096e5-307">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="096e5-308">Este método se ejecutará cuando el usuario hace clic en el **guardar** botón de la vista y realiza una solicitud HTTP-POST de los valores de formulario al servidor para hacer que persistan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="096e5-308">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="096e5-309">El elemento decorator **[HttpPost]** indica que el método debe usarse para los escenarios de HTTP-POST.</span><span class="sxs-lookup"><span data-stu-id="096e5-309">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="096e5-310">El método toma un **álbum** objeto.</span><span class="sxs-lookup"><span data-stu-id="096e5-310">The method takes an **Album** object.</span></span> <span data-ttu-id="096e5-311">ASP.NET MVC creará automáticamente el objeto de álbum desde la registrada &lt;formulario&gt; valores.</span><span class="sxs-lookup"><span data-stu-id="096e5-311">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="096e5-312">El método llevará a cabo estos pasos:</span><span class="sxs-lookup"><span data-stu-id="096e5-312">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="096e5-313">Si el modelo es válido:</span><span class="sxs-lookup"><span data-stu-id="096e5-313">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="096e5-314">Actualice la entrada de álbum en el contexto para marcarla como un objeto modificado.</span><span class="sxs-lookup"><span data-stu-id="096e5-314">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="096e5-315">Guarde los cambios y redirigir a la vista de índice.</span><span class="sxs-lookup"><span data-stu-id="096e5-315">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="096e5-316">Si el modelo no es válido, rellenará con ViewBag el **GenreId** y **ArtistId**, devolverá la vista con el objeto de álbum recibido para permitir al usuario realizar cualquier actualización necesaria.</span><span class="sxs-lookup"><span data-stu-id="096e5-316">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="096e5-317">Tarea 7: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="096e5-317">Task 7 - Running the Application</span></span>

<span data-ttu-id="096e5-318">En esta tarea, probará que la **StoreManager edición** página ver realmente guarda los datos actualizados del álbum en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="096e5-318">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="096e5-319">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="096e5-319">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="096e5-320">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="096e5-320">The project starts in the Home page.</span></span> <span data-ttu-id="096e5-321">Cambiar la dirección URL de **/StoreManager/Edit/1**.</span><span class="sxs-lookup"><span data-stu-id="096e5-321">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="096e5-322">Cambie el título de álbum a **carga** y haga clic en **guardar**.</span><span class="sxs-lookup"><span data-stu-id="096e5-322">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="096e5-323">Compruebe que el título del álbum cambiado realmente en la lista de álbumes.</span><span class="sxs-lookup"><span data-stu-id="096e5-323">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="096e5-324">![Actualizar un álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "actualizando un álbum")</span><span class="sxs-lookup"><span data-stu-id="096e5-324">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="096e5-325">*Actualización de un álbum*</span><span class="sxs-lookup"><span data-stu-id="096e5-325">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="096e5-326">Ejercicio 4: Agregar una vista de creación</span><span class="sxs-lookup"><span data-stu-id="096e5-326">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="096e5-327">Ahora que la **StoreManagerController** admite el **editar** capacidad, en este ejercicio obtendrá información sobre cómo agregar una plantilla Create View para que pueda almacenar los administradores agregarán álbumes nuevos a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="096e5-327">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="096e5-328">Igual que con la funcionalidad de edición, se implementará el escenario de creación mediante dos métodos independientes dentro de la **StoreManagerController** clase:</span><span class="sxs-lookup"><span data-stu-id="096e5-328">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="096e5-329">Un método de acción mostrará un formulario vacío cuando visite por primera vez los responsables de tienda el **/StoreManager/crear** dirección URL.</span><span class="sxs-lookup"><span data-stu-id="096e5-329">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="096e5-330">Un segundo método de acción controlará el escenario donde hace clic en el administrador del almacén la **guardar** situado dentro del formulario y envía los valores de nuevo a la **/StoreManager/crear** dirección URL como una solicitud HTTP-POST.</span><span class="sxs-lookup"><span data-stu-id="096e5-330">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="096e5-331">Tarea 1: implementar el método de acción Create HTTP-GET</span><span class="sxs-lookup"><span data-stu-id="096e5-331">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="096e5-332">En esta tarea, implementará la versión de HTTP-GET del método de acción Create para recuperar una lista de todos los géneros e intérpretes, empaquete estos datos en un **StoreManagerViewModel** objeto, que, a continuación, se pasará a una plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="096e5-332">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="096e5-333">Abra el **comenzar** solución ubicado en **origen/Ex4-AddingACreateView/inicio/** carpeta.</span><span class="sxs-lookup"><span data-stu-id="096e5-333">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="096e5-334">En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="096e5-334">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="096e5-335">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="096e5-335">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="096e5-336">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="096e5-336">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="096e5-337">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="096e5-337">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="096e5-338">Por último, compile la solución haciendo **compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="096e5-338">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="096e5-339">Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="096e5-339">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="096e5-340">Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="096e5-340">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="096e5-341">Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="096e5-341">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="096e5-342">Abra **StoreManagerController** clase.</span><span class="sxs-lookup"><span data-stu-id="096e5-342">Open **StoreManagerController** class.</span></span> <span data-ttu-id="096e5-343">Para ello, expanda el **controladores** carpeta y haga doble clic en **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="096e5-343">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="096e5-344">Reemplace el **crear** código del método de acción con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="096e5-344">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="096e5-345">(Código de fragmento de código - *las aplicaciones auxiliares de ASP.NET MVC 4 y formularios y validación: acción Ex4 StoreManagerController HTTP-GET crear*)</span><span class="sxs-lookup"><span data-stu-id="096e5-345">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="096e5-346">Tarea 2: adición de la vista de creación</span><span class="sxs-lookup"><span data-stu-id="096e5-346">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="096e5-347">En esta tarea, agregará la plantilla Create View que se mostrará un nuevo formulario de álbum (vacío).</span><span class="sxs-lookup"><span data-stu-id="096e5-347">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="096e5-348">Haga clic en el **crear** método de acción y seleccione **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="096e5-348">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="096e5-349">Se abrirá el cuadro de diálogo Agregar vista.</span><span class="sxs-lookup"><span data-stu-id="096e5-349">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="096e5-350">En el cuadro de diálogo Agregar vista, compruebe que el nombre de vista es **crear**.</span><span class="sxs-lookup"><span data-stu-id="096e5-350">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="096e5-351">Seleccione el **crear una vista fuertemente tipada** opción y seleccione **álbum (MvcMusicStore.Models)** desde el **clase modelo** desplegable y **crear** desde el **plantilla Scaffold** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="096e5-351">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="096e5-352">Deje los demás campos con sus valores predeterminados y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="096e5-352">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="096e5-353">![Agregar una vista create](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "agregar-a-create-view.png")</span><span class="sxs-lookup"><span data-stu-id="096e5-353">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="096e5-354">*Adición de la vista de creación*</span><span class="sxs-lookup"><span data-stu-id="096e5-354">*Adding the Create View*</span></span>
3. <span data-ttu-id="096e5-355">Actualización de la **GenreId** y **ArtistId** campos que se usarán una lista desplegable como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="096e5-355">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="096e5-356">Tarea 3: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="096e5-356">Task 3 - Running the Application</span></span>

<span data-ttu-id="096e5-357">En esta tarea, probará que la **StoreManager** **crear** ver página muestra un formulario vacío del álbum.</span><span class="sxs-lookup"><span data-stu-id="096e5-357">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="096e5-358">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="096e5-358">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="096e5-359">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="096e5-359">The project starts in the Home page.</span></span> <span data-ttu-id="096e5-360">Cambiar la dirección URL de **/StoreManager/crear**.</span><span class="sxs-lookup"><span data-stu-id="096e5-360">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="096e5-361">Compruebe que se muestra un formulario vacío para rellenar las nuevas propiedades del álbum.</span><span class="sxs-lookup"><span data-stu-id="096e5-361">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="096e5-362">![Crear vista con un formulario vacío](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View con un formulario vacío")</span><span class="sxs-lookup"><span data-stu-id="096e5-362">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="096e5-363">*Crear vista con un formulario vacío*</span><span class="sxs-lookup"><span data-stu-id="096e5-363">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="096e5-364">Tarea 4: implementar el HTTP-POST crear método de acción</span><span class="sxs-lookup"><span data-stu-id="096e5-364">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="096e5-365">En esta tarea, implementará la versión HTTP-POST del método de acción Create que se invocará cuando un usuario hace clic en el **guardar** botón.</span><span class="sxs-lookup"><span data-stu-id="096e5-365">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="096e5-366">El método debe guardar el nuevo álbum en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="096e5-366">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="096e5-367">Cierre el explorador si es necesario, para volver a la ventana de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="096e5-367">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="096e5-368">Abra **StoreManagerController** clase.</span><span class="sxs-lookup"><span data-stu-id="096e5-368">Open **StoreManagerController** class.</span></span> <span data-ttu-id="096e5-369">Para ello, expanda el **controladores** carpeta y haga doble clic en **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="096e5-369">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="096e5-370">Reemplace **crear HTTP-POST** código del método de acción con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="096e5-370">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="096e5-371">(Código de fragmento de código - *las aplicaciones auxiliares de ASP.NET MVC 4 y formularios y validación: acción Ex4 StoreManagerController HTTP - POST crear*)</span><span class="sxs-lookup"><span data-stu-id="096e5-371">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="096e5-372">La acción de creación es muy similar al método de acción de edición anterior, pero en lugar de establecer el objeto como modificada, se agrega al contexto.</span><span class="sxs-lookup"><span data-stu-id="096e5-372">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="096e5-373">Tarea 5: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="096e5-373">Task 5 - Running the Application</span></span>

<span data-ttu-id="096e5-374">En esta tarea, probará que la **StoreManager crear** ver página le permite crear un álbum nuevo y, a continuación, se redirige a la vista de índice StoreManager.</span><span class="sxs-lookup"><span data-stu-id="096e5-374">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="096e5-375">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="096e5-375">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="096e5-376">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="096e5-376">The project starts in the Home page.</span></span> <span data-ttu-id="096e5-377">Cambiar la dirección URL de **/StoreManager/crear**.</span><span class="sxs-lookup"><span data-stu-id="096e5-377">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="096e5-378">Rellenar todos los campos de formulario con datos de un álbum nuevo, como el de la siguiente ilustración:</span><span class="sxs-lookup"><span data-stu-id="096e5-378">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="096e5-379">![Crear un álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "creando un álbum")</span><span class="sxs-lookup"><span data-stu-id="096e5-379">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="096e5-380">*Crear un álbum*</span><span class="sxs-lookup"><span data-stu-id="096e5-380">*Creating an Album*</span></span>
3. <span data-ttu-id="096e5-381">Compruebe que se le redirigirá a la vista de índice StoreManager que incluye el nuevo álbum que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="096e5-381">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="096e5-382">![Nuevo álbum creado](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "nuevo álbum creado")</span><span class="sxs-lookup"><span data-stu-id="096e5-382">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="096e5-383">*Nuevo álbum creado*</span><span class="sxs-lookup"><span data-stu-id="096e5-383">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="096e5-384">Ejercicio 5: Eliminación de control</span><span class="sxs-lookup"><span data-stu-id="096e5-384">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="096e5-385">La capacidad de eliminar álbumes aún no está implementada.</span><span class="sxs-lookup"><span data-stu-id="096e5-385">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="096e5-386">Esto es lo que este ejercicio será aproximadamente.</span><span class="sxs-lookup"><span data-stu-id="096e5-386">This is what this exercise will be about.</span></span> <span data-ttu-id="096e5-387">Como antes, se implementará el escenario de eliminación utilizando dos métodos independientes dentro de la **StoreManagerController** clase:</span><span class="sxs-lookup"><span data-stu-id="096e5-387">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="096e5-388">Un método de acción mostrará un formulario de confirmación</span><span class="sxs-lookup"><span data-stu-id="096e5-388">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="096e5-389">Un segundo método de acción va a controlar el envío del formulario</span><span class="sxs-lookup"><span data-stu-id="096e5-389">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="096e5-390">Tarea 1: implementación del método de acción Delete de HTTP-GET</span><span class="sxs-lookup"><span data-stu-id="096e5-390">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="096e5-391">En esta tarea, implementará la versión de HTTP-GET del método de acción Delete para recuperar información del álbum.</span><span class="sxs-lookup"><span data-stu-id="096e5-391">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="096e5-392">Abra el **comenzar** solución ubicado en **origen/Ex5-HandlingDeletion/inicio/** carpeta.</span><span class="sxs-lookup"><span data-stu-id="096e5-392">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="096e5-393">En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="096e5-393">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="096e5-394">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="096e5-394">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="096e5-395">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="096e5-395">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="096e5-396">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="096e5-396">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="096e5-397">Por último, compile la solución haciendo **compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="096e5-397">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="096e5-398">Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="096e5-398">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="096e5-399">Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="096e5-399">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="096e5-400">Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="096e5-400">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="096e5-401">Abra **StoreManagerController** clase.</span><span class="sxs-lookup"><span data-stu-id="096e5-401">Open **StoreManagerController** class.</span></span> <span data-ttu-id="096e5-402">Para ello, expanda el **controladores** carpeta y haga doble clic en **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="096e5-402">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="096e5-403">La acción de controlador de eliminación es exactamente igual que la acción del controlador anterior Store detalles: consulta el **álbum** objeto desde la base de datos mediante el **id** proporcionado en la dirección URL y devuelve el adecuado **vista**.</span><span class="sxs-lookup"><span data-stu-id="096e5-403">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="096e5-404">Para ello, reemplace el HTTP-GET **eliminar** código del método de acción con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="096e5-404">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="096e5-405">(Código de fragmento de código - *las aplicaciones auxiliares de ASP.NET MVC 4 y formularios y validación - HTTP-GET eliminación de control de Ex5 Eliminar acción*)</span><span class="sxs-lookup"><span data-stu-id="096e5-405">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="096e5-406">Haga clic en el **eliminar** método de acción y seleccione **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="096e5-406">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="096e5-407">Se abrirá el cuadro de diálogo Agregar vista.</span><span class="sxs-lookup"><span data-stu-id="096e5-407">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="096e5-408">En el cuadro de diálogo Agregar vista, compruebe que el nombre de vista es **eliminar**.</span><span class="sxs-lookup"><span data-stu-id="096e5-408">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="096e5-409">Seleccione el **crear una vista fuertemente tipada** opción y seleccione **álbum (MvcMusicStore.Models)** desde el **clase modelo** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="096e5-409">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="096e5-410">Seleccione **eliminar** desde el **plantilla Scaffold** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="096e5-410">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="096e5-411">Deje los demás campos con sus valores predeterminados y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="096e5-411">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="096e5-412">![Agregar una vista Delete](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "agregar una vista Delete")</span><span class="sxs-lookup"><span data-stu-id="096e5-412">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="096e5-413">*Agregar una vista Delete*</span><span class="sxs-lookup"><span data-stu-id="096e5-413">*Adding a Delete View*</span></span>
6. <span data-ttu-id="096e5-414">La plantilla de eliminación muestra todos los campos del modelo.</span><span class="sxs-lookup"><span data-stu-id="096e5-414">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="096e5-415">Se mostrará solo del álbum.</span><span class="sxs-lookup"><span data-stu-id="096e5-415">You will show only the album's title.</span></span> <span data-ttu-id="096e5-416">Para ello, reemplace el contenido de la vista con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="096e5-416">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="096e5-417">Tarea 2: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="096e5-417">Task 2 - Running the Application</span></span>

<span data-ttu-id="096e5-418">En esta tarea, probará que la **StoreManager** **eliminar** ver página muestra un formulario de eliminación de confirmación.</span><span class="sxs-lookup"><span data-stu-id="096e5-418">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="096e5-419">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="096e5-419">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="096e5-420">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="096e5-420">The project starts in the Home page.</span></span> <span data-ttu-id="096e5-421">Cambiar la dirección URL de **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="096e5-421">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="096e5-422">Seleccione un álbum que desea eliminar haciendo clic en **eliminar** y compruebe que se cargue la nueva vista.</span><span class="sxs-lookup"><span data-stu-id="096e5-422">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="096e5-423">![Al eliminar un álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "al eliminar un álbum")</span><span class="sxs-lookup"><span data-stu-id="096e5-423">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="096e5-424">*Al eliminar un álbum*</span><span class="sxs-lookup"><span data-stu-id="096e5-424">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="096e5-425">Tarea 3: implementar el método de acción Delete de HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="096e5-425">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="096e5-426">En esta tarea, implementará la versión HTTP-POST del método de acción de eliminación que se invocará cuando un usuario hace clic en el **eliminar** botón.</span><span class="sxs-lookup"><span data-stu-id="096e5-426">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="096e5-427">El método debe eliminar el álbum en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="096e5-427">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="096e5-428">Cierre el explorador si es necesario, para volver a la ventana de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="096e5-428">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="096e5-429">Abra **StoreManagerController** clase.</span><span class="sxs-lookup"><span data-stu-id="096e5-429">Open **StoreManagerController** class.</span></span> <span data-ttu-id="096e5-430">Para ello, expanda el **controladores** carpeta y haga doble clic en **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="096e5-430">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="096e5-431">Reemplace **HTTP-POST Delete** código del método de acción con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="096e5-431">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="096e5-432">(Código de fragmento de código - *las aplicaciones auxiliares de ASP.NET MVC 4 y formularios y validación - HTTP-POST eliminación de control de Ex5 Eliminar acción*)</span><span class="sxs-lookup"><span data-stu-id="096e5-432">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="096e5-433">Tarea 4: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="096e5-433">Task 4 - Running the Application</span></span>

<span data-ttu-id="096e5-434">En esta tarea, probará que la **StoreManager Delete** ver página permite eliminar un álbum y, a continuación, se redirige a la vista de índice StoreManager.</span><span class="sxs-lookup"><span data-stu-id="096e5-434">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="096e5-435">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="096e5-435">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="096e5-436">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="096e5-436">The project starts in the Home page.</span></span> <span data-ttu-id="096e5-437">Cambiar la dirección URL de **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="096e5-437">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="096e5-438">Seleccione un álbum que desea eliminar haciendo clic en **eliminar.**</span><span class="sxs-lookup"><span data-stu-id="096e5-438">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="096e5-439">Haga clic en confirmar la eliminación **eliminar** botón:</span><span class="sxs-lookup"><span data-stu-id="096e5-439">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="096e5-440">![Al eliminar un álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "al eliminar un álbum")</span><span class="sxs-lookup"><span data-stu-id="096e5-440">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="096e5-441">*Al eliminar un álbum*</span><span class="sxs-lookup"><span data-stu-id="096e5-441">*Deleting an Album*</span></span>
3. <span data-ttu-id="096e5-442">Compruebe que se eliminó el álbum puesto que no aparece en el **índice** página.</span><span class="sxs-lookup"><span data-stu-id="096e5-442">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="096e5-443">Ejercicio 6: Agregar validación</span><span class="sxs-lookup"><span data-stu-id="096e5-443">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="096e5-444">Actualmente, los formularios de Create y Edit con que de contar no realizan ningún tipo de validación.</span><span class="sxs-lookup"><span data-stu-id="096e5-444">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="096e5-445">Si el usuario deja un campo obligatorio en blanco o escriba las letras en el campo de precio, será el primer error, obtendrá de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="096e5-445">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="096e5-446">Puede agregar validación a la aplicación mediante la adición de anotaciones de datos a la clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="096e5-446">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="096e5-447">Las anotaciones de datos permiten que describe las reglas que desea aplicadas a las propiedades del modelo y ASP.NET MVC se encargará de aplicar y mostrar el mensaje adecuado a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="096e5-447">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="096e5-448">Tarea 1: agregar anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="096e5-448">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="096e5-449">En esta tarea, agregará las anotaciones de datos para el modelo de álbum que hará que la página Create y Edit mostrar mensajes de validación cuando corresponda.</span><span class="sxs-lookup"><span data-stu-id="096e5-449">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="096e5-450">Para una clase de modelo simple, agregar una anotación de datos se controla simplemente agregando un **mediante** instrucción para **System.ComponentModel.DataAnnotation**, a continuación, colocar un **[obligatorio]** atributo en las propiedades adecuadas.</span><span class="sxs-lookup"><span data-stu-id="096e5-450">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="096e5-451">En el ejemplo siguiente se podría hacer que el **nombre** un campo obligatorio en la vista de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="096e5-451">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="096e5-452">Esto es un poco más complejo en casos como esta aplicación donde se genera el Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="096e5-452">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="096e5-453">Si agrega las anotaciones de datos directamente a las clases del modelo, se sobrescribirán si actualiza el modelo de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="096e5-453">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="096e5-454">En su lugar, puede hacer uso de clases parciales de los metadatos que existirá para almacenar las anotaciones y están asociadas con el modelo de clases con el **[MetadataType]** atributo.</span><span class="sxs-lookup"><span data-stu-id="096e5-454">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="096e5-455">Abra el **comenzar** solución ubicado en **origen/Ex6-AddingValidation/inicio/** carpeta.</span><span class="sxs-lookup"><span data-stu-id="096e5-455">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="096e5-456">En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="096e5-456">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="096e5-457">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="096e5-457">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="096e5-458">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="096e5-458">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="096e5-459">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="096e5-459">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="096e5-460">Por último, compile la solución haciendo **compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="096e5-460">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="096e5-461">Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="096e5-461">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="096e5-462">Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="096e5-462">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="096e5-463">Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="096e5-463">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="096e5-464">Abra el **Album.cs** desde el **modelos** carpeta.</span><span class="sxs-lookup"><span data-stu-id="096e5-464">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="096e5-465">Reemplace **Album.cs** de contenido con el código resaltado, por lo que se parezca a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="096e5-465">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="096e5-466">La línea **[DisplayFormat(ConvertEmptyStringToNull=false)]** indica que no se puede convertir las cadenas vacías del modelo en null cuando se actualiza el campo de datos en el origen de datos.</span><span class="sxs-lookup"><span data-stu-id="096e5-466">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="096e5-467">Esta configuración evitará una excepción cuando Entity Framework asigna valores null para el modelo antes de anotación de datos valida los campos.</span><span class="sxs-lookup"><span data-stu-id="096e5-467">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="096e5-468">(Código de fragmento de código - *las aplicaciones auxiliares de ASP.NET MVC 4 y formularios y validación - clase parcial de metadatos de álbum Ex6*)</span><span class="sxs-lookup"><span data-stu-id="096e5-468">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="096e5-469">Esto **álbum** clase parcial tiene un **MetadataType** atributo que apunta a la **AlbumMetaData** clase para las anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="096e5-469">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="096e5-470">Estos son algunos de los atributos de anotación de datos que se usa para anotar el modelo de álbum:</span><span class="sxs-lookup"><span data-stu-id="096e5-470">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="096e5-471">Requerido: indica que la propiedad es un campo obligatorio</span><span class="sxs-lookup"><span data-stu-id="096e5-471">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="096e5-472">DisplayName: define el texto que se usará en los campos de formulario y los mensajes de validación</span><span class="sxs-lookup"><span data-stu-id="096e5-472">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="096e5-473">DisplayFormat: especifica cómo se muestran y formato de los campos de datos.</span><span class="sxs-lookup"><span data-stu-id="096e5-473">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="096e5-474">StringLength - define una longitud máxima para un campo de cadena</span><span class="sxs-lookup"><span data-stu-id="096e5-474">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="096e5-475">Intervalo: da como resultado un valor máximo y mínimo de un campo numérico</span><span class="sxs-lookup"><span data-stu-id="096e5-475">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="096e5-476">ScaffoldColumn - permite la ocultación de campos de formularios de editor</span><span class="sxs-lookup"><span data-stu-id="096e5-476">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="096e5-477">Tarea 2: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="096e5-477">Task 2 - Running the Application</span></span>

<span data-ttu-id="096e5-478">En esta tarea, realizará las pruebas que las páginas Create y Edit validan campos, con los nombres para mostrar elegidos en la última tarea.</span><span class="sxs-lookup"><span data-stu-id="096e5-478">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="096e5-479">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="096e5-479">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="096e5-480">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="096e5-480">The project starts in the Home page.</span></span> <span data-ttu-id="096e5-481">Cambiar la dirección URL de **/StoreManager/crear**.</span><span class="sxs-lookup"><span data-stu-id="096e5-481">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="096e5-482">Compruebe que los nombres coinciden con los de la clase parcial (como **URL carátulas de álbum** en lugar de **AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="096e5-482">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="096e5-483">Haga clic en **crear**, sin rellenar el formulario.</span><span class="sxs-lookup"><span data-stu-id="096e5-483">Click **Create**, without filling the form.</span></span> <span data-ttu-id="096e5-484">Compruebe que se obtienen los mensajes de validación correspondiente.</span><span class="sxs-lookup"><span data-stu-id="096e5-484">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="096e5-485">![Valida los campos en la página crear](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "valida los campos en la página Crear")</span><span class="sxs-lookup"><span data-stu-id="096e5-485">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="096e5-486">*Campos validados en la página Crear*</span><span class="sxs-lookup"><span data-stu-id="096e5-486">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="096e5-487">Puede comprobar que lo mismo ocurre con el **editar** página.</span><span class="sxs-lookup"><span data-stu-id="096e5-487">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="096e5-488">Cambiar la dirección URL de **/StoreManager/Edit/1** y compruebe que los nombres coinciden con los de la clase parcial (como **URL carátulas de álbum** en lugar de **AlbumArtUrl**).</span><span class="sxs-lookup"><span data-stu-id="096e5-488">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="096e5-489">Vacía la **título** y **precio** campos y haga clic en **guardar**.</span><span class="sxs-lookup"><span data-stu-id="096e5-489">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="096e5-490">Compruebe que se obtienen los mensajes de validación correspondiente.</span><span class="sxs-lookup"><span data-stu-id="096e5-490">Verify that you get the corresponding validation messages.</span></span>

    ![Campos validados en la página de edición](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="096e5-492">*Campos validados en la página de edición*</span><span class="sxs-lookup"><span data-stu-id="096e5-492">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="096e5-493">Ejercicio 7: Uso de jQuery discreta en el lado cliente</span><span class="sxs-lookup"><span data-stu-id="096e5-493">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="096e5-494">En este ejercicio, obtendrá información sobre cómo habilitar la validación de jQuery discreta de MVC 4 en el lado del cliente.</span><span class="sxs-lookup"><span data-stu-id="096e5-494">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="096e5-495">JQuery discreta usa el prefijo de datos ajax JavaScript para invocar métodos de acción en el servidor en lugar de manera intrusiva emitir los scripts de cliente en línea.</span><span class="sxs-lookup"><span data-stu-id="096e5-495">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="096e5-496">Tarea 1: ejecutar la aplicación antes de habilitar discreta de jQuery</span><span class="sxs-lookup"><span data-stu-id="096e5-496">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="096e5-497">En esta tarea, ejecutará la aplicación antes de incluir jQuery con el fin de comparar ambos modelos de validación.</span><span class="sxs-lookup"><span data-stu-id="096e5-497">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="096e5-498">Abra el **comenzar** solución ubicado en **origen/Ex7-UnobtrusivejQueryValidation/inicio/** carpeta.</span><span class="sxs-lookup"><span data-stu-id="096e5-498">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="096e5-499">En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="096e5-499">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="096e5-500">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="096e5-500">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="096e5-501">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="096e5-501">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="096e5-502">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="096e5-502">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="096e5-503">Por último, compile la solución haciendo **compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="096e5-503">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="096e5-504">Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="096e5-504">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="096e5-505">Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="096e5-505">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="096e5-506">Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="096e5-506">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="096e5-507">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="096e5-507">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="096e5-508">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="096e5-508">The project starts in the Home page.</span></span> <span data-ttu-id="096e5-509">Examinar **/StoreManager/Create** y haga clic en **crear** sin rellenar el formulario para que compruebe que se obtienen mensajes de validación:</span><span class="sxs-lookup"><span data-stu-id="096e5-509">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="096e5-510">![Validación de cliente deshabilitado](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "validación de cliente deshabilitado")</span><span class="sxs-lookup"><span data-stu-id="096e5-510">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="096e5-511">*Validación de cliente deshabilitado*</span><span class="sxs-lookup"><span data-stu-id="096e5-511">*Client validation disabled*</span></span>
4. <span data-ttu-id="096e5-512">En el explorador, abra el código fuente HTML:</span><span class="sxs-lookup"><span data-stu-id="096e5-512">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="096e5-513">Tarea 2: Habilitar validación del cliente discreta</span><span class="sxs-lookup"><span data-stu-id="096e5-513">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="096e5-514">En esta tarea, permitirá jQuery **validación del cliente discreta** desde **Web.config** archivo, que se establece en false en todos los nuevos proyectos de ASP.NET MVC 4 de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="096e5-514">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="096e5-515">Además, agregará que las referencias para realizar el trabajo de validación de cliente discreta de jQuery de secuencias de comandos necesarios.</span><span class="sxs-lookup"><span data-stu-id="096e5-515">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="096e5-516">Abra **Web.Config** de archivos en la raíz del proyecto y asegúrese de que el **ClientValidationEnabled** y **UnobtrusiveJavaScriptEnabled** valores de las claves se establecen en **true**.</span><span class="sxs-lookup"><span data-stu-id="096e5-516">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="096e5-517">También puede habilitar la validación del cliente mediante código a Global.asax.cs para obtener los mismos resultados:</span><span class="sxs-lookup"><span data-stu-id="096e5-517">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="096e5-518">**HtmlHelper.ClientValidationEnabled = true;**</span><span class="sxs-lookup"><span data-stu-id="096e5-518">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="096e5-519">Además, puede asignar el atributo de ClientValidationEnabled en cualquier controlador de tener un comportamiento personalizado.</span><span class="sxs-lookup"><span data-stu-id="096e5-519">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="096e5-520">Abra **Create.cshtml** en **Views\StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="096e5-520">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="096e5-521">Asegúrese de que los siguientes archivos de script, **jquery.validate** y **jquery.validate.unobtrusive**, se hace referencia en el a través de la vista la &quot; **~/bundles/jqueryval** &quot; agrupación.</span><span class="sxs-lookup"><span data-stu-id="096e5-521">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view throught the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="096e5-522">Todas estas bibliotecas de jQuery se incluyen en los nuevos proyectos de MVC 4.</span><span class="sxs-lookup"><span data-stu-id="096e5-522">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="096e5-523">Puede encontrar más bibliotecas en el **/Scripts** carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="096e5-523">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="096e5-524">Para realizar esta validación bibliotecas funcionan, deberá agregar una referencia a la biblioteca del marco de jQuery.</span><span class="sxs-lookup"><span data-stu-id="096e5-524">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="096e5-525">Puesto que esta referencia se ha agregado en el  **\_Layout.cshtml** archivo, no es necesario agregarlo en esta vista.</span><span class="sxs-lookup"><span data-stu-id="096e5-525">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="096e5-526">Tarea 3: ejecutar la validación de jQuery discreta de uso de aplicación</span><span class="sxs-lookup"><span data-stu-id="096e5-526">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="096e5-527">En esta tarea, probará que la **StoreManager** Crear vista plantilla realiza la validación del lado cliente mediante bibliotecas de jQuery cuando el usuario crea un nuevo álbum.</span><span class="sxs-lookup"><span data-stu-id="096e5-527">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="096e5-528">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="096e5-528">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="096e5-529">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="096e5-529">The project starts in the Home page.</span></span> <span data-ttu-id="096e5-530">Examinar **/StoreManager/Create** y haga clic en **crear** sin rellenar el formulario para que compruebe que se obtienen mensajes de validación:</span><span class="sxs-lookup"><span data-stu-id="096e5-530">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="096e5-531">![Validación del cliente con jQuery habilitado](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "validación del cliente con jQuery habilitado")</span><span class="sxs-lookup"><span data-stu-id="096e5-531">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="096e5-532">*Validación del cliente con jQuery habilitado*</span><span class="sxs-lookup"><span data-stu-id="096e5-532">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="096e5-533">En el explorador, abra el código fuente de la vista de creación:</span><span class="sxs-lookup"><span data-stu-id="096e5-533">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > <span data-ttu-id="096e5-534">Para cada regla de validación del cliente, jQuery discreta agrega un atributo con datos-val -*rulename*=&quot;*mensaje*&quot;.</span><span class="sxs-lookup"><span data-stu-id="096e5-534">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="096e5-535">A continuación se presenta una lista de etiquetas que discreta jQuery se inserta en el campo de entrada html para realizar la validación de cliente:</span><span class="sxs-lookup"><span data-stu-id="096e5-535">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
   > 
   > - <span data-ttu-id="096e5-536">Datos val</span><span class="sxs-lookup"><span data-stu-id="096e5-536">Data-val</span></span>
   > - <span data-ttu-id="096e5-537">Número de datos val</span><span class="sxs-lookup"><span data-stu-id="096e5-537">Data-val-number</span></span>
   > - <span data-ttu-id="096e5-538">Rango de datos val</span><span class="sxs-lookup"><span data-stu-id="096e5-538">Data-val-range</span></span>
   > - <span data-ttu-id="096e5-539">Datos-val-range-min / datos-val-range-Max.</span><span class="sxs-lookup"><span data-stu-id="096e5-539">Data-val-range-min / Data-val-range-max</span></span>
   > - <span data-ttu-id="096e5-540">Datos necesarios para val</span><span class="sxs-lookup"><span data-stu-id="096e5-540">Data-val-required</span></span>
   > - <span data-ttu-id="096e5-541">Longitud de datos val</span><span class="sxs-lookup"><span data-stu-id="096e5-541">Data-val-length</span></span>
   > - <span data-ttu-id="096e5-542">Datos val longitud máxima / datos val longitud mínima</span><span class="sxs-lookup"><span data-stu-id="096e5-542">Data-val-length-max / Data-val-length-min</span></span>
   > 
   > <span data-ttu-id="096e5-543">Todos los valores de datos se rellenan con el modelo **anotación de datos**.</span><span class="sxs-lookup"><span data-stu-id="096e5-543">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="096e5-544">A continuación, se puede ejecutar toda la lógica que funciona en el lado del servidor en el lado del cliente.</span><span class="sxs-lookup"><span data-stu-id="096e5-544">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="096e5-545">Por ejemplo, el atributo precio tiene la siguiente anotación de datos en el modelo:</span><span class="sxs-lookup"><span data-stu-id="096e5-545">For example, Price attribute has the following data annotation in the model:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > <span data-ttu-id="096e5-546">Después de usar jQuery discreta, el código generado es:</span><span class="sxs-lookup"><span data-stu-id="096e5-546">After using Unobtrusive jQuery, the generated code is:</span></span>
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="096e5-547">Resumen</span><span class="sxs-lookup"><span data-stu-id="096e5-547">Summary</span></span>

<span data-ttu-id="096e5-548">Al completar este laboratorio práctico ha aprendido cómo habilitar usuarios cambiar los datos almacenados en la base de datos con el uso de las siguientes acciones:</span><span class="sxs-lookup"><span data-stu-id="096e5-548">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="096e5-549">Las acciones de controlador, como Index, Create, Edit, Delete</span><span class="sxs-lookup"><span data-stu-id="096e5-549">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="096e5-550">Característica de scaffolding de ASP.NET MVC para mostrar las propiedades en una tabla HTML</span><span class="sxs-lookup"><span data-stu-id="096e5-550">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="096e5-551">Experimentan de aplicaciones auxiliares HTML personalizadas para mejorar el usuario</span><span class="sxs-lookup"><span data-stu-id="096e5-551">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="096e5-552">Métodos de acción que reaccionan a HTTP-GET o las llamadas HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="096e5-552">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="096e5-553">Una plantilla de editor compartida para las plantillas de vista similares, como Create y Edit</span><span class="sxs-lookup"><span data-stu-id="096e5-553">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="096e5-554">Elementos de formulario como listas desplegables</span><span class="sxs-lookup"><span data-stu-id="096e5-554">Form elements like drop-downs</span></span>
- <span data-ttu-id="096e5-555">Anotaciones de datos para la validación del modelo</span><span class="sxs-lookup"><span data-stu-id="096e5-555">Data annotations for Model validation</span></span>
- <span data-ttu-id="096e5-556">Validación del lado cliente mediante la biblioteca de jQuery discreta</span><span class="sxs-lookup"><span data-stu-id="096e5-556">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="096e5-557">Apéndice A: instalación de Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="096e5-557">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="096e5-558">Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión utilizando el **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="096e5-558">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="096e5-559">Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* mediante *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="096e5-559">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="096e5-560">Vaya a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="096e5-560">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="096e5-561">Como alternativa, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con SDK de Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="096e5-561">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="096e5-562">Haga clic en **instalar ahora**.</span><span class="sxs-lookup"><span data-stu-id="096e5-562">Click on **Install Now**.</span></span> <span data-ttu-id="096e5-563">Si no tienes **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="096e5-563">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="096e5-564">Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.</span><span class="sxs-lookup"><span data-stu-id="096e5-564">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="096e5-565">![Instale Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "instale Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="096e5-565">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="096e5-566">*Instale Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="096e5-566">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="096e5-567">Leer todos los productos las licencias y los términos y haga clic en **acepto** para continuar.</span><span class="sxs-lookup"><span data-stu-id="096e5-567">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Acepte los términos de licencia](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="096e5-569">*Acepte los términos de licencia*</span><span class="sxs-lookup"><span data-stu-id="096e5-569">*Accepting the license terms*</span></span>
5. <span data-ttu-id="096e5-570">Espere hasta que finalice el proceso de descarga e instalación.</span><span class="sxs-lookup"><span data-stu-id="096e5-570">Wait until the downloading and installation process completes.</span></span>

    ![Progreso de la instalación](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="096e5-572">*Progreso de la instalación*</span><span class="sxs-lookup"><span data-stu-id="096e5-572">*Installation progress*</span></span>
6. <span data-ttu-id="096e5-573">Cuando se complete la instalación, haga clic en **finalizar**.</span><span class="sxs-lookup"><span data-stu-id="096e5-573">When the installation completes, click **Finish**.</span></span>

    ![Instalación completada](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="096e5-575">*Instalación completada*</span><span class="sxs-lookup"><span data-stu-id="096e5-575">*Installation completed*</span></span>
7. <span data-ttu-id="096e5-576">Haga clic en **Exit** para cerrar el instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="096e5-576">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="096e5-577">Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **VS Express**&quot;, a continuación, haga clic en el **VS Express para Web** icono.</span><span class="sxs-lookup"><span data-stu-id="096e5-577">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para el icono de Web](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="096e5-579">*VS Express para el icono de Web*</span><span class="sxs-lookup"><span data-stu-id="096e5-579">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="096e5-580">Apéndice B: uso de fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="096e5-580">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="096e5-581">Con fragmentos de código, tiene todo el código que necesita a su alcance.</span><span class="sxs-lookup"><span data-stu-id="096e5-581">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="096e5-582">El documento de laboratorio le dirá exactamente cuándo se pueden utilizar, como se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="096e5-582">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="096e5-583">![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "fragmentos de código en Visual Studio para insertar código en el proyecto")</span><span class="sxs-lookup"><span data-stu-id="096e5-583">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="096e5-584">*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*</span><span class="sxs-lookup"><span data-stu-id="096e5-584">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="096e5-585">***Para agregar un fragmento de código mediante el teclado (solo C#)***</span><span class="sxs-lookup"><span data-stu-id="096e5-585">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="096e5-586">Coloque el cursor donde desea insertar el código.</span><span class="sxs-lookup"><span data-stu-id="096e5-586">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="096e5-587">Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).</span><span class="sxs-lookup"><span data-stu-id="096e5-587">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="096e5-588">Observe cómo IntelliSense muestra los nombres de fragmentos de código coincidentes.</span><span class="sxs-lookup"><span data-stu-id="096e5-588">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="096e5-589">Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).</span><span class="sxs-lookup"><span data-stu-id="096e5-589">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="096e5-590">Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.</span><span class="sxs-lookup"><span data-stu-id="096e5-590">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="096e5-591">![Comience a escribir el nombre del fragmento](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "comience a escribir el nombre del fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="096e5-591">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="096e5-592">*Comience a escribir el nombre del fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="096e5-592">*Start typing the snippet name*</span></span>

<span data-ttu-id="096e5-593">![Presione la tecla Tab para seleccionar el fragmento de código resaltada](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "presione Tab para seleccionar el fragmento de código resaltada")</span><span class="sxs-lookup"><span data-stu-id="096e5-593">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="096e5-594">*Presione la tecla Tab para seleccionar el fragmento de código resaltada*</span><span class="sxs-lookup"><span data-stu-id="096e5-594">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="096e5-595">![Vuelva a presionar Tab y el fragmento de código se expandirán](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "vuelva a presionar Tab y el fragmento de código se expandirán")</span><span class="sxs-lookup"><span data-stu-id="096e5-595">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="096e5-596">*Vuelva a presionar Tab y el fragmento de código se expandirán*</span><span class="sxs-lookup"><span data-stu-id="096e5-596">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="096e5-597">***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="096e5-597">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="096e5-598">Haga doble clic en el desea insertar el fragmento de código.</span><span class="sxs-lookup"><span data-stu-id="096e5-598">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="096e5-599">Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.</span><span class="sxs-lookup"><span data-stu-id="096e5-599">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="096e5-600">Seleccione el fragmento de código relevante en la lista, haciendo clic en él.</span><span class="sxs-lookup"><span data-stu-id="096e5-600">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="096e5-601">![Con el botón secundario donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="096e5-601">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="096e5-602">*Haga doble clic donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="096e5-602">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="096e5-603">![Elegir el fragmento de código relevante en la lista, hacer clic en ella](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "elegir el fragmento de código relevante en la lista, hacer clic en ella")</span><span class="sxs-lookup"><span data-stu-id="096e5-603">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="096e5-604">*Elegir el fragmento de código relevante en la lista, hacer clic en ella*</span><span class="sxs-lookup"><span data-stu-id="096e5-604">*Pick the relevant snippet from the list, by clicking on it*</span></span>
