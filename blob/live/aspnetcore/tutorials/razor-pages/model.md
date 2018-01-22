---
title: "Adición de un modelo a una aplicación de páginas de Razor en ASP.NET Core"
author: rick-anderson
description: "Adición de un modelo a una aplicación de páginas de Razor en ASP.NET Core"
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/model
ms.openlocfilehash: 84e5ec27904b564fa6ee29843ceae0bb70754ea7
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="adding-a-model-to-a-razor-pages-app"></a><span data-ttu-id="b6fbb-103">Adición de un modelo a una aplicación de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="b6fbb-103">Adding a model to a Razor Pages app</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="b6fbb-104">Agregar un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="b6fbb-104">Add a data model</span></span>

<span data-ttu-id="b6fbb-105">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **RazorPagesMovie** > **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="b6fbb-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="b6fbb-106">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="b6fbb-106">Name the folder *Models*.</span></span>

<span data-ttu-id="b6fbb-107">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="b6fbb-107">Right click the *Models* folder.</span></span> <span data-ttu-id="b6fbb-108">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="b6fbb-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="b6fbb-109">Asigne a la clase el nombre **Movie** y agregue las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="b6fbb-109">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="b6fbb-110">Agregar una cadena de conexión de base de datos</span><span class="sxs-lookup"><span data-stu-id="b6fbb-110">Add a database connection string</span></span>

<span data-ttu-id="b6fbb-111">Agregue una cadena de conexión al archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="b6fbb-111">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="b6fbb-112">Registrar el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="b6fbb-112">Register the database context</span></span>

<span data-ttu-id="b6fbb-113">Registre el contexto de base de datos con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en el archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="b6fbb-113">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="b6fbb-114">Compile el proyecto para comprobar que no contiene errores.</span><span class="sxs-lookup"><span data-stu-id="b6fbb-114">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="b6fbb-115">Agregar herramientas de scaffolding y realizar la migración inicial</span><span class="sxs-lookup"><span data-stu-id="b6fbb-115">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="b6fbb-116">En esta sección, usará la Consola del Administrador de paquetes (PMC) para:</span><span class="sxs-lookup"><span data-stu-id="b6fbb-116">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="b6fbb-117">Agregar el paquete de generación de código de Visual Studio web.</span><span class="sxs-lookup"><span data-stu-id="b6fbb-117">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="b6fbb-118">Este paquete es necesario para ejecutar el motor de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="b6fbb-118">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="b6fbb-119">Agregar una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="b6fbb-119">Add an initial migration.</span></span>
* <span data-ttu-id="b6fbb-120">Actualizar la base de datos con la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="b6fbb-120">Update the database with the initial migration.</span></span>

<span data-ttu-id="b6fbb-121">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="b6fbb-121">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="b6fbb-123">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="b6fbb-123">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Add-Migration Initial
Update-Database
```

<span data-ttu-id="b6fbb-124">El comando `Install-Package` instala las herramientas necesarias para ejecutar el motor de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="b6fbb-124">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="b6fbb-125">El comando `Add-Migration` genera el código para crear el esquema de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="b6fbb-125">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="b6fbb-126">El esquema se basa en el modelo especificado en `DbContext` (en el archivo *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="b6fbb-126">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="b6fbb-127">El argumento `Initial` se usa para asignar un nombre a las migraciones.</span><span class="sxs-lookup"><span data-stu-id="b6fbb-127">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="b6fbb-128">Puede usar cualquier nombre, pero se suele elegir uno que describa la migración.</span><span class="sxs-lookup"><span data-stu-id="b6fbb-128">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="b6fbb-129">Vea [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) (Introducción a las migraciones) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="b6fbb-129">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="b6fbb-130">El comando `Update-Database` ejecuta el método `Up` en el archivo *Migrations/\<time-stamp>_InitialCreate.cs*, con lo que se crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b6fbb-130">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4tbl.md)]

<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="b6fbb-131">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="b6fbb-131">Test the app</span></span>

* <span data-ttu-id="b6fbb-132">Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="b6fbb-132">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="b6fbb-133">Pruebe el vínculo **Crear**.</span><span class="sxs-lookup"><span data-stu-id="b6fbb-133">Test the **Create** link.</span></span>

 ![Página Crear](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="b6fbb-135">Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="b6fbb-135">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="b6fbb-136">Si obtiene una excepción SQL, verifique que haya ejecuto las migraciones y que la base de datos esté actualizada:</span><span class="sxs-lookup"><span data-stu-id="b6fbb-136">If you get a SQL exception, verify you have run migrations and updated the database:</span></span>

<span data-ttu-id="b6fbb-137">En el tutorial siguiente se explican los archivos creados mediante scaffolding.</span><span class="sxs-lookup"><span data-stu-id="b6fbb-137">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="b6fbb-138">[Anterior: Introducción](xref:tutorials/razor-pages/razor-pages-start)
[Siguiente: Scaffolded Razor Pages (Páginas de Razor creadas mediante scaffolding)](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="b6fbb-138">[Previous: Getting Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
