---
uid: signalr/overview/performance/scaleout-with-sql-server
title: Ampliación de SignalR con SQL Server | Documentos de Microsoft
author: MikeWasson
description: Versiones de software emplea en este tema Visual Studio 2013 .NET 4.5 SignalR las versiones anteriores de la versión 2 de este tema para obtener información acerca de las versiones anteriores de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: b3189c36fc076333c0c6007bd039b12e03d63bc8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874374"
---
<a name="signalr-scaleout-with-sql-server"></a><span data-ttu-id="4be4d-103">Ampliación de SignalR con SQL Server</span><span class="sxs-lookup"><span data-stu-id="4be4d-103">SignalR Scaleout with SQL Server</span></span>
====================
<span data-ttu-id="4be4d-104">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="4be4d-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="4be4d-105">Versiones de software que se usa en este tema</span><span class="sxs-lookup"><span data-stu-id="4be4d-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="4be4d-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="4be4d-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="4be4d-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="4be4d-107">.NET 4.5</span></span>
> - <span data-ttu-id="4be4d-108">SignalR versión 2</span><span class="sxs-lookup"><span data-stu-id="4be4d-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="4be4d-109">Versiones anteriores de este tema</span><span class="sxs-lookup"><span data-stu-id="4be4d-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="4be4d-110">Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="4be4d-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="4be4d-111">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="4be4d-111">Questions and comments</span></span>
> 
> <span data-ttu-id="4be4d-112">Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="4be4d-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="4be4d-113">Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="4be4d-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="4be4d-114">En este tutorial, utilizará SQL Server para distribuir mensajes en una aplicación de SignalR que se implementa en dos instancias independientes de IIS.</span><span class="sxs-lookup"><span data-stu-id="4be4d-114">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="4be4d-115">También puede ejecutar este tutorial en un equipo de prueba individual, pero para obtener el efecto completo, debe implementar la aplicación de SignalR en dos o más servidores.</span><span class="sxs-lookup"><span data-stu-id="4be4d-115">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="4be4d-116">También debe instalar a SQL Server en uno de los servidores o en un servidor dedicado independiente.</span><span class="sxs-lookup"><span data-stu-id="4be4d-116">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="4be4d-117">Otra opción consiste en ejecutar el tutorial usando máquinas virtuales en Azure.</span><span class="sxs-lookup"><span data-stu-id="4be4d-117">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="4be4d-118">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="4be4d-118">Prerequisites</span></span>

<span data-ttu-id="4be4d-119">Microsoft SQL Server 2005 o posterior.</span><span class="sxs-lookup"><span data-stu-id="4be4d-119">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="4be4d-120">El backplane es compatible con las ediciones de escritorio y servidores de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4be4d-120">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="4be4d-121">No es compatible con SQL Server Compact Edition o base de datos de SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="4be4d-121">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="4be4d-122">(Si la aplicación se hospeda en Azure, se recomienda el backplane de Bus de servicio en su lugar.)</span><span class="sxs-lookup"><span data-stu-id="4be4d-122">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="4be4d-123">Información general</span><span class="sxs-lookup"><span data-stu-id="4be4d-123">Overview</span></span>

<span data-ttu-id="4be4d-124">Antes de entrar en el tutorial detallado, presentamos una introducción rápida de lo va a hacer.</span><span class="sxs-lookup"><span data-stu-id="4be4d-124">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="4be4d-125">Crear una nueva base de datos vacía.</span><span class="sxs-lookup"><span data-stu-id="4be4d-125">Create a new empty database.</span></span> <span data-ttu-id="4be4d-126">El backplane creará las tablas necesarias en esta base de datos.</span><span class="sxs-lookup"><span data-stu-id="4be4d-126">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="4be4d-127">Agregue estos paquetes de NuGet para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="4be4d-127">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="4be4d-128">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="4be4d-128">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="4be4d-129">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="4be4d-129">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="4be4d-130">Cree una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="4be4d-130">Create a SignalR application.</span></span>
4. <span data-ttu-id="4be4d-131">Agregue el código siguiente a Startup.cs para configurar el plano posterior:</span><span class="sxs-lookup"><span data-stu-id="4be4d-131">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   <span data-ttu-id="4be4d-132">Este código configura el plano posterior con los valores predeterminados de [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) y [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="4be4d-132">This code configures the backplane with the default values for [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="4be4d-133">Para obtener información acerca de cómo cambiar estos valores, consulte [SignalR rendimiento: ampliación métricas](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="4be4d-133">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span> 

## <a name="configure-the-database"></a><span data-ttu-id="4be4d-134">Configurar la base de datos</span><span class="sxs-lookup"><span data-stu-id="4be4d-134">Configure the Database</span></span>

<span data-ttu-id="4be4d-135">Decidir si la aplicación utilizará la autenticación de Windows o autenticación de SQL Server para tener acceso a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4be4d-135">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="4be4d-136">En cualquier caso, asegúrese de que el usuario de base de datos tiene permisos para iniciar sesión, crear esquemas y crear tablas.</span><span class="sxs-lookup"><span data-stu-id="4be4d-136">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="4be4d-137">Crear una nueva base de datos para el plano posterior a la utilice.</span><span class="sxs-lookup"><span data-stu-id="4be4d-137">Create a new database for the backplane to use.</span></span> <span data-ttu-id="4be4d-138">Puede utilizar cualquier nombre para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4be4d-138">You can give the database any name.</span></span> <span data-ttu-id="4be4d-139">No es necesario crear ninguna tabla en la base de datos; el backplane creará las tablas necesarias.</span><span class="sxs-lookup"><span data-stu-id="4be4d-139">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="4be4d-140">Habilitar Service Broker</span><span class="sxs-lookup"><span data-stu-id="4be4d-140">Enable Service Broker</span></span>

<span data-ttu-id="4be4d-141">Se recomienda habilitar Service Broker para la base de datos de backplane.</span><span class="sxs-lookup"><span data-stu-id="4be4d-141">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="4be4d-142">Service Broker proporciona compatibilidad nativa para la mensajería y puesta en cola en SQL Server, lo que permite el backplane recibir actualizaciones de forma más eficaz.</span><span class="sxs-lookup"><span data-stu-id="4be4d-142">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="4be4d-143">(Sin embargo, el plano posterior también funciona sin el Service Broker).</span><span class="sxs-lookup"><span data-stu-id="4be4d-143">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="4be4d-144">Para comprobar si está habilitado Service Broker, consultar la **es\_broker\_habilitado** columna en el **sys.databases** vista de catálogo.</span><span class="sxs-lookup"><span data-stu-id="4be4d-144">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="4be4d-145">Para habilitar a Service Broker, use la siguiente consulta SQL:</span><span class="sxs-lookup"><span data-stu-id="4be4d-145">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="4be4d-146">Si aparece esta consulta generar un interbloqueo, asegúrese de que no hay ninguna aplicación conectada a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4be4d-146">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>


<span data-ttu-id="4be4d-147">Si ha habilitado la traza, los seguimientos también mostrará si Service Broker está habilitado.</span><span class="sxs-lookup"><span data-stu-id="4be4d-147">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="4be4d-148">Crear una aplicación de SignalR</span><span class="sxs-lookup"><span data-stu-id="4be4d-148">Create a SignalR Application</span></span>

<span data-ttu-id="4be4d-149">Crear una aplicación de SignalR, siga cualquiera de estos tutoriales:</span><span class="sxs-lookup"><span data-stu-id="4be4d-149">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="4be4d-150">Introducción a SignalR 2.0</span><span class="sxs-lookup"><span data-stu-id="4be4d-150">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="4be4d-151">Introducción a SignalR 2.0 y MVC 5</span><span class="sxs-lookup"><span data-stu-id="4be4d-151">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="4be4d-152">A continuación, modificaremos la aplicación de chat para admitir la ampliación con SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4be4d-152">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="4be4d-153">En primer lugar, agregue el paquete de SignalR.SqlServer NuGet al proyecto.</span><span class="sxs-lookup"><span data-stu-id="4be4d-153">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="4be4d-154">En Visual Studio, desde la **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="4be4d-154">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="4be4d-155">En la ventana de la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="4be4d-155">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="4be4d-156">A continuación, abra el archivo de Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="4be4d-156">Next, open the Startup.cs file.</span></span> <span data-ttu-id="4be4d-157">Agregue el código siguiente a la **configurar** método:</span><span class="sxs-lookup"><span data-stu-id="4be4d-157">Add the following code to the **Configure** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="4be4d-158">Implementar y ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="4be4d-158">Deploy and Run the Application</span></span>

<span data-ttu-id="4be4d-159">Preparar las instancias del servidor de Windows para implementar la aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="4be4d-159">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="4be4d-160">Agregue el rol IIS.</span><span class="sxs-lookup"><span data-stu-id="4be4d-160">Add the IIS role.</span></span> <span data-ttu-id="4be4d-161">Incluye características de "Desarrollo de aplicaciones", incluido el protocolo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="4be4d-161">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="4be4d-162">También incluye el servicio de administración (aparecen en "Herramientas de administración").</span><span class="sxs-lookup"><span data-stu-id="4be4d-162">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="4be4d-163">**Instalar Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="4be4d-163">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="4be4d-164">Cuando se ejecuta el Administrador de IIS, se le pedirá que instale la plataforma Web de Microsoft, o bien puede [descargar el intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="4be4d-164">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="4be4d-165">En el instalador de plataforma, buscar Web Deploy e instalar Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="4be4d-165">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="4be4d-166">Compruebe que se está ejecutando el servicio de administración de Web.</span><span class="sxs-lookup"><span data-stu-id="4be4d-166">Check that the Web Management Service is running.</span></span> <span data-ttu-id="4be4d-167">Si no es así, inicie el servicio.</span><span class="sxs-lookup"><span data-stu-id="4be4d-167">If not, start the service.</span></span> <span data-ttu-id="4be4d-168">(Si no ve el servicio de administración Web en la lista de servicios de Windows, asegúrese de que instala el servicio de administración cuando se agrega el rol IIS.)</span><span class="sxs-lookup"><span data-stu-id="4be4d-168">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="4be4d-169">Por último, abrir el puerto 8172 para TCP.</span><span class="sxs-lookup"><span data-stu-id="4be4d-169">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="4be4d-170">Éste es el puerto que utiliza la herramienta de implementación Web.</span><span class="sxs-lookup"><span data-stu-id="4be4d-170">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="4be4d-171">Ahora está listo para implementar el proyecto de Visual Studio desde el equipo de desarrollo en el servidor.</span><span class="sxs-lookup"><span data-stu-id="4be4d-171">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="4be4d-172">En el Explorador de soluciones, haga clic en la solución y haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="4be4d-172">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="4be4d-173">Para obtener información más detallada acerca de la implementación de web, consulte [mapa de contenido de implementación Web para Visual Studio y ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="4be4d-173">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="4be4d-174">Si implementa la aplicación en dos servidores, puede abrir cada instancia en otra ventana del explorador y vea que reciban mensajes de SignalR desde la otra.</span><span class="sxs-lookup"><span data-stu-id="4be4d-174">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="4be4d-175">(Por supuesto, en un entorno de producción, los dos servidores se colocan detrás de un equilibrador de carga.)</span><span class="sxs-lookup"><span data-stu-id="4be4d-175">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="4be4d-176">Después de ejecutar la aplicación, puede ver que SignalR crea automáticamente tablas en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="4be4d-176">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="4be4d-177">SignalR administra las tablas.</span><span class="sxs-lookup"><span data-stu-id="4be4d-177">SignalR manages the tables.</span></span> <span data-ttu-id="4be4d-178">Siempre y cuando se implementa la aplicación, no eliminar filas, modificar la tabla y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="4be4d-178">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
