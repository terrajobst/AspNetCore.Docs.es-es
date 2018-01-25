---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: "Agregar contenido a Control de código fuente | Documentos de Microsoft"
author: jrjlee
description: "Este tema explica cómo agregar contenido a control de código fuente de Team Foundation Server (TFS) 2010. Describe cómo agregar soluciones y proyectos a un proyecto de equipo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: d46e2697d10ca27f8e08533350a6e7f2354b4a43
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="adding-content-to-source-control"></a><span data-ttu-id="93c1d-104">Agregar contenido a Control de código fuente</span><span class="sxs-lookup"><span data-stu-id="93c1d-104">Adding Content to Source Control</span></span>
====================
<span data-ttu-id="93c1d-105">por [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="93c1d-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="93c1d-106">Descarga de PDF</span><span class="sxs-lookup"><span data-stu-id="93c1d-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="93c1d-107">Este tema explica cómo agregar contenido a control de código fuente de Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="93c1d-107">This topic explains how to add content to source control in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="93c1d-108">Describe cómo agregar soluciones y proyectos a un proyecto de equipo en TFS, y se explica cómo agregar dependencias externas como .NET Framework o ensamblados al control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="93c1d-108">It describes how to add solutions and projects to a team project in TFS, and it explains how to add external dependencies like frameworks or assemblies to source control.</span></span>


<span data-ttu-id="93c1d-109">Este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación de empresa de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales que utiliza una solución de ejemplo & #x 2014; la [póngase en contacto con el administrador solución](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; para representar una aplicación web con un nivel de complejidad, incluso una aplicación de ASP.NET MVC 3, Windows realista Servicio de Communication Foundation (WCF) y un proyecto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="93c1d-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="93c1d-110">Información general sobre tareas</span><span class="sxs-lookup"><span data-stu-id="93c1d-110">Task Overview</span></span>

<span data-ttu-id="93c1d-111">En la mayoría de los casos, todos los miembros del equipo de desarrollo deben ser capaz de agregar contenido al control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="93c1d-111">In most cases, every member of the developer team should be able to add content to source control.</span></span> <span data-ttu-id="93c1d-112">Para agregar una solución al control de código fuente en TFS, debe completar estos pasos de alto nivel:</span><span class="sxs-lookup"><span data-stu-id="93c1d-112">To add a solution to source control in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="93c1d-113">Conectarse a un proyecto de equipo.</span><span class="sxs-lookup"><span data-stu-id="93c1d-113">Connect to a team project.</span></span>
- <span data-ttu-id="93c1d-114">Asignar la estructura de carpetas del proyecto de equipo en el servidor a una estructura de carpetas en el equipo local.</span><span class="sxs-lookup"><span data-stu-id="93c1d-114">Map the team project folder structure on the server to a folder structure on your local computer.</span></span>
- <span data-ttu-id="93c1d-115">Agregar la solución y su contenido al control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="93c1d-115">Add the solution and its contents to source control.</span></span>
- <span data-ttu-id="93c1d-116">Agregar las dependencias externas a control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="93c1d-116">Add any external dependencies to source control.</span></span>

<span data-ttu-id="93c1d-117">En este tema le mostrará cómo realizar estos procedimientos.</span><span class="sxs-lookup"><span data-stu-id="93c1d-117">This topic will show you how to perform these procedures.</span></span>

<span data-ttu-id="93c1d-118">Las tareas y los tutoriales en este tema se suponen que ya ha creado un nuevo proyecto de equipo TFS para administrar el contenido.</span><span class="sxs-lookup"><span data-stu-id="93c1d-118">The tasks and walkthroughs in this topic assume that you've already created a new TFS team project to manage your content.</span></span> <span data-ttu-id="93c1d-119">Para obtener más información acerca de cómo crear un nuevo proyecto de equipo, consulte [crea un proyecto de equipo en TFS](creating-a-team-project-in-tfs.md).</span><span class="sxs-lookup"><span data-stu-id="93c1d-119">For more information on creating a new team project, see [Creating a Team Project in TFS](creating-a-team-project-in-tfs.md).</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="93c1d-120">¿Que lleva a cabo estos procedimientos?</span><span class="sxs-lookup"><span data-stu-id="93c1d-120">Who Performs These Procedures?</span></span>

<span data-ttu-id="93c1d-121">En la mayoría de los casos, todos los miembros del equipo del desarrollador de podrá agregar y modificar contenido dentro de los proyectos de equipo específico.</span><span class="sxs-lookup"><span data-stu-id="93c1d-121">In most cases, every member of the developer team should be able to add and modify content within specific team projects.</span></span>

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a><span data-ttu-id="93c1d-122">Conectarse a un proyecto de equipo y crear una asignación de carpeta</span><span class="sxs-lookup"><span data-stu-id="93c1d-122">Connect to a Team Project and Create a Folder Mapping</span></span>

<span data-ttu-id="93c1d-123">Antes de agregar cualquier tipo de contenido para el control de código fuente, debe conectarse a un proyecto de equipo y crear una asignación entre la estructura de carpetas en el servidor y el sistema de archivos en el equipo local.</span><span class="sxs-lookup"><span data-stu-id="93c1d-123">Before you add any content to source control, you need to connect to a team project and create a mapping between the folder structure on the server and the file system on your local machine.</span></span>

<span data-ttu-id="93c1d-124">**Para conectarse a un proyecto de equipo y asignar una ruta de acceso local**</span><span class="sxs-lookup"><span data-stu-id="93c1d-124">**To connect to a team project and map a local path**</span></span>

1. <span data-ttu-id="93c1d-125">En la estación de trabajo de desarrollador, abra Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="93c1d-125">On your developer workstation, open Visual Studio 2010.</span></span>
2. <span data-ttu-id="93c1d-126">En Visual Studio, en el **equipo** menú, haga clic en **conectar con Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="93c1d-126">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="93c1d-127">Si ya ha configurado una conexión a un servidor TFS, puede omitir los pasos 3 a 6.</span><span class="sxs-lookup"><span data-stu-id="93c1d-127">If you have already configured a connection to a TFS server, you can omit steps 3-6.</span></span>
3. <span data-ttu-id="93c1d-128">En el **conexión al proyecto de equipo** cuadro de diálogo, haga clic en **servidores**.</span><span class="sxs-lookup"><span data-stu-id="93c1d-128">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
4. <span data-ttu-id="93c1d-129">En el **agregar o quitar Team Foundation Server** cuadro de diálogo, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="93c1d-129">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
5. <span data-ttu-id="93c1d-130">En el **agregar Team Foundation Server** cuadro de diálogo, proporcione los detalles de la instancia de TFS y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="93c1d-130">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](adding-content-to-source-control/_static/image1.png)
6. <span data-ttu-id="93c1d-131">En el **agregar o quitar Team Foundation Server** cuadro de diálogo, haga clic en **cerrar**.</span><span class="sxs-lookup"><span data-stu-id="93c1d-131">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
7. <span data-ttu-id="93c1d-132">En el **conectar al proyecto de equipo** cuadro de diálogo, seleccione la instancia TFS que desea conectarse a la red, seleccione el equipo de colección de proyectos, seleccione el proyecto de equipo que desea agregar a y, a continuación, haga clic en **conectar**.</span><span class="sxs-lookup"><span data-stu-id="93c1d-132">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection, select the team project you want to add to, and then click **Connect**.</span></span>

    ![](adding-content-to-source-control/_static/image2.png)
8. <span data-ttu-id="93c1d-133">En el **Team Explorer** ventana, expanda el proyecto de equipo y, a continuación, haga doble clic en **Control de código fuente**.</span><span class="sxs-lookup"><span data-stu-id="93c1d-133">In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image3.png)
9. <span data-ttu-id="93c1d-134">En el **Explorador de Control de código fuente** , haga clic en **no asignado**.</span><span class="sxs-lookup"><span data-stu-id="93c1d-134">On the **Source Control Explorer** tab, click **Not mapped**.</span></span>

    ![](adding-content-to-source-control/_static/image4.png)
10. <span data-ttu-id="93c1d-135">En el **mapa** cuadro de diálogo, en la **carpeta Local** cuadro, vaya a (o crear) una carpeta local para actuar como la carpeta raíz del proyecto de equipo y, a continuación, haga clic en **mapa**.</span><span class="sxs-lookup"><span data-stu-id="93c1d-135">In the **Map** dialog box, in the **Local folder** box, browse to (or create) a local folder to act as the root folder for the team project, and then click **Map**.</span></span>

    ![](adding-content-to-source-control/_static/image5.png)
11. <span data-ttu-id="93c1d-136">Cuando le pide que descargue archivos de origen, haga clic en **Sí**.</span><span class="sxs-lookup"><span data-stu-id="93c1d-136">When you're prompted to download source files, click **Yes**.</span></span>

    ![](adding-content-to-source-control/_static/image6.png)

<span data-ttu-id="93c1d-137">En este momento, ha asignado la carpeta de servidor para el proyecto de equipo a una carpeta local en la estación de trabajo de desarrollador.</span><span class="sxs-lookup"><span data-stu-id="93c1d-137">At this point, you have mapped the server-side folder for the team project to a local folder on your developer workstation.</span></span> <span data-ttu-id="93c1d-138">También ha descargado todo el contenido del proyecto de equipo a la estructura de la carpeta local.</span><span class="sxs-lookup"><span data-stu-id="93c1d-138">You've also downloaded any existing content from the team project to your local folder structure.</span></span> <span data-ttu-id="93c1d-139">Ahora puede empezar a agregar su propio contenido al control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="93c1d-139">You can now start to add your own content to source control.</span></span>

## <a name="add-projects-and-solutions-to-source-control"></a><span data-ttu-id="93c1d-140">Agregar proyectos y soluciones al Control de código fuente</span><span class="sxs-lookup"><span data-stu-id="93c1d-140">Add Projects and Solutions to Source Control</span></span>

<span data-ttu-id="93c1d-141">Para agregar proyectos y soluciones al control de código fuente, primero debe mover a la carpeta del proyecto de equipo asignada en el equipo local.</span><span class="sxs-lookup"><span data-stu-id="93c1d-141">To add projects and solutions to source control, you first need to move them to the mapped folder for the team project on your local machine.</span></span> <span data-ttu-id="93c1d-142">A continuación, puede comprobar en el contenido que se va a sincronizar las adiciones con el servidor.</span><span class="sxs-lookup"><span data-stu-id="93c1d-142">You can then check in the content to synchronize your additions with the server.</span></span>

<span data-ttu-id="93c1d-143">**Para agregar proyectos al control de código fuente**</span><span class="sxs-lookup"><span data-stu-id="93c1d-143">**To add projects to source control**</span></span>

1. <span data-ttu-id="93c1d-144">En la estación de trabajo de desarrollador, mueva los proyectos y soluciones a una ubicación adecuada en la estructura de carpetas asignadas para el proyecto de equipo.</span><span class="sxs-lookup"><span data-stu-id="93c1d-144">On your developer workstation, move your projects and solutions to an appropriate location within the mapped folder structure for the team project.</span></span>

    > [!NOTE]
    > <span data-ttu-id="93c1d-145">Muchas organizaciones tienen un método preferido para cómo deben organizarse proyectos y soluciones en el control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="93c1d-145">Many organizations will have a preferred approach to how projects and solutions should be organized in source control.</span></span> <span data-ttu-id="93c1d-146">Para obtener instrucciones sobre cómo estructurar carpetas, consulte [How To: estructura de carpetas de Control de su origen en Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span><span class="sxs-lookup"><span data-stu-id="93c1d-146">For guidance on how to structure folders, see [How To: Structure Your Source Control Folders in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span></span>
2. <span data-ttu-id="93c1d-147">Abra la solución en Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="93c1d-147">Open the solution in Visual Studio 2010.</span></span>
3. <span data-ttu-id="93c1d-148">En el **el Explorador de soluciones** ventana, haga clic en la solución y, a continuación, haga clic en **Agregar solución al Control de código fuente**.</span><span class="sxs-lookup"><span data-stu-id="93c1d-148">In the **Solution Explorer** window, right-click the solution, and then click **Add Solution to Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="93c1d-149">En algunos casos, dependiendo de cómo su organización le gusta contenido de la estructura en TFS, debe agregar proyectos al control de código fuente individualmente para proporcionar un mayor control sobre cómo se organiza el código fuente.</span><span class="sxs-lookup"><span data-stu-id="93c1d-149">In some cases, depending on how your organization likes to structure content in TFS, you may need to add projects to source control individually to provide more fine-grained control over how your source code is organized.</span></span>
4. <span data-ttu-id="93c1d-150">Compruebe que la **Explorador de Control de código fuente** pestaña muestra el contenido que haya agregado dentro de la estructura de carpetas del servidor para el proyecto de equipo.</span><span class="sxs-lookup"><span data-stu-id="93c1d-150">Verify that the **Source Control Explorer** tab displays the content you've added within the server folder structure for the team project.</span></span>

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="93c1d-151">El **Explorador de Control de código fuente** pestaña muestra el contenido con ninguna otra preguntar porque se ha agregado la solución a una carpeta asignada en el sistema de archivos local.</span><span class="sxs-lookup"><span data-stu-id="93c1d-151">The **Source Control Explorer** tab displays your content with no further prompting because you added your solution to a mapped folder on the local file system.</span></span> <span data-ttu-id="93c1d-152">Si la solución se encontraba en una ubicación sin asignar, le solicita para especificar ubicaciones de carpeta en TFS y el sistema de archivos local.</span><span class="sxs-lookup"><span data-stu-id="93c1d-152">If your solution was in an unmapped location, you'd be prompted to specify folder locations in both TFS and your local file system.</span></span>
5. <span data-ttu-id="93c1d-153">En el **Explorador de Control de código fuente** ficha la **carpetas** panel, haga clic en el proyecto de equipo (por ejemplo, **ContactManager**) y, a continuación, haga clic en **insertar en el repositorio Los cambios pendientes**.</span><span class="sxs-lookup"><span data-stu-id="93c1d-153">On the **Source Control Explorer** tab, in the **Folders** pane, right-click the team project (for example, **ContactManager**), and then click **Check In Pending Changes**.</span></span>
6. <span data-ttu-id="93c1d-154">En el **insertar en el repositorio: archivos de código fuente** cuadro de diálogo, escriba un comentario y, a continuación, haga clic en **proteger**.</span><span class="sxs-lookup"><span data-stu-id="93c1d-154">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

    ![](adding-content-to-source-control/_static/image9.png)

<span data-ttu-id="93c1d-155">En este punto ha agregado la solución al control de código fuente en TFS.</span><span class="sxs-lookup"><span data-stu-id="93c1d-155">At this point you have added your solution to source control in TFS.</span></span>

## <a name="add-external-dependencies-to-source-control"></a><span data-ttu-id="93c1d-156">Agregar dependencias externas a Control de código fuente</span><span class="sxs-lookup"><span data-stu-id="93c1d-156">Add External Dependencies to Source Control</span></span>

<span data-ttu-id="93c1d-157">Cuando se agrega un proyecto o solución al control de código fuente, también se agregará los archivos y carpetas en el proyecto o solución.</span><span class="sxs-lookup"><span data-stu-id="93c1d-157">When you add a project or solution to source control, any files and folders within your project or solution will also be added.</span></span> <span data-ttu-id="93c1d-158">Sin embargo, en muchos casos, proyectos y soluciones también se basan en las dependencias externas, como ensamblados locales para funcionar correctamente.</span><span class="sxs-lookup"><span data-stu-id="93c1d-158">However, in a lot of cases, projects and solutions also rely on external dependencies, like local assemblies, to function properly.</span></span> <span data-ttu-id="93c1d-159">Debe agregar cualquier estos recursos al control de código fuente para permitir que Team Build y otros miembros del equipo del desarrollador de compilan el código correctamente.</span><span class="sxs-lookup"><span data-stu-id="93c1d-159">You need to add any such resources to source control to let both Team Build and other members of the developer team build your code successfully.</span></span>

<span data-ttu-id="93c1d-160">Por ejemplo, la estructura de carpetas para la solución de ejemplo Contact Manager incluye una carpeta denominada paquetes.</span><span class="sxs-lookup"><span data-stu-id="93c1d-160">For example, the folder structure for the Contact Manager sample solution includes a folder named packages.</span></span> <span data-ttu-id="93c1d-161">Contiene el ensamblado y varios recursos de soporte para ADO.NET Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="93c1d-161">This contains the assembly and various supporting resources for the ADO.NET Entity Framework 4.1.</span></span> <span data-ttu-id="93c1d-162">La carpeta de paquetes no es parte de la solución póngase en contacto con el administrador, pero la solución no se compilará correctamente sin él.</span><span class="sxs-lookup"><span data-stu-id="93c1d-162">The packages folder is not part of the Contact Manager solution, but the solution will not build successfully without it.</span></span> <span data-ttu-id="93c1d-163">Para habilitar Team Build compilar la solución, debe agregar la carpeta de paquetes al control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="93c1d-163">To enable Team Build to build the solution, you need to add the packages folder to source control.</span></span>

> [!NOTE]
> <span data-ttu-id="93c1d-164">La inclusión de una carpeta de paquetes es típica de lo que sucede al agregar el Entity Framework o recursos similares, a la solución utilizando la extensión de NuGet para Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="93c1d-164">The inclusion of a packages folder is typical of what happens when you add the Entity Framework, or similar resources, to your solution using the NuGet extension for Visual Studio 2010.</span></span>


<span data-ttu-id="93c1d-165">**Para agregar contenido no son del proyecto al control de código fuente**</span><span class="sxs-lookup"><span data-stu-id="93c1d-165">**To add non-project content to source control**</span></span>

1. <span data-ttu-id="93c1d-166">Asegúrese de que los elementos que desee agregar (por ejemplo, la carpeta de paquetes) en una ubicación adecuada en una carpeta asignada en el sistema de archivos local.</span><span class="sxs-lookup"><span data-stu-id="93c1d-166">Ensure that the items you want to add (for example, the packages folder) are in an appropriate location within a mapped folder on your local file system.</span></span>
2. <span data-ttu-id="93c1d-167">En Visual Studio 2010, en la **Team Explorer** ventana, expanda el proyecto de equipo y, a continuación, haga doble clic en **Control de código fuente**.</span><span class="sxs-lookup"><span data-stu-id="93c1d-167">In Visual Studio 2010, In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image10.png)
3. <span data-ttu-id="93c1d-168">En el **Explorador de Control de código fuente** ficha la **carpetas** panel, seleccione la carpeta que contiene el elemento o elementos desea agregar.</span><span class="sxs-lookup"><span data-stu-id="93c1d-168">On the **Source Control Explorer** tab, in the **Folders** pane, select the folder that contains the item or items you want to add.</span></span>
4. <span data-ttu-id="93c1d-169">Haga clic en el **agregar elementos a la carpeta** botón.</span><span class="sxs-lookup"><span data-stu-id="93c1d-169">Click the **Add Items to Folder** button.</span></span>

    ![](adding-content-to-source-control/_static/image11.png)
5. <span data-ttu-id="93c1d-170">En el **agregar al Control de código fuente** diálogo cuadro, seleccione la carpeta o los elementos que desea agregar y, a continuación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="93c1d-170">In the **Add to Source Control** dialog box, select the folder or items you want to add, and then click **Next**.</span></span>

    ![](adding-content-to-source-control/_static/image12.png)
6. <span data-ttu-id="93c1d-171">En el **elementos excluidos** ficha, seleccione los elementos necesarios que se han excluido automáticamente (por ejemplo, ensamblados) y, a continuación, haga clic en **incluyen elementos**.</span><span class="sxs-lookup"><span data-stu-id="93c1d-171">On the **Excluded items** tab, select any required items that have been automatically excluded (for example, assemblies), and then click **Include item(s)**.</span></span>

    ![](adding-content-to-source-control/_static/image13.png)
7. <span data-ttu-id="93c1d-172">En el **elementos que desea agregar** , compruebe que se enumeran todos los archivos que van a incluir y, a continuación, haga clic en **finalizar**.</span><span class="sxs-lookup"><span data-stu-id="93c1d-172">On the **Items to add** tab, verify that all the files you want to include are listed, and then click **Finish**.</span></span>

    ![](adding-content-to-source-control/_static/image14.png)
8. <span data-ttu-id="93c1d-173">En el **Explorador de Control de código fuente** ventana, haga clic en el **proteger** botón.</span><span class="sxs-lookup"><span data-stu-id="93c1d-173">In the **Source Control Explorer** window, click the **Check In** button.</span></span>

    ![](adding-content-to-source-control/_static/image15.png)
9. <span data-ttu-id="93c1d-174">En el **insertar en el repositorio: archivos de código fuente** cuadro de diálogo, escriba un comentario y, a continuación, haga clic en **proteger**.</span><span class="sxs-lookup"><span data-stu-id="93c1d-174">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

<span data-ttu-id="93c1d-175">En este momento, ha agregado las dependencias externas para la solución al control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="93c1d-175">At this point, you have added the external dependencies for your solution to source control.</span></span>

## <a name="conclusion"></a><span data-ttu-id="93c1d-176">Conclusión</span><span class="sxs-lookup"><span data-stu-id="93c1d-176">Conclusion</span></span>

<span data-ttu-id="93c1d-177">En este tema se describe cómo conectarse a un proyecto de equipo, asigne una estructura de carpetas y agregar contenido al control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="93c1d-177">This topic described how to connect to a team project, map a folder structure, and add content to source control.</span></span> <span data-ttu-id="93c1d-178">Para obtener más información sobre cómo trabajar con elementos en el control de código fuente, consulte [Control de versiones utilizando](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="93c1d-178">For more information on how to work with items under source control, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

<span data-ttu-id="93c1d-179">El siguiente tema, [configurar un servidor de compilación de TFS para la implementación Web](configuring-a-tfs-build-server-for-web-deployment.md), se describe cómo preparar un servidor TFS Team Build para compilar e implementar la solución.</span><span class="sxs-lookup"><span data-stu-id="93c1d-179">The next topic, [Configuring a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md), describes how to prepare a TFS Team Build server to build and deploy your solution.</span></span>

## <a name="further-reading"></a><span data-ttu-id="93c1d-180">Información adicional</span><span class="sxs-lookup"><span data-stu-id="93c1d-180">Further Reading</span></span>

<span data-ttu-id="93c1d-181">Para obtener información más completa sobre cómo trabajar con control de código fuente en TFS, consulte [Control de versiones utilizando](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="93c1d-181">For more comprehensive information on working with source control in TFS, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="93c1d-182">[Anterior](creating-a-team-project-in-tfs.md)
[Siguiente](configuring-a-tfs-build-server-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="93c1d-182">[Previous](creating-a-team-project-in-tfs.md)
[Next](configuring-a-tfs-build-server-for-web-deployment.md)</span></span>
