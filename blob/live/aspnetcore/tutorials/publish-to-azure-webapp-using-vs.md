---
title: "Publicar una aplicación de ASP.NET Core en Azure con Visual Studio"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 6f697ed4d8876a19cd058533e4f6a5d4f7cdc2fb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="cd8e9-103">Publicar una aplicación web de ASP.NET Core en Azure App Service con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd8e9-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="cd8e9-104">De [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs) y [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="cd8e9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="cd8e9-105">Vea [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) (Publicación en Azure desde Visual Studio para Mac) si trabaja en un equipo Mac.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-105">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="cd8e9-106">Configurar</span><span class="sxs-lookup"><span data-stu-id="cd8e9-106">Set up</span></span>

* <span data-ttu-id="cd8e9-107">Abra una [cuenta gratuita de Azure](https://aka.ms/K5y5yh) si no tiene una.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-107">Open a [free Azure account](https://aka.ms/K5y5yh) if you do not have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="cd8e9-108">Creación de una aplicación web</span><span class="sxs-lookup"><span data-stu-id="cd8e9-108">Create a web app</span></span>

<span data-ttu-id="cd8e9-109">En la página de inicio de Visual Studio, seleccione **Archivo > Nuevo > Proyecto...**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-109">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![menú Archivo](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="cd8e9-111">Rellene el cuadro de diálogo **Nuevo proyecto**:</span><span class="sxs-lookup"><span data-stu-id="cd8e9-111">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="cd8e9-112">En el panel izquierdo, seleccione **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-112">In the left pane, select **.NET Core**.</span></span>

* <span data-ttu-id="cd8e9-113">En el panel central, seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-113">In the center pane, select **ASP.NET Core Web Application**.</span></span>

* <span data-ttu-id="cd8e9-114">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-114">Select **OK**.</span></span>

![Cuadro de diálogo Nuevo proyecto](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="cd8e9-116">En el cuadro de diálogo **Nueva aplicación web de ASP.NET Core**, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="cd8e9-116">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="cd8e9-117">Seleccione **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-117">Select **Web Application**.</span></span>

* <span data-ttu-id="cd8e9-118">Seleccione **Cambiar autenticación**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-118">Select **Change Authentication**.</span></span>

![Cuadro de diálogo Nuevo proyecto](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="cd8e9-120">Se mostrará el cuadro de diálogo **Cambiar autenticación**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-120">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="cd8e9-121">Seleccione **Cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-121">Select **Individual User Accounts**.</span></span>

* <span data-ttu-id="cd8e9-122">Seleccione **Aceptar** para volver a la **nueva aplicación web de ASP.NET Core** y vuelva a seleccionar **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-122">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Cuadro de diálogo de autenticación web de ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="cd8e9-124">Visual Studio crea la solución.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-124">Visual Studio creates the solution.</span></span>

## <a name="run-the-app-locally"></a><span data-ttu-id="cd8e9-125">Probar la aplicación localmente</span><span class="sxs-lookup"><span data-stu-id="cd8e9-125">Run the app locally</span></span>

* <span data-ttu-id="cd8e9-126">Elija **Depurar** y **Iniciar sin depurar** para ejecutar la aplicación localmente.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-126">Choose **Debug** then **Start Without Debugging** to run the app locally.</span></span>

* <span data-ttu-id="cd8e9-127">Haga clic en los vínculos **Acerca de** y **Contacto** para comprobar que la aplicación web funcione.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-127">Click the **About** and **Contact** links to verify the web application works.</span></span>

![Aplicación web abierta en Microsoft Edge en host local](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="cd8e9-129">Seleccione **Registrar** y registre a un usuario nuevo.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-129">Select **Register** and register a new user.</span></span> <span data-ttu-id="cd8e9-130">Puede usar una dirección de correo electrónico ficticia.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-130">You can use a fictitious email address.</span></span> <span data-ttu-id="cd8e9-131">Al enviar la información, la página mostrará el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="cd8e9-131">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="cd8e9-132">*"Error interno del servidor: Error en una operación de base de datos al procesar la solicitud. Excepción de SQL: No se puede abrir la base de datos. La aplicación de las migraciones existentes para el contexto de base de datos de la aplicación puede solucionar este problema".*</span><span class="sxs-lookup"><span data-stu-id="cd8e9-132">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>

* <span data-ttu-id="cd8e9-133">Seleccione **Aplicar migraciones** y, una vez actualizada la página, vuelva a cargarla.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-133">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Error interno del servidor: Error en una operación de base de datos al procesar la solicitud.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="cd8e9-137">La aplicación muestra el correo electrónico usado para registrar al nuevo usuario y el vínculo **Cerrar sesión**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-137">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Aplicación web abierta en Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="cd8e9-140">Implementación de la aplicación en Azure</span><span class="sxs-lookup"><span data-stu-id="cd8e9-140">Deploy the app to Azure</span></span>

<span data-ttu-id="cd8e9-141">Cierre la página web, vuelva a Visual Studio y seleccione **Detener depuración** en el menú **Depurar**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-141">Close the web page, return to Visual Studio, and select **Stop Debugging** from the **Debug** menu.</span></span>

<span data-ttu-id="cd8e9-142">Desde el Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **Publicar...**</span><span class="sxs-lookup"><span data-stu-id="cd8e9-142">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Menú contextual abierto con el vínculo Publicar resaltado](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="cd8e9-144">En el cuadro de diálogo **Publicar**, seleccione **Microsoft Azure App Service** y haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-144">In the **Publish** dialog, select **Microsoft Azure App Service** and click **Publish**.</span></span>

![Cuadro de diálogo Publicar](publish-to-azure-webapp-using-vs/_static/maas1.png)

* <span data-ttu-id="cd8e9-146">Asigne un nombre único a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-146">Name the app a unique name.</span></span> 

* <span data-ttu-id="cd8e9-147">Seleccionar una suscripción.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-147">Select a subscription.</span></span>

* <span data-ttu-id="cd8e9-148">Seleccione **Nuevo...** junto al grupo de recursos y escriba un nombre para el nuevo grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-148">Select **New...** for the resource group and enter a name for the new resource group.</span></span>

* <span data-ttu-id="cd8e9-149">Seleccione **Nuevo...** junto al plan de App Service y seleccione una ubicación cercana.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-149">Select **New...** for the app service plan and select a location near you.</span></span> <span data-ttu-id="cd8e9-150">Puede conservar el nombre que se genera de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-150">You can keep the name that is generated by default.</span></span>

![Cuadro de diálogo App Service](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="cd8e9-152">Seleccione la pestaña **Servicios** para crear una base de datos.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-152">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="cd8e9-153">Seleccione el icono verde **+** para crear una instancia de SQL Database.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-153">Select the green **+** icon to create a new SQL Database</span></span>

![Crear una base de datos de SQL Database](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="cd8e9-155">Seleccione **Nuevo...** en el cuadro de diálogo **Configurar SQL Database** para crear una base de datos.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-155">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Crear una base de datos y un servidor de SQL Database](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="cd8e9-157">Se mostrará el cuadro de diálogo **Configurar SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-157">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="cd8e9-158">Escriba un nombre de usuario y una contraseña de administrador y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-158">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="cd8e9-159">No olvide el nombre de usuario y la contraseña que ha creado en este paso.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-159">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="cd8e9-160">Puede conservar el **nombre de servidor** predeterminado.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-160">You can keep the default **Server Name**.</span></span> 

* <span data-ttu-id="cd8e9-161">Escriba el nombre de la cadena de conexión y el de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-161">Enter names for the database and connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="cd8e9-162">No se permite "admin" como nombre de usuario de administrador.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-162">"admin" is not allowed as the administrator user name.</span></span>

![Cuadro de diálogo Configurar SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="cd8e9-164">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-164">Select **OK**.</span></span>

<span data-ttu-id="cd8e9-165">Visual Studio volverá al cuadro de diálogo **Crear un servicio de App Service**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-165">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="cd8e9-166">Seleccione **Crear** en el cuadro de diálogo **Crear un servicio de App Service**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-166">Select **Create** on the **Create App Service** dialog.</span></span>

![Cuadro de diálogo Configurar base de datos SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="cd8e9-168">Haga clic en el vínculo **Configuración** del cuadro de diálogo **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-168">Click the **Settings** link in the **Publish** dialog.</span></span>

![Cuadro de diálogo Publicar: panel Conexión](publish-to-azure-webapp-using-vs/_static/pubc.png)

<span data-ttu-id="cd8e9-170">En la página **Configuración** del cuadro de diálogo **Publicar**, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="cd8e9-170">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="cd8e9-171">Expanda **Bases de datos** y active **Usar esta cadena de conexión en tiempo de ejecución**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-171">Expand **Databases** and check **Use this connection string at runtime**.</span></span>

  * <span data-ttu-id="cd8e9-172">Expanda **Migraciones de Entity Framework** y active **Aplicar esta migración al publicar**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-172">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="cd8e9-173">Seleccione **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-173">Select **Save**.</span></span> <span data-ttu-id="cd8e9-174">Visual Studio volverá al cuadro de diálogo **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-174">Visual Studio returns to the **Publish** dialog.</span></span> 

![Cuadro de diálogo Publicar: panel Configuración](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="cd8e9-176">Haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-176">Click **Publish**.</span></span> <span data-ttu-id="cd8e9-177">Visual Studio publica la aplicación en Azure e inicia Cloud App en el explorador.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-177">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="cd8e9-178">Prueba de la aplicación en Azure</span><span class="sxs-lookup"><span data-stu-id="cd8e9-178">Test your app in Azure</span></span>

* <span data-ttu-id="cd8e9-179">Pruebe los vínculos **Acerca de** y **Contacto**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-179">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="cd8e9-180">Registre un nuevo usuario.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-180">Register a new user</span></span>

![Aplicación web abierta en Microsoft Edge en Azure App Service](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="cd8e9-182">Actualización de la aplicación</span><span class="sxs-lookup"><span data-stu-id="cd8e9-182">Update the app</span></span>

* <span data-ttu-id="cd8e9-183">Edite la página de Razor *Pages/About.cshtml* y modifique su contenido.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-183">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="cd8e9-184">Por ejemplo, puede editar el párrafo para que incluya el mensaje "¡Hola, ASP.NET Core!":</span><span class="sxs-lookup"><span data-stu-id="cd8e9-184">For example, you can modify the paragraph to say "Hello ASP.NET Core!":</span></span>

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* <span data-ttu-id="cd8e9-185">Haga clic con el botón derecho sobre el proyecto y vuelva a seleccionar **Publicar...**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-185">Right-click on the project and select **Publish...** again.</span></span>

![Menú contextual abierto con el vínculo Publicar resaltado](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="cd8e9-187">Una vez publicada la aplicación, compruebe que los cambios realizados estén disponibles en Azure.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-187">After the app is published, verify the changes you made are available on Azure.</span></span>

![Comprobar si se ha completado la tarea](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="cd8e9-189">Limpieza</span><span class="sxs-lookup"><span data-stu-id="cd8e9-189">Clean up</span></span>

<span data-ttu-id="cd8e9-190">Cuando haya terminado de probar la aplicación, vaya a [Azure Portal](https://portal.azure.com/) y elimínela.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-190">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="cd8e9-191">Seleccione **Grupos de recursos** y, luego, el grupo de recursos que haya creado.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-191">Select **Resource groups**, then select the resource group you created.</span></span>

![Azure Portal: Grupos de recursos en el menú lateral](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="cd8e9-193">En la página **Grupos de recursos**, seleccione **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-193">In the **Resource groups** page, select **Delete**.</span></span>

![Azure Portal: página Grupos de recursos](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="cd8e9-195">Escriba el nombre del grupo de recursos y seleccione **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-195">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="cd8e9-196">La aplicación y todos los demás recursos que ha creado en este tutorial se han eliminado de Azure.</span><span class="sxs-lookup"><span data-stu-id="cd8e9-196">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="cd8e9-197">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="cd8e9-197">Next steps</span></span>

* [<span data-ttu-id="cd8e9-198">Implementación continua en Azure con Visual Studio y Git</span><span class="sxs-lookup"><span data-stu-id="cd8e9-198">Continuous Deployment to Azure with Visual Studio and Git</span></span>](../publishing/azure-continuous-deployment.md)
