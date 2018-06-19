---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Base de datos EF primero con ASP.NET MVC: generar vistas | Documentos de Microsoft'
author: tfitzmac
description: Con MVC, Entity Framework y Scaffolding de ASP.NET, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Este tutorial seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: b60e89a187a879255eb051dc87241714cef6fa63
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879756"
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a><span data-ttu-id="57ffb-104">Base de datos EF primero con ASP.NET MVC: generación de vistas</span><span class="sxs-lookup"><span data-stu-id="57ffb-104">EF Database First with ASP.NET MVC: Generating Views</span></span>
====================
<span data-ttu-id="57ffb-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="57ffb-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="57ffb-106">Con MVC, Entity Framework y Scaffolding de ASP.NET, puede crear una aplicación web que proporciona una interfaz a una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="57ffb-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="57ffb-107">Esta serie de tutoriales muestra cómo automáticamente genera código que permite a los usuarios mostrar, editar, crear y eliminar datos que residen en una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="57ffb-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="57ffb-108">El código generado corresponde a las columnas de la tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="57ffb-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="57ffb-109">Esta parte de la serie se centra en mediante Scaffolding de ASP.NET para generar los controladores y vistas.</span><span class="sxs-lookup"><span data-stu-id="57ffb-109">This part of the series focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>


## <a name="add-scaffold"></a><span data-ttu-id="57ffb-110">Agregar scaffolding</span><span class="sxs-lookup"><span data-stu-id="57ffb-110">Add scaffold</span></span>

<span data-ttu-id="57ffb-111">Está listo para generar el código que le proporcionará las operaciones de datos estándar para las clases del modelo.</span><span class="sxs-lookup"><span data-stu-id="57ffb-111">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="57ffb-112">Agregue el código agregando un elemento de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="57ffb-112">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="57ffb-113">Existen muchas opciones para el tipo de scaffolding que puede agregar; en este tutorial, el scaffolding incluirá un controlador y vistas que corresponden a los modelos de Student y Enrollment que creó en la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="57ffb-113">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="57ffb-114">Para mantener la coherencia en el proyecto, agregará el nuevo controlador a la existente **controladores** carpeta.</span><span class="sxs-lookup"><span data-stu-id="57ffb-114">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="57ffb-115">Haga clic en el **controladores** carpeta y seleccione **agregar** : **nuevo elemento de scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="57ffb-115">Right-click the **Controllers** folder, and select **Add** – **New Scaffolded Item**.</span></span>

![Agregar scaffolding](generating-views/_static/image1.png)

<span data-ttu-id="57ffb-117">Seleccione el **controlador de MVC 5 con vistas, mediante Entity Framework** opción.</span><span class="sxs-lookup"><span data-stu-id="57ffb-117">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="57ffb-118">Esta opción generará el controlador y vistas para actualizar, eliminar, crear y mostrar los datos en el modelo.</span><span class="sxs-lookup"><span data-stu-id="57ffb-118">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![Agregar controlador de mvc](generating-views/_static/image2.png)

<span data-ttu-id="57ffb-120">Seleccione **Student** para la clase de modelo y seleccione la **ContosoUniversityEntities** para la clase de contexto.</span><span class="sxs-lookup"><span data-stu-id="57ffb-120">Select **Student** for the model class, and select the **ContosoUniversityEntities** for the context class.</span></span> <span data-ttu-id="57ffb-121">Mantener el nombre del controlador como **StudentsController**,</span><span class="sxs-lookup"><span data-stu-id="57ffb-121">Keep the controller name as **StudentsController**,</span></span>

![especificar controlador](generating-views/_static/image3.png)

<span data-ttu-id="57ffb-123">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="57ffb-123">Click **Add**.</span></span>

<span data-ttu-id="57ffb-124">Si recibe un error, puede deberse a que no se ha generado el proyecto en la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="57ffb-124">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="57ffb-125">Si es así, pruebe a compilar el proyecto y, a continuación, vuelva a agregar el elemento con scaffolding.</span><span class="sxs-lookup"><span data-stu-id="57ffb-125">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="57ffb-126">Una vez completado el proceso de generación de código, verá un nuevo controlador y vistas en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="57ffb-126">After the code generation process is complete, you will see a new controller and views in your project.</span></span>

![Mostrar vistas](generating-views/_static/image4.png)

<span data-ttu-id="57ffb-128">Vuelva a realizar los mismos pasos, pero agregar scaffolding para la clase de inscripción.</span><span class="sxs-lookup"><span data-stu-id="57ffb-128">Perform the same steps again, but add a scaffold for the Enrollment class.</span></span> <span data-ttu-id="57ffb-129">Cuando termine, debe tener un **EnrollmentsController.cs** archivo y una carpeta bajo **vistas** denominado **las inscripciones** con Create, Delete, detalles, editar e índice Vistas.</span><span class="sxs-lookup"><span data-stu-id="57ffb-129">When finished, you should have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

![Mostrar vistas](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a><span data-ttu-id="57ffb-131">Agregar vínculos a las nuevas vistas</span><span class="sxs-lookup"><span data-stu-id="57ffb-131">Add links to new views</span></span>

<span data-ttu-id="57ffb-132">Para facilitar la vaya a sus vistas nuevo, puede agregar un par de hipervínculos a las vistas de índice para estudiantes y las inscripciones.</span><span class="sxs-lookup"><span data-stu-id="57ffb-132">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="57ffb-133">Abra el archivo en **Views/Home/Index.cshtml**, que es la página principal de su sitio.</span><span class="sxs-lookup"><span data-stu-id="57ffb-133">Open the file at **Views/Home/Index.cshtml**, which is the home page for your site.</span></span> <span data-ttu-id="57ffb-134">Agregue el código siguiente debajo del jumbotron.</span><span class="sxs-lookup"><span data-stu-id="57ffb-134">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="57ffb-135">El método ActionLink, el primer parámetro es el texto que se mostrará en el vínculo.</span><span class="sxs-lookup"><span data-stu-id="57ffb-135">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="57ffb-136">El segundo parámetro es la acción y el tercer parámetro es el nombre del controlador.</span><span class="sxs-lookup"><span data-stu-id="57ffb-136">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="57ffb-137">Por ejemplo, el primer vínculo apunta a la acción del índice en StudentsController.</span><span class="sxs-lookup"><span data-stu-id="57ffb-137">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="57ffb-138">El hipervínculo real se construye a partir de estos valores.</span><span class="sxs-lookup"><span data-stu-id="57ffb-138">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="57ffb-139">El primer vínculo lleva en última instancia a los usuarios para el **Index.cshtml** archivo dentro de la **vistas/estudiantes** carpeta.</span><span class="sxs-lookup"><span data-stu-id="57ffb-139">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="57ffb-140">Mostrar las vistas de estudiante</span><span class="sxs-lookup"><span data-stu-id="57ffb-140">Display student views</span></span>

<span data-ttu-id="57ffb-141">Comprobará que el código que se agregó correctamente a su proyecto de muestra una lista de los alumnos y permite a los usuarios editar, crear o eliminar los registros de estudiante en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="57ffb-141">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="57ffb-142">Haga clic en el **Views/Home/Index.cshtml** de archivos y seleccione **ver en el explorador**.</span><span class="sxs-lookup"><span data-stu-id="57ffb-142">Right-click the **Views/Home/Index.cshtml** file, and select **View in Browser**.</span></span> <span data-ttu-id="57ffb-143">En esta página, haga clic en el vínculo de la lista de estudiantes.</span><span class="sxs-lookup"><span data-stu-id="57ffb-143">On this page, click the link for the list of students.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="57ffb-144">En esta página, observe la lista de los estudiantes y vínculos para modificar estos datos.</span><span class="sxs-lookup"><span data-stu-id="57ffb-144">On this page, notice the list of the students and links to modify this data.</span></span>

![lista de estudiantes](generating-views/_static/image7.png)

<span data-ttu-id="57ffb-146">Haga clic en el **crear nuevo** vincular y proporcionar algunos valores para un nuevo alumno.</span><span class="sxs-lookup"><span data-stu-id="57ffb-146">Click the **Create New** link and provide some values for a new student.</span></span>

![Crear nuevo alumno](generating-views/_static/image8.png)

<span data-ttu-id="57ffb-148">Haga clic en **crear**y observe los estudiantes nueva se agregan a la lista.</span><span class="sxs-lookup"><span data-stu-id="57ffb-148">Click **Create**, and notice the new student is added to your list.</span></span>

![lista con nuevo alumno](generating-views/_static/image9.png)

<span data-ttu-id="57ffb-150">Seleccione el **editar** vincular y cambiar algunos de los valores de un estudiante.</span><span class="sxs-lookup"><span data-stu-id="57ffb-150">Select the **Edit** link, and change some of the values for a student.</span></span>

![Editar student](generating-views/_static/image10.png)

<span data-ttu-id="57ffb-152">Haga clic en **guardar**y observe el registro de estudiante ha cambiado.</span><span class="sxs-lookup"><span data-stu-id="57ffb-152">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="57ffb-153">Por último, seleccione la **eliminar** vincular y confirmar que desea eliminar el registro, haga clic en el **eliminar** botón.</span><span class="sxs-lookup"><span data-stu-id="57ffb-153">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

![Eliminar alumno](generating-views/_static/image11.png)

<span data-ttu-id="57ffb-155">Sin escribir ningún código, ha agregado vistas que llevan a cabo operaciones comunes en los datos en la tabla de estudiantes.</span><span class="sxs-lookup"><span data-stu-id="57ffb-155">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="57ffb-156">Es podrán que haya observado que la etiqueta de texto de un campo se basa en la propiedad de base de datos (como **LastName**) que no es necesariamente lo que desea que aparezca en la página web.</span><span class="sxs-lookup"><span data-stu-id="57ffb-156">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="57ffb-157">Por ejemplo, quizás prefiera la etiqueta sea **Last Name**.</span><span class="sxs-lookup"><span data-stu-id="57ffb-157">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="57ffb-158">Corregirá este problema de presentación más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="57ffb-158">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="57ffb-159">Mostrar las vistas de inscripción</span><span class="sxs-lookup"><span data-stu-id="57ffb-159">Display enrollment views</span></span>

<span data-ttu-id="57ffb-160">La base de datos incluye una relación de uno a varios entre las tablas Student y Enrollment y una relación de uno a varios entre las tablas de curso e inscripción.</span><span class="sxs-lookup"><span data-stu-id="57ffb-160">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="57ffb-161">Las vistas para la inscripción de administrar correctamente estas relaciones.</span><span class="sxs-lookup"><span data-stu-id="57ffb-161">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="57ffb-162">Vaya a la página principal de su sitio y seleccione el **lista de las inscripciones** vínculo y, a continuación, el **crear nuevo** vínculo.</span><span class="sxs-lookup"><span data-stu-id="57ffb-162">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span> <span data-ttu-id="57ffb-163">La vista muestra un formulario para crear un nuevo registro de inscripción.</span><span class="sxs-lookup"><span data-stu-id="57ffb-163">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="57ffb-164">En concreto, tenga en cuenta que el formulario contiene dos listas desplegables que se rellenan con los valores de las tablas relacionadas.</span><span class="sxs-lookup"><span data-stu-id="57ffb-164">In particular, notice that the form contains two drop-down lists that are populated with values from the related tables.</span></span>

![crear la inscripción](generating-views/_static/image12.png)

<span data-ttu-id="57ffb-166">Además, la validación de los valores proporcionados se aplica automáticamente en función del tipo de datos del campo.</span><span class="sxs-lookup"><span data-stu-id="57ffb-166">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="57ffb-167">Grado de requiere un número, por lo que se muestra un mensaje de error si intenta proporcionar un valor incompatible.</span><span class="sxs-lookup"><span data-stu-id="57ffb-167">Grade requires a number, so an error message is displayed if you try to provide an incompatible value.</span></span>

![mensaje de validación](generating-views/_static/image13.png)

<span data-ttu-id="57ffb-169">Ha comprobado que las vistas generado automáticamente que los usuarios puedan trabajar con los datos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="57ffb-169">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="57ffb-170">En el siguiente tutorial de esta serie, se actualizará la base de datos y realice los cambios correspondientes en la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="57ffb-170">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="57ffb-171">[Anterior](creating-the-web-application.md)
> [Siguiente](changing-the-database.md)</span><span class="sxs-lookup"><span data-stu-id="57ffb-171">[Previous](creating-the-web-application.md)
[Next](changing-the-database.md)</span></span>
