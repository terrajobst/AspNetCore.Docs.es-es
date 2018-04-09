---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Base de datos EF primero con ASP.NET MVC: creación de la aplicación Web y los modelos de datos | Documentos de Microsoft'
author: tfitzmac
description: Con MVC, Entity Framework y Scaffolding de ASP.NET, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Este tutorial seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 04ccc00fa48702608fdc7b5b00d73778985852f9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a><span data-ttu-id="d58ef-104">Base de datos EF primero con ASP.NET MVC: creación de la aplicación Web y los modelos de datos</span><span class="sxs-lookup"><span data-stu-id="d58ef-104">EF Database First with ASP.NET MVC: Creating the Web Application and Data Models</span></span>
====================
<span data-ttu-id="d58ef-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d58ef-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d58ef-106">Con MVC, Entity Framework y Scaffolding de ASP.NET, puede crear una aplicación web que proporciona una interfaz a una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="d58ef-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="d58ef-107">Esta serie de tutoriales muestra cómo automáticamente genera código que permite a los usuarios mostrar, editar, crear y eliminar datos que residen en una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="d58ef-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="d58ef-108">El código generado corresponde a las columnas de la tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="d58ef-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="d58ef-109">Esta parte de la serie se centra en crear la aplicación web y generar los modelos de datos basados en las tablas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="d58ef-109">This part of the series focuses on creating the web application, and generating the data models based on your database tables.</span></span>


## <a name="create-a-new-aspnet-web-application"></a><span data-ttu-id="d58ef-110">Cree una nueva aplicación Web de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d58ef-110">Create a new ASP.NET Web Application</span></span>

<span data-ttu-id="d58ef-111">En una nueva solución o en la misma solución que el proyecto de base de datos, cree un nuevo proyecto en Visual Studio y seleccione la **aplicación Web ASP.NET** plantilla.</span><span class="sxs-lookup"><span data-stu-id="d58ef-111">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="d58ef-112">Denomine el proyecto **ContosoSite**.</span><span class="sxs-lookup"><span data-stu-id="d58ef-112">Name the project **ContosoSite**.</span></span>

![Crear proyecto](creating-the-web-application/_static/image1.png)

<span data-ttu-id="d58ef-114">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="d58ef-114">Click **OK**.</span></span>

<span data-ttu-id="d58ef-115">En la ventana nuevo proyecto ASP.NET, seleccione la **MVC** plantilla.</span><span class="sxs-lookup"><span data-stu-id="d58ef-115">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="d58ef-116">Puede desactivar la **Host en la nube** opción por ahora porque va a implementar la aplicación a la nube.</span><span class="sxs-lookup"><span data-stu-id="d58ef-116">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="d58ef-117">Haga clic en **Aceptar** para crear la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d58ef-117">Click **OK** to create the application.</span></span>

![Seleccione la plantilla de mvc](creating-the-web-application/_static/image2.png)

<span data-ttu-id="d58ef-119">El proyecto se crea con los archivos y carpetas predeterminados.</span><span class="sxs-lookup"><span data-stu-id="d58ef-119">The project is created with the default files and folders.</span></span>

<span data-ttu-id="d58ef-120">En este tutorial, utilizará Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="d58ef-120">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="d58ef-121">Puede comprobar la versión de Entity Framework en el proyecto con la ventana Administrar paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="d58ef-121">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="d58ef-122">Si es necesario, actualice su versión de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d58ef-122">If necessary, update your version of Entity Framework.</span></span>

![Mostrar versión](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="d58ef-124">Generar los modelos</span><span class="sxs-lookup"><span data-stu-id="d58ef-124">Generate the models</span></span>

<span data-ttu-id="d58ef-125">Ahora va a crear modelos de Entity Framework desde las tablas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="d58ef-125">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="d58ef-126">Estos modelos son clases que se va a usar para trabajar con los datos.</span><span class="sxs-lookup"><span data-stu-id="d58ef-126">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="d58ef-127">Cada modelo refleja una tabla en la base de datos y contiene propiedades que corresponden a las columnas de la tabla.</span><span class="sxs-lookup"><span data-stu-id="d58ef-127">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="d58ef-128">Haga clic en el **modelos** carpeta y seleccione **agregar** y **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="d58ef-128">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

![Agregar nuevo elemento](creating-the-web-application/_static/image4.png)

<span data-ttu-id="d58ef-130">En la ventana Agregar nuevo elemento, seleccione **datos** en el panel izquierdo y **ADO.NET Entity Data Model** entre las opciones en el panel central.</span><span class="sxs-lookup"><span data-stu-id="d58ef-130">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="d58ef-131">Asigne al nuevo archivo de modelo **ContosoModel**.</span><span class="sxs-lookup"><span data-stu-id="d58ef-131">Name the new model file **ContosoModel**.</span></span>

![Crear modelo](creating-the-web-application/_static/image5.png)

<span data-ttu-id="d58ef-133">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="d58ef-133">Click **Add**.</span></span>

<span data-ttu-id="d58ef-134">En el Asistente de Entity Data Model, seleccione **EF Designer de base de datos**.</span><span class="sxs-lookup"><span data-stu-id="d58ef-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

![generar a partir de la base de datos](creating-the-web-application/_static/image6.png)

<span data-ttu-id="d58ef-136">Haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="d58ef-136">Click **Next**.</span></span>

<span data-ttu-id="d58ef-137">Si tiene conexiones de base de datos definidas en el entorno de desarrollo, verá una de estas conexiones ya seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="d58ef-137">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="d58ef-138">Sin embargo, desea crear una nueva conexión con la base de datos que creó en la primera parte de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="d58ef-138">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="d58ef-139">Haga clic en el **nueva conexión** botón.</span><span class="sxs-lookup"><span data-stu-id="d58ef-139">Click the **New Connection** button.</span></span>

![Conectarse a la base de datos](creating-the-web-application/_static/image7.png)

<span data-ttu-id="d58ef-141">En la ventana Propiedades de conexión, proporcione el nombre del servidor local donde se creó la base de datos (en este caso **\ProjectsV12 (localdb)**).</span><span class="sxs-lookup"><span data-stu-id="d58ef-141">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\ProjectsV12**).</span></span> <span data-ttu-id="d58ef-142">Después de proporcionar el nombre del servidor, seleccione la ContosoUniversityData de las bases de datos disponibles.</span><span class="sxs-lookup"><span data-stu-id="d58ef-142">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![establecer propiedades de conexión](creating-the-web-application/_static/image8.png)

<span data-ttu-id="d58ef-144">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="d58ef-144">Click **OK**.</span></span>

<span data-ttu-id="d58ef-145">Ahora se muestran las propiedades de conexión correcto.</span><span class="sxs-lookup"><span data-stu-id="d58ef-145">The correct connection properties are now displayed.</span></span> <span data-ttu-id="d58ef-146">Puede usar el nombre predeterminado para la conexión en el archivo Web.Config</span><span class="sxs-lookup"><span data-stu-id="d58ef-146">You can use the default name for connection in the Web.Config file</span></span>

![configuración de conexión](creating-the-web-application/_static/image9.png)

<span data-ttu-id="d58ef-148">Haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="d58ef-148">Click **Next**.</span></span>

<span data-ttu-id="d58ef-149">Seleccione **tablas** para generar modelos para las tres tablas.</span><span class="sxs-lookup"><span data-stu-id="d58ef-149">Select **Tables** to generate models for all three tables.</span></span>

![Seleccionar tablas](creating-the-web-application/_static/image10.png)

<span data-ttu-id="d58ef-151">Haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="d58ef-151">Click **Finish**.</span></span>

<span data-ttu-id="d58ef-152">Si recibe una advertencia de seguridad, seleccione **Aceptar** para continuar la ejecución de la plantilla.</span><span class="sxs-lookup"><span data-stu-id="d58ef-152">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="d58ef-153">Los modelos se generan a partir de las tablas de base de datos y se muestra un diagrama que muestra las propiedades y relaciones entre las tablas.</span><span class="sxs-lookup"><span data-stu-id="d58ef-153">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![diagrama del modelo](creating-the-web-application/_static/image11.png)

<span data-ttu-id="d58ef-155">La carpeta Models ahora incluye muchos nuevos archivos relacionados con los modelos que se generaron desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d58ef-155">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

![Mostrar los nuevos archivos de modelo](creating-the-web-application/_static/image12.png)

<span data-ttu-id="d58ef-157">El **ContosoModel.Context.cs** archivo contiene una clase que deriva de la **DbContext** clase y proporciona una propiedad para cada clase de modelo que corresponde a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="d58ef-157">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="d58ef-158">El **Course.cs**, **Enrollment.cs**, y **Student.cs** archivos contienen las clases del modelo que representan todas las tablas de bases de datos.</span><span class="sxs-lookup"><span data-stu-id="d58ef-158">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="d58ef-159">La clase de contexto y las clases del modelo se usará cuando se trabaja con la técnica scaffolding.</span><span class="sxs-lookup"><span data-stu-id="d58ef-159">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="d58ef-160">Antes de continuar con este tutorial, compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="d58ef-160">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="d58ef-161">En la siguiente sección, generará código basado en los modelos de datos, pero esa sección no funcionará si no se ha generado el proyecto.</span><span class="sxs-lookup"><span data-stu-id="d58ef-161">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d58ef-162">[Anterior](setting-up-database.md)
> [Siguiente](generating-views.md)</span><span class="sxs-lookup"><span data-stu-id="d58ef-162">[Previous](setting-up-database.md)
[Next](generating-views.md)</span></span>
