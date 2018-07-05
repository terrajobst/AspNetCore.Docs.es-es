---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Implementación Web de ASP.NET con Visual Studio: implementar una actualización de código | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar (publicar) una ASP.NET web application a Azure App Service Web Apps o a un proveedor de hospedaje de terceros, usa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 3649f0250e830b76c42d832e51038c2c52ed4561
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368219"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a><span data-ttu-id="f8fa1-103">Implementación Web de ASP.NET con Visual Studio: implementar una actualización de código</span><span class="sxs-lookup"><span data-stu-id="f8fa1-103">ASP.NET Web Deployment using Visual Studio: Deploying a Code Update</span></span>
====================
<span data-ttu-id="f8fa1-104">por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f8fa1-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="f8fa1-105">Descargar el proyecto de inicio</span><span class="sxs-lookup"><span data-stu-id="f8fa1-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="f8fa1-106">Esta serie de tutoriales muestra cómo implementar (publicar) una ASP.NET web application a Azure App Service Web Apps o a un proveedor de hospedaje de terceros, mediante el uso de Visual Studio 2012 o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="f8fa1-107">Para obtener información acerca de la serie, vea [el primer tutorial de la serie](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f8fa1-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="f8fa1-108">Información general</span><span class="sxs-lookup"><span data-stu-id="f8fa1-108">Overview</span></span>

<span data-ttu-id="f8fa1-109">Tras la implementación inicial, continúa el trabajo de mantenimiento y desarrollo de su sitio web y, en poco tiempo desea implementar una actualización.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-109">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="f8fa1-110">Este tutorial le guiará por el proceso de implementación de una actualización en el código de aplicación.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-110">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="f8fa1-111">La actualización que implementar en este tutorial no implica un cambio de base de datos; podrá ver cuál es la diferencia acerca de cómo implementar un cambio de base de datos en el siguiente tutorial.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-111">The update that you implement and deploy in this tutorial does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="f8fa1-112">Recordatorio: Si recibe un mensaje de error o algo no funciona conforme avance en el tutorial, no olvide consultar la [página de solución](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="f8fa1-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="make-a-code-change"></a><span data-ttu-id="f8fa1-113">Realizar un código de cambio</span><span class="sxs-lookup"><span data-stu-id="f8fa1-113">Make a code change</span></span>

<span data-ttu-id="f8fa1-114">Como ejemplo sencillo de una actualización a la aplicación, agregará el **instructores** página una lista de cursos impartidos por el instructor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-114">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="f8fa1-115">Si ejecuta el **instructores** página, observará que hay **seleccione** vínculos en la cuadrícula, pero no es algo distinto de make el color gris a su vez de fila en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-115">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

![Página de instructores con selección](deploying-a-code-update/_static/image1.png)

<span data-ttu-id="f8fa1-117">Ahora agregará código que se ejecuta cuando el **seleccione** vínculo está seleccionado y muestra una lista de cursos impartidos por el instructor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-117">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

1. <span data-ttu-id="f8fa1-118">En *Instructors.aspx*, agregue el marcado siguiente inmediatamente después de la **ErrorMessageLabel** `Label` control:</span><span class="sxs-lookup"><span data-stu-id="f8fa1-118">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. <span data-ttu-id="f8fa1-119">Ejecute la página y seleccione un instructor.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-119">Run the page and select an instructor.</span></span> <span data-ttu-id="f8fa1-120">Vea una lista de cursos impartidos por ese instructor.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-120">You see a list of courses taught by that instructor.</span></span>

    ![Página de instructores con cursos impartidos](deploying-a-code-update/_static/image2.png)
3. <span data-ttu-id="f8fa1-122">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-122">Close the browser.</span></span>

## <a name="deploy-the-code-update-to-the-test-environment"></a><span data-ttu-id="f8fa1-123">Implementar la actualización del código en el entorno de prueba</span><span class="sxs-lookup"><span data-stu-id="f8fa1-123">Deploy the code update to the test environment</span></span>

<span data-ttu-id="f8fa1-124">Para poder usar los perfiles de publicación para implementar en la prueba, ensayo y producción, deberá cambiar las opciones de publicación de base de datos.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-124">Before you can use your publish profiles to deploy to test, staging, and production, you need to change database publishing options.</span></span> <span data-ttu-id="f8fa1-125">Ya no necesita ejecutar los scripts de implementación de concesión y los datos para la base de datos de pertenencia.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-125">You no longer need to run the grant and data deployment scripts for the membership database.</span></span>

1. <span data-ttu-id="f8fa1-126">Abra el **publicación Web** Asistente haciendo clic en el proyecto ContosoUniversity y haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-126">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="f8fa1-127">Haga clic en el **prueba** de perfil en el **perfil** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-127">Click the **Test** profile in the **Profile** drop-down list.</span></span>
3. <span data-ttu-id="f8fa1-128">Haga clic en el **configuración** ficha.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-128">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="f8fa1-129">En **DefaultConnection** en el **bases de datos** sección, desactive la **Actualizar base de datos** casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-129">Under **DefaultConnection** in the **Databases** section, clear the **Update database** check box.</span></span>
5. <span data-ttu-id="f8fa1-130">Haga clic en el **perfil** pestaña y, a continuación, haga clic en el **ensayo** de perfil en el **perfil** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-130">Click the **Profile** tab, and then click the **Staging** profile in the **Profile** drop-down list.</span></span>
6. <span data-ttu-id="f8fa1-131">Cuando se le pregunte si desea guardar los cambios realizados en el **prueba** de perfil, haga clic en **Sí**.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-131">When you are asked if you want to save the changes made to the **Test** profile, click **Yes**.</span></span>
7. <span data-ttu-id="f8fa1-132">Realizar el mismo cambio en el perfil de almacenamiento provisional.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-132">Make the same change in the Staging profile.</span></span>
8. <span data-ttu-id="f8fa1-133">Repita el proceso para realizar el mismo cambio en el perfil de producción.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-133">Repeat the process to make the same change in the Production profile.</span></span>
9. <span data-ttu-id="f8fa1-134">Cerrar la **publicación Web** asistente.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-134">Close the **Publish Web** wizard.</span></span>

<span data-ttu-id="f8fa1-135">Implementación en el entorno de prueba es ahora tan sencillo ejecutar con un solo clic, vuelva a publicar.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-135">Deploying to the test environment is now a simple matter of running one-click publish again.</span></span> <span data-ttu-id="f8fa1-136">Para agilizar este proceso, puede usar el **una publicación en Web clic** barra de herramientas.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-136">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

1. <span data-ttu-id="f8fa1-137">En el **vista** menú, elija **las barras de herramientas** y, a continuación, seleccione **una publicación en Web clic**.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-137">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. <span data-ttu-id="f8fa1-139">En **el Explorador de soluciones**, seleccione el proyecto ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-139">In **Solution Explorer**, select the ContosoUniversity project.</span></span>
3. <span data-ttu-id="f8fa1-140">el **una publicación en Web clic** barra de herramientas, elija la **prueba** perfil de publicación y, a continuación, haga clic en **publicación Web** (el icono con flechas que señalan al left y right).</span><span class="sxs-lookup"><span data-stu-id="f8fa1-140">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. <span data-ttu-id="f8fa1-142">Visual Studio implementa la aplicación actualizada, y el explorador se abre automáticamente en la página principal.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-142">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span>
5. <span data-ttu-id="f8fa1-143">Ejecute la página de instructores y seleccione un instructor para comprobar que la actualización se ha implementado correctamente.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-143">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="f8fa1-144">Lo haría normalmente también hacer las pruebas de regresión (es decir, el resto del sitio para asegurarse de que el nuevo cambio no interrumpa ninguna funcionalidad existente de prueba).</span><span class="sxs-lookup"><span data-stu-id="f8fa1-144">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="f8fa1-145">Pero para este tutorial podrá omitir ese paso y continuar para implementar la actualización en ensayo y producción.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-145">But for this tutorial you'll skip that step and proceed to deploy the update to staging and production.</span></span>

<span data-ttu-id="f8fa1-146">Al implementar de nuevo, Web Deploy determina automáticamente qué archivos han cambiado y copias de solo archivos cambiados en el servidor.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-146">When you redeploy, Web Deploy automatically determines which files have changed and only copies changed files to the server.</span></span> <span data-ttu-id="f8fa1-147">De forma predeterminada, Web Deploy usa fechas último cambio en archivos para determinar cuáles se han cambiado.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-147">By default, Web Deploy uses last-changed dates on files to determine which ones have changed.</span></span> <span data-ttu-id="f8fa1-148">Algunos sistemas de control de código fuente cambian archivo fechas incluso cuando no cambie el contenido del archivo.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-148">Some source control systems change file dates even when you don't change the file contents.</span></span> <span data-ttu-id="f8fa1-149">En ese caso, es posible que desee configurar Web Deploy para usar sumas de comprobación para determinar qué archivos han cambiado.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-149">In that case, you might want to configure Web Deploy to use file checksums to determine which files have changed.</span></span> <span data-ttu-id="f8fa1-150">Para obtener más información, consulte [¿por qué todos mis archivos se vuelven a implementar aunque no cambiarlos?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) en las preguntas más frecuentes de implementación de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-150">For more information, see [Why do all of my files get redeployed although I didn't change them?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in the ASP.NET Deployment FAQ.</span></span>

## <a name="take-the-application-offline-during-deployment"></a><span data-ttu-id="f8fa1-151">Desactivar la aplicación sin conexión durante la implementación</span><span class="sxs-lookup"><span data-stu-id="f8fa1-151">Take the application offline during deployment</span></span>

<span data-ttu-id="f8fa1-152">El cambio que va a implementar ahora es un simple cambio en una sola página.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-152">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="f8fa1-153">Pero a veces de implementar cambios mayores, o implementar los cambios de código y de base de datos y el sitio es posible que se comporte incorrectamente si un usuario solicita una página antes de que finalice la implementación.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-153">But sometimes you deploy larger changes, or you deploy both code and database changes, and the site might behave incorrectly if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="f8fa1-154">Para evitar que los usuarios acceso al sitio mientras la implementación está en curso, puede usar un *aplicación\_offline.htm* archivo.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-154">To prevent users from accessing the site while deployment is in progress, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="f8fa1-155">Cuando coloca un archivo denominado *aplicación\_offline.htm* en la carpeta raíz de la aplicación, IIS muestra automáticamente ese archivo en lugar de ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-155">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="f8fa1-156">Por lo que colocar para evitar el acceso durante la implementación, *aplicación\_offline.htm* en la carpeta raíz, ejecute el proceso de implementación y, a continuación, quite *aplicación\_offline.htm* después correcta implementación.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-156">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm* after successful deployment.</span></span>

<span data-ttu-id="f8fa1-157">Puede configurar Web Deploy para colocar automáticamente el valor predeterminado es *aplicación\_offline.htm* en el servidor de archivos cuando se inicia la implementación y quitarlo cuando haya terminado.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-157">You can configure Web Deploy to automatically put a default *app\_offline.htm* file on the server when it starts deploying and remove it when it finishes.</span></span> <span data-ttu-id="f8fa1-158">Para hacer todo lo que debe hacer es agregar el siguiente elemento XML a su archivo de perfil (.pubxml) de la publicación:</span><span class="sxs-lookup"><span data-stu-id="f8fa1-158">To do that all you have to do is add the following XML element to your publish profile (.pubxml) file:</span></span>

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

<span data-ttu-id="f8fa1-159">En este tutorial verá cómo crear y utilizar una personalizada *aplicación\_offline.htm* archivo.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-159">For this tutorial you'll see how to create and use a custom *app\_offline.htm* file.</span></span>

<span data-ttu-id="f8fa1-160">Uso de *aplicación\_offline.htm* en el sitio de ensayo no es necesario, porque no tiene acceso al sitio de ensayo de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-160">Using *app\_offline.htm* in the staging site isn't required, because you don't have users accessing the staging site.</span></span> <span data-ttu-id="f8fa1-161">Pero es una buena práctica para usar el almacenamiento provisional para probar todo lo que se va a implementar en producción.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-161">But it's a good practice to use staging to test everything the way you plan to deploy in production.</span></span>

### <a name="create-appofflinehtm"></a><span data-ttu-id="f8fa1-162">Crear aplicación\_offline.htm</span><span class="sxs-lookup"><span data-stu-id="f8fa1-162">Create app\_offline.htm</span></span>

1. <span data-ttu-id="f8fa1-163">En **el Explorador de soluciones**, haga clic en la solución y haga clic en **agregar**y, a continuación, haga clic en **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-163">In **Solution Explorer**, right-click the solution and click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="f8fa1-164">Crear un **página HTML** denominado *aplicación\_offline.htm* (eliminar el último "l" en el *.html* extensión de Visual Studio crea de forma predeterminada).</span><span class="sxs-lookup"><span data-stu-id="f8fa1-164">Create an **HTML Page** named *app\_offline.htm* (delete the final "l" in the *.html* extension that Visual Studio creates by default).</span></span>
3. <span data-ttu-id="f8fa1-165">Reemplace el marcado de plantilla con el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="f8fa1-165">Replace the template markup with the following markup:</span></span>

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. <span data-ttu-id="f8fa1-166">Guarde y cierre el archivo.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-166">Save and close the file.</span></span>

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a><span data-ttu-id="f8fa1-167">Copiar la aplicación\_offline.htm a la carpeta raíz del sitio web</span><span class="sxs-lookup"><span data-stu-id="f8fa1-167">Copy app\_offline.htm to the root folder of the web site</span></span>

<span data-ttu-id="f8fa1-168">Puede usar cualquier herramienta FTP para copiar archivos en el sitio web.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-168">You can use any FTP tool to copy files to the web site.</span></span> <span data-ttu-id="f8fa1-169">[FileZilla](http://filezilla-project.org/) es una popular herramienta FTP y se muestra en las capturas de pantalla.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-169">[FileZilla](http://filezilla-project.org/) is a popular FTP tool and is shown in the screen shots.</span></span>

<span data-ttu-id="f8fa1-170">Para usar una herramienta FTP, necesita tres cosas: la dirección URL de FTP, el nombre de usuario y la contraseña.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-170">To use an FTP tool, you need three things: the FTP URL, the user name, and the password.</span></span>

<span data-ttu-id="f8fa1-171">La dirección URL se muestra en la página del panel del sitio web en el Portal de administración de Azure y el nombre de usuario y contraseña de FTP pueden encontrarse en el *.publishsettings* archivo que descargó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-171">The URL is shown on the web site's dashboard page in the Azure Management Portal, and the user name and password for FTP can be found in the *.publishsettings* file that you downloaded earlier.</span></span> <span data-ttu-id="f8fa1-172">Los pasos siguientes muestran cómo obtener estos valores.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-172">The following steps show how to get these values.</span></span>

1. <span data-ttu-id="f8fa1-173">En el Portal de administración de Azure, haga clic en **sitios Web** pestaña y, a continuación, haga clic en el sitio web de ensayo.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-173">In the Azure Management Portal, click **Web Sites** tab and then click the staging web site.</span></span>
2. <span data-ttu-id="f8fa1-174">En el **panel** página, desplácese hacia abajo hasta encontrar el nombre de host FTP en el **vista rápida** sección.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-174">On the **Dashboard** page, scroll down to find the FTP host name in the **Quick Glance** section.</span></span>

    ![Nombre de host FTP](deploying-a-code-update/_static/image5.png)
3. <span data-ttu-id="f8fa1-176">Abra el almacenamiento provisional *.publishsettings* archivo en el Bloc de notas u otro editor de texto.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-176">Open the staging *.publishsettings* file in Notepad or another text editor.</span></span>
4. <span data-ttu-id="f8fa1-177">Buscar el `publishProfile` (elemento) para el perfil de FTP.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-177">Find the `publishProfile` element for the FTP profile.</span></span>
5. <span data-ttu-id="f8fa1-178">Copia el `userName` y `userPWD` valores.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-178">Copy the `userName` and `userPWD` values.</span></span>

    ![Credenciales de FTP](deploying-a-code-update/_static/image6.png)
6. <span data-ttu-id="f8fa1-180">Abra la herramienta FTP e iniciar sesión en la dirección URL de FTP.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-180">Open your FTP tool and log on to the FTP URL.</span></span>
7. <span data-ttu-id="f8fa1-181">Copia *aplicación\_offline.htm* desde la carpeta de soluciones para la */site/wwwroot* carpeta en el sitio de ensayo.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-181">Copy *app\_offline.htm* from the solution folder to the */site/wwwroot* folder in the staging site.</span></span>

    ![Copiar app_offline](deploying-a-code-update/_static/image7.png)
8. <span data-ttu-id="f8fa1-183">Vaya a la dirección URL de su sitio de ensayo.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-183">Browse to your staging site's URL.</span></span> <span data-ttu-id="f8fa1-184">Verá que el *aplicación\_offline.htm* página aparece ahora en lugar de la página principal.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-184">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

    ![App_offline.htm en la ventana del explorador](deploying-a-code-update/_static/image8.png)

<span data-ttu-id="f8fa1-186">Ahora está listo para implementar en ensayo.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-186">You are now ready to deploy to staging.</span></span>

## <a name="deploy-the-code-update-to-staging-and-production"></a><span data-ttu-id="f8fa1-187">Implementar la actualización del código en ensayo y producción</span><span class="sxs-lookup"><span data-stu-id="f8fa1-187">Deploy the code update to staging and production</span></span>

1. <span data-ttu-id="f8fa1-188">En el **una publicación en Web clic** barra de herramientas, elija la **ensayo** perfil de publicación y, a continuación, haga clic en **publicación Web**.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-188">In the **Web One Click Publish** toolbar, choose the **Staging** publish profile and then click **Publish Web**.</span></span>

    <span data-ttu-id="f8fa1-189">Visual Studio implementa la aplicación actualizada y abre el explorador a la página principal del sitio.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-189">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="f8fa1-190">El *aplicación\_offline.htm* se muestra el archivo.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-190">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="f8fa1-191">Antes de poder probar para comprobar la implementación correcta, debe quitar la *aplicación\_offline.htm* archivo.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-191">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>
2. <span data-ttu-id="f8fa1-192">Volver a la herramienta FTP y eliminar **aplicación\_offline.htm** desde el sitio de ensayo.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-192">Return to your FTP tool, and delete **app\_offline.htm** from the staging site.</span></span>
3. <span data-ttu-id="f8fa1-193">En el explorador, abra la página de instructores en el sitio de ensayo y seleccione un instructor para comprobar que la actualización se ha implementado correctamente.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-193">In the browser, open the Instructors page in the staging site, and select an instructor to verify that the update was successfully deployed.</span></span>
4. <span data-ttu-id="f8fa1-194">Siga el mismo procedimiento para la producción como hizo el almacenamiento provisional.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-194">Follow the same procedure for production as you did for staging.</span></span>

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a><span data-ttu-id="f8fa1-195">Revisar los cambios y archivos específicos de implementación</span><span class="sxs-lookup"><span data-stu-id="f8fa1-195">Reviewing Changes and Deploying Specific Files</span></span>

<span data-ttu-id="f8fa1-196">Visual Studio 2012 también le ofrece la posibilidad de implementar los archivos individuales.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-196">Visual Studio 2012 also gives you the ability to deploy individual files.</span></span> <span data-ttu-id="f8fa1-197">Para un archivo seleccionado puede ver las diferencias entre la versión local y la versión implementada, implemente el archivo en el entorno de destino o copie el archivo desde el entorno de destino en el proyecto local.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-197">For a selected file you can view differences between the local version and the deployed version, deploy the file to the destination environment, or copy the file from the destination environment to the local project.</span></span> <span data-ttu-id="f8fa1-198">En esta sección del tutorial verá cómo usar estas características.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-198">In this section of the tutorial you see how to use these features.</span></span>

### <a name="make-a-change-to-deploy"></a><span data-ttu-id="f8fa1-199">Realiza un cambio en la implementación</span><span class="sxs-lookup"><span data-stu-id="f8fa1-199">Make a change to deploy</span></span>

1. <span data-ttu-id="f8fa1-200">Abra *Content/Site.css*y busque el bloque para el `body` etiqueta.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-200">Open *Content/Site.css*, and find the block for the `body` tag.</span></span>
2. <span data-ttu-id="f8fa1-201">Cambie el valor de `background-color` desde `#fff` a `darkblue`.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-201">Change the value for `background-color` from `#fff` to `darkblue`.</span></span>

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a><span data-ttu-id="f8fa1-202">Ver el cambio en la ventana de vista previa de publicación</span><span class="sxs-lookup"><span data-stu-id="f8fa1-202">View the change in the Publish Preview window</span></span>

<span data-ttu-id="f8fa1-203">Cuando se usa el **publicación Web** Asistente para publicar el proyecto, puede ver los cambios que se van a publicarse haciendo doble clic en el archivo en el **Preview** ventana.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-203">When you use the **Publish Web** wizard to publish the project, you can see what changes are going to be published by double-clicking the file in the **Preview** window.</span></span>

1. <span data-ttu-id="f8fa1-204">Haga clic en el proyecto ContosoUniversity y haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-204">Right-click the ContosoUniversity project and click **Publish**.</span></span>
2. <span data-ttu-id="f8fa1-205">Desde el **perfil** lista desplegable, seleccione el **prueba** perfil de publicación.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-205">From the **Profile** drop-down list, select the **Test** publish profile.</span></span>
3. <span data-ttu-id="f8fa1-206">Haga clic en **Preview**y, a continuación, haga clic en **iniciar vista previa**.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-206">Click **Preview**, and then click **Start Preview**.</span></span>
4. <span data-ttu-id="f8fa1-207">En el **Preview** panel, haga doble clic en **Site.css**.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-207">In the **Preview** pane, double-click **Site.css**.</span></span>

    ![Haga doble clic en Site.css](deploying-a-code-update/_static/image9.png)

    <span data-ttu-id="f8fa1-209">El **vista previa de cambios** cuadro de diálogo muestra una vista previa de los cambios que se va a implementar.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-209">The **Preview changes** dialog shows a preview of the changes that will be deployed.</span></span>

    ![Vista previa de cambios para Site.css](deploying-a-code-update/_static/image10.png)

    <span data-ttu-id="f8fa1-211">Si hace doble clic en el *Web.config* archivo, el **vista previa de cambios** cuadro de diálogo se muestra el efecto de la compilación de las transformaciones de configuración y las transformaciones de perfil de publicación.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-211">If you double-click the *Web.config* file, the **Preview changes** dialog shows the effect of your build configuration transformations and publish profile transformations.</span></span> <span data-ttu-id="f8fa1-212">En este momento no lo haga todo lo que provocaría la *Web.config* archivo en el servidor para cambiar, por lo que espera no ver ningún cambio.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-212">At this point you have not done anything that would cause the *Web.config* file on the server to change, so you expect to see no changes.</span></span> <span data-ttu-id="f8fa1-213">Sin embargo, el **vista previa de cambios** ventana incorrectamente muestra dos cambios.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-213">However, the **Preview changes** window incorrectly shows two changes.</span></span> <span data-ttu-id="f8fa1-214">Parece que se quitarán los dos elementos XML.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-214">It looks like two XML elements will be removed.</span></span> <span data-ttu-id="f8fa1-215">Estos elementos se agregan mediante el proceso de publicación cuando se selecciona **ejecutar migraciones de Code First al iniciar la aplicación** para una clase de contexto de Code First.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-215">These elements are added by the publish process when you select **Execute Code First Migrations on application start** for a Code First context class.</span></span> <span data-ttu-id="f8fa1-216">La comparación se realiza antes de que el proceso de publicación agrega esos elementos, para que parezca que se quitan aunque no se quitará.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-216">The comparison is done before the publish process adds those elements, so it looks like they are being removed although they will not be removed.</span></span> <span data-ttu-id="f8fa1-217">Este error se corregirá en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-217">This error will be corrected in a future release.</span></span>
5. <span data-ttu-id="f8fa1-218">Haga clic en **Cerrar**.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-218">Click **Close**.</span></span>
6. <span data-ttu-id="f8fa1-219">Haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-219">Click **Publish**.</span></span>
7. <span data-ttu-id="f8fa1-220">Cuando el explorador se abre en la página principal del sitio de prueba, presione CTRL + F5 para hacer que una actualización de disco dura con el fin de ver el efecto del cambio CSS.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-220">When the browser opens to the home page of the Test site, press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Efecto del cambio CSS](deploying-a-code-update/_static/image11.png)
8. <span data-ttu-id="f8fa1-222">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-222">Close the browser.</span></span>

### <a name="publish-specific-files-from-solution-explorer"></a><span data-ttu-id="f8fa1-223">Publicar archivos concretos desde el Explorador de soluciones</span><span class="sxs-lookup"><span data-stu-id="f8fa1-223">Publish specific files from Solution Explorer</span></span>

<span data-ttu-id="f8fa1-224">Supongamos que no quiere revertir al color original y, como el fondo azul.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-224">Suppose you don't like the blue background and want to revert to the original color.</span></span> <span data-ttu-id="f8fa1-225">En esta sección, podrá restaurar la configuración original mediante la publicación de un archivo específico directamente desde **el Explorador de soluciones**.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-225">In this section you'll restore the original settings by publishing a specific file directly from **Solution Explorer**.</span></span>

1. <span data-ttu-id="f8fa1-226">Abra *Content/Site.css* y restaurar el `background-color` si se establece en `#fff`.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-226">Open *Content/Site.css* and restore the `background-color` setting to `#fff`.</span></span>
2. <span data-ttu-id="f8fa1-227">En **el Explorador de soluciones**, haga clic en el *Content/Site.css* archivo.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-227">In **Solution Explorer**, right-click the *Content/Site.css* file.</span></span>

    <span data-ttu-id="f8fa1-228">El menú contextual muestra que tres opciones de publicación.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-228">The context menu shows three publish options.</span></span>

    ![Publicar opciones desde el Explorador de soluciones](deploying-a-code-update/_static/image12.png)
3. <span data-ttu-id="f8fa1-230">Haga clic en **cambios de vista previa para Site.css**.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-230">Click **Preview changes to Site.css**.</span></span>

    <span data-ttu-id="f8fa1-231">Se abre una ventana para mostrar las diferencias entre el archivo local y la versión del mismo en el entorno de destino.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-231">A window opens to show the differences between the local file and the version of it in the destination environment.</span></span>

    ![Diff/Site.css-contenido](deploying-a-code-update/_static/image13.png)
4. <span data-ttu-id="f8fa1-233">En **el Explorador de soluciones**, haga clic en **Site.css** nuevo y haga clic en **publicar Site.css**.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-233">In **Solution Explorer**, right-click **Site.css** again and click **Publish Site.css**.</span></span>

    <span data-ttu-id="f8fa1-234">El **actividad de publicación Web** ventana muestra que se ha publicado el archivo.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-234">The **Web Publish Activity** window shows that the file has been published.</span></span>

    ![Ventana de actividad de publicación Web](deploying-a-code-update/_static/image14.png)
5. <span data-ttu-id="f8fa1-236">Abra un explorador en la `http://localhost/contosouniversity` dirección URL y, a continuación, presione CTRL + F5 para hacer que un disco duro actualizar para ver el efecto de CSS cambiar.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-236">Open a browser to the `http://localhost/contosouniversity` URL, and then press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Página principal con CSS normal](deploying-a-code-update/_static/image15.png)
6. <span data-ttu-id="f8fa1-238">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-238">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="f8fa1-239">Resumen</span><span class="sxs-lookup"><span data-stu-id="f8fa1-239">Summary</span></span>

<span data-ttu-id="f8fa1-240">Ahora ha visto varias maneras de implementar una actualización de la aplicación que no implica un cambio de base de datos y ha visto cómo obtener una vista previa de los cambios para comprobar que se actualizarán a lo que es lo que espera.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-240">You've now seen several ways to deploy an application update that does not involve a database change, and you've seen how to preview the changes to verify that what will be updated is what you expect.</span></span> <span data-ttu-id="f8fa1-241">La página de instructores ahora tiene un **cursos impartidos** sección.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-241">The Instructors page now has a **Courses Taught** section.</span></span>

![Página de instructores con cursos impartidos](deploying-a-code-update/_static/image16.png)

<span data-ttu-id="f8fa1-243">El siguiente tutorial muestra cómo implementar un cambio de base de datos: se agregará un campo de fecha de nacimiento en la base de datos y a la página de instructores.</span><span class="sxs-lookup"><span data-stu-id="f8fa1-243">The next tutorial shows you how to deploy a database change: you'll add a birthdate field to the database and to the Instructors page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f8fa1-244">[Anterior](deploying-to-production.md)
> [Siguiente](deploying-a-database-update.md)</span><span class="sxs-lookup"><span data-stu-id="f8fa1-244">[Previous](deploying-to-production.md)
[Next](deploying-a-database-update.md)</span></span>
