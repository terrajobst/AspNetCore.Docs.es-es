---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Crear un proyecto de equipo en TFS | Documentos de Microsoft
author: jrjlee
description: En este tema se describe cómo crear un nuevo proyecto de equipo en Team Foundation Server (TFS) 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 79c069a601c0eafd84ae142241895428052acd29
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="ed1e4-103">Crear un proyecto de equipo en TFS</span><span class="sxs-lookup"><span data-stu-id="ed1e4-103">Creating a Team Project in TFS</span></span>
====================
<span data-ttu-id="ed1e4-104">por [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="ed1e4-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="ed1e4-105">Descarga de PDF</span><span class="sxs-lookup"><span data-stu-id="ed1e4-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="ed1e4-106">En este tema se describe cómo crear un nuevo proyecto de equipo en Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>


<span data-ttu-id="ed1e4-107">Este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación de empresa de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;la [póngase en contacto con el administrador solución](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluso una aplicación de ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="ed1e4-108">Información general sobre tareas</span><span class="sxs-lookup"><span data-stu-id="ed1e4-108">Task Overview</span></span>

<span data-ttu-id="ed1e4-109">Para aprovisionar y usar un nuevo proyecto de equipo en TFS, debe completar estos pasos de alto nivel:</span><span class="sxs-lookup"><span data-stu-id="ed1e4-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="ed1e4-110">Conceder permisos al usuario que se creará el nuevo proyecto de equipo.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="ed1e4-111">Crear el proyecto de equipo.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-111">Create the team project.</span></span>
- <span data-ttu-id="ed1e4-112">Conceder permisos a los miembros del equipo que vayan a trabajar en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="ed1e4-113">Compruebe en la parte del contenido.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-113">Check in some content.</span></span>

<span data-ttu-id="ed1e4-114">En este tema le mostrará cómo realizar estos procedimientos, e identificará los usuarios y roles de trabajo que están probables que sean responsables de cada procedimiento.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="ed1e4-115">Tenga en cuenta que, dependiendo de la estructura de su organización, cada una de estas tareas puede ser la responsabilidad de una persona diferente.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="ed1e4-116">Las tareas y los tutoriales en este tema se suponen que ha instalado y configurado TFS y que ha creado una colección de proyectos de equipo como parte del proceso de configuración.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="ed1e4-117">Para obtener más información sobre estas suposiciones y para obtener más información de fondo en el escenario, consulte [configurar un servidor de compilación de TFS para la implementación Web](configuring-a-tfs-build-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="ed1e4-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="ed1e4-118">Conceder permisos para el creador del proyecto de equipo</span><span class="sxs-lookup"><span data-stu-id="ed1e4-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="ed1e4-119">Para crear un nuevo proyecto de equipo, necesitará estos permisos:</span><span class="sxs-lookup"><span data-stu-id="ed1e4-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="ed1e4-120">Debe tener la **crear nuevos proyectos** permiso en el nivel de aplicación de TFS.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="ed1e4-121">Este permiso se concede normalmente mediante la adición de usuarios para el **Project Collection Administrators** grupo de TFS.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="ed1e4-122">El **administradores de Team Foundation** grupo global también incluye este permiso.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="ed1e4-123">Debe tener permiso para crear nuevos sitios de equipo dentro de la colección de sitios de SharePoint que corresponde a la colección de proyectos de equipo TFS.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="ed1e4-124">Este permiso se concede normalmente agregando el usuario a un grupo de SharePoint con **Control total** colección de sitios de derechos en SharePoint.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="ed1e4-125">Si usa características de SQL Server Reporting Services, debe ser miembro de la **Team Foundation Content Manager** rol en Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="ed1e4-126">¿Que lleva a cabo estos procedimientos?</span><span class="sxs-lookup"><span data-stu-id="ed1e4-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="ed1e4-127">Normalmente, la persona o grupo que administra la implementación de TFS también lleva a cabo estos procedimientos.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="ed1e4-128">Dado que se trata de un conjunto de permisos con privilegios elevados, nuevos proyectos de equipo se crean normalmente un pequeño subconjunto de usuarios con la responsabilidad de administrar una implementación de TFS.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="ed1e4-129">Los desarrolladores no se normalmente le concedan los permisos necesarios para crear nuevos proyectos de equipo.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="ed1e4-130">Conceder permisos en TFS</span><span class="sxs-lookup"><span data-stu-id="ed1e4-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="ed1e4-131">Si desea que los usuarios puedan crear nuevos proyectos de equipo, es la primera tarea de alto nivel agregar el usuario a la **Project Collection Administrators** grupo de la colección de proyectos de equipo.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="ed1e4-132">**Para agregar un usuario al grupo Project Collection Administrators**</span><span class="sxs-lookup"><span data-stu-id="ed1e4-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="ed1e4-133">En el servidor TFS, en la **iniciar** menú, elija **todos los programas**, haga clic en **Microsoft Team Foundation Server 2010**y, a continuación, haga clic en **Team Foundation Consola de administración de**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="ed1e4-134">En la vista de árbol de navegación, expanda **capa de aplicación**y, a continuación, haga clic en **colecciones de proyectos de equipo**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="ed1e4-135">En el **colecciones de proyectos de equipo** panel, seleccione el proyecto de equipo que desea administrar de colección.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="ed1e4-136">En el **General** , haga clic en **pertenencia al grupo**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="ed1e4-137">En el **grupos globales** cuadro de diálogo, seleccione la **Project Collection Administrators** de grupo y, a continuación, haga clic en **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="ed1e4-138">En el **propiedades de grupo de Team Foundation Server** cuadro de diálogo, seleccione **grupo o usuario de Windows**y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="ed1e4-139">En el **Seleccionar usuarios, equipos o grupos** cuadro de diálogo, escriba el nombre de usuario del usuario que desea que pueda crear nuevos proyectos de equipo, haga clic en **comprobar nombres**y, a continuación, haga clic en **Aceptar** .</span><span class="sxs-lookup"><span data-stu-id="ed1e4-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="ed1e4-140">En el **propiedades de grupo de Team Foundation Server** cuadro de diálogo, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="ed1e4-141">En el **grupos globales** cuadro de diálogo, haga clic en **cerrar**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="ed1e4-142">Conceder permisos de SharePoint Services</span><span class="sxs-lookup"><span data-stu-id="ed1e4-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="ed1e4-143">A continuación, debe conceder al usuario permiso para crear nuevos sitios de equipo en la colección de sitios de SharePoint que corresponde a la colección de proyectos de equipo TFS.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="ed1e4-144">**Para conceder permisos de Control total en la colección de sitios de SharePoint**</span><span class="sxs-lookup"><span data-stu-id="ed1e4-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="ed1e4-145">En la consola de administración de Team Foundation Server, en la **colecciones de proyectos de equipo** , seleccione la colección de proyectos de equipo que desea administrar.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="ed1e4-146">En el **sitio de SharePoint** ficha, tenga en cuenta el valor de la **ubicación predeterminada del sitio actual** dirección URL.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="ed1e4-147">Abra Internet Explorer y, a continuación, vaya a la dirección URL que anotó en el paso 2.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ed1e4-148">Si no ha iniciado sesión Windows como el usuario que creó la colección de proyectos de equipo, debe iniciar sesión en SharePoint como este usuario para poder continuar.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="ed1e4-149">En el **acciones del sitio** menú, haga clic en **configuración del sitio**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="ed1e4-150">En el **configuración del sitio** página, en **usuarios y permisos**, haga clic en **personas y grupos**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="ed1e4-151">En el panel de navegación izquierdo, haga clic en **grupos**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="ed1e4-152">En el **personas y grupos: todos los grupos de** página, haga clic en **configurar grupos para este sitio**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > <span data-ttu-id="ed1e4-153">Es posible que reciba un <strong>HTTP 404 no encontrado</strong> error debido a un error de codificación doble de HTTP.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-153">You may receive an <strong>HTTP 404 Not Found</strong> error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="ed1e4-154">Si esto ocurre, reemplace la dirección URL a este:</span><span class="sxs-lookup"><span data-stu-id="ed1e4-154">If this occurs, replace the URL with this:</span></span>   
   > <span data-ttu-id="ed1e4-155">[<em>dirección URL de la colección de sitios</em>] /\_layouts/permsetup.aspx</span><span class="sxs-lookup"><span data-stu-id="ed1e4-155">[<em>site collection URL</em>]/\_layouts/permsetup.aspx</span></span>  
   > <span data-ttu-id="ed1e4-156">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ed1e4-156">For example:</span></span>  
   > <span data-ttu-id="ed1e4-157">http://tfs/sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx</span><span class="sxs-lookup"><span data-stu-id="ed1e4-157">http://tfs/sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx</span></span>
8. <span data-ttu-id="ed1e4-158">En el **configurar grupos para este sitio** página, agregue el usuario que vayan a crear proyectos de equipo para el **propietarios** de grupo y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-158">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="ed1e4-159">Para obtener más información acerca de cómo habilitar a los usuarios crear nuevos proyectos de equipo dentro de una colección de proyectos de equipo, consulte [establecer permisos de administrador para colecciones de proyectos de equipo](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="ed1e4-159">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="ed1e4-160">Crear un nuevo proyecto de equipo y agregar usuarios</span><span class="sxs-lookup"><span data-stu-id="ed1e4-160">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="ed1e4-161">Una vez que tenga los permisos necesarios, puede usar el **Team Explorer** ventana en Visual Studio 2010 para crear un nuevo proyecto de equipo.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-161">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="ed1e4-162">Este enfoque proporciona a un asistente que recopila toda la información necesaria y realiza las tareas necesarias en TFS, SharePoint y SQL Server Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-162">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="ed1e4-163">También debe conceder permisos en el nuevo proyecto de equipo a los miembros del equipo del desarrollador, para que puedan agregar y modificar contenido.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-163">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="ed1e4-164">¿Que lleva a cabo estos procedimientos?</span><span class="sxs-lookup"><span data-stu-id="ed1e4-164">Who Performs These Procedures?</span></span>

<span data-ttu-id="ed1e4-165">Normalmente un administrador de TFS o un jefe de equipo del desarrollador lleva a cabo estos procedimientos.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-165">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="ed1e4-166">Cree un nuevo proyecto de equipo</span><span class="sxs-lookup"><span data-stu-id="ed1e4-166">Create a New Team Project</span></span>

<span data-ttu-id="ed1e4-167">El procedimiento siguiente describe cómo crear un nuevo proyecto de equipo en TFS 2010.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-167">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="ed1e4-168">**Para crear un nuevo proyecto de equipo**</span><span class="sxs-lookup"><span data-stu-id="ed1e4-168">**To create a new team project**</span></span>

1. <span data-ttu-id="ed1e4-169">En el **iniciar** menú, elija **todos los programas**, haga clic en **Microsoft Visual Studio 2010**, haga clic en **Microsoft Visual Studio 2010**, y, a continuación, haga clic en **ejecutar como administrador**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-169">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ed1e4-170">Si no ejecuta Visual Studio 2010 como administrador, se producirá un error en el Asistente para nuevo proyecto de equipo en el último paso.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-170">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="ed1e4-171">Si el **User Account Control** aparece el cuadro de diálogo, haga clic en **Sí**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-171">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="ed1e4-172">En Visual Studio, en el **equipo** menú, haga clic en **conectar con Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-172">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ed1e4-173">Si ya ha configurado una conexión a un servidor TFS, puede omitir los pasos del 4 al 7.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-173">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="ed1e4-174">En el **conexión al proyecto de equipo** cuadro de diálogo, haga clic en **servidores**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-174">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="ed1e4-175">En el **agregar o quitar Team Foundation Server** cuadro de diálogo, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-175">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="ed1e4-176">En el **agregar Team Foundation Server** cuadro de diálogo, proporcione los detalles de la instancia de TFS y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-176">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="ed1e4-177">En el **agregar o quitar Team Foundation Server** cuadro de diálogo, haga clic en **cerrar**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-177">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="ed1e4-178">En el **conectar al proyecto de equipo** cuadro de diálogo, seleccione la instancia TFS que desea conectarse a la red, seleccione el equipo de proyecto recopilación que desea agregar a y, a continuación, haga clic en **conectar**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-178">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="ed1e4-179">En el **Team Explorer** ventana, haga que el equipo de la colección de proyectos y, a continuación, haga clic en **nuevo proyecto de equipo**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-179">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="ed1e4-180">En el **nuevo proyecto de equipo** cuadro de diálogo, proporcione un nombre y una descripción para el proyecto de equipo y, a continuación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-180">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ed1e4-181">Si el proyecto de equipo incluye espacios, pueden darse algunos problemas cuando llegue a utilizar la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy) para implementar paquetes de la ruta de acceso de salida.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-181">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="ed1e4-182">Espacios en la ruta de acceso pueden hacer mucho más difícil ejecutar comandos de Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-182">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="ed1e4-183">En el **seleccione una plantilla de proceso** , seleccione la plantilla de proceso que desea usar para administrar el proceso de desarrollo y, a continuación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-183">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ed1e4-184">Para obtener más información sobre plantillas de proceso de TFS, consulte [plantillas de proceso y las herramientas](https://msdn.microsoft.com/vstudio/aa718795).</span><span class="sxs-lookup"><span data-stu-id="ed1e4-184">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/vstudio/aa718795).</span></span>
12. <span data-ttu-id="ed1e4-185">En el **configuración del sitio de equipo** página, no modifique la configuración predeterminada y, a continuación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-185">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="ed1e4-186">Esta opción crea o identifica un sitio de grupo de SharePoint que está asociado con el proyecto de equipo TFS.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-186">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="ed1e4-187">El equipo de desarrollo puede utilizar este sitio para administrar la documentación, participar en los hilos de discusión, crear páginas wiki y realice otras tareas que no están relacionados con el código.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-187">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="ed1e4-188">Para obtener más información, consulte [las interacciones entre los productos de SharePoint y Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span><span class="sxs-lookup"><span data-stu-id="ed1e4-188">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="ed1e4-189">En el **especificar configuración de Control de código fuente** página, no modifique la configuración predeterminada y, a continuación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-189">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="ed1e4-190">Esta configuración identifica o crea la ubicación en la jerarquía de carpetas TFS que actuará como una carpeta raíz para el contenido.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-190">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="ed1e4-191">En el **Confirmar configuración del proyecto de equipo** página, haga clic en **finalizar**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-191">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="ed1e4-192">Cuando el nuevo proyecto de equipo se crea correctamente, en la **proyecto de equipo creado** página, haga clic en **cerrar**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-192">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="ed1e4-193">Agregar usuarios a un proyecto de equipo</span><span class="sxs-lookup"><span data-stu-id="ed1e4-193">Add Users to a Team Project</span></span>

<span data-ttu-id="ed1e4-194">Ahora que ha creado el nuevo proyecto de equipo, puede conceder permisos a los usuarios para que puedan empezar a agregar y colaborar en el contenido.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-194">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="ed1e4-195">**Para agregar usuarios a un proyecto de equipo**</span><span class="sxs-lookup"><span data-stu-id="ed1e4-195">**To add users to a team project**</span></span>

1. <span data-ttu-id="ed1e4-196">En Visual Studio 2010, en la **Team Explorer** ventana, haga clic en el proyecto de equipo, seleccione **configuración del proyecto de equipo**y, a continuación, haga clic en **pertenencia al grupo**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-196">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="ed1e4-197">Para que los usuarios puedan agregar, modificar y quitar el código sometido a control de código fuente, debe agregarlo a la **colaboradores** grupo.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-197">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="ed1e4-198">En el **grupos proyecto** cuadro de diálogo, seleccione la **colaboradores** de grupo y, a continuación, haga clic en **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-198">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="ed1e4-199">En el **propiedades de grupo de Team Foundation Server** cuadro de diálogo, seleccione **grupo o usuario de Windows**y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-199">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="ed1e4-200">En el **Seleccionar usuarios, equipos o grupos** cuadro de diálogo, escriba el nombre de usuario del usuario que desea agregar al proyecto de equipo, haga clic en **comprobar nombres**y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-200">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="ed1e4-201">En el **propiedades de grupo de Team Foundation Server** cuadro de diálogo, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-201">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="ed1e4-202">En el **grupos proyecto** cuadro de diálogo, haga clic en **cerrar**.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-202">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="ed1e4-203">Conclusión</span><span class="sxs-lookup"><span data-stu-id="ed1e4-203">Conclusion</span></span>

<span data-ttu-id="ed1e4-204">En este punto, está listo para usar el nuevo proyecto de equipo y el equipo de desarrolladores puede empezar a agregar contenido y colaborar en el proceso de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-204">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="ed1e4-205">El siguiente tema, [agregar contenido al Control de código fuente](adding-content-to-source-control.md), se describe cómo agregar contenido al control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="ed1e4-205">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="ed1e4-206">Información adicional</span><span class="sxs-lookup"><span data-stu-id="ed1e4-206">Further Reading</span></span>

<span data-ttu-id="ed1e4-207">Para obtener más amplia información sobre la creación de proyectos de equipo en TFS, consulte [crear un proyecto de equipo](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="ed1e4-207">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="ed1e4-208">Para obtener más información acerca de cómo habilitar a los usuarios crear nuevos proyectos de equipo dentro de una colección de proyectos de equipo, consulte [establecer permisos de administrador para colecciones de proyectos de equipo](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="ed1e4-208">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span> <span data-ttu-id="ed1e4-209">Para obtener más información sobre cómo agregar usuarios a proyectos de equipo, consulte [agregar usuarios a proyectos de equipo](https://msdn.microsoft.com/library/bb558971.aspx).</span><span class="sxs-lookup"><span data-stu-id="ed1e4-209">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ed1e4-210">[Anterior](configuring-team-foundation-server-for-web-deployment.md)
> [Siguiente](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="ed1e4-210">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>
