---
uid: signalr/overview/performance/scaleout-with-sql-server
title: Escalabilidad horizontal de SignalR con SQL Server | Microsoft Docs
author: MikeWasson
description: Las versiones de software usan en este tema Visual Studio 2013 .NET 4.5 SignalR las versiones anteriores de la versión 2 de este tema para obtener información acerca de las versiones anteriores de...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 73b16ecf141732f0adc362dc5167073491cc2dea
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287018"
---
<a name="signalr-scaleout-with-sql-server"></a><span data-ttu-id="f3493-103">Escalabilidad horizontal de SignalR con SQL Server</span><span class="sxs-lookup"><span data-stu-id="f3493-103">SignalR Scaleout with SQL Server</span></span>
====================
<span data-ttu-id="f3493-104">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="f3493-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="f3493-105">Versiones de software que se usa en este tema</span><span class="sxs-lookup"><span data-stu-id="f3493-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="f3493-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f3493-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="f3493-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="f3493-107">.NET 4.5</span></span>
> - <span data-ttu-id="f3493-108">Versión 2 de SignalR</span><span class="sxs-lookup"><span data-stu-id="f3493-108">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="f3493-109">Versiones anteriores de este tema.</span><span class="sxs-lookup"><span data-stu-id="f3493-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="f3493-110">Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="f3493-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="f3493-111">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="f3493-111">Questions and comments</span></span>
>
> <span data-ttu-id="f3493-112">Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="f3493-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="f3493-113">Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="f3493-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="f3493-114">En este tutorial, usará SQL Server para distribuir los mensajes a través de una aplicación de SignalR que se implementa en dos instancias independientes de IIS.</span><span class="sxs-lookup"><span data-stu-id="f3493-114">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="f3493-115">También puede ejecutar este tutorial en un equipo de prueba único, pero para obtener el efecto completo, deberá implementar la aplicación de SignalR en dos o más servidores.</span><span class="sxs-lookup"><span data-stu-id="f3493-115">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="f3493-116">También debe instalar a SQL Server en uno de los servidores o en un servidor dedicado independiente.</span><span class="sxs-lookup"><span data-stu-id="f3493-116">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="f3493-117">Otra opción consiste en ejecutar el tutorial sobre el uso de máquinas virtuales en Azure.</span><span class="sxs-lookup"><span data-stu-id="f3493-117">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="f3493-118">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="f3493-118">Prerequisites</span></span>

<span data-ttu-id="f3493-119">Microsoft SQL Server 2005 o posterior.</span><span class="sxs-lookup"><span data-stu-id="f3493-119">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="f3493-120">El backplane es compatible con las ediciones de escritorio y servidores de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f3493-120">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="f3493-121">No es compatible con Azure SQL Database o SQL Server Compact Edition.</span><span class="sxs-lookup"><span data-stu-id="f3493-121">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="f3493-122">(Si la aplicación está hospedada en Azure, considere el backplane de Service Bus en su lugar.)</span><span class="sxs-lookup"><span data-stu-id="f3493-122">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="f3493-123">Información general</span><span class="sxs-lookup"><span data-stu-id="f3493-123">Overview</span></span>

<span data-ttu-id="f3493-124">Antes de entrar en el tutorial detallado, le presentamos una introducción rápida de lo que hará.</span><span class="sxs-lookup"><span data-stu-id="f3493-124">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="f3493-125">Cree una nueva base de datos vacía.</span><span class="sxs-lookup"><span data-stu-id="f3493-125">Create a new empty database.</span></span> <span data-ttu-id="f3493-126">El backplane creará las tablas necesarias en esta base de datos.</span><span class="sxs-lookup"><span data-stu-id="f3493-126">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="f3493-127">Agregue estos paquetes de NuGet para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="f3493-127">Add these NuGet packages to your application:</span></span>

    - [<span data-ttu-id="f3493-128">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="f3493-128">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="f3493-129">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="f3493-129">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="f3493-130">Cree una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="f3493-130">Create a SignalR application.</span></span>
4. <span data-ttu-id="f3493-131">Agregue el siguiente código en Startup.cs, para configurar la placa posterior:</span><span class="sxs-lookup"><span data-stu-id="f3493-131">Add the following code to Startup.cs to configure the backplane:</span></span>

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   <span data-ttu-id="f3493-132">Este código configura el backplane con los valores predeterminados de [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) y [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="f3493-132">This code configures the backplane with the default values for [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="f3493-133">Para obtener información acerca de cómo cambiar estos valores, vea [rendimiento de SignalR: Las métricas de escalado horizontal](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="f3493-133">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

## <a name="configure-the-database"></a><span data-ttu-id="f3493-134">Configurar la base de datos</span><span class="sxs-lookup"><span data-stu-id="f3493-134">Configure the Database</span></span>

<span data-ttu-id="f3493-135">Decidir si la aplicación utilizará autenticación de Windows o autenticación de SQL Server para tener acceso a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f3493-135">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="f3493-136">No obstante, asegúrese de que el usuario de base de datos tiene permisos para iniciar sesión, crear esquemas y crear tablas.</span><span class="sxs-lookup"><span data-stu-id="f3493-136">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="f3493-137">Cree una nueva base de datos para la placa posterior usar.</span><span class="sxs-lookup"><span data-stu-id="f3493-137">Create a new database for the backplane to use.</span></span> <span data-ttu-id="f3493-138">Puede dar cualquier nombre a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f3493-138">You can give the database any name.</span></span> <span data-ttu-id="f3493-139">No es necesario crear ninguna tabla en la base de datos; el backplane creará las tablas necesarias.</span><span class="sxs-lookup"><span data-stu-id="f3493-139">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="f3493-140">Habilitar Service Broker</span><span class="sxs-lookup"><span data-stu-id="f3493-140">Enable Service Broker</span></span>

<span data-ttu-id="f3493-141">Se recomienda para habilitar a Service Broker para la base de datos de backplane.</span><span class="sxs-lookup"><span data-stu-id="f3493-141">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="f3493-142">Service Broker proporciona compatibilidad nativa para la mensajería y puesta en cola en SQL Server, que permite el backplane recibir actualizaciones de forma más eficaz.</span><span class="sxs-lookup"><span data-stu-id="f3493-142">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="f3493-143">(Sin embargo, el backplane también funciona sin el Service Broker).</span><span class="sxs-lookup"><span data-stu-id="f3493-143">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="f3493-144">Para comprobar si está habilitado Service Broker, consulta el **es\_broker\_habilitado** columna en el **sys.databases** vista de catálogo.</span><span class="sxs-lookup"><span data-stu-id="f3493-144">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="f3493-145">Para habilitar a Service Broker, use la siguiente consulta SQL:</span><span class="sxs-lookup"><span data-stu-id="f3493-145">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="f3493-146">Si esta consulta se parece a un interbloqueo, asegúrese de que no hay ninguna aplicación conectada a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f3493-146">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>


<span data-ttu-id="f3493-147">Si ha habilitado el seguimiento, los seguimientos se mostrarán también si Service Broker está habilitado.</span><span class="sxs-lookup"><span data-stu-id="f3493-147">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="f3493-148">Crear una aplicación de SignalR</span><span class="sxs-lookup"><span data-stu-id="f3493-148">Create a SignalR Application</span></span>

<span data-ttu-id="f3493-149">Cree una aplicación de SignalR con cualquiera de estos tutoriales:</span><span class="sxs-lookup"><span data-stu-id="f3493-149">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="f3493-150">Introducción a SignalR 2.0</span><span class="sxs-lookup"><span data-stu-id="f3493-150">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="f3493-151">Introducción a SignalR 2.0 y MVC 5</span><span class="sxs-lookup"><span data-stu-id="f3493-151">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="f3493-152">A continuación, modificaremos la aplicación de chat para admitir la escalabilidad horizontal con SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f3493-152">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="f3493-153">En primer lugar, agregue el paquete de SignalR.SqlServer NuGet al proyecto.</span><span class="sxs-lookup"><span data-stu-id="f3493-153">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="f3493-154">En Visual Studio, desde el **herramientas** menú, seleccione **Administrador de paquetes de NuGet**, a continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="f3493-154">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="f3493-155">En la ventana de consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="f3493-155">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="f3493-156">A continuación, abra el archivo Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="f3493-156">Next, open the Startup.cs file.</span></span> <span data-ttu-id="f3493-157">Agregue el código siguiente a la **configurar** método:</span><span class="sxs-lookup"><span data-stu-id="f3493-157">Add the following code to the **Configure** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="f3493-158">Implemente y ejecute la aplicación</span><span class="sxs-lookup"><span data-stu-id="f3493-158">Deploy and Run the Application</span></span>

<span data-ttu-id="f3493-159">Preparar las instancias de Windows Server para implementar la aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="f3493-159">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="f3493-160">Agregue el rol IIS.</span><span class="sxs-lookup"><span data-stu-id="f3493-160">Add the IIS role.</span></span> <span data-ttu-id="f3493-161">Incluye características de "Desarrollo de aplicaciones", incluido el protocolo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="f3493-161">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="f3493-162">También incluyen el servicio de administración (que se muestran en "Herramientas de administración").</span><span class="sxs-lookup"><span data-stu-id="f3493-162">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="f3493-163">**Instalar Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="f3493-163">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="f3493-164">Cuando se ejecuta el Administrador de IIS, se le pedirá que instale la plataforma Web de Microsoft, o bien puede [descargar el intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="f3493-164">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="f3493-165">En el instalador de plataforma, buscar Web Deploy e instalar Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="f3493-165">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="f3493-166">Compruebe que se está ejecutando el servicio de administración Web.</span><span class="sxs-lookup"><span data-stu-id="f3493-166">Check that the Web Management Service is running.</span></span> <span data-ttu-id="f3493-167">Si no es así, inicie el servicio.</span><span class="sxs-lookup"><span data-stu-id="f3493-167">If not, start the service.</span></span> <span data-ttu-id="f3493-168">(Si no ve el servicio de administración en la lista de servicios de Windows, asegúrese de que instaló el servicio de administración cuando se agrega el rol de IIS.)</span><span class="sxs-lookup"><span data-stu-id="f3493-168">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="f3493-169">Por último, abra el puerto 8172 para TCP.</span><span class="sxs-lookup"><span data-stu-id="f3493-169">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="f3493-170">Este es el puerto que utiliza la herramienta Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="f3493-170">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="f3493-171">Ahora está listo para implementar el proyecto de Visual Studio desde el equipo de desarrollo para el servidor.</span><span class="sxs-lookup"><span data-stu-id="f3493-171">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="f3493-172">En el Explorador de soluciones, haga clic en la solución y haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="f3493-172">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="f3493-173">Para obtener información más detallada sobre la implementación web, consulte [mapa de contenido de implementación Web para Visual Studio y ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="f3493-173">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="f3493-174">Si implementa la aplicación en dos servidores, puede abrir cada instancia en otra ventana del explorador y vea que reciban mensajes de SignalR desde la otra.</span><span class="sxs-lookup"><span data-stu-id="f3493-174">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="f3493-175">(Por supuesto, en un entorno de producción, los dos servidores sentaba detrás de un equilibrador de carga.)</span><span class="sxs-lookup"><span data-stu-id="f3493-175">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="f3493-176">Después de ejecutar la aplicación, puede ver que SignalR crea automáticamente las tablas en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="f3493-176">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="f3493-177">SignalR administra las tablas.</span><span class="sxs-lookup"><span data-stu-id="f3493-177">SignalR manages the tables.</span></span> <span data-ttu-id="f3493-178">Siempre y cuando se implementa la aplicación, no eliminar filas, modificar la tabla y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="f3493-178">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
