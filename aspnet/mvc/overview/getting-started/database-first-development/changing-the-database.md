---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'EF Database First con MVC de ASP.NET: cambiar la base de datos | Microsoft Docs'
author: tfitzmac
description: Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Este tutorial seri...
ms.author: aspnetcontent
ms.date: 10/01/2014
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 0176274694426a527ed0862b5138919d4cf5c319
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840947"
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a><span data-ttu-id="3e9d7-104">EF Database First con MVC de ASP.NET: cambiar la base de datos</span><span class="sxs-lookup"><span data-stu-id="3e9d7-104">EF Database First with ASP.NET MVC: Changing the Database</span></span>
====================
<span data-ttu-id="3e9d7-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3e9d7-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3e9d7-106">Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="3e9d7-107">Esta serie de tutoriales muestra cómo generar el código que permite a los usuarios mostrar, editar, crear automáticamente y eliminar datos que residen en una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="3e9d7-108">El código generado corresponde a las columnas de la tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="3e9d7-109">Esta parte de la serie se centra en realizar una actualización en la estructura de base de datos y propagar ese cambio a lo largo de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-109">This part of the series focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>


## <a name="add-a-column"></a><span data-ttu-id="3e9d7-110">Agregar una columna</span><span class="sxs-lookup"><span data-stu-id="3e9d7-110">Add a column</span></span>

<span data-ttu-id="3e9d7-111">Si actualiza la estructura de una tabla en la base de datos, deberá asegurarse de que el cambio se propaga al modelo de datos, vistas y controlador.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-111">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="3e9d7-112">Para este tutorial, agregará una nueva columna a la tabla Student para registrar el apellido del alumno.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-112">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="3e9d7-113">Para agregar esta columna, abra el proyecto de base de datos y abra el archivo Student.sql.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-113">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="3e9d7-114">Mediante el diseñador o en el código de Transact-SQL, agregue una columna denominada **MiddleName** que es un nvarchar (50) y permite valores NULL.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-114">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

![agregar el segundo nombre](changing-the-database/_static/image1.png)

<span data-ttu-id="3e9d7-116">Implementar este cambio en la base de datos local, inicie el proyecto de base de datos (o F5).</span><span class="sxs-lookup"><span data-stu-id="3e9d7-116">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="3e9d7-117">El nuevo campo se agrega a la tabla.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-117">The new field is added to the table.</span></span> <span data-ttu-id="3e9d7-118">Si no se ve en el Explorador de objetos de SQL Server, haga clic en el botón Actualizar en el panel.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-118">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![Mostrar la nueva columna](changing-the-database/_static/image2.png)

<span data-ttu-id="3e9d7-120">La nueva columna existe en la tabla de base de datos, pero no existe actualmente en la clase de modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-120">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="3e9d7-121">Debe actualizar el modelo para incluir la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-121">You must update the model to include your new column.</span></span> <span data-ttu-id="3e9d7-122">En el **modelos** carpeta, abra el **ContosoModel.edmx** archivo para mostrar el diagrama del modelo.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-122">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="3e9d7-123">Tenga en cuenta que el modelo Student no contiene la propiedad MiddleName.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-123">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="3e9d7-124">Haga doble clic en cualquier lugar en la superficie de diseño y seleccione **actualizar modelo desde base de datos**.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-124">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

![actualización del modelo](changing-the-database/_static/image3.png)

<span data-ttu-id="3e9d7-126">En el asistente, seleccione el **actualizar** ficha y el **estudiante** tabla.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-126">In the Update Wizard, select the **Refresh** tab and the **Student** table.</span></span>

![Asistente para actualizar](changing-the-database/_static/image4.png)

<span data-ttu-id="3e9d7-128">Haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-128">Click **Finish**.</span></span>

<span data-ttu-id="3e9d7-129">Cuando finalice el proceso de actualización, el diagrama de base de datos incluye el nuevo **MiddleName** propiedad.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="3e9d7-130">Guardar el **ContosoModel.edmx** archivo.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="3e9d7-131">Debe guardar este archivo para la nueva propiedad se propagará a la **Student.cs** clase.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="3e9d7-132">Ahora que ha actualizado la base de datos y el modelo.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="3e9d7-133">Compile la solución.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-133">Build the solution.</span></span>

<span data-ttu-id="3e9d7-134">Lamentablemente, las vistas todavía no contienen la nueva propiedad.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-134">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="3e9d7-135">Para actualizar las vistas tiene dos opciones: puede generar volver a las vistas mediante la adición de una vez más scaffolding para la clase Student o puede agregar manualmente la nueva propiedad a las vistas existentes.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-135">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="3e9d7-136">En este tutorial, agregará el scaffolding de nuevo porque no ha realizado los cambios personalizados en las vistas que se genera automáticamente.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-136">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="3e9d7-137">Considere la posibilidad de agregar manualmente la propiedad cuando ha realizado cambios en las vistas y no desea perder los cambios.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-137">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="3e9d7-138">Para asegurarse de que las vistas se vuelven a creadas, eliminar el **estudiantes** carpeta bajo **vistas**y elimine el **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-138">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="3e9d7-139">A continuación, haga clic en el **controladores** carpeta y agregar el scaffolding para el **estudiante** modelo.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-139">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="3e9d7-140">El nombre nuevo, el controlador **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-140">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="3e9d7-141">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-141">Select **OK**.</span></span>

<span data-ttu-id="3e9d7-142">Las vistas contienen ahora la propiedad MiddleName.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-142">The views now contain the MiddleName property.</span></span>

![Mostrar el segundo nombre](changing-the-database/_static/image5.png)

<span data-ttu-id="3e9d7-144">En la siguiente sección, agregará código para personalizar la vista para mostrar los detalles acerca de un registro de estudiante.</span><span class="sxs-lookup"><span data-stu-id="3e9d7-144">In the next section, you will add code to customize the view for showing details about a student record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3e9d7-145">[Anterior](generating-views.md)
> [Siguiente](customizing-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="3e9d7-145">[Previous](generating-views.md)
[Next](customizing-a-view.md)</span></span>
