---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Tutorial: Generar vistas para EF Database First con la aplicación de ASP.NET MVC'
description: En este tutorial se centra en usar el Scaffolding de ASP.NET para generar los controladores y vistas.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 7a56c0f9197a99427bcde6103ebc69d245e8ce63
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236424"
---
# <a name="tutorial-generate-views-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="53899-103">Tutorial: Generar vistas para EF Database First con la aplicación de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="53899-103">Tutorial: Generate views for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="53899-104">Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="53899-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="53899-105">Esta serie de tutoriales muestra cómo generar el código que permite a los usuarios mostrar, editar, crear automáticamente y eliminar datos que residen en una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="53899-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="53899-106">El código generado corresponde a las columnas de la tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="53899-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="53899-107">En este tutorial se centra en usar el Scaffolding de ASP.NET para generar los controladores y vistas.</span><span class="sxs-lookup"><span data-stu-id="53899-107">This tutorial focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>

<span data-ttu-id="53899-108">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="53899-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="53899-109">Agregar scaffold</span><span class="sxs-lookup"><span data-stu-id="53899-109">Add scaffold</span></span>
> * <span data-ttu-id="53899-110">Agregar vínculos a las nuevas vistas</span><span class="sxs-lookup"><span data-stu-id="53899-110">Add links to new views</span></span>
> * <span data-ttu-id="53899-111">Mostrar las vistas de alumno</span><span class="sxs-lookup"><span data-stu-id="53899-111">Display student views</span></span>
> * <span data-ttu-id="53899-112">Mostrar las vistas de inscripción</span><span class="sxs-lookup"><span data-stu-id="53899-112">Display enrollment views</span></span>

## <a name="prerequisite"></a><span data-ttu-id="53899-113">Requisito previo</span><span class="sxs-lookup"><span data-stu-id="53899-113">Prerequisite</span></span>

* [<span data-ttu-id="53899-114">Crear modelos de datos y aplicaciones de web</span><span class="sxs-lookup"><span data-stu-id="53899-114">Create the web application and data models</span></span>](creating-the-web-application.md)

## <a name="add-scaffold"></a><span data-ttu-id="53899-115">Agregar scaffold</span><span class="sxs-lookup"><span data-stu-id="53899-115">Add scaffold</span></span>

<span data-ttu-id="53899-116">Está listo para generar el código que le proporcionará las operaciones de datos estándar para las clases del modelo.</span><span class="sxs-lookup"><span data-stu-id="53899-116">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="53899-117">Agregue el código mediante la adición de un elemento de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="53899-117">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="53899-118">Hay muchas opciones para el tipo de scaffolding que puede agregar; en este tutorial, las plantillas scaffold incluirá un controlador y vistas que corresponden a los modelos de inscripción y estudiantes que creó en la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="53899-118">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="53899-119">Para mantener la coherencia en el proyecto, agregará el nuevo controlador existente **controladores** carpeta.</span><span class="sxs-lookup"><span data-stu-id="53899-119">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="53899-120">Haga clic en el **controladores** carpeta y seleccione **agregar** > **nuevo elemento de scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="53899-120">Right-click the **Controllers** folder, and select **Add** > **New Scaffolded Item**.</span></span>

<span data-ttu-id="53899-121">Seleccione el **controlador MVC 5 con vistas, usando Entity Framework** opción.</span><span class="sxs-lookup"><span data-stu-id="53899-121">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="53899-122">Esta opción generará el controlador y vistas para actualizar, eliminar, crear y mostrar los datos en el modelo.</span><span class="sxs-lookup"><span data-stu-id="53899-122">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![Agregar controlador de mvc](generating-views/_static/image2.png)

<span data-ttu-id="53899-124">Seleccione **Student (ContosoSite.Models)** para la clase de modelo y seleccione el **ContosoUniversityDataEntities (ContosoSite.Models)** para la clase de contexto.</span><span class="sxs-lookup"><span data-stu-id="53899-124">Select **Student (ContosoSite.Models)** for the model class and select the **ContosoUniversityDataEntities (ContosoSite.Models)** for the context class.</span></span> <span data-ttu-id="53899-125">Mantenga el nombre del controlador como **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="53899-125">Keep the controller name as **StudentsController**.</span></span>

<span data-ttu-id="53899-126">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="53899-126">Click **Add**.</span></span>

<span data-ttu-id="53899-127">Si recibe un error, es posible que no se compiló el proyecto en la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="53899-127">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="53899-128">Si es así, intente compilar el proyecto y, a continuación, vuelva a agregar el elemento con scaffolding.</span><span class="sxs-lookup"><span data-stu-id="53899-128">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="53899-129">Una vez completado el proceso de generación de código, verá un nuevo controlador y vistas en el proyecto **controladores** y **vistas** > **estudiantes** carpetas .</span><span class="sxs-lookup"><span data-stu-id="53899-129">After the code generation process is complete, you will see a new controller and views in your project's **Controllers** and **Views** > **Students** folders.</span></span>


<span data-ttu-id="53899-130">Vuelva a realizar los mismos pasos, pero agregar scaffolding para el **inscripción** clase.</span><span class="sxs-lookup"><span data-stu-id="53899-130">Perform the same steps again, but add a scaffold for the **Enrollment** class.</span></span> <span data-ttu-id="53899-131">Cuando termine, tendrá un **EnrollmentsController.cs** archivo y una carpeta bajo **vistas** denominado **inscripciones** con las vistas de crear, eliminar, detalles, edición e índice.</span><span class="sxs-lookup"><span data-stu-id="53899-131">When finished, you have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

## <a name="add-links-to-new-views"></a><span data-ttu-id="53899-132">Agregar vínculos a las nuevas vistas</span><span class="sxs-lookup"><span data-stu-id="53899-132">Add links to new views</span></span>

<span data-ttu-id="53899-133">Para facilitar la vaya a las nuevas vistas, puede agregar un par de hipervínculos a las vistas de índice para estudiantes y las inscripciones.</span><span class="sxs-lookup"><span data-stu-id="53899-133">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="53899-134">Abra el archivo en **vistas** > **principal** > *Index.cshtml*, que es la página principal de su sitio.</span><span class="sxs-lookup"><span data-stu-id="53899-134">Open the file at **Views** > **Home** > *Index.cshtml*, which is the home page for your site.</span></span> <span data-ttu-id="53899-135">Agregue el código siguiente debajo del jumbotron.</span><span class="sxs-lookup"><span data-stu-id="53899-135">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="53899-136">El método ActionLink, el primer parámetro es el texto que se muestra en el vínculo.</span><span class="sxs-lookup"><span data-stu-id="53899-136">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="53899-137">El segundo parámetro es la acción y el tercer parámetro es el nombre del controlador.</span><span class="sxs-lookup"><span data-stu-id="53899-137">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="53899-138">Por ejemplo, el primer vínculo apunta a la acción del índice en StudentsController.</span><span class="sxs-lookup"><span data-stu-id="53899-138">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="53899-139">El hipervínculo real se construye a partir de estos valores.</span><span class="sxs-lookup"><span data-stu-id="53899-139">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="53899-140">En última instancia, el primer vínculo conduce al usuario a la **Index.cshtml** archivo dentro de la **vistas/estudiantes** carpeta.</span><span class="sxs-lookup"><span data-stu-id="53899-140">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="53899-141">Mostrar las vistas de alumno</span><span class="sxs-lookup"><span data-stu-id="53899-141">Display student views</span></span>

<span data-ttu-id="53899-142">Comprobará que el código que se agregó correctamente a su proyecto muestra una lista de los estudiantes y permite a los usuarios editar, crear o eliminar los registros de alumnos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="53899-142">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="53899-143">Haga clic en el **vistas** > **inicio** > *Index.cshtml* de archivo y seleccione **ver en el explorador**.</span><span class="sxs-lookup"><span data-stu-id="53899-143">Right-click the **Views** > **Home** > *Index.cshtml* file, and select **View in Browser**.</span></span> <span data-ttu-id="53899-144">En la página de inicio de la aplicación, seleccione **lista de alumnos**.</span><span class="sxs-lookup"><span data-stu-id="53899-144">On the application home page, select **List of students**.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="53899-145">En el **índice** página, observe la lista de los estudiantes y vínculos para modificar estos datos.</span><span class="sxs-lookup"><span data-stu-id="53899-145">On the **Index** page, notice the list of the students and links to modify this data.</span></span> <span data-ttu-id="53899-146">Seleccione el **crear nuevo** vincular y proporcione valores para un alumno nuevo.</span><span class="sxs-lookup"><span data-stu-id="53899-146">Select the **Create New** link and provide some values for a new student.</span></span> <span data-ttu-id="53899-147">Haga clic en **crear**y observe el alumno nuevo se agrega a la lista.</span><span class="sxs-lookup"><span data-stu-id="53899-147">Click **Create**, and notice the new student is added to your list.</span></span>

<span data-ttu-id="53899-148">En el **índice** página, seleccione el **editar** vincular y cambiar algunos de los valores de un alumno.</span><span class="sxs-lookup"><span data-stu-id="53899-148">Back on the **Index** page, select the **Edit** link, and change some of the values for a student.</span></span> <span data-ttu-id="53899-149">Haga clic en **guardar**y observe el registro de estudiante ha cambiado.</span><span class="sxs-lookup"><span data-stu-id="53899-149">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="53899-150">Por último, seleccione el **eliminar** vincular y confirmar que desea eliminar el registro, haga clic en el **eliminar** botón.</span><span class="sxs-lookup"><span data-stu-id="53899-150">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

<span data-ttu-id="53899-151">Sin escribir ningún código, ha agregado vistas que realizan operaciones comunes en los datos en la tabla Student.</span><span class="sxs-lookup"><span data-stu-id="53899-151">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="53899-152">Es podrán que haya observado que la etiqueta de texto para un campo se basa en la propiedad de base de datos (como **LastName**) que no es necesariamente lo que desea mostrar en la página web.</span><span class="sxs-lookup"><span data-stu-id="53899-152">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="53899-153">Por ejemplo, quizás prefiera la etiqueta sea **apellido**.</span><span class="sxs-lookup"><span data-stu-id="53899-153">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="53899-154">Corregirá este problema de presentación más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="53899-154">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="53899-155">Mostrar las vistas de inscripción</span><span class="sxs-lookup"><span data-stu-id="53899-155">Display enrollment views</span></span>

<span data-ttu-id="53899-156">La base de datos incluye una relación uno a varios entre las tablas Student e inscripción y una relación uno a varios entre las tablas Course y Enrollment.</span><span class="sxs-lookup"><span data-stu-id="53899-156">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="53899-157">Las vistas para la inscripción de controlan correctamente estas relaciones.</span><span class="sxs-lookup"><span data-stu-id="53899-157">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="53899-158">Vaya a la página principal de su sitio y seleccione el **lista de inscripciones de** vínculo y, a continuación, el **crear nuevo** vínculo.</span><span class="sxs-lookup"><span data-stu-id="53899-158">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span>

<span data-ttu-id="53899-159">La vista muestra un formulario para crear un nuevo registro de inscripción.</span><span class="sxs-lookup"><span data-stu-id="53899-159">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="53899-160">En concreto, tenga en cuenta que el formulario contiene un **CourseID** lista desplegable y un **StudentID** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="53899-160">In particular, notice that the form contains a **CourseID** drop-down list and a **StudentID** drop-down list.</span></span> <span data-ttu-id="53899-161">Ambos se rellenan con valores de las tablas relacionadas.</span><span class="sxs-lookup"><span data-stu-id="53899-161">Both are populated with values from the related tables.</span></span>

<span data-ttu-id="53899-162">Además, la validación de los valores proporcionados se aplica automáticamente en función del tipo de datos del campo.</span><span class="sxs-lookup"><span data-stu-id="53899-162">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="53899-163">**Grado** requiere un número, por lo que se muestra un mensaje de error si intenta proporcionar un valor incompatible: *El nivel de campo debe ser un número.*</span><span class="sxs-lookup"><span data-stu-id="53899-163">**Grade** requires a number, so an error message is displayed if you try to provide an incompatible value: *The field Grade must be a number.*</span></span>

<span data-ttu-id="53899-164">Ha comprobado que las vistas genera automáticamente los usuarios pueden trabajar con los datos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="53899-164">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="53899-165">En el siguiente tutorial de esta serie, va a actualizar la base de datos y haga los cambios correspondientes en la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="53899-165">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="53899-166">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="53899-166">Next steps</span></span>

<span data-ttu-id="53899-167">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="53899-167">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="53899-168">Se ha agregado scaffold</span><span class="sxs-lookup"><span data-stu-id="53899-168">Added scaffold</span></span>
> * <span data-ttu-id="53899-169">Se han agregado vínculos a las nuevas vistas</span><span class="sxs-lookup"><span data-stu-id="53899-169">Added links to new views</span></span>
> * <span data-ttu-id="53899-170">Vistas de alumno mostrados</span><span class="sxs-lookup"><span data-stu-id="53899-170">Displayed student views</span></span>
> * <span data-ttu-id="53899-171">Vistas de inscripción mostrados</span><span class="sxs-lookup"><span data-stu-id="53899-171">Displayed enrollment views</span></span>

<span data-ttu-id="53899-172">En el siguiente tutorial para aprender a cambiar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="53899-172">Advance to the next tutorial to learn how to change the database.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="53899-173">Cambiar la base de datos</span><span class="sxs-lookup"><span data-stu-id="53899-173">Change the database</span></span>](changing-the-database.md)