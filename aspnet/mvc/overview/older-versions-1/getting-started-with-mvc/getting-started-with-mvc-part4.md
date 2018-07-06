---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Creación de una base de datos | Microsoft Docs
author: shanselman
description: Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Cree una aplicación web simple que lee y escribe desde una base de datos.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: fff974edfaffeb164879c10b8e70bd3482c47554
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813765"
---
<a name="creating-a-database"></a><span data-ttu-id="5b104-104">Creación de una base de datos</span><span class="sxs-lookup"><span data-stu-id="5b104-104">Creating a Database</span></span>
====================
<span data-ttu-id="5b104-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="5b104-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="5b104-106">Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5b104-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="5b104-107">Creará una aplicación web simple que lee y escribe desde una base de datos.</span><span class="sxs-lookup"><span data-stu-id="5b104-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="5b104-108">Visite el [centro de aprendizaje de ASP.NET MVC](../../../index.md) para buscar otros ASP.NET MVC, tutoriales y ejemplos.</span><span class="sxs-lookup"><span data-stu-id="5b104-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="5b104-109">En esta sección vamos a crear una nueva base de datos SQL Express a objetos que vamos a usar para almacenar y recuperar los datos de la película.</span><span class="sxs-lookup"><span data-stu-id="5b104-109">In this section we are going to create a new SQL Express database that we'll use to store and retrieve our movie data.</span></span> <span data-ttu-id="5b104-110">En el IDE de Visual Web Developer, seleccione Ver | Explorador de servidores.</span><span class="sxs-lookup"><span data-stu-id="5b104-110">From within the Visual Web Developer IDE, select View | Server Explorer.</span></span> <span data-ttu-id="5b104-111">Haga clic con el botón derecho en conexiones de datos y haga clic en Agregar conexión...</span><span class="sxs-lookup"><span data-stu-id="5b104-111">Right click on Data Connections and click Add Connection...</span></span>

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

<span data-ttu-id="5b104-113">En el cuadro de diálogo Elegir origen de datos, seleccione Microsoft SQL Server y seleccione continuar.</span><span class="sxs-lookup"><span data-stu-id="5b104-113">In the Choose Data Source dialog, select Microsoft SQL Server and select Continue.</span></span>

![](getting-started-with-mvc-part4/_static/image2.png)

<span data-ttu-id="5b104-114">En el cuadro de diálogo Agregar conexión, escriba ". \SQLEXPRESS" para el nombre del servidor y escriba "Movies" como el nombre de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5b104-114">In the Add Connection dialog, enter ".\SQLEXPRESS" for your Server Name, and enter "Movies" as the name for your new database.</span></span>

<span data-ttu-id="5b104-115">[![Agregar cuadro de diálogo de conexión](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="5b104-115">[![Add Connection dialog](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span></span>

<span data-ttu-id="5b104-116">Haga clic en Aceptar y se le preguntará si desea crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5b104-116">Click OK and you'll be asked if you want to create that database.</span></span> <span data-ttu-id="5b104-117">Seleccione Sí.</span><span class="sxs-lookup"><span data-stu-id="5b104-117">Select yes.</span></span>

<span data-ttu-id="5b104-118">[![¿Crear películas?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="5b104-118">[![Create Movies?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span></span>

<span data-ttu-id="5b104-119">Ahora ya tiene una base de datos vacía en el Explorador de servidores.</span><span class="sxs-lookup"><span data-stu-id="5b104-119">Now you've got an empty database in Server Explorer.</span></span>

![Agregar nueva tabla](getting-started-with-mvc-part4/_static/image7.png)

<span data-ttu-id="5b104-121">Haga clic con el botón derecho en las tablas y haga clic en Agregar tabla.</span><span class="sxs-lookup"><span data-stu-id="5b104-121">Right click on Tables and click Add Table.</span></span> <span data-ttu-id="5b104-122">Aparecerá el Diseñador de tablas.</span><span class="sxs-lookup"><span data-stu-id="5b104-122">The Table Designer will appear.</span></span> <span data-ttu-id="5b104-123">Agregar columnas para Id., Title, ReleaseDate, género y precio.</span><span class="sxs-lookup"><span data-stu-id="5b104-123">Add columns for Id, Title, ReleaseDate, Genre, and Price.</span></span> <span data-ttu-id="5b104-124">Haga clic con el botón derecho en la columna de identificador y haga clic en Establecer clave principal.</span><span class="sxs-lookup"><span data-stu-id="5b104-124">Right click on the ID column and click set Primary Key.</span></span> <span data-ttu-id="5b104-125">Aquí es lo que mis áreas de diseño similar a.</span><span class="sxs-lookup"><span data-stu-id="5b104-125">Here's what my design areas looks like.</span></span>

<span data-ttu-id="5b104-126">[![Editor de la tabla de base de datos](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="5b104-126">[![Database Table Editor](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span></span>

<span data-ttu-id="5b104-127">Además, seleccione la columna Id. y en las propiedades de columna siguiente, cambie "Especificación de identidad" en "Sí".</span><span class="sxs-lookup"><span data-stu-id="5b104-127">Also, select the Id column and under Column Properties below change "Identity Specification" to "Yes."</span></span>

<span data-ttu-id="5b104-128">[![IsIdentity - propiedades de columna](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="5b104-128">[![IsIdentity - Column Properties](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span></span>

<span data-ttu-id="5b104-129">Cuando haya entendido listo, haga clic en el icono Guardar en la barra de herramientas o seleccione archivo | Guardar en el menú y el nombre de la tabla "**película**" (simples).</span><span class="sxs-lookup"><span data-stu-id="5b104-129">When you've got it done, click the Save icon in the toolbar or select File | Save from the menu, and name your table "**Movie**" (singular).</span></span> <span data-ttu-id="5b104-130">Tenemos una base de datos y una tabla.</span><span class="sxs-lookup"><span data-stu-id="5b104-130">We've got a database and a table!</span></span>

<span data-ttu-id="5b104-131">[![Elegir nombre](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="5b104-131">[![Choose Name](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span></span>

<span data-ttu-id="5b104-132">Vuelva al explorador de servidores y haga clic en la tabla Movie, seleccione "Mostrar datos de tabla".</span><span class="sxs-lookup"><span data-stu-id="5b104-132">Go back to Server Explorer and right click the Movie table, then select "Show Table Data."</span></span> <span data-ttu-id="5b104-133">Escriba algunas películas para que nuestra base de datos tiene algunos datos.</span><span class="sxs-lookup"><span data-stu-id="5b104-133">Enter a few movies so our database has some data.</span></span>

<span data-ttu-id="5b104-134">[![Edición de tablas de base de datos](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="5b104-134">[![Database Table Editing](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span></span>

## <a name="creating-a-model"></a><span data-ttu-id="5b104-135">Creación de un modelo</span><span class="sxs-lookup"><span data-stu-id="5b104-135">Creating a Model</span></span>

<span data-ttu-id="5b104-136">Ahora, vuelva al explorador de soluciones en el lado derecho del IDE y haga doble clic en la carpeta Models y seleccione Agregar | Nuevo elemento.</span><span class="sxs-lookup"><span data-stu-id="5b104-136">Now, switch back to the Solution Explorer on the right hand side of the IDE and right-click on the Models folder and select Add | New Item.</span></span>

<span data-ttu-id="5b104-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="5b104-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span></span>

<span data-ttu-id="5b104-138">Vamos a crear un modelo de entidades de nuestra nueva base de datos.</span><span class="sxs-lookup"><span data-stu-id="5b104-138">We're going to create an Entity Model from our new database.</span></span> <span data-ttu-id="5b104-139">Esto agregará un conjunto de clases al proyecto que resulta muy fácil para nosotros consultar y manipular los datos dentro de nuestra base de datos.</span><span class="sxs-lookup"><span data-stu-id="5b104-139">This will add a set of classes to our project that makes it easy for us to query and manipulate the data within our database.</span></span> <span data-ttu-id="5b104-140">Seleccione el nodo de datos en el lado izquierdo del cuadro de diálogo y, a continuación, seleccione la plantilla de elemento de ADO.NET Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="5b104-140">Select the Data node on the left-hand side of the dialog, and then select the ADO.NET Entity Data Model item template.</span></span> <span data-ttu-id="5b104-141">Asígnele el nombre Movies.edmx.</span><span class="sxs-lookup"><span data-stu-id="5b104-141">Name it Movies.edmx.</span></span>

<span data-ttu-id="5b104-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="5b104-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span></span>

<span data-ttu-id="5b104-143">Haga clic en el botón "Agregar".</span><span class="sxs-lookup"><span data-stu-id="5b104-143">Click the "Add" button.</span></span> <span data-ttu-id="5b104-144">A continuación, se iniciará al "modelo de Asistente para Entity Data".</span><span class="sxs-lookup"><span data-stu-id="5b104-144">This will then launch the "Entity Data Model Wizard".</span></span>

<span data-ttu-id="5b104-145">En el nuevo cuadro de diálogo que aparece, seleccione Generar desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5b104-145">In the new dialog that pops up, select Generate from Database.</span></span> <span data-ttu-id="5b104-146">Puesto que solo hemos realizado una base de datos, solo se necesitará indicar a Entity Framework sobre nuestra nueva base de datos y su tabla.</span><span class="sxs-lookup"><span data-stu-id="5b104-146">Since we've just made a database, we'll only need to tell the Entity Framework about our new database and its table.</span></span> <span data-ttu-id="5b104-147">Haga clic en siguiente para guardar nuestra conexión de base de datos en la configuración de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="5b104-147">Click Next to save our database connection in our web application's configuration.</span></span> <span data-ttu-id="5b104-148">Ahora, compruebe las tablas y la película y haga clic en finalización.</span><span class="sxs-lookup"><span data-stu-id="5b104-148">Now, check the Tables and Movie checkbox and click Finish.</span></span>

<span data-ttu-id="5b104-149">[![Asistente para Entity Data Model](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="5b104-149">[![Entity Data Model Wizard](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span></span>

<span data-ttu-id="5b104-150">Ahora podemos ver nuestra nueva tabla de películas en Entity Framework Designer y acceder a él desde el código.</span><span class="sxs-lookup"><span data-stu-id="5b104-150">Now we can see our new Movie table in the Entity Framework Designer and access it from code.</span></span>

<span data-ttu-id="5b104-151">[![Películas - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="5b104-151">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span></span>

<span data-ttu-id="5b104-152">En la superficie de diseño puede ver una clase "llamada Movie".</span><span class="sxs-lookup"><span data-stu-id="5b104-152">On the design surface you can see a "Movie" class.</span></span> <span data-ttu-id="5b104-153">Esta clase se asigna a la tabla "Movie" en nuestra base de datos, y cada propiedad en él se asigna a una columna con la tabla.</span><span class="sxs-lookup"><span data-stu-id="5b104-153">This class maps to the "Movie" table in our database, and each property within it maps to a column with the table.</span></span> <span data-ttu-id="5b104-154">Cada instancia de una clase "Movie" corresponderá a una fila dentro de la tabla "Movie".</span><span class="sxs-lookup"><span data-stu-id="5b104-154">Each instance of a "Movie" class will correspond to a row within the "Movie" table.</span></span>

<span data-ttu-id="5b104-155">Si no le gusta la nomenclatura predeterminada y la asignación convenciones empleadas por Entity Framework, puede utilizar el Diseñador de Entity Framework para cambiar o personalizarlos.</span><span class="sxs-lookup"><span data-stu-id="5b104-155">If you don't like the default naming and mapping conventions used by the Entity Framework, you can use the Entity Framework designer to change or customize them.</span></span> <span data-ttu-id="5b104-156">Para esta aplicación podrá usar los valores predeterminados y guarde el archivo como de solo-es.</span><span class="sxs-lookup"><span data-stu-id="5b104-156">For this application we'll use the defaults and just save the file as-is.</span></span>

<span data-ttu-id="5b104-157">Ahora, vamos a trabajar con algunos datos reales.</span><span class="sxs-lookup"><span data-stu-id="5b104-157">Now, let's work with some real data!</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5b104-158">[Anterior](getting-started-with-mvc-part3.md)
> [Siguiente](getting-started-with-mvc-part5.md)</span><span class="sxs-lookup"><span data-stu-id="5b104-158">[Previous](getting-started-with-mvc-part3.md)
[Next](getting-started-with-mvc-part5.md)</span></span>
