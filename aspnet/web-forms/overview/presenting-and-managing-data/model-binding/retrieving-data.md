---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Recuperar y mostrar datos con formularios web y el enlace de modelos | Microsoft Docs
author: Rick-Anderson
description: Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos permite interactuar con los datos más sencilla de...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: c53c27f4852eab9813bd917315111e7cd3b04953
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396290"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a><span data-ttu-id="cba28-104">Recuperar y mostrar datos con enlace de modelos y formularios web forms</span><span class="sxs-lookup"><span data-stu-id="cba28-104">Retrieving and displaying data with model binding and web forms</span></span>
====================

> <span data-ttu-id="cba28-105">Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cba28-105">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="cba28-106">Enlace de modelos permite interactuar con los datos más sencilla de tratar con datos de objetos de origen (como ObjectDataSource o SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="cba28-106">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="cba28-107">Esta serie comienza con material introductorio y mueve a conceptos más avanzados en los tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="cba28-107">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
>  <span data-ttu-id="cba28-108">El patrón de enlace modelo funciona con cualquier tecnología de acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="cba28-108">The model binding pattern works with any data access technology.</span></span> <span data-ttu-id="cba28-109">En este tutorial, va a usar Entity Framework, pero podría usar la tecnología de acceso a datos que le resulte más familiar.</span><span class="sxs-lookup"><span data-stu-id="cba28-109">In this tutorial, you will use Entity Framework, but you could use the data access technology that is most familiar to you.</span></span> <span data-ttu-id="cba28-110">Desde un control de servidor enlazado a datos, como un control ListView, GridView, DetailsView o FormView, especifique los nombres de los métodos que se usará para seleccionar, actualizar, eliminar y crear datos.</span><span class="sxs-lookup"><span data-stu-id="cba28-110">From a data-bound server control, such as a GridView, ListView, DetailsView, or FormView control, you specify the names of the methods to use for selecting, updating, deleting, and creating data.</span></span> <span data-ttu-id="cba28-111">En este tutorial, especificará un valor para el SelectMethod.</span><span class="sxs-lookup"><span data-stu-id="cba28-111">In this tutorial, you will specify a value for the SelectMethod.</span></span> 
> 
> <span data-ttu-id="cba28-112">Dentro de ese método, proporcionan la lógica para recuperar los datos.</span><span class="sxs-lookup"><span data-stu-id="cba28-112">Within that method, you provide the logic for retrieving the data.</span></span> <span data-ttu-id="cba28-113">En el siguiente tutorial, establecerá los valores para UpdateMethod, InsertMethod y la DeleteMethod.</span><span class="sxs-lookup"><span data-stu-id="cba28-113">In the next tutorial, you will set values for UpdateMethod, DeleteMethod and InsertMethod.</span></span>
>
> <span data-ttu-id="cba28-114">También puede [descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="cba28-114">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or Visual Basic.</span></span> <span data-ttu-id="cba28-115">El código descargable funciona con Visual Studio 2012 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="cba28-115">The downloadable code works with Visual Studio 2012 and later.</span></span> <span data-ttu-id="cba28-116">Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2017 que se muestra en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="cba28-116">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2017 template shown in this tutorial.</span></span>
> 
> <span data-ttu-id="cba28-117">En el tutorial para ejecutar la aplicación en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cba28-117">In the tutorial you run the application in Visual Studio.</span></span> <span data-ttu-id="cba28-118">También puede implementar la aplicación en un proveedor de hospedaje y que esté disponible a través de internet.</span><span class="sxs-lookup"><span data-stu-id="cba28-118">You can also deploy the application to a hosting provider and make it available over the internet.</span></span> <span data-ttu-id="cba28-119">Microsoft ofrece hospedaje web gratuito para hasta 10 sitios web en un</span><span class="sxs-lookup"><span data-stu-id="cba28-119">Microsoft offers free web hosting for up to 10 web sites in a</span></span>  
>  <span data-ttu-id="cba28-120">[cuenta de evaluación de Azure gratuita](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="cba28-120">[free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="cba28-121">Para obtener información acerca de cómo implementar un proyecto web de Visual Studio en Azure App Service Web Apps, consulte el [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) serie.</span><span class="sxs-lookup"><span data-stu-id="cba28-121">For information about how to deploy a Visual Studio web project to Azure App Service Web Apps, see the [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) series.</span></span> <span data-ttu-id="cba28-122">Este tutorial también muestra cómo usar migraciones de Entity Framework Code First para implementar la base de datos de SQL Server en Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="cba28-122">That tutorial also shows how to use Entity Framework Code First Migrations to deploy your SQL Server database to Azure SQL Database.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="cba28-123">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="cba28-123">Software versions used in the tutorial</span></span>
> 
> - <span data-ttu-id="cba28-124">Microsoft Visual Studio 2017 o Microsoft Visual Studio Community 2017</span><span class="sxs-lookup"><span data-stu-id="cba28-124">Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017</span></span>
>   
> <span data-ttu-id="cba28-125">Este tutorial también funciona con Visual Studio 2012 y Visual Studio 2013, pero hay algunas diferencias en la plantilla de proyecto y la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="cba28-125">This tutorial also works with Visual Studio 2012 and Visual Studio 2013, but there are some differences in the user interface and project template.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="cba28-126">¿Qué va a crear</span><span class="sxs-lookup"><span data-stu-id="cba28-126">What you'll build</span></span>

<span data-ttu-id="cba28-127">En este tutorial, necesitará:</span><span class="sxs-lookup"><span data-stu-id="cba28-127">In this tutorial, you'll:</span></span>

* <span data-ttu-id="cba28-128">Generar objetos de datos que reflejan una universidad con los estudiantes matriculados en cursos</span><span class="sxs-lookup"><span data-stu-id="cba28-128">Build data objects that reflect a university with students enrolled in courses</span></span>
* <span data-ttu-id="cba28-129">Crear tablas de base de datos de los objetos</span><span class="sxs-lookup"><span data-stu-id="cba28-129">Build database tables from the objects</span></span>
* <span data-ttu-id="cba28-130">Rellenar la base de datos con datos de prueba</span><span class="sxs-lookup"><span data-stu-id="cba28-130">Populate the database with test data</span></span>
* <span data-ttu-id="cba28-131">Mostrar datos en un formulario web Forms</span><span class="sxs-lookup"><span data-stu-id="cba28-131">Display data in a web form</span></span>

## <a name="create-the-project"></a><span data-ttu-id="cba28-132">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="cba28-132">Create the project</span></span>

1. <span data-ttu-id="cba28-133">En Visual Studio 2017, cree un **aplicación Web ASP.NET (.NET Framework)** proyecto denominado **ContosoUniversityModelBinding**.</span><span class="sxs-lookup"><span data-stu-id="cba28-133">In Visual Studio 2017, create a **ASP.NET Web Application (.NET Framework)** project called **ContosoUniversityModelBinding**.</span></span>

   ![Crear proyecto](retrieving-data/_static/image19.png)

2. <span data-ttu-id="cba28-135">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="cba28-135">Select **OK**.</span></span> <span data-ttu-id="cba28-136">Aparece el cuadro de diálogo para seleccionar una plantilla.</span><span class="sxs-lookup"><span data-stu-id="cba28-136">The dialog box to select a template appears.</span></span>

   ![Seleccione los formularios web forms](retrieving-data/_static/image3.png)

3. <span data-ttu-id="cba28-138">Seleccione el **formularios Web Forms** plantilla.</span><span class="sxs-lookup"><span data-stu-id="cba28-138">Select the **Web Forms** template.</span></span> 

4. <span data-ttu-id="cba28-139">Si es necesario, cambie a la autenticación **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="cba28-139">If necessary, change the authentication to **Individual User Accounts**.</span></span> 

5. <span data-ttu-id="cba28-140">Seleccione **Aceptar** para crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="cba28-140">Select **OK** to create the project.</span></span>

## <a name="modify-site-appearance"></a><span data-ttu-id="cba28-141">Modificar la apariencia del sitio</span><span class="sxs-lookup"><span data-stu-id="cba28-141">Modify site appearance</span></span>

   <span data-ttu-id="cba28-142">Realizar algunos cambios para personalizar la apariencia del sitio.</span><span class="sxs-lookup"><span data-stu-id="cba28-142">Make a few changes to customize site appearance.</span></span> 
   
   1. <span data-ttu-id="cba28-143">Abra el archivo Site.Master.</span><span class="sxs-lookup"><span data-stu-id="cba28-143">Open the Site.Master file.</span></span>
   
   2. <span data-ttu-id="cba28-144">Cambie el título para mostrar **Contoso University** y no **My ASP.NET Application**.</span><span class="sxs-lookup"><span data-stu-id="cba28-144">Change the title to display **Contoso University** and not **My ASP.NET Application**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. <span data-ttu-id="cba28-145">Cambiar el texto del encabezado de **nombre de la aplicación** a **Contoso University**.</span><span class="sxs-lookup"><span data-stu-id="cba28-145">Change the header text from **Application name** to **Contoso University**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. <span data-ttu-id="cba28-146">Cambie los vínculos del encabezado de navegación para las columnas adecuadas del sitio.</span><span class="sxs-lookup"><span data-stu-id="cba28-146">Change the navigation header links to site appropriate ones.</span></span> 
   
      <span data-ttu-id="cba28-147">Quitar los vínculos para **sobre** y **póngase en contacto con** y, en su lugar, vincular a un **estudiantes** página, que se va a crear.</span><span class="sxs-lookup"><span data-stu-id="cba28-147">Remove the links for **About** and **Contact** and, instead, link to a **Students** page, which you will create.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. <span data-ttu-id="cba28-148">Guardar Site.Master.</span><span class="sxs-lookup"><span data-stu-id="cba28-148">Save Site.Master.</span></span>

## <a name="add-a-web-form-to-display-student-data"></a><span data-ttu-id="cba28-149">Agregue un formulario web para mostrar datos de estudiante</span><span class="sxs-lookup"><span data-stu-id="cba28-149">Add a web form to display student data</span></span>

   1. <span data-ttu-id="cba28-150">En **el Explorador de soluciones**, haga clic en el proyecto, seleccione **agregar** y, a continuación, **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="cba28-150">In **Solution Explorer**, right-click your project, select **Add** and then **New Item**.</span></span> 
   
   2. <span data-ttu-id="cba28-151">En el **Agregar nuevo elemento** cuadro de diálogo, seleccione el **formulario Web Forms con página maestra** plantilla y asígnele el nombre **Students.aspx**.</span><span class="sxs-lookup"><span data-stu-id="cba28-151">In the **Add New Item** dialog box, select the **Web Form with Master Page** template and name it **Students.aspx**.</span></span>

      ![Crear página](retrieving-data/_static/image5.png)

   3. <span data-ttu-id="cba28-153">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="cba28-153">Select **Add**.</span></span>
   
   4. <span data-ttu-id="cba28-154">Página principal del formulario web Forms, seleccione **Site.Master**.</span><span class="sxs-lookup"><span data-stu-id="cba28-154">For the web form's master page, select **Site.Master**.</span></span>
   
   5. <span data-ttu-id="cba28-155">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="cba28-155">Select **OK**.</span></span>
   

## <a name="add-the-data-model"></a><span data-ttu-id="cba28-156">Agregar el modelo de datos</span><span class="sxs-lookup"><span data-stu-id="cba28-156">Add the data model</span></span>

<span data-ttu-id="cba28-157">En el **modelos** carpeta, agregue una clase denominada **UniversityModels.cs**.</span><span class="sxs-lookup"><span data-stu-id="cba28-157">In the **Models** folder, add a class named **UniversityModels.cs**.</span></span>

   1. <span data-ttu-id="cba28-158">Haga clic en **modelos**, seleccione **agregar**y, a continuación, **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="cba28-158">Right-click **Models**, select **Add**, and then **New Item**.</span></span> <span data-ttu-id="cba28-159">Aparecerá el cuadro de diálogo **Agregar nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="cba28-159">The **Add New Item** dialog box appears.</span></span>

   2. <span data-ttu-id="cba28-160">En el menú de navegación izquierdo, seleccione **código**, a continuación, **clase**.</span><span class="sxs-lookup"><span data-stu-id="cba28-160">From the left navigation menu, select **Code**, then **Class**.</span></span>

      ![Crear clase de modelo](retrieving-data/_static/image20.png)

   3. <span data-ttu-id="cba28-162">Nombre de la clase **UniversityModels.cs** y seleccione **agregar**.</span><span class="sxs-lookup"><span data-stu-id="cba28-162">Name the class **UniversityModels.cs** and select **Add**.</span></span>

      <span data-ttu-id="cba28-163">En este archivo, defina el `SchoolContext`, `Student`, `Enrollment`, y `Course` clases como sigue:</span><span class="sxs-lookup"><span data-stu-id="cba28-163">In this file, define the `SchoolContext`, `Student`, `Enrollment`, and `Course` classes as follows:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      <span data-ttu-id="cba28-164">El `SchoolContext` clase se deriva de `DbContext`, que administra la conexión de base de datos y los cambios en los datos.</span><span class="sxs-lookup"><span data-stu-id="cba28-164">The `SchoolContext` class derives from `DbContext`, which manages the database connection and changes in the data.</span></span>

      <span data-ttu-id="cba28-165">En el `Student` class, tenga en cuenta los atributos aplicados a la `FirstName`, `LastName`, y `Year` propiedades.</span><span class="sxs-lookup"><span data-stu-id="cba28-165">In the `Student` class, notice the attributes applied to the `FirstName`, `LastName`, and `Year` properties.</span></span> <span data-ttu-id="cba28-166">Este tutorial usa estos atributos para la validación de datos.</span><span class="sxs-lookup"><span data-stu-id="cba28-166">This tutorial uses these attributes for data validation.</span></span> <span data-ttu-id="cba28-167">Para simplificar el código, estas propiedades se marcan con atributos de validación de datos.</span><span class="sxs-lookup"><span data-stu-id="cba28-167">To simplify the code, only these properties are marked with data-validation attributes.</span></span> <span data-ttu-id="cba28-168">En un proyecto real, los atributos de validación se aplicaría a todas las propiedades que necesitan validación.</span><span class="sxs-lookup"><span data-stu-id="cba28-168">In a real project, you would apply validation attributes to all properties needing validation.</span></span>

   4. <span data-ttu-id="cba28-169">Save UniversityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="cba28-169">Save UniversityModels.cs.</span></span>

## <a name="set-up-the-database-based-on-classes"></a><span data-ttu-id="cba28-170">Configurar la base de datos basado en clases</span><span class="sxs-lookup"><span data-stu-id="cba28-170">Set up the database based on classes</span></span>

<span data-ttu-id="cba28-171">Este tutorial se usa [migraciones de Code First](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/) para crear objetos y las tablas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="cba28-171">This tutorial uses [Code First Migrations](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/) to create objects and database tables.</span></span> <span data-ttu-id="cba28-172">Estas tablas almacenan información acerca de los alumnos y sus cursos.</span><span class="sxs-lookup"><span data-stu-id="cba28-172">These tables store information about the students and their courses.</span></span>

   1. <span data-ttu-id="cba28-173">Seleccione **herramientas** > **Administrador de paquetes de NuGet** > **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="cba28-173">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

   2. <span data-ttu-id="cba28-174">En **Package Manager Console**, ejecute este comando:</span><span class="sxs-lookup"><span data-stu-id="cba28-174">In **Package Manager Console**, run this command:</span></span>  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      <span data-ttu-id="cba28-175">Si el comando se completa correctamente, aparece un mensaje que indica que se han habilitado las migraciones.</span><span class="sxs-lookup"><span data-stu-id="cba28-175">If the command completes successfully, a message stating migrations have been enabled appears.</span></span>

      ![Habilitación de migraciones](retrieving-data/_static/image8.png)

      <span data-ttu-id="cba28-177">Tenga en cuenta que un archivo denominado *Configuration.cs* se ha creado.</span><span class="sxs-lookup"><span data-stu-id="cba28-177">Notice that a file named *Configuration.cs* has been created.</span></span> <span data-ttu-id="cba28-178">El `Configuration` clase tiene un `Seed` método, que puede rellenar previamente las tablas de base de datos con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="cba28-178">The `Configuration` class has a `Seed` method, which can pre-populate the database tables with test data.</span></span>

## <a name="pre-populate-the-database"></a><span data-ttu-id="cba28-179">Rellenar previamente la base de datos</span><span class="sxs-lookup"><span data-stu-id="cba28-179">Pre-populate the database</span></span>

   1. <span data-ttu-id="cba28-180">Abra Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="cba28-180">Open Configuration.cs.</span></span>
   
   2. <span data-ttu-id="cba28-181">Agregue el código siguiente al método `Seed` .</span><span class="sxs-lookup"><span data-stu-id="cba28-181">Add the following code to the `Seed` method.</span></span> <span data-ttu-id="cba28-182">Además, agregue un `using` instrucción para el `ContosoUniversityModelBinding. Models` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="cba28-182">Also, add a `using` statement for the `ContosoUniversityModelBinding. Models` namespace.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. <span data-ttu-id="cba28-183">Guardar Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="cba28-183">Save Configuration.cs.</span></span>

   4. <span data-ttu-id="cba28-184">En la consola de administrador de paquetes, ejecute el comando **inicial de migración agregar**.</span><span class="sxs-lookup"><span data-stu-id="cba28-184">In the Package Manager Console, run the command **add-migration initial**.</span></span>

   5. <span data-ttu-id="cba28-185">Ejecute el comando **Actualizar base de datos**.</span><span class="sxs-lookup"><span data-stu-id="cba28-185">Run the command **update-database**.</span></span>

      <span data-ttu-id="cba28-186">Si recibe una excepción al ejecutar este comando, el `StudentID` y `CourseID` valores pueden diferir de la `Seed` valores del método.</span><span class="sxs-lookup"><span data-stu-id="cba28-186">If you receive an exception when running this command, the `StudentID` and `CourseID` values might be different from the `Seed` method values.</span></span> <span data-ttu-id="cba28-187">Abra las tablas de base de datos y busque los valores existentes de `StudentID` y `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="cba28-187">Open those database tables and find existing values for `StudentID` and `CourseID`.</span></span> <span data-ttu-id="cba28-188">Agregue esos valores en el código para la propagación del `Enrollments` tabla.</span><span class="sxs-lookup"><span data-stu-id="cba28-188">Add those values to the code for seeding the `Enrollments` table.</span></span>

## <a name="add-a-gridview-control"></a><span data-ttu-id="cba28-189">Agregar un control GridView</span><span class="sxs-lookup"><span data-stu-id="cba28-189">Add a GridView control</span></span>

<span data-ttu-id="cba28-190">Con los datos de la base de datos rellenada, ahora está listo para recuperar los datos y mostrarlos.</span><span class="sxs-lookup"><span data-stu-id="cba28-190">With populated database data, you're now ready to retrieve that data and display it.</span></span> 

1. <span data-ttu-id="cba28-191">Abra Students.aspx.</span><span class="sxs-lookup"><span data-stu-id="cba28-191">Open Students.aspx.</span></span>

2. <span data-ttu-id="cba28-192">Busque el `MainContent` marcador de posición.</span><span class="sxs-lookup"><span data-stu-id="cba28-192">Locate the `MainContent` placeholder.</span></span> <span data-ttu-id="cba28-193">Dentro de ese marcador de posición, agregue un **GridView** control que incluye este código.</span><span class="sxs-lookup"><span data-stu-id="cba28-193">Within that placeholder, add a **GridView** control that includes this code.</span></span>

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   <span data-ttu-id="cba28-194">Cosas a tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="cba28-194">Things to note:</span></span>
   * <span data-ttu-id="cba28-195">Tenga en cuenta el valor establecido para el `SelectMethod` propiedad en el elemento GridView.</span><span class="sxs-lookup"><span data-stu-id="cba28-195">Notice the value set for the `SelectMethod` property in the GridView element.</span></span> <span data-ttu-id="cba28-196">Este valor especifica el método utilizado para recuperar datos de GridView, que se crean en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="cba28-196">This value specifies the method used to retrieve GridView data, which you create in the next step.</span></span> 
   
   * <span data-ttu-id="cba28-197">El `ItemType` propiedad está establecida en el `Student` clase creada anteriormente.</span><span class="sxs-lookup"><span data-stu-id="cba28-197">The `ItemType` property is set to the `Student` class created earlier.</span></span> <span data-ttu-id="cba28-198">Esta configuración permite hacer referencia a propiedades de clase en el marcado.</span><span class="sxs-lookup"><span data-stu-id="cba28-198">This setting allows you to reference class properties in the markup.</span></span> <span data-ttu-id="cba28-199">Por ejemplo, el `Student` clase tiene una colección denominada `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="cba28-199">For example, the `Student` class has a collection named `Enrollments`.</span></span> <span data-ttu-id="cba28-200">Puede usar `Item.Enrollments` para recuperar esa colección y, a continuación, usar [sintaxis LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) recuperar cada estudiante del inscrito suma créditos.</span><span class="sxs-lookup"><span data-stu-id="cba28-200">You can use `Item.Enrollments` to retrieve that collection and then use [LINQ syntax](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) to retrieve each student's enrolled credits sum.</span></span>
   
3. <span data-ttu-id="cba28-201">Guardar Students.aspx.</span><span class="sxs-lookup"><span data-stu-id="cba28-201">Save Students.aspx.</span></span>

## <a name="add-code-to-retrieve-data"></a><span data-ttu-id="cba28-202">Agregue código para recuperar datos</span><span class="sxs-lookup"><span data-stu-id="cba28-202">Add code to retrieve data</span></span>

   <span data-ttu-id="cba28-203">En el archivo de código subyacente Students.aspx, agregue el método especificado para el `SelectMethod` valor.</span><span class="sxs-lookup"><span data-stu-id="cba28-203">In the Students.aspx code-behind file, add the method specified for the `SelectMethod` value.</span></span> 
   
   1. <span data-ttu-id="cba28-204">Abra Students.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="cba28-204">Open Students.aspx.cs.</span></span>
   
   2. <span data-ttu-id="cba28-205">Agregar `using` instrucciones para la `ContosoUniversityModelBinding. Models` y `System.Data.Entity` espacios de nombres.</span><span class="sxs-lookup"><span data-stu-id="cba28-205">Add `using` statements for the `ContosoUniversityModelBinding. Models` and `System.Data.Entity` namespaces.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. <span data-ttu-id="cba28-206">Agregue el método especificado para `SelectMethod`:</span><span class="sxs-lookup"><span data-stu-id="cba28-206">Add the method you specified for `SelectMethod`:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      <span data-ttu-id="cba28-207">El `Include` cláusula mejora el rendimiento de las consultas, pero no es necesario.</span><span class="sxs-lookup"><span data-stu-id="cba28-207">The `Include` clause improves query performance but isn't required.</span></span> <span data-ttu-id="cba28-208">Sin el `Include` cláusula, los datos se recupera mediante [ *la carga diferida*](https://en.wikipedia.org/wiki/Lazy_loading), lo que implica enviar una consulta independiente para la base de datos cada vez relacionados se recuperan los datos.</span><span class="sxs-lookup"><span data-stu-id="cba28-208">Without the `Include` clause, the data is retrieved using [*lazy loading*](https://en.wikipedia.org/wiki/Lazy_loading), which involves sending a separate query to the database each time related data is retrieved.</span></span> <span data-ttu-id="cba28-209">Con el `Include` cláusula, los datos se recupera mediante *carga diligente*, lo que significa que una consulta de base de datos única recupera todos los datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="cba28-209">With the `Include` clause, data is retrieved using *eager loading*, which means a single database query retrieves all related data.</span></span> <span data-ttu-id="cba28-210">Si no se usan datos relacionados, la carga diligente es menos eficiente porque se recuperan más datos.</span><span class="sxs-lookup"><span data-stu-id="cba28-210">If related data isn't used, eager loading is less efficient because more data is retrieved.</span></span> <span data-ttu-id="cba28-211">Sin embargo, en este caso, la carga diligente ofrece el mejor rendimiento porque los datos relacionados se muestran para cada registro.</span><span class="sxs-lookup"><span data-stu-id="cba28-211">However, in this case, eager loading gives you the best performance because the related data is displayed for each record.</span></span>

      <span data-ttu-id="cba28-212">Para obtener más información acerca de las consideraciones de rendimiento al cargar los datos relacionados, consulte el **explícita de carga de datos relacionados, diligente y diferida** sección la [leer los datos relacionados con Entity Framework en ASP.NET Aplicación MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) artículo.</span><span class="sxs-lookup"><span data-stu-id="cba28-212">For more information about performance considerations when loading related data, see the **Lazy, Eager, and Explicit Loading of Related Data** section in the [Reading Related Data with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) article.</span></span>

      <span data-ttu-id="cba28-213">De forma predeterminada, los datos se ordenan por los valores de la propiedad marcada como la clave.</span><span class="sxs-lookup"><span data-stu-id="cba28-213">By default, the data is sorted by the values of the property marked as the key.</span></span> <span data-ttu-id="cba28-214">Puede agregar un `OrderBy` cláusula para especificar un valor de ordenación diferente.</span><span class="sxs-lookup"><span data-stu-id="cba28-214">You can add an `OrderBy` clause to specify a different sort value.</span></span> <span data-ttu-id="cba28-215">En este ejemplo, el valor predeterminado `StudentID` propiedad se utiliza para ordenar.</span><span class="sxs-lookup"><span data-stu-id="cba28-215">In this example, the default `StudentID` property is used for sorting.</span></span> <span data-ttu-id="cba28-216">En el [datos filtrado, paginación y ordenación](sorting-paging-and-filtering-data.md) artículo, el usuario está habilitado para seleccionar una columna para ordenar.</span><span class="sxs-lookup"><span data-stu-id="cba28-216">In the [Sorting, Paging, and Filtering Data](sorting-paging-and-filtering-data.md) article, the user is enabled to select a column for sorting.</span></span>
 
   4. <span data-ttu-id="cba28-217">Guardar Students.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="cba28-217">Save Students.aspx.cs.</span></span>

## <a name="run-your-application"></a><span data-ttu-id="cba28-218">Ejecute la aplicación</span><span class="sxs-lookup"><span data-stu-id="cba28-218">Run your application</span></span> 

<span data-ttu-id="cba28-219">Ejecute la aplicación web (**F5**) y navegue hasta la **estudiantes** página, que muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="cba28-219">Run your web application (**F5**) and navigate to the **Students** page, which displays the following:</span></span>

   ![Mostrar datos](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a><span data-ttu-id="cba28-221">Generación automática de los métodos de enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="cba28-221">Automatic generation of model binding methods</span></span>

<span data-ttu-id="cba28-222">Cuando se trabaja a través de esta serie de tutoriales, simplemente copie el código del tutorial al proyecto.</span><span class="sxs-lookup"><span data-stu-id="cba28-222">When working through this tutorial series, you can simply copy the code from the tutorial to your project.</span></span> <span data-ttu-id="cba28-223">Sin embargo, una desventaja de este enfoque es que es posible que no será consciente de la característica proporcionada por Visual Studio para generar automáticamente código para los métodos de enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="cba28-223">However, one disadvantage of this approach is that you may not become aware of the feature provided by Visual Studio to automatically generate code for model binding methods.</span></span> <span data-ttu-id="cba28-224">Cuando se trabaja en sus propios proyectos, generación automática de código puede ahorrarle tiempo y ayudarle a obtener una idea de cómo implementar una operación.</span><span class="sxs-lookup"><span data-stu-id="cba28-224">When working on your own projects, automatic code generation can save you time and help you gain a sense of how to implement an operation.</span></span> <span data-ttu-id="cba28-225">En esta sección se describe la característica de generación automática de código.</span><span class="sxs-lookup"><span data-stu-id="cba28-225">This section describes the automatic code generation feature.</span></span> <span data-ttu-id="cba28-226">Esta sección solo es informativa y no contiene ningún código que necesita para implementar en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="cba28-226">This section is only informational and does not contain any code you need to implement in your project.</span></span> 

<span data-ttu-id="cba28-227">Al establecer un valor para el `SelectMethod`, `UpdateMethod`, `InsertMethod`, o `DeleteMethod` propiedades en el código de marcado, puede seleccionar la **crear un nuevo método** opción.</span><span class="sxs-lookup"><span data-stu-id="cba28-227">When setting a value for the `SelectMethod`, `UpdateMethod`, `InsertMethod`, or `DeleteMethod` properties in the markup code, you can select the **Create New Method** option.</span></span>

![Cree un método](retrieving-data/_static/image18.png)

<span data-ttu-id="cba28-229">Visual Studio no solo crea un método en el código subyacente con la firma correcta, sino que también genera código de implementación para realizar la operación.</span><span class="sxs-lookup"><span data-stu-id="cba28-229">Visual Studio not only creates a method in the code-behind with the proper signature, but also generates implementation code to perform the operation.</span></span> <span data-ttu-id="cba28-230">Si se establece en primer lugar el `ItemType` propiedad antes de usar la generación automática de código de características, los usos del código generado que escriban para las operaciones.</span><span class="sxs-lookup"><span data-stu-id="cba28-230">If you first set the `ItemType` property before using the automatic code generation feature, the generated code uses that type for the operations.</span></span> <span data-ttu-id="cba28-231">Por ejemplo, al establecer el `UpdateMethod` propiedad, el código siguiente se genera automáticamente:</span><span class="sxs-lookup"><span data-stu-id="cba28-231">For example, when setting the `UpdateMethod` property, the following code is automatically generated:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

<span data-ttu-id="cba28-232">De nuevo, este código no tiene que agregarse al proyecto.</span><span class="sxs-lookup"><span data-stu-id="cba28-232">Again, this code doesn't need to be added to your project.</span></span> <span data-ttu-id="cba28-233">En el siguiente tutorial, implementará métodos para actualizar, eliminar y agregar datos nuevos.</span><span class="sxs-lookup"><span data-stu-id="cba28-233">In the next tutorial, you'll implement methods for updating, deleting, and adding new data.</span></span>

## <a name="summary"></a><span data-ttu-id="cba28-234">Resumen</span><span class="sxs-lookup"><span data-stu-id="cba28-234">Summary</span></span>

<span data-ttu-id="cba28-235">En este tutorial, creó clases de modelo de datos y genera una base de datos de esas clases.</span><span class="sxs-lookup"><span data-stu-id="cba28-235">In this tutorial, you created data model classes and generated a database from those classes.</span></span> <span data-ttu-id="cba28-236">Rellena las tablas de base de datos con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="cba28-236">You filled the database tables with test data.</span></span> <span data-ttu-id="cba28-237">Usa el enlace de modelos para recuperar datos de la base de datos y, a continuación, muestra los datos en un control GridView.</span><span class="sxs-lookup"><span data-stu-id="cba28-237">You used model binding to retrieve data from the database, and then displayed the data in a GridView.</span></span>

<span data-ttu-id="cba28-238">En los próximos [tutorial](updating-deleting-and-creating-data.md) en esta serie, permitiremos a actualizar, eliminar y crear datos.</span><span class="sxs-lookup"><span data-stu-id="cba28-238">In the next [tutorial](updating-deleting-and-creating-data.md) in this series, you'll enable updating, deleting, and creating data.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="cba28-239">Siguiente</span><span class="sxs-lookup"><span data-stu-id="cba28-239">Next</span></span>](updating-deleting-and-creating-data.md)
