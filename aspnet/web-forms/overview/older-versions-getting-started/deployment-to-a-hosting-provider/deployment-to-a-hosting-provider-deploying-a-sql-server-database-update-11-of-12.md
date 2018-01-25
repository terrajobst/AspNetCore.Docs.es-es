---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: "Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar una actualización de base de datos de SQL Server - 11 de 12 | Documentos de Microsoft"
author: tdykstra
description: "Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact usando Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: aeec69c7373a111d30e8f32a374a9f02fb4c080a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a><span data-ttu-id="128f3-103">Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar una actualización de base de datos de SQL Server - 11 de 12</span><span class="sxs-lookup"><span data-stu-id="128f3-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a SQL Server Database Update - 11 of 12</span></span>
====================
<span data-ttu-id="128f3-104">Por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="128f3-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="128f3-105">Descargar proyecto de inicio</span><span class="sxs-lookup"><span data-stu-id="128f3-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="128f3-106">Esta serie de tutoriales muestra cómo implementar (publicar) ASP.NET proyecto de aplicación web que incluye una base de datos de SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC for Web.</span><span class="sxs-lookup"><span data-stu-id="128f3-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="128f3-107">También puede usar Visual Studio 2010 si instala la actualización de publicación de Web.</span><span class="sxs-lookup"><span data-stu-id="128f3-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="128f3-108">Para obtener una introducción a la serie, consulte [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="128f3-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="128f3-109">Para obtener un tutorial que muestra las características de implementación introducidas después del lanzamiento de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea SQL Server Compact y muestra cómo implementar sitios Web de Windows Azure, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="128f3-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Windows Azure Web Sites, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="128f3-110">Información general</span><span class="sxs-lookup"><span data-stu-id="128f3-110">Overview</span></span>

<span data-ttu-id="128f3-111">Este tutorial muestra cómo implementar una actualización de la base de datos en una base de datos completa de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="128f3-111">This tutorial shows you how to deploy a database update to a full SQL Server database.</span></span> <span data-ttu-id="128f3-112">Dado que migraciones de Code First realiza todo el trabajo de actualización de la base de datos, el proceso es casi idéntico a lo que hizo para SQL Server Compact en el [implementar una actualización de la base de datos](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="128f3-112">Because Code First Migrations does all the work of updating the database, the process is almost identical to what you did for SQL Server Compact in the [Deploying a Database Update](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.</span></span>

<span data-ttu-id="128f3-113">Aviso: Si aparece un mensaje de error o algo no funciona a medida que avances en el tutorial, asegúrese de comprobar la [página solución de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="128f3-113">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="128f3-114">Agregar una nueva columna a una tabla</span><span class="sxs-lookup"><span data-stu-id="128f3-114">Adding a New Column to a Table</span></span>

<span data-ttu-id="128f3-115">En esta sección del tutorial realizará una base de datos cambiar y cambios de código correspondiente, a continuación, las pruebas en Visual Studio como preparación para su implementación en los entornos de prueba y producción.</span><span class="sxs-lookup"><span data-stu-id="128f3-115">In this section of the tutorial you'll make a database change and corresponding code changes, then test them in Visual Studio in preparation for deploying them to the test and production environments.</span></span> <span data-ttu-id="128f3-116">El cambio implica agregar un `OfficeHours` columna a la `Instructor` entidad y mostrar la nueva información en el **instructores** página web.</span><span class="sxs-lookup"><span data-stu-id="128f3-116">The change involves adding an `OfficeHours` column to the `Instructor` entity and displaying the new information in the **Instructors** web page.</span></span>

<span data-ttu-id="128f3-117">En el proyecto ContosoUniversity.DAL, abra *Instructor.cs* y agregue la siguiente propiedad entre el `HireDate` y `Courses` propiedades:</span><span class="sxs-lookup"><span data-stu-id="128f3-117">In the ContosoUniversity.DAL project, open *Instructor.cs* and add the following property between the `HireDate` and `Courses` properties:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

<span data-ttu-id="128f3-118">Actualizar la clase de inicializador para que propaga la nueva columna con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="128f3-118">Update the initializer class so that it seeds the new column with test data.</span></span> <span data-ttu-id="128f3-119">Abra *Migrations\Configuration.cs* y reemplace el bloque de código que se inicia `var instructors = new List<Instructor>` con el siguiente bloque de código que incluye la nueva columna:</span><span class="sxs-lookup"><span data-stu-id="128f3-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes the new column:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

<span data-ttu-id="128f3-120">En el proyecto ContosoUniversity, abra *Instructors.aspx* y agregar un nuevo campo de plantilla para el horario de oficina inmediatamente antes de cerrar `</Columns>` en la primera etiqueta `GridView` control:</span><span class="sxs-lookup"><span data-stu-id="128f3-120">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field for office hours just before the closing `</Columns>` tag in the first `GridView` control:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

<span data-ttu-id="128f3-121">Compile la solución.</span><span class="sxs-lookup"><span data-stu-id="128f3-121">Build the solution.</span></span>

<span data-ttu-id="128f3-122">Abra la **Package Manager Console** ventana y seleccione ContosoUniversity.DAL como el **proyecto predeterminado**.</span><span class="sxs-lookup"><span data-stu-id="128f3-122">Open the **Package Manager Console** window, and select ContosoUniversity.DAL as the **Default project**.</span></span>

<span data-ttu-id="128f3-123">Escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="128f3-123">Enter the following commands:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

<span data-ttu-id="128f3-124">Ejecute la aplicación y seleccione la **instructores** página.</span><span class="sxs-lookup"><span data-stu-id="128f3-124">Run the application and select the **Instructors** page.</span></span> <span data-ttu-id="128f3-125">La página tarda un poco más de lo habitual en cargarse, dado que Entity Framework vuelve a crear la base de datos y propaga con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="128f3-125">The page takes a little longer than usual to load, because the Entity Framework re-creates the database and seeds it with test data.</span></span>

<span data-ttu-id="128f3-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="128f3-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="128f3-127">Implementación de la actualización de la base de datos en el entorno de prueba</span><span class="sxs-lookup"><span data-stu-id="128f3-127">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="128f3-128">Cuando usas migraciones de Code First, el método para implementar un cambio de base de datos en SQL Server es el mismo que para SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="128f3-128">When you use Code First Migrations, the method for deploying a database change to SQL Server is the same as for SQL Server Compact.</span></span> <span data-ttu-id="128f3-129">Sin embargo, tendrá que cambiar la prueba de perfil de publicación porque todavía se configuró para migrar de SQL Server Compact a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="128f3-129">However, you have to change the Test publish profile because it is still set up to migrate from SQL Server Compact to SQL Server.</span></span>

<span data-ttu-id="128f3-130">El primer paso es quitar las transformaciones de cadena de conexión que creó en el tutorial anterior.</span><span class="sxs-lookup"><span data-stu-id="128f3-130">The first step is to remove the connection string transformations that you created in the previous tutorial.</span></span> <span data-ttu-id="128f3-131">Ya no son necesarios porque especificará las transformaciones de cadena de conexión en el perfil de publicación, como lo hizo antes de que configure la **Empaquetar/publicar SQL** ficha para la migración a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="128f3-131">These are no longer needed because you'll specify connection string transformations in the publish profile, as you did before you configured the **Package/Publish SQL** tab for migration to SQL Server.</span></span>

<span data-ttu-id="128f3-132">Abra la *Web.Test.config* de archivos y quitar el `connectionStrings` elemento.</span><span class="sxs-lookup"><span data-stu-id="128f3-132">Open the *Web.Test.config* file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="128f3-133">La transformación sola restante en el *Web.Test.config* archivo es para el `Environment` valor en el `appSettings` elemento.</span><span class="sxs-lookup"><span data-stu-id="128f3-133">The only remaining transformation in the *Web.Test.config* file is for the `Environment` value in the `appSettings` element.</span></span>

<span data-ttu-id="128f3-134">Ahora puede actualizar el perfil de publicación y la publicación en el entorno de prueba.</span><span class="sxs-lookup"><span data-stu-id="128f3-134">Now you can update the publish profile and publish to the test environment.</span></span>

<span data-ttu-id="128f3-135">Abra la **Publicar Web** asistente y, a continuación, cambie a la **perfil** ficha.</span><span class="sxs-lookup"><span data-stu-id="128f3-135">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="128f3-136">Seleccione el **prueba** perfil de publicación.</span><span class="sxs-lookup"><span data-stu-id="128f3-136">Select the **Test** publish profile.</span></span>

<span data-ttu-id="128f3-137">Seleccione el **configuración** ficha.</span><span class="sxs-lookup"><span data-stu-id="128f3-137">Select the **Settings** tab.</span></span>

<span data-ttu-id="128f3-138">Haga clic en **habilitar la base de datos nueva publicación mejoras**.</span><span class="sxs-lookup"><span data-stu-id="128f3-138">Click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="128f3-139">En el cuadro de la cadena de conexión para **SchoolContext**, escriba el mismo valor que utilizó en el *Web.Test.config* archivo de transformación en el tutorial anterior:</span><span class="sxs-lookup"><span data-stu-id="128f3-139">In the connection string box for **SchoolContext**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

<span data-ttu-id="128f3-140">Seleccione **ejecutar migraciones de Code First (se ejecuta al iniciarse la aplicación)**.</span><span class="sxs-lookup"><span data-stu-id="128f3-140">Select **Execute Code First Migrations (runs on application start)**.</span></span> <span data-ttu-id="128f3-141">(En la versión de Visual Studio, la casilla de verificación podría etiquetarse **aplicar Code First Migrations**.)</span><span class="sxs-lookup"><span data-stu-id="128f3-141">(In your version of Visual Studio, the check box might be labeled **Apply Code First Migrations**.)</span></span>

<span data-ttu-id="128f3-142">En el cuadro de la cadena de conexión para **DefaultConnection**, escriba el mismo valor que utilizó en el *Web.Test.config* archivo de transformación en el tutorial anterior:</span><span class="sxs-lookup"><span data-stu-id="128f3-142">In the connection string box for **DefaultConnection**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

<span data-ttu-id="128f3-143">Deje **Actualizar base de datos** desactivada.</span><span class="sxs-lookup"><span data-stu-id="128f3-143">Leave **Update database** cleared.</span></span>

<span data-ttu-id="128f3-144">Haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="128f3-144">Click **Publish**.</span></span>

<span data-ttu-id="128f3-145">Visual Studio implementa los cambios de código en el entorno de prueba y abre el explorador a la página de inicio de la Universidad de Contoso.</span><span class="sxs-lookup"><span data-stu-id="128f3-145">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="128f3-146">Seleccione la página de instructores.</span><span class="sxs-lookup"><span data-stu-id="128f3-146">Select the Instructors page.</span></span>

<span data-ttu-id="128f3-147">Cuando la aplicación ejecuta esta página, intenta obtener acceso a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="128f3-147">When the application runs this page, it tries to access the database.</span></span> <span data-ttu-id="128f3-148">Migraciones de Code First comprueba si la base de datos es actual y encuentra que la migración más reciente no se ha aplicado todavía.</span><span class="sxs-lookup"><span data-stu-id="128f3-148">Code First Migrations checks if the database is current, and finds that the latest migration has not been applied yet.</span></span> <span data-ttu-id="128f3-149">Migraciones de Code First se aplica la migración más reciente, se ejecuta el `Seed` método y, a continuación, en la página se ejecuta con normalidad.</span><span class="sxs-lookup"><span data-stu-id="128f3-149">Code First Migrations applies the latest migration, runs the `Seed` method, and then the page runs normally.</span></span> <span data-ttu-id="128f3-150">Ver la nueva columna de horario de oficina con los datos a la inicialización.</span><span class="sxs-lookup"><span data-stu-id="128f3-150">You see the new Office Hours column with the seeded data.</span></span>

<span data-ttu-id="128f3-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="128f3-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="128f3-152">Implementación de la actualización de la base de datos en el entorno de producción</span><span class="sxs-lookup"><span data-stu-id="128f3-152">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="128f3-153">Tendrá que cambiar también el perfil de publicación para el entorno de producción.</span><span class="sxs-lookup"><span data-stu-id="128f3-153">You have to change the publish profile for the production environment also.</span></span> <span data-ttu-id="128f3-154">En este caso deberá quitar el perfil existente y crear uno nuevo mediante la importación de un archivo .publishsettings actualizada.</span><span class="sxs-lookup"><span data-stu-id="128f3-154">In this case you'll remove the existing profile and create a new one by importing an updated .publishsettings file.</span></span> <span data-ttu-id="128f3-155">El archivo actualizado incluirá la cadena de conexión para la base de datos de SQL Server en Cytanium.</span><span class="sxs-lookup"><span data-stu-id="128f3-155">The updated file will include the connection string for the SQL Server database at Cytanium.</span></span>

<span data-ttu-id="128f3-156">Como ha podido comprobar cuando implementa en el entorno de prueba, ya no necesita transformaciones de cadena de conexión en el *Web.Production.config* archivo de transformación.</span><span class="sxs-lookup"><span data-stu-id="128f3-156">As you saw when you deployed to the test environment, you no longer need connection string transforms in the *Web.Production.config* transformation file.</span></span> <span data-ttu-id="128f3-157">Abrir archivos y quitar el `connectionStrings` elemento.</span><span class="sxs-lookup"><span data-stu-id="128f3-157">Open that file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="128f3-158">Las transformaciones restantes son para el `Environment` valor en el `appSettings` elemento y el `location` elemento que restringe el acceso a los informes de errores de Elmah.</span><span class="sxs-lookup"><span data-stu-id="128f3-158">The remaining transformations are for the `Environment` value in the `appSettings` element and the `location` element that restricts access to Elmah error reports.</span></span>

<span data-ttu-id="128f3-159">Antes de crear un nuevo perfil de publicación para la producción, descargue un archivo .publishsettings actualizada del mismo modo que lo hizo anteriormente en el [implementarla en el entorno de producción](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="128f3-159">Before you create a new publish profile for production, download an updated .publishsettings file the same way you did earlier in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span> <span data-ttu-id="128f3-160">(En el panel de control Cytanium, haga clic en **sitios Web**y, a continuación, haga clic en el **contosouniversity.com** sitio Web.</span><span class="sxs-lookup"><span data-stu-id="128f3-160">(In the Cytanium control panel, click **Web Sites**, and then click the **contosouniversity.com** website.</span></span> <span data-ttu-id="128f3-161">Seleccione el **publicación Web** ficha y, a continuación, haga clic en **descargar perfil de publicación para este sitio web**.) La razón por la que están haciendo esto es recoger la cadena de conexión de base de datos en el archivo .publishsettings.</span><span class="sxs-lookup"><span data-stu-id="128f3-161">Select the **Web Publishing** tab, and then click **Download Publishing Profile for this web site**.) The reason you are doing this is to pick up the database connection string in the .publishsettings file.</span></span> <span data-ttu-id="128f3-162">La cadena de conexión no estaba disponible la primera vez que se descargó el archivo, ya que se sigue usando SQL Server Compact y no había creado la base de datos de SQL Server en Cytanium todavía.</span><span class="sxs-lookup"><span data-stu-id="128f3-162">The connection string wasn't available the first time you downloaded the file, because you were still using SQL Server Compact and hadn't created the SQL Server database at Cytanium yet.</span></span>

<span data-ttu-id="128f3-163">Ahora puede actualizar el perfil de publicación y la publicación en el entorno de producción.</span><span class="sxs-lookup"><span data-stu-id="128f3-163">Now you can update the publish profile and publish to the production environment.</span></span>

<span data-ttu-id="128f3-164">Abra la **Publicar Web** asistente y, a continuación, cambie a la **perfil** ficha.</span><span class="sxs-lookup"><span data-stu-id="128f3-164">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="128f3-165">Haga clic en **administrar perfiles**y, a continuación, elimine el perfil de producción.</span><span class="sxs-lookup"><span data-stu-id="128f3-165">Click **Manage Profiles**, and then delete the Production profile.</span></span>

<span data-ttu-id="128f3-166">Cerrar la **Publicar Web** Asistente para guardar los cambios.</span><span class="sxs-lookup"><span data-stu-id="128f3-166">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="128f3-167">Abra la **Publicar Web** Asistente nuevo y, a continuación, haga clic en **importación**.</span><span class="sxs-lookup"><span data-stu-id="128f3-167">Open the **Publish Web** wizard again, and then click **Import**.</span></span>

<span data-ttu-id="128f3-168">En el **conexión** , modifique **dirección URL de destino** en el valor adecuado si usa una dirección URL temporal.</span><span class="sxs-lookup"><span data-stu-id="128f3-168">On the **Connection** tab, change **Destination URL** to the appropriate value if you are using a temporary URL.</span></span>

<span data-ttu-id="128f3-169">Haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="128f3-169">Click **Next**.</span></span>

<span data-ttu-id="128f3-170">En el **configuración** , haga clic en **habilitar la base de datos nueva publicación mejoras**.</span><span class="sxs-lookup"><span data-stu-id="128f3-170">On the **Settings** tab, click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="128f3-171">En la lista de desplegable de cadena de conexión para **SchoolContext**, seleccione la cadena de conexión Cytanium.</span><span class="sxs-lookup"><span data-stu-id="128f3-171">In the connection string drop-down list for **SchoolContext**, select the Cytanium connection string.</span></span>

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

<span data-ttu-id="128f3-173">Seleccione **migraciones ejecutar Code First (se ejecuta al iniciarse la aplicación)**.</span><span class="sxs-lookup"><span data-stu-id="128f3-173">Select **Execute Code First migrations (runs on application start)**.</span></span>

<span data-ttu-id="128f3-174">En la lista de desplegable de cadena de conexión para **DefaultConnection**, seleccione la cadena de conexión Cytanium.</span><span class="sxs-lookup"><span data-stu-id="128f3-174">In the connection string drop-down list for **DefaultConnection**, select the Cytanium connection string.</span></span>

<span data-ttu-id="128f3-175">Seleccione el **perfil** , haga clic en **administrar perfiles**y cambie el nombre del perfil de "contosouniversity.com - Web Deploy" a "Producción".</span><span class="sxs-lookup"><span data-stu-id="128f3-175">Select the **Profile** tab, click **Manage Profiles**, and rename the profile from "contosouniversity.com - Web Deploy" to "Production".</span></span>

<span data-ttu-id="128f3-176">Cierre el perfil de publicación para guardar el cambio, a continuación, vuelva a abrirlo.</span><span class="sxs-lookup"><span data-stu-id="128f3-176">Close the publish profile to save the change, then open it again.</span></span>

<span data-ttu-id="128f3-177">Haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="128f3-177">Click **Publish**.</span></span> <span data-ttu-id="128f3-178">(Para un sitio Web de producción real, se copiaría *aplicación\_offline.htm* a producción y put en la carpeta del proyecto antes de publicar, a continuación, quítela cuando se completa la implementación.)</span><span class="sxs-lookup"><span data-stu-id="128f3-178">(For a real production website, you would copy *app\_offline.htm* to production and put it in your project folder before publishing, then remove it when deployment is complete.)</span></span>

<span data-ttu-id="128f3-179">Visual Studio implementa los cambios de código en el entorno de prueba y abre el explorador a la página de inicio de la Universidad de Contoso.</span><span class="sxs-lookup"><span data-stu-id="128f3-179">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="128f3-180">Seleccione la página de instructores.</span><span class="sxs-lookup"><span data-stu-id="128f3-180">Select the Instructors page.</span></span>

<span data-ttu-id="128f3-181">Migraciones de Code First actualiza la base de datos de la misma manera que se hacía en el entorno de prueba.</span><span class="sxs-lookup"><span data-stu-id="128f3-181">Code First Migrations updates the database the same way it did in the Test environment.</span></span> <span data-ttu-id="128f3-182">Ver la nueva columna de horario de oficina con los datos a la inicialización.</span><span class="sxs-lookup"><span data-stu-id="128f3-182">You see the new Office Hours column with the seeded data.</span></span>

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

<span data-ttu-id="128f3-184">Ahora correctamente implementó una actualización de la aplicación que incluye un cambio de la base de datos, con una base de datos de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="128f3-184">You have now successfully deployed an application update that included a database change, using a SQL Server database.</span></span>

## <a name="more-information"></a><span data-ttu-id="128f3-185">Más información</span><span class="sxs-lookup"><span data-stu-id="128f3-185">More Information</span></span>

<span data-ttu-id="128f3-186">Con esto finaliza la siguiente serie de tutoriales sobre la implementación de una aplicación web ASP.NET en un proveedor de hospedaje de terceros.</span><span class="sxs-lookup"><span data-stu-id="128f3-186">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="128f3-187">Para obtener más información acerca de cualquiera de los temas tratados en estos tutoriales, vea el [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) en el sitio web MSDN.</span><span class="sxs-lookup"><span data-stu-id="128f3-187">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) on the MSDN web site.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="128f3-188">Reconocimientos</span><span class="sxs-lookup"><span data-stu-id="128f3-188">Acknowledgements</span></span>

<span data-ttu-id="128f3-189">Me gustaría dar las gracias a las siguientes personas que realizado aportaciones importantes para el contenido de esta serie de tutoriales:</span><span class="sxs-lookup"><span data-stu-id="128f3-189">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="128f3-190">Alberto Poblacion, MVP &amp; MCT, España</span><span class="sxs-lookup"><span data-stu-id="128f3-190">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.support.microsoft.com/profile/Alberto)
- <span data-ttu-id="128f3-191">Estados Unidos de Jarod Ferguson, MVP de desarrollo de plataforma de datos,</span><span class="sxs-lookup"><span data-stu-id="128f3-191">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="128f3-192">Harsh Mittal, Microsoft</span><span class="sxs-lookup"><span data-stu-id="128f3-192">Harsh Mittal, Microsoft</span></span>
- [<span data-ttu-id="128f3-193">Kristina Olson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="128f3-193">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="128f3-194">Mike Pope, Microsoft</span><span class="sxs-lookup"><span data-stu-id="128f3-194">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="128f3-195">Mohit Srivastava, Microsoft</span><span class="sxs-lookup"><span data-stu-id="128f3-195">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="128f3-196">Raffaele Rialdi, Italia</span><span class="sxs-lookup"><span data-stu-id="128f3-196">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="128f3-197">Rick Anderson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="128f3-197">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="128f3-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="128f3-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="128f3-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="128f3-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="128f3-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="128f3-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="128f3-201">Srđan Božović, Serbia</span><span class="sxs-lookup"><span data-stu-id="128f3-201">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="128f3-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="128f3-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="128f3-203">[Anterior](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Siguiente](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="128f3-203">[Previous](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Next](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span></span>
