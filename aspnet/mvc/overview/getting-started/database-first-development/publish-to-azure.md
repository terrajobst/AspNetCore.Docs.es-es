---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: Publicar sitio MVC Database First en Azure | Microsoft Docs
author: tfitzmac
description: Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente. Este tutorial seri...
ms.author: aspnetcontent
ms.date: 12/22/2014
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 0aaa8e2a586a89f6ea5eaeb4f3d280993342b2f9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835753"
---
<a name="publish-mvc-database-first-site-to-azure"></a><span data-ttu-id="93102-104">Publicar sitio MVC Database First en Azure</span><span class="sxs-lookup"><span data-stu-id="93102-104">Publish MVC Database First site to Azure</span></span>
====================
<span data-ttu-id="93102-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="93102-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="93102-106">Con Scaffolding de ASP.NET, MVC y Entity Framework, puede crear una aplicación web que proporciona una interfaz a una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="93102-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="93102-107">Esta serie de tutoriales muestra cómo generar el código que permite a los usuarios mostrar, editar, crear automáticamente y eliminar datos que residen en una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="93102-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="93102-108">El código generado corresponde a las columnas de la tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="93102-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="93102-109">Esta parte de la serie se centra en la aplicación web y la base de datos de publicación en Azure.</span><span class="sxs-lookup"><span data-stu-id="93102-109">This part of the series focuses on publishing the web app and database to Azure.</span></span> <span data-ttu-id="93102-110">Puede leer este tema para obtener información sobre cómo publicar una aplicación web y la base de datos, pero para llevar a cabo los pasos que debe empezar al principio del tutorial.</span><span class="sxs-lookup"><span data-stu-id="93102-110">You can read this topic to learn about publishing a web app and database, but to actually perform the steps you must start at the beginning of the tutorial.</span></span> <span data-ttu-id="93102-111">Consulte [Introducción](setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="93102-111">See [Getting Started](setting-up-database.md).</span></span>


## <a name="deploy-your-web-app-on-azure"></a><span data-ttu-id="93102-112">Implementar la aplicación web en Azure</span><span class="sxs-lookup"><span data-stu-id="93102-112">Deploy your web app on Azure</span></span>

<span data-ttu-id="93102-113">Necesita una cuenta de Azure para completar este tutorial:</span><span class="sxs-lookup"><span data-stu-id="93102-113">You need an Azure account to complete this tutorial:</span></span>

- <span data-ttu-id="93102-114">También puede [abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) -obtenga créditos puede usar para probar los servicios de Azure de pago e incluso después de que se usan hasta, puede mantener la cuenta y usar servicios gratuitos de Azure.</span><span class="sxs-lookup"><span data-stu-id="93102-114">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="93102-115">También puede [activar las ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -su suscripción a MSDN le proporciona crédito todos los meses que puede usar para servicios de Azure de pago.</span><span class="sxs-lookup"><span data-stu-id="93102-115">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

<span data-ttu-id="93102-116">Para publicar la aplicación web, haga clic en el proyecto y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="93102-116">To publish your web app, right-click the project and select **Publish**.</span></span>

![Publicar el sitio](publish-to-azure/_static/image1.png)

<span data-ttu-id="93102-118">Seleccione los sitios Web de Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="93102-118">Select Microsoft Azure Websites.</span></span>

![Seleccione Azure](publish-to-azure/_static/image2.png)

<span data-ttu-id="93102-120">Si no inició sesión en Azure, proporcione sus credenciales de cuenta de Azure.</span><span class="sxs-lookup"><span data-stu-id="93102-120">If you are not signed in to Azure, provide your Azure account credentials.</span></span> <span data-ttu-id="93102-121">A continuación, seleccione Nuevo para crear una nueva aplicación web.</span><span class="sxs-lookup"><span data-stu-id="93102-121">Then, select New to create a new web app.</span></span>

![nuevo sitio](publish-to-azure/_static/image3.png)

<span data-ttu-id="93102-123">Crear un nombre único para la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="93102-123">Create a unique name for your web app.</span></span> <span data-ttu-id="93102-124">Indica que el nombre es único si ve una marca de verificación verde a la derecha del campo name.</span><span class="sxs-lookup"><span data-stu-id="93102-124">You will know the name is unique if you see a green check mark to the right of the name field.</span></span> <span data-ttu-id="93102-125">Seleccione una región de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="93102-125">Select a region for your web app.</span></span> <span data-ttu-id="93102-126">Seleccione **crear nuevo servidor** para la base de datos y proporcione un nombre de usuario y una contraseña para este nuevo servidor de base de datos.</span><span class="sxs-lookup"><span data-stu-id="93102-126">Select **Create new server** for the database, and provide a user name and password for this new database server.</span></span> <span data-ttu-id="93102-127">Cuando termine, haga clic en **crear**.</span><span class="sxs-lookup"><span data-stu-id="93102-127">When finished, click **Create**.</span></span>

![Crear sitio](publish-to-azure/_static/image4.png)

<span data-ttu-id="93102-129">Los valores de conexión están ahora listo.</span><span class="sxs-lookup"><span data-stu-id="93102-129">Your connection values are now all set.</span></span> <span data-ttu-id="93102-130">Puede dejar estos valores sin modificar.</span><span class="sxs-lookup"><span data-stu-id="93102-130">You can leave these values unchanged.</span></span>

![connection](publish-to-azure/_static/image5.png)

<span data-ttu-id="93102-132">Haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="93102-132">Click **Next**.</span></span>

<span data-ttu-id="93102-133">Para la configuración, tenga en cuenta que dos bases de datos las conexiones se especifican - ApplicationDBContext y ContosoUniversityDataEntities.</span><span class="sxs-lookup"><span data-stu-id="93102-133">For Settings, notice that two database connections are specified - ApplicationDBContext and ContosoUniversityDataEntities.</span></span> <span data-ttu-id="93102-134">ApplicationDBContext es la conexión para las tablas de la cuenta de usuario.</span><span class="sxs-lookup"><span data-stu-id="93102-134">ApplicationDBContext is the connection for user account tables.</span></span> <span data-ttu-id="93102-135">Estos valores solo muestran las cadenas de conexión para las bases de datos.</span><span class="sxs-lookup"><span data-stu-id="93102-135">These values only show the connection strings for the databases.</span></span> <span data-ttu-id="93102-136">No significa que se va a publicar estas bases de datos al publicar su sitio.</span><span class="sxs-lookup"><span data-stu-id="93102-136">It does not mean that these databases will be published when you publish your site.</span></span> <span data-ttu-id="93102-137">Cuando haya terminado de publicar la aplicación web se publicará el proyecto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="93102-137">You will publish your database project after you have finished publishing the web app.</span></span>

![](publish-to-azure/_static/image6.png)

<span data-ttu-id="93102-138">Los puntos suspensivos (...) junto a la conexión de base de datos muestran los detalles de la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="93102-138">The ellipsis (...) next to the database connection shows you the details of the connection string.</span></span> <span data-ttu-id="93102-139">Haga clic en el botón de puntos suspensivos situado junto a ContosoUniversityDataEntities.</span><span class="sxs-lookup"><span data-stu-id="93102-139">Click the ellipsis next to ContosoUniversityDataEntities.</span></span>

![](publish-to-azure/_static/image7.png)

<span data-ttu-id="93102-140">Tenga en cuenta el nombre del servidor de base de datos y de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="93102-140">Note the name of the database server and the database.</span></span> <span data-ttu-id="93102-141">El nombre del servidor se genera aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="93102-141">The server name is randomly generated.</span></span> <span data-ttu-id="93102-142">El nombre de la base de datos es sencillo el nombre de su sitio con  **\_db** anexado.</span><span class="sxs-lookup"><span data-stu-id="93102-142">The database name is simple the name of your site with **\_db** appended.</span></span> <span data-ttu-id="93102-143">Necesitará ambos nombres más adelante al publicar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="93102-143">You will need both names later when you publish your database.</span></span>

<span data-ttu-id="93102-144">Haga clic en **Aceptar** para cerrar la ventana de cadena de conexión de base de datos.</span><span class="sxs-lookup"><span data-stu-id="93102-144">Click **OK** to close the database connection string window.</span></span>

<span data-ttu-id="93102-145">En la ventana de publicación Web, haga clic en **siguiente** para ver la vista previa.</span><span class="sxs-lookup"><span data-stu-id="93102-145">In the Publish Web window, click **Next** to see the preview.</span></span>

![](publish-to-azure/_static/image8.png)

<span data-ttu-id="93102-146">Puede hacer clic en iniciar vista previa para ver una lista de los archivos para publicar.</span><span class="sxs-lookup"><span data-stu-id="93102-146">You can click Start Preview to see a list of the files to publish.</span></span> <span data-ttu-id="93102-147">Puesto que es la primera vez que se ha publicado este sitio, la lista es todos los archivos pertinentes en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="93102-147">Since this is the first time you have published this site, the list is every relevant file in the project.</span></span>

<span data-ttu-id="93102-148">Haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="93102-148">Click **Publish**.</span></span>

<span data-ttu-id="93102-149">El panel de resultados mostrará el resultado de la publicación.</span><span class="sxs-lookup"><span data-stu-id="93102-149">The Output pane will display the result of your publication.</span></span>

![](publish-to-azure/_static/image9.png)

<span data-ttu-id="93102-150">Después de la publicación, el sitio es immedialely iniciado en un explorador web.</span><span class="sxs-lookup"><span data-stu-id="93102-150">After publication, the site is immedialely launched in a web browser.</span></span> <span data-ttu-id="93102-151">Se ha implementado el sitio y puede registrar un nuevo usuario al sitio; Sin embargo, las tablas en el proyecto ContosoUniversityData aún no se han publicado.</span><span class="sxs-lookup"><span data-stu-id="93102-151">Your site has been deployed and you can register a new user to the site; however, your tables in the ContosoUniversityData project have not yet been published.</span></span> <span data-ttu-id="93102-152">Si hace clic en la lista de vínculos de los estudiantes recibirán un error.</span><span class="sxs-lookup"><span data-stu-id="93102-152">If you click on the List of students link you will receive an error.</span></span>

## <a name="publish-database-to-sql-azure"></a><span data-ttu-id="93102-153">Publicar la base de datos en SQL Azure</span><span class="sxs-lookup"><span data-stu-id="93102-153">Publish database to SQL Azure</span></span>

<span data-ttu-id="93102-154">Antes de publicar la base de datos, debe asegurarse de que el equipo local puede conectarse al servidor de base de datos.</span><span class="sxs-lookup"><span data-stu-id="93102-154">Before publishing your database, you must make sure your local computer can connect to the database server.</span></span> <span data-ttu-id="93102-155">El firewall para el servidor de base de datos restringe que las máquinas pueden conectarse a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="93102-155">The firewall for your database server restricts which machines can connect to the database.</span></span> <span data-ttu-id="93102-156">Debe agregar la dirección IP del equipo a las direcciones IP permitidas para el firewall.</span><span class="sxs-lookup"><span data-stu-id="93102-156">You need to add the IP address of your computer to the allowed IP addresses for the firewall.</span></span>

<span data-ttu-id="93102-157">Inicie sesión en su cuenta de Azure a través del portal de Azure.</span><span class="sxs-lookup"><span data-stu-id="93102-157">Login to your Azure account through the Azure portal.</span></span>

<span data-ttu-id="93102-158">Seleccione la base de datos y seleccione **administrar**.</span><span class="sxs-lookup"><span data-stu-id="93102-158">Select your new database and select **Manage**.</span></span>

![administrar](publish-to-azure/_static/image10.png)

<span data-ttu-id="93102-160">Debe configurar el servidor de base de datos para permitir conexiones desde su equipo.</span><span class="sxs-lookup"><span data-stu-id="93102-160">You must configure your database server to allow connections from your computer.</span></span> <span data-ttu-id="93102-161">Al seleccionar administrar, deberá agregar la dirección IP actual, según lo permitido en el servidor de base de datos.</span><span class="sxs-lookup"><span data-stu-id="93102-161">When you select Manage, you are asked to add the current IP address as permitted to the database server.</span></span> <span data-ttu-id="93102-162">Seleccione Sí.</span><span class="sxs-lookup"><span data-stu-id="93102-162">Select Yes.</span></span>

![Agregar dirección ip](publish-to-azure/_static/image11.png)

<span data-ttu-id="93102-164">Es probable que la dirección IP que agregó en el paso anterior no es la única dirección IP que se debe configurar para las conexiones.</span><span class="sxs-lookup"><span data-stu-id="93102-164">There is a chance that the IP address you added in the previous step is not the only IP address you need to configure for connections.</span></span> <span data-ttu-id="93102-165">Puede intentar iniciar sesión en la base de datos para ver si las conexiones se han configurado correctamente.</span><span class="sxs-lookup"><span data-stu-id="93102-165">You can attempt to login to the database to see if the connections have been properly set up.</span></span> <span data-ttu-id="93102-166">Proporcione el usuario y contraseña que creó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="93102-166">Provide the user and password you created earlier.</span></span>

![inicio de sesión](publish-to-azure/_static/image12.png)

<span data-ttu-id="93102-168">Si recibe un mensaje de error, deberá agregar otra dirección IP.</span><span class="sxs-lookup"><span data-stu-id="93102-168">If you receive an error message, you need to add another IP address.</span></span> <span data-ttu-id="93102-169">Haga clic en el mensaje de error para ver más detalles sobre el error.</span><span class="sxs-lookup"><span data-stu-id="93102-169">Click the error message to see more details about error.</span></span> <span data-ttu-id="93102-170">En los detalles, verá la dirección IP que se debe agregar.</span><span class="sxs-lookup"><span data-stu-id="93102-170">In the details you will see the IP address that you need to add.</span></span> <span data-ttu-id="93102-171">Tenga en cuenta esta dirección IP.</span><span class="sxs-lookup"><span data-stu-id="93102-171">Note this IP address.</span></span>

![no se permite](publish-to-azure/_static/image13.png)

<span data-ttu-id="93102-173">Cierre esta ventana de inicio de sesión y vuelva al portal de Azure.</span><span class="sxs-lookup"><span data-stu-id="93102-173">Close this login window, and return to the Azure portal.</span></span>

<span data-ttu-id="93102-174">Vaya al panel de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="93102-174">Navigate to the Dashboard for your database.</span></span> <span data-ttu-id="93102-175">Haga clic en **administrar direcciones IP permitidas**.</span><span class="sxs-lookup"><span data-stu-id="93102-175">Click **Manage allowed IP addresses**.</span></span>

![administración de direcciones ip](publish-to-azure/_static/image14.png)

<span data-ttu-id="93102-177">Ahora debe agregar la dirección IP desde el mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="93102-177">You must now add the IP address from the error message.</span></span> <span data-ttu-id="93102-178">Cambie el intervalo de direcciones IP permitidas para incluir uno de los mensajes de error o agregar esa dirección IP como una entrada independiente.</span><span class="sxs-lookup"><span data-stu-id="93102-178">Either change the range of allowed IP addresses to include the one from the error message, or add that IP address as a separate entry.</span></span>

![Agregar nueva dirección](publish-to-azure/_static/image15.png)

<span data-ttu-id="93102-180">Guarde el cambio a las direcciones IP permitidas.</span><span class="sxs-lookup"><span data-stu-id="93102-180">Save the change to allowed IP addresses.</span></span>

<span data-ttu-id="93102-181">Haga clic en administrar y vuelva a conectarse a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="93102-181">Click Manage, and try logging in again to the database.</span></span> <span data-ttu-id="93102-182">Es posible que deba esperar unos minutos antes de que las direcciones IP permitidas están configuradas correctamente para el firewall.</span><span class="sxs-lookup"><span data-stu-id="93102-182">You may need to wait a few minutes before the allowed IP addresses are correctly configured for the firewall.</span></span> <span data-ttu-id="93102-183">Cuando puede iniciar sesión correctamente en la base de datos, se haya terminado de configurar la conexión a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="93102-183">When you can successfully log in the database, you have finished setting up your connection to the database.</span></span>

<span data-ttu-id="93102-184">Puede dejar abierta esta ventana de administración porque se comprobará el resultado de la implementación de la base de datos en breve.</span><span class="sxs-lookup"><span data-stu-id="93102-184">You can leave this management window open because you will check the result of your database deployment shortly.</span></span>

<span data-ttu-id="93102-185">Vuelva a su proyecto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="93102-185">Return to your database project.</span></span> <span data-ttu-id="93102-186">Haga clic en el proyecto y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="93102-186">Right-click the project and select **Publish**.</span></span>

![publicar base de datos](publish-to-azure/_static/image16.png)

<span data-ttu-id="93102-188">En la ventana de la base de datos de publicación, seleccione **editar**.</span><span class="sxs-lookup"><span data-stu-id="93102-188">In the Publish Database window, select **Edit**.</span></span>

![editar](publish-to-azure/_static/image17.png)

<span data-ttu-id="93102-190">Proporcione el nombre del servidor de base de datos y de las credenciales de autenticación para el servidor.</span><span class="sxs-lookup"><span data-stu-id="93102-190">Provide the name of the database server and your authentication credentials for the server.</span></span> <span data-ttu-id="93102-191">Después de proporcionar las credenciales, seleccione la base de datos que creó en la lista de bases de datos disponibles.</span><span class="sxs-lookup"><span data-stu-id="93102-191">After providing the credentials, select the database you created from the list of available databases.</span></span> <span data-ttu-id="93102-192">De forma predeterminada, Visual Studio establece el nombre del campo de base de datos en el nombre del proyecto que puede no ser la misma que la base de datos que creó.</span><span class="sxs-lookup"><span data-stu-id="93102-192">By default, Visual Studio sets the name of the database field to the name of your project which might not be the same as the database you created.</span></span>

![](publish-to-azure/_static/image18.png)

<span data-ttu-id="93102-193">Haga clic en Aceptar.</span><span class="sxs-lookup"><span data-stu-id="93102-193">Click OK.</span></span>

<span data-ttu-id="93102-194">Probablemente deseará guardar este perfil, por lo que puede publicar actualizaciones en el futuro sin volver a escribir toda la información de conexión.</span><span class="sxs-lookup"><span data-stu-id="93102-194">You will probably want to save this profile so you can publish updates in the future without re-entering all of the connection information.</span></span> <span data-ttu-id="93102-195">Seleccione **Crear perfil**.</span><span class="sxs-lookup"><span data-stu-id="93102-195">Select **Create Profile**.</span></span>

![Guardar perfil](publish-to-azure/_static/image19.png)

<span data-ttu-id="93102-197">Creará un archivo en el proyecto denominado **ContosoUniversityData.publish.xml**.</span><span class="sxs-lookup"><span data-stu-id="93102-197">It will create a file in your project named **ContosoUniversityData.publish.xml**.</span></span> <span data-ttu-id="93102-198">La próxima vez que desea publicar la base de datos en Azure, simplemente cargue dicho perfil.</span><span class="sxs-lookup"><span data-stu-id="93102-198">The next time you want to publish the database to Azure, simply load that profile.</span></span>

<span data-ttu-id="93102-199">Ahora, haga clic en **publicar** para crear la base de datos en Azure.</span><span class="sxs-lookup"><span data-stu-id="93102-199">Now, click **Publish** to create the database on Azure.</span></span>

![publicar](publish-to-azure/_static/image20.png)

<span data-ttu-id="93102-201">Después de ejecutarse durante un tiempo, se muestran los resultados de la publicación.</span><span class="sxs-lookup"><span data-stu-id="93102-201">After running for a while, the publishing results are displayed.</span></span>

![results](publish-to-azure/_static/image21.png)

<span data-ttu-id="93102-203">Ahora, puede volver al portal de administración para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="93102-203">Now, you can go back to the management portal for your database.</span></span> <span data-ttu-id="93102-204">Actualice la vista de diseño y observe que se han implementado las 3 tablas con datos previamente rellenadas.</span><span class="sxs-lookup"><span data-stu-id="93102-204">Refresh the design view, and notice the 3 tables with pre-filled data have been deployed.</span></span>

![nuevas tablas](publish-to-azure/_static/image22.png)

<span data-ttu-id="93102-206">Ahora está listo para probar la aplicación web que se implementa en Azure.</span><span class="sxs-lookup"><span data-stu-id="93102-206">Now you are ready to test the web app that is deployed to Azure.</span></span> <span data-ttu-id="93102-207">Vaya a la aplicación web en Azure (como http://contosositeexample.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="93102-207">Navigate to the web app on Azure (such as http://contosositeexample.azurewebsites.net/).</span></span> <span data-ttu-id="93102-208">Haga clic en el vínculo para obtener la lista de alumnos y debería ver la vista de índice para estudiantes.</span><span class="sxs-lookup"><span data-stu-id="93102-208">Click the link for List of students and you should see the index view for students.</span></span>

![vista](publish-to-azure/_static/image23.png)

<span data-ttu-id="93102-210">En ocasiones, la base de datos y la conexión necesitan un poco de tiempo para configurarse correctamente.</span><span class="sxs-lookup"><span data-stu-id="93102-210">Occasionally, the database and connection need a little time to be properly configured.</span></span> <span data-ttu-id="93102-211">Si recibe un error, espere unos minutos y, a continuación, vuelva a intentarlo.</span><span class="sxs-lookup"><span data-stu-id="93102-211">If you receive an error, wait a few minutes and then try again.</span></span>

## <a name="conclusion"></a><span data-ttu-id="93102-212">Conclusión</span><span class="sxs-lookup"><span data-stu-id="93102-212">Conclusion</span></span>

<span data-ttu-id="93102-213">Esta serie proporciona un ejemplo sencillo de cómo generar código a partir de una base de datos existente que permite a los usuarios editar, actualizar, crear y eliminar datos.</span><span class="sxs-lookup"><span data-stu-id="93102-213">This series provided a simple example of how to generate code from an existing database that enables users to edit, update, create and delete data.</span></span> <span data-ttu-id="93102-214">ASP.NET MVC 5, Entity Framework y ASP.NET Scaffolding usan para crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="93102-214">It used ASP.NET MVC 5, Entity Framework and ASP.NET Scaffolding to create the project.</span></span>

<span data-ttu-id="93102-215">Para obtener un ejemplo introductorio de desarrollo Code First, consulte [Introducción a ASP.NET MVC 5](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="93102-215">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span>

<span data-ttu-id="93102-216">Para obtener un ejemplo más avanzado, consulte [crear un modelo de datos de Entity Framework para una aplicación de ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="93102-216">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="93102-217">Tenga en cuenta que la API de DbContext que usan para trabajar con datos en la primera base de datos es igual a la API que se utiliza para trabajar con datos en Code First.</span><span class="sxs-lookup"><span data-stu-id="93102-217">Note that the DbContext API that you use for working with data in Database First is the same as the API you use for working with data in Code First.</span></span> <span data-ttu-id="93102-218">Incluso si piensa usar la primera base de datos, puede aprender a administrar los escenarios más complejos, como leer y actualizar datos relacionados, controlar los conflictos de simultaneidad, y así sucesivamente de un tutorial de Code First.</span><span class="sxs-lookup"><span data-stu-id="93102-218">Even if you intend to use Database First, you can learn how to handle more complex scenarios such as reading and updating related data, handling concurrency conflicts, and so forth from a Code First tutorial.</span></span> <span data-ttu-id="93102-219">La única diferencia está en cómo se crean la base de datos, clase de contexto y clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="93102-219">The only difference is in how the database, context class, and entity classes are created.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="93102-220">Anterior</span><span class="sxs-lookup"><span data-stu-id="93102-220">Previous</span></span>](enhancing-data-validation.md)
