---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: Introducción a Entity Framework 6 Database First con MVC 5 | Documentos de Microsoft
author: tfitzmac
description: Con MVC, Entity Framework y Scaffolding de ASP.NET, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Este tutorial seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: ae60b5c808d2522c66dc17ccf7d16fefdc65d552
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879340"
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a><span data-ttu-id="dda52-104">Introducción a Entity Framework 6 Database First con MVC 5</span><span class="sxs-lookup"><span data-stu-id="dda52-104">Getting Started with Entity Framework 6 Database First using MVC 5</span></span>
====================
<span data-ttu-id="dda52-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="dda52-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="dda52-106">Con MVC, Entity Framework y Scaffolding de ASP.NET, puede crear una aplicación web que proporciona una interfaz a una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="dda52-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="dda52-107">Esta serie de tutoriales muestra cómo automáticamente genera código que permite a los usuarios mostrar, editar, crear y eliminar datos que residen en una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="dda52-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="dda52-108">El código generado corresponde a las columnas de la tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="dda52-108">The generated code corresponds to the columns in the database table.</span></span> <span data-ttu-id="dda52-109">En la última parte de la serie, se implementará el sitio y la base de datos en Azure.</span><span class="sxs-lookup"><span data-stu-id="dda52-109">In the last part of the series, you will deploy the site and database to Azure.</span></span>
> 
> <span data-ttu-id="dda52-110">Esta parte de la serie se centra en crear una base de datos y rellenarlo con datos.</span><span class="sxs-lookup"><span data-stu-id="dda52-110">This part of the series focuses on creating a database and populating it with data.</span></span>
> 
> <span data-ttu-id="dda52-111">Esta serie se escribió con las contribuciones de Tom Dykstra y Rick Anderson.</span><span class="sxs-lookup"><span data-stu-id="dda52-111">This series was written with contributions from Tom Dykstra and Rick Anderson.</span></span> <span data-ttu-id="dda52-112">Se ha mejorado en función de los comentarios de los usuarios en la sección Comentarios.</span><span class="sxs-lookup"><span data-stu-id="dda52-112">It was improved based on feedback from users in the comments section.</span></span>


## <a name="introduction"></a><span data-ttu-id="dda52-113">Introducción</span><span class="sxs-lookup"><span data-stu-id="dda52-113">Introduction</span></span>

<span data-ttu-id="dda52-114">Este tema muestra cómo comenzar con otra base de datos y crear rápidamente una aplicación web que permite a los usuarios interactuar con los datos.</span><span class="sxs-lookup"><span data-stu-id="dda52-114">This topic shows how to start with an existing database and quickly create a web application that enables users to interact with the data.</span></span> <span data-ttu-id="dda52-115">Usa el Entity Framework 6 y 5 de MVC para compilar la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="dda52-115">It uses the Entity Framework 6 and MVC 5 to build the web application.</span></span> <span data-ttu-id="dda52-116">La característica de Scaffolding de ASP.NET permite generar automáticamente código para mostrar, actualizar, crear y eliminar datos.</span><span class="sxs-lookup"><span data-stu-id="dda52-116">The ASP.NET Scaffolding feature enables you to automatically generate code for displaying, updating, creating and deleting data.</span></span> <span data-ttu-id="dda52-117">Con las herramientas de publicación en Visual Studio, puede implementar fácilmente el sitio y la base de datos en Azure.</span><span class="sxs-lookup"><span data-stu-id="dda52-117">Using the publishing tools within Visual Studio, you can easily deploy the site and database to Azure.</span></span>

<span data-ttu-id="dda52-118">Este tema trata la situación donde tiene una base de datos y desea generar código para una aplicación web basada en los campos de esa base de datos.</span><span class="sxs-lookup"><span data-stu-id="dda52-118">This topic addresses the situation where you have a database and want to generate code for a web application based on the fields of that database.</span></span> <span data-ttu-id="dda52-119">Este enfoque se denomina desarrollo First de base de datos.</span><span class="sxs-lookup"><span data-stu-id="dda52-119">This approach is called Database First development.</span></span> <span data-ttu-id="dda52-120">Si no dispone de una base de datos existente, puede usar un método denominado desarrollo Code First que consiste en definir las clases de datos y generar la base de datos de las propiedades de clase.</span><span class="sxs-lookup"><span data-stu-id="dda52-120">If you do not already have an existing database, you can instead use an approach called Code First development which involves defining data classes and generating the database from the class properties.</span></span>

<span data-ttu-id="dda52-121">Para obtener un ejemplo de introducción de desarrollo Code First, vea [Introducción a ASP.NET MVC 5](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="dda52-121">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span> <span data-ttu-id="dda52-122">Para obtener un ejemplo más avanzado, vea [crear un modelo de datos de Entity Framework para una aplicación de ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="dda52-122">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="dda52-123">Para obtener instrucciones sobre cómo seleccionar qué enfoque de Entity Framework para utilizar, consulte [enfoques de desarrollo de Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span><span class="sxs-lookup"><span data-stu-id="dda52-123">For guidance on selecting which Entity Framework approach to use, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dda52-124">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="dda52-124">Prerequisites</span></span>

<span data-ttu-id="dda52-125">Visual Studio 2013 o Visual Studio Express 2013 para Web</span><span class="sxs-lookup"><span data-stu-id="dda52-125">Visual Studio 2013 or Visual Studio Express 2013 for Web</span></span>

## <a name="set-up-the-database"></a><span data-ttu-id="dda52-126">Configurar la base de datos</span><span class="sxs-lookup"><span data-stu-id="dda52-126">Set up the database</span></span>

<span data-ttu-id="dda52-127">Para imitar el entorno de tener una base de datos existente, creará primero una base de datos con datos previamente rellenadas y, a continuación, crear la aplicación web que se conecta a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="dda52-127">To mimic the environment of having an existing database, you will first create a database with some pre-filled data, and then create your web application that connects to the database.</span></span>

<span data-ttu-id="dda52-128">Este tutorial se desarrolló con LocalDB con Visual Studio 2013 o Visual Studio Express 2013 para Web.</span><span class="sxs-lookup"><span data-stu-id="dda52-128">This tutorial was developed using LocalDB with either Visual Studio 2013 or Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="dda52-129">Puede usar un servidor de base de datos existente en lugar de LocalDB, pero dependiendo de la versión de Visual Studio y el tipo de base de datos, todas las herramientas de datos en Visual Studio podrían no admitir.</span><span class="sxs-lookup"><span data-stu-id="dda52-129">You can use an existing database server instead of LocalDB, but depending on your version of Visual Studio and your type of database, all of the data tools in Visual Studio might not be supported.</span></span> <span data-ttu-id="dda52-130">Si las herramientas no están disponibles para la base de datos, debe realizar algunos de los pasos específicos de la base de datos en el conjunto de administración de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="dda52-130">If the tools are not available for your database, you may need to perform some of the database-specific steps within the management suite for your database.</span></span>

<span data-ttu-id="dda52-131">Si tiene un problema con las herramientas de base de datos de la versión de Visual Studio, asegúrese de que ha instalado la versión más reciente de las herramientas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="dda52-131">If you have a problem with the database tools in your version of Visual Studio, make sure you have installed the latest version of the database tools.</span></span> <span data-ttu-id="dda52-132">Para obtener información sobre cómo actualizar o instalar las herramientas de base de datos, vea [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).</span><span class="sxs-lookup"><span data-stu-id="dda52-132">For information about updating or installing the database tools, see [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).</span></span>

<span data-ttu-id="dda52-133">Inicie Visual Studio y cree una **proyecto de base de datos de SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="dda52-133">Launch Visual Studio and create a **SQL Server Database Project**.</span></span> <span data-ttu-id="dda52-134">Denomine el proyecto **ContosoUniversityData**.</span><span class="sxs-lookup"><span data-stu-id="dda52-134">Name the project **ContosoUniversityData**.</span></span>

![Crear proyecto de base de datos](setting-up-database/_static/image1.png)

<span data-ttu-id="dda52-136">Ahora tiene un proyecto de base de datos vacía.</span><span class="sxs-lookup"><span data-stu-id="dda52-136">You now have an empty database project.</span></span> <span data-ttu-id="dda52-137">Se implementará esta base de datos en Azure, más adelante en este tutorial, por lo que necesitará establecer la base de datos de SQL Azure como la plataforma de destino para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="dda52-137">You will deploy this database to Azure later in this tutorial, so you'll need to set Azure SQL Database as the target platform for the project.</span></span> <span data-ttu-id="dda52-138">Configuración de la plataforma de destino no implementa realmente la base de datos; sólo significa que el proyecto de base de datos comprobará que el diseño de base de datos es compatible con la plataforma de destino.</span><span class="sxs-lookup"><span data-stu-id="dda52-138">Setting the target platform does not actually deploy the database; it only means that the database project will verify that the database design is compatible with the target platform.</span></span> <span data-ttu-id="dda52-139">Para establecer la plataforma de destino, abra el **propiedades** para el proyecto y seleccione **base de datos de Microsoft Azure SQL** para la plataforma de destino.</span><span class="sxs-lookup"><span data-stu-id="dda52-139">To set the target platform, open the **Properties** for the project and select **Microsoft Azure SQL Database** for the target platform.</span></span>

![plataforma de destino de conjunto](setting-up-database/_static/image2.png)

<span data-ttu-id="dda52-141">Puede crear las tablas necesarias para este tutorial mediante la adición de secuencias de comandos SQL que definen las tablas.</span><span class="sxs-lookup"><span data-stu-id="dda52-141">You can create the tables needed for this tutorial by adding SQL scripts that define the tables.</span></span> <span data-ttu-id="dda52-142">Haga clic en el proyecto y agregar un nuevo elemento.</span><span class="sxs-lookup"><span data-stu-id="dda52-142">Right-click your project and add a new item.</span></span>

![Agregar nuevo elemento](setting-up-database/_static/image3.png)

<span data-ttu-id="dda52-144">Agregar una nueva tabla denominada Student.</span><span class="sxs-lookup"><span data-stu-id="dda52-144">Add a new table named Student.</span></span>

![Agregar tabla student](setting-up-database/_static/image4.png)

<span data-ttu-id="dda52-146">En el archivo de la tabla, reemplace el comando de T-SQL con el código siguiente para crear la tabla.</span><span class="sxs-lookup"><span data-stu-id="dda52-146">In the table file, replace the T-SQL command with the following code to create the table.</span></span>

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

<span data-ttu-id="dda52-147">Tenga en cuenta que la ventana de diseño se sincroniza automáticamente con el código.</span><span class="sxs-lookup"><span data-stu-id="dda52-147">Notice that the design window automatically synchronizes with the code.</span></span> <span data-ttu-id="dda52-148">También puede trabajar con el código o en el diseñador.</span><span class="sxs-lookup"><span data-stu-id="dda52-148">You can work with either the code or designer.</span></span>

![Mostrar código y diseño](setting-up-database/_static/image5.png)

<span data-ttu-id="dda52-150">Agregar otra tabla.</span><span class="sxs-lookup"><span data-stu-id="dda52-150">Add another table.</span></span> <span data-ttu-id="dda52-151">Esta vez denomínelo curso y utilice el siguiente comando de T-SQL.</span><span class="sxs-lookup"><span data-stu-id="dda52-151">This time name it Course and use the following T-SQL command.</span></span>

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

<span data-ttu-id="dda52-152">Y repita una vez más para crear una tabla denominada inscripción.</span><span class="sxs-lookup"><span data-stu-id="dda52-152">And, repeat one more time to create a table named Enrollment.</span></span>

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

<span data-ttu-id="dda52-153">Puede rellenar la base de datos con datos a través de una secuencia de comandos que se ejecuta una vez implementada la base de datos.</span><span class="sxs-lookup"><span data-stu-id="dda52-153">You can populate your database with data through a script that is run after the database is deployed.</span></span> <span data-ttu-id="dda52-154">Agregar un Script posterior a la implementación para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="dda52-154">Add a Post-Deployment Script to the project.</span></span> <span data-ttu-id="dda52-155">Puede usar el nombre predeterminado.</span><span class="sxs-lookup"><span data-stu-id="dda52-155">You can use the default name.</span></span>

![Agregar script posterior a la implementación](setting-up-database/_static/image6.png)

<span data-ttu-id="dda52-157">Agregue el código de T-SQL siguiente a la secuencia de comandos posterior a la implementación.</span><span class="sxs-lookup"><span data-stu-id="dda52-157">Add the following T-SQL code to the post-deployment script.</span></span> <span data-ttu-id="dda52-158">Esta secuencia de comandos simplemente agrega datos a la base de datos cuando no se encuentra ningún registro coincidente.</span><span class="sxs-lookup"><span data-stu-id="dda52-158">This script simply adds data to the database when no matching record is found.</span></span> <span data-ttu-id="dda52-159">No sobrescribir o eliminar los datos que se insertó en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="dda52-159">It does not overwrite or delete any data you may have entered into the database.</span></span>

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

<span data-ttu-id="dda52-160">Es importante tener en cuenta que el script posterior a la implementación se ejecuta cada vez que implementa el proyecto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="dda52-160">It is important to note that the post-deployment script is run every time you deploy your database project.</span></span> <span data-ttu-id="dda52-161">Por lo tanto, debe considerar detenidamente los requisitos al escribir esta secuencia de comandos.</span><span class="sxs-lookup"><span data-stu-id="dda52-161">Therefore, you need to carefully consider your requirements when writing this script.</span></span> <span data-ttu-id="dda52-162">En algunos casos, puede que desee iniciar desde un conjunto conocido de datos cada vez que se implementa el proyecto.</span><span class="sxs-lookup"><span data-stu-id="dda52-162">In some cases, you may wish to start over from a known set of data every time the project is deployed.</span></span> <span data-ttu-id="dda52-163">En otros casos, no puede modificar los datos existentes de ninguna manera.</span><span class="sxs-lookup"><span data-stu-id="dda52-163">In other cases, you may not want to alter the existing data in any way.</span></span> <span data-ttu-id="dda52-164">Según sus requisitos, puede decidir si necesita un script posterior a la implementación o lo que necesita incluir en la secuencia de comandos.</span><span class="sxs-lookup"><span data-stu-id="dda52-164">Based on your requirements, you can decide whether you need a post-deployment script or what you need to include in the script.</span></span> <span data-ttu-id="dda52-165">Para obtener más información acerca de cómo rellenar la base de datos con un script posterior a la implementación, consulte [datos incluidos en un proyecto de base de datos de SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span><span class="sxs-lookup"><span data-stu-id="dda52-165">For more information about populating your database with a post-deployment script, see [Including Data in a SQL Server Database Project](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span></span>

<span data-ttu-id="dda52-166">Ahora tiene 4 archivos de script SQL, pero ninguna de las tablas real.</span><span class="sxs-lookup"><span data-stu-id="dda52-166">You now have 4 SQL script files but no actual tables.</span></span> <span data-ttu-id="dda52-167">Está listo para implementar el proyecto de base de datos en localdb.</span><span class="sxs-lookup"><span data-stu-id="dda52-167">You are ready to deploy your database project to localdb.</span></span> <span data-ttu-id="dda52-168">En Visual Studio, haga clic en el botón Inicio (o F5) para compilar e implementar el proyecto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="dda52-168">In Visual Studio, click the Start button (or F5) to build and deploy your database project.</span></span> <span data-ttu-id="dda52-169">Compruebe la ficha salida para comprobar que la compilación e implementación se realizó correctamente.</span><span class="sxs-lookup"><span data-stu-id="dda52-169">Check the Output tab to verify that the build and deployment succeeded.</span></span>

![Mostrar salida](setting-up-database/_static/image7.png)

<span data-ttu-id="dda52-171">Para ver que se ha creado la nueva base de datos, abra **Explorador de objetos de SQL Server** y busque el nombre del proyecto en el servidor de base de datos local correcta (en este caso **\ProjectsV12 (localdb)**).</span><span class="sxs-lookup"><span data-stu-id="dda52-171">To see that the new database has been created, open **SQL Server Object Explorer** and look for the name of the project in the correct local database server (in this case **(localdb)\ProjectsV12**).</span></span>

![Mostrar la nueva base de datos](setting-up-database/_static/image8.png)

<span data-ttu-id="dda52-173">Para ver que las tablas se rellenan con datos, haga clic en una tabla y seleccione **ver datos**.</span><span class="sxs-lookup"><span data-stu-id="dda52-173">To see that the tables are populated with data, right-click a table, and select **View Data**.</span></span>

![Mostrar datos de la tabla](setting-up-database/_static/image9.png)

<span data-ttu-id="dda52-175">Se muestra una vista editable de los datos de tabla.</span><span class="sxs-lookup"><span data-stu-id="dda52-175">An editable view of the table data is displayed.</span></span>

![Mostrar resultados de datos de tabla](setting-up-database/_static/image10.png)

<span data-ttu-id="dda52-177">La base de datos ahora se configura y se rellena con datos.</span><span class="sxs-lookup"><span data-stu-id="dda52-177">Your database is now set up and populated with data.</span></span> <span data-ttu-id="dda52-178">En el siguiente tutorial, creará una aplicación web para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="dda52-178">In the next tutorial, you will create a web application for the database.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="dda52-179">Siguiente</span><span class="sxs-lookup"><span data-stu-id="dda52-179">Next</span></span>](creating-the-web-application.md)
