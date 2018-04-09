---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Implementación de Web ASP.NET con Visual Studio: implementar una actualización de la base de datos | Documentos de Microsoft'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar (publicar) un ASP.NET web aplicación para aplicaciones de Web del servicio de aplicación de Azure o en un proveedor de hospedaje de terceros, usa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 3020cfa19bf12f21c6d42a77ed257595431b4e86
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="e8f6d-103">Implementación de Web ASP.NET con Visual Studio: implementar una actualización de la base de datos</span><span class="sxs-lookup"><span data-stu-id="e8f6d-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>
====================
<span data-ttu-id="e8f6d-104">por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="e8f6d-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="e8f6d-105">Descargar proyecto de inicio</span><span class="sxs-lookup"><span data-stu-id="e8f6d-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="e8f6d-106">Esta serie de tutoriales muestra cómo implementar (publicar) un ASP.NET web de aplicación para aplicaciones de Web del servicio de aplicación de Azure o en un proveedor de hospedaje de terceros, mediante Visual Studio 2012 o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="e8f6d-107">Para obtener información acerca de la serie, consulte [el primer tutorial de la serie](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e8f6d-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="e8f6d-108">Información general</span><span class="sxs-lookup"><span data-stu-id="e8f6d-108">Overview</span></span>

<span data-ttu-id="e8f6d-109">En este tutorial, realice un cambio de base de datos y los cambios de código relacionado, comprobar los cambios en Visual Studio, y luego implementar la actualización en los entornos de prueba, ensayo y producción.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="e8f6d-110">En primer lugar, el tutorial muestra cómo actualizar una base de datos que se administra mediante migraciones de Code First y, a continuación, más adelante muestra cómo actualizar una base de datos mediante el proveedor dbDacFx.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="e8f6d-111">Aviso: Si aparece un mensaje de error o algo no funciona a medida que avances en el tutorial, asegúrese de comprobar la [página solución de problemas](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="e8f6d-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="e8f6d-112">Implementar una actualización de la base de datos mediante el uso de migraciones de Code First</span><span class="sxs-lookup"><span data-stu-id="e8f6d-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="e8f6d-113">En esta sección, agregará una columna de fecha de nacimiento a la `Person` la clase base para la `Student` y `Instructor` entidades.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="e8f6d-114">A continuación, actualice la página que muestra los datos de instructor para que se muestre la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="e8f6d-115">Por último, implementa los cambios en la prueba, ensayo y producción.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="e8f6d-116">Agregar una columna a una tabla en la base de datos de aplicación</span><span class="sxs-lookup"><span data-stu-id="e8f6d-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="e8f6d-117">En el *ContosoUniversity.DAL* proyecto abierto *Person.cs* y agregue la siguiente propiedad al final de la `Person` clase (debe haber dos sigue llaves de cierre):</span><span class="sxs-lookup"><span data-stu-id="e8f6d-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="e8f6d-118">A continuación, actualice el `Seed` método por lo que proporciona un valor para la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="e8f6d-119">Abra *Migrations\Configuration.cs* y reemplace el bloque de código que se inicia `var instructors = new List<Instructor>` con el siguiente bloque de código que incluye información de fecha de nacimiento:</span><span class="sxs-lookup"><span data-stu-id="e8f6d-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="e8f6d-120">Compile la solución y, a continuación, abra el **Package Manager Console** ventana.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="e8f6d-121">Asegúrese de que ContosoUniversity.DAL todavía está seleccionado como el **proyecto predeterminado**.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="e8f6d-122">En el **Package Manager Console** ventana, seleccione **ContosoUniversity.DAL** como el **proyecto predeterminado**y, a continuación, escriba el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="e8f6d-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="e8f6d-123">Cuando finaliza este comando, Visual Studio abre el archivo de clase que define la nueva `DbMIgration` (clase) y en el `Up` método puede ver el código que crea la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="e8f6d-124">El `Up` método crea la columna de la hora de implementar el cambio y el `Down` método elimina la columna cuando se están Revirtiendo el cambio.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="e8f6d-126">Compile la solución y, a continuación, escriba el siguiente comando en el **Package Manager Console** ventana (asegúrese de que todavía esté seleccionado el proyecto de ContosoUniversity.DAL):</span><span class="sxs-lookup"><span data-stu-id="e8f6d-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="e8f6d-127">Entity Framework se ejecuta el `Up` método y, a continuación, se ejecuta el `Seed` método.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="e8f6d-128">Mostrar la nueva columna en la página de instructores</span><span class="sxs-lookup"><span data-stu-id="e8f6d-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="e8f6d-129">En el proyecto ContosoUniversity, abra *Instructors.aspx* y agregar un nuevo campo de plantilla para mostrar la fecha de nacimiento.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="e8f6d-130">Agregarlo entre los de asignación de fecha y la oficina de contratación:</span><span class="sxs-lookup"><span data-stu-id="e8f6d-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="e8f6d-131">(Si la sangría del código obtiene la sincronización, puede presionar CTRL-K y, a continuación, CTRL+D para volver a formatear automáticamente el archivo.)</span><span class="sxs-lookup"><span data-stu-id="e8f6d-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="e8f6d-132">Ejecute la aplicación y haga clic en el **instructores** vínculo.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="e8f6d-133">Cuando se carga la página, verá que tiene el nuevo campo de fecha de nacimiento.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-133">When the page loads, you see that it has the new birth date field.</span></span>

    ![Página de instructores con fecha de nacimiento](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="e8f6d-135">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="e8f6d-136">Implementar la actualización de la base de datos</span><span class="sxs-lookup"><span data-stu-id="e8f6d-136">Deploy the database update</span></span>

1. <span data-ttu-id="e8f6d-137">En **el Explorador de soluciones** seleccione el proyecto ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="e8f6d-138">En el **Web uno haga clic en publicar** barra de herramientas, haga clic en el **prueba** perfil de publicación y, a continuación, haga clic en **Publicar Web**.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="e8f6d-139">(Si se deshabilita la barra de herramientas, seleccione el proyecto ContosoUniversity en **el Explorador de soluciones**.)</span><span class="sxs-lookup"><span data-stu-id="e8f6d-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="e8f6d-140">Visual Studio implementa la aplicación actualizada, y el explorador se abre en la página principal.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="e8f6d-141">Ejecute el **instructores** página para comprobar que la actualización se ha implementado correctamente.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="e8f6d-142">Cuando la aplicación intenta tener acceso a la base de datos de esta página, Code First actualiza el esquema de base de datos y se ejecuta el `Seed` método.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="e8f6d-143">Cuando se muestra la página, verá el esperado **la fecha de nacimiento** columna con fechas en ella.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="e8f6d-144">En el **Web uno haga clic en publicar** barra de herramientas, haga clic en el **ensayo** perfil de publicación y, a continuación, haga clic en **Publicar Web**.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="e8f6d-145">Ejecute el **instructores** página de almacenamiento provisional para comprobar que la actualización se ha implementado correctamente.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="e8f6d-146">En el **Web uno haga clic en publicar** barra de herramientas, haga clic en el **producción** perfil de publicación y, a continuación, haga clic en **Publicar Web**.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="e8f6d-147">Ejecute el **instructores** página en producción para comprobar que la actualización se ha implementado correctamente.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="e8f6d-148">Para una actualización de la aplicación de producción real que incluye un cambio de base de datos normalmente, también se podría pasar la aplicación sin conexión durante la implementación mediante *aplicación\_offline.htm*, tal y como se vio en el tutorial anterior.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-148">For a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="e8f6d-149">Implementar una actualización de la base de datos mediante el proveedor dbDacFx</span><span class="sxs-lookup"><span data-stu-id="e8f6d-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="e8f6d-150">En esta sección, agregará un *comentarios* columna a la *usuario* de la tabla en la base de datos de pertenencia y cree una página que permite mostrar y editar comentarios para cada usuario.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="e8f6d-151">A continuación, implementar los cambios a la prueba, ensayo y producción.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="e8f6d-152">Agregar una columna a una tabla en la base de datos de pertenencia</span><span class="sxs-lookup"><span data-stu-id="e8f6d-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="e8f6d-153">En Visual Studio, abra **Explorador de objetos de SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="e8f6d-154">Expanda **(localdb) \v11.0**, expanda **bases de datos**, expanda **aspnet ContosoUniversity** (no **aspnet-ContosoUniversity-Prod**) y, a continuación, expanda **tablas**.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="e8f6d-155">Si no ve **(localdb) \v11.0** en el **SQL Server** nodo, haga clic en el **SQL Server** nodo y haga clic en **agregar SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="e8f6d-156">En el **conectar al servidor** cuadro de diálogo Escriba *(localdb) \v11.0* como el **nombre del servidor**y, a continuación, haga clic en **conectar**.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="e8f6d-157">Si no ve **aspnet ContosoUniversity**, ejecute el proyecto e inicie sesión con la *administración* credenciales (contraseña es *devpwd*) y, a continuación, actualice el  **Explorador de objetos de SQL Server** ventana.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="e8f6d-158">Haga clic en el **usuarios** de tabla y, a continuación, haga clic en **Diseñador de vistas**.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![Diseñador de vistas SSOX](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="e8f6d-160">En el diseñador, agregue un *comentarios* columna y hacerla *nvarchar (128)* y acepta valores NULL y, a continuación, haga clic en **actualización**.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![Agregar columna de comentarios](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="e8f6d-162">En el **actualizaciones de base de datos de vista previa** cuadro, haga clic en **Actualizar base de datos**.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![Vista previa de actualizaciones de base de datos](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="e8f6d-164">Crear una página para mostrar y editar la nueva columna</span><span class="sxs-lookup"><span data-stu-id="e8f6d-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="e8f6d-165">En **el Explorador de soluciones**, haga clic en el **cuenta** carpeta en el proyecto ContosoUniversity, haga clic en **agregar**y, a continuación, haga clic en **nuevo elemento** .</span><span class="sxs-lookup"><span data-stu-id="e8f6d-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="e8f6d-166">Crear un nuevo **formulario de Web usar la página maestra** y asígnele el nombre *UserInfo.aspx*.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="e8f6d-167">Acepte el valor predeterminado *Site.Master* archivo como la página maestra.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="e8f6d-168">Copie el siguiente marcado en el `MainContent` `Content` elemento (la última de las 3 `Content` elementos):</span><span class="sxs-lookup"><span data-stu-id="e8f6d-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="e8f6d-169">Haga clic en el *UserInfo.aspx* página y haga clic en **ver en el explorador**.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="e8f6d-170">Inicie sesión con su *administración* las credenciales de usuario (contraseña es *devpwd*) y agregar algunos comentarios a un usuario para comprobar que la página funciona correctamente.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![Página de información de usuario](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="e8f6d-172">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="e8f6d-173">Implementar la actualización de la base de datos</span><span class="sxs-lookup"><span data-stu-id="e8f6d-173">Deploy the database update</span></span>

<span data-ttu-id="e8f6d-174">Para implementar mediante el proveedor dbDacFx, basta con que seleccione el **Actualizar base de datos** opción en el perfil de publicación.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="e8f6d-175">Sin embargo, para la implementación inicial cuando se usa esta opción también configuró algunos scripts SQL adicionales para ejecutar: los que todavía están en el perfil y deberá volver a impedir que se ejecute de nuevo.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="e8f6d-176">Abra la **Publicar Web** Asistente haciendo clic en el proyecto ContosoUniversity y haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="e8f6d-177">Seleccione el **prueba** perfil.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="e8f6d-178">Haga clic en el **configuración** ficha.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="e8f6d-179">En **DefaultConnection**, seleccione **Actualizar base de datos**.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="e8f6d-180">Deshabilitar las secuencias de comandos adicionales que se configuró para ejecutarse para la implementación inicial:</span><span class="sxs-lookup"><span data-stu-id="e8f6d-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="e8f6d-181">Haga clic en **configurar actualizaciones de base de datos**.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="e8f6d-182">En el **configurar actualizaciones de base de datos** cuadro de diálogo, desactive las casillas de verificación junto a *Grant.sql* y *dev.sql de datos de aspnet*.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="e8f6d-183">Haga clic en **Cerrar**.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-183">Click **Close**.</span></span>
6. <span data-ttu-id="e8f6d-184">Haga clic en el **vista previa** ficha.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="e8f6d-185">En **bases de datos** y a la derecha del **DefaultConnection**, haga clic en el **base de datos de vista previa** vínculo.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![Vista previa de la base de datos](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="e8f6d-187">La ventana de vista previa muestra la secuencia de comandos que se ejecutará en la base de datos de destino para realizar ese esquema de base de datos coincide con el esquema de la base de datos de origen.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="e8f6d-188">El script incluye un comando ALTER TABLE que agrega la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="e8f6d-189">Cerrar la **vista previa de la base de datos** cuadro de diálogo y, a continuación, haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="e8f6d-190">Visual Studio implementa la aplicación actualizada, y el explorador se abre en la página principal.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="e8f6d-191">Ejecute la página de información de usuario (agregar *Account/UserInfo.aspx* a la dirección URL de la página de inicio) para comprobar que la actualización se ha implementado correctamente.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="e8f6d-192">Deberá volver a iniciar sesión escribiendo *administración* y *devpwd*.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="e8f6d-193">Datos de tablas no se implementan de forma predeterminada y no ha configurado un script de implementación de datos que se ejecuta, por lo que no encontrará el comentario que ha agregado en el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="e8f6d-194">Ahora puede agregar un nuevo comentario de ensayo para comprobar que el cambio se implementó en la base de datos y la página funciona correctamente.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="e8f6d-195">Siga el mismo procedimiento para implementar en ensayo y producción.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="e8f6d-196">No olvide deshabilitar las secuencias de comandos adicionales.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="e8f6d-197">La única diferencia en comparación con el perfil de prueba es que se deshabilitará solo un script en el almacenamiento provisional y perfiles de producción porque se configuraron para ejecutar solo *aspnet-prod-data.sql*.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="e8f6d-198">Las credenciales de ensayo y producción son admin y prodpwd.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="e8f6d-199">Para una actualización de la aplicación de producción real que incluye un cambio de base de datos se tardaría normalmente también la aplicación sin conexión durante la implementación mediante la carga de *aplicación\_offline.htm* antes de la publicación y la eliminación a continuación, tal y como se vio en [el tutorial anterior](deploying-a-code-update.md).</span><span class="sxs-lookup"><span data-stu-id="e8f6d-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="e8f6d-200">Resumen</span><span class="sxs-lookup"><span data-stu-id="e8f6d-200">Summary</span></span>

<span data-ttu-id="e8f6d-201">Ahora que ha implementado una actualización de la aplicación que incluye un cambio de base de datos mediante el proveedor dbDacFx y migraciones de Code First.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

![Página de instructores con fecha de nacimiento](deploying-a-database-update/_static/image8.png)

![Página de información de usuario](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="e8f6d-204">El siguiente tutorial muestra cómo ejecutar las implementaciones mediante el uso de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="e8f6d-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e8f6d-205">[Anterior](deploying-a-code-update.md)
> [Siguiente](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="e8f6d-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>
