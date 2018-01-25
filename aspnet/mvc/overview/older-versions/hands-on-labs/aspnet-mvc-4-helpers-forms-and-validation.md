---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: "Aplicaciones auxiliares de MVC de ASP.NET 4, formularios y la validación | Documentos de Microsoft"
author: rick-anderson
description: "En ASP.NET MVC 4 modelos y los laboratorios de prácticas de acceso de datos, se han cargar y mostrar los datos de la base de datos. En este laboratorio práctico, va a agregar a la..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 925d659f42496045089ba056e194ac977c37a8de
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="666ed-104">Validación, formularios y aplicaciones auxiliares de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="666ed-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>
====================
<span data-ttu-id="666ed-105">por [Web colonias equipo](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="666ed-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="666ed-106">En **acceso a datos y modelos de ASP.NET MVC 4** los laboratorios de prácticas, han sido cargar y mostrar los datos de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="666ed-106">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="666ed-107">En este laboratorio práctico, va a agregar a la **tienda de música** aplicación la capacidad de editar esos datos.</span><span class="sxs-lookup"><span data-stu-id="666ed-107">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>
> 
> <span data-ttu-id="666ed-108">Con ese objetivo en mente, primero creará el controlador que admitirá las acciones de creación, lectura, actualización y eliminación (CRUD) de álbumes.</span><span class="sxs-lookup"><span data-stu-id="666ed-108">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="666ed-109">Se generará una plantilla de vista de índice sacar partido de las características de scaffolding de ASP.NET MVC para mostrar las propiedades de los álbumes en una tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="666ed-109">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="666ed-110">Para mejorar esa vista, deberá agregar una aplicación auxiliar HTML personalizada que se truncará descripciones largas.</span><span class="sxs-lookup"><span data-stu-id="666ed-110">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>
> 
> <span data-ttu-id="666ed-111">Después, agregará la edición y crear vistas que se pueden modificar los álbumes en la base de datos, con la Ayuda de los elementos de formulario como listas desplegables.</span><span class="sxs-lookup"><span data-stu-id="666ed-111">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>
> 
> <span data-ttu-id="666ed-112">Por último, se permitirán a los usuarios eliminar un álbum y también se les impedirá introducir datos erróneos al validar su entrada.</span><span class="sxs-lookup"><span data-stu-id="666ed-112">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="666ed-113">Este laboratorio práctico se da por supuesto que tiene conocimientos básicos de **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="666ed-113">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="666ed-114">Si no ha usado **ASP.NET MVC** antes, le recomendamos que repase **Fundamentos de MVC de ASP.NET** laboratorio práctico.</span><span class="sxs-lookup"><span data-stu-id="666ed-114">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>
> 
> 
> <span data-ttu-id="666ed-115">Esta práctica le guiará a través de las mejoras y nuevas características descritos anteriormente aplicando cambios menores a una aplicación Web de ejemplo proporcionada en la carpeta de origen.</span><span class="sxs-lookup"><span data-stu-id="666ed-115">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="666ed-116">Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web colonias, disponible en [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span><span class="sxs-lookup"><span data-stu-id="666ed-116">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="666ed-117">Objetivos</span><span class="sxs-lookup"><span data-stu-id="666ed-117">Objectives</span></span>

<span data-ttu-id="666ed-118">En este laboratorio práctico, aprenderá cómo:</span><span class="sxs-lookup"><span data-stu-id="666ed-118">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="666ed-119">Crear un controlador para que admita operaciones CRUD</span><span class="sxs-lookup"><span data-stu-id="666ed-119">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="666ed-120">Generar una vista de índice para mostrar las propiedades de entidad en una tabla HTML</span><span class="sxs-lookup"><span data-stu-id="666ed-120">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="666ed-121">Agregar una aplicación auxiliar HTML personalizada</span><span class="sxs-lookup"><span data-stu-id="666ed-121">Add a custom HTML helper</span></span>
- <span data-ttu-id="666ed-122">Crear y personalizar una vista de edición</span><span class="sxs-lookup"><span data-stu-id="666ed-122">Create and customize an Edit View</span></span>
- <span data-ttu-id="666ed-123">Diferenciar entre los métodos de acción que reaccionar frente a HTTP-GET o llamadas HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="666ed-123">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="666ed-124">Agregar y personalizar una instrucción Create View</span><span class="sxs-lookup"><span data-stu-id="666ed-124">Add and customize a Create View</span></span>
- <span data-ttu-id="666ed-125">Identificador de la eliminación de una entidad</span><span class="sxs-lookup"><span data-stu-id="666ed-125">Handle the deletion of an entity</span></span>
- <span data-ttu-id="666ed-126">Validar entrada de usuario</span><span class="sxs-lookup"><span data-stu-id="666ed-126">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="666ed-127">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="666ed-127">Prerequisites</span></span>

<span data-ttu-id="666ed-128">Debe tener los elementos siguientes para completar esta práctica:</span><span class="sxs-lookup"><span data-stu-id="666ed-128">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="666ed-129">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (leer [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).</span><span class="sxs-lookup"><span data-stu-id="666ed-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="666ed-130">Programa de instalación</span><span class="sxs-lookup"><span data-stu-id="666ed-130">Setup</span></span>

<span data-ttu-id="666ed-131">**Instalación de fragmentos de código**</span><span class="sxs-lookup"><span data-stu-id="666ed-131">**Installing Code Snippets**</span></span>

<span data-ttu-id="666ed-132">Para mayor comodidad, gran parte del código que se va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="666ed-132">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="666ed-133">Para instalar los fragmentos de código que se ejecute **.\Source\Setup\CodeSnippets.vsi** archivo.</span><span class="sxs-lookup"><span data-stu-id="666ed-133">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="666ed-134">Si no está familiarizado con los fragmentos de código de Visual Studio y desea obtener información sobre cómo utilizarlas, puede consultar el apéndice de este documento &quot; [Apéndice B: Using Code Snippets](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="666ed-134">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="666ed-135">Ejercicios</span><span class="sxs-lookup"><span data-stu-id="666ed-135">Exercises</span></span>

<span data-ttu-id="666ed-136">Este laboratorio práctico se componen de los ejercicios siguientes:</span><span class="sxs-lookup"><span data-stu-id="666ed-136">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="666ed-137">Crear el controlador de administrador del almacén y su vista de índice</span><span class="sxs-lookup"><span data-stu-id="666ed-137">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="666ed-138">Agregar una aplicación auxiliar de HTML</span><span class="sxs-lookup"><span data-stu-id="666ed-138">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="666ed-139">Crear la vista de edición</span><span class="sxs-lookup"><span data-stu-id="666ed-139">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="666ed-140">Agregar una vista de creación</span><span class="sxs-lookup"><span data-stu-id="666ed-140">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="666ed-141">Control de eliminación</span><span class="sxs-lookup"><span data-stu-id="666ed-141">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="666ed-142">Agregar una validación</span><span class="sxs-lookup"><span data-stu-id="666ed-142">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="666ed-143">Uso de jQuery discreta en el cliente</span><span class="sxs-lookup"><span data-stu-id="666ed-143">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="666ed-144">Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debería obtener después de completar los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="666ed-144">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="666ed-145">Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="666ed-145">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="666ed-146">Tiempo estimado para completar esta práctica: **60 minutos**</span><span class="sxs-lookup"><span data-stu-id="666ed-146">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="666ed-147">Ejercicio 1: Crear el controlador de administrador del almacén y su vista de índice</span><span class="sxs-lookup"><span data-stu-id="666ed-147">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="666ed-148">En este ejercicio, aprenderá a crear un nuevo controlador para admitir operaciones CRUD, personalizar su método de acción de índice para devolver una lista de álbumes de la base de datos y por último, generar una plantilla de vista de índice al aprovechar la posibilidad de scaffolding de ASP.NET MVC característica para mostrar las propiedades de los álbumes en una tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="666ed-148">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="666ed-149">Tarea 1: crear el StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="666ed-149">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="666ed-150">En esta tarea, creará un nuevo controlador denominado **StoreManagerController** para admitir las operaciones CRUD.</span><span class="sxs-lookup"><span data-stu-id="666ed-150">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="666ed-151">Abra la **comenzar** solución ubicado en **origen/Ex1-CreatingTheStoreManagerController/Begin/** carpeta.</span><span class="sxs-lookup"><span data-stu-id="666ed-151">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

    1. <span data-ttu-id="666ed-152">Deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="666ed-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="666ed-153">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="666ed-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="666ed-154">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="666ed-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="666ed-155">Por último, compile la solución haciendo clic en **generar** | **generar solución**.</span><span class="sxs-lookup"><span data-stu-id="666ed-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="666ed-156">Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="666ed-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="666ed-157">Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="666ed-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="666ed-158">Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="666ed-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="666ed-159">Agrega un nuevo controlador.</span><span class="sxs-lookup"><span data-stu-id="666ed-159">Add a new controller.</span></span> <span data-ttu-id="666ed-160">Para ello, haga clic en el **controladores** carpeta en el Explorador de soluciones, seleccione **agregar** y, a continuación, el **controlador** comando.</span><span class="sxs-lookup"><span data-stu-id="666ed-160">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="666ed-161">Cambiar el **controlador** **nombre** a **StoreManagerController** y asegúrese de que la opción **controlador de MVC con acciones de lectura/escritura vacías**está seleccionada.</span><span class="sxs-lookup"><span data-stu-id="666ed-161">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="666ed-162">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="666ed-162">Click **Add**.</span></span>

    <span data-ttu-id="666ed-163">![Cuadro de diálogo Agregar controlador](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "cuadro de diálogo Agregar controlador")</span><span class="sxs-lookup"><span data-stu-id="666ed-163">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="666ed-164">*Agregar cuadro de diálogo de controlador*</span><span class="sxs-lookup"><span data-stu-id="666ed-164">*Add Controller Dialog*</span></span>

    <span data-ttu-id="666ed-165">Se genera una nueva clase de controlador.</span><span class="sxs-lookup"><span data-stu-id="666ed-165">A new Controller class is generated.</span></span> <span data-ttu-id="666ed-166">Puesto que indicado para agregar acciones de lectura/escritura, métodos de código auxiliar para los usuarios, acciones CRUD comunes se crean con comentarios TODO que se rellena, solicita si se deben para incluir la lógica específica de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="666ed-166">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="666ed-167">Tarea 2: personalizar el índice StoreManager</span><span class="sxs-lookup"><span data-stu-id="666ed-167">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="666ed-168">En esta tarea, va a personalizar el método de acción de índice StoreManager para devolver una vista con la lista de álbumes de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="666ed-168">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="666ed-169">En la clase StoreManagerController, agregue las siguientes *con* directivas.</span><span class="sxs-lookup"><span data-stu-id="666ed-169">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="666ed-170">(Código de fragmento de código: *aplicaciones auxiliares de ASP.NET MVC 4 y los formularios y validación - Ex1 mediante MvcMusicStore*)</span><span class="sxs-lookup"><span data-stu-id="666ed-170">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="666ed-171">Agregar un campo a la **StoreManagerController** que hospede una instancia de **MusicStoreEntities.**</span><span class="sxs-lookup"><span data-stu-id="666ed-171">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="666ed-172">(Código de fragmento de código: *aplicaciones auxiliares de MVC de ASP.NET 4 y los formularios y validación - Ex1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="666ed-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="666ed-173">Implementar la acción del índice de StoreManagerController para devolver una vista con la lista de álbumes.</span><span class="sxs-lookup"><span data-stu-id="666ed-173">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="666ed-174">La lógica de acción de controlador será muy similar a la acción del índice del StoreController escrito anteriormente.</span><span class="sxs-lookup"><span data-stu-id="666ed-174">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="666ed-175">Usar LINQ para recuperar todos los álbumes, incluida la información de género e intérprete para su presentación.</span><span class="sxs-lookup"><span data-stu-id="666ed-175">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="666ed-176">(Código de fragmento de código: *aplicaciones auxiliares de MVC de ASP.NET 4 y los formularios y validación - Ex1 StoreManagerController índice*)</span><span class="sxs-lookup"><span data-stu-id="666ed-176">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="666ed-177">Tarea 3: crear la vista de índice</span><span class="sxs-lookup"><span data-stu-id="666ed-177">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="666ed-178">En esta tarea, va a crear la plantilla de vista de índice para mostrar la lista de álbumes devuelto por la **StoreManager** controlador.</span><span class="sxs-lookup"><span data-stu-id="666ed-178">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="666ed-179">Antes de crear la nueva plantilla de vista, debe compilar el proyecto para que la **Agregar cuadro de diálogo de vista** conoce la **álbum** clase se debe utilizar.</span><span class="sxs-lookup"><span data-stu-id="666ed-179">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="666ed-180">Seleccione **compilar | Generar MvcMusicStore** para compilar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="666ed-180">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="666ed-181">Pulse el botón derecho dentro de la **índice** método de acción y seleccione **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="666ed-181">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="666ed-182">Esto le llevará a la **agregar vista** cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="666ed-182">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="666ed-183">![Agregar vista](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "agregar vista")</span><span class="sxs-lookup"><span data-stu-id="666ed-183">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="666ed-184">*Agregar una vista desde dentro del método de índice*</span><span class="sxs-lookup"><span data-stu-id="666ed-184">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="666ed-185">En el cuadro de diálogo Agregar vista, compruebe que el nombre de vista es **índice**.</span><span class="sxs-lookup"><span data-stu-id="666ed-185">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="666ed-186">Seleccione el **crear una vista fuertemente tipada** opción y seleccione **álbum (MvcMusicStore.Models)** desde el **clase modelo** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="666ed-186">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="666ed-187">Seleccione **lista** desde el **plantilla Scaffold** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="666ed-187">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="666ed-188">Deje el **motor de vista** a **Razor** y los demás campos con su valor predeterminado de valor y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="666ed-188">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="666ed-189">![Agregar una vista de índice](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "agregar una vista de índice")</span><span class="sxs-lookup"><span data-stu-id="666ed-189">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="666ed-190">*Agregar una vista de índice*</span><span class="sxs-lookup"><span data-stu-id="666ed-190">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="666ed-191">Tarea 4: personalizar el scaffolding de la vista de índice</span><span class="sxs-lookup"><span data-stu-id="666ed-191">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="666ed-192">En esta tarea, se ajustará la plantilla de la vista simple creada con la característica de scaffolding de ASP.NET MVC para mostrar los campos que desee.</span><span class="sxs-lookup"><span data-stu-id="666ed-192">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="666ed-193">El **scaffolding** compatibilidad de ASP.NET MVC genera una plantilla de vista simple que enumera todos los campos en el modelo de álbum.</span><span class="sxs-lookup"><span data-stu-id="666ed-193">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="666ed-194">**Scaffolding** proporciona una forma rápida de empezar a trabajar en una vista fuertemente tipada: en lugar de tener que escribir manualmente la plantilla de vista, scaffolding rápidamente genera una plantilla predeterminada y, a continuación, puede modificar el código generado.</span><span class="sxs-lookup"><span data-stu-id="666ed-194">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>


1. <span data-ttu-id="666ed-195">Revise el código creado.</span><span class="sxs-lookup"><span data-stu-id="666ed-195">Review the code created.</span></span> <span data-ttu-id="666ed-196">La lista de campos generada va a formar parte de los siguientes valores de tabla HTML que **Scaffolding** usa para mostrar datos tabulares.</span><span class="sxs-lookup"><span data-stu-id="666ed-196">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="666ed-197">Reemplace el  **&lt;tabla&gt;**  código con el código siguiente para mostrar solamente la **género**, **intérprete**, **título del álbum**, y **precio** campos.</span><span class="sxs-lookup"><span data-stu-id="666ed-197">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="666ed-198">Esto elimina la **AlbumId** y **dirección URL de carátulas de álbum** columnas.</span><span class="sxs-lookup"><span data-stu-id="666ed-198">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="666ed-199">Además, cambia las columnas GenreId y ArtistId para mostrar sus propiedades de clase vinculados de **Artist.Name** y **Genre.Name**y quita la **detalles** vínculo.</span><span class="sxs-lookup"><span data-stu-id="666ed-199">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="666ed-200">Cambie las siguientes descripciones.</span><span class="sxs-lookup"><span data-stu-id="666ed-200">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="666ed-201">Tarea 5: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="666ed-201">Task 5 - Running the Application</span></span>

<span data-ttu-id="666ed-202">En esta tarea, realizará las pruebas que el **StoreManager** **índice** plantilla de vista muestra una lista de álbumes de acuerdo con el diseño de los pasos anteriores.</span><span class="sxs-lookup"><span data-stu-id="666ed-202">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="666ed-203">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="666ed-203">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="666ed-204">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="666ed-204">The project starts in the Home page.</span></span> <span data-ttu-id="666ed-205">Cambiar la dirección URL de **/StoreManager** para comprobar que se muestra una lista de álbumes, que muestra sus **título**, **intérprete** y **género**.</span><span class="sxs-lookup"><span data-stu-id="666ed-205">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="666ed-206">![Examinar la lista de los álbumes](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "examinar la lista de los álbumes")</span><span class="sxs-lookup"><span data-stu-id="666ed-206">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="666ed-207">*Examinar la lista de los álbumes*</span><span class="sxs-lookup"><span data-stu-id="666ed-207">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="666ed-208">Ejercicio 2: Agregar una aplicación auxiliar de HTML</span><span class="sxs-lookup"><span data-stu-id="666ed-208">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="666ed-209">La página de índice StoreManager tiene un problema potencial: propiedades de título y el nombre del intérprete tanto pueden ser lo suficientemente largo para deshacerse del formato de tabla.</span><span class="sxs-lookup"><span data-stu-id="666ed-209">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="666ed-210">En este ejercicio obtendrá información sobre cómo agregar una aplicación auxiliar HTML personalizada para truncar ese texto.</span><span class="sxs-lookup"><span data-stu-id="666ed-210">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="666ed-211">En la siguiente ilustración, puede ver cómo se modifica el formato debido a la longitud del texto cuando se utiliza un tamaño pequeño del explorador.</span><span class="sxs-lookup"><span data-stu-id="666ed-211">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="666ed-212">![Examinar la lista de los álbumes con texto truncado no](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "no examinar la lista de los álbumes con texto truncado")</span><span class="sxs-lookup"><span data-stu-id="666ed-212">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="666ed-213">*Examinar la lista de los álbumes con texto truncado no*</span><span class="sxs-lookup"><span data-stu-id="666ed-213">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="666ed-214">Tarea 1: Extender la aplicación auxiliar HTML</span><span class="sxs-lookup"><span data-stu-id="666ed-214">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="666ed-215">En esta tarea, agregará un nuevo método **Truncate** a la **HTML** objeto expuesto en las vistas de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="666ed-215">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="666ed-216">Para ello, implementará un **método de extensión** a la integrada **System.Web.Mvc.HtmlHelper** clase proporcionada por ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="666ed-216">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="666ed-217">Para obtener más información sobre **métodos de extensión**, visite este artículo de msdn.</span><span class="sxs-lookup"><span data-stu-id="666ed-217">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="666ed-218">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="666ed-218">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>


1. <span data-ttu-id="666ed-219">Abra la **comenzar** solución ubicado en **origen/Ex2-AddingAnHTMLHelper/Begin/** carpeta.</span><span class="sxs-lookup"><span data-stu-id="666ed-219">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="666ed-220">En caso contrario, puede seguir usando el **final** solución obtenido siguiendo el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="666ed-220">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="666ed-221">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="666ed-221">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="666ed-222">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="666ed-222">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="666ed-223">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="666ed-223">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="666ed-224">Por último, compile la solución haciendo clic en **generar** | **generar solución**.</span><span class="sxs-lookup"><span data-stu-id="666ed-224">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="666ed-225">Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="666ed-225">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="666ed-226">Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="666ed-226">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="666ed-227">Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="666ed-227">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="666ed-228">Abra la vista de índice del StoreManager.</span><span class="sxs-lookup"><span data-stu-id="666ed-228">Open StoreManager's Index View.</span></span> <span data-ttu-id="666ed-229">Para ello, en el Explorador de soluciones, expanda la **vistas** carpeta, la **StoreManager** y abra el **Index.cshtml** archivo.</span><span class="sxs-lookup"><span data-stu-id="666ed-229">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="666ed-230">Agregue el código siguiente la  **@model**  directiva para definir la **Truncate** método auxiliar.</span><span class="sxs-lookup"><span data-stu-id="666ed-230">Add the following code below the **@model** directive to define the **Truncate** helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="666ed-231">Tarea 2: truncar texto en la página</span><span class="sxs-lookup"><span data-stu-id="666ed-231">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="666ed-232">En esta tarea, va a usar el **Truncate** método truncar el texto en la plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="666ed-232">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="666ed-233">Abra la vista de índice del StoreManager.</span><span class="sxs-lookup"><span data-stu-id="666ed-233">Open StoreManager's Index View.</span></span> <span data-ttu-id="666ed-234">Para ello, en el Explorador de soluciones, expanda la **vistas** carpeta, la **StoreManager** y abra el **Index.cshtml** archivo.</span><span class="sxs-lookup"><span data-stu-id="666ed-234">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="666ed-235">Reemplace las líneas que muestran la **el nombre del intérprete** y del álbum **título**.</span><span class="sxs-lookup"><span data-stu-id="666ed-235">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="666ed-236">Para ello, reemplace las líneas siguientes.</span><span class="sxs-lookup"><span data-stu-id="666ed-236">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="666ed-237">Tarea 3: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="666ed-237">Task 3 - Running the Application</span></span>

<span data-ttu-id="666ed-238">En esta tarea, realizará las pruebas que el **StoreManager** **índice** plantilla vista trunca el título y el nombre del intérprete del álbum.</span><span class="sxs-lookup"><span data-stu-id="666ed-238">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="666ed-239">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="666ed-239">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="666ed-240">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="666ed-240">The project starts in the Home page.</span></span> <span data-ttu-id="666ed-241">Cambiar la dirección URL de **/StoreManager** para comprobar esa larga textos en el **título** y **intérprete** columna se ha truncado.</span><span class="sxs-lookup"><span data-stu-id="666ed-241">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="666ed-242">![Trunca los nombres de títulos e intérpretes](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "trunca los nombres de títulos e intérpretes")</span><span class="sxs-lookup"><span data-stu-id="666ed-242">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="666ed-243">*Nombres de intérpretes y títulos truncados*</span><span class="sxs-lookup"><span data-stu-id="666ed-243">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="666ed-244">Ejercicio 3: Crear la vista de edición</span><span class="sxs-lookup"><span data-stu-id="666ed-244">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="666ed-245">En este ejercicio, obtendrá información sobre cómo crear un formulario para permitir que los jefes de almacén editar un álbum.</span><span class="sxs-lookup"><span data-stu-id="666ed-245">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="666ed-246">Examinará el **/StoreManager/Edit/id** dirección URL (**Id. de** que el identificador único del álbum que desea editar), lo que hace que una llamada HTTP-GET en el servidor.</span><span class="sxs-lookup"><span data-stu-id="666ed-246">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="666ed-247">El método de acción de controlador Edit van a recuperar el álbum adecuado de la base de datos, cree un **StoreManagerViewModel** objeto va a encapsular (junto con una lista de intérpretes, géneros) y, a continuación, pasar a una plantilla de vista para presentar la página HTML al usuario.</span><span class="sxs-lookup"><span data-stu-id="666ed-247">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="666ed-248">Esta página contendrá una  **&lt;formulario&gt;**  elemento con cuadros de texto y listas desplegables para editar las propiedades de álbum.</span><span class="sxs-lookup"><span data-stu-id="666ed-248">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="666ed-249">Una vez que el usuario actualiza los valores de formulario de álbum y hace clic en el **guardar** botón, los cambios se envían a través de devolución de llamada una solicitud HTTP POST a **/StoreManager/Edit/id**. Aunque la dirección URL sigue siendo el mismo que en la última llamada, ASP.NET MVC que identifica este tiempo es una solicitud HTTP-POST y, por tanto, se ejecuta un método de acción de edición diferente (uno decorada con **[HttpPost]**).</span><span class="sxs-lookup"><span data-stu-id="666ed-249">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="666ed-250">Tarea 1: implementar el método de acción de edición de HTTP-GET</span><span class="sxs-lookup"><span data-stu-id="666ed-250">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="666ed-251">En esta tarea, implementará la versión de HTTP-GET del método de acción de edición para recuperar el álbum adecuado de la base de datos, así como una lista de todos los géneros e intérpretes.</span><span class="sxs-lookup"><span data-stu-id="666ed-251">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="666ed-252">Empaqueta estos datos en el **StoreManagerViewModel** definido en el último paso, que, a continuación, se pasa a una plantilla de vista para representar la respuesta con el objeto.</span><span class="sxs-lookup"><span data-stu-id="666ed-252">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="666ed-253">Abra la **comenzar** solución ubicado en **origen/Ex3-CreatingTheEditView/Begin/** carpeta.</span><span class="sxs-lookup"><span data-stu-id="666ed-253">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="666ed-254">En caso contrario, puede seguir usando el **final** solución obtenido siguiendo el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="666ed-254">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="666ed-255">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="666ed-255">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="666ed-256">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="666ed-256">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="666ed-257">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="666ed-257">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="666ed-258">Por último, compile la solución haciendo clic en **generar** | **generar solución**.</span><span class="sxs-lookup"><span data-stu-id="666ed-258">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="666ed-259">Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="666ed-259">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="666ed-260">Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="666ed-260">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="666ed-261">Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="666ed-261">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="666ed-262">Abra la **StoreManagerController** clase.</span><span class="sxs-lookup"><span data-stu-id="666ed-262">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="666ed-263">Para ello, expanda el **controladores** carpeta y haga doble clic en **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="666ed-263">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="666ed-264">Reemplace el **HTTP-GET editar** método de acción con el código siguiente para recuperar la correspondiente **álbum** , así como la **géneros** y **intérpretes**se enumeran.</span><span class="sxs-lookup"><span data-stu-id="666ed-264">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="666ed-265">(Código de fragmento de código: *aplicaciones auxiliares de ASP.NET MVC 4 y los formularios y validación - Ex3 StoreManagerController HTTP-GET Editar acción*)</span><span class="sxs-lookup"><span data-stu-id="666ed-265">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="666ed-266">Usa **System.Web.Mvc** **SelectList** de intérpretes y géneros en lugar de la **System.Collections.Generic** lista.</span><span class="sxs-lookup"><span data-stu-id="666ed-266">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="666ed-267">**SelectList** es una forma más limpia para rellenar listas desplegables HTML y administran cosas como la selección actual.</span><span class="sxs-lookup"><span data-stu-id="666ed-267">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="666ed-268">Creación de instancias y configuración posterior de estos objetos de modelo de vista en la acción de controlador hará que el escenario de la forma de editar limpiador.</span><span class="sxs-lookup"><span data-stu-id="666ed-268">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="666ed-269">Tarea 2: crear la vista de edición</span><span class="sxs-lookup"><span data-stu-id="666ed-269">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="666ed-270">En esta tarea, creará una plantilla de vista de edición que más adelante se mostrará las propiedades de álbum.</span><span class="sxs-lookup"><span data-stu-id="666ed-270">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="666ed-271">Crear la vista de edición.</span><span class="sxs-lookup"><span data-stu-id="666ed-271">Create the Edit View.</span></span> <span data-ttu-id="666ed-272">Para ello, pulse el botón derecho dentro de la **editar** método de acción y seleccione **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="666ed-272">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="666ed-273">En el cuadro de diálogo Agregar vista, compruebe que el nombre de vista es **editar**.</span><span class="sxs-lookup"><span data-stu-id="666ed-273">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="666ed-274">Compruebe el **crear una vista fuertemente tipada** casilla de verificación y seleccione **álbum (MvcMusicStore.Models)** desde el **Ver clase de datos** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="666ed-274">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="666ed-275">Seleccione **editar** desde el **plantilla Scaffold** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="666ed-275">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="666ed-276">Deje los demás campos con sus valores predeterminados y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="666ed-276">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="666ed-277">![Agregar una vista de edición](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "agregar una vista de edición")</span><span class="sxs-lookup"><span data-stu-id="666ed-277">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="666ed-278">*Agregar una vista de edición*</span><span class="sxs-lookup"><span data-stu-id="666ed-278">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="666ed-279">Tarea 3: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="666ed-279">Task 3 - Running the Application</span></span>

<span data-ttu-id="666ed-280">En esta tarea, realizará las pruebas que el **StoreManager** **editar** página de vista muestra los valores de las propiedades para el álbum que se pasa como parámetro.</span><span class="sxs-lookup"><span data-stu-id="666ed-280">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="666ed-281">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="666ed-281">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="666ed-282">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="666ed-282">The project starts in the Home page.</span></span> <span data-ttu-id="666ed-283">Cambiar la dirección URL de **/StoreManager/Edit/1** para comprobar que se muestran los valores de las propiedades para el álbum pasado.</span><span class="sxs-lookup"><span data-stu-id="666ed-283">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="666ed-284">![Exploración Editar vista del álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "exploración Editar vista del álbum")</span><span class="sxs-lookup"><span data-stu-id="666ed-284">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="666ed-285">*Exploración de la vista de edición del álbum*</span><span class="sxs-lookup"><span data-stu-id="666ed-285">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="666ed-286">Tarea 4: implementación de listas desplegables en la plantilla del Editor de álbum</span><span class="sxs-lookup"><span data-stu-id="666ed-286">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="666ed-287">En esta tarea, agregará listas desplegables para la plantilla de vista creada en la última tarea, para que el usuario puede seleccionar de una lista de intérpretes, géneros.</span><span class="sxs-lookup"><span data-stu-id="666ed-287">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="666ed-288">Reemplazar todo el **álbum** fieldset código con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="666ed-288">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="666ed-289">Un **Html.DropDownList** se ha agregado la aplicación auxiliar para representar listas desplegables para elegir intérpretes y géneros.</span><span class="sxs-lookup"><span data-stu-id="666ed-289">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="666ed-290">Los parámetros pasados a **Html.DropDownList** son:</span><span class="sxs-lookup"><span data-stu-id="666ed-290">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="666ed-291">El nombre del campo de formulario (**&quot;ArtistId&quot;**).</span><span class="sxs-lookup"><span data-stu-id="666ed-291">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="666ed-292">El **SelectList** de valores de la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="666ed-292">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="666ed-293">Tarea 5: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="666ed-293">Task 5 - Running the Application</span></span>

<span data-ttu-id="666ed-294">En esta tarea, realizará las pruebas que el **StoreManager** **editar** página de vista muestra listas desplegables en lugar de los campos de texto de intérprete y el identificador de género.</span><span class="sxs-lookup"><span data-stu-id="666ed-294">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="666ed-295">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="666ed-295">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="666ed-296">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="666ed-296">The project starts in the Home page.</span></span> <span data-ttu-id="666ed-297">Cambiar la dirección URL de **/StoreManager/Edit/1** para comprobar que muestra listas desplegables en lugar de los campos de texto de intérprete y el identificador de género.</span><span class="sxs-lookup"><span data-stu-id="666ed-297">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="666ed-298">![Exploración Editar vista del álbum con listas desplegables](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "vista de edición del álbum de exploración con listas desplegables")</span><span class="sxs-lookup"><span data-stu-id="666ed-298">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="666ed-299">*Exploración de la vista de edición del álbum, esta vez con listas desplegables*</span><span class="sxs-lookup"><span data-stu-id="666ed-299">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="666ed-300">Tarea 6: implementar el método de acción HTTP-POST editar</span><span class="sxs-lookup"><span data-stu-id="666ed-300">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="666ed-301">Ahora que la vista de edición se muestra según lo esperado, debe implementar el método HTTP POST Editar acción para guardar los cambios realizados en el álbum.</span><span class="sxs-lookup"><span data-stu-id="666ed-301">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="666ed-302">Cierre el explorador si es necesario, para volver a la ventana de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="666ed-302">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="666ed-303">Abra **StoreManagerController** desde el **controladores** carpeta.</span><span class="sxs-lookup"><span data-stu-id="666ed-303">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="666ed-304">Reemplace **HTTP-POST editar** código del método de acción con los siguientes (tenga en cuenta que el método que se debe reemplazar es una versión sobrecargada que recibe dos parámetros):</span><span class="sxs-lookup"><span data-stu-id="666ed-304">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="666ed-305">(Código de fragmento de código: *aplicaciones auxiliares de ASP.NET MVC 4 y los formularios y validación - Ex3 StoreManagerController HTTP-POST Editar acción*)</span><span class="sxs-lookup"><span data-stu-id="666ed-305">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="666ed-306">Este método se ejecutará cuando el usuario hace clic en el **guardar** botón de la vista y realiza una solicitud HTTP-POST de los valores de formulario al servidor para hacer que persistan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="666ed-306">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="666ed-307">El elemento decorator **[HttpPost]** indica que el método se debe utilizar en esos escenarios HTTP-POST.</span><span class="sxs-lookup"><span data-stu-id="666ed-307">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="666ed-308">El método toma un **álbum** objeto.</span><span class="sxs-lookup"><span data-stu-id="666ed-308">The method takes an **Album** object.</span></span> <span data-ttu-id="666ed-309">ASP.NET MVC creará automáticamente el objeto de álbum desde la expuesto &lt;formulario&gt; valores.</span><span class="sxs-lookup"><span data-stu-id="666ed-309">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="666ed-310">El método llevará a cabo estos pasos:</span><span class="sxs-lookup"><span data-stu-id="666ed-310">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="666ed-311">Si el modelo es válido:</span><span class="sxs-lookup"><span data-stu-id="666ed-311">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="666ed-312">Actualice la entrada de álbum en el contexto para marcarla como un objeto modificado.</span><span class="sxs-lookup"><span data-stu-id="666ed-312">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="666ed-313">Guarde los cambios y redirigir a la vista de índice.</span><span class="sxs-lookup"><span data-stu-id="666ed-313">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="666ed-314">Si el modelo no es válido, rellenará el elemento ViewBag con el **GenreId** y **ArtistId**, a continuación, devolverá la vista con el objeto de álbum recibido para permitir al usuario realizar cualquier actualización necesaria.</span><span class="sxs-lookup"><span data-stu-id="666ed-314">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="666ed-315">Tarea 7: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="666ed-315">Task 7 - Running the Application</span></span>

<span data-ttu-id="666ed-316">En esta tarea, realizará las pruebas que el **StoreManager editar** página de vista realmente guarda los datos actualizados del álbum en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="666ed-316">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="666ed-317">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="666ed-317">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="666ed-318">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="666ed-318">The project starts in the Home page.</span></span> <span data-ttu-id="666ed-319">Cambiar la dirección URL de **/StoreManager/Edit/1**.</span><span class="sxs-lookup"><span data-stu-id="666ed-319">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="666ed-320">Cambie el título de álbum a **carga** y haga clic en **guardar**.</span><span class="sxs-lookup"><span data-stu-id="666ed-320">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="666ed-321">Compruebe que el título del álbum cambia realmente en la lista de álbumes.</span><span class="sxs-lookup"><span data-stu-id="666ed-321">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="666ed-322">![Actualizar un álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "actualizar un álbum")</span><span class="sxs-lookup"><span data-stu-id="666ed-322">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="666ed-323">*Actualizar un álbum*</span><span class="sxs-lookup"><span data-stu-id="666ed-323">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="666ed-324">Ejercicio 4: Agregar una vista de creación</span><span class="sxs-lookup"><span data-stu-id="666ed-324">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="666ed-325">Ahora que el **StoreManagerController** es compatible con la **editar** capacidad, en este ejercicio obtendrá información sobre cómo agregar una plantilla Create View para permitir que almacenar los administradores agregarán álbumes nuevos a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="666ed-325">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="666ed-326">Como hizo con la funcionalidad de edición, se implementará el escenario de crear mediante dos métodos independientes dentro de la **StoreManagerController** clase:</span><span class="sxs-lookup"><span data-stu-id="666ed-326">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="666ed-327">Un método de acción mostrará un formulario vacío cuando visitan gerentes por primera vez el **/StoreManager/crear** dirección URL.</span><span class="sxs-lookup"><span data-stu-id="666ed-327">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="666ed-328">Un segundo método de acción controlará el escenario donde el administrador del almacén hace clic en el **guardar** situado dentro del formulario y envían los valores a la **/StoreManager/crear** dirección URL como una solicitud HTTP-POST.</span><span class="sxs-lookup"><span data-stu-id="666ed-328">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="666ed-329">Tarea 1: implementar el método de acción HTTP-GET crear</span><span class="sxs-lookup"><span data-stu-id="666ed-329">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="666ed-330">En esta tarea, implementará la versión de HTTP-GET del método de acción Create para recuperar una lista de todos los géneros e intérpretes, empaquete estos datos en un **StoreManagerViewModel** objeto, que, a continuación, se pasará a una plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="666ed-330">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="666ed-331">Abra la **comenzar** solución ubicado en **origen/Ex4-AddingACreateView/Begin/** carpeta.</span><span class="sxs-lookup"><span data-stu-id="666ed-331">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="666ed-332">En caso contrario, puede seguir usando el **final** solución obtenido siguiendo el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="666ed-332">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="666ed-333">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="666ed-333">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="666ed-334">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="666ed-334">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="666ed-335">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="666ed-335">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="666ed-336">Por último, compile la solución haciendo clic en **generar** | **generar solución**.</span><span class="sxs-lookup"><span data-stu-id="666ed-336">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="666ed-337">Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="666ed-337">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="666ed-338">Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="666ed-338">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="666ed-339">Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="666ed-339">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="666ed-340">Abra **StoreManagerController** clase.</span><span class="sxs-lookup"><span data-stu-id="666ed-340">Open **StoreManagerController** class.</span></span> <span data-ttu-id="666ed-341">Para ello, expanda el **controladores** carpeta y haga doble clic en **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="666ed-341">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="666ed-342">Reemplace el **crear** código del método de acción con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="666ed-342">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="666ed-343">(Código de fragmento de código: *aplicaciones auxiliares de ASP.NET MVC 4 y los formularios y validación - Ex4 StoreManagerController HTTP-GET Crear acción*)</span><span class="sxs-lookup"><span data-stu-id="666ed-343">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="666ed-344">Tarea 2: agregar la vista de creación</span><span class="sxs-lookup"><span data-stu-id="666ed-344">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="666ed-345">En esta tarea, agregará la plantilla Create View que se mostrará un nuevo formulario de álbum (vacío).</span><span class="sxs-lookup"><span data-stu-id="666ed-345">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="666ed-346">Pulse el botón derecho dentro de la **crear** método de acción y seleccione **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="666ed-346">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="666ed-347">Se abrirá el cuadro de diálogo Agregar vista.</span><span class="sxs-lookup"><span data-stu-id="666ed-347">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="666ed-348">En el cuadro de diálogo Agregar vista, compruebe que el nombre de vista es **crear**.</span><span class="sxs-lookup"><span data-stu-id="666ed-348">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="666ed-349">Seleccione el **crear una vista fuertemente tipada** opción y seleccione **álbum (MvcMusicStore.Models)** desde el **clase modelo** desplegable y **crear** desde el **plantilla Scaffold** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="666ed-349">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="666ed-350">Deje los demás campos con sus valores predeterminados y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="666ed-350">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="666ed-351">![Agregar una vista de creación](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "agregar-a-create-view.png")</span><span class="sxs-lookup"><span data-stu-id="666ed-351">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="666ed-352">*Agregar la vista de creación*</span><span class="sxs-lookup"><span data-stu-id="666ed-352">*Adding the Create View*</span></span>
3. <span data-ttu-id="666ed-353">Actualización de la **GenreId** y **ArtistId** campos que se utilizan una lista desplegable, tal y como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="666ed-353">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="666ed-354">Tarea 3: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="666ed-354">Task 3 - Running the Application</span></span>

<span data-ttu-id="666ed-355">En esta tarea, realizará las pruebas que el **StoreManager** **crear** página de vista muestra un formulario de álbum vacío.</span><span class="sxs-lookup"><span data-stu-id="666ed-355">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="666ed-356">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="666ed-356">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="666ed-357">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="666ed-357">The project starts in the Home page.</span></span> <span data-ttu-id="666ed-358">Cambiar la dirección URL de **/StoreManager/crear**.</span><span class="sxs-lookup"><span data-stu-id="666ed-358">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="666ed-359">Compruebe que se muestra un formulario vacío con el que rellenar las nuevas propiedades de álbum.</span><span class="sxs-lookup"><span data-stu-id="666ed-359">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="666ed-360">![Crear vista con un formulario vacío](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View con un formulario vacío")</span><span class="sxs-lookup"><span data-stu-id="666ed-360">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="666ed-361">*Crear vista con un formulario vacío*</span><span class="sxs-lookup"><span data-stu-id="666ed-361">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="666ed-362">Tarea 4: implementar el HTTP-POST crear método de acción</span><span class="sxs-lookup"><span data-stu-id="666ed-362">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="666ed-363">En esta tarea, implementará la versión de HTTP-POST del método de acción Create que se invoca cuando un usuario hace clic en el **guardar** botón.</span><span class="sxs-lookup"><span data-stu-id="666ed-363">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="666ed-364">El método debe guardar el nuevo álbum en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="666ed-364">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="666ed-365">Cierre el explorador si es necesario, para volver a la ventana de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="666ed-365">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="666ed-366">Abra **StoreManagerController** clase.</span><span class="sxs-lookup"><span data-stu-id="666ed-366">Open **StoreManagerController** class.</span></span> <span data-ttu-id="666ed-367">Para ello, expanda el **controladores** carpeta y haga doble clic en **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="666ed-367">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="666ed-368">Reemplace **crear HTTP-POST** código del método de acción con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="666ed-368">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="666ed-369">(Código de fragmento de código: *aplicaciones auxiliares de ASP.NET MVC 4 y los formularios y validación - Ex4 StoreManagerController HTTP - POST acción crear*)</span><span class="sxs-lookup"><span data-stu-id="666ed-369">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="666ed-370">La acción de creación es muy similar al método de acción de edición anterior pero en lugar de establecer el objeto como modificada, se agrega al contexto.</span><span class="sxs-lookup"><span data-stu-id="666ed-370">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="666ed-371">Tarea 5: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="666ed-371">Task 5 - Running the Application</span></span>

<span data-ttu-id="666ed-372">En esta tarea, realizará las pruebas que el **StoreManager crear** página de vista le permite crear un nuevo álbum y, a continuación, se redirige a la vista de índice StoreManager.</span><span class="sxs-lookup"><span data-stu-id="666ed-372">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="666ed-373">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="666ed-373">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="666ed-374">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="666ed-374">The project starts in the Home page.</span></span> <span data-ttu-id="666ed-375">Cambiar la dirección URL de **/StoreManager/crear**.</span><span class="sxs-lookup"><span data-stu-id="666ed-375">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="666ed-376">Rellenar todos los campos de formulario con los datos para un nuevo álbum, como el de la siguiente ilustración:</span><span class="sxs-lookup"><span data-stu-id="666ed-376">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="666ed-377">![Crear un álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "crear un álbum")</span><span class="sxs-lookup"><span data-stu-id="666ed-377">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="666ed-378">*Crear un álbum*</span><span class="sxs-lookup"><span data-stu-id="666ed-378">*Creating an Album*</span></span>
3. <span data-ttu-id="666ed-379">Compruebe que se le redirigirá a la vista de índice StoreManager que incluye el nuevo álbum que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="666ed-379">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="666ed-380">![Nuevo álbum creado](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "nuevo álbum creado")</span><span class="sxs-lookup"><span data-stu-id="666ed-380">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="666ed-381">*Nuevo álbum creado*</span><span class="sxs-lookup"><span data-stu-id="666ed-381">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="666ed-382">Ejercicio 5: Control de eliminación</span><span class="sxs-lookup"><span data-stu-id="666ed-382">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="666ed-383">La capacidad de eliminar álbumes aún no está implementada.</span><span class="sxs-lookup"><span data-stu-id="666ed-383">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="666ed-384">Esto es lo que este ejercicio será aproximadamente.</span><span class="sxs-lookup"><span data-stu-id="666ed-384">This is what this exercise will be about.</span></span> <span data-ttu-id="666ed-385">Al igual que antes, implementará el escenario de eliminación mediante dos métodos independientes dentro de la **StoreManagerController** clase:</span><span class="sxs-lookup"><span data-stu-id="666ed-385">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="666ed-386">Un método de acción mostrará un formulario de confirmación</span><span class="sxs-lookup"><span data-stu-id="666ed-386">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="666ed-387">Un segundo método de acción controlará el envío del formulario</span><span class="sxs-lookup"><span data-stu-id="666ed-387">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="666ed-388">Tarea 1: implementar el método de acción Delete de HTTP-GET</span><span class="sxs-lookup"><span data-stu-id="666ed-388">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="666ed-389">En esta tarea, implementará la versión de HTTP-GET del método de acción Delete para recuperar información del álbum.</span><span class="sxs-lookup"><span data-stu-id="666ed-389">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="666ed-390">Abra la **comenzar** solución ubicado en **origen/Ex5-HandlingDeletion/Begin/** carpeta.</span><span class="sxs-lookup"><span data-stu-id="666ed-390">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="666ed-391">En caso contrario, puede seguir usando el **final** solución obtenido siguiendo el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="666ed-391">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="666ed-392">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="666ed-392">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="666ed-393">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="666ed-393">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="666ed-394">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="666ed-394">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="666ed-395">Por último, compile la solución haciendo clic en **generar** | **generar solución**.</span><span class="sxs-lookup"><span data-stu-id="666ed-395">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="666ed-396">Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="666ed-396">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="666ed-397">Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="666ed-397">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="666ed-398">Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="666ed-398">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="666ed-399">Abra **StoreManagerController** clase.</span><span class="sxs-lookup"><span data-stu-id="666ed-399">Open **StoreManagerController** class.</span></span> <span data-ttu-id="666ed-400">Para ello, expanda el **controladores** carpeta y haga doble clic en **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="666ed-400">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="666ed-401">La acción de controlador de eliminación es exactamente igual que la acción de controlador de detalles del almacén anterior: consulta el **álbum** objeto de la base de datos, utilizando la **Id. de** proporcionado en la dirección URL y devuelve la adecuado **vista**.</span><span class="sxs-lookup"><span data-stu-id="666ed-401">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="666ed-402">Para ello, reemplace el HTTP-GET **eliminar** código del método de acción con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="666ed-402">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="666ed-403">(Código de fragmento de código: *aplicaciones auxiliares de ASP.NET MVC 4 y los formularios y validación - HTTP-GET eliminación de control de Ex5 Eliminar acción*)</span><span class="sxs-lookup"><span data-stu-id="666ed-403">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="666ed-404">Pulse el botón derecho dentro de la **eliminar** método de acción y seleccione **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="666ed-404">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="666ed-405">Se abrirá el cuadro de diálogo Agregar vista.</span><span class="sxs-lookup"><span data-stu-id="666ed-405">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="666ed-406">En el cuadro de diálogo Agregar vista, compruebe que el nombre de vista es **eliminar**.</span><span class="sxs-lookup"><span data-stu-id="666ed-406">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="666ed-407">Seleccione el **crear una vista fuertemente tipada** opción y seleccione **álbum (MvcMusicStore.Models)** desde el **clase modelo** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="666ed-407">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="666ed-408">Seleccione **eliminar** desde el **plantilla Scaffold** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="666ed-408">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="666ed-409">Deje los demás campos con sus valores predeterminados y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="666ed-409">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="666ed-410">![Agregar una vista de eliminación](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "agregar una vista de eliminación")</span><span class="sxs-lookup"><span data-stu-id="666ed-410">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="666ed-411">*Agregar una vista de eliminación*</span><span class="sxs-lookup"><span data-stu-id="666ed-411">*Adding a Delete View*</span></span>
6. <span data-ttu-id="666ed-412">La plantilla de eliminación muestra todos los campos del modelo.</span><span class="sxs-lookup"><span data-stu-id="666ed-412">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="666ed-413">Se mostrará solo los título del álbum.</span><span class="sxs-lookup"><span data-stu-id="666ed-413">You will show only the album's title.</span></span> <span data-ttu-id="666ed-414">Para ello, reemplace el contenido de la vista con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="666ed-414">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="666ed-415">Tarea 2: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="666ed-415">Task 2 - Running the Application</span></span>

<span data-ttu-id="666ed-416">En esta tarea, realizará las pruebas que el **StoreManager** **eliminar** página de vista muestra un formulario de eliminación de confirmación.</span><span class="sxs-lookup"><span data-stu-id="666ed-416">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="666ed-417">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="666ed-417">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="666ed-418">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="666ed-418">The project starts in the Home page.</span></span> <span data-ttu-id="666ed-419">Cambiar la dirección URL de **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="666ed-419">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="666ed-420">Seleccione un álbum que desea eliminar haciendo clic en **eliminar** y compruebe que se cargue la nueva vista.</span><span class="sxs-lookup"><span data-stu-id="666ed-420">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="666ed-421">![Al eliminar un álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "al eliminar un álbum")</span><span class="sxs-lookup"><span data-stu-id="666ed-421">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="666ed-422">*Al eliminar un álbum*</span><span class="sxs-lookup"><span data-stu-id="666ed-422">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="666ed-423">Tarea 3: implementar el método de acción Delete de HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="666ed-423">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="666ed-424">En esta tarea, implementará la versión de HTTP-POST del método de acción Delete que se invoca cuando un usuario hace clic en el **eliminar** botón.</span><span class="sxs-lookup"><span data-stu-id="666ed-424">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="666ed-425">El método debe eliminar el álbum en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="666ed-425">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="666ed-426">Cierre el explorador si es necesario, para volver a la ventana de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="666ed-426">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="666ed-427">Abra **StoreManagerController** clase.</span><span class="sxs-lookup"><span data-stu-id="666ed-427">Open **StoreManagerController** class.</span></span> <span data-ttu-id="666ed-428">Para ello, expanda el **controladores** carpeta y haga doble clic en **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="666ed-428">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="666ed-429">Reemplace **HTTP-POST eliminar** código del método de acción con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="666ed-429">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="666ed-430">(Código de fragmento de código: *aplicaciones auxiliares de ASP.NET MVC 4 y los formularios y validación - HTTP-POST eliminación de control de Ex5 Eliminar acción*)</span><span class="sxs-lookup"><span data-stu-id="666ed-430">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="666ed-431">Tarea 4: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="666ed-431">Task 4 - Running the Application</span></span>

<span data-ttu-id="666ed-432">En esta tarea, realizará las pruebas que el **StoreManager Delete** ver página permite eliminar un álbum y, a continuación, redirige a la vista de índice StoreManager.</span><span class="sxs-lookup"><span data-stu-id="666ed-432">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="666ed-433">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="666ed-433">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="666ed-434">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="666ed-434">The project starts in the Home page.</span></span> <span data-ttu-id="666ed-435">Cambiar la dirección URL de **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="666ed-435">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="666ed-436">Seleccione un álbum que desea eliminar haciendo clic en **eliminar.**</span><span class="sxs-lookup"><span data-stu-id="666ed-436">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="666ed-437">Confirmar la eliminación, haga clic **eliminar** botón:</span><span class="sxs-lookup"><span data-stu-id="666ed-437">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="666ed-438">![Al eliminar un álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "al eliminar un álbum")</span><span class="sxs-lookup"><span data-stu-id="666ed-438">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="666ed-439">*Al eliminar un álbum*</span><span class="sxs-lookup"><span data-stu-id="666ed-439">*Deleting an Album*</span></span>
3. <span data-ttu-id="666ed-440">Compruebe que se eliminó el álbum ya que no aparece en la **índice** página.</span><span class="sxs-lookup"><span data-stu-id="666ed-440">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="666ed-441">Ejercicio 6: Agregar validación</span><span class="sxs-lookup"><span data-stu-id="666ed-441">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="666ed-442">Actualmente, las formas de crear y editar que tiene en su lugar no realizan ningún tipo de validación.</span><span class="sxs-lookup"><span data-stu-id="666ed-442">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="666ed-443">Si el usuario deja un campo obligatorio en blanco o escriba las letras en el campo Precio, será el primer error que se obtendrá de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="666ed-443">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="666ed-444">Puede agregar validación a la aplicación mediante la adición de anotaciones de datos a la clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="666ed-444">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="666ed-445">Las anotaciones de datos permiten que describe las reglas que desee aplicadas a las propiedades del modelo y ASP.NET MVC se encargará de aplicar y mostrar los mensajes adecuados a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="666ed-445">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="666ed-446">Tarea 1: agregar anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="666ed-446">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="666ed-447">En esta tarea, agregará las anotaciones de datos para el modelo de álbum que harán que la página de creación y edición de mostrar mensajes de validación cuando corresponda.</span><span class="sxs-lookup"><span data-stu-id="666ed-447">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="666ed-448">Para una clase de modelo simple, agregar una anotación de datos se controla simplemente mediante la adición de un **con** instrucción para **System.ComponentModel.DataAnnotation**, a continuación, colocar un **[obligatorio]**atributo en las propiedades adecuadas.</span><span class="sxs-lookup"><span data-stu-id="666ed-448">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="666ed-449">En el ejemplo siguiente, se haría que el **nombre** propiedad un campo obligatorio en la vista.</span><span class="sxs-lookup"><span data-stu-id="666ed-449">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="666ed-450">Esto es un poco más complejo en casos como este aplicación donde se genera el Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="666ed-450">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="666ed-451">Si agrega las anotaciones de datos directamente a las clases del modelo, se sobrescribirán si actualiza el modelo desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="666ed-451">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="666ed-452">En su lugar, puede hacer uso de clases parciales de metadatos que existirá para albergar las anotaciones y se asocian con el modelo de clases con el **[MetadataType]** atributo.</span><span class="sxs-lookup"><span data-stu-id="666ed-452">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="666ed-453">Abra la **comenzar** solución ubicado en **origen/Ex6-AddingValidation/Begin/** carpeta.</span><span class="sxs-lookup"><span data-stu-id="666ed-453">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="666ed-454">En caso contrario, puede seguir usando el **final** solución obtenido siguiendo el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="666ed-454">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="666ed-455">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="666ed-455">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="666ed-456">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="666ed-456">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="666ed-457">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="666ed-457">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="666ed-458">Por último, compile la solución haciendo clic en **generar** | **generar solución**.</span><span class="sxs-lookup"><span data-stu-id="666ed-458">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="666ed-459">Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="666ed-459">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="666ed-460">Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="666ed-460">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="666ed-461">Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="666ed-461">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="666ed-462">Abra la **Album.cs** desde el **modelos** carpeta.</span><span class="sxs-lookup"><span data-stu-id="666ed-462">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="666ed-463">Reemplace **Album.cs** de contenido con el código resaltado, por lo que tenga un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="666ed-463">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="666ed-464">La línea **[DisplayFormat(ConvertEmptyStringToNull=false)]** indica que las cadenas vacías del modelo no puede convertirse en null cuando se actualiza el campo de datos del origen de datos.</span><span class="sxs-lookup"><span data-stu-id="666ed-464">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="666ed-465">Este valor evitará una excepción cuando Entity Framework asigna valores null para el modelo antes de la anotación de datos valida los campos.</span><span class="sxs-lookup"><span data-stu-id="666ed-465">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="666ed-466">(Código de fragmento de código: *aplicaciones auxiliares de ASP.NET MVC 4 y los formularios y validación - clase parcial de metadatos de álbum Ex6*)</span><span class="sxs-lookup"><span data-stu-id="666ed-466">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="666ed-467">Esto **álbum** clase parcial tiene un **MetadataType** atributo que señala a la **AlbumMetaData** clase para las anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="666ed-467">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="666ed-468">Estos son algunos de los atributos de anotación de datos que usa para anotar el modelo de álbum:</span><span class="sxs-lookup"><span data-stu-id="666ed-468">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="666ed-469">Requerido: indica que la propiedad es un campo obligatorio</span><span class="sxs-lookup"><span data-stu-id="666ed-469">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="666ed-470">DisplayName: define el texto que se utilizará en los campos de formulario y mensajes de validación</span><span class="sxs-lookup"><span data-stu-id="666ed-470">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="666ed-471">DisplayFormat - especifica cómo se muestran y formato de campos de datos.</span><span class="sxs-lookup"><span data-stu-id="666ed-471">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="666ed-472">StringLength - define una longitud máxima para un campo de cadena</span><span class="sxs-lookup"><span data-stu-id="666ed-472">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="666ed-473">Intervalo: da como resultado un valor máximo y mínimo para un campo numérico</span><span class="sxs-lookup"><span data-stu-id="666ed-473">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="666ed-474">ScaffoldColumn - permite ocultar campos de formularios de editor</span><span class="sxs-lookup"><span data-stu-id="666ed-474">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="666ed-475">Tarea 2: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="666ed-475">Task 2 - Running the Application</span></span>

<span data-ttu-id="666ed-476">En esta tarea, probará que validan campos, en la creación y edición de páginas con los nombres para mostrar elegidos en la última tarea.</span><span class="sxs-lookup"><span data-stu-id="666ed-476">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="666ed-477">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="666ed-477">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="666ed-478">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="666ed-478">The project starts in the Home page.</span></span> <span data-ttu-id="666ed-479">Cambiar la dirección URL de **/StoreManager/crear**.</span><span class="sxs-lookup"><span data-stu-id="666ed-479">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="666ed-480">Compruebe que los nombres para mostrar coinciden con las de la clase parcial (como **dirección URL de carátulas de álbum** en lugar de **AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="666ed-480">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="666ed-481">Haga clic en **crear**, sin rellenar el formulario.</span><span class="sxs-lookup"><span data-stu-id="666ed-481">Click **Create**, without filling the form.</span></span> <span data-ttu-id="666ed-482">Compruebe que se obtienen los mensajes de validación correspondiente.</span><span class="sxs-lookup"><span data-stu-id="666ed-482">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="666ed-483">![Validar campos en la página crear](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "validar campos en la página Crear")</span><span class="sxs-lookup"><span data-stu-id="666ed-483">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="666ed-484">*Campos validados en la página Crear*</span><span class="sxs-lookup"><span data-stu-id="666ed-484">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="666ed-485">Puede comprobar que lo mismo sucede con la **editar** página.</span><span class="sxs-lookup"><span data-stu-id="666ed-485">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="666ed-486">Cambiar la dirección URL de **/StoreManager/Edit/1** y compruebe que los nombres para mostrar coinciden con los de la clase parcial (como **dirección URL de carátulas de álbum** en lugar de **AlbumArtUrl**).</span><span class="sxs-lookup"><span data-stu-id="666ed-486">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="666ed-487">Vacía la **título** y **precio** campos y haga clic en **guardar**.</span><span class="sxs-lookup"><span data-stu-id="666ed-487">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="666ed-488">Compruebe que se obtienen los mensajes de validación correspondiente.</span><span class="sxs-lookup"><span data-stu-id="666ed-488">Verify that you get the corresponding validation messages.</span></span>

    ![Campos validados en la página Editar](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="666ed-490">*Campos validados en la página Editar*</span><span class="sxs-lookup"><span data-stu-id="666ed-490">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="666ed-491">Ejercicio 7: Uso de jQuery discreta en el cliente</span><span class="sxs-lookup"><span data-stu-id="666ed-491">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="666ed-492">En este ejercicio, obtendrá información sobre cómo habilitar la validación de jQuery MVC 4 discreto en el cliente.</span><span class="sxs-lookup"><span data-stu-id="666ed-492">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="666ed-493">JQuery discreta utiliza el prefijo de datos ajax JavaScript para invocar métodos de acción en el servidor en lugar de manera intrusiva emitir scripts de cliente en línea.</span><span class="sxs-lookup"><span data-stu-id="666ed-493">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="666ed-494">Tarea 1: ejecutar la aplicación antes de habilitar discreto jQuery</span><span class="sxs-lookup"><span data-stu-id="666ed-494">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="666ed-495">En esta tarea, se ejecutará la aplicación antes de incluir jQuery para comparar los modelos de validación.</span><span class="sxs-lookup"><span data-stu-id="666ed-495">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="666ed-496">Abra la **comenzar** solución ubicado en **origen/Ex7-UnobtrusivejQueryValidation/Begin/** carpeta.</span><span class="sxs-lookup"><span data-stu-id="666ed-496">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="666ed-497">En caso contrario, puede seguir usando el **final** solución obtenido siguiendo el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="666ed-497">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="666ed-498">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="666ed-498">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="666ed-499">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="666ed-499">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="666ed-500">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="666ed-500">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="666ed-501">Por último, compile la solución haciendo clic en **generar** | **generar solución**.</span><span class="sxs-lookup"><span data-stu-id="666ed-501">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="666ed-502">Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="666ed-502">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="666ed-503">Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="666ed-503">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="666ed-504">Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="666ed-504">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="666ed-505">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="666ed-505">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="666ed-506">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="666ed-506">The project starts in the Home page.</span></span> <span data-ttu-id="666ed-507">Examinar **/StoreManager/crear** y haga clic en **crear** sin rellenar el formulario para comprobar que aparece un mensaje de validación:</span><span class="sxs-lookup"><span data-stu-id="666ed-507">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="666ed-508">![Validación de cliente deshabilitado](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "validación del cliente deshabilitado")</span><span class="sxs-lookup"><span data-stu-id="666ed-508">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="666ed-509">*Validación de cliente deshabilitado*</span><span class="sxs-lookup"><span data-stu-id="666ed-509">*Client validation disabled*</span></span>
4. <span data-ttu-id="666ed-510">En el explorador, abra el código fuente HTML:</span><span class="sxs-lookup"><span data-stu-id="666ed-510">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="666ed-511">Tarea 2: Habilitar validación del cliente discreta</span><span class="sxs-lookup"><span data-stu-id="666ed-511">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="666ed-512">En esta tarea, habilitará jQuery **validación del cliente discreta** de **Web.config** archivo, que se establece en false en todos los nuevos proyectos de ASP.NET MVC 4 de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="666ed-512">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="666ed-513">Además, deberá agregar que referencias para realizar trabajo de validación del cliente discreto de jQuery de secuencias de comandos de las necesarias.</span><span class="sxs-lookup"><span data-stu-id="666ed-513">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="666ed-514">Abra **Web.Config** de archivos en la raíz del proyecto y asegúrese de que el **ClientValidationEnabled** y **UnobtrusiveJavaScriptEnabled** valores de claves se establecen en **true**.</span><span class="sxs-lookup"><span data-stu-id="666ed-514">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="666ed-515">También puede habilitar la validación de cliente por código en Global.asax.cs para obtener los mismos resultados:</span><span class="sxs-lookup"><span data-stu-id="666ed-515">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="666ed-516">**HtmlHelper.ClientValidationEnabled = true;**</span><span class="sxs-lookup"><span data-stu-id="666ed-516">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="666ed-517">Además, puede asignar el atributo ClientValidationEnabled en cualquier controlador de tener un comportamiento personalizado.</span><span class="sxs-lookup"><span data-stu-id="666ed-517">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="666ed-518">Abra **Create.cshtml** en **Views\StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="666ed-518">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="666ed-519">Asegúrese de que los siguientes archivos de script, **jquery.validate** y **jquery.validate.unobtrusive**, se hace referencia en el a través de la vista la &quot; **~/bundles/jqueryval** &quot; agrupación.</span><span class="sxs-lookup"><span data-stu-id="666ed-519">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view throught the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="666ed-520">Estas bibliotecas de jQuery se incluyen en los nuevos proyectos de MVC 4.</span><span class="sxs-lookup"><span data-stu-id="666ed-520">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="666ed-521">Puede encontrar más bibliotecas en el **/Scripts** carpeta de proyecto.</span><span class="sxs-lookup"><span data-stu-id="666ed-521">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="666ed-522">Para realizar esta validación bibliotecas funcionan, debe agregar una referencia a la biblioteca de jQuery framework.</span><span class="sxs-lookup"><span data-stu-id="666ed-522">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="666ed-523">Puesto que ya se ha agregado esta referencia en la  **\_Layout.cshtml** archivo, no es necesario agregar en la vista.</span><span class="sxs-lookup"><span data-stu-id="666ed-523">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="666ed-524">Tarea 3: ejecutar la validación de jQuery aplicación usando discreto</span><span class="sxs-lookup"><span data-stu-id="666ed-524">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="666ed-525">En esta tarea, realizará las pruebas que el **StoreManager** Crear vista plantilla realiza la validación del lado cliente mediante las bibliotecas de jQuery cuando el usuario crea un nuevo álbum.</span><span class="sxs-lookup"><span data-stu-id="666ed-525">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="666ed-526">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="666ed-526">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="666ed-527">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="666ed-527">The project starts in the Home page.</span></span> <span data-ttu-id="666ed-528">Examinar **/StoreManager/crear** y haga clic en **crear** sin rellenar el formulario para comprobar que aparece un mensaje de validación:</span><span class="sxs-lookup"><span data-stu-id="666ed-528">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="666ed-529">![Validación del cliente con jQuery habilitado](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "validación del cliente con jQuery habilitado")</span><span class="sxs-lookup"><span data-stu-id="666ed-529">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="666ed-530">*Validación del cliente con jQuery habilitado*</span><span class="sxs-lookup"><span data-stu-id="666ed-530">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="666ed-531">En el explorador, abra el código fuente de la vista de creación:</span><span class="sxs-lookup"><span data-stu-id="666ed-531">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

    > [!NOTE]
    > <span data-ttu-id="666ed-532">Para cada regla de validación de cliente, jQuery discreta agrega un atributo con datos-val -*rulename*=&quot;*mensaje*&quot;.</span><span class="sxs-lookup"><span data-stu-id="666ed-532">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="666ed-533">A continuación se muestra una lista de etiquetas que discreta jQuery se inserta en el campo de entrada html para realizar la validación de cliente:</span><span class="sxs-lookup"><span data-stu-id="666ed-533">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
    > 
    > - <span data-ttu-id="666ed-534">Datos val</span><span class="sxs-lookup"><span data-stu-id="666ed-534">Data-val</span></span>
    > - <span data-ttu-id="666ed-535">Número de val de datos</span><span class="sxs-lookup"><span data-stu-id="666ed-535">Data-val-number</span></span>
    > - <span data-ttu-id="666ed-536">Intervalo de datos val</span><span class="sxs-lookup"><span data-stu-id="666ed-536">Data-val-range</span></span>
    > - <span data-ttu-id="666ed-537">Datos val intervalo mínima / datos val intervalo máxima</span><span class="sxs-lookup"><span data-stu-id="666ed-537">Data-val-range-min / Data-val-range-max</span></span>
    > - <span data-ttu-id="666ed-538">Datos requeridos para val</span><span class="sxs-lookup"><span data-stu-id="666ed-538">Data-val-required</span></span>
    > - <span data-ttu-id="666ed-539">Longitud de datos de val</span><span class="sxs-lookup"><span data-stu-id="666ed-539">Data-val-length</span></span>
    > - <span data-ttu-id="666ed-540">Datos val longitud máxima / datos val longitud mínima</span><span class="sxs-lookup"><span data-stu-id="666ed-540">Data-val-length-max / Data-val-length-min</span></span>
    > 
    > <span data-ttu-id="666ed-541">Todos los valores de datos se rellenan con modelo **anotación de datos**.</span><span class="sxs-lookup"><span data-stu-id="666ed-541">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="666ed-542">A continuación, toda la lógica que trabaje en el lado del servidor se puede ejecutar en el cliente.</span><span class="sxs-lookup"><span data-stu-id="666ed-542">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="666ed-543">Por ejemplo, el atributo precio tiene la siguiente anotación de datos en el modelo:</span><span class="sxs-lookup"><span data-stu-id="666ed-543">For example, Price attribute has the following data annotation in the model:</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
    > 
    > <span data-ttu-id="666ed-544">Después de usar jQuery discreta, el código generado es:</span><span class="sxs-lookup"><span data-stu-id="666ed-544">After using Unobtrusive jQuery, the generated code is:</span></span>
    >  
    > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="666ed-545">Resumen</span><span class="sxs-lookup"><span data-stu-id="666ed-545">Summary</span></span>

<span data-ttu-id="666ed-546">Al completar este laboratorio práctico ha aprendido cómo permitir a los usuarios cambiar los datos almacenados en la base de datos con el uso de las siguientes acciones:</span><span class="sxs-lookup"><span data-stu-id="666ed-546">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="666ed-547">Las acciones de controlador como índice, crear, editar, eliminar</span><span class="sxs-lookup"><span data-stu-id="666ed-547">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="666ed-548">Característica de scaffolding de ASP.NET MVC para mostrar propiedades en una tabla HTML</span><span class="sxs-lookup"><span data-stu-id="666ed-548">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="666ed-549">Experimentan de aplicaciones auxiliares HTML personalizadas para mejorar el usuario</span><span class="sxs-lookup"><span data-stu-id="666ed-549">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="666ed-550">Métodos de acción que reaccionan a HTTP-GET o llamadas HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="666ed-550">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="666ed-551">Una plantilla de editor compartida para las plantillas de vista similares como crear y editar</span><span class="sxs-lookup"><span data-stu-id="666ed-551">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="666ed-552">Elementos de formulario, como listas desplegables</span><span class="sxs-lookup"><span data-stu-id="666ed-552">Form elements like drop-downs</span></span>
- <span data-ttu-id="666ed-553">Anotaciones de datos para la validación del modelo</span><span class="sxs-lookup"><span data-stu-id="666ed-553">Data annotations for Model validation</span></span>
- <span data-ttu-id="666ed-554">Validación del lado cliente mediante la biblioteca de jQuery discreto</span><span class="sxs-lookup"><span data-stu-id="666ed-554">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="666ed-555">Apéndice A: instalación de Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="666ed-555">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="666ed-556">Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión usando la  **[instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="666ed-556">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="666ed-557">Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* con *instalador de plataforma Web de Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="666ed-557">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="666ed-558">Vaya a [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="666ed-558">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="666ed-559">O bien, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; *Visual Studio Express 2012 for Web con SDK de Windows Azure*&quot;.</span><span class="sxs-lookup"><span data-stu-id="666ed-559">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="666ed-560">Haga clic en **instalar ahora**.</span><span class="sxs-lookup"><span data-stu-id="666ed-560">Click on **Install Now**.</span></span> <span data-ttu-id="666ed-561">Si no tiene **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo primero.</span><span class="sxs-lookup"><span data-stu-id="666ed-561">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="666ed-562">Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.</span><span class="sxs-lookup"><span data-stu-id="666ed-562">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="666ed-563">![Instale Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "instale Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="666ed-563">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="666ed-564">*Instale Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="666ed-564">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="666ed-565">Lea los términos y licencias de todos los productos y haga clic en **acepto** para continuar.</span><span class="sxs-lookup"><span data-stu-id="666ed-565">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Acepte los términos de licencia](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="666ed-567">*Acepte los términos de licencia*</span><span class="sxs-lookup"><span data-stu-id="666ed-567">*Accepting the license terms*</span></span>
5. <span data-ttu-id="666ed-568">Espere hasta que finalice el proceso de descarga e instalación.</span><span class="sxs-lookup"><span data-stu-id="666ed-568">Wait until the downloading and installation process completes.</span></span>

    ![Progreso de la instalación](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="666ed-570">*Progreso de la instalación*</span><span class="sxs-lookup"><span data-stu-id="666ed-570">*Installation progress*</span></span>
6. <span data-ttu-id="666ed-571">Cuando se complete la instalación, haga clic en **finalizar**.</span><span class="sxs-lookup"><span data-stu-id="666ed-571">When the installation completes, click **Finish**.</span></span>

    ![Se completó la instalación](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="666ed-573">*Se completó la instalación*</span><span class="sxs-lookup"><span data-stu-id="666ed-573">*Installation completed*</span></span>
7. <span data-ttu-id="666ed-574">Haga clic en **Exit** para cerrar el instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="666ed-574">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="666ed-575">Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **Express frente a**&quot;, a continuación, haga clic en el **VS Express para Web** colocar en mosaico.</span><span class="sxs-lookup"><span data-stu-id="666ed-575">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Express de VS para icono Web](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="666ed-577">*Express de VS para icono Web*</span><span class="sxs-lookup"><span data-stu-id="666ed-577">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="666ed-578">Apéndice B: usar fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="666ed-578">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="666ed-579">Con fragmentos de código, tiene todo el código que necesita a su alcance.</span><span class="sxs-lookup"><span data-stu-id="666ed-579">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="666ed-580">El documento de laboratorio le indicará exactamente cuándo utilizarlas, tal como se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="666ed-580">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="666ed-581">![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "fragmentos de código en Visual Studio para insertar código en el proyecto")</span><span class="sxs-lookup"><span data-stu-id="666ed-581">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="666ed-582">*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*</span><span class="sxs-lookup"><span data-stu-id="666ed-582">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="666ed-583">***Para agregar un fragmento de código mediante el teclado (solo C#)***</span><span class="sxs-lookup"><span data-stu-id="666ed-583">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="666ed-584">Coloque el cursor donde desea insertar el código.</span><span class="sxs-lookup"><span data-stu-id="666ed-584">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="666ed-585">Comience a escribir el nombre del fragmento (sin espacios ni guiones).</span><span class="sxs-lookup"><span data-stu-id="666ed-585">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="666ed-586">Observe como coincidencia de nombres de fragmentos de código de muestra de IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="666ed-586">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="666ed-587">Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).</span><span class="sxs-lookup"><span data-stu-id="666ed-587">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="666ed-588">Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.</span><span class="sxs-lookup"><span data-stu-id="666ed-588">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="666ed-589">![Comience a escribir el nombre del fragmento](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "comience a escribir el nombre del fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="666ed-589">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="666ed-590">*Comience a escribir el nombre del fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="666ed-590">*Start typing the snippet name*</span></span>

<span data-ttu-id="666ed-591">![Presione la tecla Tab para seleccionar el fragmento de código resaltada](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "presione Tab para seleccionar el fragmento de código resaltada")</span><span class="sxs-lookup"><span data-stu-id="666ed-591">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="666ed-592">*Presione la tecla Tab para seleccionar el fragmento de código resaltada*</span><span class="sxs-lookup"><span data-stu-id="666ed-592">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="666ed-593">![Vuelva a presionar Tab y el fragmento de código se expandirán](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "vuelva a presionar Tab y el fragmento de código se expandirán")</span><span class="sxs-lookup"><span data-stu-id="666ed-593">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="666ed-594">*Vuelva a presionar Tab y el fragmento de código se expandirán*</span><span class="sxs-lookup"><span data-stu-id="666ed-594">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="666ed-595">***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="666ed-595">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="666ed-596">Haga clic en donde desea insertar el fragmento de código.</span><span class="sxs-lookup"><span data-stu-id="666ed-596">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="666ed-597">Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.</span><span class="sxs-lookup"><span data-stu-id="666ed-597">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="666ed-598">Seleccione el fragmento de código relevante de la lista haciendo clic en él.</span><span class="sxs-lookup"><span data-stu-id="666ed-598">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="666ed-599">![Menú contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="666ed-599">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="666ed-600">*Haga clic en donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="666ed-600">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="666ed-601">![Seleccione el fragmento de código relevante de la lista haciendo clic en él](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "elegir el fragmento de código relevante de la lista haciendo clic en él")</span><span class="sxs-lookup"><span data-stu-id="666ed-601">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="666ed-602">*Seleccione el fragmento de código relevante de la lista haciendo clic en él*</span><span class="sxs-lookup"><span data-stu-id="666ed-602">*Pick the relevant snippet from the list, by clicking on it*</span></span>
