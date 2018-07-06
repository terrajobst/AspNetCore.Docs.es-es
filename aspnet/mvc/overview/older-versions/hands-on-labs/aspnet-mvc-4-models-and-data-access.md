---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: Acceso a datos y modelos de ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: 'Nota: En este laboratorio práctico se supone que tiene conocimientos básicos de ASP.NET MVC. Si no ha usado ASP.NET MVC antes, se recomienda ir a través de ASP.NET MVC 4...'
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: bbb726f9f4abb2f9c72153a8c7986c5c83a9a840
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818377"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="c3f4e-104">Acceso a datos y modelos de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="c3f4e-104">ASP.NET MVC 4 Models and Data Access</span></span>

<span data-ttu-id="c3f4e-105">por [campamentos Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="c3f4e-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="c3f4e-106">Descargue el Kit de aprendizaje de campamentos de Web</span><span class="sxs-lookup"><span data-stu-id="c3f4e-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="c3f4e-107">Esta práctica se da por supuesto que tiene conocimientos básicos de **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="c3f4e-108">Si no ha usado **ASP.NET MVC** antes, le recomendamos que repase **aspectos básicos de ASP.NET MVC 4** laboratorios prácticos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-108">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="c3f4e-109">Esta práctica le guiará a través de las mejoras y nuevas características descritas anteriormente aplicando cambios menores a una aplicación Web de ejemplo proporcionada en la carpeta de origen.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-109">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="c3f4e-110">Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de entrenamiento campamentos de Web, que está disponible en [versiones de Microsoft de Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="c3f4e-110">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="c3f4e-111">Está disponible en el proyecto específico para este laboratorio [acceso a datos y modelos de ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span><span class="sxs-lookup"><span data-stu-id="c3f4e-111">The project specific to this lab is available at [ASP.NET MVC 4 Models and Data Access](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span></span>

<span data-ttu-id="c3f4e-112">En **aspectos básicos de ASP.NET MVC** laboratorios prácticos, que ha ha pasando datos codificados de forma rígida desde los controladores para las plantillas de vista.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-112">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="c3f4e-113">Sin embargo, para crear una aplicación Web real, es posible que desea usar una base de datos real.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-113">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="c3f4e-114">Esta práctica le mostrará cómo usar un motor de base de datos con el fin de almacenar y recuperar los datos necesarios para la aplicación Music Store.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-114">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="c3f4e-115">Para ello, se iniciará con una base de datos existente y crear el Entity Data Model a partir de él.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-115">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="c3f4e-116">A lo largo de este laboratorio, cumplirá la **Database First** enfoque así como el **Code First** enfoque.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-116">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="c3f4e-117">Sin embargo, también puede usar el **Model First** enfocar, crear el mismo modelo de uso de las herramientas y, a continuación, generar la base de datos de él.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-117">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="c3f4e-118">![Base de datos primer lugar. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "vs Database First. En primer lugar del modelo")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-118">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="c3f4e-119">*Base de datos primer lugar. En primer lugar del modelo*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-119">*Database First vs. Model First*</span></span>

<span data-ttu-id="c3f4e-120">Después de generar el modelo, hará que los ajustes adecuados en el StoreController para proporcionar las vistas de Store con los datos procedentes de la base de datos, en lugar de usar datos codificados de forma rígida.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-120">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="c3f4e-121">No necesitará realizar ningún cambio en las plantillas de vista porque el StoreController van a devolver los mismos ViewModel a las plantillas de vista, aunque esta vez los datos procederán de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-121">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="c3f4e-122">**El enfoque de Code First**</span><span class="sxs-lookup"><span data-stu-id="c3f4e-122">**The Code First Approach**</span></span>

<span data-ttu-id="c3f4e-123">El enfoque Code First nos permite definir el modelo desde el código sin generar clases que normalmente están acopladas con el marco de trabajo.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-123">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="c3f4e-124">En el código en primer lugar, se definen los objetos de modelo con poco, &quot;objetos CLR&quot;.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-124">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="c3f4e-125">Poco es simples clases sin formato que no tienen se hereda y no implementan interfaces.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-125">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="c3f4e-126">Podemos generar automáticamente la base de datos de ellos, o podemos usar una base de datos existente y generar la asignación de clase desde el código.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-126">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="c3f4e-127">Las ventajas de usar este enfoque es que el modelo sigue siendo independiente entre el marco de persistencia (en este caso, Entity Framework), como las clases poco no están vinculadas con el marco de asignación.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-127">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="c3f4e-128">Este laboratorio se basa en ASP.NET MVC 4 y una versión de la aplicación de ejemplo de Music Store personalizada y minimiza para ajustarse a solo las características mostradas en este laboratorio práctico.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-128">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="c3f4e-129">Si desea explorar todo el **Music Store** aplicación del tutorial puede encontrarlo en [MVC-música-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="c3f4e-129">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="c3f4e-130">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="c3f4e-130">Prerequisites</span></span>

<span data-ttu-id="c3f4e-131">Debe tener los elementos siguientes para completar esta práctica:</span><span class="sxs-lookup"><span data-stu-id="c3f4e-131">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="c3f4e-132">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (leer [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).</span><span class="sxs-lookup"><span data-stu-id="c3f4e-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="c3f4e-133">Programa de instalación</span><span class="sxs-lookup"><span data-stu-id="c3f4e-133">Setup</span></span>

<span data-ttu-id="c3f4e-134">**Instalación de fragmentos de código**</span><span class="sxs-lookup"><span data-stu-id="c3f4e-134">**Installing Code Snippets**</span></span>

<span data-ttu-id="c3f4e-135">Para mayor comodidad, gran parte del código que se va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-135">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="c3f4e-136">Para instalar los fragmentos de código que se ejecute **.\Source\Setup\CodeSnippets.vsi** archivo.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-136">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="c3f4e-137">Si no está familiarizado con los fragmentos de código de Visual Studio y desea aprender a usarlas, puede consultar el apéndice de este documento &quot; [Apéndice C: Using Code Snippets](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="c3f4e-138">Ejercicios</span><span class="sxs-lookup"><span data-stu-id="c3f4e-138">Exercises</span></span>

<span data-ttu-id="c3f4e-139">Esta práctica se compone de los ejercicios siguientes:</span><span class="sxs-lookup"><span data-stu-id="c3f4e-139">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="c3f4e-140">Ejercicio 1: Agregar una base de datos</span><span class="sxs-lookup"><span data-stu-id="c3f4e-140">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="c3f4e-141">Ejercicio 2: Creación de una base de datos mediante Code First</span><span class="sxs-lookup"><span data-stu-id="c3f4e-141">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="c3f4e-142">Ejercicio 3: Consultar la base de datos con parámetros</span><span class="sxs-lookup"><span data-stu-id="c3f4e-142">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="c3f4e-143">Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debe obtener después de completar los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="c3f4e-144">Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="c3f4e-145">Tiempo estimado para completar esta práctica: **35 minutos**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-145">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="c3f4e-146">Ejercicio 1: Agregar una base de datos</span><span class="sxs-lookup"><span data-stu-id="c3f4e-146">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="c3f4e-147">En este ejercicio, obtendrá información sobre cómo agregar una base de datos con las tablas de la aplicación Music Store tal a la solución para consumir los datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-147">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="c3f4e-148">Una vez que la base de datos se genera con el modelo y agrega a la solución, modificará la clase StoreController para proporcionar la plantilla de vista con los datos procedentes de la base de datos, en lugar de usar valores codificados de forma rígida.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-148">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="c3f4e-149">Tarea 1: agregar una base de datos</span><span class="sxs-lookup"><span data-stu-id="c3f4e-149">Task 1 - Adding a Database</span></span>

<span data-ttu-id="c3f4e-150">En esta tarea, agregará una base de datos ya se ha creado con las tablas principales de la aplicación Music Store tal a la solución.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-150">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="c3f4e-151">Abra el **comenzar** solución ubicado en **origen/Ex1-AddingADatabaseDBFirst/inicio/** carpeta.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-151">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

   1. <span data-ttu-id="c3f4e-152">Deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c3f4e-153">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="c3f4e-154">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="c3f4e-155">Por último, compile la solución haciendo **compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="c3f4e-156">Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c3f4e-157">Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c3f4e-158">Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="c3f4e-159">Agregar **MvcMusicStore** el archivo de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-159">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="c3f4e-160">En este laboratorio práctico, usará una base de datos ya creada denominada **MvcMusicStore.mdf**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-160">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="c3f4e-161">Para ello, haga clic en **aplicación\_datos** carpeta, seleccione **agregar** y, a continuación, haga clic en **elemento existente**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-161">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="c3f4e-162">Vaya a **\Source\Assets** y seleccione el **MvcMusicStore.mdf** archivo.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-162">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="c3f4e-163">![Agregar un elemento existente](aspnet-mvc-4-models-and-data-access/_static/image2.png "agregando un elemento existente")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-163">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="c3f4e-164">*Agregar un elemento existente*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-164">*Adding an Existing Item*</span></span>

    <span data-ttu-id="c3f4e-165">![Archivo de base de datos MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf archivo de base de datos")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-165">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="c3f4e-166">*Archivo de base de datos MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-166">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="c3f4e-167">La base de datos se agregó al proyecto.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-167">The database has been added to the project.</span></span> <span data-ttu-id="c3f4e-168">Incluso cuando la base de datos se encuentra dentro de la solución, puede consultar y actualizarla a medida que se hospeda en un servidor de base de datos diferente.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-168">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="c3f4e-169">![MvcMusicStore base de datos en el Explorador de soluciones](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore base de datos en el Explorador de soluciones")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-169">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="c3f4e-170">*MvcMusicStore base de datos en el Explorador de soluciones*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-170">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="c3f4e-171">Compruebe la conexión a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-171">Verify the connection to the database.</span></span> <span data-ttu-id="c3f4e-172">Para ello, haga doble clic en **MvcMusicStore.mdf** para establecer una conexión.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-172">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="c3f4e-173">![Conectarse a MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "conectarse a MvcMusicStore.mdf")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-173">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="c3f4e-174">*Conectarse a MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-174">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="c3f4e-175">Tarea 2: crear un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="c3f4e-175">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="c3f4e-176">En esta tarea, creará un modelo de datos para interactuar con la base de datos agregado en la tarea anterior.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-176">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="c3f4e-177">Crear un modelo de datos que representa la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-177">Create a data model that will represent the database.</span></span> <span data-ttu-id="c3f4e-178">Para ello, en el botón secundario del explorador de soluciones el **modelos** carpeta, seleccione **agregar** y, a continuación, haga clic en **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-178">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="c3f4e-179">En el **Agregar nuevo elemento** cuadro de diálogo, seleccione el **datos** plantilla y, a continuación, el **ADO.NET Entity Data Model** elemento.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-179">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="c3f4e-180">Cambie el nombre del modelo de datos a **StoreDB.edmx** y haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-180">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="c3f4e-181">![Agregar StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "agregar StoreDB ADO.NET Entity Data Model")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-181">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="c3f4e-182">*Agregar StoreDB ADO.NET Entity Data Model*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-182">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="c3f4e-183">El **Asistente para Entity Data Model** aparecerá.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-183">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="c3f4e-184">Este asistente le guiará a través de la creación de la capa del modelo.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-184">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="c3f4e-185">Puesto que el modelo debe crearse en función de la existente recentyl de base de datos agregado, seleccione **generar desde la base de datos** y haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-185">Since the model should be created based on the existing database recentyl added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="c3f4e-186">![Elegir el contenido del modelo](aspnet-mvc-4-models-and-data-access/_static/image7.png "elegir el contenido del modelo")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-186">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="c3f4e-187">*Elegir el contenido del modelo*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-187">*Choosing the model content*</span></span>
3. <span data-ttu-id="c3f4e-188">Puesto que está generando un modelo desde una base de datos, deberá especificar la conexión debe usar.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-188">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="c3f4e-189">Haga clic en **nueva conexión**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-189">Click **New Connection**.</span></span>
4. <span data-ttu-id="c3f4e-190">Seleccione **el archivo de base de datos de Microsoft SQL Server** y haga clic en **continuar**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-190">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="c3f4e-191">![Elegir origen de datos](aspnet-mvc-4-models-and-data-access/_static/image8.png "Elegir origen de datos")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-191">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="c3f4e-192">*Elija el cuadro de diálogo de origen de datos*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-192">*Choose data source dialog*</span></span>
5. <span data-ttu-id="c3f4e-193">Haga clic en **examinar** y seleccione la base de datos **MvcMusicStore.mdf** encuentra en la **aplicación\_datos** carpeta y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-193">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="c3f4e-194">![Propiedades de conexión](aspnet-mvc-4-models-and-data-access/_static/image9.png "las propiedades de conexión")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-194">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="c3f4e-195">*Propiedades de conexión*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-195">*Connection properties*</span></span>
6. <span data-ttu-id="c3f4e-196">Debe tener el mismo nombre que la cadena de conexión de entidad, cambie su nombre a la clase generada **MusicStoreEntities** y haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-196">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="c3f4e-197">![Elegir la conexión de datos](aspnet-mvc-4-models-and-data-access/_static/image10.png "elegir la conexión de datos")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-197">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="c3f4e-198">*Elegir la conexión de datos*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-198">*Choosing the data connection*</span></span>
7. <span data-ttu-id="c3f4e-199">Elija los objetos de base de datos que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-199">Choose the database objects to use.</span></span> <span data-ttu-id="c3f4e-200">Como el modelo de entidad utilizarán solo las tablas de la base de datos, seleccione el **tablas** opción y asegúrese de que el **incluir columnas de clave externa en el modelo** y **poner en plural o en singular generado nombres de objeto** opciones también están seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-200">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="c3f4e-201">Cambiar el Namespace de modelo para **MvcMusicStore.Model** y haga clic en **finalizar**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-201">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="c3f4e-202">![Elegir los objetos de base de datos](aspnet-mvc-4-models-and-data-access/_static/image11.png "elegir los objetos de base de datos")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-202">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="c3f4e-203">*Elegir los objetos de base de datos*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-203">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c3f4e-204">Si se muestra un cuadro de diálogo de advertencia de seguridad, haga clic en **Aceptar** para ejecutar la plantilla y generar las clases para las entidades del modelo.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-204">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="c3f4e-205">Un diagrama de entidades para la base de datos aparecerá, mientras que se creará una clase independiente que se asigna cada tabla a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-205">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="c3f4e-206">Por ejemplo, el **álbumes** tabla se representa mediante un **álbum** (clase), donde cada columna de la tabla se asignará a una propiedad de clase.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-206">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="c3f4e-207">Esto le permitirá consultar y trabajar con objetos que representan las filas de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-207">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="c3f4e-208">![Diagrama de la entidad](aspnet-mvc-4-models-and-data-access/_static/image12.png "diagrama de entidades")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-208">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="c3f4e-209">*Diagrama de la entidad*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-209">*Entity diagram*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c3f4e-210">Las plantillas T4 (.tt) ejecutan código para generar las clases de entidades y las clases existentes, sobrescribirán con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-210">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="c3f4e-211">En este ejemplo, las clases &quot;álbum&quot;, &quot;género&quot; y &quot;artista&quot; se sobrescribieron con el código generado.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-211">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="c3f4e-212">Tarea 3: creación de la aplicación</span><span class="sxs-lookup"><span data-stu-id="c3f4e-212">Task 3 - Building the Application</span></span>

<span data-ttu-id="c3f4e-213">En esta tarea, comprobará que, aunque la generación del modelo se ha quitado el **álbum**, **género** y **artista** clases de modelo, el proyecto se compila correctamente mediante el uso de las nuevas clases de modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-213">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="c3f4e-214">Compile el proyecto seleccionando el **compilar** elemento de menú y, a continuación, **MvcMusicStore compilar**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-214">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="c3f4e-215">![Compilar el proyecto](aspnet-mvc-4-models-and-data-access/_static/image13.png "compilar el proyecto")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-215">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="c3f4e-216">*Compilar el proyecto*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-216">*Building the project*</span></span>
2. <span data-ttu-id="c3f4e-217">El proyecto se compila correctamente.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-217">The project builds successfully.</span></span> <span data-ttu-id="c3f4e-218">¿Por qué todavía funciona?</span><span class="sxs-lookup"><span data-stu-id="c3f4e-218">Why does it still work?</span></span> <span data-ttu-id="c3f4e-219">Funciona porque las tablas de base de datos tienen campos que se incluyen las propiedades que se usaba en las clases quitadas **álbum** y **género**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-219">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="c3f4e-220">![Las compilaciones que se ha realizado correctamente](aspnet-mvc-4-models-and-data-access/_static/image14.png "las compilaciones que se realizó correctamente")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-220">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="c3f4e-221">*Compilaciones correctas*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-221">*Builds succeeded*</span></span>
3. <span data-ttu-id="c3f4e-222">Aunque el diseñador muestra las entidades en un formato de diagrama, son realmente las clases de C#.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-222">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="c3f4e-223">Expanda el **StoreDB.edmx** nodo en el Explorador de soluciones y, a continuación, **StoreDB.tt**, verá las nuevas entidades generadas.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-223">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="c3f4e-224">![Los archivos generados](aspnet-mvc-4-models-and-data-access/_static/image15.png "los archivos generados")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-224">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="c3f4e-225">*Archivos generados*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-225">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="c3f4e-226">Tarea 4: consultar la base de datos</span><span class="sxs-lookup"><span data-stu-id="c3f4e-226">Task 4 - Querying the Database</span></span>

<span data-ttu-id="c3f4e-227">En esta tarea, actualizará la clase StoreController que, en lugar de los datos codificados de forma rígida, consultará la base de datos para recuperar la información.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-227">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="c3f4e-228">Abra **Controllers\StoreController.cs** y agregue el siguiente campo a la clase para contener una instancia de la **MusicStoreEntities** clase, denominada **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="c3f4e-228">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="c3f4e-229">(Código de fragmento de código - *modelos y acceso a datos - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="c3f4e-229">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. <span data-ttu-id="c3f4e-230">El **MusicStoreEntities** clase expone una propiedad de colección para cada tabla en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-230">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="c3f4e-231">Actualización **examinar** método de acción para recuperar un género con todos los **álbumes**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-231">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="c3f4e-232">(Código de fragmento de código - *modelos y Data Access: Ex1 Store examinar*)</span><span class="sxs-lookup"><span data-stu-id="c3f4e-232">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > <span data-ttu-id="c3f4e-233">Utiliza una funcionalidad de .NET llamado **LINQ** (language integrated query) para escribir expresiones de consulta fuertemente tipada en estas colecciones - que ejecutará el código en la base de datos y devolver los objetos que se pueden programar con.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-233">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="c3f4e-234">Para obtener más información acerca de LINQ, visite la [sitio msdn](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="c3f4e-234">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span></span>
3. <span data-ttu-id="c3f4e-235">Actualización **índice** método de acción para recuperar todos los géneros.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-235">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="c3f4e-236">(Código de fragmento de código - *modelos y Data Access: Ex1 Store índice*)</span><span class="sxs-lookup"><span data-stu-id="c3f4e-236">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. <span data-ttu-id="c3f4e-237">Actualización **índice** método de acción para recuperar todos los géneros y transformar la colección a una lista.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-237">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="c3f4e-238">(Código de fragmento de código - *modelos y Data Access: Ex1 Store GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="c3f4e-238">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="c3f4e-239">Tarea 5: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="c3f4e-239">Task 5 - Running the Application</span></span>

<span data-ttu-id="c3f4e-240">En esta tarea, comprobará que la página de índice Store ahora muestra los géneros que se almacenan en la base de datos en lugar del codificado de forma rígida las.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-240">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="c3f4e-241">No es necesario cambiar la plantilla de vista porque la **StoreController** devuelve las mismas entidades como antes, aunque esta vez los datos procederán de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-241">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="c3f4e-242">Volver a generar la solución y presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-242">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="c3f4e-243">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-243">The project starts in the Home page.</span></span> <span data-ttu-id="c3f4e-244">Compruebe que el menú de **géneros** ya no es una lista de codificado de forma rígida, y los datos se recuperan directamente de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-244">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="c3f4e-246">*Exploración de géneros de la base de datos*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-246">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="c3f4e-247">Ahora, vaya a cualquier género y comprobar que los álbumes se rellenan a partir de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-247">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="c3f4e-248">![Exploración álbumes desde la base de datos](aspnet-mvc-4-models-and-data-access/_static/image17.png "álbumes de exploración de la base de datos")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-248">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="c3f4e-249">*Álbumes de exploración de la base de datos*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-249">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="c3f4e-250">Ejercicio 2: Creación de una base de datos usando código</span><span class="sxs-lookup"><span data-stu-id="c3f4e-250">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="c3f4e-251">En este ejercicio, obtendrá información sobre cómo usar el enfoque Code First para crear una base de datos con las tablas de la aplicación Music Store tal y cómo tener acceso a sus datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-251">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="c3f4e-252">Una vez que se genera el modelo, modificará el StoreController para proporcionar la plantilla de vista con los datos procedentes de la base de datos, en lugar de usar valores codificados.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-252">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="c3f4e-253">Si se ha completado el ejercicio 1 y ya ha trabajado con el enfoque Database First, ahora obtendrá información sobre cómo obtener los mismos resultados con un proceso diferente.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-253">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="c3f4e-254">Las tareas que están en común con el ejercicio 1 se han marcado para facilitar su lectura.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-254">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="c3f4e-255">Si no se ha completado el ejercicio 1, pero quiere obtener información sobre el enfoque Code First, puede iniciar desde este ejercicio y obtener una cobertura completa del tema.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-255">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="c3f4e-256">Tarea 1: rellenar datos de ejemplo</span><span class="sxs-lookup"><span data-stu-id="c3f4e-256">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="c3f4e-257">En esta tarea, se rellenará la base de datos con datos de ejemplo cuando se crea desde el principio mediante Code First.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-257">In this task, you will populate the database with sample data when it is intially created using Code-First.</span></span>

1. <span data-ttu-id="c3f4e-258">Abra el **comenzar** solución ubicado en **origen/Ex2-CreatingADatabaseCodeFirst/inicio/** carpeta.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-258">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="c3f4e-259">En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-259">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="c3f4e-260">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-260">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c3f4e-261">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-261">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="c3f4e-262">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-262">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="c3f4e-263">Por último, compile la solución haciendo **compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-263">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="c3f4e-264">Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-264">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c3f4e-265">Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-265">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c3f4e-266">Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-266">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="c3f4e-267">Agregar el **SampleData.cs** del archivo a la **modelos** carpeta.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-267">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="c3f4e-268">Para ello, haga clic en **modelos** carpeta, seleccione **agregar** y, a continuación, haga clic en **elemento existente**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-268">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="c3f4e-269">Vaya a **\Source\Assets** y seleccione el **SampleData.cs** archivo.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-269">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="c3f4e-270">![Datos de ejemplo rellenan código](aspnet-mvc-4-models-and-data-access/_static/image18.png "código de rellenar de datos de ejemplo")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-270">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="c3f4e-271">*Código de rellenar de datos de ejemplo*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-271">*Sample data populate code*</span></span>
3. <span data-ttu-id="c3f4e-272">Abra el **Global.asax.cs** de archivo y agregue la siguiente *mediante* instrucciones.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-272">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="c3f4e-273">(Código de fragmento de código - *modelos y Data Access: Ex2 Global Asax instrucciones using*)</span><span class="sxs-lookup"><span data-stu-id="c3f4e-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. <span data-ttu-id="c3f4e-274">En el **aplicación\_Start()** método agregue la siguiente línea para establecer el inicializador de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-274">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="c3f4e-275">(Código de fragmento de código - *modelos y Data Access: Ex2 Global Asax SetInitializer*)</span><span class="sxs-lookup"><span data-stu-id="c3f4e-275">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="c3f4e-276">Tarea 2: configurar la conexión a la base de datos</span><span class="sxs-lookup"><span data-stu-id="c3f4e-276">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="c3f4e-277">Ahora que ya ha agregado una base de datos a nuestro proyecto, escribirá el **Web.config** la cadena de conexión de archivos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-277">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="c3f4e-278">Agregar una cadena de conexión en **Web.config**. Para ello, abra **Web.config** en la raíz del proyecto y reemplace la cadena de conexión denominado DefaultConnection con esta línea en el **&lt;connectionStrings&gt;** sección:</span><span class="sxs-lookup"><span data-stu-id="c3f4e-278">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="c3f4e-279">![Ubicación del archivo Web.config](aspnet-mvc-4-models-and-data-access/_static/image19.png "ubicación del archivo Web.config")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-279">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="c3f4e-280">*Ubicación del archivo Web.config*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-280">*Web.config file location*</span></span>

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="c3f4e-281">Tarea 3: trabajar con el modelo</span><span class="sxs-lookup"><span data-stu-id="c3f4e-281">Task 3 - Working with the Model</span></span>

<span data-ttu-id="c3f4e-282">Ahora que ya ha configurado la conexión a la base de datos, se vinculará el modelo con las tablas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-282">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="c3f4e-283">En esta tarea, creará una clase que se vinculará a la base de datos con Code First.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-283">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="c3f4e-284">Recuerde que hay una clase de modelo POCO existente que se debe modificar.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-284">Remember that there is an existent POCO model class that should be modified.</span></span>

> [!NOTE]
> <span data-ttu-id="c3f4e-285">Si completó el ejercicio 1, observará que este paso se realiza mediante un asistente.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-285">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="c3f4e-286">Haciendo Code First, creará manualmente las clases que se vinculará a entidades de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-286">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>

1. <span data-ttu-id="c3f4e-287">Abra la clase de modelo POCO **género** desde **modelos** carpeta del proyecto e incluir un identificador.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-287">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="c3f4e-288">Utilice una propiedad int con el nombre **GenreId**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-288">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="c3f4e-289">(Código de fragmento de código - *modelos y Data Access: Ex2 código primera género*)</span><span class="sxs-lookup"><span data-stu-id="c3f4e-289">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > <span data-ttu-id="c3f4e-290">Para trabajar con las convenciones de Code First, el género de la clase debe tener una propiedad de clave principal que se detectará automáticamente.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-290">To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.</span></span>
    > 
    > <span data-ttu-id="c3f4e-291">Puede leer más sobre las convenciones de Code First en este [artículo de msdn](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="c3f4e-291">You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span></span>
2. <span data-ttu-id="c3f4e-292">Ahora, abra la clase de modelo POCO **álbum** desde **modelos** carpeta del proyecto y se incluye las claves externas, cree propiedades con los nombres **GenreId** y  **ArtistId**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-292">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="c3f4e-293">Esta clase ya tiene la **GenreId** para la clave principal.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-293">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="c3f4e-294">(Código de fragmento de código - *modelos y Data Access: Ex2 código primer álbum*)</span><span class="sxs-lookup"><span data-stu-id="c3f4e-294">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. <span data-ttu-id="c3f4e-295">Abra la clase de modelo POCO **artista** e incluyen el **ArtistId** propiedad.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-295">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="c3f4e-296">(Código de fragmento de código - *modelos y Data Access: Ex2 código primer intérprete*)</span><span class="sxs-lookup"><span data-stu-id="c3f4e-296">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. <span data-ttu-id="c3f4e-297">Haga clic en el **modelos** carpeta del proyecto y seleccione **Add | Clase**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-297">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="c3f4e-298">Nombre del archivo **MusicStoreEntities.cs**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-298">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="c3f4e-299">A continuación, haga clic en **agregar.**</span><span class="sxs-lookup"><span data-stu-id="c3f4e-299">Then, click **Add.**</span></span>

    <span data-ttu-id="c3f4e-300">![Agregar una clase](aspnet-mvc-4-models-and-data-access/_static/image20.png "agregar una clase")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-300">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="c3f4e-301">*Agregar un nuevo elemento*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-301">*Adding a new item*</span></span>

    <span data-ttu-id="c3f4e-302">![Agregar un class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "agregar class2")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-302">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="c3f4e-303">*Agregar una clase*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-303">*Adding a class*</span></span>
5. <span data-ttu-id="c3f4e-304">Abra la clase que acaba de crear, **MusicStoreEntities.cs**e incluyen los espacios de nombres **System.Data.Entity** y **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-304">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. <span data-ttu-id="c3f4e-305">Reemplace la declaración de clase para extender la **DbContext** clase: declarar public **DBSet** e invalidar **OnModelCreating** método.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-305">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="c3f4e-306">Después de este paso, obtendrá una clase de dominio que se vinculará el modelo con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-306">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="c3f4e-307">Para ello, reemplace el código de clase por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c3f4e-307">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="c3f4e-308">(Código de fragmento de código - *modelos y Data Access: Ex2 código primera MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="c3f4e-308">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="c3f4e-309">Con Entity Framework **DbContext** y **DBSet** podrá consultar la clase POCO género.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-309">With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre.</span></span> <span data-ttu-id="c3f4e-310">Extendiendo **OnModelCreating** método, se especifica en el **código** cómo género se asignará a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-310">By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table.</span></span> <span data-ttu-id="c3f4e-311">Puede encontrar más información acerca de DBContext y DBSet en este artículo de msdn: [vínculo](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span><span class="sxs-lookup"><span data-stu-id="c3f4e-311">You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="c3f4e-312">Tarea 4: consultar la base de datos</span><span class="sxs-lookup"><span data-stu-id="c3f4e-312">Task 4 - Querying the Database</span></span>

<span data-ttu-id="c3f4e-313">En esta tarea, actualizará la clase StoreController que, en lugar de los datos codificados de forma rígida, que se recuperará de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-313">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="c3f4e-314">Esta tarea está en común con el ejercicio 1.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-314">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="c3f4e-315">Si ha completado el ejercicio 1 puede observar estos pasos son los mismos en ambos enfoques (primero la base de datos o Code first).</span><span class="sxs-lookup"><span data-stu-id="c3f4e-315">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="c3f4e-316">Son diferentes en cómo los datos se vinculan con el modelo, pero el acceso a las entidades de datos aún es transparente desde el controlador.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-316">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>


1. <span data-ttu-id="c3f4e-317">Abra **Controllers\StoreController.cs** y agregue el siguiente campo a la clase para contener una instancia de la **MusicStoreEntities** clase, denominada **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="c3f4e-317">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="c3f4e-318">(Código de fragmento de código - *modelos y acceso a datos - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="c3f4e-318">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. <span data-ttu-id="c3f4e-319">El **MusicStoreEntities** clase expone una propiedad de colección para cada tabla en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-319">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="c3f4e-320">Actualización **examinar** método de acción para recuperar un género con todos los **álbumes**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-320">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="c3f4e-321">(Código de fragmento de código - *modelos y Data Access: Ex2 Store examinar*)</span><span class="sxs-lookup"><span data-stu-id="c3f4e-321">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="c3f4e-322">Utiliza una funcionalidad de .NET llamado **LINQ** (language integrated query) para escribir expresiones de consulta fuertemente tipada en estas colecciones - que ejecutará el código en la base de datos y devolver los objetos que se pueden programar con.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-322">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="c3f4e-323">Para obtener más información acerca de LINQ, visite la [sitio msdn](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="c3f4e-323">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span></span>
3. <span data-ttu-id="c3f4e-324">Actualización **índice** método de acción para recuperar todos los géneros.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-324">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="c3f4e-325">(Código de fragmento de código - *modelos y Data Access: Ex2 Store índice*)</span><span class="sxs-lookup"><span data-stu-id="c3f4e-325">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. <span data-ttu-id="c3f4e-326">Actualización **índice** método de acción para recuperar todos los géneros y transformar la colección a una lista.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-326">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="c3f4e-327">(Código de fragmento de código - *modelos y Data Access: Ex2 Store GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="c3f4e-327">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="c3f4e-328">Tarea 5: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="c3f4e-328">Task 5 - Running the Application</span></span>

<span data-ttu-id="c3f4e-329">En esta tarea, comprobará que la página de índice Store ahora muestra los géneros que se almacenan en la base de datos en lugar del codificado de forma rígida las.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-329">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="c3f4e-330">No es necesario cambiar la plantilla de vista porque la **StoreController** está devolviendo el mismo **StoreIndexViewModel** como antes, pero esta vez los datos procederán de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-330">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="c3f4e-331">Volver a generar la solución y presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-331">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="c3f4e-332">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-332">The project starts in the Home page.</span></span> <span data-ttu-id="c3f4e-333">Compruebe que el menú de **géneros** ya no es una lista de codificado de forma rígida, y los datos se recuperan directamente de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-333">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="c3f4e-335">*Exploración de géneros de la base de datos*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-335">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="c3f4e-336">Ahora, vaya a cualquier género y comprobar que los álbumes se rellenan a partir de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-336">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="c3f4e-337">![Exploración álbumes desde la base de datos](aspnet-mvc-4-models-and-data-access/_static/image23.png "álbumes de exploración de la base de datos")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-337">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="c3f4e-338">*Álbumes de exploración de la base de datos*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-338">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="c3f4e-339">Ejercicio 3: Consultar la base de datos con parámetros</span><span class="sxs-lookup"><span data-stu-id="c3f4e-339">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="c3f4e-340">En este ejercicio, obtendrá información sobre cómo consultar la base de datos con parámetros y cómo usar la forma de resultado de consulta, una característica que reduce el número base de datos tiene acceso a la recuperación de datos en una forma más eficaz.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-340">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="c3f4e-341">Para obtener más información sobre la forma de resultado de consulta, visite el siguiente [artículo de msdn](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="c3f4e-341">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="c3f4e-342">Tarea 1: modificación StoreController para recuperar los álbumes de base de datos</span><span class="sxs-lookup"><span data-stu-id="c3f4e-342">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="c3f4e-343">En esta tarea, cambiará el **StoreController** clase para tener acceso a la base de datos para recuperar los álbumes de un género concreto.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-343">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="c3f4e-344">Abra el **comenzar** solución ubicado en el **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** carpeta si desea usar el enfoque de Code First o **Source\ Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin** carpeta si desea usar el enfoque centrado en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-344">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="c3f4e-345">En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-345">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="c3f4e-346">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-346">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c3f4e-347">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-347">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="c3f4e-348">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-348">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="c3f4e-349">Por último, compile la solución haciendo **compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-349">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="c3f4e-350">Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-350">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c3f4e-351">Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-351">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c3f4e-352">Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-352">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="c3f4e-353">Abra el **StoreController** clase para cambiar el **examinar** método de acción.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-353">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="c3f4e-354">Para ello, en el **el Explorador de soluciones**, expanda el **controladores** carpeta y haga doble clic en **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-354">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="c3f4e-355">Cambiar el **examinar** método de acción para recuperar los álbumes de un género concreto.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-355">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="c3f4e-356">Para ello, reemplace el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c3f4e-356">To do this, replace the following code:</span></span>

    <span data-ttu-id="c3f4e-357">(Código de fragmento de código - *modelos y Data Access: Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="c3f4e-357">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> <span data-ttu-id="c3f4e-358">Para rellenar una colección de la entidad, deberá usar el **Include** método para especificar que desea recuperar los álbumes demasiado.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-358">To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too.</span></span> <span data-ttu-id="c3f4e-359">Puede usar el. **Single()** extensión de LINQ porque en este caso se espera solo un género de un álbum.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-359">You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album.</span></span> <span data-ttu-id="c3f4e-360">El **Single()** método toma una expresión Lambda como un parámetro, que en este caso especifica un único objeto género tal que su nombre coincide con el valor definido.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-360">The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.</span></span>
> 
> <span data-ttu-id="c3f4e-361">Se aprovechará una característica que le permite indicar otras entidades relacionadas que desea que se carguen también cuando se recupera el objeto de género.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-361">You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="c3f4e-362">Esta característica se denomina **forma de resultado de consulta**y permite reducir el número de veces que se necesita para tener acceso a la base de datos para recuperar información.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-362">This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information.</span></span> <span data-ttu-id="c3f4e-363">En este escenario, desea capturar previamente los álbumes para el género que recupera.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-363">In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.</span></span>
> 
> <span data-ttu-id="c3f4e-364">La consulta incluye **Genres.Include (&quot;álbumes&quot;)** para indicar que desea también álbumes relacionados.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-364">The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well.</span></span> <span data-ttu-id="c3f4e-365">Esto dará como resultado en una aplicación más eficaz, ya que recuperará los datos de género y álbum en una solicitud de base de datos única.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-365">This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="c3f4e-366">Tarea 2: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="c3f4e-366">Task 2 - Running the Application</span></span>

<span data-ttu-id="c3f4e-367">En esta tarea, ejecutará la aplicación y recuperar los álbumes de un género específico de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-367">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="c3f4e-368">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-368">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="c3f4e-369">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-369">The project starts in the Home page.</span></span> <span data-ttu-id="c3f4e-370">Cambiar la dirección URL de **/Store/examinar? genre = Pop** para comprobar que se están recuperando los resultados de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-370">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="c3f4e-371">![Exploración por género](aspnet-mvc-4-models-and-data-access/_static/image24.png "exploración por género")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-371">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="c3f4e-372">*Exploración/Store/examinar? genre = Pop*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-372">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="c3f4e-373">Tarea 3: obtener acceso a los álbumes por Id.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-373">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="c3f4e-374">En esta tarea, se repetirá el procedimiento anterior para obtener álbumes por su identificador.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-374">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="c3f4e-375">Cierre el explorador si es necesario, para volver a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-375">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="c3f4e-376">Abra el **StoreController** clase para cambiar el **detalles** método de acción.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-376">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="c3f4e-377">Para ello, en el **el Explorador de soluciones**, expanda el **controladores** carpeta y haga doble clic en **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-377">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="c3f4e-378">Cambiar el **detalles** según el método de acción para recuperar los detalles de álbumes sus **Id**. Para ello, reemplace el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c3f4e-378">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="c3f4e-379">(Código de fragmento de código - *modelos y Data Access: Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="c3f4e-379">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="c3f4e-380">Tarea 4: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="c3f4e-380">Task 4 - Running the Application</span></span>

<span data-ttu-id="c3f4e-381">En esta tarea, ejecutará la aplicación en un explorador web y obtener detalles del álbum por su identificador.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-381">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="c3f4e-382">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-382">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="c3f4e-383">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-383">The project starts in the Home page.</span></span> <span data-ttu-id="c3f4e-384">Cambiar la dirección URL de **/Store/Details/51** o examinar los géneros y seleccionar un álbum para comprobar que se están recuperando los resultados de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-384">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="c3f4e-385">![Detalles de exploración](aspnet-mvc-4-models-and-data-access/_static/image25.png "detalles de exploración")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-385">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="c3f4e-386">*Exploración /Store/Details/51*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-386">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="c3f4e-387">Además, puede implementar esta aplicación a los siguientes sitios Web Windows Azure [Apéndice B: publicar una aplicación ASP.NET MVC 4 mediante Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="c3f4e-387">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="c3f4e-388">Resumen</span><span class="sxs-lookup"><span data-stu-id="c3f4e-388">Summary</span></span>

<span data-ttu-id="c3f4e-389">Por completar este laboratorio práctico, ha aprendido los aspectos básicos de acceso a datos y modelos de MVC de ASP.NET, mediante el **Database First** enfoque así como el **Code First** enfoque:</span><span class="sxs-lookup"><span data-stu-id="c3f4e-389">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="c3f4e-390">Cómo agregar una base de datos a la solución con el fin de consumir sus datos</span><span class="sxs-lookup"><span data-stu-id="c3f4e-390">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="c3f4e-391">Cómo actualizar los controladores para proporcionar plantillas de vista con los datos procedentes de la base de datos en lugar de codificada de forma rígida</span><span class="sxs-lookup"><span data-stu-id="c3f4e-391">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="c3f4e-392">Cómo consultar la base de datos con parámetros</span><span class="sxs-lookup"><span data-stu-id="c3f4e-392">How to query the database using parameters</span></span>
- <span data-ttu-id="c3f4e-393">Uso de la consulta resultado forma, una característica que reduce el número de accesos de base de datos, recuperar datos de una manera más eficaz</span><span class="sxs-lookup"><span data-stu-id="c3f4e-393">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="c3f4e-394">Cómo usar los enfoques Database First o Code First en Entity Framework de Microsoft para vincular la base de datos con el modelo</span><span class="sxs-lookup"><span data-stu-id="c3f4e-394">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="c3f4e-395">Apéndice A: instalación de Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="c3f4e-395">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="c3f4e-396">Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión utilizando el **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-396">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="c3f4e-397">Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* mediante *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-397">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="c3f4e-398">Vaya a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="c3f4e-398">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="c3f4e-399">Como alternativa, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con SDK de Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-399">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="c3f4e-400">Haga clic en **instalar ahora**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-400">Click on **Install Now**.</span></span> <span data-ttu-id="c3f4e-401">Si no tienes **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-401">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="c3f4e-402">Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-402">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="c3f4e-403">![Instale Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "instale Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-403">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="c3f4e-404">*Instale Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-404">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="c3f4e-405">Leer todos los productos las licencias y los términos y haga clic en **acepto** para continuar.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-405">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Acepte los términos de licencia](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="c3f4e-407">*Acepte los términos de licencia*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-407">*Accepting the license terms*</span></span>
5. <span data-ttu-id="c3f4e-408">Espere hasta que finalice el proceso de descarga e instalación.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-408">Wait until the downloading and installation process completes.</span></span>

    ![Progreso de la instalación](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="c3f4e-410">*Progreso de la instalación*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-410">*Installation progress*</span></span>
6. <span data-ttu-id="c3f4e-411">Cuando se complete la instalación, haga clic en **finalizar**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-411">When the installation completes, click **Finish**.</span></span>

    ![Instalación completada](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="c3f4e-413">*Instalación completada*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-413">*Installation completed*</span></span>
7. <span data-ttu-id="c3f4e-414">Haga clic en **Exit** para cerrar el instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-414">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="c3f4e-415">Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **VS Express**&quot;, a continuación, haga clic en el **VS Express para Web** icono.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-415">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para el icono de Web](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="c3f4e-417">*VS Express para el icono de Web*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-417">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="c3f4e-418">Apéndice B: publicar una aplicación ASP.NET MVC 4 mediante Web Deploy</span><span class="sxs-lookup"><span data-stu-id="c3f4e-418">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="c3f4e-419">En este apéndice se mostrará cómo crear un nuevo sitio web desde el Portal de administración de Windows Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, que aprovecha la característica de publicación Web Deploy proporcionada por Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-419">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="c3f4e-420">Tarea 1: crear un nuevo sitio Web de Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c3f4e-420">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="c3f4e-421">Vaya a la [Portal de administración de Windows Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas con su suscripción.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-421">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c3f4e-422">Con Windows Azure puede hospedar 10 sitios Web ASP.NET de forma gratuita y, a continuación, escalar a medida que crece el tráfico.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-422">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="c3f4e-423">Puede registrarse [aquí](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="c3f4e-423">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="c3f4e-424">![Inicie sesión en el portal de Windows Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "inicie sesión en el portal de Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-424">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="c3f4e-425">*Inicie sesión en el Portal de administración de Azure de Windows*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-425">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="c3f4e-426">Haga clic en **New** en la barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-426">Click **New** on the command bar.</span></span>

    <span data-ttu-id="c3f4e-427">![Crear un nuevo sitio Web](aspnet-mvc-4-models-and-data-access/_static/image32.png "crear un nuevo sitio Web")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-427">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="c3f4e-428">*Crear un nuevo sitio Web*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-428">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="c3f4e-429">Haga clic en **proceso** | **sitio Web**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-429">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="c3f4e-430">A continuación, seleccione **creación rápida** opción.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-430">Then select **Quick Create** option.</span></span> <span data-ttu-id="c3f4e-431">Proporcione una dirección URL disponible para el nuevo sitio web y haga clic en **crear sitio Web**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-431">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c3f4e-432">Un sitio Web de Windows Azure es el host para una aplicación web que se ejecutan en la nube que puede controlar y administrar.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-432">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="c3f4e-433">La opción Creación rápida permite implementar una aplicación web completada en el sitio Web Windows Azure desde fuera del portal.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-433">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="c3f4e-434">No incluye los pasos para configurar una base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-434">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="c3f4e-435">![Crear un nuevo sitio Web mediante Creación rápida](aspnet-mvc-4-models-and-data-access/_static/image33.png "crear un nuevo sitio Web mediante Creación rápida")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-435">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="c3f4e-436">*Crear un nuevo sitio Web mediante Creación rápida*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-436">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="c3f4e-437">Espere hasta que el nuevo **sitio Web** se crea.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-437">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="c3f4e-438">Una vez creado el sitio Web, haga clic en el vínculo situado bajo el **URL** columna.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-438">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="c3f4e-439">Compruebe que funciona el nuevo sitio Web.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-439">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="c3f4e-440">![Exploración al sitio web nuevo](aspnet-mvc-4-models-and-data-access/_static/image34.png "exploración al sitio web nuevo")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-440">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="c3f4e-441">*Exploración al sitio web nuevo*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-441">*Browsing to the new web site*</span></span>

    <span data-ttu-id="c3f4e-442">![Sitio Web que se ejecuta](aspnet-mvc-4-models-and-data-access/_static/image35.png "sitio Web que se ejecuta")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-442">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="c3f4e-443">*Sitio Web que se ejecuta*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-443">*Web site running*</span></span>
6. <span data-ttu-id="c3f4e-444">Vuelva al portal y haga clic en el nombre del sitio web en el **nombre** columna para mostrar las páginas de administración.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-444">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="c3f4e-445">![Abrir las páginas de administración del sitio web](aspnet-mvc-4-models-and-data-access/_static/image36.png "abrir las páginas de administración del sitio web")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-445">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="c3f4e-446">*Abrir las páginas de administración del sitio Web*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-446">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="c3f4e-447">En el **panel** página, en el **primea** sección, haga clic en el **descargar perfil de publicación** vínculo.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-447">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c3f4e-448">El *perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un sitio Web de Windows Azure para cada método de publicación habilitado.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-448">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="c3f4e-449">El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse y autenticarse en cada uno de los puntos de conexión para el que está habilitado un método de publicación.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-449">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="c3f4e-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para publicación de aplicaciones web para sitios Web de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="c3f4e-451">![Descargando el sitio web de perfil de publicación](aspnet-mvc-4-models-and-data-access/_static/image37.png "descargando el sitio web de perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-451">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="c3f4e-452">*Descargando el sitio Web de perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-452">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="c3f4e-453">Descargue el archivo de perfil de publicación en una ubicación conocida.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-453">Download the publish profile file to a known location.</span></span> <span data-ttu-id="c3f4e-454">Aún más en este ejercicio verá cómo usar este archivo para publicar una aplicación web a un sitios Web de Windows Azure desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-454">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="c3f4e-455">![Guardar el archivo de perfil de publicación](aspnet-mvc-4-models-and-data-access/_static/image38.png "guardar el perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-455">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="c3f4e-456">*Guardar el archivo de perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-456">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="c3f4e-457">Tarea 2: configurar el servidor de base de datos</span><span class="sxs-lookup"><span data-stu-id="c3f4e-457">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="c3f4e-458">Si la aplicación hace uso de SQL Server deberá crear un servidor de base de datos SQL de bases de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-458">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="c3f4e-459">Si desea implementar una aplicación simple que no utiliza SQL Server, podría omitir esta tarea.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-459">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="c3f4e-460">Necesitará un servidor de base de datos SQL para almacenar la base de datos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-460">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="c3f4e-461">Puede ver los servidores de base de datos SQL de la suscripción en el portal de administración de Windows Azure en **bases de datos Sql** | **servidores** | **del servidor Panel**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-461">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="c3f4e-462">Si no tiene un servidor creado, puede crear una mediante el **agregar** botón en la barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-462">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="c3f4e-463">Tome nota de la **nombre del servidor y dirección URL, nombre de inicio de sesión de administrador y contraseña**, ya que lo usará en las siguientes tareas.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-463">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="c3f4e-464">No cree la base de datos, como se crearán en una etapa posterior.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-464">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="c3f4e-465">![Panel de base de datos SQL Server](aspnet-mvc-4-models-and-data-access/_static/image39.png "panel de base de datos SQL Server")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-465">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="c3f4e-466">*Panel de base de datos SQL Server*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-466">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="c3f4e-467">En la siguiente tarea probará la conexión de base de datos desde Visual Studio, por ese motivo debe incluir la dirección IP local en la lista del servidor de **direcciones IP permitidas**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-467">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="c3f4e-468">Para ello, haga clic en **configurar**, seleccione la dirección IP de **dirección IP del cliente actual** y péguelo en el **dirección IP inicial** y **ladirecciónIPfinal** cuadros de texto y haga clic en el ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) botón.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-468">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![Agregar dirección IP del cliente](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="c3f4e-470">*Agregar dirección IP del cliente*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-470">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="c3f4e-471">Una vez el **dirección IP del cliente** se agrega a las direcciones IP permitidas de lista, haga clic en **guardar** para confirmar los cambios.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-471">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmar cambios](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="c3f4e-473">*Confirmar cambios*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-473">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="c3f4e-474">Tarea 3: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy</span><span class="sxs-lookup"><span data-stu-id="c3f4e-474">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="c3f4e-475">Vuelva a la solución de ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-475">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="c3f4e-476">En el **el Explorador de soluciones**, haga clic en el proyecto de sitio web y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-476">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="c3f4e-477">![Publicar la aplicación](aspnet-mvc-4-models-and-data-access/_static/image43.png "publicar la aplicación")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-477">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="c3f4e-478">*Publicar el sitio web*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-478">*Publishing the web site*</span></span>
2. <span data-ttu-id="c3f4e-479">Importar el perfil de publicación que guardó en la primera tarea.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-479">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="c3f4e-480">![Importar el perfil de publicación](aspnet-mvc-4-models-and-data-access/_static/image44.png "importar el perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-480">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="c3f4e-481">*Importar perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-481">*Importing publish profile*</span></span>
3. <span data-ttu-id="c3f4e-482">Haga clic en **validar conexión**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-482">Click **Validate Connection**.</span></span> <span data-ttu-id="c3f4e-483">Una vez completada la validación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-483">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c3f4e-484">Validación está completa cuando aparece una marca de verificación verde junto al botón Validar conexión.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-484">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="c3f4e-485">![Validación de la conexión](aspnet-mvc-4-models-and-data-access/_static/image45.png "validación de la conexión")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-485">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="c3f4e-486">*Validación de la conexión*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-486">*Validating connection*</span></span>
4. <span data-ttu-id="c3f4e-487">En el **configuración** página, en el **bases de datos** sección, haga clic en el botón situado junto al cuadro de texto de la conexión de base de datos (es decir, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="c3f4e-487">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="c3f4e-488">![Configuración de implementación Web](aspnet-mvc-4-models-and-data-access/_static/image46.png "configuración de implementación Web")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-488">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="c3f4e-489">*Configuración de implementación Web*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-489">*Web deploy configuration*</span></span>
5. <span data-ttu-id="c3f4e-490">Configure la conexión de base de datos como sigue:</span><span class="sxs-lookup"><span data-stu-id="c3f4e-490">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="c3f4e-491">En el **nombre del servidor** escriba la dirección URL de base de datos de SQL server mediante el *tcp:* prefijo.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-491">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="c3f4e-492">En **nombre de usuario** escriba el nombre de inicio de sesión del Administrador de servidor.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-492">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="c3f4e-493">En **contraseña** escriba la contraseña de inicio de sesión del Administrador de servidor.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-493">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="c3f4e-494">Escriba un nuevo nombre de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-494">Type a new database name.</span></span>

     <span data-ttu-id="c3f4e-495">![Configuración de la cadena de conexión de destino](aspnet-mvc-4-models-and-data-access/_static/image47.png "configuración de la cadena de conexión de destino")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-495">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="c3f4e-496">*Configuración de la cadena de conexión de destino*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-496">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="c3f4e-497">A continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-497">Then click **OK**.</span></span> <span data-ttu-id="c3f4e-498">Cuando se le pedirá que cree la base de datos, haga clic en **Sí**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-498">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="c3f4e-499">![Creación de la base de datos](aspnet-mvc-4-models-and-data-access/_static/image48.png "creación de la cadena de la base de datos")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-499">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="c3f4e-500">*Creación de la base de datos*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-500">*Creating the database*</span></span>
7. <span data-ttu-id="c3f4e-501">La cadena de conexión que se usará para conectarse a la base de datos de SQL en Windows Azure se muestra en el cuadro de texto de conexión predeterminado.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-501">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="c3f4e-502">Después, haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-502">Then click **Next**.</span></span>

    <span data-ttu-id="c3f4e-503">![Cadena de conexión que apunte a la base de datos SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "cadena de conexión que apunte a la base de datos SQL")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-503">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="c3f4e-504">*Cadena de conexión que apunte a la base de datos SQL*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-504">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="c3f4e-505">En el **Preview** página, haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-505">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="c3f4e-506">![Publicar la aplicación web](aspnet-mvc-4-models-and-data-access/_static/image50.png "publicar la aplicación web")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-506">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="c3f4e-507">*Publicar la aplicación web*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-507">*Publishing the web application*</span></span>
9. <span data-ttu-id="c3f4e-508">Una vez que finalice el proceso de publicación, el explorador predeterminado abrirá el sitio web publicado.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-508">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="c3f4e-509">Apéndice C: uso de fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="c3f4e-509">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="c3f4e-510">Con fragmentos de código, tiene todo el código que necesita a su alcance.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-510">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="c3f4e-511">El documento de laboratorio le dirá exactamente cuándo se pueden utilizar, como se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-511">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="c3f4e-512">![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-models-and-data-access/_static/image51.png "fragmentos de código en Visual Studio para insertar código en el proyecto")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-512">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="c3f4e-513">*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-513">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="c3f4e-514">***Para agregar un fragmento de código mediante el teclado (solo C#)***</span><span class="sxs-lookup"><span data-stu-id="c3f4e-514">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="c3f4e-515">Coloque el cursor donde desea insertar el código.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-515">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="c3f4e-516">Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).</span><span class="sxs-lookup"><span data-stu-id="c3f4e-516">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="c3f4e-517">Observe cómo IntelliSense muestra los nombres de fragmentos de código coincidentes.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-517">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="c3f4e-518">Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).</span><span class="sxs-lookup"><span data-stu-id="c3f4e-518">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="c3f4e-519">Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-519">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="c3f4e-520">![Comience a escribir el nombre del fragmento](aspnet-mvc-4-models-and-data-access/_static/image52.png "comience a escribir el nombre del fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-520">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="c3f4e-521">*Comience a escribir el nombre del fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-521">*Start typing the snippet name*</span></span>

<span data-ttu-id="c3f4e-522">![Presione la tecla Tab para seleccionar el fragmento de código resaltada](aspnet-mvc-4-models-and-data-access/_static/image53.png "presione Tab para seleccionar el fragmento de código resaltada")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-522">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="c3f4e-523">*Presione la tecla Tab para seleccionar el fragmento de código resaltada*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-523">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="c3f4e-524">![Vuelva a presionar Tab y el fragmento de código se expandirán](aspnet-mvc-4-models-and-data-access/_static/image54.png "vuelva a presionar Tab y el fragmento de código se expandirán")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-524">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="c3f4e-525">*Vuelva a presionar Tab y el fragmento de código se expandirán*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-525">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="c3f4e-526">***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-526">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="c3f4e-527">Haga doble clic en el desea insertar el fragmento de código.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-527">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="c3f4e-528">Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-528">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="c3f4e-529">Seleccione el fragmento de código relevante en la lista, haciendo clic en él.</span><span class="sxs-lookup"><span data-stu-id="c3f4e-529">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="c3f4e-530">![Con el botón secundario donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](aspnet-mvc-4-models-and-data-access/_static/image55.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-530">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="c3f4e-531">*Haga doble clic donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-531">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="c3f4e-532">![Elegir el fragmento de código relevante en la lista, hacer clic en ella](aspnet-mvc-4-models-and-data-access/_static/image56.png "elegir el fragmento de código relevante en la lista, hacer clic en ella")</span><span class="sxs-lookup"><span data-stu-id="c3f4e-532">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="c3f4e-533">*Elegir el fragmento de código relevante en la lista, hacer clic en ella*</span><span class="sxs-lookup"><span data-stu-id="c3f4e-533">*Pick the relevant snippet from the list, by clicking on it*</span></span>
