---
title: Publicar una aplicación de ASP.NET Core en Azure con Visual Studio
author: rick-anderson
description: Obtenga información sobre cómo publicar una aplicación de ASP.NET Core en Azure App Service con Visual Studio.
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2019
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 7fc3644df3dcb957f2537538aaa9506c6b38a480
ms.sourcegitcommit: 79850db9e79b1705b89f466c6f2c961ff15485de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2020
ms.locfileid: "75693978"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-visual-studio"></a><span data-ttu-id="246bf-103">Publicar una aplicación de ASP.NET Core en Azure con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="246bf-103">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>

<span data-ttu-id="246bf-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="246bf-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>
::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

::: moniker-end


<span data-ttu-id="246bf-105">Consulte [Publicación de una aplicación web en Azure App Service mediante Visual Studio para Mac](https://docs.microsoft.com/visualstudio/mac/publish-app-svc?view=vsmac-2019) si trabaja en macOS.</span><span class="sxs-lookup"><span data-stu-id="246bf-105">See [Publish a Web app to Azure App Service using Visual Studio for Mac](https://docs.microsoft.com/visualstudio/mac/publish-app-svc?view=vsmac-2019) if you are working on macOS.</span></span>

<span data-ttu-id="246bf-106">Para solucionar un problema de implementación de App Service, vea <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="246bf-106">To troubleshoot an App Service deployment issue, see <xref:test/troubleshoot-azure-iis>.</span></span>

## <a name="set-up"></a><span data-ttu-id="246bf-107">Configurar</span><span class="sxs-lookup"><span data-stu-id="246bf-107">Set up</span></span>

* <span data-ttu-id="246bf-108">Abra una [cuenta gratuita de Azure](https://azure.microsoft.com/free/dotnet/) si no tiene una.</span><span class="sxs-lookup"><span data-stu-id="246bf-108">Open a [free Azure account](https://azure.microsoft.com/free/dotnet/) if you don't have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="246bf-109">Creación de una aplicación web</span><span class="sxs-lookup"><span data-stu-id="246bf-109">Create a web app</span></span>

<span data-ttu-id="246bf-110">En la página de inicio de Visual Studio, seleccione **Archivo > Nuevo > Proyecto...** .</span><span class="sxs-lookup"><span data-stu-id="246bf-110">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![menú Archivo](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="246bf-112">Complete el cuadro de diálogo **Nuevo proyecto**:</span><span class="sxs-lookup"><span data-stu-id="246bf-112">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="246bf-113">En el panel izquierdo, seleccione **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="246bf-113">In the left pane, select **.NET Core**.</span></span>
* <span data-ttu-id="246bf-114">En el panel central, seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="246bf-114">In the center pane, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="246bf-115">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="246bf-115">Select **OK**.</span></span>

![Cuadro de diálogo Nuevo proyecto](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="246bf-117">En el cuadro de diálogo **Nueva aplicación web de ASP.NET Core**, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="246bf-117">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="246bf-118">Seleccione **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="246bf-118">Select **Web Application**.</span></span>
* <span data-ttu-id="246bf-119">Seleccione **Cambiar autenticación**.</span><span class="sxs-lookup"><span data-stu-id="246bf-119">Select **Change Authentication**.</span></span>

![Cuadro de diálogo Nuevo proyecto](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="246bf-121">Se mostrará el cuadro de diálogo **Cambiar autenticación**.</span><span class="sxs-lookup"><span data-stu-id="246bf-121">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="246bf-122">Seleccione **Cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="246bf-122">Select **Individual User Accounts**.</span></span>
* <span data-ttu-id="246bf-123">Seleccione **Aceptar** para volver a la **nueva aplicación web de ASP.NET Core** y vuelva a seleccionar **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="246bf-123">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Cuadro de diálogo de autenticación web de ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="246bf-125">Visual Studio crea la solución.</span><span class="sxs-lookup"><span data-stu-id="246bf-125">Visual Studio creates the solution.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="246bf-126">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="246bf-126">Run the app</span></span>

* <span data-ttu-id="246bf-127">Presione CTRL+F5 para ejecutar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="246bf-127">Press CTRL+F5 to run the project.</span></span>
* <span data-ttu-id="246bf-128">Pruebe los vínculos **Acerca de** y **Contacto**.</span><span class="sxs-lookup"><span data-stu-id="246bf-128">Test the **About** and **Contact** links.</span></span>

![Aplicación web abierta en Microsoft Edge en host local](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a><span data-ttu-id="246bf-130">Registrar un usuario</span><span class="sxs-lookup"><span data-stu-id="246bf-130">Register a user</span></span>

* <span data-ttu-id="246bf-131">Seleccione **Registrar** y registre a un usuario nuevo.</span><span class="sxs-lookup"><span data-stu-id="246bf-131">Select **Register** and register a new user.</span></span> <span data-ttu-id="246bf-132">Puede usar una dirección de correo electrónico ficticia.</span><span class="sxs-lookup"><span data-stu-id="246bf-132">You can use a fictitious email address.</span></span> <span data-ttu-id="246bf-133">Al enviar la información, la página mostrará el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="246bf-133">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="246bf-134">*"Error interno del servidor: Error en una operación de base de datos al procesar la solicitud. Excepción de SQL: No se puede abrir la base de datos. La aplicación de las migraciones existentes para el contexto de base de datos de la aplicación puede solucionar este problema".*</span><span class="sxs-lookup"><span data-stu-id="246bf-134">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>
* <span data-ttu-id="246bf-135">Seleccione **Aplicar migraciones** y, una vez actualizada la página, vuelva a cargarla.</span><span class="sxs-lookup"><span data-stu-id="246bf-135">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Error interno del servidor: Error en una operación de base de datos al procesar la solicitud.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="246bf-139">La aplicación muestra el correo electrónico usado para registrar al nuevo usuario y el vínculo **Cerrar sesión**.</span><span class="sxs-lookup"><span data-stu-id="246bf-139">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Aplicación web abierta en Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="246bf-142">Implementación de la aplicación en Azure</span><span class="sxs-lookup"><span data-stu-id="246bf-142">Deploy the app to Azure</span></span>

<span data-ttu-id="246bf-143">Desde el Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **Publicar...**</span><span class="sxs-lookup"><span data-stu-id="246bf-143">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Menú contextual abierto con el vínculo Publicar resaltado](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="246bf-145">En el cuadro de diálogo **Publicar**:</span><span class="sxs-lookup"><span data-stu-id="246bf-145">In the **Publish** dialog:</span></span>

* <span data-ttu-id="246bf-146">Seleccione **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="246bf-146">Select **Microsoft Azure App Service**.</span></span>
* <span data-ttu-id="246bf-147">Seleccione el icono de engranaje y luego **Crear perfil**.</span><span class="sxs-lookup"><span data-stu-id="246bf-147">Select the gear icon and then select **Create Profile**.</span></span>
* <span data-ttu-id="246bf-148">Seleccione **Crear perfil**.</span><span class="sxs-lookup"><span data-stu-id="246bf-148">Select **Create Profile**.</span></span>

![Cuadro de diálogo Publicar](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a><span data-ttu-id="246bf-150">Crear recursos de Azure</span><span class="sxs-lookup"><span data-stu-id="246bf-150">Create Azure resources</span></span>

<span data-ttu-id="246bf-151">Aparece el cuadro de diálogo **Crear servicio de aplicaciones**:</span><span class="sxs-lookup"><span data-stu-id="246bf-151">The **Create App Service** dialog appears:</span></span>

* <span data-ttu-id="246bf-152">Especifique la suscripción.</span><span class="sxs-lookup"><span data-stu-id="246bf-152">Enter your subscription.</span></span>
* <span data-ttu-id="246bf-153">Se rellenan los campos de entrada **Nombre de la aplicación**, **Grupo de recursos** y **Plan de App Service**.</span><span class="sxs-lookup"><span data-stu-id="246bf-153">The **App Name**, **Resource Group**, and **App Service Plan** entry fields are populated.</span></span> <span data-ttu-id="246bf-154">Puede mantener estos nombres o cambiarlos.</span><span class="sxs-lookup"><span data-stu-id="246bf-154">You can keep these names or change them.</span></span>

![Cuadro de diálogo App Service](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="246bf-156">Seleccione la pestaña **Servicios** para crear una base de datos.</span><span class="sxs-lookup"><span data-stu-id="246bf-156">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="246bf-157">Seleccione el icono verde **+** para crear una instancia de SQL Database.</span><span class="sxs-lookup"><span data-stu-id="246bf-157">Select the green **+** icon to create a new SQL Database</span></span>

![Crear una base de datos de SQL Database](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="246bf-159">Seleccione **Nuevo...** en el cuadro de diálogo **Configurar SQL Database** para crear una base de datos.</span><span class="sxs-lookup"><span data-stu-id="246bf-159">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Crear una base de datos y un servidor de SQL Database](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="246bf-161">Se mostrará el cuadro de diálogo **Configurar SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="246bf-161">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="246bf-162">Escriba un nombre de usuario y una contraseña de administrador y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="246bf-162">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="246bf-163">Puede conservar el **nombre de servidor** predeterminado.</span><span class="sxs-lookup"><span data-stu-id="246bf-163">You can keep the default **Server Name**.</span></span> 

> [!NOTE]
> <span data-ttu-id="246bf-164">No se permite "admin" como nombre de usuario de administrador.</span><span class="sxs-lookup"><span data-stu-id="246bf-164">"admin" isn't allowed as the administrator user name.</span></span>

![Cuadro de diálogo Configurar SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="246bf-166">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="246bf-166">Select **OK**.</span></span>

<span data-ttu-id="246bf-167">Visual Studio volverá al cuadro de diálogo **Crear un servicio de App Service**.</span><span class="sxs-lookup"><span data-stu-id="246bf-167">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="246bf-168">Seleccione **Crear** en el cuadro de diálogo **Crear un servicio de App Service**.</span><span class="sxs-lookup"><span data-stu-id="246bf-168">Select **Create** on the **Create App Service** dialog.</span></span>

![Cuadro de diálogo Configurar base de datos SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

<span data-ttu-id="246bf-170">Visual Studio crea la aplicación web y SQL Server en Azure.</span><span class="sxs-lookup"><span data-stu-id="246bf-170">Visual Studio creates the Web app and SQL Server on Azure.</span></span> <span data-ttu-id="246bf-171">Este paso puede llevar varios minutos.</span><span class="sxs-lookup"><span data-stu-id="246bf-171">This step can take a few minutes.</span></span> <span data-ttu-id="246bf-172">Para más información sobre los recursos creados, vea [Recursos adicionales](#additional-resources).</span><span class="sxs-lookup"><span data-stu-id="246bf-172">For information on the resources created, see [Additional resources](#additional-resources).</span></span>

<span data-ttu-id="246bf-173">Cuando termine la implementación, seleccione **Configuración**:</span><span class="sxs-lookup"><span data-stu-id="246bf-173">When deployment completes, select **Settings**:</span></span>

![Cuadro de diálogo Configurar SQL Server](publish-to-azure-webapp-using-vs/_static/set.png)

<span data-ttu-id="246bf-175">En la página **Configuración** del cuadro de diálogo **Publicar**, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="246bf-175">On the **Settings** page of the **Publish** dialog:</span></span>

* <span data-ttu-id="246bf-176">Expanda **Bases de datos** y active **Usar esta cadena de conexión en tiempo de ejecución**.</span><span class="sxs-lookup"><span data-stu-id="246bf-176">Expand **Databases** and check **Use this connection string at runtime**.</span></span>
* <span data-ttu-id="246bf-177">Expanda **Migraciones de Entity Framework** y active **Aplicar esta migración al publicar**.</span><span class="sxs-lookup"><span data-stu-id="246bf-177">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="246bf-178">Seleccione **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="246bf-178">Select **Save**.</span></span> <span data-ttu-id="246bf-179">Visual Studio volverá al cuadro de diálogo **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="246bf-179">Visual Studio returns to the **Publish** dialog.</span></span> 

![Cuadro de diálogo Publicar: Panel Configuración](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="246bf-181">Haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="246bf-181">Click **Publish**.</span></span> <span data-ttu-id="246bf-182">Visual Studio publica la aplicación en Azure.</span><span class="sxs-lookup"><span data-stu-id="246bf-182">Visual Studio publishes your app to Azure.</span></span> <span data-ttu-id="246bf-183">Cuando la implementación termine, la aplicación se abre en un explorador.</span><span class="sxs-lookup"><span data-stu-id="246bf-183">When the deployment completes, the app is opened in a browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="246bf-184">Prueba de la aplicación en Azure</span><span class="sxs-lookup"><span data-stu-id="246bf-184">Test your app in Azure</span></span>

* <span data-ttu-id="246bf-185">Pruebe los vínculos **Acerca de** y **Contacto**.</span><span class="sxs-lookup"><span data-stu-id="246bf-185">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="246bf-186">Registre un nuevo usuario.</span><span class="sxs-lookup"><span data-stu-id="246bf-186">Register a new user</span></span>

![Aplicación web abierta en Microsoft Edge en Azure App Service](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="246bf-188">Actualización de la aplicación</span><span class="sxs-lookup"><span data-stu-id="246bf-188">Update the app</span></span>

* <span data-ttu-id="246bf-189">Edite la página de Razor *Pages/About.cshtml* y modifique su contenido.</span><span class="sxs-lookup"><span data-stu-id="246bf-189">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="246bf-190">Por ejemplo, puede editar el párrafo para que incluya el mensaje "¡Hola, ASP.NET Core!":</span><span class="sxs-lookup"><span data-stu-id="246bf-190">For example, you can modify the paragraph to say "Hello ASP.NET Core!":</span></span>

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* <span data-ttu-id="246bf-191">Haga clic con el botón derecho sobre el proyecto y vuelva a seleccionar **Publicar...** .</span><span class="sxs-lookup"><span data-stu-id="246bf-191">Right-click on the project and select **Publish...** again.</span></span>

![Menú contextual abierto con el vínculo Publicar resaltado](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="246bf-193">Una vez publicada la aplicación, compruebe que los cambios realizados estén disponibles en Azure.</span><span class="sxs-lookup"><span data-stu-id="246bf-193">After the app is published, verify the changes you made are available on Azure.</span></span>

![Comprobar si se ha completado la tarea](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="246bf-195">Limpieza</span><span class="sxs-lookup"><span data-stu-id="246bf-195">Clean up</span></span>

<span data-ttu-id="246bf-196">Cuando haya terminado de probar la aplicación, vaya a [Azure Portal](https://portal.azure.com/) y elimínela.</span><span class="sxs-lookup"><span data-stu-id="246bf-196">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="246bf-197">Seleccione **Grupos de recursos** y, luego, el grupo de recursos que haya creado.</span><span class="sxs-lookup"><span data-stu-id="246bf-197">Select **Resource groups**, then select the resource group you created.</span></span>

![Azure Portal: Grupos de recursos en el menú lateral](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="246bf-199">En la página **Grupos de recursos**, seleccione **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="246bf-199">In the **Resource groups** page, select **Delete**.</span></span>

![Azure Portal: página Grupos de recursos](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="246bf-201">Escriba el nombre del grupo de recursos y seleccione **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="246bf-201">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="246bf-202">La aplicación y todos los demás recursos que ha creado en este tutorial se han eliminado de Azure.</span><span class="sxs-lookup"><span data-stu-id="246bf-202">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="246bf-203">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="246bf-203">Next steps</span></span>

* <xref:host-and-deploy/azure-apps/azure-continuous-deployment>

## <a name="additional-resources"></a><span data-ttu-id="246bf-204">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="246bf-204">Additional resources</span></span>

* <span data-ttu-id="246bf-205">Para Visual Studio Code, vea [Publicar los perfiles](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="246bf-205">For Visual Studio Code, see [Publish profiles](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles).</span></span>
* [<span data-ttu-id="246bf-206">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="246bf-206">Azure App Service</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="246bf-207">Grupos de recursos de Azure</span><span class="sxs-lookup"><span data-stu-id="246bf-207">Azure resource groups</span></span>](/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [<span data-ttu-id="246bf-208">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="246bf-208">Azure SQL Database</span></span>](/azure/sql-database/)
* <xref:host-and-deploy/visual-studio-publish-profiles>
* <xref:test/troubleshoot-azure-iis>
