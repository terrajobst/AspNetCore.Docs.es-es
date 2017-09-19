---
title: "Publicar una aplicación de ASP.NET Core en Azure con Visual Studio"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 09/01/2017
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 0c0ec1c7c1408b0460c594a47a3e5738cd170d5f
ms.sourcegitcommit: d022d4b96795ee473fa3847a1d8a8c7430423a86
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="790db-103">Publicar una aplicación web de ASP.NET Core en Azure App Service con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="790db-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="790db-104">De [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs) y [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="790db-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="set-up-the-development-environment"></a><span data-ttu-id="790db-105">Configuración del entorno de desarrollo</span><span class="sxs-lookup"><span data-stu-id="790db-105">Set up the development environment</span></span>

* <span data-ttu-id="790db-106">Instale la versión más reciente de [Azure SDK para Visual Studio](https://www.visualstudio.com/vs/azure-tools/).</span><span class="sxs-lookup"><span data-stu-id="790db-106">Install the latest [Azure SDK for Visual Studio](https://www.visualstudio.com/vs/azure-tools/).</span></span> <span data-ttu-id="790db-107">El SDK instala Visual Studio si aún no lo tiene.</span><span class="sxs-lookup"><span data-stu-id="790db-107">The SDK installs Visual Studio if you don't already have it.</span></span>

* <span data-ttu-id="790db-108">Compruebe la [cuenta de Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="790db-108">Verify your [Azure account](https://portal.azure.com/).</span></span> <span data-ttu-id="790db-109">Puede [abrir una cuenta de Azure gratuita](https://azure.microsoft.com/pricing/free-trial/) o [activar las ventajas de suscriptor de Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="790db-109">You can [open a free Azure account](https://azure.microsoft.com/pricing/free-trial/) or [Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="790db-110">Creación de una aplicación web</span><span class="sxs-lookup"><span data-stu-id="790db-110">Create a web app</span></span>

<span data-ttu-id="790db-111">En la página de inicio de Visual Studio, seleccione **Archivo > Nuevo > Proyecto...**.</span><span class="sxs-lookup"><span data-stu-id="790db-111">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![menú Archivo](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="790db-113">Rellene el cuadro de diálogo **Nuevo proyecto**:</span><span class="sxs-lookup"><span data-stu-id="790db-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="790db-114">En el panel izquierdo, seleccione **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="790db-114">In the left pane, select **.NET Core**.</span></span>

* <span data-ttu-id="790db-115">En el panel central, seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="790db-115">In the center pane, select **ASP.NET Core Web Application**.</span></span>

* <span data-ttu-id="790db-116">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="790db-116">Select **OK**.</span></span>

![Cuadro de diálogo Nuevo proyecto](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="790db-118">En el cuadro de diálogo **Nueva aplicación web de ASP.NET Core**, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="790db-118">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="790db-119">Seleccione **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="790db-119">Select **Web Application**.</span></span>

* <span data-ttu-id="790db-120">Seleccione **Cambiar autenticación**.</span><span class="sxs-lookup"><span data-stu-id="790db-120">Select **Change Authentication**.</span></span>

![Cuadro de diálogo Nuevo proyecto](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="790db-122">Se mostrará el cuadro de diálogo **Cambiar autenticación**.</span><span class="sxs-lookup"><span data-stu-id="790db-122">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="790db-123">Seleccione **Cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="790db-123">Select **Individual User Accounts**.</span></span>

* <span data-ttu-id="790db-124">Seleccione **Aceptar** para volver a la **nueva aplicación web de ASP.NET Core** y vuelva a seleccionar **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="790db-124">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Cuadro de diálogo de autenticación web de ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="790db-126">Visual Studio crea la solución.</span><span class="sxs-lookup"><span data-stu-id="790db-126">Visual Studio creates the solution.</span></span>

## <a name="run-the-app-locally"></a><span data-ttu-id="790db-127">Probar la aplicación localmente</span><span class="sxs-lookup"><span data-stu-id="790db-127">Run the app locally</span></span>

* <span data-ttu-id="790db-128">Elija **Depurar** y **Iniciar sin depurar** para ejecutar la aplicación localmente.</span><span class="sxs-lookup"><span data-stu-id="790db-128">Choose **Debug** then **Start Without Debugging** to run the app locally.</span></span>

* <span data-ttu-id="790db-129">Haga clic en los vínculos **Acerca de** y **Contacto** para comprobar que la aplicación web funcione.</span><span class="sxs-lookup"><span data-stu-id="790db-129">Click the **About** and **Contact** links to verify the web application works.</span></span>

![Aplicación web abierta en Microsoft Edge en host local](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="790db-131">Seleccione **Registrar** y registre a un usuario nuevo.</span><span class="sxs-lookup"><span data-stu-id="790db-131">Select **Register** and register a new user.</span></span> <span data-ttu-id="790db-132">Puede usar una dirección de correo electrónico ficticia.</span><span class="sxs-lookup"><span data-stu-id="790db-132">You can use a fictitious email address.</span></span> <span data-ttu-id="790db-133">Al enviar la información, la página mostrará el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="790db-133">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="790db-134">*"Error interno del servidor: Error en una operación de base de datos al procesar la solicitud. Excepción de SQL: No se puede abrir la base de datos. La aplicación de las migraciones existentes para el contexto de base de datos de la aplicación puede solucionar este problema".*</span><span class="sxs-lookup"><span data-stu-id="790db-134">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>

* <span data-ttu-id="790db-135">Seleccione **Aplicar migraciones** y, una vez actualizada la página, vuelva a cargarla.</span><span class="sxs-lookup"><span data-stu-id="790db-135">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Error interno del servidor: Error en una operación de base de datos al procesar la solicitud.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="790db-139">La aplicación muestra el correo electrónico usado para registrar al nuevo usuario y el vínculo **Cerrar sesión**.</span><span class="sxs-lookup"><span data-stu-id="790db-139">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Aplicación web abierta en Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="790db-142">Implementación de la aplicación en Azure</span><span class="sxs-lookup"><span data-stu-id="790db-142">Deploy the app to Azure</span></span>

<span data-ttu-id="790db-143">Cierre la página web, vuelva a Visual Studio y seleccione **Detener depuración** en el menú **Depurar**.</span><span class="sxs-lookup"><span data-stu-id="790db-143">Close the web page, return to Visual Studio, and select **Stop Debugging** from the **Debug** menu.</span></span>

<span data-ttu-id="790db-144">Desde el Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **Publicar...**</span><span class="sxs-lookup"><span data-stu-id="790db-144">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Menú contextual abierto con el vínculo Publicar resaltado](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="790db-146">En el cuadro de diálogo **Publicar**, seleccione **Microsoft Azure App Service** y haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="790db-146">In the **Publish** dialog, select **Microsoft Azure App Service** and click **Publish**.</span></span>

![Cuadro de diálogo Publicar](publish-to-azure-webapp-using-vs/_static/maas1.png)

* <span data-ttu-id="790db-148">Asigne un nombre único a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="790db-148">Name the app a unique name.</span></span> 

* <span data-ttu-id="790db-149">Seleccionar una suscripción.</span><span class="sxs-lookup"><span data-stu-id="790db-149">Select a subscription.</span></span>

* <span data-ttu-id="790db-150">Seleccione **Nuevo...** junto al grupo de recursos y escriba un nombre para el nuevo grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="790db-150">Select **New...** for the resource group and enter a name for the new resource group.</span></span>

* <span data-ttu-id="790db-151">Seleccione **Nuevo...** junto al plan de App Service y seleccione una ubicación cercana.</span><span class="sxs-lookup"><span data-stu-id="790db-151">Select **New...** for the app service plan and select a location near you.</span></span> <span data-ttu-id="790db-152">Puede conservar el nombre que se genera de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="790db-152">You can keep the name that is generated by default.</span></span>

![Cuadro de diálogo App Service](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="790db-154">Seleccione la pestaña **Servicios** para crear una base de datos.</span><span class="sxs-lookup"><span data-stu-id="790db-154">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="790db-155">Seleccione el icono verde **+** para crear una instancia de SQL Database.</span><span class="sxs-lookup"><span data-stu-id="790db-155">Select the green **+** icon to create a new SQL Database</span></span>

![Crear una base de datos de SQL Database](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="790db-157">Seleccione **Nuevo...** en el cuadro de diálogo **Configurar SQL Database** para crear una base de datos.</span><span class="sxs-lookup"><span data-stu-id="790db-157">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Crear una base de datos y un servidor de SQL Database](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="790db-159">Se mostrará el cuadro de diálogo **Configurar SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="790db-159">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="790db-160">Escriba un nombre de usuario y una contraseña de administrador y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="790db-160">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="790db-161">No olvide el nombre de usuario y la contraseña que ha creado en este paso.</span><span class="sxs-lookup"><span data-stu-id="790db-161">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="790db-162">Puede conservar el **nombre de servidor** predeterminado.</span><span class="sxs-lookup"><span data-stu-id="790db-162">You can keep the default **Server Name**.</span></span> 

* <span data-ttu-id="790db-163">Escriba el nombre de la cadena de conexión y el de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="790db-163">Enter names for the database and connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="790db-164">No se permite "admin" como nombre de usuario de administrador.</span><span class="sxs-lookup"><span data-stu-id="790db-164">"admin" is not allowed as the administrator user name.</span></span>

![Cuadro de diálogo Configurar SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="790db-166">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="790db-166">Select **OK**.</span></span>

<span data-ttu-id="790db-167">Visual Studio volverá al cuadro de diálogo **Crear un servicio de App Service**.</span><span class="sxs-lookup"><span data-stu-id="790db-167">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="790db-168">Seleccione **Crear** en el cuadro de diálogo **Crear un servicio de App Service**.</span><span class="sxs-lookup"><span data-stu-id="790db-168">Select **Create** on the **Create App Service** dialog.</span></span>

![Cuadro de diálogo Configurar base de datos SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="790db-170">Haga clic en el vínculo **Configuración** del cuadro de diálogo **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="790db-170">Click the **Settings** link in the **Publish** dialog.</span></span>

![Cuadro de diálogo Publicar: panel Conexión](publish-to-azure-webapp-using-vs/_static/pubc.png)

<span data-ttu-id="790db-172">En la página **Configuración** del cuadro de diálogo **Publicar**, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="790db-172">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="790db-173">Expanda **Bases de datos** y active **Usar esta cadena de conexión en tiempo de ejecución**.</span><span class="sxs-lookup"><span data-stu-id="790db-173">Expand **Databases** and check **Use this connection string at runtime**.</span></span>

  * <span data-ttu-id="790db-174">Expanda **Migraciones de Entity Framework** y active **Aplicar esta migración al publicar**.</span><span class="sxs-lookup"><span data-stu-id="790db-174">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="790db-175">Seleccione **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="790db-175">Select **Save**.</span></span> <span data-ttu-id="790db-176">Visual Studio volverá al cuadro de diálogo **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="790db-176">Visual Studio returns to the **Publish** dialog.</span></span> 

![Cuadro de diálogo Publicar: panel Configuración](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="790db-178">Haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="790db-178">Click **Publish**.</span></span> <span data-ttu-id="790db-179">Visual Studio publica la aplicación en Azure e inicia Cloud App en el explorador.</span><span class="sxs-lookup"><span data-stu-id="790db-179">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="790db-180">Prueba de la aplicación en Azure</span><span class="sxs-lookup"><span data-stu-id="790db-180">Test your app in Azure</span></span>

* <span data-ttu-id="790db-181">Pruebe los vínculos **Acerca de** y **Contacto**.</span><span class="sxs-lookup"><span data-stu-id="790db-181">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="790db-182">Registre un nuevo usuario.</span><span class="sxs-lookup"><span data-stu-id="790db-182">Register a new user</span></span>

![Aplicación web abierta en Microsoft Edge en Azure App Service](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="790db-184">Actualización de la aplicación</span><span class="sxs-lookup"><span data-stu-id="790db-184">Update the app</span></span>

* <span data-ttu-id="790db-185">Edite la página de Razor *Pages/About.cshtml* y modifique su contenido.</span><span class="sxs-lookup"><span data-stu-id="790db-185">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="790db-186">Por ejemplo, puede editar el párrafo para que incluya el mensaje "¡Hola, ASP.NET Core!":</span><span class="sxs-lookup"><span data-stu-id="790db-186">For example, you can modify the paragraph to say "Hello ASP.NET Core!":</span></span>

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* <span data-ttu-id="790db-187">Haga clic con el botón derecho sobre el proyecto y vuelva a seleccionar **Publicar...**.</span><span class="sxs-lookup"><span data-stu-id="790db-187">Right-click on the project and select **Publish...** again.</span></span>

![Menú contextual abierto con el vínculo Publicar resaltado](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="790db-189">Una vez publicada la aplicación, compruebe que los cambios realizados estén disponibles en Azure.</span><span class="sxs-lookup"><span data-stu-id="790db-189">After the app is published, verify the changes you made are available on Azure.</span></span>

![Comprobar si se ha completado la tarea](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="790db-191">Limpieza</span><span class="sxs-lookup"><span data-stu-id="790db-191">Clean up</span></span>

<span data-ttu-id="790db-192">Cuando haya terminado de probar la aplicación, vaya a [Azure Portal](https://portal.azure.com/) y elimínela.</span><span class="sxs-lookup"><span data-stu-id="790db-192">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="790db-193">Seleccione **Grupos de recursos** y, luego, el grupo de recursos que haya creado.</span><span class="sxs-lookup"><span data-stu-id="790db-193">Select **Resource groups**, then select the resource group you created.</span></span>

![Azure Portal: Grupos de recursos en el menú lateral](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="790db-195">En la página **Grupos de recursos**, seleccione **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="790db-195">In the **Resource groups** page, select **Delete**.</span></span>

![Azure Portal: página Grupos de recursos](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="790db-197">Escriba el nombre del grupo de recursos y seleccione **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="790db-197">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="790db-198">La aplicación y todos los demás recursos que ha creado en este tutorial se han eliminado de Azure.</span><span class="sxs-lookup"><span data-stu-id="790db-198">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="790db-199">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="790db-199">Next steps</span></span>

* [<span data-ttu-id="790db-200">Introducción a ASP.NET Core MVC y Visual Studio</span><span class="sxs-lookup"><span data-stu-id="790db-200">Getting started with ASP.NET Core MVC and Visual Studio</span></span>](first-mvc-app/start-mvc.md)

* [<span data-ttu-id="790db-201">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="790db-201">Introduction to ASP.NET Core</span></span>](../index.md)

* [<span data-ttu-id="790db-202">Aspectos básicos</span><span class="sxs-lookup"><span data-stu-id="790db-202">Fundamentals</span></span>](../fundamentals/index.md)
