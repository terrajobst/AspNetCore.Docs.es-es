---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Base de datos EF primero con ASP.NET MVC: cambiar la base de datos | Documentos de Microsoft'
author: tfitzmac
description: "Con MVC, Entity Framework y Scaffolding de ASP.NET, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Este tutorial seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 1ffe753812e5eef817f03ab488a28ae5fcefd41e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a><span data-ttu-id="32585-104">Base de datos EF primero con ASP.NET MVC: cambiar la base de datos</span><span class="sxs-lookup"><span data-stu-id="32585-104">EF Database First with ASP.NET MVC: Changing the Database</span></span>
====================
<span data-ttu-id="32585-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="32585-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="32585-106">Con MVC, Entity Framework y Scaffolding de ASP.NET, puede crear una aplicación web que proporciona una interfaz a una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="32585-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="32585-107">Esta serie de tutoriales muestra cómo automáticamente genera código que permite a los usuarios mostrar, editar, crear y eliminar datos que residen en una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="32585-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="32585-108">El código generado corresponde a las columnas de la tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="32585-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="32585-109">Esta parte de la serie se centra en realizar una actualización en la estructura de base de datos y la propagación de dicho cambio a lo largo de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="32585-109">This part of the series focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>


## <a name="add-a-column"></a><span data-ttu-id="32585-110">Agregar una columna</span><span class="sxs-lookup"><span data-stu-id="32585-110">Add a column</span></span>

<span data-ttu-id="32585-111">Si actualiza la estructura de una tabla en la base de datos, debe asegurarse de que el cambio se propaga al modelo de datos, vistas y controlador.</span><span class="sxs-lookup"><span data-stu-id="32585-111">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="32585-112">Para este tutorial, agregará una nueva columna a la tabla de estudiantes para registrar el nombre completo de los estudiantes.</span><span class="sxs-lookup"><span data-stu-id="32585-112">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="32585-113">Para agregar esta columna, abra el proyecto de base de datos y abra el archivo Student.sql.</span><span class="sxs-lookup"><span data-stu-id="32585-113">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="32585-114">Mediante el diseñador o en el código de T-SQL, agregar una columna denominada **MiddleName** que es un nvarchar (50) y permite valores NULL.</span><span class="sxs-lookup"><span data-stu-id="32585-114">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

![agregar el segundo apellido](changing-the-database/_static/image1.png)

<span data-ttu-id="32585-116">Implemente este cambio en la base de datos local, inicie el proyecto de base de datos (o F5).</span><span class="sxs-lookup"><span data-stu-id="32585-116">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="32585-117">El nuevo campo se agrega a la tabla.</span><span class="sxs-lookup"><span data-stu-id="32585-117">The new field is added to the table.</span></span> <span data-ttu-id="32585-118">Si no se ve en el Explorador de objetos de SQL Server, haga clic en el botón Actualizar en el panel.</span><span class="sxs-lookup"><span data-stu-id="32585-118">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![Mostrar la nueva columna](changing-the-database/_static/image2.png)

<span data-ttu-id="32585-120">La nueva columna existe en la tabla de base de datos, pero no existe actualmente en la clase de modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="32585-120">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="32585-121">Debe actualizar el modelo para incluir la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="32585-121">You must update the model to include your new column.</span></span> <span data-ttu-id="32585-122">En el **modelos** carpeta, abra el **ContosoModel.edmx** archivo para mostrar el diagrama del modelo.</span><span class="sxs-lookup"><span data-stu-id="32585-122">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="32585-123">Tenga en cuenta que el modelo de estudiantes no contiene la propiedad MiddleName.</span><span class="sxs-lookup"><span data-stu-id="32585-123">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="32585-124">Haga doble clic en cualquier lugar en la superficie de diseño y seleccione **actualizar modelo desde base de datos**.</span><span class="sxs-lookup"><span data-stu-id="32585-124">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

![modelo de actualización](changing-the-database/_static/image3.png)

<span data-ttu-id="32585-126">En el asistente, seleccione la **actualizar** ficha y **estudiante** tabla.</span><span class="sxs-lookup"><span data-stu-id="32585-126">In the Update Wizard, select the **Refresh** tab and the **Student** table.</span></span>

![Asistente para actualización](changing-the-database/_static/image4.png)

<span data-ttu-id="32585-128">Haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="32585-128">Click **Finish**.</span></span>

<span data-ttu-id="32585-129">Cuando finalice el proceso de actualización, el diagrama de base de datos incluye la nueva **MiddleName** propiedad.</span><span class="sxs-lookup"><span data-stu-id="32585-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="32585-130">Guardar el **ContosoModel.edmx** archivo.</span><span class="sxs-lookup"><span data-stu-id="32585-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="32585-131">Debe guardar este archivo para la nueva propiedad se propagará a la **Student.cs** clase.</span><span class="sxs-lookup"><span data-stu-id="32585-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="32585-132">Ahora que ha actualizado la base de datos y el modelo.</span><span class="sxs-lookup"><span data-stu-id="32585-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="32585-133">Compile la solución.</span><span class="sxs-lookup"><span data-stu-id="32585-133">Build the solution.</span></span>

<span data-ttu-id="32585-134">Desafortunadamente, las vistas aún no contienen la nueva propiedad.</span><span class="sxs-lookup"><span data-stu-id="32585-134">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="32585-135">Para actualizar las vistas tiene dos opciones: puede generar volver a las vistas mediante la adición de una vez más scaffolding para la clase Student o puede agregar manualmente la nueva propiedad a las vistas existentes.</span><span class="sxs-lookup"><span data-stu-id="32585-135">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="32585-136">En este tutorial, agregará el scaffolding nuevo porque no se ha realizado algún cambio personalizada a las vistas generados automáticamente.</span><span class="sxs-lookup"><span data-stu-id="32585-136">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="32585-137">Considere la posibilidad de agregar manualmente la propiedad cuando ha realizado cambios en las vistas y no desea perder los cambios.</span><span class="sxs-lookup"><span data-stu-id="32585-137">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="32585-138">Para asegurarse de que las vistas se vuelven a creadas, eliminar el **estudiantes** carpeta bajo **vistas**y elimine la **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="32585-138">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="32585-139">A continuación, haga clic en el **controladores** carpeta y agregar el scaffolding para el **estudiante** modelo.</span><span class="sxs-lookup"><span data-stu-id="32585-139">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="32585-140">El nombre nuevo, el controlador **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="32585-140">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="32585-141">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="32585-141">Select **OK**.</span></span>

<span data-ttu-id="32585-142">Las vistas contienen ahora la propiedad MiddleName.</span><span class="sxs-lookup"><span data-stu-id="32585-142">The views now contain the MiddleName property.</span></span>

![Mostrar el segundo apellido](changing-the-database/_static/image5.png)

<span data-ttu-id="32585-144">En la siguiente sección, agregará código para personalizar la vista para mostrar detalles acerca de un registro de estudiante.</span><span class="sxs-lookup"><span data-stu-id="32585-144">In the next section, you will add code to customize the view for showing details about a student record.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="32585-145">[Anterior](generating-views.md)
[Siguiente](customizing-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="32585-145">[Previous](generating-views.md)
[Next](customizing-a-view.md)</span></span>
