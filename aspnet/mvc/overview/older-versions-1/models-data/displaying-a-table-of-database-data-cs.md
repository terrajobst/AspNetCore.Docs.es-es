---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: Muestra una tabla de la base de datos (C#) | Documentos de Microsoft
author: microsoft
description: En este tutorial, muestran dos métodos de presentación de un conjunto de registros de base de datos. Muestran dos métodos para dar formato a un conjunto de registros de base de datos en un elemento HTML ta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 1d5dc9dd4a82e4577c6c1a3b124d45fef0b0f67c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="displaying-a-table-of-database-data-c"></a><span data-ttu-id="925e4-104">Muestra una tabla de la base de datos (C#)</span><span class="sxs-lookup"><span data-stu-id="925e4-104">Displaying a Table of Database Data (C#)</span></span>
====================
<span data-ttu-id="925e4-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="925e4-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="925e4-106">Descarga de PDF</span><span class="sxs-lookup"><span data-stu-id="925e4-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> <span data-ttu-id="925e4-107">En este tutorial, muestran dos métodos de presentación de un conjunto de registros de base de datos.</span><span class="sxs-lookup"><span data-stu-id="925e4-107">In this tutorial, I demonstrate two methods of displaying a set of database records.</span></span> <span data-ttu-id="925e4-108">Muestran dos métodos para dar formato a un conjunto de registros de base de datos en una tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="925e4-108">I show two methods of formatting a set of database records in an HTML table.</span></span> <span data-ttu-id="925e4-109">En primer lugar, muestra cómo dar formato a los registros de base de datos directamente dentro de una vista.</span><span class="sxs-lookup"><span data-stu-id="925e4-109">First, I show how you can format the database records directly within a view.</span></span> <span data-ttu-id="925e4-110">A continuación, muestra cómo puede aprovechar las ventajas de parciales al dar formato a los registros de base de datos.</span><span class="sxs-lookup"><span data-stu-id="925e4-110">Next, I demonstrate how you can take advantage of partials when formatting database records.</span></span>


<span data-ttu-id="925e4-111">El objetivo de este tutorial es explicar cómo se puede mostrar una tabla HTML de la base de datos en una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="925e4-111">The goal of this tutorial is to explain how you can display an HTML table of database data in an ASP.NET MVC application.</span></span> <span data-ttu-id="925e4-112">En primer lugar, se muestra cómo utilizar las herramientas de scaffolding incluidas en Visual Studio para generar una vista que muestra automáticamente un conjunto de registros.</span><span class="sxs-lookup"><span data-stu-id="925e4-112">First, you learn how to use the scaffolding tools included in Visual Studio to generate a view that displays a set of records automatically.</span></span> <span data-ttu-id="925e4-113">A continuación, se muestra cómo utilizar un parcial como una plantilla al dar formato a los registros de base de datos.</span><span class="sxs-lookup"><span data-stu-id="925e4-113">Next, you learn how to use a partial as a template when formatting database records.</span></span>

## <a name="create-the-model-classes"></a><span data-ttu-id="925e4-114">Crear las clases del modelo</span><span class="sxs-lookup"><span data-stu-id="925e4-114">Create the Model Classes</span></span>

<span data-ttu-id="925e4-115">Vamos a mostrar el conjunto de registros de la tabla de base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="925e4-115">We are going to display the set of records from the Movies database table.</span></span> <span data-ttu-id="925e4-116">La tabla de base de datos de películas contiene las columnas siguientes:</span><span class="sxs-lookup"><span data-stu-id="925e4-116">The Movies database table contains the following columns:</span></span>

<a id="0.3_table01"></a>


| <span data-ttu-id="925e4-117">**Nombre de columna**</span><span class="sxs-lookup"><span data-stu-id="925e4-117">**Column Name**</span></span> | <span data-ttu-id="925e4-118">**Tipo de datos**</span><span class="sxs-lookup"><span data-stu-id="925e4-118">**Data Type**</span></span> | <span data-ttu-id="925e4-119">**Permitir valores null**</span><span class="sxs-lookup"><span data-stu-id="925e4-119">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="925e4-120">Id.</span><span class="sxs-lookup"><span data-stu-id="925e4-120">Id</span></span> | <span data-ttu-id="925e4-121">Valor int.</span><span class="sxs-lookup"><span data-stu-id="925e4-121">Int</span></span> | <span data-ttu-id="925e4-122">False</span><span class="sxs-lookup"><span data-stu-id="925e4-122">False</span></span> |
| <span data-ttu-id="925e4-123">Title</span><span class="sxs-lookup"><span data-stu-id="925e4-123">Title</span></span> | <span data-ttu-id="925e4-124">Nvarchar(200)</span><span class="sxs-lookup"><span data-stu-id="925e4-124">Nvarchar(200)</span></span> | <span data-ttu-id="925e4-125">False</span><span class="sxs-lookup"><span data-stu-id="925e4-125">False</span></span> |
| <span data-ttu-id="925e4-126">Director de</span><span class="sxs-lookup"><span data-stu-id="925e4-126">Director</span></span> | <span data-ttu-id="925e4-127">NVarchar(50)</span><span class="sxs-lookup"><span data-stu-id="925e4-127">NVarchar(50)</span></span> | <span data-ttu-id="925e4-128">False</span><span class="sxs-lookup"><span data-stu-id="925e4-128">False</span></span> |
| <span data-ttu-id="925e4-129">DateReleased</span><span class="sxs-lookup"><span data-stu-id="925e4-129">DateReleased</span></span> | <span data-ttu-id="925e4-130">DateTime</span><span class="sxs-lookup"><span data-stu-id="925e4-130">DateTime</span></span> | <span data-ttu-id="925e4-131">False</span><span class="sxs-lookup"><span data-stu-id="925e4-131">False</span></span> |


<span data-ttu-id="925e4-132">Para representar la tabla de películas en nuestra aplicación de ASP.NET MVC, necesitamos crear una clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="925e4-132">In order to represent the Movies table in our ASP.NET MVC application, we need to create a model class.</span></span> <span data-ttu-id="925e4-133">En este tutorial, se utiliza el marco de la entidad de Microsoft para crear las clases de modelo.</span><span class="sxs-lookup"><span data-stu-id="925e4-133">In this tutorial, we use the Microsoft Entity Framework to create our model classes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="925e4-134">En este tutorial, se utiliza el marco de la entidad de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="925e4-134">In this tutorial, we use the Microsoft Entity Framework.</span></span> <span data-ttu-id="925e4-135">Sin embargo, es importante entender que puede usar varias tecnologías diferentes para interactuar con una base de datos desde una aplicación de ASP.NET MVC además de LINQ to SQL, NHibernate o ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="925e4-135">However, it is important to understand that you can use a variety of different technologies to interact with a database from an ASP.NET MVC application including LINQ to SQL, NHibernate, or ADO.NET.</span></span>


<span data-ttu-id="925e4-136">Siga estos pasos para iniciar al Asistente para Entity Data Model:</span><span class="sxs-lookup"><span data-stu-id="925e4-136">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="925e4-137">Haga clic en la carpeta de modelos en la ventana Explorador de soluciones y seleccione la opción de menú **agregar, nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="925e4-137">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="925e4-138">Seleccione el **datos** categoría y seleccione la **ADO.NET Entity Data Model** plantilla.</span><span class="sxs-lookup"><span data-stu-id="925e4-138">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="925e4-139">Asigne el nombre de su modelo de datos *MoviesDBModel.edmx* y haga clic en el **agregar** botón.</span><span class="sxs-lookup"><span data-stu-id="925e4-139">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="925e4-140">Tras hacer clic en el botón Agregar, el Asistente para Entity Data Model aparece (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="925e4-140">After you click the Add button, the Entity Data Model Wizard appears (see Figure 1).</span></span> <span data-ttu-id="925e4-141">Siga estos pasos para completar al asistente:</span><span class="sxs-lookup"><span data-stu-id="925e4-141">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="925e4-142">En el **Elegir contenido del modelo** paso, seleccione la **generar desde la base de datos** opción.</span><span class="sxs-lookup"><span data-stu-id="925e4-142">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="925e4-143">En el **elegir la conexión de datos** paso a paso, use la *MoviesDB.mdf* conexión de datos y el nombre *MoviesDBEntities* para la configuración de conexión.</span><span class="sxs-lookup"><span data-stu-id="925e4-143">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="925e4-144">Haga clic en el **siguiente** botón.</span><span class="sxs-lookup"><span data-stu-id="925e4-144">Click the **Next** button.</span></span>
3. <span data-ttu-id="925e4-145">En el **elija los objetos de base de datos** paso a paso, expanda el nodo tablas, seleccione la tabla de películas.</span><span class="sxs-lookup"><span data-stu-id="925e4-145">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="925e4-146">Escriba el espacio de nombres *modelos* y haga clic en el **finalizar** botón.</span><span class="sxs-lookup"><span data-stu-id="925e4-146">Enter the namespace *Models* and click the **Finish** button.</span></span>


<span data-ttu-id="925e4-147">[![Creación de LINQ a las clases SQL](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="925e4-147">[![Creating LINQ to SQL classes](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span></span>

<span data-ttu-id="925e4-148">**Figura 01**: crear clases de LINQ to SQL ([haga clic aquí para ver la imagen a tamaño completo](displaying-a-table-of-database-data-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="925e4-148">**Figure 01**: Creating LINQ to SQL classes ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image2.png))</span></span>


<span data-ttu-id="925e4-149">Después de completar al Asistente para Entity Data Model, se abre el Entity Data Model Designer.</span><span class="sxs-lookup"><span data-stu-id="925e4-149">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="925e4-150">El diseñador debe mostrar la entidad de películas (consulte la figura 2).</span><span class="sxs-lookup"><span data-stu-id="925e4-150">The Designer should display the Movies entity (see Figure 2).</span></span>


<span data-ttu-id="925e4-151">[![Entity Data Model Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="925e4-151">[![The Entity Data Model Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span></span>

<span data-ttu-id="925e4-152">**Figura 02**: el Entity Data Model Designer ([haga clic aquí para ver la imagen a tamaño completo](displaying-a-table-of-database-data-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="925e4-152">**Figure 02**: The Entity Data Model Designer ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image4.png))</span></span>


<span data-ttu-id="925e4-153">Es preciso realizar un cambio antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="925e4-153">We need to make one change before we continue.</span></span> <span data-ttu-id="925e4-154">El Asistente para Entity Data genera una clase de modelo denominada *películas* que representa la tabla de base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="925e4-154">The Entity Data Wizard generates a model class named *Movies* that represents the Movies database table.</span></span> <span data-ttu-id="925e4-155">Dado que vamos a usar la clase de películas para representar una película determinada, es necesario modificar el nombre de la clase es *película* en lugar de *películas* (singular en lugar de plural).</span><span class="sxs-lookup"><span data-stu-id="925e4-155">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="925e4-156">Haga doble clic en el nombre de la clase en la superficie del diseñador y cambiar el nombre de la clase de películas a película.</span><span class="sxs-lookup"><span data-stu-id="925e4-156">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="925e4-157">Después de realizar este cambio, haga clic en el **guardar** botón (el icono del disquete) para generar la clase de la película.</span><span class="sxs-lookup"><span data-stu-id="925e4-157">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="create-the-movies-controller"></a><span data-ttu-id="925e4-158">Crear el controlador de películas</span><span class="sxs-lookup"><span data-stu-id="925e4-158">Create the Movies Controller</span></span>

<span data-ttu-id="925e4-159">Ahora que tenemos una manera de representar nuestros registros de base de datos, podemos crear un controlador que devuelve la colección de películas.</span><span class="sxs-lookup"><span data-stu-id="925e4-159">Now that we have a way to represent our database records, we can create a controller that returns the collection of movies.</span></span> <span data-ttu-id="925e4-160">Dentro de la ventana Explorador de soluciones de Visual Studio, haga clic en la carpeta Controllers y seleccione la opción de menú **agregar, controlador** (consulte la figura 3).</span><span class="sxs-lookup"><span data-stu-id="925e4-160">Within the Visual Studio Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller** (see Figure 3).</span></span>


<span data-ttu-id="925e4-161">[![El controlador menú Agregar](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="925e4-161">[![The Add Controller Menu](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span></span>

<span data-ttu-id="925e4-162">**Figura 03**: controlador de agregar el menú ([haga clic aquí para ver la imagen a tamaño completo](displaying-a-table-of-database-data-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="925e4-162">**Figure 03**: The Add Controller Menu ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image6.png))</span></span>


<span data-ttu-id="925e4-163">Cuando el **Agregar controlador** aparece el cuadro de diálogo, escriba el nombre del controlador MovieController (consulte la figura 4).</span><span class="sxs-lookup"><span data-stu-id="925e4-163">When the **Add Controller** dialog appears, enter the controller name MovieController (see Figure 4).</span></span> <span data-ttu-id="925e4-164">Haga clic en el **agregar** botón para agregar el nuevo controlador.</span><span class="sxs-lookup"><span data-stu-id="925e4-164">Click the **Add** button to add the new controller.</span></span>


<span data-ttu-id="925e4-165">[![El cuadro de diálogo Agregar controlador](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="925e4-165">[![The Add Controller dialog](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span></span>

<span data-ttu-id="925e4-166">**Figura 04**: cuadro de diálogo de agregar el controlador ([haga clic aquí para ver la imagen a tamaño completo](displaying-a-table-of-database-data-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="925e4-166">**Figure 04**: The Add Controller dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image8.png))</span></span>


<span data-ttu-id="925e4-167">Es necesario modificar la acción de Index() expuesta por el controlador de película para que devuelva el conjunto de registros de base de datos.</span><span class="sxs-lookup"><span data-stu-id="925e4-167">We need to modify the Index() action exposed by the Movie controller so that it returns the set of database records.</span></span> <span data-ttu-id="925e4-168">Modifique el controlador de modo que parece que el controlador en la lista 1.</span><span class="sxs-lookup"><span data-stu-id="925e4-168">Modify the controller so that it looks like the controller in Listing 1.</span></span>

<span data-ttu-id="925e4-169">**Lista 1 – Controllers\MovieController.cs**</span><span class="sxs-lookup"><span data-stu-id="925e4-169">**Listing 1 – Controllers\MovieController.cs**</span></span>

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

<span data-ttu-id="925e4-170">En la lista 1, la clase MoviesDBEntities se usa para representar la base de datos de MoviesDB.</span><span class="sxs-lookup"><span data-stu-id="925e4-170">In Listing 1, the MoviesDBEntities class is used to represent the MoviesDB database.</span></span> <span data-ttu-id="925e4-171">Para utilizar esta clase, debe importar el espacio de nombres MvcApplication1.Models similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="925e4-171">To use this class, you need to import the MvcApplication1.Models namespace like this:</span></span>

<span data-ttu-id="925e4-172">uso de MvcApplication1.Models;</span><span class="sxs-lookup"><span data-stu-id="925e4-172">using MvcApplication1.Models;</span></span>

<span data-ttu-id="925e4-173">La expresión *entidades. MovieSet.ToList()* devuelve el conjunto de todas las películas de la tabla de base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="925e4-173">The expression *entities.MovieSet.ToList()* returns the set of all movies from the Movies database table.</span></span>

## <a name="create-the-view"></a><span data-ttu-id="925e4-174">Crear la vista</span><span class="sxs-lookup"><span data-stu-id="925e4-174">Create the View</span></span>

<span data-ttu-id="925e4-175">Es la manera más fácil de mostrar un conjunto de registros de base de datos en una tabla HTML aprovechar el scaffolding proporcionado por Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="925e4-175">The easiest way to display a set of database records in an HTML table is to take advantage of the scaffolding provided by Visual Studio.</span></span>

<span data-ttu-id="925e4-176">Compilar la aplicación seleccionando la opción de menú **compilar, compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="925e4-176">Build your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="925e4-177">Debe compilar la aplicación antes de abrir el **agregar vista** cuadro de diálogo o las clases de datos no aparecerán en el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="925e4-177">You must build your application before opening the **Add View** dialog or your data classes won't appear in the dialog.</span></span>

<span data-ttu-id="925e4-178">Haga clic en la acción de Index() y seleccione la opción de menú **agregar vista** (Véase la figura 5).</span><span class="sxs-lookup"><span data-stu-id="925e4-178">Right-click the Index() action and select the menu option **Add View** (see Figure 5).</span></span>


<span data-ttu-id="925e4-179">[![Agregar una vista](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="925e4-179">[![Adding a view](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span></span>

<span data-ttu-id="925e4-180">**Figura 05**: agregar una vista ([haga clic aquí para ver la imagen a tamaño completo](displaying-a-table-of-database-data-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="925e4-180">**Figure 05**: Adding a view ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image10.png))</span></span>


<span data-ttu-id="925e4-181">En el **agregar vista** cuadro de diálogo, active la casilla etiquetada **crear una vista fuertemente tipada**.</span><span class="sxs-lookup"><span data-stu-id="925e4-181">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="925e4-182">Seleccione la clase de película como el **Ver clase de datos**.</span><span class="sxs-lookup"><span data-stu-id="925e4-182">Select the Movie class as the **view data class**.</span></span> <span data-ttu-id="925e4-183">Seleccione *lista* como el **ver contenido** (consulte la figura 6).</span><span class="sxs-lookup"><span data-stu-id="925e4-183">Select *List* as the **view content** (see Figure 6).</span></span> <span data-ttu-id="925e4-184">Al seleccionar estas opciones, se generará una vista fuertemente tipada que muestra una lista de películas.</span><span class="sxs-lookup"><span data-stu-id="925e4-184">Selecting these options will generate a strongly-typed view that displays a list of movies.</span></span>


<span data-ttu-id="925e4-185">[![El cuadro de diálogo Agregar vista](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="925e4-185">[![The Add View dialog](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span></span>

<span data-ttu-id="925e4-186">**Figura 06**: cuadro de diálogo de agregar la vista ([haga clic aquí para ver la imagen a tamaño completo](displaying-a-table-of-database-data-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="925e4-186">**Figure 06**: The Add View dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image12.png))</span></span>


<span data-ttu-id="925e4-187">Tras hacer clic en el **agregar** botón, la vista en el listado 2 se genera automáticamente.</span><span class="sxs-lookup"><span data-stu-id="925e4-187">After you click the **Add** button, the view in Listing 2 is generated automatically.</span></span> <span data-ttu-id="925e4-188">Esta vista contiene el código necesario para recorrer en iteración la colección de películas y mostrará cada una de las propiedades de una película.</span><span class="sxs-lookup"><span data-stu-id="925e4-188">This view contains the code required to iterate through the collection of movies and display each of the properties of a movie.</span></span>

<span data-ttu-id="925e4-189">**La lista 2 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="925e4-189">**Listing 2 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

<span data-ttu-id="925e4-190">Puede ejecutar la aplicación seleccionando la opción de menú **depurar, Iniciar depuración** (o presionar la tecla F5).</span><span class="sxs-lookup"><span data-stu-id="925e4-190">You can run the application by selecting the menu option **Debug, Start Debugging** (or hitting the F5 key).</span></span> <span data-ttu-id="925e4-191">La aplicación se ejecuta, inicia Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="925e4-191">Running the application launches Internet Explorer.</span></span> <span data-ttu-id="925e4-192">Si navega a la dirección URL /Movie, a continuación, verá la página en la figura 7.</span><span class="sxs-lookup"><span data-stu-id="925e4-192">If you navigate to the /Movie URL then you'll see the page in Figure 7.</span></span>


<span data-ttu-id="925e4-193">[![Una tabla de películas](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="925e4-193">[![A table of movies](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span></span>

<span data-ttu-id="925e4-194">**Figura 07**: una tabla de películas ([haga clic aquí para ver la imagen a tamaño completo](displaying-a-table-of-database-data-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="925e4-194">**Figure 07**: A table of movies([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image14.png))</span></span>


<span data-ttu-id="925e4-195">Si no le gusta a nada sobre el aspecto de la cuadrícula de registros de base de datos en la figura 7, a continuación, simplemente puede modificar la vista de índice.</span><span class="sxs-lookup"><span data-stu-id="925e4-195">If you don't like anything about the appearance of the grid of database records in Figure 7 then you can simply modify the Index view.</span></span> <span data-ttu-id="925e4-196">Por ejemplo, puede cambiar la *DateReleased* encabezado a *fecha de lanzamiento* mediante la modificación de la vista de índice.</span><span class="sxs-lookup"><span data-stu-id="925e4-196">For example, you can change the *DateReleased* header to *Date Released* by modifying the Index view.</span></span>

## <a name="create-a-template-with-a-partial"></a><span data-ttu-id="925e4-197">Crear una plantilla con una confianza parcial</span><span class="sxs-lookup"><span data-stu-id="925e4-197">Create a Template with a Partial</span></span>

<span data-ttu-id="925e4-198">Cuando una vista será demasiado complicada, resulta una buena idea empezar interrumpa la vista parciales.</span><span class="sxs-lookup"><span data-stu-id="925e4-198">When a view gets too complicated, it is a good idea to start breaking the view into partials.</span></span> <span data-ttu-id="925e4-199">Usar parciales hace que las vistas fáciles de entender y mantener.</span><span class="sxs-lookup"><span data-stu-id="925e4-199">Using partials makes your views easier to understand and maintain.</span></span> <span data-ttu-id="925e4-200">Vamos a crear un parcial que podemos usar como plantilla para dar formato a cada uno de los registros de base de datos de la película.</span><span class="sxs-lookup"><span data-stu-id="925e4-200">We'll create a partial that we can use as a template to format each of the movie database records.</span></span>

<span data-ttu-id="925e4-201">Siga estos pasos para crear la parcial:</span><span class="sxs-lookup"><span data-stu-id="925e4-201">Follow these steps to create the partial:</span></span>

1. <span data-ttu-id="925e4-202">Haga clic en la carpeta Views\Movie y seleccione la opción de menú **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="925e4-202">Right-click the Views\Movie folder and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="925e4-203">Active la casilla etiquetada *crear una vista parcial (.ascx)*.</span><span class="sxs-lookup"><span data-stu-id="925e4-203">Check the checkbox labeled *Create a partial view (.ascx)*.</span></span>
3. <span data-ttu-id="925e4-204">Nombre de parte *MovieTemplate*.</span><span class="sxs-lookup"><span data-stu-id="925e4-204">Name the partial *MovieTemplate*.</span></span>
4. <span data-ttu-id="925e4-205">Active la casilla etiquetada **crear una vista fuertemente tipada**.</span><span class="sxs-lookup"><span data-stu-id="925e4-205">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
5. <span data-ttu-id="925e4-206">Seleccione la película como el *Ver clase de datos*.</span><span class="sxs-lookup"><span data-stu-id="925e4-206">Select Movie as the *view data class*.</span></span>
6. <span data-ttu-id="925e4-207">Seleccione vacío como el *ver contenido*.</span><span class="sxs-lookup"><span data-stu-id="925e4-207">Select Empty as the *view content*.</span></span>
7. <span data-ttu-id="925e4-208">Haga clic en el **agregar** botón para agregar la parcial al proyecto.</span><span class="sxs-lookup"><span data-stu-id="925e4-208">Click the **Add** button to add the partial to your project.</span></span>

<span data-ttu-id="925e4-209">Después de completar estos pasos, modifique la MovieTemplate parcial que se parezca a listado 3.</span><span class="sxs-lookup"><span data-stu-id="925e4-209">After you complete these steps, modify the MovieTemplate partial to look like Listing 3.</span></span>

<span data-ttu-id="925e4-210">**El listado 3 – Views\Movie\MovieTemplate.ascx**</span><span class="sxs-lookup"><span data-stu-id="925e4-210">**Listing 3 – Views\Movie\MovieTemplate.ascx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

<span data-ttu-id="925e4-211">Parte en el listado 3 contiene una plantilla para una sola fila de registros.</span><span class="sxs-lookup"><span data-stu-id="925e4-211">The partial in Listing 3 contains a template for a single row of records.</span></span>

<span data-ttu-id="925e4-212">La vista de índice modificada en el listado 4 utiliza la MovieTemplate parcial.</span><span class="sxs-lookup"><span data-stu-id="925e4-212">The modified Index view in Listing 4 uses the MovieTemplate partial.</span></span>

<span data-ttu-id="925e4-213">**Listado 4 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="925e4-213">**Listing 4 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

<span data-ttu-id="925e4-214">La vista en el listado 4 contiene un bucle foreach que recorre en iteración todas las películas.</span><span class="sxs-lookup"><span data-stu-id="925e4-214">The view in Listing 4 contains a foreach loop that iterates through all of the movies.</span></span> <span data-ttu-id="925e4-215">Para cada película, el MovieTemplate parcial se usa para dar formato a la película.</span><span class="sxs-lookup"><span data-stu-id="925e4-215">For each movie, the MovieTemplate partial is used to format the movie.</span></span> <span data-ttu-id="925e4-216">El MovieTemplate se representa mediante una llamada al método de aplicación auxiliar RenderPartial().</span><span class="sxs-lookup"><span data-stu-id="925e4-216">The MovieTemplate is rendered by calling the RenderPartial() helper method.</span></span>

<span data-ttu-id="925e4-217">La vista de índice modificada representa la tabla HTML muy misma de registros de base de datos.</span><span class="sxs-lookup"><span data-stu-id="925e4-217">The modified Index view renders the very same HTML table of database records.</span></span> <span data-ttu-id="925e4-218">Sin embargo, la vista se ha simplificado en gran medida.</span><span class="sxs-lookup"><span data-stu-id="925e4-218">However, the view has been greatly simplified.</span></span>


<span data-ttu-id="925e4-219">El método RenderPartial() es diferente de la mayoría de los otros métodos de aplicación auxiliar porque no devuelve una cadena.</span><span class="sxs-lookup"><span data-stu-id="925e4-219">The RenderPartial() method is different than most of the other helper methods because it does not return a string.</span></span> <span data-ttu-id="925e4-220">Por lo tanto, debe llamar el método RenderPartial() mediante &lt;% Html.RenderPartial(); %&gt; en lugar de &lt;% = Html.RenderPartial(); %&gt;.</span><span class="sxs-lookup"><span data-stu-id="925e4-220">Therefore, you must call the RenderPartial() method using &lt;% Html.RenderPartial(); %&gt; instead of &lt;%= Html.RenderPartial(); %&gt;.</span></span>


## <a name="summary"></a><span data-ttu-id="925e4-221">Resumen</span><span class="sxs-lookup"><span data-stu-id="925e4-221">Summary</span></span>

<span data-ttu-id="925e4-222">El objetivo de este tutorial era explicar cómo se puede mostrar un conjunto de registros de base de datos en una tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="925e4-222">The goal of this tutorial was to explain how you can display a set of database records in an HTML table.</span></span> <span data-ttu-id="925e4-223">En primer lugar, aprendió cómo devolver un conjunto de registros de base de datos de una acción de controlador aprovechando las ventajas de Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="925e4-223">First, you learned how to return a set of database records from a controller action by taking advantage of the Microsoft Entity Framework.</span></span> <span data-ttu-id="925e4-224">A continuación, aprendió a utilizar el scaffolding de Visual Studio para generar una vista que muestra automáticamente una colección de elementos.</span><span class="sxs-lookup"><span data-stu-id="925e4-224">Next, you learned how to use Visual Studio scaffolding to generate a view that displays a collection of items automatically.</span></span> <span data-ttu-id="925e4-225">Por último, aprendió a simplificar la vista aprovechando las ventajas de una parcial.</span><span class="sxs-lookup"><span data-stu-id="925e4-225">Finally, you learned how to simplify the view by taking advantage of a partial.</span></span> <span data-ttu-id="925e4-226">Aprendió a utilizar un parcial como una plantilla, por lo que puede dar formato a cada registro de base de datos.</span><span class="sxs-lookup"><span data-stu-id="925e4-226">You learned how to use a partial as a template so that you can format each database record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="925e4-227">[Anterior](creating-model-classes-with-linq-to-sql-cs.md)
> [Siguiente](performing-simple-validation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="925e4-227">[Previous](creating-model-classes-with-linq-to-sql-cs.md)
[Next](performing-simple-validation-cs.md)</span></span>
