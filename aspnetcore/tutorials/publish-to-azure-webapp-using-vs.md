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
ms.openlocfilehash: df22852d2daddb2a3faef8404d0d250a6a1697a5
ms.sourcegitcommit: e987c950caae7af9c4ece8a82228caa364e0a5df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="1f1ea-103">Publicar una aplicación web de ASP.NET Core en Azure App Service con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1f1ea-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="1f1ea-104">De [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs) y [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="1f1ea-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="set-up"></a><span data-ttu-id="1f1ea-105">Configurar</span><span class="sxs-lookup"><span data-stu-id="1f1ea-105">Set up</span></span>

* <span data-ttu-id="1f1ea-106">Abra una [cuenta gratuita de Azure](https://aka.ms/K5y5yh) si no tiene una.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-106">Open a [free Azure account](https://aka.ms/K5y5yh) if you do not have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="1f1ea-107">Creación de una aplicación web</span><span class="sxs-lookup"><span data-stu-id="1f1ea-107">Create a web app</span></span>

<span data-ttu-id="1f1ea-108">En la página de inicio de Visual Studio, seleccione **Archivo > Nuevo > Proyecto...**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-108">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![menú Archivo](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="1f1ea-110">Rellene el cuadro de diálogo **Nuevo proyecto**:</span><span class="sxs-lookup"><span data-stu-id="1f1ea-110">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="1f1ea-111">En el panel izquierdo, seleccione **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-111">In the left pane, select **.NET Core**.</span></span>

* <span data-ttu-id="1f1ea-112">En el panel central, seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-112">In the center pane, select **ASP.NET Core Web Application**.</span></span>

* <span data-ttu-id="1f1ea-113">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-113">Select **OK**.</span></span>

![Cuadro de diálogo Nuevo proyecto](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="1f1ea-115">En el cuadro de diálogo **Nueva aplicación web de ASP.NET Core**, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1f1ea-115">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="1f1ea-116">Seleccione **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-116">Select **Web Application**.</span></span>

* <span data-ttu-id="1f1ea-117">Seleccione **Cambiar autenticación**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-117">Select **Change Authentication**.</span></span>

![Cuadro de diálogo Nuevo proyecto](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="1f1ea-119">Se mostrará el cuadro de diálogo **Cambiar autenticación**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-119">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="1f1ea-120">Seleccione **Cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-120">Select **Individual User Accounts**.</span></span>

* <span data-ttu-id="1f1ea-121">Seleccione **Aceptar** para volver a la **nueva aplicación web de ASP.NET Core** y vuelva a seleccionar **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-121">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Cuadro de diálogo de autenticación web de ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="1f1ea-123">Visual Studio crea la solución.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-123">Visual Studio creates the solution.</span></span>

## <a name="run-the-app-locally"></a><span data-ttu-id="1f1ea-124">Probar la aplicación localmente</span><span class="sxs-lookup"><span data-stu-id="1f1ea-124">Run the app locally</span></span>

* <span data-ttu-id="1f1ea-125">Elija **Depurar** y **Iniciar sin depurar** para ejecutar la aplicación localmente.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-125">Choose **Debug** then **Start Without Debugging** to run the app locally.</span></span>

* <span data-ttu-id="1f1ea-126">Haga clic en los vínculos **Acerca de** y **Contacto** para comprobar que la aplicación web funcione.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-126">Click the **About** and **Contact** links to verify the web application works.</span></span>

![Aplicación web abierta en Microsoft Edge en host local](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="1f1ea-128">Seleccione **Registrar** y registre a un usuario nuevo.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-128">Select **Register** and register a new user.</span></span> <span data-ttu-id="1f1ea-129">Puede usar una dirección de correo electrónico ficticia.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-129">You can use a fictitious email address.</span></span> <span data-ttu-id="1f1ea-130">Al enviar la información, la página mostrará el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="1f1ea-130">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="1f1ea-131">*"Error interno del servidor: Error en una operación de base de datos al procesar la solicitud. Excepción de SQL: No se puede abrir la base de datos. La aplicación de las migraciones existentes para el contexto de base de datos de la aplicación puede solucionar este problema".*</span><span class="sxs-lookup"><span data-stu-id="1f1ea-131">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>

* <span data-ttu-id="1f1ea-132">Seleccione **Aplicar migraciones** y, una vez actualizada la página, vuelva a cargarla.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-132">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Error interno del servidor: Error en una operación de base de datos al procesar la solicitud.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="1f1ea-136">La aplicación muestra el correo electrónico usado para registrar al nuevo usuario y el vínculo **Cerrar sesión**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-136">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Aplicación web abierta en Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="1f1ea-139">Implementación de la aplicación en Azure</span><span class="sxs-lookup"><span data-stu-id="1f1ea-139">Deploy the app to Azure</span></span>

<span data-ttu-id="1f1ea-140">Cierre la página web, vuelva a Visual Studio y seleccione **Detener depuración** en el menú **Depurar**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-140">Close the web page, return to Visual Studio, and select **Stop Debugging** from the **Debug** menu.</span></span>

<span data-ttu-id="1f1ea-141">Desde el Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **Publicar...**</span><span class="sxs-lookup"><span data-stu-id="1f1ea-141">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Menú contextual abierto con el vínculo Publicar resaltado](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="1f1ea-143">En el cuadro de diálogo **Publicar**, seleccione **Microsoft Azure App Service** y haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-143">In the **Publish** dialog, select **Microsoft Azure App Service** and click **Publish**.</span></span>

![Cuadro de diálogo Publicar](publish-to-azure-webapp-using-vs/_static/maas1.png)

* <span data-ttu-id="1f1ea-145">Asigne un nombre único a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-145">Name the app a unique name.</span></span> 

* <span data-ttu-id="1f1ea-146">Seleccionar una suscripción.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-146">Select a subscription.</span></span>

* <span data-ttu-id="1f1ea-147">Seleccione **Nuevo...** junto al grupo de recursos y escriba un nombre para el nuevo grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-147">Select **New...** for the resource group and enter a name for the new resource group.</span></span>

* <span data-ttu-id="1f1ea-148">Seleccione **Nuevo...** junto al plan de App Service y seleccione una ubicación cercana.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-148">Select **New...** for the app service plan and select a location near you.</span></span> <span data-ttu-id="1f1ea-149">Puede conservar el nombre que se genera de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-149">You can keep the name that is generated by default.</span></span>

![Cuadro de diálogo App Service](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="1f1ea-151">Seleccione la pestaña **Servicios** para crear una base de datos.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-151">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="1f1ea-152">Seleccione el icono verde **+** para crear una instancia de SQL Database.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-152">Select the green **+** icon to create a new SQL Database</span></span>

![Crear una base de datos de SQL Database](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="1f1ea-154">Seleccione **Nuevo...** en el cuadro de diálogo **Configurar SQL Database** para crear una base de datos.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-154">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Crear una base de datos y un servidor de SQL Database](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="1f1ea-156">Se mostrará el cuadro de diálogo **Configurar SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-156">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="1f1ea-157">Escriba un nombre de usuario y una contraseña de administrador y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-157">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="1f1ea-158">No olvide el nombre de usuario y la contraseña que ha creado en este paso.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-158">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="1f1ea-159">Puede conservar el **nombre de servidor** predeterminado.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-159">You can keep the default **Server Name**.</span></span> 

* <span data-ttu-id="1f1ea-160">Escriba el nombre de la cadena de conexión y el de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-160">Enter names for the database and connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="1f1ea-161">No se permite "admin" como nombre de usuario de administrador.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-161">"admin" is not allowed as the administrator user name.</span></span>

![Cuadro de diálogo Configurar SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="1f1ea-163">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-163">Select **OK**.</span></span>

<span data-ttu-id="1f1ea-164">Visual Studio volverá al cuadro de diálogo **Crear un servicio de App Service**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-164">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="1f1ea-165">Seleccione **Crear** en el cuadro de diálogo **Crear un servicio de App Service**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-165">Select **Create** on the **Create App Service** dialog.</span></span>

![Cuadro de diálogo Configurar base de datos SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="1f1ea-167">Haga clic en el vínculo **Configuración** del cuadro de diálogo **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-167">Click the **Settings** link in the **Publish** dialog.</span></span>

![Cuadro de diálogo Publicar: panel Conexión](publish-to-azure-webapp-using-vs/_static/pubc.png)

<span data-ttu-id="1f1ea-169">En la página **Configuración** del cuadro de diálogo **Publicar**, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1f1ea-169">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="1f1ea-170">Expanda **Bases de datos** y active **Usar esta cadena de conexión en tiempo de ejecución**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-170">Expand **Databases** and check **Use this connection string at runtime**.</span></span>

  * <span data-ttu-id="1f1ea-171">Expanda **Migraciones de Entity Framework** y active **Aplicar esta migración al publicar**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-171">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="1f1ea-172">Seleccione **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-172">Select **Save**.</span></span> <span data-ttu-id="1f1ea-173">Visual Studio volverá al cuadro de diálogo **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-173">Visual Studio returns to the **Publish** dialog.</span></span> 

![Cuadro de diálogo Publicar: panel Configuración](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="1f1ea-175">Haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-175">Click **Publish**.</span></span> <span data-ttu-id="1f1ea-176">Visual Studio publica la aplicación en Azure e inicia Cloud App en el explorador.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-176">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="1f1ea-177">Prueba de la aplicación en Azure</span><span class="sxs-lookup"><span data-stu-id="1f1ea-177">Test your app in Azure</span></span>

* <span data-ttu-id="1f1ea-178">Pruebe los vínculos **Acerca de** y **Contacto**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-178">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="1f1ea-179">Registre un nuevo usuario.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-179">Register a new user</span></span>

![Aplicación web abierta en Microsoft Edge en Azure App Service](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="1f1ea-181">Actualización de la aplicación</span><span class="sxs-lookup"><span data-stu-id="1f1ea-181">Update the app</span></span>

* <span data-ttu-id="1f1ea-182">Edite la página de Razor *Pages/About.cshtml* y modifique su contenido.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-182">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="1f1ea-183">Por ejemplo, puede editar el párrafo para que incluya el mensaje "¡Hola, ASP.NET Core!":</span><span class="sxs-lookup"><span data-stu-id="1f1ea-183">For example, you can modify the paragraph to say "Hello ASP.NET Core!":</span></span>

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* <span data-ttu-id="1f1ea-184">Haga clic con el botón derecho sobre el proyecto y vuelva a seleccionar **Publicar...**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-184">Right-click on the project and select **Publish...** again.</span></span>

![Menú contextual abierto con el vínculo Publicar resaltado](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="1f1ea-186">Una vez publicada la aplicación, compruebe que los cambios realizados estén disponibles en Azure.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-186">After the app is published, verify the changes you made are available on Azure.</span></span>

![Comprobar si se ha completado la tarea](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="1f1ea-188">Limpieza</span><span class="sxs-lookup"><span data-stu-id="1f1ea-188">Clean up</span></span>

<span data-ttu-id="1f1ea-189">Cuando haya terminado de probar la aplicación, vaya a [Azure Portal](https://portal.azure.com/) y elimínela.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-189">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="1f1ea-190">Seleccione **Grupos de recursos** y, luego, el grupo de recursos que haya creado.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-190">Select **Resource groups**, then select the resource group you created.</span></span>

![Azure Portal: Grupos de recursos en el menú lateral](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="1f1ea-192">En la página **Grupos de recursos**, seleccione **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-192">In the **Resource groups** page, select **Delete**.</span></span>

![Azure Portal: página Grupos de recursos](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="1f1ea-194">Escriba el nombre del grupo de recursos y seleccione **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-194">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="1f1ea-195">La aplicación y todos los demás recursos que ha creado en este tutorial se han eliminado de Azure.</span><span class="sxs-lookup"><span data-stu-id="1f1ea-195">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="1f1ea-196">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="1f1ea-196">Next steps</span></span>

* [<span data-ttu-id="1f1ea-197">Implementación continua en Azure con Visual Studio y Git</span><span class="sxs-lookup"><span data-stu-id="1f1ea-197">Continuous Deployment to Azure with Visual Studio and Git</span></span>](../publishing/azure-continuous-deployment.md)