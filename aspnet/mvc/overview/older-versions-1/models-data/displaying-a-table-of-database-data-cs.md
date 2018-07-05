---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: Mostrar una tabla de base de datos (C#) | Microsoft Docs
author: microsoft
description: En este tutorial, muestran dos métodos para mostrar un conjunto de registros de base de datos. Muestran dos métodos para dar formato a un conjunto de registros de base de datos en un elemento HTML ta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 06dc5c9398adb45d5a5ff8f57ff42816c983ee04
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395985"
---
<a name="displaying-a-table-of-database-data-c"></a><span data-ttu-id="c7343-104">Mostrar una tabla de base de datos (C#)</span><span class="sxs-lookup"><span data-stu-id="c7343-104">Displaying a Table of Database Data (C#)</span></span>
====================
<span data-ttu-id="c7343-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c7343-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="c7343-106">Descargar PDF</span><span class="sxs-lookup"><span data-stu-id="c7343-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> <span data-ttu-id="c7343-107">En este tutorial, muestran dos métodos para mostrar un conjunto de registros de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c7343-107">In this tutorial, I demonstrate two methods of displaying a set of database records.</span></span> <span data-ttu-id="c7343-108">Muestran dos métodos para dar formato a un conjunto de registros de base de datos en una tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="c7343-108">I show two methods of formatting a set of database records in an HTML table.</span></span> <span data-ttu-id="c7343-109">En primer lugar, voy a mostrar cómo dar formato a los registros de base de datos directamente dentro de una vista.</span><span class="sxs-lookup"><span data-stu-id="c7343-109">First, I show how you can format the database records directly within a view.</span></span> <span data-ttu-id="c7343-110">A continuación, demuestro cómo puede sacar partido de parciales al dar formato a los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c7343-110">Next, I demonstrate how you can take advantage of partials when formatting database records.</span></span>


<span data-ttu-id="c7343-111">El objetivo de este tutorial es explicar cómo se puede mostrar una tabla HTML de la base de datos en una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c7343-111">The goal of this tutorial is to explain how you can display an HTML table of database data in an ASP.NET MVC application.</span></span> <span data-ttu-id="c7343-112">En primer lugar, obtenga información sobre cómo usar las herramientas de scaffolding incluidas en Visual Studio para generar una vista que muestra automáticamente un conjunto de registros.</span><span class="sxs-lookup"><span data-stu-id="c7343-112">First, you learn how to use the scaffolding tools included in Visual Studio to generate a view that displays a set of records automatically.</span></span> <span data-ttu-id="c7343-113">A continuación, obtenga información sobre cómo usar una parcial como una plantilla al dar formato a los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c7343-113">Next, you learn how to use a partial as a template when formatting database records.</span></span>

## <a name="create-the-model-classes"></a><span data-ttu-id="c7343-114">Crear las clases del modelo</span><span class="sxs-lookup"><span data-stu-id="c7343-114">Create the Model Classes</span></span>

<span data-ttu-id="c7343-115">Vamos a mostrar el conjunto de registros de la tabla de base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="c7343-115">We are going to display the set of records from the Movies database table.</span></span> <span data-ttu-id="c7343-116">La tabla de base de datos de películas contiene las columnas siguientes:</span><span class="sxs-lookup"><span data-stu-id="c7343-116">The Movies database table contains the following columns:</span></span>

<a id="0.3_table01"></a>


| <span data-ttu-id="c7343-117">**Nombre de columna**</span><span class="sxs-lookup"><span data-stu-id="c7343-117">**Column Name**</span></span> | <span data-ttu-id="c7343-118">**Tipo de datos**</span><span class="sxs-lookup"><span data-stu-id="c7343-118">**Data Type**</span></span> | <span data-ttu-id="c7343-119">**Permitir valores null**</span><span class="sxs-lookup"><span data-stu-id="c7343-119">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c7343-120">Id.</span><span class="sxs-lookup"><span data-stu-id="c7343-120">Id</span></span> | <span data-ttu-id="c7343-121">Valor int.</span><span class="sxs-lookup"><span data-stu-id="c7343-121">Int</span></span> | <span data-ttu-id="c7343-122">False</span><span class="sxs-lookup"><span data-stu-id="c7343-122">False</span></span> |
| <span data-ttu-id="c7343-123">Title</span><span class="sxs-lookup"><span data-stu-id="c7343-123">Title</span></span> | <span data-ttu-id="c7343-124">Nvarchar (200)</span><span class="sxs-lookup"><span data-stu-id="c7343-124">Nvarchar(200)</span></span> | <span data-ttu-id="c7343-125">False</span><span class="sxs-lookup"><span data-stu-id="c7343-125">False</span></span> |
| <span data-ttu-id="c7343-126">Director</span><span class="sxs-lookup"><span data-stu-id="c7343-126">Director</span></span> | <span data-ttu-id="c7343-127">Nvarchar (50)</span><span class="sxs-lookup"><span data-stu-id="c7343-127">NVarchar(50)</span></span> | <span data-ttu-id="c7343-128">False</span><span class="sxs-lookup"><span data-stu-id="c7343-128">False</span></span> |
| <span data-ttu-id="c7343-129">DateReleased</span><span class="sxs-lookup"><span data-stu-id="c7343-129">DateReleased</span></span> | <span data-ttu-id="c7343-130">DateTime</span><span class="sxs-lookup"><span data-stu-id="c7343-130">DateTime</span></span> | <span data-ttu-id="c7343-131">False</span><span class="sxs-lookup"><span data-stu-id="c7343-131">False</span></span> |


<span data-ttu-id="c7343-132">Para representar la tabla de películas en nuestra aplicación de ASP.NET MVC, necesitamos crear una clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="c7343-132">In order to represent the Movies table in our ASP.NET MVC application, we need to create a model class.</span></span> <span data-ttu-id="c7343-133">En este tutorial, usamos Microsoft Entity Framework para crear nuestras clases de modelo.</span><span class="sxs-lookup"><span data-stu-id="c7343-133">In this tutorial, we use the Microsoft Entity Framework to create our model classes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="c7343-134">En este tutorial, usamos Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c7343-134">In this tutorial, we use the Microsoft Entity Framework.</span></span> <span data-ttu-id="c7343-135">Sin embargo, es importante entender que puede usar una variedad de tecnologías diferentes para interactuar con una base de datos desde una aplicación ASP.NET MVC, además de LINQ to SQL, NHibernate o ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="c7343-135">However, it is important to understand that you can use a variety of different technologies to interact with a database from an ASP.NET MVC application including LINQ to SQL, NHibernate, or ADO.NET.</span></span>


<span data-ttu-id="c7343-136">Siga estos pasos para iniciar al Asistente para Entity Data Model:</span><span class="sxs-lookup"><span data-stu-id="c7343-136">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="c7343-137">Haga clic en la carpeta de modelos en la ventana Explorador de soluciones y seleccione la opción de menú **agregar, nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="c7343-137">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="c7343-138">Seleccione el **datos** categoría y seleccione el **ADO.NET Entity Data Model** plantilla.</span><span class="sxs-lookup"><span data-stu-id="c7343-138">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="c7343-139">Asigne el nombre de su modelo de datos *MoviesDBModel.edmx* y haga clic en el **agregar** botón.</span><span class="sxs-lookup"><span data-stu-id="c7343-139">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="c7343-140">Tras hacer clic en el botón Agregar, el Asistente para Entity Data Model aparece (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="c7343-140">After you click the Add button, the Entity Data Model Wizard appears (see Figure 1).</span></span> <span data-ttu-id="c7343-141">Siga estos pasos para completar al asistente:</span><span class="sxs-lookup"><span data-stu-id="c7343-141">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="c7343-142">En el **Elegir contenido de Model** paso, seleccione la **generar desde la base de datos** opción.</span><span class="sxs-lookup"><span data-stu-id="c7343-142">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="c7343-143">En el **elegir la conexión de datos** paso, utilice el *MoviesDB.mdf* conexión de datos y el nombre *MoviesDBEntities* para la configuración de conexión.</span><span class="sxs-lookup"><span data-stu-id="c7343-143">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="c7343-144">Haga clic en el **siguiente** botón.</span><span class="sxs-lookup"><span data-stu-id="c7343-144">Click the **Next** button.</span></span>
3. <span data-ttu-id="c7343-145">En el **elija los objetos de base de datos** paso, expanda el nodo tablas, seleccione la tabla de películas.</span><span class="sxs-lookup"><span data-stu-id="c7343-145">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="c7343-146">Escriba el espacio de nombres *modelos* y haga clic en el **finalizar** botón.</span><span class="sxs-lookup"><span data-stu-id="c7343-146">Enter the namespace *Models* and click the **Finish** button.</span></span>


<span data-ttu-id="c7343-147">[![Creación de LINQ a las clases SQL](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c7343-147">[![Creating LINQ to SQL classes](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span></span>

<span data-ttu-id="c7343-148">**Figura 01**: crear clases de LINQ to SQL ([haga clic aquí para ver imagen en tamaño completo](displaying-a-table-of-database-data-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="c7343-148">**Figure 01**: Creating LINQ to SQL classes ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image2.png))</span></span>


<span data-ttu-id="c7343-149">Después de completar al Asistente para Entity Data Model, se abre el Entity Data Model Designer.</span><span class="sxs-lookup"><span data-stu-id="c7343-149">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="c7343-150">El diseñador debe mostrar la entidad de películas (consulte la figura 2).</span><span class="sxs-lookup"><span data-stu-id="c7343-150">The Designer should display the Movies entity (see Figure 2).</span></span>


<span data-ttu-id="c7343-151">[![El Entity Data Model Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="c7343-151">[![The Entity Data Model Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span></span>

<span data-ttu-id="c7343-152">**Figura 02**: The Entity Data Model Designer ([haga clic aquí para ver imagen en tamaño completo](displaying-a-table-of-database-data-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="c7343-152">**Figure 02**: The Entity Data Model Designer ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image4.png))</span></span>


<span data-ttu-id="c7343-153">Es necesario realizar un cambio antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="c7343-153">We need to make one change before we continue.</span></span> <span data-ttu-id="c7343-154">El Asistente para Entity Data genera una clase de modelo denominada *películas* que representa la tabla de base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="c7343-154">The Entity Data Wizard generates a model class named *Movies* that represents the Movies database table.</span></span> <span data-ttu-id="c7343-155">Dado que vamos a usar la clase de películas para representar una película concreta, es necesario modificar el nombre de la clase se *película* en lugar de *películas* (singular lugar plural).</span><span class="sxs-lookup"><span data-stu-id="c7343-155">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="c7343-156">Haga doble clic en el nombre de la clase en la superficie del diseñador y cambie el nombre de la clase de películas para película.</span><span class="sxs-lookup"><span data-stu-id="c7343-156">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="c7343-157">Después de realizar este cambio, haga clic en el **guardar** botón (el icono del disquete) para generar la clase de la película.</span><span class="sxs-lookup"><span data-stu-id="c7343-157">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="create-the-movies-controller"></a><span data-ttu-id="c7343-158">Crear el controlador de películas</span><span class="sxs-lookup"><span data-stu-id="c7343-158">Create the Movies Controller</span></span>

<span data-ttu-id="c7343-159">Ahora que tenemos una forma de representar nuestros registros de base de datos, podemos crear un controlador que devuelve la colección de películas.</span><span class="sxs-lookup"><span data-stu-id="c7343-159">Now that we have a way to represent our database records, we can create a controller that returns the collection of movies.</span></span> <span data-ttu-id="c7343-160">En la ventana Explorador de soluciones de Visual Studio, haga clic en la carpeta Controllers y seleccione la opción de menú **Add, controlador** (consulte la figura 3).</span><span class="sxs-lookup"><span data-stu-id="c7343-160">Within the Visual Studio Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller** (see Figure 3).</span></span>


<span data-ttu-id="c7343-161">[![El controlador menú Agregar](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="c7343-161">[![The Add Controller Menu](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span></span>

<span data-ttu-id="c7343-162">**Figura 03**: The Agregar controlador de menú ([haga clic aquí para ver imagen en tamaño completo](displaying-a-table-of-database-data-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="c7343-162">**Figure 03**: The Add Controller Menu ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image6.png))</span></span>


<span data-ttu-id="c7343-163">Cuando el **Agregar controlador** aparece el cuadro de diálogo, escriba el nombre del controlador MovieController (consulte la figura 4).</span><span class="sxs-lookup"><span data-stu-id="c7343-163">When the **Add Controller** dialog appears, enter the controller name MovieController (see Figure 4).</span></span> <span data-ttu-id="c7343-164">Haga clic en el **agregar** para agregar el nuevo controlador.</span><span class="sxs-lookup"><span data-stu-id="c7343-164">Click the **Add** button to add the new controller.</span></span>


<span data-ttu-id="c7343-165">[![El cuadro de diálogo Agregar controlador](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="c7343-165">[![The Add Controller dialog](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span></span>

<span data-ttu-id="c7343-166">**Figura 04**: cuadro de diálogo de agregar el controlador ([haga clic aquí para ver imagen en tamaño completo](displaying-a-table-of-database-data-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="c7343-166">**Figure 04**: The Add Controller dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image8.png))</span></span>


<span data-ttu-id="c7343-167">Es necesario modificar la acción de Index() expuesta por el controlador de película para que devuelva el conjunto de registros de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c7343-167">We need to modify the Index() action exposed by the Movie controller so that it returns the set of database records.</span></span> <span data-ttu-id="c7343-168">Modificar el controlador de modo que quede como el controlador en el listado 1.</span><span class="sxs-lookup"><span data-stu-id="c7343-168">Modify the controller so that it looks like the controller in Listing 1.</span></span>

<span data-ttu-id="c7343-169">**Listado 1 – Controllers\MovieController.cs**</span><span class="sxs-lookup"><span data-stu-id="c7343-169">**Listing 1 – Controllers\MovieController.cs**</span></span>

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

<span data-ttu-id="c7343-170">En el listado 1, la clase MoviesDBEntities se usa para representar la base de datos MoviesDB.</span><span class="sxs-lookup"><span data-stu-id="c7343-170">In Listing 1, the MoviesDBEntities class is used to represent the MoviesDB database.</span></span> <span data-ttu-id="c7343-171">Para usar esta clase, deberá importar el espacio de nombres MvcApplication1.Models similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="c7343-171">To use this class, you need to import the MvcApplication1.Models namespace like this:</span></span>

<span data-ttu-id="c7343-172">uso de MvcApplication1.Models;</span><span class="sxs-lookup"><span data-stu-id="c7343-172">using MvcApplication1.Models;</span></span>

<span data-ttu-id="c7343-173">La expresión *entidades. MovieSet.ToList()* devuelve el conjunto de todas las películas de la tabla de base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="c7343-173">The expression *entities.MovieSet.ToList()* returns the set of all movies from the Movies database table.</span></span>

## <a name="create-the-view"></a><span data-ttu-id="c7343-174">Crear la vista</span><span class="sxs-lookup"><span data-stu-id="c7343-174">Create the View</span></span>

<span data-ttu-id="c7343-175">Es la manera más fácil de mostrar un conjunto de registros de base de datos en una tabla HTML aprovechar el scaffolding proporcionado por Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c7343-175">The easiest way to display a set of database records in an HTML table is to take advantage of the scaffolding provided by Visual Studio.</span></span>

<span data-ttu-id="c7343-176">Compile la aplicación seleccionando la opción de menú **crear, compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="c7343-176">Build your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="c7343-177">Debe compilar la aplicación antes de abrir el **agregar vista** no aparecerán el cuadro de diálogo o las clases de datos en el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="c7343-177">You must build your application before opening the **Add View** dialog or your data classes won't appear in the dialog.</span></span>

<span data-ttu-id="c7343-178">Haga clic en la acción de Index() y seleccione la opción de menú **agregar vista** (consulte la figura 5).</span><span class="sxs-lookup"><span data-stu-id="c7343-178">Right-click the Index() action and select the menu option **Add View** (see Figure 5).</span></span>


<span data-ttu-id="c7343-179">[![Agregar una vista](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="c7343-179">[![Adding a view](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span></span>

<span data-ttu-id="c7343-180">**Figura 05**: adición de una vista ([haga clic aquí para ver imagen en tamaño completo](displaying-a-table-of-database-data-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="c7343-180">**Figure 05**: Adding a view ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image10.png))</span></span>


<span data-ttu-id="c7343-181">En el **agregar vista** cuadro de diálogo, active la casilla etiquetada **crear una vista fuertemente tipada**.</span><span class="sxs-lookup"><span data-stu-id="c7343-181">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="c7343-182">Seleccione la clase Movie como el **Ver clase de datos**.</span><span class="sxs-lookup"><span data-stu-id="c7343-182">Select the Movie class as the **view data class**.</span></span> <span data-ttu-id="c7343-183">Seleccione *lista* como el **ver contenido** (consulte la figura 6).</span><span class="sxs-lookup"><span data-stu-id="c7343-183">Select *List* as the **view content** (see Figure 6).</span></span> <span data-ttu-id="c7343-184">Al seleccionar estas opciones, se generará una vista fuertemente tipada que muestra una lista de películas.</span><span class="sxs-lookup"><span data-stu-id="c7343-184">Selecting these options will generate a strongly-typed view that displays a list of movies.</span></span>


<span data-ttu-id="c7343-185">[![El cuadro de diálogo Agregar vista](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="c7343-185">[![The Add View dialog](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span></span>

<span data-ttu-id="c7343-186">**Figura 06**: cuadro de diálogo de agregar la vista ([haga clic aquí para ver imagen en tamaño completo](displaying-a-table-of-database-data-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="c7343-186">**Figure 06**: The Add View dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image12.png))</span></span>


<span data-ttu-id="c7343-187">Tras hacer clic en el **agregar** botón, la vista en el listado 2 se genera automáticamente.</span><span class="sxs-lookup"><span data-stu-id="c7343-187">After you click the **Add** button, the view in Listing 2 is generated automatically.</span></span> <span data-ttu-id="c7343-188">Esta vista contiene el código necesario para recorrer en iteración la colección de películas y mostrar las propiedades de una película.</span><span class="sxs-lookup"><span data-stu-id="c7343-188">This view contains the code required to iterate through the collection of movies and display each of the properties of a movie.</span></span>

<span data-ttu-id="c7343-189">**Listado 2 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="c7343-189">**Listing 2 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

<span data-ttu-id="c7343-190">Puede ejecutar la aplicación seleccionando la opción de menú **depurar, Iniciar depuración** (o presionar la tecla F5).</span><span class="sxs-lookup"><span data-stu-id="c7343-190">You can run the application by selecting the menu option **Debug, Start Debugging** (or hitting the F5 key).</span></span> <span data-ttu-id="c7343-191">Se ejecuta la aplicación, inicia Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="c7343-191">Running the application launches Internet Explorer.</span></span> <span data-ttu-id="c7343-192">Si navega a la dirección URL de /Movie verá la página en la figura 7.</span><span class="sxs-lookup"><span data-stu-id="c7343-192">If you navigate to the /Movie URL then you'll see the page in Figure 7.</span></span>


<span data-ttu-id="c7343-193">[![Una tabla de películas](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="c7343-193">[![A table of movies](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span></span>

<span data-ttu-id="c7343-194">**Figura 07**: una tabla de películas ([haga clic aquí para ver imagen en tamaño completo](displaying-a-table-of-database-data-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="c7343-194">**Figure 07**: A table of movies([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image14.png))</span></span>


<span data-ttu-id="c7343-195">Si no le gusta nada acerca de la apariencia de la cuadrícula de registros de base de datos en la figura 7, a continuación, simplemente puede modificar la vista de índice.</span><span class="sxs-lookup"><span data-stu-id="c7343-195">If you don't like anything about the appearance of the grid of database records in Figure 7 then you can simply modify the Index view.</span></span> <span data-ttu-id="c7343-196">Por ejemplo, puede cambiar el *DateReleased* encabezado a *fecha de lanzamiento* mediante la modificación de la vista de índice.</span><span class="sxs-lookup"><span data-stu-id="c7343-196">For example, you can change the *DateReleased* header to *Date Released* by modifying the Index view.</span></span>

## <a name="create-a-template-with-a-partial"></a><span data-ttu-id="c7343-197">Crear una plantilla con una confianza parcial</span><span class="sxs-lookup"><span data-stu-id="c7343-197">Create a Template with a Partial</span></span>

<span data-ttu-id="c7343-198">Cuando una vista se complica demasiado, es una buena idea comenzar dividir la vista en parciales.</span><span class="sxs-lookup"><span data-stu-id="c7343-198">When a view gets too complicated, it is a good idea to start breaking the view into partials.</span></span> <span data-ttu-id="c7343-199">Uso parciales facilita las vistas de entender y mantener.</span><span class="sxs-lookup"><span data-stu-id="c7343-199">Using partials makes your views easier to understand and maintain.</span></span> <span data-ttu-id="c7343-200">Vamos a crear una parcial que podemos usar como plantilla para dar formato a cada uno de los registros de base de datos de la película.</span><span class="sxs-lookup"><span data-stu-id="c7343-200">We'll create a partial that we can use as a template to format each of the movie database records.</span></span>

<span data-ttu-id="c7343-201">Siga estos pasos para crear la parcial:</span><span class="sxs-lookup"><span data-stu-id="c7343-201">Follow these steps to create the partial:</span></span>

1. <span data-ttu-id="c7343-202">Haga clic en la carpeta Views\Movie y seleccione la opción de menú **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="c7343-202">Right-click the Views\Movie folder and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="c7343-203">Active la casilla etiquetada *crear una vista parcial (.ascx)*.</span><span class="sxs-lookup"><span data-stu-id="c7343-203">Check the checkbox labeled *Create a partial view (.ascx)*.</span></span>
3. <span data-ttu-id="c7343-204">Nombre de la parte *MovieTemplate*.</span><span class="sxs-lookup"><span data-stu-id="c7343-204">Name the partial *MovieTemplate*.</span></span>
4. <span data-ttu-id="c7343-205">Active la casilla etiquetada **crear una vista fuertemente tipada**.</span><span class="sxs-lookup"><span data-stu-id="c7343-205">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
5. <span data-ttu-id="c7343-206">Seleccione la película como la *Ver clase de datos*.</span><span class="sxs-lookup"><span data-stu-id="c7343-206">Select Movie as the *view data class*.</span></span>
6. <span data-ttu-id="c7343-207">Seleccione vacía como el *ver contenido*.</span><span class="sxs-lookup"><span data-stu-id="c7343-207">Select Empty as the *view content*.</span></span>
7. <span data-ttu-id="c7343-208">Haga clic en el **agregar** para agregar la parcial al proyecto.</span><span class="sxs-lookup"><span data-stu-id="c7343-208">Click the **Add** button to add the partial to your project.</span></span>

<span data-ttu-id="c7343-209">Después de completar estos pasos, modifique el MovieTemplate parcial sea similar a la lista 3.</span><span class="sxs-lookup"><span data-stu-id="c7343-209">After you complete these steps, modify the MovieTemplate partial to look like Listing 3.</span></span>

<span data-ttu-id="c7343-210">**Listado 3 – Views\Movie\MovieTemplate.ascx**</span><span class="sxs-lookup"><span data-stu-id="c7343-210">**Listing 3 – Views\Movie\MovieTemplate.ascx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

<span data-ttu-id="c7343-211">La parcial en el listado 3 contiene una plantilla para una sola fila de registros.</span><span class="sxs-lookup"><span data-stu-id="c7343-211">The partial in Listing 3 contains a template for a single row of records.</span></span>

<span data-ttu-id="c7343-212">La vista de índice modificada en el listado 4 usa el MovieTemplate parcial.</span><span class="sxs-lookup"><span data-stu-id="c7343-212">The modified Index view in Listing 4 uses the MovieTemplate partial.</span></span>

<span data-ttu-id="c7343-213">**Listado 4 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="c7343-213">**Listing 4 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

<span data-ttu-id="c7343-214">La vista en el listado 4 contiene un bucle foreach que itera a través de todas las películas.</span><span class="sxs-lookup"><span data-stu-id="c7343-214">The view in Listing 4 contains a foreach loop that iterates through all of the movies.</span></span> <span data-ttu-id="c7343-215">Para cada película, el MovieTemplate parcial se utiliza para dar formato a la película.</span><span class="sxs-lookup"><span data-stu-id="c7343-215">For each movie, the MovieTemplate partial is used to format the movie.</span></span> <span data-ttu-id="c7343-216">Se representa el MovieTemplate llamando al método auxiliar RenderPartial().</span><span class="sxs-lookup"><span data-stu-id="c7343-216">The MovieTemplate is rendered by calling the RenderPartial() helper method.</span></span>

<span data-ttu-id="c7343-217">La vista de índice modificada representa la tabla HTML muy misma de los registros de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c7343-217">The modified Index view renders the very same HTML table of database records.</span></span> <span data-ttu-id="c7343-218">Sin embargo, la vista se ha simplificado en gran medida.</span><span class="sxs-lookup"><span data-stu-id="c7343-218">However, the view has been greatly simplified.</span></span>


<span data-ttu-id="c7343-219">El método RenderPartial() es diferente que la mayoría de los otros métodos auxiliares, porque no devuelve una cadena.</span><span class="sxs-lookup"><span data-stu-id="c7343-219">The RenderPartial() method is different than most of the other helper methods because it does not return a string.</span></span> <span data-ttu-id="c7343-220">Por lo tanto, debe llamar el método RenderPartial() mediante &lt;% Html.RenderPartial(); %&gt; en lugar de &lt;% = Html.RenderPartial(); %&gt;.</span><span class="sxs-lookup"><span data-stu-id="c7343-220">Therefore, you must call the RenderPartial() method using &lt;% Html.RenderPartial(); %&gt; instead of &lt;%= Html.RenderPartial(); %&gt;.</span></span>


## <a name="summary"></a><span data-ttu-id="c7343-221">Resumen</span><span class="sxs-lookup"><span data-stu-id="c7343-221">Summary</span></span>

<span data-ttu-id="c7343-222">El objetivo de este tutorial era explicar cómo se puede mostrar un conjunto de registros de base de datos en una tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="c7343-222">The goal of this tutorial was to explain how you can display a set of database records in an HTML table.</span></span> <span data-ttu-id="c7343-223">En primer lugar, ha aprendido cómo devolver un conjunto de registros de base de datos de una acción de controlador aprovechando las ventajas de Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c7343-223">First, you learned how to return a set of database records from a controller action by taking advantage of the Microsoft Entity Framework.</span></span> <span data-ttu-id="c7343-224">A continuación, ha aprendido a usar scaffolding de Visual Studio para generar una vista que muestra automáticamente una colección de elementos.</span><span class="sxs-lookup"><span data-stu-id="c7343-224">Next, you learned how to use Visual Studio scaffolding to generate a view that displays a collection of items automatically.</span></span> <span data-ttu-id="c7343-225">Por último, ha aprendido cómo simplificar la vista aprovechando las ventajas de una parcial.</span><span class="sxs-lookup"><span data-stu-id="c7343-225">Finally, you learned how to simplify the view by taking advantage of a partial.</span></span> <span data-ttu-id="c7343-226">Ha aprendido a usar una parcial como una plantilla de modo que puede dar formato a cada registro de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c7343-226">You learned how to use a partial as a template so that you can format each database record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c7343-227">[Anterior](creating-model-classes-with-linq-to-sql-cs.md)
> [Siguiente](performing-simple-validation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="c7343-227">[Previous](creating-model-classes-with-linq-to-sql-cs.md)
[Next](performing-simple-validation-cs.md)</span></span>
