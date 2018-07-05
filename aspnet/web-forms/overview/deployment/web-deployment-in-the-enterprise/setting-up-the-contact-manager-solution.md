---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Configuración de la solución Contact Manager | Microsoft Docs
author: jrjlee
description: Este tema describe cómo descargar y configurar la solución Contact Manager para ejecutarse localmente en una estación de trabajo de desarrollador.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: a7675a793909ec4d95164ee47a3a43f73600c5bc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366694"
---
<a name="setting-up-the-contact-manager-solution"></a><span data-ttu-id="07064-103">Configuración de la solución Contact Manager</span><span class="sxs-lookup"><span data-stu-id="07064-103">Setting Up the Contact Manager Solution</span></span>
====================
<span data-ttu-id="07064-104">por [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="07064-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="07064-105">Descargar PDF</span><span class="sxs-lookup"><span data-stu-id="07064-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="07064-106">Este tema describe cómo descargar y configurar la solución Contact Manager para ejecutarse localmente en una estación de trabajo de desarrollador.</span><span class="sxs-lookup"><span data-stu-id="07064-106">This topic describes how to download and configure the Contact Manager solution to run locally on a developer workstation.</span></span>


## <a name="system-requirements"></a><span data-ttu-id="07064-107">Requisitos del sistema</span><span class="sxs-lookup"><span data-stu-id="07064-107">System Requirements</span></span>

<span data-ttu-id="07064-108">Para ejecutar la solución Contact Manager localmente y realizar otras tareas que se describen en este tutorial, deberá instalar este software en su estación de trabajo del desarrollador:</span><span class="sxs-lookup"><span data-stu-id="07064-108">To run the Contact Manager solution locally and to perform the other tasks described in this tutorial, you'll need to install this software on your developer workstation:</span></span>

- <span data-ttu-id="07064-109">Visual Studio 2010 Service Pack 1, Premium o Ultimate Edition</span><span class="sxs-lookup"><span data-stu-id="07064-109">Visual Studio 2010 Service Pack 1, Premium or Ultimate Edition</span></span>
- <span data-ttu-id="07064-110">Internet Information Services (IIS) 7.5 Express</span><span class="sxs-lookup"><span data-stu-id="07064-110">Internet Information Services (IIS) 7.5 Express</span></span>
- <span data-ttu-id="07064-111">SQL Server Express 2008 R2</span><span class="sxs-lookup"><span data-stu-id="07064-111">SQL Server Express 2008 R2</span></span>
- <span data-ttu-id="07064-112">IIS herramienta de implementación Web (Web Deploy) 2.1 o posterior</span><span class="sxs-lookup"><span data-stu-id="07064-112">IIS Web Deployment Tool (Web Deploy) 2.1 or later</span></span>
- <span data-ttu-id="07064-113">ASP.NET 4.0</span><span class="sxs-lookup"><span data-stu-id="07064-113">ASP.NET 4.0</span></span>
- <span data-ttu-id="07064-114">ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="07064-114">ASP.NET MVC 3</span></span>
- <span data-ttu-id="07064-115">.NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="07064-115">.NET Framework 4</span></span>
- <span data-ttu-id="07064-116">.NET Framework 3.5 SP1</span><span class="sxs-lookup"><span data-stu-id="07064-116">.NET Framework 3.5 SP1</span></span>

<span data-ttu-id="07064-117">Con la excepción de Visual Studio 2010, puede descargar e instalar las versiones más recientes de todos estos productos y componentes a través de la [instalador de plataforma Web](https://go.microsoft.com/?linkid=9805118).</span><span class="sxs-lookup"><span data-stu-id="07064-117">With the exception of Visual Studio 2010, you can download and install the latest versions of all of these products and components through the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>

## <a name="download-and-extract-the-solution"></a><span data-ttu-id="07064-118">Descargue y extraiga la solución</span><span class="sxs-lookup"><span data-stu-id="07064-118">Download and Extract the Solution</span></span>

<span data-ttu-id="07064-119">Puede descargar la aplicación de ejemplo de Contact Manager desde la Galería de código de MSDN [aquí](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span><span class="sxs-lookup"><span data-stu-id="07064-119">You can download the Contact Manager sample application from the MSDN Code Gallery [here](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span></span>

## <a name="configure-and-run-the-solution"></a><span data-ttu-id="07064-120">Configurar y ejecutar la solución</span><span class="sxs-lookup"><span data-stu-id="07064-120">Configure and Run the Solution</span></span>

<span data-ttu-id="07064-121">Para configurar y ejecutar la solución Contact Manager en el equipo local, necesita realizar estos pasos generales:</span><span class="sxs-lookup"><span data-stu-id="07064-121">To configure and run the Contact Manager solution on your local machine, you'll need to perform these high-level steps:</span></span>

1. <span data-ttu-id="07064-122">Si no tiene uno ya, cree una base de datos de servicios de aplicación ASP.NET local con las características de administración de pertenencia y funciones habilitadas.</span><span class="sxs-lookup"><span data-stu-id="07064-122">If you don't have one already, create a local ASP.NET application services database with the membership and role management features enabled.</span></span>
2. <span data-ttu-id="07064-123">Editar las cadenas de conexión en el *web.config* archivos para que apunte a la instancia local de SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="07064-123">Edit connection strings in the *web.config* files to point to your local SQL Server Express instance.</span></span>
3. <span data-ttu-id="07064-124">Ejecutar la solución de Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="07064-124">Run the solution from Visual Studio 2010.</span></span>

<span data-ttu-id="07064-125">El resto de esta sección proporciona más información sobre cómo completar estas tareas.</span><span class="sxs-lookup"><span data-stu-id="07064-125">The remainder of this section provides more guidance on how to complete each of these tasks.</span></span>

<span data-ttu-id="07064-126">**Para crear la base de datos de servicios de aplicación**</span><span class="sxs-lookup"><span data-stu-id="07064-126">**To create the application services database**</span></span>

1. <span data-ttu-id="07064-127">Abra un símbolo del sistema de Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="07064-127">Open a Visual Studio 2010 command prompt.</span></span> <span data-ttu-id="07064-128">Para ello, en el **iniciar** menú, elija **todos los programas**, haga clic en **Microsoft Visual Studio 2010**, haga clic en **Visual Studio Tools**y, a continuación, Haga clic en **Visual Studio Command Prompt (2010)**.</span><span class="sxs-lookup"><span data-stu-id="07064-128">To do this, on the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, click **Visual Studio Tools**, and then click **Visual Studio Command Prompt (2010)**.</span></span>
2. <span data-ttu-id="07064-129">En el símbolo del sistema, escriba el siguiente comando y, a continuación, presione ENTRAR:</span><span class="sxs-lookup"><span data-stu-id="07064-129">At the command prompt, type this command, and then press Enter:</span></span>

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. <span data-ttu-id="07064-130">Use la **– C** conmutador para especificar la cadena de conexión para el servidor de base de datos.</span><span class="sxs-lookup"><span data-stu-id="07064-130">Use the **–C** switch to specify the connection string for your database server.</span></span>
    2. <span data-ttu-id="07064-131">Use la **– A** conmutador para especificar las características que desea agregar a la base de datos de servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="07064-131">Use the **–A** switch to specify the application services features you want to add to the database.</span></span> <span data-ttu-id="07064-132">En este caso, **m** indica que desea agregar compatibilidad para el proveedor de pertenencia y **r** indica que desea agregar compatibilidad con el Administrador de roles.</span><span class="sxs-lookup"><span data-stu-id="07064-132">In this case, **m** indicates that you want to add support for the membership provider and **r** indicates that you want to add support for the role manager.</span></span>
    3. <span data-ttu-id="07064-133">Use la **– d** modificador para especificar un nombre para la base de datos de servicios de aplicación.</span><span class="sxs-lookup"><span data-stu-id="07064-133">Use the **–d** switch to specify a name for your application services database.</span></span> <span data-ttu-id="07064-134">Si omite este modificador, la utilidad creará una base de datos con el nombre predeterminado de **aspnetdb**.</span><span class="sxs-lookup"><span data-stu-id="07064-134">If you omit this switch, the utility will create a database with the default name of **aspnetdb**.</span></span>
3. <span data-ttu-id="07064-135">Cuando la base de datos se ha creado correctamente, la línea de comandos mostrará una confirmación.</span><span class="sxs-lookup"><span data-stu-id="07064-135">When the database has been created successfully, the command prompt will show a confirmation.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="07064-136">Para obtener más información sobre aspnet\_regsql utilidad, vea [herramienta de registro de SQL Server de ASP.NET (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="07064-136">For more information on the aspnet\_regsql utility, see [ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span></span>


<span data-ttu-id="07064-137">El siguiente paso es asegurarse de que las cadenas de conexión en la solución Contact Manager señalan a la instancia local de SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="07064-137">The next step is to make sure that the connection strings in the Contact Manager solution point to your local instance of SQL Server Express.</span></span>

<span data-ttu-id="07064-138">**Para actualizar las cadenas de conexión**</span><span class="sxs-lookup"><span data-stu-id="07064-138">**To update the connection strings**</span></span>

1. <span data-ttu-id="07064-139">Abra la solución Contact Manager en Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="07064-139">Open the Contact Manager solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="07064-140">En el **el Explorador de soluciones** ventana, expanda el **ContactManager.Mvc** del proyecto y, a continuación, haga doble clic en el **Web.config** nodo.</span><span class="sxs-lookup"><span data-stu-id="07064-140">In the **Solution Explorer** window, expand the **ContactManager.Mvc** project, and then double-click the **Web.config** node.</span></span>

    > [!NOTE]
    > <span data-ttu-id="07064-141">El proyecto ContactManager.Mvc incluye dos *web.config* archivos.</span><span class="sxs-lookup"><span data-stu-id="07064-141">The ContactManager.Mvc project includes two *web.config* files.</span></span> <span data-ttu-id="07064-142">Debe editar el archivo de nivel de proyecto.</span><span class="sxs-lookup"><span data-stu-id="07064-142">You need to edit the project-level file.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. <span data-ttu-id="07064-143">En el **connectionStrings** elemento, compruebe que la cadena de conexión con el nombre **ApplicationServices** apunta a la base de datos de servicios de aplicación ASP.NET local.</span><span class="sxs-lookup"><span data-stu-id="07064-143">In the **connectionStrings** element, verify that the connection string named **ApplicationServices** points to your local ASP.NET application services database.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. <span data-ttu-id="07064-144">En el **el Explorador de soluciones** ventana, expanda el **ContactManager.Service** del proyecto y, a continuación, haga doble clic en el **Web.config** nodo.</span><span class="sxs-lookup"><span data-stu-id="07064-144">In the **Solution Explorer** window, expand the **ContactManager.Service** project, and then double-click the **Web.config** node.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. <span data-ttu-id="07064-145">En el **connectionStrings** elemento, en la cadena de conexión denominada **ContactManagerContext**, compruebe que la **origen de datos** propiedad se establece en la instancia local de SQL Servidor Express.</span><span class="sxs-lookup"><span data-stu-id="07064-145">In the **connectionStrings** element, in the connection string named **ContactManagerContext**, verify that the **Data Source** property is set to your local instance of SQL Server Express.</span></span> <span data-ttu-id="07064-146">No es necesario cambiar nada más en la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="07064-146">You don't need to change anything else in the connection string.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. <span data-ttu-id="07064-147">Guarde todos los archivos abiertos.</span><span class="sxs-lookup"><span data-stu-id="07064-147">Save all open files.</span></span>

<span data-ttu-id="07064-148">Ahora estará preparado para ejecutar la solución Contact Manager en el equipo local.</span><span class="sxs-lookup"><span data-stu-id="07064-148">You should now be ready to run the Contact Manager solution on your local machine.</span></span>

> [!NOTE]
> <span data-ttu-id="07064-149">Si sigue estos pasos sin crear primero una base de datos de servicios de aplicación, ASP.NET creará la base de datos la primera vez que intente crear un usuario.</span><span class="sxs-lookup"><span data-stu-id="07064-149">If you follow these steps without first creating an application services database, ASP.NET will create the database the first time you attempt to create a user.</span></span> <span data-ttu-id="07064-150">Sin embargo, la creación manual de la base de datos ofrece mucho más control sobre el conjunto de características de servicios de aplicación que desea admitir.</span><span class="sxs-lookup"><span data-stu-id="07064-150">However, manually creating the database gives you a lot more control over the application services feature set you want to support.</span></span>


<span data-ttu-id="07064-151">**Para ejecutar la solución Contact Manager**</span><span class="sxs-lookup"><span data-stu-id="07064-151">**To run the Contact Manager solution**</span></span>

1. <span data-ttu-id="07064-152">En Visual Studio 2010, presione F5.</span><span class="sxs-lookup"><span data-stu-id="07064-152">In Visual Studio 2010, press F5.</span></span>
2. <span data-ttu-id="07064-153">Internet Explorer se inicia y solicita la dirección URL de la aplicación de ASP.NET MVC 3 de Contact Manager.</span><span class="sxs-lookup"><span data-stu-id="07064-153">Internet Explorer starts up and requests the URL of the Contact Manager ASP.NET MVC 3 application.</span></span> <span data-ttu-id="07064-154">De forma predeterminada, la aplicación muestra el **todos los contactos** página.</span><span class="sxs-lookup"><span data-stu-id="07064-154">By default, the application displays the **All Contacts** page.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. <span data-ttu-id="07064-155">Agregar algunos contactos y, a continuación, compruebe que la aplicación funciona según lo previsto.</span><span class="sxs-lookup"><span data-stu-id="07064-155">Add a few contacts, and then verify that the application works as expected.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. <span data-ttu-id="07064-156">Vaya a `http://localhost:50114/Account/Register` (ajuste la dirección URL si está hospedando la aplicación en un puerto diferente).</span><span class="sxs-lookup"><span data-stu-id="07064-156">Browse to `http://localhost:50114/Account/Register` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="07064-157">Agregar un nombre de usuario, dirección de correo electrónico y contraseña y confirma que eres capaz de registrar una cuenta correctamente.</span><span class="sxs-lookup"><span data-stu-id="07064-157">Add a user name, email address, and password, and verify that you're able to register an account successfully.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. <span data-ttu-id="07064-158">Vaya a `http://localhost:50114/Account/LogOn` (ajuste la dirección URL si está hospedando la aplicación en un puerto diferente).</span><span class="sxs-lookup"><span data-stu-id="07064-158">Browse to `http://localhost:50114/Account/LogOn` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="07064-159">Confirma que eres capaz de iniciar sesión con la cuenta que recién creada.</span><span class="sxs-lookup"><span data-stu-id="07064-159">Verify that you're able to log on using the account you just created.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. <span data-ttu-id="07064-160">Cierre Internet Explorer para detener la depuración.</span><span class="sxs-lookup"><span data-stu-id="07064-160">Close Internet Explorer to stop debugging.</span></span>

## <a name="conclusion"></a><span data-ttu-id="07064-161">Conclusión</span><span class="sxs-lookup"><span data-stu-id="07064-161">Conclusion</span></span>

<span data-ttu-id="07064-162">En este momento, la solución Contact Manager debe configurarse totalmente para ejecutarse en el equipo local.</span><span class="sxs-lookup"><span data-stu-id="07064-162">At this point, the Contact Manager solution should be fully configured to run on your local machine.</span></span> <span data-ttu-id="07064-163">Puede usar la solución como una referencia cuando se trabaja con los otros temas en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="07064-163">You can use the solution as a reference when you work through the other topics in this tutorial.</span></span>

<span data-ttu-id="07064-164">El tema siguiente, [descripción del archivo de proyecto](understanding-the-project-file.md), explica cómo puede usar los archivos de proyecto personalizados de Microsoft Build Engine (MSBuild) dentro de la solución Contact Manager para controlar el proceso de implementación.</span><span class="sxs-lookup"><span data-stu-id="07064-164">The next topic, [Understanding the Project File](understanding-the-project-file.md), explains how you can use the custom Microsoft Build Engine (MSBuild) project files within the Contact Manager solution to control the deployment process.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="07064-165">[Anterior](the-contact-manager-solution.md)
> [Siguiente](understanding-the-project-file.md)</span><span class="sxs-lookup"><span data-stu-id="07064-165">[Previous](the-contact-manager-solution.md)
[Next](understanding-the-project-file.md)</span></span>
