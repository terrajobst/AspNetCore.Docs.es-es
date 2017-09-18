---
title: "Publicar una aplicación de ASP.NET Core en Azure con Visual Studio"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: c4df5f4551760fbc4ed4b11362d249b24f186e00
ms.sourcegitcommit: 8f5277871eff86134ebf68d3737196cfd4a62c2c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="2c27f-103">Publicar una aplicación web de ASP.NET Core en Azure App Service con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c27f-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="2c27f-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Cesar Blum Silveira](https://github.com/cesarbs)</span><span class="sxs-lookup"><span data-stu-id="2c27f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Cesar Blum Silveira](https://github.com/cesarbs)</span></span>

## <a name="set-up-the-development-environment"></a><span data-ttu-id="2c27f-105">Configuración del entorno de desarrollo</span><span class="sxs-lookup"><span data-stu-id="2c27f-105">Set up the development environment</span></span>

* <span data-ttu-id="2c27f-106">Instale la versión más reciente de [Azure SDK para Visual Studio](https://www.visualstudio.com/features/azure-tools-vs).</span><span class="sxs-lookup"><span data-stu-id="2c27f-106">Install the latest [Azure SDK for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs).</span></span> <span data-ttu-id="2c27f-107">El SDK instala Visual Studio si aún no lo tiene.</span><span class="sxs-lookup"><span data-stu-id="2c27f-107">The SDK installs Visual Studio if you don't already have it.</span></span>

> [!NOTE]
> <span data-ttu-id="2c27f-108">La instalación del SDK puede tardar más de 30 minutos si el equipo no tiene muchas de las dependencias.</span><span class="sxs-lookup"><span data-stu-id="2c27f-108">The SDK installation can take more than 30 minutes if your machine doesn't have many of the dependencies.</span></span>

* <span data-ttu-id="2c27f-109">Instale [.NET Core + las herramientas de Visual Studio](http://go.microsoft.com/fwlink/?LinkID=798306)</span><span class="sxs-lookup"><span data-stu-id="2c27f-109">Install [.NET Core + Visual Studio tooling](http://go.microsoft.com/fwlink/?LinkID=798306)</span></span>

* <span data-ttu-id="2c27f-110">Compruebe la [cuenta de Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="2c27f-110">Verify your [Azure account](https://portal.azure.com/).</span></span> <span data-ttu-id="2c27f-111">Puede [abrir una cuenta de Azure gratuita](https://azure.microsoft.com/pricing/free-trial/) o [activar las ventajas de suscriptor de Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="2c27f-111">You can [open a free Azure account](https://azure.microsoft.com/pricing/free-trial/) or [Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="2c27f-112">Creación de una aplicación web</span><span class="sxs-lookup"><span data-stu-id="2c27f-112">Create a web app</span></span>

<span data-ttu-id="2c27f-113">En la página de inicio de Visual Studio, pulse en **Nuevo proyecto...**</span><span class="sxs-lookup"><span data-stu-id="2c27f-113">In the Visual Studio Start Page, tap **New Project...**.</span></span>

![Página de inicio](publish-to-azure-webapp-using-vs/_static/new_project.png)

<span data-ttu-id="2c27f-115">También puede usar los menús para crear un nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="2c27f-115">Alternatively, you can use the menus to create a new project.</span></span> <span data-ttu-id="2c27f-116">Pulse en **Archivo > Nuevo > Proyecto...**</span><span class="sxs-lookup"><span data-stu-id="2c27f-116">Tap **File > New > Project...**.</span></span>

![menú Archivo](publish-to-azure-webapp-using-vs/_static/alt_new_project.png)

<span data-ttu-id="2c27f-118">Rellene el cuadro de diálogo **Nuevo proyecto**:</span><span class="sxs-lookup"><span data-stu-id="2c27f-118">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="2c27f-119">En el panel izquierdo, pulse en **Web**.</span><span class="sxs-lookup"><span data-stu-id="2c27f-119">In the left pane, tap **Web**</span></span>

* <span data-ttu-id="2c27f-120">En el panel central, pulse en **Aplicación web de ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="2c27f-120">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>

* <span data-ttu-id="2c27f-121">Pulse en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="2c27f-121">Tap **OK**</span></span>

![Cuadro de diálogo Nuevo proyecto](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="2c27f-123">En el cuadro de diálogo **Nueva aplicación web de ASP.NET Core (.NET Core)**:</span><span class="sxs-lookup"><span data-stu-id="2c27f-123">In the **New ASP.NET Core Web Application (.NET Core)** dialog:</span></span>

* <span data-ttu-id="2c27f-124">Pulse en **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="2c27f-124">Tap **Web Application**</span></span>

* <span data-ttu-id="2c27f-125">Compruebe que **Autenticación** está establecido en **Cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="2c27f-125">Verify **Authentication** is set to **Individual User Accounts**</span></span>

* <span data-ttu-id="2c27f-126">Compruebe que **Host en la nube** **no** está activado.</span><span class="sxs-lookup"><span data-stu-id="2c27f-126">Verify **Host in the cloud** is **not** checked</span></span>

* <span data-ttu-id="2c27f-127">Pulse en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="2c27f-127">Tap **OK**</span></span>

![Cuadro de diálogo Nueva aplicación web de ASP.NET Core (.NET Core)](publish-to-azure-webapp-using-vs/_static/noath.png)

## <a name="test-the-app-locally"></a><span data-ttu-id="2c27f-129">Prueba de la aplicación en local</span><span class="sxs-lookup"><span data-stu-id="2c27f-129">Test the app locally</span></span>

* <span data-ttu-id="2c27f-130">Presione **Ctrl-F5** para ejecutar la aplicación en local.</span><span class="sxs-lookup"><span data-stu-id="2c27f-130">Press **Ctrl-F5** to run the app locally</span></span>

* <span data-ttu-id="2c27f-131">Pulse en los vínculos **Acerca de** y **Contacto**.</span><span class="sxs-lookup"><span data-stu-id="2c27f-131">Tap the **About** and **Contact** links.</span></span> <span data-ttu-id="2c27f-132">Según el tamaño del dispositivo, es posible que tenga que pulsar en el icono de navegación para que se muestren los vínculos.</span><span class="sxs-lookup"><span data-stu-id="2c27f-132">Depending on the size of your device, you might need to tap the navigation icon to show the links</span></span>

![Aplicación web abierta en Microsoft Edge en host local](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="2c27f-134">Pulse en **Registrar** y registre un nuevo usuario.</span><span class="sxs-lookup"><span data-stu-id="2c27f-134">Tap **Register** and register a new user.</span></span> <span data-ttu-id="2c27f-135">Puede usar una dirección de correo electrónico ficticia.</span><span class="sxs-lookup"><span data-stu-id="2c27f-135">You can use a fictitious email address.</span></span> <span data-ttu-id="2c27f-136">Al enviar, aparece el siguiente error:</span><span class="sxs-lookup"><span data-stu-id="2c27f-136">When you submit, you'll get the following error:</span></span>

![Error interno del servidor: Error en una operación de base de datos al procesar la solicitud.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="2c27f-140">Puede corregir el problema de dos maneras distintas:</span><span class="sxs-lookup"><span data-stu-id="2c27f-140">You can fix the problem in two different ways:</span></span>

* <span data-ttu-id="2c27f-141">Pulse en **Aplicar migraciones** y, una vez actualizada la página, actualice la página; o</span><span class="sxs-lookup"><span data-stu-id="2c27f-141">Tap **Apply Migrations** and, once the page updates, refresh the page; or</span></span>

* <span data-ttu-id="2c27f-142">Ejecute lo siguiente desde un símbolo del sistema en el directorio del proyecto:</span><span class="sxs-lookup"><span data-stu-id="2c27f-142">Run the following from a command prompt in the project's directory:</span></span>

  <!-- literal_block {"ids": [], "xml:space": "preserve"} -->

  ```
  dotnet ef database update
     ```

<span data-ttu-id="2c27f-143">La aplicación muestra el correo electrónico usado para registrar al nuevo usuario y un vínculo **Cerrar sesión**.</span><span class="sxs-lookup"><span data-stu-id="2c27f-143">The app displays the email used to register the new user and a **Log off** link.</span></span>

![Aplicación web abierta en Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="2c27f-146">Implementación de la aplicación en Azure</span><span class="sxs-lookup"><span data-stu-id="2c27f-146">Deploy the app to Azure</span></span>

<span data-ttu-id="2c27f-147">Confirme que la aplicación publicada para la implementación no se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="2c27f-147">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="2c27f-148">Los archivos de la carpeta *publish* quedan bloqueados mientras se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2c27f-148">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="2c27f-149">No se puede llevar a cabo la implementación porque no se pueden copiar los archivos bloqueados.</span><span class="sxs-lookup"><span data-stu-id="2c27f-149">Deployment can't occur because locked files can't be copied.</span></span>

<span data-ttu-id="2c27f-150">Desde el Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **Publicar...**</span><span class="sxs-lookup"><span data-stu-id="2c27f-150">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Menú contextual abierto con el vínculo Publicar resaltado](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="2c27f-152">En el cuadro de diálogo **Publicar**, pulse en **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="2c27f-152">In the **Publish** dialog, tap **Microsoft Azure App Service**.</span></span>

![Cuadro de diálogo Publicar](publish-to-azure-webapp-using-vs/_static/maas1.png)

<span data-ttu-id="2c27f-154">Pulse en **Nuevo...** para crear un nuevo grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="2c27f-154">Tap **New...** to create a new resource group.</span></span> <span data-ttu-id="2c27f-155">Al crear un grupo de recursos nuevo, le será más fácil eliminar todos los recursos de Azure que se creen en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="2c27f-155">Creating a new resource group will make it easier to delete all the Azure resources you create in this tutorial.</span></span>

![Cuadro de diálogo App Service](publish-to-azure-webapp-using-vs/_static/newrg1.png)

<span data-ttu-id="2c27f-157">Cree un nuevo grupo de recursos y un plan de App Service:</span><span class="sxs-lookup"><span data-stu-id="2c27f-157">Create a new resource group and app service plan:</span></span>

* <span data-ttu-id="2c27f-158">Pulse en **Nuevo...** junto al grupo de recursos y escriba un nombre para el nuevo grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="2c27f-158">Tap **New...** for the resource group and enter a name for the new resource group</span></span>

* <span data-ttu-id="2c27f-159">Pulse en **Nuevo...** junto al plan de App Service y seleccione una ubicación cercana.</span><span class="sxs-lookup"><span data-stu-id="2c27f-159">Tap **New...** for the  app service plan and select a location near you.</span></span> <span data-ttu-id="2c27f-160">Puede conservar el nombre generado predeterminado.</span><span class="sxs-lookup"><span data-stu-id="2c27f-160">You can keep the default generated name</span></span>

* <span data-ttu-id="2c27f-161">Pulse en **Explorar servicios adicionales de Azure** para crear una nueva base de datos.</span><span class="sxs-lookup"><span data-stu-id="2c27f-161">Tap **Explore additional Azure services** to create a new database</span></span>

![Cuadro de diálogo Nuevo grupo de recursos: panel Hospedaje](publish-to-azure-webapp-using-vs/_static/cas.png)

* <span data-ttu-id="2c27f-163">Pulse en el icono verde **+** para crear una nueva instancia de SQL Database.</span><span class="sxs-lookup"><span data-stu-id="2c27f-163">Tap the green **+** icon to create a new SQL Database</span></span>

![Cuadro de diálogo Nuevo grupo de recursos: panel Servicios](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="2c27f-165">Pulse en **Nuevo...** en el cuadro de diálogo **Configurar base de datos SQL** para crear un nuevo servidor de base de datos.</span><span class="sxs-lookup"><span data-stu-id="2c27f-165">Tap **New...** on the **Configure SQL Database** dialog to create a new database server.</span></span>

![Cuadro de diálogo Configurar base de datos SQL](publish-to-azure-webapp-using-vs/_static/conf.png)

* <span data-ttu-id="2c27f-167">Escriba un nombre de usuario y una contraseña de administrador y luego pulse en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="2c27f-167">Enter an administrator user name and password, and then tap **OK**.</span></span> <span data-ttu-id="2c27f-168">No olvide el nombre de usuario y la contraseña que ha creado en este paso.</span><span class="sxs-lookup"><span data-stu-id="2c27f-168">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="2c27f-169">Puede conservar el **nombre de servidor** predeterminado.</span><span class="sxs-lookup"><span data-stu-id="2c27f-169">You can keep the default **Server Name**</span></span>

![Cuadro de diálogo Configurar SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

> [!NOTE]
> <span data-ttu-id="2c27f-171">No se permite "admin" como nombre de usuario de administrador.</span><span class="sxs-lookup"><span data-stu-id="2c27f-171">"admin" is not allowed as the administrator user name.</span></span>

* <span data-ttu-id="2c27f-172">Pulse en **Aceptar** en el cuadro de diálogo **Configurar base de datos SQL**.</span><span class="sxs-lookup"><span data-stu-id="2c27f-172">Tap **OK** on the  **Configure SQL Database** dialog</span></span>

![Cuadro de diálogo Configurar base de datos SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="2c27f-174">Pulse en **Crear** en el cuadro de diálogo **Crear servicio de aplicaciones**.</span><span class="sxs-lookup"><span data-stu-id="2c27f-174">Tap **Create** on the **Create App Service** dialog</span></span>

![Cuadro de diálogo Crear servicio de aplicaciones](publish-to-azure-webapp-using-vs/_static/create_as.png)

* <span data-ttu-id="2c27f-176">Pulse en **Siguiente** en el cuadro de diálogo **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="2c27f-176">Tap **Next** in the **Publish** dialog</span></span>

![Cuadro de diálogo Publicar: panel Conexión](publish-to-azure-webapp-using-vs/_static/pubc.png)

* <span data-ttu-id="2c27f-178">En la fase **Configuración** del cuadro de diálogo **Publicar**:</span><span class="sxs-lookup"><span data-stu-id="2c27f-178">On the **Settings** stage of the **Publish** dialog:</span></span>

  * <span data-ttu-id="2c27f-179">Expanda **Bases de datos** y active **Usar esta cadena de conexión en tiempo de ejecución**.</span><span class="sxs-lookup"><span data-stu-id="2c27f-179">Expand **Databases** and check **Use this connection string at runtime**</span></span>

  * <span data-ttu-id="2c27f-180">Expanda **Migraciones de Entity Framework** y active **Aplicar esta migración al publicar**.</span><span class="sxs-lookup"><span data-stu-id="2c27f-180">Expand **Entity Framework Migrations** and check **Apply this migration on publish**</span></span>

* <span data-ttu-id="2c27f-181">Pulse en **Publicar** y espere hasta que Visual Studio termine de publicar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2c27f-181">Tap **Publish** and wait until Visual Studio finishes publishing your app</span></span>

![Cuadro de diálogo Publicar: panel Configuración](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="2c27f-183">Visual Studio publica la aplicación en Azure e inicia Cloud App en el explorador.</span><span class="sxs-lookup"><span data-stu-id="2c27f-183">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="2c27f-184">Prueba de la aplicación en Azure</span><span class="sxs-lookup"><span data-stu-id="2c27f-184">Test your app in Azure</span></span>

* <span data-ttu-id="2c27f-185">Pruebe los vínculos **Acerca de** y **Contacto**.</span><span class="sxs-lookup"><span data-stu-id="2c27f-185">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="2c27f-186">Registre un nuevo usuario.</span><span class="sxs-lookup"><span data-stu-id="2c27f-186">Register a new user</span></span>

![Aplicación web abierta en Microsoft Edge en Azure App Service](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="update-the-app"></a><span data-ttu-id="2c27f-188">Actualización de la aplicación</span><span class="sxs-lookup"><span data-stu-id="2c27f-188">Update the app</span></span>

* <span data-ttu-id="2c27f-189">Edite el archivo de vista `Views/Home/About.cshtml` de Razor y cambie su contenido.</span><span class="sxs-lookup"><span data-stu-id="2c27f-189">Edit the `Views/Home/About.cshtml` Razor view file and change its contents.</span></span> <span data-ttu-id="2c27f-190">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2c27f-190">For example:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [7]}} -->

```html
@{
       ViewData["Title"] = "About";
   }
   <h2>@ViewData["Title"].</h2>
   <h3>@ViewData["Message"]</h3>

   <p>My updated about page.</p>
   ```

* <span data-ttu-id="2c27f-191">Haga clic con el botón derecho en el proyecto y vuelva a pulsar en **Publicar...**</span><span class="sxs-lookup"><span data-stu-id="2c27f-191">Right-click on the project and tap **Publish...** again</span></span>

![Menú contextual abierto con el vínculo Publicar resaltado](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="2c27f-193">Una vez publicada la aplicación, compruebe que los cambios realizados están disponibles en Azure.</span><span class="sxs-lookup"><span data-stu-id="2c27f-193">After the app is published, verify the changes you made are available on Azure</span></span>

### <a name="clean-up"></a><span data-ttu-id="2c27f-194">Limpieza</span><span class="sxs-lookup"><span data-stu-id="2c27f-194">Clean up</span></span>

<span data-ttu-id="2c27f-195">Cuando haya terminado de probar la aplicación, vaya a [Azure Portal](https://portal.azure.com/) y elimínela.</span><span class="sxs-lookup"><span data-stu-id="2c27f-195">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="2c27f-196">Seleccione **Grupos de recursos** y luego pulse en el grupo de recursos que ha creado.</span><span class="sxs-lookup"><span data-stu-id="2c27f-196">Select **Resource groups**, then tap the resource group you created</span></span>

![Azure Portal: Grupos de recursos en el menú lateral](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="2c27f-198">En la hoja **Grupo de recursos**, pulse en **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="2c27f-198">In the **Resource group** blade, tap **Delete**</span></span>

![Azure Portal: hoja Grupos de recursos](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="2c27f-200">Escriba el nombre del grupo de recursos y pulse en **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="2c27f-200">Enter the name of the resource group and tap **Delete**.</span></span> <span data-ttu-id="2c27f-201">La aplicación y todos los demás recursos que ha creado en este tutorial se han eliminado de Azure.</span><span class="sxs-lookup"><span data-stu-id="2c27f-201">Your app and all other resources created in this tutorial are now deleted from Azure</span></span>

### <a name="next-steps"></a><span data-ttu-id="2c27f-202">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="2c27f-202">Next steps</span></span>

* [<span data-ttu-id="2c27f-203">Introducción a ASP.NET Core MVC y Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c27f-203">Getting started with ASP.NET Core MVC and Visual Studio</span></span>](first-mvc-app/start-mvc.md)

* [<span data-ttu-id="2c27f-204">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2c27f-204">Introduction to ASP.NET Core</span></span>](../index.md)

* [<span data-ttu-id="2c27f-205">Aspectos básicos</span><span class="sxs-lookup"><span data-stu-id="2c27f-205">Fundamentals</span></span>](../fundamentals/index.md)
