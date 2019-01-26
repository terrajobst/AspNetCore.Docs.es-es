---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Tutorial: Crear la base de datos de la aplicación Web y los modelos de datos para EF primero con ASP.NET MVC'
description: En este artículo se centra en crear la aplicación web y generar los modelos de datos basados en las tablas de base de datos.
author: Rick-Anderson
ms.author: riande
ms.date: 01/23/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 095d355866c9ab8fba3759f3e05e2a521992f3d6
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889774"
---
# <a name="tutorial-create-the-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a><span data-ttu-id="ead8d-103">Tutorial: Crear la base de datos de la aplicación Web y los modelos de datos para EF primero con ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="ead8d-103">Tutorial: Create the the Web Application and Data Models for EF Database First with ASP.NET MVC</span></span>

 <span data-ttu-id="ead8d-104">Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="ead8d-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="ead8d-105">Esta serie de tutoriales muestra cómo generar el código que permite a los usuarios mostrar, editar, crear automáticamente y eliminar datos que residen en una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="ead8d-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="ead8d-106">El código generado corresponde a las columnas de la tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="ead8d-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="ead8d-107">En este artículo se centra en crear la aplicación web y generar los modelos de datos basados en las tablas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="ead8d-107">This article focuses on creating the web application, and generating the data models based on your database tables.</span></span>

<span data-ttu-id="ead8d-108">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="ead8d-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ead8d-109">Crear una aplicación web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ead8d-109">Create an ASP.NET web app</span></span>
> * <span data-ttu-id="ead8d-110">Generar los modelos</span><span class="sxs-lookup"><span data-stu-id="ead8d-110">Generate the models</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ead8d-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="ead8d-111">Prerequisites</span></span>

* [<span data-ttu-id="ead8d-112">Introducción a Entity Framework 6 Database First con MVC 5</span><span class="sxs-lookup"><span data-stu-id="ead8d-112">Getting started with Entity Framework 6 Database First using MVC 5</span></span>](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a><span data-ttu-id="ead8d-113">Crear una aplicación web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ead8d-113">Create an ASP.NET web app</span></span>

<span data-ttu-id="ead8d-114">En una nueva solución o en la misma solución que el proyecto de base de datos, cree un nuevo proyecto en Visual Studio y seleccione el **aplicación Web ASP.NET** plantilla.</span><span class="sxs-lookup"><span data-stu-id="ead8d-114">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="ead8d-115">Denomine el proyecto **ContosoSite**.</span><span class="sxs-lookup"><span data-stu-id="ead8d-115">Name the project **ContosoSite**.</span></span>

![Crear proyecto](creating-the-web-application/_static/image1.png)

<span data-ttu-id="ead8d-117">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ead8d-117">Click **OK**.</span></span>

<span data-ttu-id="ead8d-118">En la ventana nuevo proyecto ASP.NET, seleccione la **MVC** plantilla.</span><span class="sxs-lookup"><span data-stu-id="ead8d-118">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="ead8d-119">Puede borrar el **Host en la nube** opción por ahora, ya que se implementará la aplicación en la nube.</span><span class="sxs-lookup"><span data-stu-id="ead8d-119">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="ead8d-120">Haga clic en **Aceptar** para crear la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ead8d-120">Click **OK** to create the application.</span></span>

<span data-ttu-id="ead8d-121">El proyecto se crea con los archivos predeterminados y las carpetas.</span><span class="sxs-lookup"><span data-stu-id="ead8d-121">The project is created with the default files and folders.</span></span>

<span data-ttu-id="ead8d-122">En este tutorial, usará Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="ead8d-122">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="ead8d-123">Puede comprobar la versión de Entity Framework en el proyecto a través de la ventana Administrar paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="ead8d-123">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="ead8d-124">Si es necesario, actualice su versión de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ead8d-124">If necessary, update your version of Entity Framework.</span></span>

![Mostrar la versión](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="ead8d-126">Generar los modelos</span><span class="sxs-lookup"><span data-stu-id="ead8d-126">Generate the models</span></span>

<span data-ttu-id="ead8d-127">Ahora va a crear modelos de Entity Framework desde las tablas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="ead8d-127">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="ead8d-128">Estos modelos son clases que se va a utilizar para trabajar con los datos.</span><span class="sxs-lookup"><span data-stu-id="ead8d-128">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="ead8d-129">Cada modelo refleja una tabla en la base de datos y contiene propiedades que corresponden a las columnas de la tabla.</span><span class="sxs-lookup"><span data-stu-id="ead8d-129">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="ead8d-130">Haga clic en el **modelos** carpeta y seleccione **agregar** y **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="ead8d-130">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

<span data-ttu-id="ead8d-131">En la ventana Agregar nuevo elemento, seleccione **datos** en el panel izquierdo y **ADO.NET Entity Data Model** entre las opciones en el panel central.</span><span class="sxs-lookup"><span data-stu-id="ead8d-131">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="ead8d-132">Asigne al nuevo archivo de modelo **ContosoModel**.</span><span class="sxs-lookup"><span data-stu-id="ead8d-132">Name the new model file **ContosoModel**.</span></span>

<span data-ttu-id="ead8d-133">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="ead8d-133">Click **Add**.</span></span>

<span data-ttu-id="ead8d-134">En el Asistente de Entity Data Model, seleccione **EF Designer de base de datos**.</span><span class="sxs-lookup"><span data-stu-id="ead8d-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

<span data-ttu-id="ead8d-135">Haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="ead8d-135">Click **Next**.</span></span>

<span data-ttu-id="ead8d-136">Si tiene conexiones de base de datos definidas dentro de su entorno de desarrollo, es posible que vea una de estas conexiones previamente seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="ead8d-136">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="ead8d-137">Sin embargo, desea crear una nueva conexión a la base de datos que creó en la primera parte de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="ead8d-137">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="ead8d-138">Haga clic en el **nueva conexión** botón.</span><span class="sxs-lookup"><span data-stu-id="ead8d-138">Click the **New Connection** button.</span></span>

<span data-ttu-id="ead8d-139">En la ventana Propiedades de conexión, proporcione el nombre del servidor local donde se creó la base de datos (en este caso **(localdb) \Projects13**).</span><span class="sxs-lookup"><span data-stu-id="ead8d-139">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\Projects13**).</span></span> <span data-ttu-id="ead8d-140">Después de proporcionar el nombre del servidor, seleccione el ContosoUniversityData de las bases de datos disponibles.</span><span class="sxs-lookup"><span data-stu-id="ead8d-140">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![establecer propiedades de conexión](creating-the-web-application/_static/image8.png)

<span data-ttu-id="ead8d-142">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ead8d-142">Click **OK**.</span></span>

<span data-ttu-id="ead8d-143">Ahora se muestran las propiedades de conexión correcto.</span><span class="sxs-lookup"><span data-stu-id="ead8d-143">The correct connection properties are now displayed.</span></span> <span data-ttu-id="ead8d-144">Puede usar el nombre predeterminado para la conexión en el archivo Web.Config.</span><span class="sxs-lookup"><span data-stu-id="ead8d-144">You can use the default name for connection in the Web.Config file.</span></span>

<span data-ttu-id="ead8d-145">Haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="ead8d-145">Click **Next**.</span></span>

<span data-ttu-id="ead8d-146">Seleccione la versión más reciente de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ead8d-146">Select the latest version of Entity Framework.</span></span>

<span data-ttu-id="ead8d-147">Haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="ead8d-147">Click **Next**.</span></span>

<span data-ttu-id="ead8d-148">Seleccione **tablas** para generar modelos para las tres tablas.</span><span class="sxs-lookup"><span data-stu-id="ead8d-148">Select **Tables** to generate models for all three tables.</span></span>

<span data-ttu-id="ead8d-149">Haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="ead8d-149">Click **Finish**.</span></span>

<span data-ttu-id="ead8d-150">Si recibes una advertencia de seguridad, seleccione **Aceptar** para continuar la ejecución de la plantilla.</span><span class="sxs-lookup"><span data-stu-id="ead8d-150">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="ead8d-151">Los modelos se generan a partir de las tablas de base de datos y se muestra un diagrama que muestra las propiedades y relaciones entre las tablas.</span><span class="sxs-lookup"><span data-stu-id="ead8d-151">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![diagrama del modelo](creating-the-web-application/_static/image11.png)

<span data-ttu-id="ead8d-153">La carpeta Models ahora incluye muchos nuevos archivos relacionados con los modelos que se generaron a partir de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="ead8d-153">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

<span data-ttu-id="ead8d-154">El **ContosoModel.Context.cs** archivo contiene una clase que deriva el **DbContext** clase y proporciona una propiedad para cada clase de modelo que corresponde a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="ead8d-154">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="ead8d-155">El **Course.cs**, **Enrollment.cs**, y **Student.cs** archivos contienen las clases de modelo que representan las tablas de bases de datos.</span><span class="sxs-lookup"><span data-stu-id="ead8d-155">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="ead8d-156">La clase de contexto y las clases del modelo se usará cuando se trabaja con la técnica scaffolding.</span><span class="sxs-lookup"><span data-stu-id="ead8d-156">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="ead8d-157">Antes de continuar con este tutorial, compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="ead8d-157">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="ead8d-158">En la siguiente sección, generará código basado en los modelos de datos, pero esa sección no funcionará si no se ha compilado el proyecto.</span><span class="sxs-lookup"><span data-stu-id="ead8d-158">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ead8d-159">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="ead8d-159">Next steps</span></span>

<span data-ttu-id="ead8d-160">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="ead8d-160">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ead8d-161">Crea una aplicación web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ead8d-161">Created an ASP.NET web app</span></span>
> * <span data-ttu-id="ead8d-162">Genera los modelos</span><span class="sxs-lookup"><span data-stu-id="ead8d-162">Generated the models</span></span>

<span data-ttu-id="ead8d-163">Avance al siguiente artículo para obtener información sobre cómo crear generar código basado en los modelos de datos.</span><span class="sxs-lookup"><span data-stu-id="ead8d-163">Advance to the next article to learn how to create generate code based on the data models.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="ead8d-164">Generación de vistas</span><span class="sxs-lookup"><span data-stu-id="ead8d-164">Generating views</span></span>](generating-views.md)