---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: Acceso a datos y modelos de ASP.NET MVC 4 | Documentos de Microsoft
author: rick-anderson
description: "Nota: Este laboratorio práctico se supone que tener conocimientos básicos de ASP.NET MVC. Si no ha usado ASP.NET MVC antes, le recomendamos que repase ASP.NET MVC 4..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: bf4bb5c6f9ad8339c3597b0d6666c7077ac476e0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="c2d30-104">Acceso a datos y modelos de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="c2d30-104">ASP.NET MVC 4 Models and Data Access</span></span>
====================
<span data-ttu-id="c2d30-105">por [Web colonias equipo](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="c2d30-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> [!NOTE]
> <span data-ttu-id="c2d30-106">Este laboratorio práctico se da por supuesto que tiene conocimientos básicos de **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-106">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="c2d30-107">Si no ha usado **ASP.NET MVC** antes, le recomendamos que repase **Fundamentos de ASP.NET MVC 4** laboratorio práctico.</span><span class="sxs-lookup"><span data-stu-id="c2d30-107">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>
> 
> <span data-ttu-id="c2d30-108">Esta práctica le guiará a través de las mejoras y nuevas características descritos anteriormente aplicando cambios menores a una aplicación Web de ejemplo proporcionada en la carpeta de origen.</span><span class="sxs-lookup"><span data-stu-id="c2d30-108">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="c2d30-109">Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web colonias, disponible en [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span><span class="sxs-lookup"><span data-stu-id="c2d30-109">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span></span>


<span data-ttu-id="c2d30-110">En **Fundamentos de MVC de ASP.NET** los laboratorios de prácticas, que se ha pasando datos codificados de forma rígida desde los controladores para las plantillas de vista.</span><span class="sxs-lookup"><span data-stu-id="c2d30-110">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="c2d30-111">Sin embargo, para crear una aplicación Web real, debe usar una base de datos real.</span><span class="sxs-lookup"><span data-stu-id="c2d30-111">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="c2d30-112">Este laboratorio práctico le mostrará cómo usar un motor de base de datos con el fin de almacenar y recuperar los datos necesarios para la aplicación de tienda de música.</span><span class="sxs-lookup"><span data-stu-id="c2d30-112">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="c2d30-113">Para hacerlo, iniciará con una base de datos existente y crear el Entity Data Model a partir de él.</span><span class="sxs-lookup"><span data-stu-id="c2d30-113">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="c2d30-114">A lo largo de este laboratorio, cumplirá el **Database First** enfoque, así como la **Code First** enfoque.</span><span class="sxs-lookup"><span data-stu-id="c2d30-114">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="c2d30-115">Sin embargo, también puede usar el **Model First** enfocar, crear el mismo modelo de uso de las herramientas y, a continuación, generar la base de datos del mismo.</span><span class="sxs-lookup"><span data-stu-id="c2d30-115">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="c2d30-116">![Primer frente a base de datos. Modelo primero](aspnet-mvc-4-models-and-data-access/_static/image1.png "frente a Database First. En primer lugar del modelo")</span><span class="sxs-lookup"><span data-stu-id="c2d30-116">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="c2d30-117">*Primer frente a base de datos. En primer lugar del modelo*</span><span class="sxs-lookup"><span data-stu-id="c2d30-117">*Database First vs. Model First*</span></span>

<span data-ttu-id="c2d30-118">Después de generar el modelo, hará que los ajustes adecuados en el StoreController para proporcionar las vistas del almacén con los datos tomados de la base de datos, en lugar de utilizar datos codificados de forma rígida.</span><span class="sxs-lookup"><span data-stu-id="c2d30-118">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="c2d30-119">No necesitará realizar cualquier cambio en las plantillas de vista porque el StoreController devolverá el mismo ViewModels a las plantillas de vista, aunque esta vez los datos depende de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-119">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="c2d30-120">**El primer enfoque de código**</span><span class="sxs-lookup"><span data-stu-id="c2d30-120">**The Code First Approach**</span></span>

<span data-ttu-id="c2d30-121">El enfoque de Code First nos permite definir el modelo desde el código sin necesidad de generar las clases que normalmente están acopladas con el marco de trabajo.</span><span class="sxs-lookup"><span data-stu-id="c2d30-121">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="c2d30-122">En el código en primer lugar, se definen los objetos del modelo con POCOs, &quot;objetos de CLR antiguos sin formato&quot;.</span><span class="sxs-lookup"><span data-stu-id="c2d30-122">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="c2d30-123">POCOs son simples clases sin formato que no tienen herencia y no implementan interfaces.</span><span class="sxs-lookup"><span data-stu-id="c2d30-123">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="c2d30-124">Podemos generar automáticamente la base de datos de ellos, o podemos usar una base de datos existente y generar la asignación de la clase desde el código.</span><span class="sxs-lookup"><span data-stu-id="c2d30-124">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="c2d30-125">Las ventajas de utilizar este enfoque es que el modelo sigue siendo independiente desde el marco de persistencia (en este caso, Entity Framework), como las clases de POCOs no están acopladas con el marco de trabajo de asignación.</span><span class="sxs-lookup"><span data-stu-id="c2d30-125">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="c2d30-126">Este laboratorio se basa en ASP.NET MVC 4 y una versión de la aplicación de ejemplo de la tienda de música personalizada y reducirse para ajustarse solo las características mostradas en este laboratorio práctico.</span><span class="sxs-lookup"><span data-stu-id="c2d30-126">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="c2d30-127">Si desea explorar todo el **tienda de música** aplicación del tutorial puede encontrarlo en [tienda de música de MVC](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="c2d30-127">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="c2d30-128">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="c2d30-128">Prerequisites</span></span>

<span data-ttu-id="c2d30-129">Debe tener los elementos siguientes para completar esta práctica:</span><span class="sxs-lookup"><span data-stu-id="c2d30-129">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="c2d30-130">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (leer [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).</span><span class="sxs-lookup"><span data-stu-id="c2d30-130">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="c2d30-131">Programa de instalación</span><span class="sxs-lookup"><span data-stu-id="c2d30-131">Setup</span></span>

<span data-ttu-id="c2d30-132">**Instalación de fragmentos de código**</span><span class="sxs-lookup"><span data-stu-id="c2d30-132">**Installing Code Snippets**</span></span>

<span data-ttu-id="c2d30-133">Para mayor comodidad, gran parte del código que se va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c2d30-133">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="c2d30-134">Para instalar los fragmentos de código que se ejecute **.\Source\Setup\CodeSnippets.vsi** archivo.</span><span class="sxs-lookup"><span data-stu-id="c2d30-134">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="c2d30-135">Si no está familiarizado con los fragmentos de código de Visual Studio y desea obtener información sobre cómo utilizarlas, puede consultar el apéndice de este documento &quot; [Apéndice C: Using Code Snippets](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="c2d30-135">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="c2d30-136">Ejercicios</span><span class="sxs-lookup"><span data-stu-id="c2d30-136">Exercises</span></span>

<span data-ttu-id="c2d30-137">Este laboratorio práctico se compone de los ejercicios siguientes:</span><span class="sxs-lookup"><span data-stu-id="c2d30-137">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="c2d30-138">Ejercicio 1: Agregar una base de datos</span><span class="sxs-lookup"><span data-stu-id="c2d30-138">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="c2d30-139">Ejercicio 2: Crear una base de datos mediante Code First</span><span class="sxs-lookup"><span data-stu-id="c2d30-139">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="c2d30-140">Ejercicio 3: Consultar la base de datos con parámetros</span><span class="sxs-lookup"><span data-stu-id="c2d30-140">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="c2d30-141">Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debería obtener después de completar los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="c2d30-141">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="c2d30-142">Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="c2d30-142">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="c2d30-143">Tiempo estimado para completar esta práctica: **35 minutos**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-143">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="c2d30-144">Ejercicio 1: Agregar una base de datos</span><span class="sxs-lookup"><span data-stu-id="c2d30-144">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="c2d30-145">En este ejercicio, obtendrá información sobre cómo agregar una base de datos con las tablas de la aplicación de MusicStore a la solución para consumir los datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-145">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="c2d30-146">Una vez que la base de datos se genera con el modelo y agrega a la solución, se modificará la clase StoreController para proporcionar la plantilla de vista con los datos tomados de la base de datos, en lugar de usar valores codificados de forma rígida.</span><span class="sxs-lookup"><span data-stu-id="c2d30-146">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="c2d30-147">Tarea 1: agregar una base de datos</span><span class="sxs-lookup"><span data-stu-id="c2d30-147">Task 1 - Adding a Database</span></span>

<span data-ttu-id="c2d30-148">En esta tarea, agregará una base de datos ya se ha creado con las tablas principales de la aplicación MusicStore a la solución.</span><span class="sxs-lookup"><span data-stu-id="c2d30-148">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="c2d30-149">Abra la **comenzar** solución ubicado en **origen/Ex1-AddingADatabaseDBFirst/Begin/** carpeta.</span><span class="sxs-lookup"><span data-stu-id="c2d30-149">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

    1. <span data-ttu-id="c2d30-150">Deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="c2d30-150">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c2d30-151">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-151">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="c2d30-152">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="c2d30-152">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="c2d30-153">Por último, compile la solución haciendo clic en **generar** | **generar solución**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-153">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c2d30-154">Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="c2d30-154">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c2d30-155">Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="c2d30-155">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c2d30-156">Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="c2d30-156">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="c2d30-157">Agregar **MvcMusicStore** archivo de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-157">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="c2d30-158">En este laboratorio práctico, va a usar una base de datos ya se ha creado denominada **MvcMusicStore.mdf**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-158">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="c2d30-159">Para ello, haga clic en **aplicación\_datos** carpeta, seleccione **agregar** y, a continuación, haga clic en **elemento existente**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-159">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="c2d30-160">Vaya a **\Source\Assets** y seleccione la **MvcMusicStore.mdf** archivo.</span><span class="sxs-lookup"><span data-stu-id="c2d30-160">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="c2d30-161">![Agregar un elemento existente](aspnet-mvc-4-models-and-data-access/_static/image2.png "agregar un elemento existente")</span><span class="sxs-lookup"><span data-stu-id="c2d30-161">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="c2d30-162">*Agregar un elemento existente*</span><span class="sxs-lookup"><span data-stu-id="c2d30-162">*Adding an Existing Item*</span></span>

    <span data-ttu-id="c2d30-163">![Archivo de base de datos de MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf archivo de base de datos")</span><span class="sxs-lookup"><span data-stu-id="c2d30-163">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="c2d30-164">*Archivo de base de datos de MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="c2d30-164">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="c2d30-165">La base de datos se ha agregado al proyecto.</span><span class="sxs-lookup"><span data-stu-id="c2d30-165">The database has been added to the project.</span></span> <span data-ttu-id="c2d30-166">Incluso cuando la base de datos se encuentra dentro de la solución, puede consultar y actualizarla a medida que se hospede en un servidor de base de datos diferente.</span><span class="sxs-lookup"><span data-stu-id="c2d30-166">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="c2d30-167">![Base de datos de MvcMusicStore en el Explorador de soluciones](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore base de datos en el Explorador de soluciones")</span><span class="sxs-lookup"><span data-stu-id="c2d30-167">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="c2d30-168">*Base de datos de MvcMusicStore en el Explorador de soluciones*</span><span class="sxs-lookup"><span data-stu-id="c2d30-168">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="c2d30-169">Compruebe la conexión a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-169">Verify the connection to the database.</span></span> <span data-ttu-id="c2d30-170">Para ello, haga doble clic en **MvcMusicStore.mdf** para establecer una conexión.</span><span class="sxs-lookup"><span data-stu-id="c2d30-170">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="c2d30-171">![Conectarse a MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "conectarse a MvcMusicStore.mdf")</span><span class="sxs-lookup"><span data-stu-id="c2d30-171">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="c2d30-172">*Conectarse a MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="c2d30-172">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="c2d30-173">Tarea 2: crear un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="c2d30-173">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="c2d30-174">En esta tarea, creará un modelo de datos para interactuar con la base de datos que agregó en la tarea anterior.</span><span class="sxs-lookup"><span data-stu-id="c2d30-174">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="c2d30-175">Crear un modelo de datos que representa la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-175">Create a data model that will represent the database.</span></span> <span data-ttu-id="c2d30-176">Para ello, en el menú contextual del explorador de soluciones la **modelos** carpeta, seleccione **agregar** y, a continuación, haga clic en **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-176">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="c2d30-177">En el **Agregar nuevo elemento** cuadro de diálogo, seleccione la **datos** plantilla y, a continuación, el **ADO.NET Entity Data Model** elemento.</span><span class="sxs-lookup"><span data-stu-id="c2d30-177">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="c2d30-178">Cambie el nombre del modelo de datos a **StoreDB.edmx** y haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-178">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="c2d30-179">![Agregar StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "agregar StoreDB ADO.NET Entity Data Model")</span><span class="sxs-lookup"><span data-stu-id="c2d30-179">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="c2d30-180">*Agregar StoreDB ADO.NET Entity Data Model*</span><span class="sxs-lookup"><span data-stu-id="c2d30-180">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="c2d30-181">El **Entity Data Model Wizard** aparecerá.</span><span class="sxs-lookup"><span data-stu-id="c2d30-181">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="c2d30-182">Este asistente le guiará a través de la creación de la capa del modelo.</span><span class="sxs-lookup"><span data-stu-id="c2d30-182">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="c2d30-183">Puesto que el modelo se debe crear en función de la recentyl de base de datos existente agregado, seleccione **generar desde la base de datos** y haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-183">Since the model should be created based on the existing database recentyl added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="c2d30-184">![Elegir el contenido del modelo](aspnet-mvc-4-models-and-data-access/_static/image7.png "elegir el contenido del modelo")</span><span class="sxs-lookup"><span data-stu-id="c2d30-184">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="c2d30-185">*Elegir el contenido del modelo*</span><span class="sxs-lookup"><span data-stu-id="c2d30-185">*Choosing the model content*</span></span>
3. <span data-ttu-id="c2d30-186">Puesto que está generando un modelo de una base de datos, debe especificar la conexión a la utilice.</span><span class="sxs-lookup"><span data-stu-id="c2d30-186">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="c2d30-187">Haga clic en **nueva conexión**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-187">Click **New Connection**.</span></span>
4. <span data-ttu-id="c2d30-188">Seleccione **archivo de base de datos de Microsoft SQL Server** y haga clic en **continuar**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-188">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="c2d30-189">![Elegir origen de datos](aspnet-mvc-4-models-and-data-access/_static/image8.png "Elegir origen de datos")</span><span class="sxs-lookup"><span data-stu-id="c2d30-189">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="c2d30-190">*Elija el cuadro de diálogo de origen de datos*</span><span class="sxs-lookup"><span data-stu-id="c2d30-190">*Choose data source dialog*</span></span>
5. <span data-ttu-id="c2d30-191">Haga clic en **examinar** y seleccione la base de datos **MvcMusicStore.mdf** ubicado en el **aplicación\_datos** carpeta y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-191">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="c2d30-192">![Propiedades de conexión](aspnet-mvc-4-models-and-data-access/_static/image9.png "propiedades de conexión")</span><span class="sxs-lookup"><span data-stu-id="c2d30-192">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="c2d30-193">*Propiedades de conexión*</span><span class="sxs-lookup"><span data-stu-id="c2d30-193">*Connection properties*</span></span>
6. <span data-ttu-id="c2d30-194">Debe tener el mismo nombre que la cadena de conexión de entidad, por lo que cambie su nombre a la clase generada **MusicStoreEntities** y haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-194">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="c2d30-195">![Elegir la conexión de datos](aspnet-mvc-4-models-and-data-access/_static/image10.png "elegir la conexión de datos")</span><span class="sxs-lookup"><span data-stu-id="c2d30-195">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="c2d30-196">*Elegir la conexión de datos*</span><span class="sxs-lookup"><span data-stu-id="c2d30-196">*Choosing the data connection*</span></span>
7. <span data-ttu-id="c2d30-197">Elija los objetos de base de datos que usará.</span><span class="sxs-lookup"><span data-stu-id="c2d30-197">Choose the database objects to use.</span></span> <span data-ttu-id="c2d30-198">Como el modelo de entidad utilizará las tablas de la base de datos, seleccione la **tablas** opción y asegúrese de que el **incluir columnas de clave externa en el modelo** y **poner en plural o en singular los generados nombres de objeto** opciones también están seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="c2d30-198">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="c2d30-199">Cambiar el Namespace de modelo para **MvcMusicStore.Model** y haga clic en **finalizar**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-199">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="c2d30-200">![Elegir los objetos de base de datos](aspnet-mvc-4-models-and-data-access/_static/image11.png "elegir los objetos de base de datos")</span><span class="sxs-lookup"><span data-stu-id="c2d30-200">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="c2d30-201">*Elegir los objetos de base de datos*</span><span class="sxs-lookup"><span data-stu-id="c2d30-201">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c2d30-202">Si se muestra un cuadro de diálogo de advertencia de seguridad, haga clic en **Aceptar** para ejecutar la plantilla y generar las clases para las entidades del modelo.</span><span class="sxs-lookup"><span data-stu-id="c2d30-202">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="c2d30-203">Un diagrama de la entidad para la base de datos aparecerá mientras se creará una clase independiente que se asigna cada tabla a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-203">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="c2d30-204">Por ejemplo, el **álbumes** tabla se representará mediante un **álbum** (clase), donde cada columna de la tabla se asignará a una propiedad de clase.</span><span class="sxs-lookup"><span data-stu-id="c2d30-204">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="c2d30-205">Esto le permitirá consultar y trabajar con objetos que representan las filas de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-205">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="c2d30-206">![Diagrama de la entidad](aspnet-mvc-4-models-and-data-access/_static/image12.png "diagrama de entidad")</span><span class="sxs-lookup"><span data-stu-id="c2d30-206">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="c2d30-207">*Diagrama de entidad*</span><span class="sxs-lookup"><span data-stu-id="c2d30-207">*Entity diagram*</span></span>

> [!NOTE]
> <span data-ttu-id="c2d30-208">Las plantillas T4 (.tt) ejecutan el código para generar las clases de entidades y las clases existentes, sobrescribirán con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="c2d30-208">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="c2d30-209">En este ejemplo, las clases de &quot;álbum&quot;, &quot;género&quot; y &quot;intérprete&quot; se sobrescriben con el código generado.</span><span class="sxs-lookup"><span data-stu-id="c2d30-209">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="c2d30-210">Tarea 3: creación de la aplicación</span><span class="sxs-lookup"><span data-stu-id="c2d30-210">Task 3 - Building the Application</span></span>

<span data-ttu-id="c2d30-211">En esta tarea, va a comprobar que, aunque la generación del modelo se ha quitado el **álbum**, **género** y **intérprete** clases del modelo, el proyecto se compila correctamente mediante el uso de las nuevas clases de modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-211">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="c2d30-212">Compile el proyecto seleccionando la **generar** elemento de menú y, a continuación, **MvcMusicStore generar**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-212">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="c2d30-213">![Compilar el proyecto](aspnet-mvc-4-models-and-data-access/_static/image13.png "compilar el proyecto")</span><span class="sxs-lookup"><span data-stu-id="c2d30-213">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="c2d30-214">*Compilar el proyecto*</span><span class="sxs-lookup"><span data-stu-id="c2d30-214">*Building the project*</span></span>
2. <span data-ttu-id="c2d30-215">El proyecto se compila correctamente.</span><span class="sxs-lookup"><span data-stu-id="c2d30-215">The project builds successfully.</span></span> <span data-ttu-id="c2d30-216">¿Por qué todavía funciona?</span><span class="sxs-lookup"><span data-stu-id="c2d30-216">Why does it still work?</span></span> <span data-ttu-id="c2d30-217">Funciona porque las tablas de base de datos tienen campos que incluyan las propiedades que se usaba en las clases quitadas **álbum** y **género**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-217">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="c2d30-218">![Compilaciones se realizó correctamente](aspnet-mvc-4-models-and-data-access/_static/image14.png "compilaciones se realizó correctamente")</span><span class="sxs-lookup"><span data-stu-id="c2d30-218">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="c2d30-219">*Compilaciones se realizó correctamente*</span><span class="sxs-lookup"><span data-stu-id="c2d30-219">*Builds succeeded*</span></span>
3. <span data-ttu-id="c2d30-220">Aunque el diseñador muestra las entidades en un formato de diagrama, realmente son clases de C#.</span><span class="sxs-lookup"><span data-stu-id="c2d30-220">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="c2d30-221">Expanda el **StoreDB.edmx** nodo en el Explorador de soluciones y, a continuación, **StoreDB.tt**, verá las nuevas entidades generadas.</span><span class="sxs-lookup"><span data-stu-id="c2d30-221">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="c2d30-222">![Los archivos generados](aspnet-mvc-4-models-and-data-access/_static/image15.png "los archivos generados")</span><span class="sxs-lookup"><span data-stu-id="c2d30-222">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="c2d30-223">*Archivos generados*</span><span class="sxs-lookup"><span data-stu-id="c2d30-223">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="c2d30-224">Tarea 4: consultar la base de datos</span><span class="sxs-lookup"><span data-stu-id="c2d30-224">Task 4 - Querying the Database</span></span>

<span data-ttu-id="c2d30-225">En esta tarea, actualizará la clase StoreController de modo que, en lugar de utilizar datos codificados de forma rígida, consultará la base de datos para recuperar la información.</span><span class="sxs-lookup"><span data-stu-id="c2d30-225">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="c2d30-226">Abra **Controllers\StoreController.cs** y agregue el siguiente campo a la clase para que contenga una instancia de la **MusicStoreEntities** (clase), denominado **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="c2d30-226">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="c2d30-227">(Código de fragmento de código: *modelos y acceso a datos - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="c2d30-227">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. <span data-ttu-id="c2d30-228">El **MusicStoreEntities** clase expone una propiedad de colección para cada tabla de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-228">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="c2d30-229">Actualización **examinar** método de acción para recuperar un género con todos los **álbumes**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-229">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="c2d30-230">(Código de fragmento de código: *modelos y acceso a datos - Ex1 almacén examinar*)</span><span class="sxs-lookup"><span data-stu-id="c2d30-230">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > <span data-ttu-id="c2d30-231">Usa una funcionalidad de .NET denominada **LINQ** (language integrated query) para escribir expresiones de consulta fuertemente tipada en estas colecciones - que ejecutar el código en la base de datos y devolver objetos que se puede programar con.</span><span class="sxs-lookup"><span data-stu-id="c2d30-231">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="c2d30-232">Para obtener más información acerca de LINQ, visite la [sitio msdn](https://msdn.microsoft.com/en-us/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="c2d30-232">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/en-us/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span></span>
3. <span data-ttu-id="c2d30-233">Actualización **índice** método de acción para recuperar todos los géneros.</span><span class="sxs-lookup"><span data-stu-id="c2d30-233">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="c2d30-234">(Código de fragmento de código: *modelos y acceso a datos - índice de almacén Ex1*)</span><span class="sxs-lookup"><span data-stu-id="c2d30-234">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. <span data-ttu-id="c2d30-235">Actualización **índice** método de acción para recuperar todos los géneros y transformar la colección a una lista.</span><span class="sxs-lookup"><span data-stu-id="c2d30-235">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="c2d30-236">(Código de fragmento de código: *modelos y acceso a datos - Ex1 almacén GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="c2d30-236">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="c2d30-237">Tarea 5: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="c2d30-237">Task 5 - Running the Application</span></span>

<span data-ttu-id="c2d30-238">En esta tarea, comprobará que el géneros almacenados en la base de datos en lugar de los codificados de forma rígida que mostrará ahora en la página de índice de almacén.</span><span class="sxs-lookup"><span data-stu-id="c2d30-238">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="c2d30-239">No es necesario cambiar la plantilla de vista porque el **StoreController** devuelve las mismas entidades como antes, aunque esta vez los datos depende de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-239">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="c2d30-240">Volver a generar la solución y presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c2d30-240">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="c2d30-241">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="c2d30-241">The project starts in the Home page.</span></span> <span data-ttu-id="c2d30-242">Compruebe que el menú de **géneros** ya no es una lista codificada, y que los datos se recuperan directamente desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-242">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="c2d30-244">*Géneros exploración desde la base de datos*</span><span class="sxs-lookup"><span data-stu-id="c2d30-244">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="c2d30-245">Ahora, vaya a cualquier género y comprobar que los álbumes que se rellenan de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-245">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="c2d30-246">![Exploración álbumes de la base de datos](aspnet-mvc-4-models-and-data-access/_static/image17.png "álbumes de exploración de la base de datos")</span><span class="sxs-lookup"><span data-stu-id="c2d30-246">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="c2d30-247">*Exploración álbumes de la base de datos*</span><span class="sxs-lookup"><span data-stu-id="c2d30-247">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="c2d30-248">Ejercicio 2: Crear una base de datos utilizando código en primer lugar</span><span class="sxs-lookup"><span data-stu-id="c2d30-248">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="c2d30-249">En este ejercicio, obtendrá información sobre cómo usar el enfoque de Code First para crear una base de datos con las tablas de la aplicación MusicStore y cómo obtener acceso a sus datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-249">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="c2d30-250">Una vez que se genera el modelo, se modificará el StoreController para proporcionar la plantilla de vista con los datos tomados de la base de datos, en lugar de usar valores codificados.</span><span class="sxs-lookup"><span data-stu-id="c2d30-250">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="c2d30-251">Si se ha completado el ejercicio 1 y ya ha trabajado con el enfoque Database First, ahora obtendrá información sobre cómo obtener los mismos resultados con un proceso diferente.</span><span class="sxs-lookup"><span data-stu-id="c2d30-251">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="c2d30-252">Las tareas que están en común con el ejercicio 1 se han marcado para facilitar su lectura.</span><span class="sxs-lookup"><span data-stu-id="c2d30-252">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="c2d30-253">Si no se han completado el ejercicio 1, pero le gustaría obtener información sobre el enfoque de Code First, puede iniciar desde este ejercicio y obtener una cobertura total del tema.</span><span class="sxs-lookup"><span data-stu-id="c2d30-253">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="c2d30-254">Tarea 1: rellenar de los datos de ejemplo</span><span class="sxs-lookup"><span data-stu-id="c2d30-254">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="c2d30-255">En esta tarea, rellenará la base de datos con datos de ejemplo cuando se crea desde el principio mediante Code First.</span><span class="sxs-lookup"><span data-stu-id="c2d30-255">In this task, you will populate the database with sample data when it is intially created using Code-First.</span></span>

1. <span data-ttu-id="c2d30-256">Abra la **comenzar** solución ubicado en **origen/Ex2-CreatingADatabaseCodeFirst/Begin/** carpeta.</span><span class="sxs-lookup"><span data-stu-id="c2d30-256">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="c2d30-257">En caso contrario, puede seguir usando el **final** solución obtenido siguiendo el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="c2d30-257">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="c2d30-258">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="c2d30-258">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c2d30-259">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-259">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="c2d30-260">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="c2d30-260">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="c2d30-261">Por último, compile la solución haciendo clic en **generar** | **generar solución**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-261">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c2d30-262">Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="c2d30-262">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c2d30-263">Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="c2d30-263">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c2d30-264">Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="c2d30-264">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="c2d30-265">Agregar el **SampleData.cs** del archivo a la **modelos** carpeta.</span><span class="sxs-lookup"><span data-stu-id="c2d30-265">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="c2d30-266">Para ello, haga clic en **modelos** carpeta, seleccione **agregar** y, a continuación, haga clic en **elemento existente**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-266">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="c2d30-267">Vaya a **\Source\Assets** y seleccione la **SampleData.cs** archivo.</span><span class="sxs-lookup"><span data-stu-id="c2d30-267">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="c2d30-268">![Datos de ejemplo rellenan código](aspnet-mvc-4-models-and-data-access/_static/image18.png "código de rellenar de datos de ejemplo")</span><span class="sxs-lookup"><span data-stu-id="c2d30-268">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="c2d30-269">*Código de rellenar de datos de ejemplo*</span><span class="sxs-lookup"><span data-stu-id="c2d30-269">*Sample data populate code*</span></span>
3. <span data-ttu-id="c2d30-270">Abra la **Global.asax.cs** de archivos y agregue el siguiente *con* instrucciones.</span><span class="sxs-lookup"><span data-stu-id="c2d30-270">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="c2d30-271">(Código de fragmento de código: *modelos y acceso a datos - Ex2 Global Asax instrucciones using*)</span><span class="sxs-lookup"><span data-stu-id="c2d30-271">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. <span data-ttu-id="c2d30-272">En el **aplicación\_Start()** método agregue la línea siguiente para establecer el inicializador de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-272">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="c2d30-273">(Código de fragmento de código: *modelos y acceso a datos - Ex2 Global Asax SetInitializer*)</span><span class="sxs-lookup"><span data-stu-id="c2d30-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="c2d30-274">Tarea 2: configurar la conexión a la base de datos</span><span class="sxs-lookup"><span data-stu-id="c2d30-274">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="c2d30-275">Ahora que ya ha agregado una base de datos para el proyecto, se escribirá el **Web.config** la cadena de conexión de archivos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-275">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="c2d30-276">Agregar una cadena de conexión en **Web.config**. Para ello, abra **Web.config** en la raíz del proyecto y reemplace la cadena de conexión denominado DefaultConnection con esta línea en el  **&lt;connectionStrings&gt;**  sección:</span><span class="sxs-lookup"><span data-stu-id="c2d30-276">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="c2d30-277">![Ubicación del archivo Web.config](aspnet-mvc-4-models-and-data-access/_static/image19.png "ubicación del archivo Web.config")</span><span class="sxs-lookup"><span data-stu-id="c2d30-277">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="c2d30-278">*Ubicación del archivo Web.config*</span><span class="sxs-lookup"><span data-stu-id="c2d30-278">*Web.config file location*</span></span>


    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="c2d30-279">Tarea 3: trabajar con el modelo</span><span class="sxs-lookup"><span data-stu-id="c2d30-279">Task 3 - Working with the Model</span></span>

<span data-ttu-id="c2d30-280">Ahora que ya ha configurado la conexión a la base de datos, vinculará el modelo con las tablas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-280">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="c2d30-281">En esta tarea, creará una clase que se vinculará a la base de datos con Code First.</span><span class="sxs-lookup"><span data-stu-id="c2d30-281">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="c2d30-282">Recuerde que hay una clase de modelo POCO existente que se deben modificar.</span><span class="sxs-lookup"><span data-stu-id="c2d30-282">Remember that there is an existent POCO model class that should be modified.</span></span>

> [!NOTE]
> <span data-ttu-id="c2d30-283">Si completó el ejercicio 1, se tenga en cuenta que este paso se realiza mediante un asistente.</span><span class="sxs-lookup"><span data-stu-id="c2d30-283">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="c2d30-284">Haciendo Code First, creará manualmente las clases que se vinculará a las entidades de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-284">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>


1. <span data-ttu-id="c2d30-285">Abra la clase de modelo POCO **género** de **modelos** carpeta del proyecto e incluir un identificador.</span><span class="sxs-lookup"><span data-stu-id="c2d30-285">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="c2d30-286">Usar una propiedad de tipo int con el nombre **GenreId**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-286">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="c2d30-287">(Código de fragmento de código: *modelos y acceso a datos - Ex2 código primera género*)</span><span class="sxs-lookup"><span data-stu-id="c2d30-287">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > <span data-ttu-id="c2d30-288">Para trabajar con las convenciones de Code First, el género de clase debe tener una propiedad de clave principal que se detectarán automáticamente.</span><span class="sxs-lookup"><span data-stu-id="c2d30-288">To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.</span></span>
    > 
    > <span data-ttu-id="c2d30-289">Puede leer más acerca de las convenciones de primer código de esta [artículo de msdn](https://msdn.microsoft.com/en-us/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="c2d30-289">You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/en-us/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span></span>
2. <span data-ttu-id="c2d30-290">Ahora, abra la clase del modelo POCO **álbum** de **modelos** carpeta del proyecto y se incluye las claves externas, cree propiedades con los nombres **GenreId** y  **ArtistId**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-290">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="c2d30-291">Esta clase ya tiene la **GenreId** para la clave principal.</span><span class="sxs-lookup"><span data-stu-id="c2d30-291">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="c2d30-292">(Código de fragmento de código: *modelos y acceso a datos - Ex2 código primer álbum*)</span><span class="sxs-lookup"><span data-stu-id="c2d30-292">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. <span data-ttu-id="c2d30-293">Abra la clase de modelo POCO **intérprete** e incluya el **ArtistId** propiedad.</span><span class="sxs-lookup"><span data-stu-id="c2d30-293">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="c2d30-294">(Código de fragmento de código: *modelos y acceso a datos - Ex2 código primer intérprete*)</span><span class="sxs-lookup"><span data-stu-id="c2d30-294">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. <span data-ttu-id="c2d30-295">Haga clic en el **modelos** carpeta del proyecto y seleccione **agregar | Clase**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-295">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="c2d30-296">Asignar nombre al archivo **MusicStoreEntities.cs**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-296">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="c2d30-297">A continuación, haga clic en **agregar.**</span><span class="sxs-lookup"><span data-stu-id="c2d30-297">Then, click **Add.**</span></span>

    <span data-ttu-id="c2d30-298">![Agregar una clase](aspnet-mvc-4-models-and-data-access/_static/image20.png "agregar una clase")</span><span class="sxs-lookup"><span data-stu-id="c2d30-298">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="c2d30-299">*Agregar un nuevo elemento*</span><span class="sxs-lookup"><span data-stu-id="c2d30-299">*Adding a new item*</span></span>

    <span data-ttu-id="c2d30-300">![Agregar una clase2](aspnet-mvc-4-models-and-data-access/_static/image21.png "agregar una clase2")</span><span class="sxs-lookup"><span data-stu-id="c2d30-300">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="c2d30-301">*Agregar una clase*</span><span class="sxs-lookup"><span data-stu-id="c2d30-301">*Adding a class*</span></span>
5. <span data-ttu-id="c2d30-302">Abra la clase que se acaba de crear, **MusicStoreEntities.cs**e incluyen los espacios de nombres **System.Data.Entity** y **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-302">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. <span data-ttu-id="c2d30-303">Reemplace la declaración de clase para extender la **DbContext** clase: declarar una variable public **DBSet** e invalide **OnModelCreating** método.</span><span class="sxs-lookup"><span data-stu-id="c2d30-303">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="c2d30-304">Después de este paso, obtendrá una clase de dominio que se vinculará el modelo con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c2d30-304">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="c2d30-305">Para ello, reemplace el código de clase por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c2d30-305">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="c2d30-306">(Código de fragmento de código: *modelos y acceso a datos - Ex2 código primera MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="c2d30-306">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="c2d30-307">Con Entity Framework **DbContext** y **DBSet** podrá consultar la clase POCO género.</span><span class="sxs-lookup"><span data-stu-id="c2d30-307">With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre.</span></span> <span data-ttu-id="c2d30-308">Extendiendo **OnModelCreating** método, se especifica en el **código** cómo género se asignarán a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-308">By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table.</span></span> <span data-ttu-id="c2d30-309">Puede encontrar más información sobre DBContext y DBSet en este artículo de msdn: [vínculo](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=vs.103).aspx)</span><span class="sxs-lookup"><span data-stu-id="c2d30-309">You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=vs.103).aspx)</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="c2d30-310">Tarea 4: consultar la base de datos</span><span class="sxs-lookup"><span data-stu-id="c2d30-310">Task 4 - Querying the Database</span></span>

<span data-ttu-id="c2d30-311">En esta tarea, actualizará la clase StoreController para que, en lugar de utilizar datos codificados de forma rígida, lo recuperará de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-311">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="c2d30-312">Esta tarea consiste en común con el ejercicio 1.</span><span class="sxs-lookup"><span data-stu-id="c2d30-312">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="c2d30-313">Si completó ejercicio 1 puede observar estos pasos son los mismos en ambos enfoques (primero la base de datos o Code first).</span><span class="sxs-lookup"><span data-stu-id="c2d30-313">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="c2d30-314">Son diferentes en la manera en que los datos se vinculan con el modelo, pero el acceso a las entidades de datos todavía es transparente desde el controlador.</span><span class="sxs-lookup"><span data-stu-id="c2d30-314">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>


1. <span data-ttu-id="c2d30-315">Abra **Controllers\StoreController.cs** y agregue el siguiente campo a la clase para que contenga una instancia de la **MusicStoreEntities** (clase), denominado **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="c2d30-315">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="c2d30-316">(Código de fragmento de código: *modelos y acceso a datos - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="c2d30-316">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. <span data-ttu-id="c2d30-317">El **MusicStoreEntities** clase expone una propiedad de colección para cada tabla de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-317">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="c2d30-318">Actualización **examinar** método de acción para recuperar un género con todos los **álbumes**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-318">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="c2d30-319">(Código de fragmento de código: *modelos y acceso a datos - Ex2 almacén examinar*)</span><span class="sxs-lookup"><span data-stu-id="c2d30-319">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="c2d30-320">Usa una funcionalidad de .NET denominada **LINQ** (language integrated query) para escribir expresiones de consulta fuertemente tipada en estas colecciones - que ejecutar el código en la base de datos y devolver objetos que se puede programar con.</span><span class="sxs-lookup"><span data-stu-id="c2d30-320">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="c2d30-321">Para obtener más información acerca de LINQ, visite la [sitio msdn](https://msdn.microsoft.com/en-us/library/bb397926(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="c2d30-321">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/en-us/library/bb397926(v=vs.110).aspx).</span></span>
3. <span data-ttu-id="c2d30-322">Actualización **índice** método de acción para recuperar todos los géneros.</span><span class="sxs-lookup"><span data-stu-id="c2d30-322">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="c2d30-323">(Código de fragmento de código: *modelos y acceso a datos - índice de almacén Ex2*)</span><span class="sxs-lookup"><span data-stu-id="c2d30-323">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. <span data-ttu-id="c2d30-324">Actualización **índice** método de acción para recuperar todos los géneros y transformar la colección a una lista.</span><span class="sxs-lookup"><span data-stu-id="c2d30-324">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="c2d30-325">(Código de fragmento de código: *modelos y acceso a datos - Ex2 almacén GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="c2d30-325">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="c2d30-326">Tarea 5: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="c2d30-326">Task 5 - Running the Application</span></span>

<span data-ttu-id="c2d30-327">En esta tarea, comprobará que el géneros almacenados en la base de datos en lugar de los codificados de forma rígida que mostrará ahora en la página de índice de almacén.</span><span class="sxs-lookup"><span data-stu-id="c2d30-327">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="c2d30-328">No es necesario cambiar la plantilla de vista porque el **StoreController** está devolviendo los mismos **StoreIndexViewModel** como antes, pero esta vez los datos depende de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-328">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="c2d30-329">Volver a generar la solución y presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c2d30-329">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="c2d30-330">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="c2d30-330">The project starts in the Home page.</span></span> <span data-ttu-id="c2d30-331">Compruebe que el menú de **géneros** ya no es una lista codificada, y que los datos se recuperan directamente desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-331">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="c2d30-333">*Géneros exploración desde la base de datos*</span><span class="sxs-lookup"><span data-stu-id="c2d30-333">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="c2d30-334">Ahora, vaya a cualquier género y comprobar que los álbumes que se rellenan de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-334">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="c2d30-335">![Exploración álbumes de la base de datos](aspnet-mvc-4-models-and-data-access/_static/image23.png "álbumes de exploración de la base de datos")</span><span class="sxs-lookup"><span data-stu-id="c2d30-335">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="c2d30-336">*Exploración álbumes de la base de datos*</span><span class="sxs-lookup"><span data-stu-id="c2d30-336">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="c2d30-337">Ejercicio 3: Consultar la base de datos con parámetros</span><span class="sxs-lookup"><span data-stu-id="c2d30-337">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="c2d30-338">En este ejercicio, obtendrá información sobre cómo consultar la base de datos con parámetros y cómo usar la forma del resultado de consulta, una característica que reduce el número base de datos tiene acceso a recuperación de datos de una manera más eficaz.</span><span class="sxs-lookup"><span data-stu-id="c2d30-338">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="c2d30-339">Para obtener más información sobre la forma de resultado de consulta, visite el siguiente [artículo de msdn](https://msdn.microsoft.com/en-us/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="c2d30-339">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/en-us/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>


<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="c2d30-340">Tarea 1: modificar StoreController para recuperar los álbumes de base de datos</span><span class="sxs-lookup"><span data-stu-id="c2d30-340">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="c2d30-341">En esta tarea, cambiará la **StoreController** clase para tener acceso a la base de datos para recuperar los álbumes de un género específico.</span><span class="sxs-lookup"><span data-stu-id="c2d30-341">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="c2d30-342">Abra la **comenzar** solución se encuentra en la **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** carpeta si desea usar el enfoque de Code First o **Source\ Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin** carpeta si desea usar el enfoque basado principalmente en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-342">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="c2d30-343">En caso contrario, puede seguir usando el **final** solución obtenido siguiendo el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="c2d30-343">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="c2d30-344">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="c2d30-344">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c2d30-345">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-345">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="c2d30-346">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="c2d30-346">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="c2d30-347">Por último, compile la solución haciendo clic en **generar** | **generar solución**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-347">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c2d30-348">Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="c2d30-348">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c2d30-349">Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="c2d30-349">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c2d30-350">Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="c2d30-350">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="c2d30-351">Abra la **StoreController** clase para cambiar la **examinar** método de acción.</span><span class="sxs-lookup"><span data-stu-id="c2d30-351">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="c2d30-352">Para ello, en el **el Explorador de soluciones**, expanda la **controladores** carpeta y haga doble clic en **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-352">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="c2d30-353">Cambiar el **examinar** método de acción para recuperar los álbumes de un género específico.</span><span class="sxs-lookup"><span data-stu-id="c2d30-353">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="c2d30-354">Para ello, reemplace el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c2d30-354">To do this, replace the following code:</span></span>

    <span data-ttu-id="c2d30-355">(Código de fragmento de código: *modelos y acceso a datos - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="c2d30-355">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="c2d30-356">Para rellenar una colección de la entidad, debe usar el **Include** método para especificar que van a recuperar los álbumes demasiado.</span><span class="sxs-lookup"><span data-stu-id="c2d30-356">To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too.</span></span> <span data-ttu-id="c2d30-357">Puede usar el. **Single()** extensión de LINQ porque en este caso se espera solo un género para un álbum.</span><span class="sxs-lookup"><span data-stu-id="c2d30-357">You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album.</span></span> <span data-ttu-id="c2d30-358">El **Single()** método toma una expresión Lambda como un parámetro, que en este caso especifica un único objeto de género tal que su nombre coincide con el valor definido.</span><span class="sxs-lookup"><span data-stu-id="c2d30-358">The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.</span></span>
    > 
    > <span data-ttu-id="c2d30-359">Se aprovechará una característica que le permite indicar otras entidades relacionadas que se desea cargar también cuando se recupere el objeto de género.</span><span class="sxs-lookup"><span data-stu-id="c2d30-359">You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="c2d30-360">Esta característica se denomina **forma de resultado de consulta**y permite reducir el número de veces que sea necesario para tener acceso a la base de datos para recuperar información.</span><span class="sxs-lookup"><span data-stu-id="c2d30-360">This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information.</span></span> <span data-ttu-id="c2d30-361">En este escenario, desea realizar una captura previa de los álbumes para el género que recupera.</span><span class="sxs-lookup"><span data-stu-id="c2d30-361">In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.</span></span>
    > 
    > <span data-ttu-id="c2d30-362">La consulta incluye **Genres.Include (&quot;álbumes&quot;)** para indicar que desea que también álbumes relacionados.</span><span class="sxs-lookup"><span data-stu-id="c2d30-362">The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well.</span></span> <span data-ttu-id="c2d30-363">Esto provocará en una aplicación más eficaz, ya que se recuperarán datos de género y álbum en una solicitud de base de datos único.</span><span class="sxs-lookup"><span data-stu-id="c2d30-363">This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="c2d30-364">Tarea 2: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="c2d30-364">Task 2 - Running the Application</span></span>

<span data-ttu-id="c2d30-365">En esta tarea, ejecutará la aplicación y recuperar los álbumes de un género específico de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-365">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="c2d30-366">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c2d30-366">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="c2d30-367">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="c2d30-367">The project starts in the Home page.</span></span> <span data-ttu-id="c2d30-368">Cambiar la dirección URL de **/almacén/examinar? género = Pop** para comprobar que se están recuperando los resultados de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-368">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="c2d30-369">![Explorar por género](aspnet-mvc-4-models-and-data-access/_static/image24.png "explorar por género")</span><span class="sxs-lookup"><span data-stu-id="c2d30-369">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="c2d30-370">*Exploración/almacén/examinar? género = Pop*</span><span class="sxs-lookup"><span data-stu-id="c2d30-370">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="c2d30-371">Tarea 3: obtener acceso a los álbumes por Id.</span><span class="sxs-lookup"><span data-stu-id="c2d30-371">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="c2d30-372">En esta tarea, se repetirá el procedimiento anterior para obtener álbumes por su identificador.</span><span class="sxs-lookup"><span data-stu-id="c2d30-372">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="c2d30-373">Cierre el explorador si es necesario, para volver a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c2d30-373">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="c2d30-374">Abra la **StoreController** clase para cambiar la **detalles** método de acción.</span><span class="sxs-lookup"><span data-stu-id="c2d30-374">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="c2d30-375">Para ello, en el **el Explorador de soluciones**, expanda la **controladores** carpeta y haga doble clic en **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-375">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="c2d30-376">Cambiar el **detalles** método de acción para recuperar los detalles de álbumes en función de sus **Id. de**. Para ello, reemplace el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c2d30-376">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="c2d30-377">(Código de fragmento de código: *modelos y acceso a datos - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="c2d30-377">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="c2d30-378">Tarea 4: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="c2d30-378">Task 4 - Running the Application</span></span>

<span data-ttu-id="c2d30-379">En esta tarea, ejecutará la aplicación en un explorador web y obtener detalles del álbum por su identificador.</span><span class="sxs-lookup"><span data-stu-id="c2d30-379">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="c2d30-380">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c2d30-380">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="c2d30-381">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="c2d30-381">The project starts in the Home page.</span></span> <span data-ttu-id="c2d30-382">Cambiar la dirección URL de **/Store/Details/51** o examinar los géneros y seleccione un álbum para comprobar que se están recuperando los resultados de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-382">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="c2d30-383">![Detalles de exploración](aspnet-mvc-4-models-and-data-access/_static/image25.png "detalles de exploración")</span><span class="sxs-lookup"><span data-stu-id="c2d30-383">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="c2d30-384">*Exploración /Store/Details/51*</span><span class="sxs-lookup"><span data-stu-id="c2d30-384">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="c2d30-385">Además, puede implementar esta aplicación a los siguientes sitios Web Windows Azure [Apéndice B: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="c2d30-385">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="c2d30-386">Resumen</span><span class="sxs-lookup"><span data-stu-id="c2d30-386">Summary</span></span>

<span data-ttu-id="c2d30-387">Por completar este laboratorio práctico ha aprendido los conceptos básicos del acceso a datos y modelos de ASP.NET MVC, usando la **Database First** enfoque, así como la **Code First** enfoque:</span><span class="sxs-lookup"><span data-stu-id="c2d30-387">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="c2d30-388">Cómo agregar una base de datos a la solución para consumir los datos</span><span class="sxs-lookup"><span data-stu-id="c2d30-388">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="c2d30-389">Cómo actualizar los controladores para proporcionar las plantillas de vista con los datos tomados de la base de datos en lugar de codificado de forma rígida</span><span class="sxs-lookup"><span data-stu-id="c2d30-389">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="c2d30-390">Cómo consultar la base de datos con parámetros</span><span class="sxs-lookup"><span data-stu-id="c2d30-390">How to query the database using parameters</span></span>
- <span data-ttu-id="c2d30-391">Cómo usar la consulta de resultados forma, una característica que reduce el número de accesos de base de datos, recuperar datos de una manera más eficaz</span><span class="sxs-lookup"><span data-stu-id="c2d30-391">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="c2d30-392">Cómo usar métodos Database First o Code First en Entity Framework de Microsoft para vincular la base de datos con el modelo</span><span class="sxs-lookup"><span data-stu-id="c2d30-392">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="c2d30-393">Apéndice A: instalación de Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="c2d30-393">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="c2d30-394">Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión usando la  **[instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="c2d30-394">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="c2d30-395">Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* con *instalador de plataforma Web de Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="c2d30-395">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="c2d30-396">Vaya a [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="c2d30-396">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="c2d30-397">O bien, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; *Visual Studio Express 2012 for Web con SDK de Windows Azure*&quot;.</span><span class="sxs-lookup"><span data-stu-id="c2d30-397">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="c2d30-398">Haga clic en **instalar ahora**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-398">Click on **Install Now**.</span></span> <span data-ttu-id="c2d30-399">Si no tiene **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo primero.</span><span class="sxs-lookup"><span data-stu-id="c2d30-399">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="c2d30-400">Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.</span><span class="sxs-lookup"><span data-stu-id="c2d30-400">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="c2d30-401">![Instale Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "instale Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="c2d30-401">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="c2d30-402">*Instale Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="c2d30-402">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="c2d30-403">Lea los términos y licencias de todos los productos y haga clic en **acepto** para continuar.</span><span class="sxs-lookup"><span data-stu-id="c2d30-403">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Acepte los términos de licencia](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="c2d30-405">*Acepte los términos de licencia*</span><span class="sxs-lookup"><span data-stu-id="c2d30-405">*Accepting the license terms*</span></span>
5. <span data-ttu-id="c2d30-406">Espere hasta que finalice el proceso de descarga e instalación.</span><span class="sxs-lookup"><span data-stu-id="c2d30-406">Wait until the downloading and installation process completes.</span></span>

    ![Progreso de la instalación](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="c2d30-408">*Progreso de la instalación*</span><span class="sxs-lookup"><span data-stu-id="c2d30-408">*Installation progress*</span></span>
6. <span data-ttu-id="c2d30-409">Cuando se complete la instalación, haga clic en **finalizar**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-409">When the installation completes, click **Finish**.</span></span>

    ![Se completó la instalación](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="c2d30-411">*Se completó la instalación*</span><span class="sxs-lookup"><span data-stu-id="c2d30-411">*Installation completed*</span></span>
7. <span data-ttu-id="c2d30-412">Haga clic en **Exit** para cerrar el instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="c2d30-412">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="c2d30-413">Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **Express frente a**&quot;, a continuación, haga clic en el **VS Express para Web** colocar en mosaico.</span><span class="sxs-lookup"><span data-stu-id="c2d30-413">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Express de VS para icono Web](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="c2d30-415">*Express de VS para icono Web*</span><span class="sxs-lookup"><span data-stu-id="c2d30-415">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="c2d30-416">Apéndice B: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy</span><span class="sxs-lookup"><span data-stu-id="c2d30-416">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="c2d30-417">Este apéndice le mostrará cómo crear un nuevo sitio web del Portal de administración de Windows Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, aprovechando la característica de publicación Web Deploy proporcionada por Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="c2d30-417">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="c2d30-418">Tarea 1: crear un nuevo sitio Web desde las ventanas de Portal de Azure</span><span class="sxs-lookup"><span data-stu-id="c2d30-418">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="c2d30-419">Vaya a la [Portal de administración de Windows Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas con su suscripción.</span><span class="sxs-lookup"><span data-stu-id="c2d30-419">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c2d30-420">Con Windows Azure puede hospedar 10 sitios Web de ASP.NET de forma gratuita y, a continuación, ajustar la escala a medida que crece el tráfico.</span><span class="sxs-lookup"><span data-stu-id="c2d30-420">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="c2d30-421">Puede registrarse [aquí](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="c2d30-421">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="c2d30-422">![Inicie sesión en el portal de Windows Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "inicie sesión en el portal de Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="c2d30-422">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="c2d30-423">*Inicie sesión en el Portal de administración de Azure*</span><span class="sxs-lookup"><span data-stu-id="c2d30-423">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="c2d30-424">Haga clic en **New** en la barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-424">Click **New** on the command bar.</span></span>

    <span data-ttu-id="c2d30-425">![Crear un nuevo sitio Web](aspnet-mvc-4-models-and-data-access/_static/image32.png "crear un nuevo sitio Web")</span><span class="sxs-lookup"><span data-stu-id="c2d30-425">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="c2d30-426">*Crear un nuevo sitio Web*</span><span class="sxs-lookup"><span data-stu-id="c2d30-426">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="c2d30-427">Haga clic en **proceso** | **sitio Web**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-427">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="c2d30-428">A continuación, seleccione **creación rápida** opción.</span><span class="sxs-lookup"><span data-stu-id="c2d30-428">Then select **Quick Create** option.</span></span> <span data-ttu-id="c2d30-429">Proporcione una dirección de URL disponible para el nuevo sitio web y haga clic en **crear sitio Web**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-429">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c2d30-430">Un sitio Web de Windows Azure es el host para una aplicación web que se ejecutan en la nube que puede controlar y administrar.</span><span class="sxs-lookup"><span data-stu-id="c2d30-430">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="c2d30-431">La opción Creación rápida permite implementar una aplicación web completada al Windows Azure sitio Web desde fuera del portal.</span><span class="sxs-lookup"><span data-stu-id="c2d30-431">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="c2d30-432">No se incluyen pasos para configurar una base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-432">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="c2d30-433">![Crear un nuevo sitio Web mediante Creación rápida](aspnet-mvc-4-models-and-data-access/_static/image33.png "crear un nuevo sitio Web mediante Creación rápida")</span><span class="sxs-lookup"><span data-stu-id="c2d30-433">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="c2d30-434">*Crear un nuevo sitio Web mediante Creación rápida*</span><span class="sxs-lookup"><span data-stu-id="c2d30-434">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="c2d30-435">Espere hasta que el nuevo **sitio Web** se crea.</span><span class="sxs-lookup"><span data-stu-id="c2d30-435">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="c2d30-436">Una vez creado el sitio Web, haga clic en el vínculo situado bajo el **URL** columna.</span><span class="sxs-lookup"><span data-stu-id="c2d30-436">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="c2d30-437">Compruebe que funciona el nuevo sitio Web.</span><span class="sxs-lookup"><span data-stu-id="c2d30-437">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="c2d30-438">![En el nuevo sitio web](aspnet-mvc-4-models-and-data-access/_static/image34.png "exploración al sitio web nuevo")</span><span class="sxs-lookup"><span data-stu-id="c2d30-438">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="c2d30-439">*Examinar el sitio web nuevo*</span><span class="sxs-lookup"><span data-stu-id="c2d30-439">*Browsing to the new web site*</span></span>

    <span data-ttu-id="c2d30-440">![Sitio Web que ejecute](aspnet-mvc-4-models-and-data-access/_static/image35.png "sitio Web que se ejecuta")</span><span class="sxs-lookup"><span data-stu-id="c2d30-440">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="c2d30-441">*Sitio Web que se ejecuta*</span><span class="sxs-lookup"><span data-stu-id="c2d30-441">*Web site running*</span></span>
6. <span data-ttu-id="c2d30-442">Vuelva al portal y haga clic en el nombre del sitio web en el **nombre** columna para mostrar las páginas de administración.</span><span class="sxs-lookup"><span data-stu-id="c2d30-442">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="c2d30-443">![Abrir las páginas de administración del sitio web](aspnet-mvc-4-models-and-data-access/_static/image36.png "abrir las páginas de administración del sitio web")</span><span class="sxs-lookup"><span data-stu-id="c2d30-443">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="c2d30-444">*Abrir las páginas de administración del sitio Web*</span><span class="sxs-lookup"><span data-stu-id="c2d30-444">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="c2d30-445">En el **panel** página, en la **vista rápida** sección, haga clic en el **descargar perfil de publicación** vínculo.</span><span class="sxs-lookup"><span data-stu-id="c2d30-445">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c2d30-446">El *perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un sitio Web de Windows Azure para cada método de publicación habilitado.</span><span class="sxs-lookup"><span data-stu-id="c2d30-446">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="c2d30-447">El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse y autenticarse en cada uno de los puntos de conexión para el que está habilitado un método de publicación.</span><span class="sxs-lookup"><span data-stu-id="c2d30-447">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="c2d30-448">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para publicación de aplicaciones web a los sitios Web de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="c2d30-448">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="c2d30-449">![Descargar el sitio web de perfil de publicación](aspnet-mvc-4-models-and-data-access/_static/image37.png "descargar el sitio web de perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="c2d30-449">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="c2d30-450">*Descargar el sitio Web de perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="c2d30-450">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="c2d30-451">Descargue el archivo de perfil de publicación en una ubicación conocida.</span><span class="sxs-lookup"><span data-stu-id="c2d30-451">Download the publish profile file to a known location.</span></span> <span data-ttu-id="c2d30-452">Aún más en este ejercicio verá cómo utilizar este archivo para publicar una aplicación web a un sitios Web de Windows Azure desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c2d30-452">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="c2d30-453">![Guardar el archivo de perfil de publicación](aspnet-mvc-4-models-and-data-access/_static/image38.png "guardar el perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="c2d30-453">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="c2d30-454">*Guardar el archivo de perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="c2d30-454">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="c2d30-455">Tarea 2: configurar el servidor de base de datos</span><span class="sxs-lookup"><span data-stu-id="c2d30-455">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="c2d30-456">Si la aplicación realiza el uso de SQL Server debe crear un servidor de base de datos SQL de bases de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-456">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="c2d30-457">Si desea implementar una aplicación simple que no utiliza SQL Server, podría omitir esta tarea.</span><span class="sxs-lookup"><span data-stu-id="c2d30-457">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="c2d30-458">Necesitará un servidor de base de datos SQL para almacenar la base de datos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="c2d30-458">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="c2d30-459">Puede ver los servidores de base de datos SQL de la suscripción en el portal de administración de Windows Azure en **bases de datos Sql** | **servidores** | **del servidor Panel**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-459">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="c2d30-460">Si no tiene un servidor que creó, puede crear uno mediante la **agregar** botón en la barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-460">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="c2d30-461">Tome nota de la **nombre del servidor y la dirección URL, nombre de inicio de sesión de administrador y contraseña**, tal y como se va a utilizar en las siguientes tareas.</span><span class="sxs-lookup"><span data-stu-id="c2d30-461">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="c2d30-462">No cree la base de datos, tal y como se creará en una fase posterior.</span><span class="sxs-lookup"><span data-stu-id="c2d30-462">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="c2d30-463">![Panel de servidor de base de datos SQL](aspnet-mvc-4-models-and-data-access/_static/image39.png "panel de base de datos SQL Server")</span><span class="sxs-lookup"><span data-stu-id="c2d30-463">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="c2d30-464">*Panel de base de datos SQL Server*</span><span class="sxs-lookup"><span data-stu-id="c2d30-464">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="c2d30-465">En la siguiente tarea probará la conexión de base de datos de Visual Studio, por ese motivo debe incluir la dirección IP local en la lista del servidor de **direcciones IP permitidas**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-465">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="c2d30-466">Para ello, haga clic en **configurar**, seleccione la dirección IP de **dirección IP del cliente actual** y péguela en el **dirección IP inicial** y **ladirecciónIPfinal** cuadros de texto y haga clic en el ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) botón.</span><span class="sxs-lookup"><span data-stu-id="c2d30-466">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![Agregar dirección IP del cliente](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="c2d30-468">*Agregar dirección IP del cliente*</span><span class="sxs-lookup"><span data-stu-id="c2d30-468">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="c2d30-469">Una vez el **dirección IP del cliente** se agrega a las direcciones IP permitidas lista, haga clic en **guardar** para confirmar los cambios.</span><span class="sxs-lookup"><span data-stu-id="c2d30-469">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmar cambios](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="c2d30-471">*Confirmar cambios*</span><span class="sxs-lookup"><span data-stu-id="c2d30-471">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="c2d30-472">Tarea 3: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy</span><span class="sxs-lookup"><span data-stu-id="c2d30-472">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="c2d30-473">Vuelva a la solución de ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="c2d30-473">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="c2d30-474">En el **el Explorador de soluciones**, haga clic en el proyecto de sitio web y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-474">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="c2d30-475">![Publicar la aplicación](aspnet-mvc-4-models-and-data-access/_static/image43.png "publicar la aplicación")</span><span class="sxs-lookup"><span data-stu-id="c2d30-475">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="c2d30-476">*Publicar el sitio web*</span><span class="sxs-lookup"><span data-stu-id="c2d30-476">*Publishing the web site*</span></span>
2. <span data-ttu-id="c2d30-477">Importar el perfil de publicación que se ha guardado en la primera tarea.</span><span class="sxs-lookup"><span data-stu-id="c2d30-477">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="c2d30-478">![Importar el perfil de publicación](aspnet-mvc-4-models-and-data-access/_static/image44.png "importar el perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="c2d30-478">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="c2d30-479">*Importar perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="c2d30-479">*Importing publish profile*</span></span>
3. <span data-ttu-id="c2d30-480">Haga clic en **validar conexión**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-480">Click **Validate Connection**.</span></span> <span data-ttu-id="c2d30-481">Una vez completada la validación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-481">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c2d30-482">Validación está completa cuando aparece una marca de verificación verde junto al botón Validar conexión.</span><span class="sxs-lookup"><span data-stu-id="c2d30-482">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="c2d30-483">![Validación de la conexión](aspnet-mvc-4-models-and-data-access/_static/image45.png "validación de la conexión")</span><span class="sxs-lookup"><span data-stu-id="c2d30-483">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="c2d30-484">*Validación de la conexión*</span><span class="sxs-lookup"><span data-stu-id="c2d30-484">*Validating connection*</span></span>
4. <span data-ttu-id="c2d30-485">En el **configuración** página, en la **bases de datos** sección, haga clic en el botón situado junto al cuadro de texto de la conexión de la base de datos (es decir, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="c2d30-485">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="c2d30-486">![Configuración de Web deploy](aspnet-mvc-4-models-and-data-access/_static/image46.png "configuración de Web deploy")</span><span class="sxs-lookup"><span data-stu-id="c2d30-486">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="c2d30-487">*Configuración de Web deploy*</span><span class="sxs-lookup"><span data-stu-id="c2d30-487">*Web deploy configuration*</span></span>
5. <span data-ttu-id="c2d30-488">Configure la conexión de base de datos de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="c2d30-488">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="c2d30-489">En el **nombre del servidor** escriba la dirección URL de base de datos de SQL server mediante la *tcp:* prefijo.</span><span class="sxs-lookup"><span data-stu-id="c2d30-489">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="c2d30-490">En **nombre de usuario** escriba el nombre de inicio de sesión del Administrador de servidor.</span><span class="sxs-lookup"><span data-stu-id="c2d30-490">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="c2d30-491">En **contraseña** escriba la contraseña de inicio de sesión de administrador de servidor.</span><span class="sxs-lookup"><span data-stu-id="c2d30-491">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="c2d30-492">Escriba un nuevo nombre de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c2d30-492">Type a new database name.</span></span>

    <span data-ttu-id="c2d30-493">![Configurar la cadena de conexión de destino](aspnet-mvc-4-models-and-data-access/_static/image47.png "configurar la cadena de conexión de destino")</span><span class="sxs-lookup"><span data-stu-id="c2d30-493">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="c2d30-494">*Configurar la cadena de conexión de destino*</span><span class="sxs-lookup"><span data-stu-id="c2d30-494">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="c2d30-495">A continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-495">Then click **OK**.</span></span> <span data-ttu-id="c2d30-496">Cuando se le solicite para crear la base de datos, haga clic en **Sí**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-496">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="c2d30-497">![Crear la base de datos](aspnet-mvc-4-models-and-data-access/_static/image48.png "crear la cadena de la base de datos")</span><span class="sxs-lookup"><span data-stu-id="c2d30-497">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="c2d30-498">*Crear la base de datos*</span><span class="sxs-lookup"><span data-stu-id="c2d30-498">*Creating the database*</span></span>
7. <span data-ttu-id="c2d30-499">La cadena de conexión que se va a usar para conectarse a la base de datos de SQL en Windows Azure se muestra en el cuadro de texto de conexión predeterminado.</span><span class="sxs-lookup"><span data-stu-id="c2d30-499">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="c2d30-500">Después, haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-500">Then click **Next**.</span></span>

    <span data-ttu-id="c2d30-501">![Cadena de conexión que apunte a la base de datos SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "cadena de conexión que apunte a la base de datos SQL")</span><span class="sxs-lookup"><span data-stu-id="c2d30-501">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="c2d30-502">*Cadena de conexión que apunte a la base de datos SQL*</span><span class="sxs-lookup"><span data-stu-id="c2d30-502">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="c2d30-503">En el **vista previa** página, haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-503">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="c2d30-504">![Publicar la aplicación web](aspnet-mvc-4-models-and-data-access/_static/image50.png "publicar la aplicación web")</span><span class="sxs-lookup"><span data-stu-id="c2d30-504">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="c2d30-505">*Publicar la aplicación web*</span><span class="sxs-lookup"><span data-stu-id="c2d30-505">*Publishing the web application*</span></span>
9. <span data-ttu-id="c2d30-506">Una vez que finalice el proceso de publicación, el explorador predeterminado abrirá el sitio web publicado.</span><span class="sxs-lookup"><span data-stu-id="c2d30-506">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="c2d30-507">Apéndice C: usar fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="c2d30-507">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="c2d30-508">Con fragmentos de código, tiene todo el código que necesita a su alcance.</span><span class="sxs-lookup"><span data-stu-id="c2d30-508">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="c2d30-509">El documento de laboratorio le indicará exactamente cuándo utilizarlas, tal como se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="c2d30-509">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="c2d30-510">![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-models-and-data-access/_static/image51.png "fragmentos de código en Visual Studio para insertar código en el proyecto")</span><span class="sxs-lookup"><span data-stu-id="c2d30-510">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="c2d30-511">*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*</span><span class="sxs-lookup"><span data-stu-id="c2d30-511">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="c2d30-512">***Para agregar un fragmento de código mediante el teclado (solo C#)***</span><span class="sxs-lookup"><span data-stu-id="c2d30-512">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="c2d30-513">Coloque el cursor donde desea insertar el código.</span><span class="sxs-lookup"><span data-stu-id="c2d30-513">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="c2d30-514">Comience a escribir el nombre del fragmento (sin espacios ni guiones).</span><span class="sxs-lookup"><span data-stu-id="c2d30-514">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="c2d30-515">Observe como coincidencia de nombres de fragmentos de código de muestra de IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="c2d30-515">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="c2d30-516">Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).</span><span class="sxs-lookup"><span data-stu-id="c2d30-516">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="c2d30-517">Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.</span><span class="sxs-lookup"><span data-stu-id="c2d30-517">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="c2d30-518">![Comience a escribir el nombre del fragmento](aspnet-mvc-4-models-and-data-access/_static/image52.png "comience a escribir el nombre del fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="c2d30-518">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="c2d30-519">*Comience a escribir el nombre del fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="c2d30-519">*Start typing the snippet name*</span></span>

<span data-ttu-id="c2d30-520">![Presione la tecla Tab para seleccionar el fragmento de código resaltada](aspnet-mvc-4-models-and-data-access/_static/image53.png "presione Tab para seleccionar el fragmento de código resaltada")</span><span class="sxs-lookup"><span data-stu-id="c2d30-520">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="c2d30-521">*Presione la tecla Tab para seleccionar el fragmento de código resaltada*</span><span class="sxs-lookup"><span data-stu-id="c2d30-521">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="c2d30-522">![Vuelva a presionar Tab y el fragmento de código se expandirán](aspnet-mvc-4-models-and-data-access/_static/image54.png "vuelva a presionar Tab y el fragmento de código se expandirán")</span><span class="sxs-lookup"><span data-stu-id="c2d30-522">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="c2d30-523">*Vuelva a presionar Tab y el fragmento de código se expandirán*</span><span class="sxs-lookup"><span data-stu-id="c2d30-523">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="c2d30-524">***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="c2d30-524">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="c2d30-525">Haga clic en donde desea insertar el fragmento de código.</span><span class="sxs-lookup"><span data-stu-id="c2d30-525">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="c2d30-526">Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.</span><span class="sxs-lookup"><span data-stu-id="c2d30-526">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="c2d30-527">Seleccione el fragmento de código relevante de la lista haciendo clic en él.</span><span class="sxs-lookup"><span data-stu-id="c2d30-527">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="c2d30-528">![Menú contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](aspnet-mvc-4-models-and-data-access/_static/image55.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="c2d30-528">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="c2d30-529">*Haga clic en donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="c2d30-529">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="c2d30-530">![Seleccione el fragmento de código relevante de la lista haciendo clic en él](aspnet-mvc-4-models-and-data-access/_static/image56.png "elegir el fragmento de código relevante de la lista haciendo clic en él")</span><span class="sxs-lookup"><span data-stu-id="c2d30-530">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="c2d30-531">*Seleccione el fragmento de código relevante de la lista haciendo clic en él*</span><span class="sxs-lookup"><span data-stu-id="c2d30-531">*Pick the relevant snippet from the list, by clicking on it*</span></span>
