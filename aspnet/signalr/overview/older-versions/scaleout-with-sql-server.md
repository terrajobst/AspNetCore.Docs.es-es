---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: Escalabilidad horizontal de SignalR con SQL Server (SignalR 1.x) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 05/01/2013
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: be4adf7a5e8af603d64bb26d97e0149f40fb92a3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810374"
---
<a name="signalr-scaleout-with-sql-server-signalr-1x"></a><span data-ttu-id="d613f-102">Escalabilidad horizontal de SignalR con SQL Server (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="d613f-102">SignalR Scaleout with SQL Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="d613f-103">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="d613f-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="d613f-104">En este tutorial, usará SQL Server para distribuir los mensajes a través de una aplicación de SignalR que se implementa en dos instancias independientes de IIS.</span><span class="sxs-lookup"><span data-stu-id="d613f-104">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="d613f-105">También puede ejecutar este tutorial en un equipo de prueba único, pero para obtener el efecto completo, deberá implementar la aplicación de SignalR en dos o más servidores.</span><span class="sxs-lookup"><span data-stu-id="d613f-105">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="d613f-106">También debe instalar a SQL Server en uno de los servidores o en un servidor dedicado independiente.</span><span class="sxs-lookup"><span data-stu-id="d613f-106">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="d613f-107">Otra opción consiste en ejecutar el tutorial sobre el uso de máquinas virtuales en Azure.</span><span class="sxs-lookup"><span data-stu-id="d613f-107">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="d613f-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="d613f-108">Prerequisites</span></span>

<span data-ttu-id="d613f-109">Microsoft SQL Server 2005 o posterior.</span><span class="sxs-lookup"><span data-stu-id="d613f-109">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="d613f-110">El backplane es compatible con las ediciones de escritorio y servidores de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d613f-110">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="d613f-111">No es compatible con Azure SQL Database o SQL Server Compact Edition.</span><span class="sxs-lookup"><span data-stu-id="d613f-111">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="d613f-112">(Si la aplicación está hospedada en Azure, considere el backplane de Service Bus en su lugar.)</span><span class="sxs-lookup"><span data-stu-id="d613f-112">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="d613f-113">Información general</span><span class="sxs-lookup"><span data-stu-id="d613f-113">Overview</span></span>

<span data-ttu-id="d613f-114">Antes de entrar en el tutorial detallado, le presentamos una introducción rápida de lo que hará.</span><span class="sxs-lookup"><span data-stu-id="d613f-114">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="d613f-115">Cree una nueva base de datos vacía.</span><span class="sxs-lookup"><span data-stu-id="d613f-115">Create a new empty database.</span></span> <span data-ttu-id="d613f-116">El backplane creará las tablas necesarias en esta base de datos.</span><span class="sxs-lookup"><span data-stu-id="d613f-116">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="d613f-117">Agregue estos paquetes de NuGet para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="d613f-117">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="d613f-118">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="d613f-118">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="d613f-119">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="d613f-119">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="d613f-120">Cree una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="d613f-120">Create a SignalR application.</span></span>
4. <span data-ttu-id="d613f-121">Agregue el código siguiente en Global.asax para configurar la placa posterior:</span><span class="sxs-lookup"><span data-stu-id="d613f-121">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a><span data-ttu-id="d613f-122">Configurar la base de datos</span><span class="sxs-lookup"><span data-stu-id="d613f-122">Configure the Database</span></span>

<span data-ttu-id="d613f-123">Decidir si la aplicación utilizará autenticación de Windows o autenticación de SQL Server para tener acceso a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d613f-123">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="d613f-124">No obstante, asegúrese de que el usuario de base de datos tiene permisos para iniciar sesión, crear esquemas y crear tablas.</span><span class="sxs-lookup"><span data-stu-id="d613f-124">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="d613f-125">Cree una nueva base de datos para la placa posterior usar.</span><span class="sxs-lookup"><span data-stu-id="d613f-125">Create a new database for the backplane to use.</span></span> <span data-ttu-id="d613f-126">Puede dar cualquier nombre a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d613f-126">You can give the database any name.</span></span> <span data-ttu-id="d613f-127">No es necesario crear ninguna tabla en la base de datos; el backplane creará las tablas necesarias.</span><span class="sxs-lookup"><span data-stu-id="d613f-127">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="d613f-128">Habilitar Service Broker</span><span class="sxs-lookup"><span data-stu-id="d613f-128">Enable Service Broker</span></span>

<span data-ttu-id="d613f-129">Se recomienda para habilitar a Service Broker para la base de datos de backplane.</span><span class="sxs-lookup"><span data-stu-id="d613f-129">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="d613f-130">Service Broker proporciona compatibilidad nativa para la mensajería y puesta en cola en SQL Server, que permite el backplane recibir actualizaciones de forma más eficaz.</span><span class="sxs-lookup"><span data-stu-id="d613f-130">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="d613f-131">(Sin embargo, el backplane también funciona sin el Service Broker).</span><span class="sxs-lookup"><span data-stu-id="d613f-131">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="d613f-132">Para comprobar si está habilitado Service Broker, consulta el **es\_broker\_habilitado** columna en el **sys.databases** vista de catálogo.</span><span class="sxs-lookup"><span data-stu-id="d613f-132">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="d613f-133">Para habilitar a Service Broker, use la siguiente consulta SQL:</span><span class="sxs-lookup"><span data-stu-id="d613f-133">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="d613f-134">Si esta consulta se parece a un interbloqueo, asegúrese de que no hay ninguna aplicación conectada a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d613f-134">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>


<span data-ttu-id="d613f-135">Si ha habilitado el seguimiento, los seguimientos se mostrarán también si Service Broker está habilitado.</span><span class="sxs-lookup"><span data-stu-id="d613f-135">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="d613f-136">Crear una aplicación de SignalR</span><span class="sxs-lookup"><span data-stu-id="d613f-136">Create a SignalR Application</span></span>

<span data-ttu-id="d613f-137">Cree una aplicación de SignalR con cualquiera de estos tutoriales:</span><span class="sxs-lookup"><span data-stu-id="d613f-137">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="d613f-138">Introducción a SignalR</span><span class="sxs-lookup"><span data-stu-id="d613f-138">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="d613f-139">Introducción a SignalR y MVC 4</span><span class="sxs-lookup"><span data-stu-id="d613f-139">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="d613f-140">A continuación, modificaremos la aplicación de chat para admitir la escalabilidad horizontal con SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d613f-140">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="d613f-141">En primer lugar, agregue el paquete de SignalR.SqlServer NuGet al proyecto.</span><span class="sxs-lookup"><span data-stu-id="d613f-141">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="d613f-142">En Visual Studio, desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="d613f-142">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="d613f-143">En la ventana de consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="d613f-143">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="d613f-144">A continuación, abra el archivo Global.asax.</span><span class="sxs-lookup"><span data-stu-id="d613f-144">Next, open the Global.asax file.</span></span> <span data-ttu-id="d613f-145">Agregue el código siguiente a la **aplicación\_iniciar** método:</span><span class="sxs-lookup"><span data-stu-id="d613f-145">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="d613f-146">Implemente y ejecute la aplicación</span><span class="sxs-lookup"><span data-stu-id="d613f-146">Deploy and Run the Application</span></span>

<span data-ttu-id="d613f-147">Preparar las instancias de Windows Server para implementar la aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="d613f-147">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="d613f-148">Agregue el rol IIS.</span><span class="sxs-lookup"><span data-stu-id="d613f-148">Add the IIS role.</span></span> <span data-ttu-id="d613f-149">Incluye características de "Desarrollo de aplicaciones", incluido el protocolo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d613f-149">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="d613f-150">También incluyen el servicio de administración (que se muestran en "Herramientas de administración").</span><span class="sxs-lookup"><span data-stu-id="d613f-150">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="d613f-151">**Instalar Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="d613f-151">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="d613f-152">Cuando se ejecuta el Administrador de IIS, se le pedirá que instale la plataforma Web de Microsoft, o bien puede [descargar el intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="d613f-152">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="d613f-153">En el instalador de plataforma, buscar Web Deploy e instalar Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="d613f-153">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="d613f-154">Compruebe que se está ejecutando el servicio de administración Web.</span><span class="sxs-lookup"><span data-stu-id="d613f-154">Check that the Web Management Service is running.</span></span> <span data-ttu-id="d613f-155">Si no es así, inicie el servicio.</span><span class="sxs-lookup"><span data-stu-id="d613f-155">If not, start the service.</span></span> <span data-ttu-id="d613f-156">(Si no ve el servicio de administración en la lista de servicios de Windows, asegúrese de que instaló el servicio de administración cuando se agrega el rol de IIS.)</span><span class="sxs-lookup"><span data-stu-id="d613f-156">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="d613f-157">Por último, abra el puerto 8172 para TCP.</span><span class="sxs-lookup"><span data-stu-id="d613f-157">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="d613f-158">Este es el puerto que utiliza la herramienta Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="d613f-158">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="d613f-159">Ahora está listo para implementar el proyecto de Visual Studio desde el equipo de desarrollo para el servidor.</span><span class="sxs-lookup"><span data-stu-id="d613f-159">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="d613f-160">En el Explorador de soluciones, haga clic en la solución y haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="d613f-160">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="d613f-161">Para obtener información más detallada sobre la implementación web, consulte [mapa de contenido de implementación Web para Visual Studio y ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="d613f-161">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="d613f-162">Si implementa la aplicación en dos servidores, puede abrir cada instancia en otra ventana del explorador y vea que reciban mensajes de SignalR desde la otra.</span><span class="sxs-lookup"><span data-stu-id="d613f-162">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="d613f-163">(Por supuesto, en un entorno de producción, los dos servidores sentaba detrás de un equilibrador de carga.)</span><span class="sxs-lookup"><span data-stu-id="d613f-163">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="d613f-164">Después de ejecutar la aplicación, puede ver que SignalR crea automáticamente las tablas en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="d613f-164">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="d613f-165">SignalR administra las tablas.</span><span class="sxs-lookup"><span data-stu-id="d613f-165">SignalR manages the tables.</span></span> <span data-ttu-id="d613f-166">Siempre y cuando se implementa la aplicación, no eliminar filas, modificar la tabla y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="d613f-166">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
