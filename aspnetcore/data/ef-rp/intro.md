---
title: 'Páginas de Razor con Entity Framework Core en ASP.NET Core: Tutorial 1 de 8'
author: tdykstra
description: Se muestra cómo crear una aplicación de páginas de Razor mediante Entity Framework Core
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 07/22/2019
uid: data/ef-rp/intro
ms.openlocfilehash: 107b348b4484301b86eeb5528833914fe4c1eaf7
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080936"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="4a36a-103">Páginas de Razor con Entity Framework Core en ASP.NET Core: Tutorial 1 de 8</span><span class="sxs-lookup"><span data-stu-id="4a36a-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

<span data-ttu-id="4a36a-104">Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4a36a-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4a36a-105">Este es el primero de una serie de tutoriales en los que se muestra cómo usar Entity Framework (EF) Core en una aplicación Razor Pages en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4a36a-105">This is the first in a series of tutorials that show how to use Entity Framework (EF) Core in an ASP.NET Core Razor Pages app.</span></span> <span data-ttu-id="4a36a-106">En el tutorial se crea un sitio web de una universidad ficticia, Contoso University.</span><span class="sxs-lookup"><span data-stu-id="4a36a-106">The tutorials build a web site for a fictional Contoso University.</span></span> <span data-ttu-id="4a36a-107">El sitio incluye funciones como la admisión de alumnos, la creación de cursos y las asignaciones de instructores.</span><span class="sxs-lookup"><span data-stu-id="4a36a-107">The site includes functionality such as student admission, course creation, and instructor assignments.</span></span>

[<span data-ttu-id="4a36a-108">Descargue o vea la aplicación completa.</span><span class="sxs-lookup"><span data-stu-id="4a36a-108">Download or view the completed app.</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="4a36a-109">[Instrucciones de descarga](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="4a36a-109">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4a36a-110">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="4a36a-110">Prerequisites</span></span>

* <span data-ttu-id="4a36a-111">Si no está familiarizado con Razor Pages, consulte la serie de tutoriales [Introducción a Razor Pages](xref:tutorials/razor-pages/razor-pages-start) antes de empezar este.</span><span class="sxs-lookup"><span data-stu-id="4a36a-111">If you're new to Razor Pages, go through the [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) tutorial series before starting this one.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4a36a-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4a36a-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[VS prereqs](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4a36a-113">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4a36a-113">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[VS Code prereqs](~/includes/net-core-prereqs-vsc-3.0.md)]

---

## <a name="database-engines"></a><span data-ttu-id="4a36a-114">Motores de bases de datos</span><span class="sxs-lookup"><span data-stu-id="4a36a-114">Database engines</span></span>

<span data-ttu-id="4a36a-115">En las instrucciones de Visual Studio se usa [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb), una versión de SQL Server Express que solo se ejecuta en Windows.</span><span class="sxs-lookup"><span data-stu-id="4a36a-115">The Visual Studio instructions use [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb), a version of SQL Server Express that runs only on Windows.</span></span>

<span data-ttu-id="4a36a-116">En las instrucciones de Visual Studio Code se usa [SQLite](https://www.sqlite.org/), un motor de base de datos multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="4a36a-116">The Visual Studio Code instructions use [SQLite](https://www.sqlite.org/), a cross-platform database engine.</span></span>

<span data-ttu-id="4a36a-117">Si decide usar SQLite, descargue e instale una herramienta de terceros para administrar y ver una base de datos de SQLite, como [DB Browser for SQLite](https://sqlitebrowser.org/).</span><span class="sxs-lookup"><span data-stu-id="4a36a-117">If you choose to use SQLite, download and install a third-party tool for managing and viewing a SQLite database, such as [DB Browser for SQLite](https://sqlitebrowser.org/).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="4a36a-118">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="4a36a-118">Troubleshooting</span></span>

<span data-ttu-id="4a36a-119">Si experimenta un problema que no puede resolver, compare el código con el [proyecto completado](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="4a36a-119">If you run into a problem you can't resolve, compare your code to the [completed project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="4a36a-120">Una buena forma de obtener ayuda consiste en publicar una pregunta en StackOverflow.com con la [etiqueta ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) o la [etiqueta EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="4a36a-120">A good way to get help is by posting a question to StackOverflow.com, using the [ASP.NET Core tag](https://stackoverflow.com/questions/tagged/asp.net-core) or the [EF Core tag](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-sample-app"></a><span data-ttu-id="4a36a-121">La aplicación de ejemplo</span><span class="sxs-lookup"><span data-stu-id="4a36a-121">The sample app</span></span>

<span data-ttu-id="4a36a-122">La aplicación compilada en estos tutoriales es un sitio web básico de una universidad.</span><span class="sxs-lookup"><span data-stu-id="4a36a-122">The app built in these tutorials is a basic university web site.</span></span> <span data-ttu-id="4a36a-123">Los usuarios pueden ver y actualizar la información de estudiantes, cursos e instructores.</span><span class="sxs-lookup"><span data-stu-id="4a36a-123">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="4a36a-124">Estas son algunas de las pantallas que se crean en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="4a36a-124">Here are a few of the screens created in the tutorial.</span></span>

![Página de índice de Students](intro/_static/students-index30.png)

![Página de edición de estudiantes](intro/_static/student-edit30.png)

<span data-ttu-id="4a36a-127">El estilo de la interfaz de usuario de este sitio se basa en las plantillas de proyecto integradas.</span><span class="sxs-lookup"><span data-stu-id="4a36a-127">The UI style of this site is based on the built-in project templates.</span></span> <span data-ttu-id="4a36a-128">El enfoque del tutorial es cómo usar EF Core, no cómo personalizar la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="4a36a-128">The tutorial's focus is on how to use EF Core, not how to customize the UI.</span></span>

<span data-ttu-id="4a36a-129">Siga el vínculo de la parte superior de la página para obtener el código fuente para el proyecto completado.</span><span class="sxs-lookup"><span data-stu-id="4a36a-129">Follow the link at the top of the page to get the source code for the completed project.</span></span> <span data-ttu-id="4a36a-130">La carpeta *cu30* contiene el código para la versión ASP.NET Core 3.0 del tutorial.</span><span class="sxs-lookup"><span data-stu-id="4a36a-130">The *cu30* folder has the code for the ASP.NET Core 3.0 version of the tutorial.</span></span> <span data-ttu-id="4a36a-131">Los archivos que reflejan el estado del código para los tutoriales 1-7 se pueden encontrar en la carpeta *cu30snapshots*.</span><span class="sxs-lookup"><span data-stu-id="4a36a-131">Files that reflect the state of the code for tutorials 1-7 can be found in the *cu30snapshots* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4a36a-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4a36a-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4a36a-133">Para ejecutar la aplicación después de descargar el proyecto completado:</span><span class="sxs-lookup"><span data-stu-id="4a36a-133">To run the app after downloading the completed project:</span></span>

* <span data-ttu-id="4a36a-134">Elimine tres archivos y una carpeta que contienen *SQLite* en el nombre.</span><span class="sxs-lookup"><span data-stu-id="4a36a-134">Delete three files and one folder that have *SQLite* in the name.</span></span>
* <span data-ttu-id="4a36a-135">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="4a36a-135">Build the project.</span></span>
* <span data-ttu-id="4a36a-136">En la Consola del administrador de paquetes (PMC), ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="4a36a-136">In Package Manager Console (PMC) run the following command:</span></span>

  ```powershell
  Update-Database
  ```

* <span data-ttu-id="4a36a-137">Ejecute el proyecto para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-137">Run the project to seed the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4a36a-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4a36a-138">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4a36a-139">Para ejecutar la aplicación después de descargar el proyecto completado:</span><span class="sxs-lookup"><span data-stu-id="4a36a-139">To run the app after downloading the completed project:</span></span>

* <span data-ttu-id="4a36a-140">Elimine *ContosoUniversity.csproj* y cambie el nombre de *ContosoUniversitySQLite.csproj* por *ContosoUniversity.csproj*.</span><span class="sxs-lookup"><span data-stu-id="4a36a-140">Delete *ContosoUniversity.csproj*, and rename *ContosoUniversitySQLite.csproj* to *ContosoUniversity.csproj*.</span></span>
* <span data-ttu-id="4a36a-141">Elimine *Startup.cs*, y cambie el nombre de *StartupSQLite.cs* por *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="4a36a-141">Delete *Startup.cs*, and rename *StartupSQLite.cs* to *Startup.cs*.</span></span>
* <span data-ttu-id="4a36a-142">Elimine *appSettings.json* y cambie el nombre de *appSettingsSQLite.json* por *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="4a36a-142">Delete *appSettings.json*, and rename *appSettingsSQLite.json* to *appSettings.json*.</span></span>
* <span data-ttu-id="4a36a-143">Elimine la carpeta *Migrations* y cambie el nombre de *MigrationsSQL* por *Migrations*.</span><span class="sxs-lookup"><span data-stu-id="4a36a-143">Delete the *Migrations* folder, and rename *MigrationsSQL* to *Migrations*.</span></span>
* <span data-ttu-id="4a36a-144">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="4a36a-144">Build the project.</span></span>
* <span data-ttu-id="4a36a-145">En un símbolo del sistema en la carpeta del proyecto, ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="4a36a-145">At a command prompt in the project folder, run the following commands:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-ef --version 3.0.0-*
  dotnet ef database update
  ```

* <span data-ttu-id="4a36a-146">En la herramienta SQLite, ejecute la siguiente instrucción SQL:</span><span class="sxs-lookup"><span data-stu-id="4a36a-146">In your SQLite tool, run the following SQL statement:</span></span>

  ```sql
  UPDATE Department SET RowVersion = randomblob(8)
  ```

* <span data-ttu-id="4a36a-147">Ejecute el proyecto para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-147">Run the project to seed the database.</span></span>

---

## <a name="create-the-web-app-project"></a><span data-ttu-id="4a36a-148">Creación del proyecto de aplicación web</span><span class="sxs-lookup"><span data-stu-id="4a36a-148">Create the web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4a36a-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4a36a-149">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4a36a-150">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="4a36a-150">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="4a36a-151">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="4a36a-151">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="4a36a-152">Asigne el nombre *ContosoUniversity* al proyecto.</span><span class="sxs-lookup"><span data-stu-id="4a36a-152">Name the project *ContosoUniversity*.</span></span> <span data-ttu-id="4a36a-153">Es importante usar este nombre exacto incluido el uso de mayúsculas, para que los espacios de nombres coincidan cuando se copie y pegue el código.</span><span class="sxs-lookup"><span data-stu-id="4a36a-153">It's important to use this exact name including capitalization, so the namespaces match when code is copied and pasted.</span></span>
* <span data-ttu-id="4a36a-154">Seleccione **.NET Core** y **ASP.NET Core 3.0** en las listas desplegables y, luego, **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="4a36a-154">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdowns, and then select **Web Application**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4a36a-155">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4a36a-155">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="4a36a-156">En un terminal, vaya a la carpeta en la que se debe crear la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="4a36a-156">In a terminal, navigate to the folder in which the project folder should be created.</span></span>

* <span data-ttu-id="4a36a-157">Ejecute los comandos siguientes para crear un proyecto de Razor Pages y `cd` para acceder a la nueva carpeta del proyecto:</span><span class="sxs-lookup"><span data-stu-id="4a36a-157">Run the following commands to create a Razor Pages project and `cd` into the new project folder:</span></span>

  ```dotnetcli
  dotnet new webapp -o ContosoUniversity
  cd ContosoUniversity
  ```

---

## <a name="set-up-the-site-style"></a><span data-ttu-id="4a36a-158">Configurar el estilo del sitio</span><span class="sxs-lookup"><span data-stu-id="4a36a-158">Set up the site style</span></span>

<span data-ttu-id="4a36a-159">Configure el encabezado, el pie de página y el menú del sitio mediante la actualización de *Pages/Shared/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="4a36a-159">Set up the site header, footer, and menu by updating *Pages/Shared/_Layout.cshtml*:</span></span>

* <span data-ttu-id="4a36a-160">Cambie todas las repeticiones de "ContosoUniversity" por "Contoso University".</span><span class="sxs-lookup"><span data-stu-id="4a36a-160">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="4a36a-161">Hay tres repeticiones.</span><span class="sxs-lookup"><span data-stu-id="4a36a-161">There are three occurrences.</span></span>

* <span data-ttu-id="4a36a-162">Elimine las entradas de menú **Home** y **Privacy**, y agregue entradas para **About**, **Students**, **Courses**, **Instructors** y **Departments**.</span><span class="sxs-lookup"><span data-stu-id="4a36a-162">Delete the **Home** and **Privacy** menu entries, and add entries for **About**, **Students**, **Courses**, **Instructors**, and **Departments**.</span></span>

<span data-ttu-id="4a36a-163">Los cambios aparecen resaltados.</span><span class="sxs-lookup"><span data-stu-id="4a36a-163">The changes are highlighted.</span></span>

[!code-cshtml[Main](intro/samples/cu30/Pages/Shared/_Layout.cshtml?highlight=6,14,21-35,49)]

<span data-ttu-id="4a36a-164">En *Pages/Index.cshtml*, reemplace el contenido del archivo con el código siguiente para reemplazar el texto sobre ASP.NET Core con texto sobre esta aplicación:</span><span class="sxs-lookup"><span data-stu-id="4a36a-164">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET Core with text about this app:</span></span>

[!code-cshtml[Main](intro/samples/cu30/Pages/Index.cshtml)]

<span data-ttu-id="4a36a-165">Ejecute la aplicación para comprobar que aparece la página principal.</span><span class="sxs-lookup"><span data-stu-id="4a36a-165">Run the app to verify that the home page appears.</span></span>

## <a name="the-data-model"></a><span data-ttu-id="4a36a-166">El modelo de datos</span><span class="sxs-lookup"><span data-stu-id="4a36a-166">The data model</span></span>

<span data-ttu-id="4a36a-167">En las secciones siguientes se crea un modelo de datos:</span><span class="sxs-lookup"><span data-stu-id="4a36a-167">The following sections create a data model:</span></span>

![Diagrama del modelo de datos Course-Enrollment-Student](intro/_static/data-model-diagram.png)

<span data-ttu-id="4a36a-169">Un alumno se puede inscribir en cualquier número de cursos y un curso puede tener cualquier número de alumnos inscritos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-169">A student can enroll in any number of courses, and a course can have any number of students enrolled in it.</span></span>

## <a name="the-student-entity"></a><span data-ttu-id="4a36a-170">La entidad Student</span><span class="sxs-lookup"><span data-stu-id="4a36a-170">The Student entity</span></span>

![Diagrama de la entidad Student](intro/_static/student-entity.png)

* <span data-ttu-id="4a36a-172">Cree una carpeta *Models* en la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="4a36a-172">Create a *Models* folder in the project folder.</span></span> 

* <span data-ttu-id="4a36a-173">Cree *Models/Student.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="4a36a-173">Create *Models/Student.cs* with the following code:</span></span>

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Student.cs)]

<span data-ttu-id="4a36a-174">La propiedad `ID` se convierte en la columna de clave principal de la tabla de base de datos que se corresponde a esta clase.</span><span class="sxs-lookup"><span data-stu-id="4a36a-174">The `ID` property becomes the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="4a36a-175">De forma predeterminada, EF Core interpreta como la clave principal una propiedad que se denomine `ID` o `classnameID`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-175">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="4a36a-176">Por tanto, el nombre que se reconoce de forma automática para la clave principal de la clase `Student` es `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-176">So the alternative automatically recognized name for the `Student` class primary key is `StudentID`.</span></span>

<span data-ttu-id="4a36a-177">La propiedad `Enrollments` es una [propiedad de navegación](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="4a36a-177">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="4a36a-178">Las propiedades de navegación contienen otras entidades relacionadas con esta entidad.</span><span class="sxs-lookup"><span data-stu-id="4a36a-178">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="4a36a-179">En este caso, la propiedad `Enrollments` de una entidad `Student` contiene todas las entidades `Enrollment` que están relacionadas con esa instancia de Student.</span><span class="sxs-lookup"><span data-stu-id="4a36a-179">In this case, the `Enrollments` property of a `Student` entity holds all of the `Enrollment` entities that are related to that Student.</span></span> <span data-ttu-id="4a36a-180">Por ejemplo, si una fila Student de la base de datos tiene dos filas Enrollment relacionadas, la propiedad de navegación `Enrollments` contiene esas dos entidades Enrollment.</span><span class="sxs-lookup"><span data-stu-id="4a36a-180">For example, if a Student row in the database has two related Enrollment rows, the `Enrollments` navigation property contains those two Enrollment entities.</span></span> 

<span data-ttu-id="4a36a-181">En la base de datos, una fila Enrollment se relaciona con una fila Student si su columna StudentID contiene el valor de identificador del alumno.</span><span class="sxs-lookup"><span data-stu-id="4a36a-181">In the database, an Enrollment row is related to a Student row if its StudentID column contains the student's ID value.</span></span> <span data-ttu-id="4a36a-182">Por ejemplo, imagine que una fila Student tiene el identificador 1.</span><span class="sxs-lookup"><span data-stu-id="4a36a-182">For example, suppose a Student row has ID=1.</span></span> <span data-ttu-id="4a36a-183">Las filas Enrollment relacionadas tendrán StudentID = 1.</span><span class="sxs-lookup"><span data-stu-id="4a36a-183">Related Enrollment rows will have StudentID = 1.</span></span> <span data-ttu-id="4a36a-184">StudentID es una *clave externa* de la tabla Enrollment.</span><span class="sxs-lookup"><span data-stu-id="4a36a-184">StudentID is a *foreign key* in the Enrollment table.</span></span> 

<span data-ttu-id="4a36a-185">La propiedad `Enrollments` se define como `ICollection<Enrollment>` porque puede haber varias entidades Enrollment relacionadas.</span><span class="sxs-lookup"><span data-stu-id="4a36a-185">The `Enrollments` property is defined as `ICollection<Enrollment>` because there may be multiple related Enrollment entities.</span></span> <span data-ttu-id="4a36a-186">Puede usar otros tipos de colección, como `List<Enrollment>` o `HashSet<Enrollment>`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-186">You can use other collection types, such as `List<Enrollment>` or `HashSet<Enrollment>`.</span></span> <span data-ttu-id="4a36a-187">Cuando se usa `ICollection<Enrollment>`, EF Core crea una colección `HashSet<Enrollment>` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="4a36a-187">When `ICollection<Enrollment>` is used, EF Core creates a `HashSet<Enrollment>` collection by default.</span></span>

## <a name="the-enrollment-entity"></a><span data-ttu-id="4a36a-188">La entidad Enrollment</span><span class="sxs-lookup"><span data-stu-id="4a36a-188">The Enrollment entity</span></span>

![Diagrama de la entidad Enrollment](intro/_static/enrollment-entity.png)

<span data-ttu-id="4a36a-190">Cree *Models/Enrollment.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="4a36a-190">Create *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Enrollment.cs)]

<span data-ttu-id="4a36a-191">La propiedad `EnrollmentID` es la clave principal; esta entidad usa el patrón `classnameID` en lugar de `ID` por sí solo.</span><span class="sxs-lookup"><span data-stu-id="4a36a-191">The `EnrollmentID` property is the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself.</span></span> <span data-ttu-id="4a36a-192">Para un modelo de datos de producción, elija un patrón y úselo de forma coherente.</span><span class="sxs-lookup"><span data-stu-id="4a36a-192">For a production data model, choose one pattern and use it consistently.</span></span> <span data-ttu-id="4a36a-193">En este tutorial se usan los dos simplemente para ilustrar el trabajo.</span><span class="sxs-lookup"><span data-stu-id="4a36a-193">This tutorial uses both just to illustrate that both work.</span></span> <span data-ttu-id="4a36a-194">El uso de `ID` sin `classname` facilita la implementación de algunos tipos de cambios del modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-194">Using `ID` without `classname` makes it easier to implement some kinds of data model changes.</span></span>

<span data-ttu-id="4a36a-195">La propiedad `Grade` es una `enum`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-195">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="4a36a-196">El signo de interrogación después de la declaración de tipo `Grade` indica que la propiedad `Grade` [acepta valores NULL](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/).</span><span class="sxs-lookup"><span data-stu-id="4a36a-196">The question mark after the `Grade` type declaration indicates that the `Grade` property is [nullable](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/).</span></span> <span data-ttu-id="4a36a-197">Una calificación que sea NULL es diferente de una calificación que sea cero; NULL significa que no se conoce una calificación o que todavía no se ha asignado.</span><span class="sxs-lookup"><span data-stu-id="4a36a-197">A grade that's null is different from a zero grade&mdash;null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="4a36a-198">La propiedad `StudentID` es una clave externa y la propiedad de navegación correspondiente es `Student`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-198">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="4a36a-199">Una entidad `Enrollment` está asociada con una entidad `Student`, por lo que la propiedad contiene una única entidad `Student`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-199">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span>

<span data-ttu-id="4a36a-200">La propiedad `CourseID` es una clave externa y la propiedad de navegación correspondiente es `Course`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-200">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="4a36a-201">Una entidad `Enrollment` está asociada con una entidad `Course`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-201">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="4a36a-202">EF Core interpreta una propiedad como una clave externa si se denomina `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-202">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="4a36a-203">Por ejemplo, `StudentID` es la clave externa para la propiedad de navegación `Student`, ya que la clave principal de la entidad `Student` es `ID`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-203">For example,`StudentID` is the foreign key for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="4a36a-204">Las propiedades de clave externa también se pueden denominar `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-204">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="4a36a-205">Por ejemplo `CourseID`, dado que la clave principal de la entidad `Course` es `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-205">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

## <a name="the-course-entity"></a><span data-ttu-id="4a36a-206">La entidad Course</span><span class="sxs-lookup"><span data-stu-id="4a36a-206">The Course entity</span></span>

![Diagrama de la entidad Course](intro/_static/course-entity.png)

<span data-ttu-id="4a36a-208">Cree *Models/Course.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="4a36a-208">Create *Models/Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Course.cs)]

<span data-ttu-id="4a36a-209">La propiedad `Enrollments` es una propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="4a36a-209">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="4a36a-210">Una entidad `Course` puede estar relacionada con cualquier número de entidades `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-210">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="4a36a-211">El atributo `DatabaseGenerated` permite que la aplicación especifique la clave principal en lugar de hacer que la base de datos la genere.</span><span class="sxs-lookup"><span data-stu-id="4a36a-211">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the database generate it.</span></span>

<span data-ttu-id="4a36a-212">Compile el proyecto para comprobar que no hay errores de compilación.</span><span class="sxs-lookup"><span data-stu-id="4a36a-212">Build the project to validate that there are no compiler errors.</span></span>

## <a name="scaffold-student-pages"></a><span data-ttu-id="4a36a-213">Scaffolding de las páginas Student</span><span class="sxs-lookup"><span data-stu-id="4a36a-213">Scaffold Student pages</span></span>

<span data-ttu-id="4a36a-214">En esta sección, se usa la herramienta de scaffolding de ASP.NET Core para generar lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="4a36a-214">In this section, you use the ASP.NET Core scaffolding tool to generate:</span></span>

* <span data-ttu-id="4a36a-215">Una clase de *contexto* de EF Core.</span><span class="sxs-lookup"><span data-stu-id="4a36a-215">An EF Core *context* class.</span></span> <span data-ttu-id="4a36a-216">El contexto es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos determinado.</span><span class="sxs-lookup"><span data-stu-id="4a36a-216">The context is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="4a36a-217">Se deriva de la clase `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-217">It derives from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>
* <span data-ttu-id="4a36a-218">Páginas de Razor que controlan las operaciones de creación, lectura, actualización y eliminación (CRUD) de la entidad `Student`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-218">Razor pages that handle Create, Read, Update, and Delete (CRUD) operations for the `Student` entity.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4a36a-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4a36a-219">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4a36a-220">Cree una carpeta *Students* en la carpeta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="4a36a-220">Create a *Students* folder in the *Pages* folder.</span></span>
* <span data-ttu-id="4a36a-221">En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Pages/Students* y seleccione **Agregar** > **Nuevo elemento con scaffold**.</span><span class="sxs-lookup"><span data-stu-id="4a36a-221">In **Solution Explorer**, right-click the *Pages/Students* folder and select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="4a36a-222">En el cuadro de diálogo **Agregar scaffold**, seleccione **Razor Pages using Entity Framework (CRUD)** [Páginas de Razor Pages que usan Entity Framework (CRUD)] > **AGREGAR**.</span><span class="sxs-lookup"><span data-stu-id="4a36a-222">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>
* <span data-ttu-id="4a36a-223">En el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="4a36a-223">In the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
  * <span data-ttu-id="4a36a-224">En la lista desplegable **Clase de modelo**, seleccione **Student (ContosoUniversity.Models)** .</span><span class="sxs-lookup"><span data-stu-id="4a36a-224">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
  * <span data-ttu-id="4a36a-225">En la fila **Clase de contexto de datos**, seleccione el signo **+** (más).</span><span class="sxs-lookup"><span data-stu-id="4a36a-225">In the **Data context class** row, select the **+** (plus) sign.</span></span>
  * <span data-ttu-id="4a36a-226">Cambie el nombre del contexto de datos *ContosoUniversity.Models.ContosoUniversityContext* por *ContosoUniversity.Data.SchoolContext*.</span><span class="sxs-lookup"><span data-stu-id="4a36a-226">Change the data context name from *ContosoUniversity.Models.ContosoUniversityContext* to *ContosoUniversity.Data.SchoolContext*.</span></span>
  * <span data-ttu-id="4a36a-227">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="4a36a-227">Select **Add**.</span></span>

<span data-ttu-id="4a36a-228">Los paquetes siguientes se instalan de forma automática:</span><span class="sxs-lookup"><span data-stu-id="4a36a-228">The following packages are automatically installed:</span></span>

* `Microsoft.VisualStudio.Web.CodeGeneration.Design`
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Microsoft.Extensions.Logging.Debug`
* `Microsoft.EntityFrameworkCore.Tools`

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4a36a-229">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4a36a-229">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="4a36a-230">Ejecute los siguientes comandos de la CLI de .NET Core para instalar los paquetes NuGet necesarios:</span><span class="sxs-lookup"><span data-stu-id="4a36a-230">Run the following .NET Core CLI commands to install required NuGet packages:</span></span>

  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.SQLite --version 3.0.0-*
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
  dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
  dotnet add package Microsoft.EntityFrameworkCore.Tools --version 3.0.0-*
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
  dotnet add package Microsoft.Extensions.Logging.Debug --version 3.0.0-*
  ```

  <span data-ttu-id="4a36a-231">El paquete Microsoft.VisualStudio.Web.CodeGeneration.Design es necesario para scaffolding.</span><span class="sxs-lookup"><span data-stu-id="4a36a-231">The Microsoft.VisualStudio.Web.CodeGeneration.Design package is required for scaffolding.</span></span> <span data-ttu-id="4a36a-232">Aunque en la aplicación no se usará SQL Server, la herramienta de scaffolding necesita el paquete de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4a36a-232">Although the app won't use SQL Server, the scaffolding tool needs the SQL Server package.</span></span>

* <span data-ttu-id="4a36a-233">Cree una carpeta *Pages/Students*.</span><span class="sxs-lookup"><span data-stu-id="4a36a-233">Create a *Pages/Students* folder.</span></span>

* <span data-ttu-id="4a36a-234">Ejecute el comando siguiente para instalar la [herramienta de scaffolding aspnet-codegenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span><span class="sxs-lookup"><span data-stu-id="4a36a-234">Run the following command to install the [aspnet-codegenerator scaffolding tool](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-aspnet-codegenerator --version 3.0.0-*
  ```

* <span data-ttu-id="4a36a-235">Ejecute el comando siguiente para aplicar scaffolding a las páginas Student.</span><span class="sxs-lookup"><span data-stu-id="4a36a-235">Run the following command to scaffold Student pages.</span></span>

  <span data-ttu-id="4a36a-236">**En Windows**</span><span class="sxs-lookup"><span data-stu-id="4a36a-236">**On Windows**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
  ```

  <span data-ttu-id="4a36a-237">**En macOS o Linux**</span><span class="sxs-lookup"><span data-stu-id="4a36a-237">**On macOS or Linux**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
  ```

---

<span data-ttu-id="4a36a-238">Si tiene un problema con el paso anterior, compile el proyecto y vuelva a intentar el paso de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="4a36a-238">If you have a problem with the preceding step, build the project and retry the scaffold step.</span></span>

<span data-ttu-id="4a36a-239">El proceso de scaffolding:</span><span class="sxs-lookup"><span data-stu-id="4a36a-239">The scaffolding process:</span></span>

* <span data-ttu-id="4a36a-240">Crea páginas de Razor en la carpeta *Pages/Students*:</span><span class="sxs-lookup"><span data-stu-id="4a36a-240">Creates Razor pages in the *Pages/Students* folder:</span></span>
  * <span data-ttu-id="4a36a-241">*Create.cshtml* y *Create.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="4a36a-241">*Create.cshtml* and *Create.cshtml.cs*</span></span>
  * <span data-ttu-id="4a36a-242">*Delete.cshtml* y *Delete.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="4a36a-242">*Delete.cshtml* and *Delete.cshtml.cs*</span></span>
  * <span data-ttu-id="4a36a-243">*Details.cshtml* y *Details.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="4a36a-243">*Details.cshtml* and *Details.cshtml.cs*</span></span>
  * <span data-ttu-id="4a36a-244">*Edit.cshtml* y *Edit.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="4a36a-244">*Edit.cshtml* and *Edit.cshtml.cs*</span></span>
  * <span data-ttu-id="4a36a-245">*Index.cshtml* e *Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="4a36a-245">*Index.cshtml* and *Index.cshtml.cs*</span></span>
* <span data-ttu-id="4a36a-246">Crea *Data/SchoolContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="4a36a-246">Creates *Data/SchoolContext.cs*.</span></span>
* <span data-ttu-id="4a36a-247">Agrega el contexto a la inserción de dependencias en *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="4a36a-247">Adds the context to dependency injection in *Startup.cs*.</span></span>
* <span data-ttu-id="4a36a-248">Agrega una cadena de conexión de base de datos a *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="4a36a-248">Adds a database connection string to *appsettings.json*.</span></span>

## <a name="database-connection-string"></a><span data-ttu-id="4a36a-249">Cadena de conexión de base de datos</span><span class="sxs-lookup"><span data-stu-id="4a36a-249">Database connection string</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4a36a-250">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4a36a-250">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4a36a-251">La cadena de conexión especifica [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="4a36a-251">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> 

[!code-json[Main](intro/samples/cu30/appsettings.json?highlight=11)]

<span data-ttu-id="4a36a-252">LocalDB es una versión ligera del motor de base de datos de SQL Server Express y está dirigida al desarrollo de aplicaciones, no al uso en producción.</span><span class="sxs-lookup"><span data-stu-id="4a36a-252">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="4a36a-253">De forma predeterminada, LocalDB crea archivos *.mdf* en el directorio `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-253">By default, LocalDB creates *.mdf* files in the `C:/Users/<user>` directory.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4a36a-254">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4a36a-254">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4a36a-255">Cambie la cadena de conexión para que apunte a un archivo de base de datos de SQLite denominado *CU.db*:</span><span class="sxs-lookup"><span data-stu-id="4a36a-255">Change the connection string to point to a SQLite database file named *CU.db*:</span></span>

[!code-json[Main](intro/samples/cu30/appsettingsSQLite.json?highlight=11)]

---

## <a name="update-the-database-context-class"></a><span data-ttu-id="4a36a-256">Actualización de la clase de contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="4a36a-256">Update the database context class</span></span>

<span data-ttu-id="4a36a-257">La clase principal que coordina la funcionalidad de EF Core para un modelo de datos determinado es la clase de contexto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-257">The main class that coordinates EF Core functionality for a given data model is the database context class.</span></span> <span data-ttu-id="4a36a-258">El contexto se deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="4a36a-258">The context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="4a36a-259">En el contexto se especifica qué entidades se incluyen en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-259">The context specifies which entities are included in the data model.</span></span> <span data-ttu-id="4a36a-260">En este proyecto, la clase se denomina `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-260">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="4a36a-261">Actualice *SchoolContext.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="4a36a-261">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/SchoolContext.cs?highlight=13-22)]

<span data-ttu-id="4a36a-262">El código resaltado crea una propiedad [DbSet\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para cada conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="4a36a-262">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="4a36a-263">En la terminología de EF Core:</span><span class="sxs-lookup"><span data-stu-id="4a36a-263">In EF Core terminology:</span></span>

* <span data-ttu-id="4a36a-264">Un conjunto de entidades normalmente se corresponde a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-264">An entity set typically corresponds to a database table.</span></span>
* <span data-ttu-id="4a36a-265">Una entidad se corresponde con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="4a36a-265">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="4a36a-266">Puesto que un conjunto de entidades contiene varias entidades, las propiedades DBSet deben ser nombres en plural.</span><span class="sxs-lookup"><span data-stu-id="4a36a-266">Since an entity set contains multiple entities, the DBSet properties should be plural names.</span></span> <span data-ttu-id="4a36a-267">Como la herramienta de scaffolding ha creado una instancia `Student` de DBSet, en este paso se cambia a `Students` en plural.</span><span class="sxs-lookup"><span data-stu-id="4a36a-267">Since the scaffolding tool created a`Student` DBSet, this step changes it to plural `Students`.</span></span> 

<span data-ttu-id="4a36a-268">Para que el código de Razor Pages coincida con el nuevo nombre de DBSet, realice un cambio global de `_context.Student` a `_context.Students` en todo el proyecto.</span><span class="sxs-lookup"><span data-stu-id="4a36a-268">To make the Razor Pages code match the new DBSet name, make a global change across the whole project of `_context.Student` to `_context.Students`.</span></span>  <span data-ttu-id="4a36a-269">Hay ocho repeticiones.</span><span class="sxs-lookup"><span data-stu-id="4a36a-269">There are 8 occurrences.</span></span>

<span data-ttu-id="4a36a-270">Compile el proyecto para comprobar que no haya errores del compilador.</span><span class="sxs-lookup"><span data-stu-id="4a36a-270">Build the project to verify there are no compiler errors.</span></span>

## <a name="startupcs"></a><span data-ttu-id="4a36a-271">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="4a36a-271">Startup.cs</span></span>

<span data-ttu-id="4a36a-272">ASP.NET Core integra la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="4a36a-272">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="4a36a-273">Los servicios (como el contexto de base de datos de EF Core) se registran con la inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4a36a-273">Services (such as the EF Core database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="4a36a-274">Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="4a36a-274">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="4a36a-275">El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="4a36a-275">The constructor code that gets a database context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="4a36a-276">La herramienta de scaffolding ha registrado de forma automática la clase de contexto con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="4a36a-276">The scaffolding tool automatically registered the context class with the dependency injection container.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4a36a-277">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4a36a-277">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4a36a-278">En `ConfigureServices`, el proveedor de scaffolding ha agregado la línea resaltada:</span><span class="sxs-lookup"><span data-stu-id="4a36a-278">In `ConfigureServices`, the highlighted lines were added by the scaffolder:</span></span>

  [!code-csharp[Main](intro/samples/cu30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4a36a-279">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4a36a-279">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="4a36a-280">En `ConfigureServices`, asegúrese de que el código agregado por el proveedor de scaffolding llama a `UseSqlite`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-280">In `ConfigureServices`, make sure the code added by the scaffolder calls `UseSqlite`.</span></span>

  [!code-csharp[Main](intro/samples/cu30/StartupSQLite.cs?name=snippet_ConfigureServices&highlight=5-6)]

---

<span data-ttu-id="4a36a-281">El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="4a36a-281">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="4a36a-282">Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="4a36a-282">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="4a36a-283">Creación de la base de datos</span><span class="sxs-lookup"><span data-stu-id="4a36a-283">Create the database</span></span>

<span data-ttu-id="4a36a-284">Actualice *Program.cs* para crear la base de datos si no existe:</span><span class="sxs-lookup"><span data-stu-id="4a36a-284">Update *Program.cs* to create the database if it doesn't exist:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Program.cs?highlight=1-2,14-18,21-38)]

<span data-ttu-id="4a36a-285">El método [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) no realiza ninguna acción si existe una base de datos para el contexto.</span><span class="sxs-lookup"><span data-stu-id="4a36a-285">The [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) method takes no action if a database for the context exists.</span></span> <span data-ttu-id="4a36a-286">Si no existe ninguna base de datos, se crean la base de datos y el esquema.</span><span class="sxs-lookup"><span data-stu-id="4a36a-286">If no database exists, it creates the database and schema.</span></span> <span data-ttu-id="4a36a-287">`EnsureCreated` habilita el flujo de trabajo siguiente para controlar los cambios del modelo de datos:</span><span class="sxs-lookup"><span data-stu-id="4a36a-287">`EnsureCreated` enables the following workflow for handling data model changes:</span></span>

* <span data-ttu-id="4a36a-288">Se elimina la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-288">Delete the database.</span></span> <span data-ttu-id="4a36a-289">Se pierden los datos existentes.</span><span class="sxs-lookup"><span data-stu-id="4a36a-289">Any existing data is lost.</span></span>
* <span data-ttu-id="4a36a-290">Se cambia el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-290">Change the data model.</span></span> <span data-ttu-id="4a36a-291">Por ejemplo, se agrega un campo `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-291">For example, add an `EmailAddress` field.</span></span>
* <span data-ttu-id="4a36a-292">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4a36a-292">Run the app.</span></span>
* <span data-ttu-id="4a36a-293">`EnsureCreated` crea una base de datos con el esquema nuevo.</span><span class="sxs-lookup"><span data-stu-id="4a36a-293">`EnsureCreated` creates a database with the new schema.</span></span>

<span data-ttu-id="4a36a-294">Este flujo de trabajo funciona bien al principio de la fase de desarrollo cuando el esquema evoluciona rápidamente, siempre y cuando no sea necesario conservar los datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-294">This workflow works well early in development when the schema is rapidly evolving, as long as you don't need to preserve data.</span></span> <span data-ttu-id="4a36a-295">La situación es distinta cuando es necesario conservar los datos introducidos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-295">The situation is different when data that has been entered into the database needs to be preserved.</span></span> <span data-ttu-id="4a36a-296">En ese caso, use las migraciones.</span><span class="sxs-lookup"><span data-stu-id="4a36a-296">When that is the case, use migrations.</span></span>

<span data-ttu-id="4a36a-297">Más adelante en la serie de tutoriales, eliminará la base de datos creada por `EnsureCreated` y, en su lugar, usará las migraciones.</span><span class="sxs-lookup"><span data-stu-id="4a36a-297">Later in the tutorial series, you delete the database that was created by `EnsureCreated` and use migrations instead.</span></span> <span data-ttu-id="4a36a-298">Una base de datos creada por `EnsureCreated` no se puede actualizar mediante migraciones.</span><span class="sxs-lookup"><span data-stu-id="4a36a-298">A database that is created by `EnsureCreated` can't be updated by using migrations.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="4a36a-299">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="4a36a-299">Test the app</span></span>

* <span data-ttu-id="4a36a-300">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4a36a-300">Run the app.</span></span>
* <span data-ttu-id="4a36a-301">Haga clic en el vínculo **Students** y, después, en **Crear nuevo**.</span><span class="sxs-lookup"><span data-stu-id="4a36a-301">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="4a36a-302">Pruebe los vínculos Edit, Details y Delete.</span><span class="sxs-lookup"><span data-stu-id="4a36a-302">Test the Edit, Details, and Delete links.</span></span>

## <a name="seed-the-database"></a><span data-ttu-id="4a36a-303">Inicializar la base de datos</span><span class="sxs-lookup"><span data-stu-id="4a36a-303">Seed the database</span></span>

<span data-ttu-id="4a36a-304">El método `EnsureCreated` crea una base de datos vacía.</span><span class="sxs-lookup"><span data-stu-id="4a36a-304">The `EnsureCreated` method creates an empty database.</span></span> <span data-ttu-id="4a36a-305">En esta sección se agrega código que rellena la base de datos con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="4a36a-305">This section adds code that populates the database with test data.</span></span>

<span data-ttu-id="4a36a-306">Cree *Data/DbInitializer.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="4a36a-306">Create *Data/DbInitializer.cs* with the following code:</span></span>

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/DbInitializer.cs)]

  <span data-ttu-id="4a36a-307">El código comprueba si hay alumnos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-307">The code checks if there are any students in the database.</span></span> <span data-ttu-id="4a36a-308">Si no hay ningún alumno, agrega datos de prueba a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-308">If there are no students, it adds test data to the database.</span></span> <span data-ttu-id="4a36a-309">Crea los datos de prueba en matrices en lugar de colecciones `List<T>` para optimizar el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="4a36a-309">It creates the test data in arrays rather than `List<T>` collections to optimize performance.</span></span>

* <span data-ttu-id="4a36a-310">En *Program.cs*, reemplace la llamada a `EnsureCreated` con una llamada a `DbInitializer.Initialize`:</span><span class="sxs-lookup"><span data-stu-id="4a36a-310">In *Program.cs*, replace the `EnsureCreated` call with a `DbInitializer.Initialize` call:</span></span>

  ```csharp
  // context.Database.EnsureCreated();
  DbInitializer.Initialize(context);
  ````

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4a36a-311">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4a36a-311">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4a36a-312">Detenga la aplciación si se está ejecutando y ejecute el comando siguiente en la **Consola del Administrador de paquetes** (PMC):</span><span class="sxs-lookup"><span data-stu-id="4a36a-312">Stop the app if it's running, and run the following command in the **Package Manager Console** (PMC):</span></span>

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4a36a-313">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4a36a-313">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="4a36a-314">Detenga la aplicación si se está ejecutando y elimine el archivo *CU.db*.</span><span class="sxs-lookup"><span data-stu-id="4a36a-314">Stop the app if it's running, and delete the *CU.db* file.</span></span>

---

* <span data-ttu-id="4a36a-315">Reinicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4a36a-315">Restart the app.</span></span>

* <span data-ttu-id="4a36a-316">Seleccione la página Students para ver los datos inicializados.</span><span class="sxs-lookup"><span data-stu-id="4a36a-316">Select the Students page to see the seeded data.</span></span>

## <a name="view-the-database"></a><span data-ttu-id="4a36a-317">Consulta la base de datos</span><span class="sxs-lookup"><span data-stu-id="4a36a-317">View the database</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4a36a-318">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4a36a-318">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4a36a-319">Abra el **Explorador de objetos de SQL Server** (SSOX) desde el menú **Vista** en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4a36a-319">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
* <span data-ttu-id="4a36a-320">En SSOX, seleccione **(localdb)\MSSQLLocalDB > Bases de datos > SchoolContext-{GUID}** .</span><span class="sxs-lookup"><span data-stu-id="4a36a-320">In SSOX, select **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span></span> <span data-ttu-id="4a36a-321">El nombre de la base de datos se genera a partir del nombre de contexto proporcionado anteriormente, más un guión y un GUID.</span><span class="sxs-lookup"><span data-stu-id="4a36a-321">The database name is generated from the context name you provided earlier plus a dash and a GUID.</span></span>
* <span data-ttu-id="4a36a-322">Expanda el nodo **Tablas**.</span><span class="sxs-lookup"><span data-stu-id="4a36a-322">Expand the **Tables** node.</span></span>
* <span data-ttu-id="4a36a-323">Haga clic con el botón derecho en la tabla **Student** y haga clic en **Ver datos** para ver las columnas que se crearon y las filas que se insertaron en la tabla.</span><span class="sxs-lookup"><span data-stu-id="4a36a-323">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>
* <span data-ttu-id="4a36a-324">Haga clic con el botón derecho en la tabla **Student** y haga clic en **Ver código** para ver cómo el modelo `Student` se asigna al esquema de tabla `Student`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-324">Right-click the **Student** table and click **View Code** to see how the `Student` model maps to the `Student` table schema.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4a36a-325">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4a36a-325">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4a36a-326">Use la herramienta SQLite para ver el esquema de la base de datos y los datos inicializados.</span><span class="sxs-lookup"><span data-stu-id="4a36a-326">Use your SQLite tool to view the database schema and seeded data.</span></span> <span data-ttu-id="4a36a-327">El archivo de base de datos se denomina *CU.db* y se encuentra en la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="4a36a-327">The database file is named *CU.db* and is located in the project folder.</span></span>

---

## <a name="asynchronous-code"></a><span data-ttu-id="4a36a-328">Código asincrónico</span><span class="sxs-lookup"><span data-stu-id="4a36a-328">Asynchronous code</span></span>

<span data-ttu-id="4a36a-329">La programación asincrónica es el modo predeterminado de ASP.NET Core y EF Core.</span><span class="sxs-lookup"><span data-stu-id="4a36a-329">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="4a36a-330">Un servidor web tiene un número limitado de subprocesos disponibles y, en situaciones de carga alta, es posible que todos los subprocesos disponibles estén en uso.</span><span class="sxs-lookup"><span data-stu-id="4a36a-330">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="4a36a-331">Cuando esto ocurre, el servidor no puede procesar nuevas solicitudes hasta que los subprocesos se liberen.</span><span class="sxs-lookup"><span data-stu-id="4a36a-331">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="4a36a-332">Con el código sincrónico, se pueden acumular muchos subprocesos mientras no estén realizando ningún trabajo porque están a la espera de que finalice la E/S.</span><span class="sxs-lookup"><span data-stu-id="4a36a-332">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="4a36a-333">Con el código asincrónico, cuando un proceso está a la espera de que finalice la E/S, se libera su subproceso para el que el servidor lo use para el procesamiento de otras solicitudes.</span><span class="sxs-lookup"><span data-stu-id="4a36a-333">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="4a36a-334">Como resultado, el código asincrónico permite que los recursos de servidor se usen de forma más eficaz y el servidor pueda administrar más tráfico sin retrasos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-334">As a result, asynchronous code enables server resources to be used more efficiently, and the server can handle more traffic without delays.</span></span>

<span data-ttu-id="4a36a-335">El código asincrónico introduce una pequeña cantidad de sobrecarga en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="4a36a-335">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="4a36a-336">En situaciones de poco tráfico, la disminución del rendimiento es insignificante, mientras que en situaciones de tráfico elevado, la posible mejora del rendimiento es importante.</span><span class="sxs-lookup"><span data-stu-id="4a36a-336">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="4a36a-337">En el código siguiente, la palabra clave [async](/dotnet/csharp/language-reference/keywords/async), el valor devuelto `Task<T>`, la palabra clave `await` y el método `ToListAsync` hacen que el código se ejecute de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="4a36a-337">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

```csharp
public async Task OnGetAsync()
{
    Students = await _context.Students.ToListAsync();
}
```

* <span data-ttu-id="4a36a-338">La palabra clave `async` indica al compilador que:</span><span class="sxs-lookup"><span data-stu-id="4a36a-338">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="4a36a-339">Genere devoluciones de llamada para partes del cuerpo del método.</span><span class="sxs-lookup"><span data-stu-id="4a36a-339">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="4a36a-340">Cree el objeto [Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) que se devuelve.</span><span class="sxs-lookup"><span data-stu-id="4a36a-340">Create the [Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) object that's returned.</span></span>
* <span data-ttu-id="4a36a-341">El tipo devuelto `Task<T>` representa el trabajo en curso.</span><span class="sxs-lookup"><span data-stu-id="4a36a-341">The `Task<T>` return type represents ongoing work.</span></span>
* <span data-ttu-id="4a36a-342">La palabra clave `await` hace que el compilador divida el método en dos partes.</span><span class="sxs-lookup"><span data-stu-id="4a36a-342">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="4a36a-343">La primera parte termina con la operación que se inició de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="4a36a-343">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="4a36a-344">La segunda parte se coloca en un método de devolución de llamada que se llama cuando finaliza la operación.</span><span class="sxs-lookup"><span data-stu-id="4a36a-344">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="4a36a-345">`ToListAsync` es la versión asincrónica del método de extensión `ToList`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-345">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="4a36a-346">Algunos aspectos que tener en cuenta al escribir código asincrónico en el que se usa EF Core son los siguientes:</span><span class="sxs-lookup"><span data-stu-id="4a36a-346">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="4a36a-347">Solo se ejecutan de forma asincrónica las instrucciones que hacen que las consultas o los comandos se envíen a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-347">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="4a36a-348">Esto incluye `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` y `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-348">That includes `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="4a36a-349">No incluye las instrucciones que solo cambian una `IQueryable`, como `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-349">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="4a36a-350">Un contexto de EF Core no es seguro para subprocesos: no intente realizar varias operaciones en paralelo.</span><span class="sxs-lookup"><span data-stu-id="4a36a-350">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="4a36a-351">Para aprovechar las ventajas de rendimiento del código asincrónico, compruebe que en los paquetes de biblioteca (por ejemplo para la paginación) se usa async si llaman a métodos de EF Core que envían consultas a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-351">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the database.</span></span>

<span data-ttu-id="4a36a-352">Para obtener más información sobre la programación asincrónica en .NET, vea [Programación asincrónica](/dotnet/standard/async) y [Programación asincrónica con async y await](/dotnet/csharp/programming-guide/concepts/async/).</span><span class="sxs-lookup"><span data-stu-id="4a36a-352">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4a36a-353">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="4a36a-353">Next steps</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4a36a-354">Siguiente tutorial</span><span class="sxs-lookup"><span data-stu-id="4a36a-354">Next tutorial</span></span>](xref:data/ef-rp/crud)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="4a36a-355">En la aplicación web de ejemplo Contoso University se muestra cómo crear una aplicación web de Razor Pages de ASP.NET Core con Entity Framework (EF) Core.</span><span class="sxs-lookup"><span data-stu-id="4a36a-355">The Contoso University sample web app demonstrates how to create an ASP.NET Core Razor Pages app using Entity Framework (EF) Core.</span></span>

<span data-ttu-id="4a36a-356">La aplicación de ejemplo es un sitio web de una universidad ficticia, Contoso University.</span><span class="sxs-lookup"><span data-stu-id="4a36a-356">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="4a36a-357">Incluye funciones como la admisión de estudiantes, la creación de cursos y asignaciones de instructores.</span><span class="sxs-lookup"><span data-stu-id="4a36a-357">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="4a36a-358">Esta página es la primera de una serie de tutoriales en los que se explica cómo crear la aplicación de ejemplo Contoso University.</span><span class="sxs-lookup"><span data-stu-id="4a36a-358">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="4a36a-359">Descargue o vea la aplicación completa.</span><span class="sxs-lookup"><span data-stu-id="4a36a-359">Download or view the completed app.</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="4a36a-360">[Instrucciones de descarga](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="4a36a-360">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4a36a-361">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="4a36a-361">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4a36a-362">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4a36a-362">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4a36a-363">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4a36a-363">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [](~/includes/2.1-SDK.md)]

---

<span data-ttu-id="4a36a-364">Familiaridad con las [Páginas de Razor](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="4a36a-364">Familiarity with [Razor Pages](xref:razor-pages/index).</span></span> <span data-ttu-id="4a36a-365">Los programadores nuevos deben completar [Introducción a las páginas de Razor en ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start) antes de empezar esta serie.</span><span class="sxs-lookup"><span data-stu-id="4a36a-365">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="4a36a-366">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="4a36a-366">Troubleshooting</span></span>

<span data-ttu-id="4a36a-367">Si experimenta un problema que no puede resolver, por lo general podrá encontrar la solución si compara el código con el [proyecto completado](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="4a36a-367">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="4a36a-368">Una buena forma de obtener ayuda consiste en publicar una pregunta en [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) para [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) o [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="4a36a-368">A good way to get help is by posting a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="4a36a-369">La aplicación web Contoso University</span><span class="sxs-lookup"><span data-stu-id="4a36a-369">The Contoso University web app</span></span>

<span data-ttu-id="4a36a-370">La aplicación compilada en estos tutoriales es un sitio web básico de una universidad.</span><span class="sxs-lookup"><span data-stu-id="4a36a-370">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="4a36a-371">Los usuarios pueden ver y actualizar la información de estudiantes, cursos e instructores.</span><span class="sxs-lookup"><span data-stu-id="4a36a-371">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="4a36a-372">Estas son algunas de las pantallas que se crean en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="4a36a-372">Here are a few of the screens created in the tutorial.</span></span>

![Página de índice de Students](intro/_static/students-index.png)

![Página de edición de estudiantes](intro/_static/student-edit.png)

<span data-ttu-id="4a36a-375">El estilo de la interfaz de usuario de este sitio se mantiene fiel a lo que generan las plantillas integradas.</span><span class="sxs-lookup"><span data-stu-id="4a36a-375">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="4a36a-376">El tutorial se centra en EF Core con páginas de Razor, no en la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="4a36a-376">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-the-contosouniversity-razor-pages-web-app"></a><span data-ttu-id="4a36a-377">Creación de la aplicación web de Razor Pages ContosoUniversity</span><span class="sxs-lookup"><span data-stu-id="4a36a-377">Create the ContosoUniversity Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4a36a-378">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4a36a-378">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4a36a-379">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="4a36a-379">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="4a36a-380">Cree una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4a36a-380">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="4a36a-381">Asigne el nombre **ContosoUniversity** al proyecto.</span><span class="sxs-lookup"><span data-stu-id="4a36a-381">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="4a36a-382">Es importante que el nombre del proyecto sea *ContosoUniversity* para que coincidan los espacios de nombres al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="4a36a-382">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
* <span data-ttu-id="4a36a-383">Seleccione **ASP.NET Core 2.1** en la lista desplegable y, luego, **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="4a36a-383">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

<span data-ttu-id="4a36a-384">Para ver las imágenes de los pasos anteriores, consulte [Creación de una aplicación web de Razor](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span><span class="sxs-lookup"><span data-stu-id="4a36a-384">For images of the preceding steps, see [Create a Razor web app](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span></span>
<span data-ttu-id="4a36a-385">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4a36a-385">Run the app.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4a36a-386">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4a36a-386">Visual Studio Code</span></span>](#tab/visual-studio-code)

```dotnetcli
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

---

## <a name="set-up-the-site-style"></a><span data-ttu-id="4a36a-387">Configurar el estilo del sitio</span><span class="sxs-lookup"><span data-stu-id="4a36a-387">Set up the site style</span></span>

<span data-ttu-id="4a36a-388">Con algunos cambios se configura el menú del sitio, el diseño y la página principal.</span><span class="sxs-lookup"><span data-stu-id="4a36a-388">A few changes set up the site menu, layout, and home page.</span></span> <span data-ttu-id="4a36a-389">Actualice *Pages/Shared/_Layout.cshtml* con los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="4a36a-389">Update *Pages/Shared/_Layout.cshtml* with the following changes:</span></span>

* <span data-ttu-id="4a36a-390">Cambie todas las repeticiones de "ContosoUniversity" por "Contoso University".</span><span class="sxs-lookup"><span data-stu-id="4a36a-390">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="4a36a-391">Hay tres repeticiones.</span><span class="sxs-lookup"><span data-stu-id="4a36a-391">There are three occurrences.</span></span>

* <span data-ttu-id="4a36a-392">Agregue entradas de menú para **Students**, **Courses**, **Instructors** y **Departments**, y elimine la entrada de menú **Contact**.</span><span class="sxs-lookup"><span data-stu-id="4a36a-392">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="4a36a-393">Los cambios aparecen resaltados.</span><span class="sxs-lookup"><span data-stu-id="4a36a-393">The changes are highlighted.</span></span> <span data-ttu-id="4a36a-394">(*No* se muestra todo el marcado).</span><span class="sxs-lookup"><span data-stu-id="4a36a-394">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

<span data-ttu-id="4a36a-395">En *Pages/Index.cshtml*, reemplace el contenido del archivo con el código siguiente para reemplazar el texto sobre ASP.NET y MVC con texto sobre esta aplicación:</span><span class="sxs-lookup"><span data-stu-id="4a36a-395">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a><span data-ttu-id="4a36a-396">Crear el modelo de datos</span><span class="sxs-lookup"><span data-stu-id="4a36a-396">Create the data model</span></span>

<span data-ttu-id="4a36a-397">Cree las clases de entidad para la aplicación Contoso University.</span><span class="sxs-lookup"><span data-stu-id="4a36a-397">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="4a36a-398">Comience con las tres entidades siguientes:</span><span class="sxs-lookup"><span data-stu-id="4a36a-398">Start with the following three entities:</span></span>

![Diagrama del modelo de datos Course-Enrollment-Student](intro/_static/data-model-diagram.png)

<span data-ttu-id="4a36a-400">Hay una relación uno a varios entre las entidades `Student` y `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-400">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="4a36a-401">Hay una relación uno a varios entre las entidades `Course` y `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-401">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="4a36a-402">Un estudiante se puede inscribir en cualquier número de cursos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-402">A student can enroll in any number of courses.</span></span> <span data-ttu-id="4a36a-403">Un curso puede tener cualquier número de alumnos inscritos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-403">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="4a36a-404">En las secciones siguientes, se crea una clase para cada una de estas entidades.</span><span class="sxs-lookup"><span data-stu-id="4a36a-404">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="4a36a-405">La entidad Student</span><span class="sxs-lookup"><span data-stu-id="4a36a-405">The Student entity</span></span>

![Diagrama de la entidad Student](intro/_static/student-entity.png)

<span data-ttu-id="4a36a-407">Cree una carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="4a36a-407">Create a *Models* folder.</span></span> <span data-ttu-id="4a36a-408">En la carpeta *Models*, cree un archivo de clase denominado *Student.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="4a36a-408">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="4a36a-409">La propiedad `ID` se convierte en la columna de clave principal de la tabla de base de datos (DB) que corresponde a esta clase.</span><span class="sxs-lookup"><span data-stu-id="4a36a-409">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="4a36a-410">De forma predeterminada, EF Core interpreta como la clave principal una propiedad que se denomine `ID` o `classnameID`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-410">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="4a36a-411">En `classnameID`, `classname` es el nombre de la clase.</span><span class="sxs-lookup"><span data-stu-id="4a36a-411">In `classnameID`, `classname` is the name of the class.</span></span> <span data-ttu-id="4a36a-412">En el ejemplo anterior, la clave principal alternativa que se reconoce de forma automática es `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-412">The alternative automatically recognized primary key is `StudentID` in the preceding example.</span></span>

<span data-ttu-id="4a36a-413">La propiedad `Enrollments` es una [propiedad de navegación](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="4a36a-413">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="4a36a-414">Las propiedades de navegación se vinculan a otras entidades relacionadas con esta entidad.</span><span class="sxs-lookup"><span data-stu-id="4a36a-414">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="4a36a-415">En este caso, la propiedad `Enrollments` de una `Student entity` contiene todas las entidades `Enrollment` que están relacionadas con esa entidad `Student`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-415">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="4a36a-416">Por ejemplo, si una fila Student de la base de datos tiene dos filas Enrollment relacionadas, la propiedad de navegación `Enrollments` contiene esas dos entidades `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-416">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="4a36a-417">Una fila `Enrollment` relacionada es la que contiene el valor de clave principal de ese estudiante en la columna `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-417">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="4a36a-418">Por ejemplo, suponga que el estudiante con ID=1 tiene dos filas en la tabla `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-418">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="4a36a-419">La tabla `Enrollment` tiene dos filas con `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="4a36a-419">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="4a36a-420">`StudentID` es una clave externa en la tabla `Enrollment` que especifica el estudiante en la tabla `Student`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-420">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="4a36a-421">Si una propiedad de navegación puede contener varias entidades, la propiedad de navegación debe ser un tipo de lista, como `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-421">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="4a36a-422">Se puede especificar `ICollection<T>`, o bien un tipo como `List<T>` o `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-422">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="4a36a-423">Cuando se usa `ICollection<T>`, EF Core crea una colección `HashSet<T>` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="4a36a-423">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="4a36a-424">Las propiedades de navegación que contienen varias entidades proceden de relaciones de varios a varios y uno a varios.</span><span class="sxs-lookup"><span data-stu-id="4a36a-424">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="4a36a-425">La entidad Enrollment</span><span class="sxs-lookup"><span data-stu-id="4a36a-425">The Enrollment entity</span></span>

![Diagrama de la entidad Enrollment](intro/_static/enrollment-entity.png)

<span data-ttu-id="4a36a-427">En la carpeta *Models*, cree *Enrollment.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="4a36a-427">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="4a36a-428">La propiedad `EnrollmentID` es la clave principal.</span><span class="sxs-lookup"><span data-stu-id="4a36a-428">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="4a36a-429">En esta entidad se usa el patrón `classnameID` en lugar de `ID` como en la entidad `Student`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-429">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="4a36a-430">Normalmente, los desarrolladores eligen un patrón y lo usan en todo el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-430">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="4a36a-431">En un tutorial posterior, se muestra el uso de ID sin un nombre de clase para facilitar la implementación de la herencia en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-431">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="4a36a-432">La propiedad `Grade` es una `enum`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-432">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="4a36a-433">El signo de interrogación después de la declaración de tipo `Grade` indica que la propiedad `Grade` acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="4a36a-433">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="4a36a-434">Una calificación que sea NULL es diferente de una calificación que sea cero; NULL significa que no se conoce una calificación o que todavía no se ha asignado.</span><span class="sxs-lookup"><span data-stu-id="4a36a-434">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="4a36a-435">La propiedad `StudentID` es una clave externa y la propiedad de navegación correspondiente es `Student`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-435">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="4a36a-436">Una entidad `Enrollment` está asociada con una entidad `Student`, por lo que la propiedad contiene una única entidad `Student`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-436">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="4a36a-437">La entidad `Student` difiere de la propiedad de navegación `Student.Enrollments`, que contiene varias entidades `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-437">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="4a36a-438">La propiedad `CourseID` es una clave externa y la propiedad de navegación correspondiente es `Course`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-438">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="4a36a-439">Una entidad `Enrollment` está asociada con una entidad `Course`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-439">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="4a36a-440">EF Core interpreta una propiedad como una clave externa si se denomina `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-440">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="4a36a-441">Por ejemplo, `StudentID` para la propiedad de navegación `Student`, puesto que la clave principal de la entidad `Student` es `ID`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-441">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="4a36a-442">Las propiedades de clave externa también se pueden denominar `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-442">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="4a36a-443">Por ejemplo `CourseID`, dado que la clave principal de la entidad `Course` es `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-443">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="4a36a-444">La entidad Course</span><span class="sxs-lookup"><span data-stu-id="4a36a-444">The Course entity</span></span>

![Diagrama de la entidad Course](intro/_static/course-entity.png)

<span data-ttu-id="4a36a-446">En la carpeta *Models*, cree *Course.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="4a36a-446">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="4a36a-447">La propiedad `Enrollments` es una propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="4a36a-447">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="4a36a-448">Una entidad `Course` puede estar relacionada con cualquier número de entidades `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-448">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="4a36a-449">El atributo `DatabaseGenerated` permite que la aplicación especifique la clave principal en lugar de hacer que la base de datos la genere.</span><span class="sxs-lookup"><span data-stu-id="4a36a-449">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="scaffold-the-student-model"></a><span data-ttu-id="4a36a-450">Aplicación de scaffolding al modelo de alumnos</span><span class="sxs-lookup"><span data-stu-id="4a36a-450">Scaffold the student model</span></span>

<span data-ttu-id="4a36a-451">En esta sección, se aplica scaffolding al modelo de alumnos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-451">In this section, the student model is scaffolded.</span></span> <span data-ttu-id="4a36a-452">Es decir, la herramienta de scaffolding genera páginas para las operaciones de creación, lectura, actualización y eliminación (CRUD) del modelo de alumnos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-452">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the student model.</span></span>

* <span data-ttu-id="4a36a-453">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="4a36a-453">Build the project.</span></span>
* <span data-ttu-id="4a36a-454">Cree la carpeta *Pages/Students*.</span><span class="sxs-lookup"><span data-stu-id="4a36a-454">Create the *Pages/Students* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4a36a-455">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4a36a-455">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4a36a-456">En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Pages/Students* > **Agregar** > **Nuevo elemento con scanffold**.</span><span class="sxs-lookup"><span data-stu-id="4a36a-456">In **Solution Explorer**, right click on the *Pages/Students* folder > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="4a36a-457">En el cuadro de diálogo **Agregar scaffold**, seleccione **Razor Pages using Entity Framework (CRUD)** [Páginas de Razor Pages que usan Entity Framework (CRUD)] > **AGREGAR**.</span><span class="sxs-lookup"><span data-stu-id="4a36a-457">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

<span data-ttu-id="4a36a-458">Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="4a36a-458">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="4a36a-459">En la lista desplegable **Clase de modelo**, seleccione **Student (ContosoUniversity.Models)** .</span><span class="sxs-lookup"><span data-stu-id="4a36a-459">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
* <span data-ttu-id="4a36a-460">En la fila **Clase de contexto de datos**, haga clic en el signo **+** (más) y cambie el nombre generado por **ContosoUniversity.Models.SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="4a36a-460">In the **Data context class** row, select the **+** (plus) sign and change the generated name to **ContosoUniversity.Models.SchoolContext**.</span></span>
* <span data-ttu-id="4a36a-461">En la lista desplegable **Clase de contexto de datos**, seleccione **ContosoUniversity.Models.SchoolContext**</span><span class="sxs-lookup"><span data-stu-id="4a36a-461">In the **Data context class** drop-down,  select **ContosoUniversity.Models.SchoolContext**</span></span>
* <span data-ttu-id="4a36a-462">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="4a36a-462">Select **Add**.</span></span>

![Cuadro de diálogo CRUD](intro/_static/s1.png)

<span data-ttu-id="4a36a-464">Si tiene algún problema con el paso anterior, consulte [Aplicar scaffolding al modelo de película](xref:tutorials/razor-pages/model#scaffold-the-movie-model).</span><span class="sxs-lookup"><span data-stu-id="4a36a-464">See [Scaffold the movie model](xref:tutorials/razor-pages/model#scaffold-the-movie-model) if you have a problem with the preceding step.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4a36a-465">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4a36a-465">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="4a36a-466">Ejecute los comandos siguientes para aplicar scaffolding al modelo de alumnos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-466">Run the following commands to scaffold the student model.</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
```

---

<span data-ttu-id="4a36a-467">El proceso de scaffolding ha creado y cambiado los archivos siguientes:</span><span class="sxs-lookup"><span data-stu-id="4a36a-467">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="4a36a-468">Archivos creados</span><span class="sxs-lookup"><span data-stu-id="4a36a-468">Files created</span></span>

* <span data-ttu-id="4a36a-469">*Pages/Students* Create, Delete, Details, Edit, Index.</span><span class="sxs-lookup"><span data-stu-id="4a36a-469">*Pages/Students* Create, Delete, Details, Edit, Index.</span></span>
* <span data-ttu-id="4a36a-470">*Data/SchoolContext.cs*</span><span class="sxs-lookup"><span data-stu-id="4a36a-470">*Data/SchoolContext.cs*</span></span>

### <a name="file-updates"></a><span data-ttu-id="4a36a-471">Actualizaciones de archivos</span><span class="sxs-lookup"><span data-stu-id="4a36a-471">File updates</span></span>

* <span data-ttu-id="4a36a-472">*Startup.cs*: en la sección siguiente se detallan los cambios realizados en este archivo.</span><span class="sxs-lookup"><span data-stu-id="4a36a-472">*Startup.cs* : Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="4a36a-473">*appsettings.json*: se agrega la cadena de conexión que se usa para conectarse a una base de datos local.</span><span class="sxs-lookup"><span data-stu-id="4a36a-473">*appsettings.json* : The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="4a36a-474">Examinar el contexto registrado con la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="4a36a-474">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="4a36a-475">ASP.NET Core integra la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="4a36a-475">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="4a36a-476">Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4a36a-476">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="4a36a-477">Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="4a36a-477">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="4a36a-478">El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="4a36a-478">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="4a36a-479">La herramienta de scaffolding creó de forma automática un contexto de base de datos y lo registró con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="4a36a-479">The scaffolding tool automatically created a DB Context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="4a36a-480">Examine el método `ConfigureServices` de *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="4a36a-480">Examine the `ConfigureServices` method in *Startup.cs*.</span></span> <span data-ttu-id="4a36a-481">El proveedor de scaffolding ha agregado la línea resaltada:</span><span class="sxs-lookup"><span data-stu-id="4a36a-481">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

<span data-ttu-id="4a36a-482">El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="4a36a-482">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="4a36a-483">Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="4a36a-483">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="update-main"></a><span data-ttu-id="4a36a-484">Actualización de main</span><span class="sxs-lookup"><span data-stu-id="4a36a-484">Update main</span></span>

<span data-ttu-id="4a36a-485">En *Program.cs*, modifique el método `Main` para que haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="4a36a-485">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="4a36a-486">Obtener una instancia del contexto de base de datos desde el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="4a36a-486">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="4a36a-487">Llame a [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span><span class="sxs-lookup"><span data-stu-id="4a36a-487">Call the  [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span></span>
* <span data-ttu-id="4a36a-488">Elimine el contexto cuando finalice el método `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-488">Dispose the context when the `EnsureCreated` method completes.</span></span>

<span data-ttu-id="4a36a-489">En el código siguiente se muestra el archivo *Program.cs* actualizado.</span><span class="sxs-lookup"><span data-stu-id="4a36a-489">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

<span data-ttu-id="4a36a-490">`EnsureCreated` garantiza la existencia de la base de datos para el contexto.</span><span class="sxs-lookup"><span data-stu-id="4a36a-490">`EnsureCreated` ensures that the database for the context exists.</span></span> <span data-ttu-id="4a36a-491">Si existe, no se realiza ninguna acción.</span><span class="sxs-lookup"><span data-stu-id="4a36a-491">If it exists, no action is taken.</span></span> <span data-ttu-id="4a36a-492">Si no existe, se crean la base de datos y todo su esquema.</span><span class="sxs-lookup"><span data-stu-id="4a36a-492">If it does not exist, then the database and all its schema are created.</span></span> <span data-ttu-id="4a36a-493">En `EnsureCreated` no se usan migraciones para crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-493">`EnsureCreated` does not use migrations to create the database.</span></span> <span data-ttu-id="4a36a-494">Una base de datos que se cree con `EnsureCreated` no se podrá actualizar más adelante mediante las migraciones.</span><span class="sxs-lookup"><span data-stu-id="4a36a-494">A database that is created with `EnsureCreated` cannot be later updated using migrations.</span></span>

<span data-ttu-id="4a36a-495">`EnsureCreated` se llama durante el inicio de la aplicación, lo que permite el flujo de trabajo siguiente:</span><span class="sxs-lookup"><span data-stu-id="4a36a-495">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="4a36a-496">Se elimina la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-496">Delete the DB.</span></span>
* <span data-ttu-id="4a36a-497">Se cambia el esquema de base de datos (por ejemplo, se agrega un campo `EmailAddress`).</span><span class="sxs-lookup"><span data-stu-id="4a36a-497">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="4a36a-498">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4a36a-498">Run the app.</span></span>
* <span data-ttu-id="4a36a-499">`EnsureCreated` crea una base de datos con la columna `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-499">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

<span data-ttu-id="4a36a-500">`EnsureCreated` es útil al principio del desarrollo, cuando el esquema evoluciona rápidamente.</span><span class="sxs-lookup"><span data-stu-id="4a36a-500">`EnsureCreated` is convenient early in development when the schema is rapidly evolving.</span></span> <span data-ttu-id="4a36a-501">Más adelante, en el tutorial se elimina la base de datos y se usan las migraciones.</span><span class="sxs-lookup"><span data-stu-id="4a36a-501">Later in the tutorial the DB is deleted and migrations are used.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="4a36a-502">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="4a36a-502">Test the app</span></span>

<span data-ttu-id="4a36a-503">Ejecute la aplicación y acepte la directiva de cookies.</span><span class="sxs-lookup"><span data-stu-id="4a36a-503">Run the app and accept the cookie policy.</span></span> <span data-ttu-id="4a36a-504">Esta aplicación no conserva información de carácter personal.</span><span class="sxs-lookup"><span data-stu-id="4a36a-504">This app doesn't keep personal information.</span></span> <span data-ttu-id="4a36a-505">Puede obtener más información sobre la directiva de cookies en [Compatibilidad con el Reglamento general de protección de datos (RGPD) de la UE](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="4a36a-505">You can read about the cookie policy at [EU General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span>

* <span data-ttu-id="4a36a-506">Haga clic en el vínculo **Students** y, después, en **Crear nuevo**.</span><span class="sxs-lookup"><span data-stu-id="4a36a-506">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="4a36a-507">Pruebe los vínculos Edit, Details y Delete.</span><span class="sxs-lookup"><span data-stu-id="4a36a-507">Test the Edit, Details, and Delete links.</span></span>

## <a name="examine-the-schoolcontext-db-context"></a><span data-ttu-id="4a36a-508">Examinar el contexto de base de datos SchoolContext</span><span class="sxs-lookup"><span data-stu-id="4a36a-508">Examine the SchoolContext DB context</span></span>

<span data-ttu-id="4a36a-509">La clase principal que coordina la funcionalidad de EF Core para un modelo de datos determinado es la clase de contexto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-509">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="4a36a-510">El contexto de datos se deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="4a36a-510">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="4a36a-511">En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-511">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="4a36a-512">En este proyecto, la clase se denomina `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-512">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="4a36a-513">Actualice *SchoolContext.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="4a36a-513">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

<span data-ttu-id="4a36a-514">El código resaltado crea una propiedad [DbSet\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para cada conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="4a36a-514">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="4a36a-515">En la terminología de EF Core:</span><span class="sxs-lookup"><span data-stu-id="4a36a-515">In EF Core terminology:</span></span>

* <span data-ttu-id="4a36a-516">Un conjunto de entidades normalmente se corresponde a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-516">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="4a36a-517">Una entidad se corresponde con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="4a36a-517">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="4a36a-518">`DbSet<Enrollment>` y `DbSet<Course>` se pueden omitir.</span><span class="sxs-lookup"><span data-stu-id="4a36a-518">`DbSet<Enrollment>` and `DbSet<Course>` could be omitted.</span></span> <span data-ttu-id="4a36a-519">EF Core las incluye implícitamente porque la entidad `Student` hace referencia a la entidad `Enrollment` y la entidad `Enrollment` hace referencia a la entidad `Course`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-519">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="4a36a-520">Para este tutorial, conserve `DbSet<Enrollment>` y `DbSet<Course>` en el `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-520">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="4a36a-521">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="4a36a-521">SQL Server Express LocalDB</span></span>

<span data-ttu-id="4a36a-522">La cadena de conexión especifica [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="4a36a-522">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="4a36a-523">LocalDB es una versión ligera del motor de base de datos de SQL Server Express y está dirigida al desarrollo de aplicaciones, no al uso en producción.</span><span class="sxs-lookup"><span data-stu-id="4a36a-523">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="4a36a-524">LocalDB se inicia a petición y se ejecuta en modo de usuario, sin necesidad de una configuración compleja.</span><span class="sxs-lookup"><span data-stu-id="4a36a-524">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="4a36a-525">De forma predeterminada, LocalDB crea archivos de base de datos *.mdf* en el directorio `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-525">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="4a36a-526">Agregar código para inicializar la base de datos con datos de prueba</span><span class="sxs-lookup"><span data-stu-id="4a36a-526">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="4a36a-527">EF Core crea una base de datos vacía.</span><span class="sxs-lookup"><span data-stu-id="4a36a-527">EF Core creates an empty DB.</span></span> <span data-ttu-id="4a36a-528">En esta sección, se escribe un método `Initialize` para rellenarlo con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="4a36a-528">In this section, an `Initialize` method is written to populate it with test data.</span></span>

<span data-ttu-id="4a36a-529">En la carpeta *Data*, cree un archivo de clase denominado *DbInitializer.cs* y agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="4a36a-529">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="4a36a-530">Nota: El código anterior usa `Models` para el espacio de nombres (`namespace ContosoUniversity.Models`) en lugar de `Data`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-530">Note: The preceding code uses `Models` for the namespace (`namespace ContosoUniversity.Models`) rather than `Data`.</span></span> <span data-ttu-id="4a36a-531">`Models` es coherente con el código generado por el proveedor de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="4a36a-531">`Models` is consistent with the scaffolder-generated code.</span></span> <span data-ttu-id="4a36a-532">Para obtener más información, consulte [este problema de scaffolding de GitHub](https://github.com/aspnet/Scaffolding/issues/822).</span><span class="sxs-lookup"><span data-stu-id="4a36a-532">For more information, see [this GitHub scaffolding issue](https://github.com/aspnet/Scaffolding/issues/822).</span></span>

<span data-ttu-id="4a36a-533">El código comprueba si hay estudiantes en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-533">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="4a36a-534">Si no hay alumnos en la base de datos, se inicializa con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="4a36a-534">If there are no students in the DB, the DB is initialized with test data.</span></span> <span data-ttu-id="4a36a-535">Carga los datos de prueba en matrices en lugar de colecciones `List<T>` para optimizar el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="4a36a-535">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="4a36a-536">El método `EnsureCreated` crea automáticamente la base de datos para el contexto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-536">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="4a36a-537">Si la base de datos existe, `EnsureCreated` vuelve sin modificarla.</span><span class="sxs-lookup"><span data-stu-id="4a36a-537">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="4a36a-538">En *Program.cs*, modifique el método `Main` para que llame a `Initialize`:</span><span class="sxs-lookup"><span data-stu-id="4a36a-538">In *Program.cs*, modify the `Main` method to call `Initialize`:</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4a36a-539">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4a36a-539">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4a36a-540">Detenga la aplciación si se está ejecutando y ejecute el comando siguiente en la **Consola del Administrador de paquetes** (PMC):</span><span class="sxs-lookup"><span data-stu-id="4a36a-540">Stop the app if it's running, and run the following command in the **Package Manager Console** (PMC):</span></span>

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4a36a-541">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4a36a-541">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="4a36a-542">Detenga la aplicación si se está ejecutando y elimine el archivo *CU.db*.</span><span class="sxs-lookup"><span data-stu-id="4a36a-542">Stop the app if it's running, and delete the *CU.db* file.</span></span>

---

## <a name="view-the-db"></a><span data-ttu-id="4a36a-543">Ver la base de datos</span><span class="sxs-lookup"><span data-stu-id="4a36a-543">View the DB</span></span>

<span data-ttu-id="4a36a-544">El nombre de la base de datos se genera a partir del nombre de contexto proporcionado anteriormente, más un guión y un GUID.</span><span class="sxs-lookup"><span data-stu-id="4a36a-544">The database name is generated from the context name you provided earlier plus a dash and a GUID.</span></span> <span data-ttu-id="4a36a-545">Por lo tanto, el nombre de la base de datos será "SchoolContext-{GUID}".</span><span class="sxs-lookup"><span data-stu-id="4a36a-545">Thus, the database name will be "SchoolContext-{GUID}".</span></span> <span data-ttu-id="4a36a-546">El GUID será diferente para cada usuario.</span><span class="sxs-lookup"><span data-stu-id="4a36a-546">The GUID will be different for each user.</span></span>
<span data-ttu-id="4a36a-547">Abra el **Explorador de objetos de SQL Server** (SSOX) desde el menú **Vista** en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4a36a-547">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="4a36a-548">En SSOX, haga clic en **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}** .</span><span class="sxs-lookup"><span data-stu-id="4a36a-548">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span></span>

<span data-ttu-id="4a36a-549">Expanda el nodo **Tablas**.</span><span class="sxs-lookup"><span data-stu-id="4a36a-549">Expand the **Tables** node.</span></span>

<span data-ttu-id="4a36a-550">Haga clic con el botón derecho en la tabla **Student** y haga clic en **Ver datos** para ver las columnas que se crearon y las filas que se insertaron en la tabla.</span><span class="sxs-lookup"><span data-stu-id="4a36a-550">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="4a36a-551">Código asincrónico</span><span class="sxs-lookup"><span data-stu-id="4a36a-551">Asynchronous code</span></span>

<span data-ttu-id="4a36a-552">La programación asincrónica es el modo predeterminado de ASP.NET Core y EF Core.</span><span class="sxs-lookup"><span data-stu-id="4a36a-552">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="4a36a-553">Un servidor web tiene un número limitado de subprocesos disponibles y, en situaciones de carga alta, es posible que todos los subprocesos disponibles estén en uso.</span><span class="sxs-lookup"><span data-stu-id="4a36a-553">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="4a36a-554">Cuando esto ocurre, el servidor no puede procesar nuevas solicitudes hasta que los subprocesos se liberen.</span><span class="sxs-lookup"><span data-stu-id="4a36a-554">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="4a36a-555">Con el código sincrónico, se pueden acumular muchos subprocesos mientras no estén realizando ningún trabajo porque están a la espera de que finalice la E/S.</span><span class="sxs-lookup"><span data-stu-id="4a36a-555">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="4a36a-556">Con el código asincrónico, cuando un proceso está a la espera de que finalice la E/S, se libera su subproceso para el que el servidor lo use para el procesamiento de otras solicitudes.</span><span class="sxs-lookup"><span data-stu-id="4a36a-556">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="4a36a-557">Como resultado, el código asincrónico permite que los recursos de servidor se usen de forma más eficaz, y el servidor está habilitado para administrar más tráfico sin retrasos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-557">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="4a36a-558">El código asincrónico introduce una pequeña cantidad de sobrecarga en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="4a36a-558">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="4a36a-559">En situaciones de poco tráfico, la disminución del rendimiento es insignificante, mientras que en situaciones de tráfico elevado, la posible mejora del rendimiento es importante.</span><span class="sxs-lookup"><span data-stu-id="4a36a-559">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="4a36a-560">En el código siguiente, la palabra clave [async](/dotnet/csharp/language-reference/keywords/async), el valor devuelto `Task<T>`, la palabra clave `await` y el método `ToListAsync` hacen que el código se ejecute de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="4a36a-560">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="4a36a-561">La palabra clave `async` indica al compilador que:</span><span class="sxs-lookup"><span data-stu-id="4a36a-561">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="4a36a-562">Genere devoluciones de llamada para partes del cuerpo del método.</span><span class="sxs-lookup"><span data-stu-id="4a36a-562">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="4a36a-563">Cree automáticamente el objeto [Task](/dotnet/api/system.threading.tasks.task) que se devuelve.</span><span class="sxs-lookup"><span data-stu-id="4a36a-563">Automatically create the [Task](/dotnet/api/system.threading.tasks.task) object that's returned.</span></span> <span data-ttu-id="4a36a-564">Para más información, vea [Tipo de valor devuelto Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="4a36a-564">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="4a36a-565">El tipo devuelto implícito `Task` representa el trabajo en curso.</span><span class="sxs-lookup"><span data-stu-id="4a36a-565">The implicit return type `Task` represents ongoing work.</span></span>
* <span data-ttu-id="4a36a-566">La palabra clave `await` hace que el compilador divida el método en dos partes.</span><span class="sxs-lookup"><span data-stu-id="4a36a-566">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="4a36a-567">La primera parte termina con la operación que se inició de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="4a36a-567">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="4a36a-568">La segunda parte se coloca en un método de devolución de llamada que se llama cuando finaliza la operación.</span><span class="sxs-lookup"><span data-stu-id="4a36a-568">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="4a36a-569">`ToListAsync` es la versión asincrónica del método de extensión `ToList`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-569">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="4a36a-570">Algunos aspectos que tener en cuenta al escribir código asincrónico en el que se usa EF Core son los siguientes:</span><span class="sxs-lookup"><span data-stu-id="4a36a-570">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="4a36a-571">Solo se ejecutan de forma asincrónica las instrucciones que hacen que las consultas o los comandos se envíen a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-571">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="4a36a-572">Esto incluye `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` y `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-572">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="4a36a-573">No incluye las instrucciones que solo cambian una `IQueryable`, como `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="4a36a-573">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="4a36a-574">Un contexto de EF Core no es seguro para subprocesos: no intente realizar varias operaciones en paralelo.</span><span class="sxs-lookup"><span data-stu-id="4a36a-574">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="4a36a-575">Para aprovechar las ventajas de rendimiento del código asincrónico, compruebe que en los paquetes de biblioteca (por ejemplo para paginación) se usa async si llaman a métodos de EF Core que envían consultas a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4a36a-575">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="4a36a-576">Para obtener más información sobre la programación asincrónica en .NET, vea [Programación asincrónica](/dotnet/standard/async) y [Programación asincrónica con async y await](/dotnet/csharp/programming-guide/concepts/async/).</span><span class="sxs-lookup"><span data-stu-id="4a36a-576">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

<span data-ttu-id="4a36a-577">En el siguiente tutorial, se examinan las operaciones CRUD (crear, leer, actualizar y eliminar) básicas.</span><span class="sxs-lookup"><span data-stu-id="4a36a-577">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>



## <a name="additional-resources"></a><span data-ttu-id="4a36a-578">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="4a36a-578">Additional resources</span></span>

* [<span data-ttu-id="4a36a-579">Versión en YouTube de este tutorial</span><span class="sxs-lookup"><span data-stu-id="4a36a-579">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=P7iTtQnkrNs)

> [!div class="step-by-step"]
> [<span data-ttu-id="4a36a-580">Siguiente</span><span class="sxs-lookup"><span data-stu-id="4a36a-580">Next</span></span>](xref:data/ef-rp/crud)

::: moniker-end
