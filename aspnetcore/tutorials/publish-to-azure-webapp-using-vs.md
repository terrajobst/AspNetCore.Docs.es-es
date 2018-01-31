---
title: "Publicar una aplicación de ASP.NET Core en Azure con Visual Studio"
author: rick-anderson
description: "Obtenga información sobre cómo publicar una aplicación de ASP.NET Core en Azure App Service con Visual Studio."
manager: wpickett
ms.author: riande
ms.date: 12/16/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 14d8dd0a5e6a99bacce3bf50b0468b20e0dddb96
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="23f8a-103">Publicar una aplicación web de ASP.NET Core en Azure App Service con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="23f8a-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="23f8a-104">De [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs) y [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="23f8a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="23f8a-105">Vea [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) (Publicación en Azure desde Visual Studio para Mac) si trabaja en un equipo Mac.</span><span class="sxs-lookup"><span data-stu-id="23f8a-105">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="23f8a-106">Configurar</span><span class="sxs-lookup"><span data-stu-id="23f8a-106">Set up</span></span>

* <span data-ttu-id="23f8a-107">Abra una [cuenta gratuita de Azure](https://aka.ms/K5y5yh) si no tiene una.</span><span class="sxs-lookup"><span data-stu-id="23f8a-107">Open a [free Azure account](https://aka.ms/K5y5yh) if you don't have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="23f8a-108">Creación de una aplicación web</span><span class="sxs-lookup"><span data-stu-id="23f8a-108">Create a web app</span></span>

<span data-ttu-id="23f8a-109">En la página de inicio de Visual Studio, seleccione **Archivo > Nuevo > Proyecto...**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-109">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![menú Archivo](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="23f8a-111">Rellene el cuadro de diálogo **Nuevo proyecto**:</span><span class="sxs-lookup"><span data-stu-id="23f8a-111">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="23f8a-112">En el panel izquierdo, seleccione **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-112">In the left pane, select **.NET Core**.</span></span>
* <span data-ttu-id="23f8a-113">En el panel central, seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-113">In the center pane, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="23f8a-114">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-114">Select **OK**.</span></span>

![Cuadro de diálogo Nuevo proyecto](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="23f8a-116">En el cuadro de diálogo **Nueva aplicación web de ASP.NET Core**, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="23f8a-116">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="23f8a-117">Seleccione **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-117">Select **Web Application**.</span></span>
* <span data-ttu-id="23f8a-118">Seleccione **Cambiar autenticación**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-118">Select **Change Authentication**.</span></span>

![Cuadro de diálogo Nuevo proyecto](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="23f8a-120">Se mostrará el cuadro de diálogo **Cambiar autenticación**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-120">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="23f8a-121">Seleccione **Cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-121">Select **Individual User Accounts**.</span></span>
* <span data-ttu-id="23f8a-122">Seleccione **Aceptar** para volver a la **nueva aplicación web de ASP.NET Core** y vuelva a seleccionar **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-122">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Cuadro de diálogo de autenticación web de ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="23f8a-124">Visual Studio crea la solución.</span><span class="sxs-lookup"><span data-stu-id="23f8a-124">Visual Studio creates the solution.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="23f8a-125">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="23f8a-125">Run the app</span></span>

* <span data-ttu-id="23f8a-126">Presione CTRL+F5 para ejecutar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="23f8a-126">Press CTRL+F5 to run the project.</span></span>
* <span data-ttu-id="23f8a-127">Pruebe los vínculos **Acerca de** y **Contacto**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-127">Test the **About** and **Contact** links.</span></span>

![Aplicación web abierta en Microsoft Edge en host local](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a><span data-ttu-id="23f8a-129">Registrar un usuario</span><span class="sxs-lookup"><span data-stu-id="23f8a-129">Register a user</span></span>

* <span data-ttu-id="23f8a-130">Seleccione **Registrar** y registre a un usuario nuevo.</span><span class="sxs-lookup"><span data-stu-id="23f8a-130">Select **Register** and register a new user.</span></span> <span data-ttu-id="23f8a-131">Puede usar una dirección de correo electrónico ficticia.</span><span class="sxs-lookup"><span data-stu-id="23f8a-131">You can use a fictitious email address.</span></span> <span data-ttu-id="23f8a-132">Al enviar la información, la página mostrará el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="23f8a-132">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="23f8a-133">*"Error interno del servidor: Error en una operación de base de datos al procesar la solicitud. Excepción de SQL: No se puede abrir la base de datos. La aplicación de las migraciones existentes para el contexto de base de datos de la aplicación puede solucionar este problema".*</span><span class="sxs-lookup"><span data-stu-id="23f8a-133">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>
* <span data-ttu-id="23f8a-134">Seleccione **Aplicar migraciones** y, una vez actualizada la página, vuelva a cargarla.</span><span class="sxs-lookup"><span data-stu-id="23f8a-134">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Error interno del servidor: Error en una operación de base de datos al procesar la solicitud.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="23f8a-138">La aplicación muestra el correo electrónico usado para registrar al nuevo usuario y el vínculo **Cerrar sesión**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-138">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Aplicación web abierta en Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="23f8a-141">Implementación de la aplicación en Azure</span><span class="sxs-lookup"><span data-stu-id="23f8a-141">Deploy the app to Azure</span></span>

<span data-ttu-id="23f8a-142">Desde el Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **Publicar...**</span><span class="sxs-lookup"><span data-stu-id="23f8a-142">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Menú contextual abierto con el vínculo Publicar resaltado](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="23f8a-144">En el cuadro de diálogo **Publicar**:</span><span class="sxs-lookup"><span data-stu-id="23f8a-144">In the **Publish** dialog:</span></span>

* <span data-ttu-id="23f8a-145">Seleccione **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-145">Select **Microsoft Azure App Service**.</span></span>
* <span data-ttu-id="23f8a-146">Seleccione el icono de engranaje y luego **Crear perfil**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-146">Select the gear icon and then select **Create Profile**.</span></span>
* <span data-ttu-id="23f8a-147">Seleccione **Crear perfil**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-147">Select **Create Profile**.</span></span>

![Cuadro de diálogo Publicar](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a><span data-ttu-id="23f8a-149">Crear recursos de Azure</span><span class="sxs-lookup"><span data-stu-id="23f8a-149">Create Azure resources</span></span>

<span data-ttu-id="23f8a-150">Aparece el cuadro de diálogo **Crear servicio de aplicaciones**:</span><span class="sxs-lookup"><span data-stu-id="23f8a-150">The **Create App Service** dialog appears:</span></span>

* <span data-ttu-id="23f8a-151">Especifique la suscripción.</span><span class="sxs-lookup"><span data-stu-id="23f8a-151">Enter your subscription.</span></span>
* <span data-ttu-id="23f8a-152">Se rellenan los campos de entrada **Nombre de la aplicación**, **Grupo de recursos** y **Plan de App Service**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-152">The **App Name**, **Resource Group**, and **App Service Plan** entry fields are populated.</span></span> <span data-ttu-id="23f8a-153">Puede mantener estos nombres o cambiarlos.</span><span class="sxs-lookup"><span data-stu-id="23f8a-153">You can keep these names or change them.</span></span>

![Cuadro de diálogo App Service](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="23f8a-155">Seleccione la pestaña **Servicios** para crear una base de datos.</span><span class="sxs-lookup"><span data-stu-id="23f8a-155">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="23f8a-156">Seleccione el icono verde **+** para crear una instancia de SQL Database.</span><span class="sxs-lookup"><span data-stu-id="23f8a-156">Select the green **+** icon to create a new SQL Database</span></span>

![Crear una base de datos de SQL Database](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="23f8a-158">Seleccione **Nuevo...** en el cuadro de diálogo **Configurar SQL Database** para crear una base de datos.</span><span class="sxs-lookup"><span data-stu-id="23f8a-158">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Crear una base de datos y un servidor de SQL Database](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="23f8a-160">Se mostrará el cuadro de diálogo **Configurar SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-160">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="23f8a-161">Escriba un nombre de usuario y una contraseña de administrador y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-161">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="23f8a-162">Puede conservar el **nombre de servidor** predeterminado.</span><span class="sxs-lookup"><span data-stu-id="23f8a-162">You can keep the default **Server Name**.</span></span> 

> [!NOTE]
> <span data-ttu-id="23f8a-163">No se permite "admin" como nombre de usuario de administrador.</span><span class="sxs-lookup"><span data-stu-id="23f8a-163">"admin" isn't allowed as the administrator user name.</span></span>

![Cuadro de diálogo Configurar SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="23f8a-165">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-165">Select **OK**.</span></span>

<span data-ttu-id="23f8a-166">Visual Studio volverá al cuadro de diálogo **Crear un servicio de App Service**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-166">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="23f8a-167">Seleccione **Crear** en el cuadro de diálogo **Crear un servicio de App Service**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-167">Select **Create** on the **Create App Service** dialog.</span></span>

![Cuadro de diálogo Configurar base de datos SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

<span data-ttu-id="23f8a-169">Visual Studio crea la aplicación web y SQL Server en Azure.</span><span class="sxs-lookup"><span data-stu-id="23f8a-169">Visual Studio creates the Web app and SQL Server on Azure.</span></span> <span data-ttu-id="23f8a-170">Este paso puede llevar varios minutos.</span><span class="sxs-lookup"><span data-stu-id="23f8a-170">This step can take a few minutes.</span></span> <span data-ttu-id="23f8a-171">Para más información sobre los recursos creados, vea [Recursos adicionales](#additonal-resources).</span><span class="sxs-lookup"><span data-stu-id="23f8a-171">For information on the resources created, see [Additonal resources](#additonal-resources).</span></span>

<span data-ttu-id="23f8a-172">Cuando termine la implementación, seleccione **Configuración**:</span><span class="sxs-lookup"><span data-stu-id="23f8a-172">When deployment completes, select **Settings**:</span></span>

![Cuadro de diálogo Configurar SQL Server](publish-to-azure-webapp-using-vs/_static/set.png)

<span data-ttu-id="23f8a-174">En la página **Configuración** del cuadro de diálogo **Publicar**, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="23f8a-174">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="23f8a-175">Expanda **Bases de datos** y active **Usar esta cadena de conexión en tiempo de ejecución**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-175">Expand **Databases** and check **Use this connection string at runtime**.</span></span>
  * <span data-ttu-id="23f8a-176">Expanda **Migraciones de Entity Framework** y active **Aplicar esta migración al publicar**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-176">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="23f8a-177">Seleccione **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-177">Select **Save**.</span></span> <span data-ttu-id="23f8a-178">Visual Studio volverá al cuadro de diálogo **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-178">Visual Studio returns to the **Publish** dialog.</span></span> 

![Cuadro de diálogo Publicar: panel Configuración](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="23f8a-180">Haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-180">Click **Publish**.</span></span> <span data-ttu-id="23f8a-181">Visual Studio publica la aplicación en Azure.</span><span class="sxs-lookup"><span data-stu-id="23f8a-181">Visual Studio publishs your app to Azure.</span></span> <span data-ttu-id="23f8a-182">Cuando termina la implementación, la aplicación se abre en un explorador.</span><span class="sxs-lookup"><span data-stu-id="23f8a-182">When the depoyment completes, the app is opened in a browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="23f8a-183">Prueba de la aplicación en Azure</span><span class="sxs-lookup"><span data-stu-id="23f8a-183">Test your app in Azure</span></span>

* <span data-ttu-id="23f8a-184">Pruebe los vínculos **Acerca de** y **Contacto**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-184">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="23f8a-185">Registre un nuevo usuario.</span><span class="sxs-lookup"><span data-stu-id="23f8a-185">Register a new user</span></span>

![Aplicación web abierta en Microsoft Edge en Azure App Service](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="23f8a-187">Actualización de la aplicación</span><span class="sxs-lookup"><span data-stu-id="23f8a-187">Update the app</span></span>

* <span data-ttu-id="23f8a-188">Edite la página de Razor *Pages/About.cshtml* y modifique su contenido.</span><span class="sxs-lookup"><span data-stu-id="23f8a-188">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="23f8a-189">Por ejemplo, puede modificar el párrafo para que diga "¡Hola, ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="23f8a-189">For example, you can modify the paragraph to say "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span></span>

* <span data-ttu-id="23f8a-190">Haga clic con el botón derecho sobre el proyecto y vuelva a seleccionar **Publicar...**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-190">Right-click on the project and select **Publish...** again.</span></span>

![Menú contextual abierto con el vínculo Publicar resaltado](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="23f8a-192">Una vez publicada la aplicación, compruebe que los cambios realizados estén disponibles en Azure.</span><span class="sxs-lookup"><span data-stu-id="23f8a-192">After the app is published, verify the changes you made are available on Azure.</span></span>

![Comprobar si se ha completado la tarea](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="23f8a-194">Limpieza</span><span class="sxs-lookup"><span data-stu-id="23f8a-194">Clean up</span></span>

<span data-ttu-id="23f8a-195">Cuando haya terminado de probar la aplicación, vaya a [Azure Portal](https://portal.azure.com/) y elimínela.</span><span class="sxs-lookup"><span data-stu-id="23f8a-195">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="23f8a-196">Seleccione **Grupos de recursos** y, luego, el grupo de recursos que haya creado.</span><span class="sxs-lookup"><span data-stu-id="23f8a-196">Select **Resource groups**, then select the resource group you created.</span></span>

![Azure Portal: Grupos de recursos en el menú lateral](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="23f8a-198">En la página **Grupos de recursos**, seleccione **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-198">In the **Resource groups** page, select **Delete**.</span></span>

![Azure Portal: página Grupos de recursos](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="23f8a-200">Escriba el nombre del grupo de recursos y seleccione **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="23f8a-200">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="23f8a-201">La aplicación y todos los demás recursos que ha creado en este tutorial se han eliminado de Azure.</span><span class="sxs-lookup"><span data-stu-id="23f8a-201">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="23f8a-202">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="23f8a-202">Next steps</span></span>

* [<span data-ttu-id="23f8a-203">Implementación continua en Azure con Visual Studio y Git</span><span class="sxs-lookup"><span data-stu-id="23f8a-203">Continuous Deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)

## <a name="additonal-resources"></a><span data-ttu-id="23f8a-204">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="23f8a-204">Additonal resources</span></span>

* [<span data-ttu-id="23f8a-205">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="23f8a-205">Azure App Service</span></span>](https://docs.microsoft.com/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="23f8a-206">Grupos de recursos de Azure</span><span class="sxs-lookup"><span data-stu-id="23f8a-206">Azure resource groups</span></span>](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [<span data-ttu-id="23f8a-207">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="23f8a-207">Azure SQL Database</span></span>](https://docs.microsoft.com/azure/sql-database/)
