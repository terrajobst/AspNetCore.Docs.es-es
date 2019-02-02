---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Tutorial: Cambiar la base de datos para EF Database First con la aplicación de ASP.NET MVC'
description: En este tutorial se centra en realizar una actualización en la estructura de base de datos y propagar ese cambio a lo largo de la aplicación web.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667640"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="13fb3-103">Tutorial: Cambiar la base de datos para EF Database First con la aplicación de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="13fb3-103">Tutorial: Change the database for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="13fb3-104">Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="13fb3-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="13fb3-105">Esta serie de tutoriales muestra cómo generar el código que permite a los usuarios mostrar, editar, crear automáticamente y eliminar datos que residen en una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="13fb3-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="13fb3-106">El código generado corresponde a las columnas de la tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="13fb3-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="13fb3-107">En este tutorial se centra en realizar una actualización en la estructura de base de datos y propagar ese cambio a lo largo de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="13fb3-107">This tutorial focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>

<span data-ttu-id="13fb3-108">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="13fb3-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="13fb3-109">Agregar una columna</span><span class="sxs-lookup"><span data-stu-id="13fb3-109">Add a column</span></span>
> * <span data-ttu-id="13fb3-110">Agregue la propiedad a las vistas</span><span class="sxs-lookup"><span data-stu-id="13fb3-110">Add the property to the views</span></span>

## <a name="prerequisites"></a><span data-ttu-id="13fb3-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="13fb3-111">Prerequisites</span></span>

* [<span data-ttu-id="13fb3-112">Generación de vistas</span><span class="sxs-lookup"><span data-stu-id="13fb3-112">Generating views</span></span>](generating-views.md)

## <a name="add-a-column"></a><span data-ttu-id="13fb3-113">Agregar una columna</span><span class="sxs-lookup"><span data-stu-id="13fb3-113">Add a column</span></span>

<span data-ttu-id="13fb3-114">Si actualiza la estructura de una tabla en la base de datos, deberá asegurarse de que el cambio se propaga al modelo de datos, vistas y controlador.</span><span class="sxs-lookup"><span data-stu-id="13fb3-114">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="13fb3-115">Para este tutorial, agregará una nueva columna a la tabla Student para registrar el apellido del alumno.</span><span class="sxs-lookup"><span data-stu-id="13fb3-115">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="13fb3-116">Para agregar esta columna, abra el proyecto de base de datos y abra el archivo Student.sql.</span><span class="sxs-lookup"><span data-stu-id="13fb3-116">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="13fb3-117">Mediante el diseñador o en el código de Transact-SQL, agregue una columna denominada **MiddleName** que es un nvarchar (50) y permite valores NULL.</span><span class="sxs-lookup"><span data-stu-id="13fb3-117">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

<span data-ttu-id="13fb3-118">Implementar este cambio en la base de datos local, inicie el proyecto de base de datos (o F5).</span><span class="sxs-lookup"><span data-stu-id="13fb3-118">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="13fb3-119">El nuevo campo se agrega a la tabla.</span><span class="sxs-lookup"><span data-stu-id="13fb3-119">The new field is added to the table.</span></span> <span data-ttu-id="13fb3-120">Si no se ve en el Explorador de objetos de SQL Server, haga clic en el botón Actualizar en el panel.</span><span class="sxs-lookup"><span data-stu-id="13fb3-120">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![Mostrar la nueva columna](changing-the-database/_static/image2.png)

<span data-ttu-id="13fb3-122">La nueva columna existe en la tabla de base de datos, pero no existe actualmente en la clase de modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="13fb3-122">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="13fb3-123">Debe actualizar el modelo para incluir la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="13fb3-123">You must update the model to include your new column.</span></span> <span data-ttu-id="13fb3-124">En el **modelos** carpeta, abra el **ContosoModel.edmx** archivo para mostrar el diagrama del modelo.</span><span class="sxs-lookup"><span data-stu-id="13fb3-124">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="13fb3-125">Tenga en cuenta que el modelo Student no contiene la propiedad MiddleName.</span><span class="sxs-lookup"><span data-stu-id="13fb3-125">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="13fb3-126">Haga doble clic en cualquier lugar en la superficie de diseño y seleccione **actualizar modelo desde base de datos**.</span><span class="sxs-lookup"><span data-stu-id="13fb3-126">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

<span data-ttu-id="13fb3-127">En el asistente, seleccione el **actualizar** pestaña y, a continuación, seleccione **tablas** > **dbo** > **estudiante**.</span><span class="sxs-lookup"><span data-stu-id="13fb3-127">In the Update Wizard, select the **Refresh** tab and then select **Tables** > **dbo** > **Student**.</span></span> <span data-ttu-id="13fb3-128">Haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="13fb3-128">Click **Finish**.</span></span>

<span data-ttu-id="13fb3-129">Cuando finalice el proceso de actualización, el diagrama de base de datos incluye el nuevo **MiddleName** propiedad.</span><span class="sxs-lookup"><span data-stu-id="13fb3-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="13fb3-130">Guardar el **ContosoModel.edmx** archivo.</span><span class="sxs-lookup"><span data-stu-id="13fb3-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="13fb3-131">Debe guardar este archivo para la nueva propiedad se propagará a la **Student.cs** clase.</span><span class="sxs-lookup"><span data-stu-id="13fb3-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="13fb3-132">Ahora que ha actualizado la base de datos y el modelo.</span><span class="sxs-lookup"><span data-stu-id="13fb3-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="13fb3-133">Compile la solución.</span><span class="sxs-lookup"><span data-stu-id="13fb3-133">Build the solution.</span></span>

## <a name="add-the-property-to-the-views"></a><span data-ttu-id="13fb3-134">Agregue la propiedad a las vistas</span><span class="sxs-lookup"><span data-stu-id="13fb3-134">Add the property to the views</span></span>

<span data-ttu-id="13fb3-135">Lamentablemente, las vistas todavía no contienen la nueva propiedad.</span><span class="sxs-lookup"><span data-stu-id="13fb3-135">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="13fb3-136">Para actualizar las vistas tiene dos opciones: puede generar volver a las vistas mediante la adición de una vez más scaffolding para la clase Student o puede agregar manualmente la nueva propiedad a las vistas existentes.</span><span class="sxs-lookup"><span data-stu-id="13fb3-136">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="13fb3-137">En este tutorial, agregará el scaffolding de nuevo porque no ha realizado los cambios personalizados en las vistas que se genera automáticamente.</span><span class="sxs-lookup"><span data-stu-id="13fb3-137">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="13fb3-138">Considere la posibilidad de agregar manualmente la propiedad cuando ha realizado cambios en las vistas y no desea perder los cambios.</span><span class="sxs-lookup"><span data-stu-id="13fb3-138">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="13fb3-139">Para asegurarse de que las vistas se vuelven a creadas, eliminar el **estudiantes** carpeta bajo **vistas**y elimine el **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="13fb3-139">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="13fb3-140">A continuación, haga clic en el **controladores** carpeta y agregar el scaffolding para el **estudiante** modelo.</span><span class="sxs-lookup"><span data-stu-id="13fb3-140">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="13fb3-141">El nombre nuevo, el controlador **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="13fb3-141">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="13fb3-142">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="13fb3-142">Select **Add**.</span></span>

<span data-ttu-id="13fb3-143">Compile la solución de nuevo.</span><span class="sxs-lookup"><span data-stu-id="13fb3-143">Build the solution again.</span></span> <span data-ttu-id="13fb3-144">Las vistas contienen ahora la propiedad MiddleName.</span><span class="sxs-lookup"><span data-stu-id="13fb3-144">The views now contain the MiddleName property.</span></span>

![Mostrar el segundo nombre](changing-the-database/_static/image5.png)

## <a name="next-steps"></a><span data-ttu-id="13fb3-146">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="13fb3-146">Next steps</span></span>

<span data-ttu-id="13fb3-147">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="13fb3-147">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="13fb3-148">Se ha agregado una columna</span><span class="sxs-lookup"><span data-stu-id="13fb3-148">Added a column</span></span>
> * <span data-ttu-id="13fb3-149">Agrega la propiedad a las vistas</span><span class="sxs-lookup"><span data-stu-id="13fb3-149">Added the property to the views</span></span>

<span data-ttu-id="13fb3-150">Avance al siguiente tutorial para aprender a personalizar la vista para mostrar los detalles acerca de un registro de estudiante.</span><span class="sxs-lookup"><span data-stu-id="13fb3-150">Advance to the next tutorial to learn how to customize the view for showing details about a student record.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="13fb3-151">Personalizar una vista</span><span class="sxs-lookup"><span data-stu-id="13fb3-151">Customize a view</span></span>](customizing-a-view.md)