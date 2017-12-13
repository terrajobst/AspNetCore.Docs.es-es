---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: "Ampliación de SignalR con SQL Server (SignalR 1.x) | Documentos de Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 7a589f5c6a5196444c3c616b39f501f3a7512471
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-sql-server-signalr-1x"></a><span data-ttu-id="d8af2-102">Ampliación de SignalR con SQL Server (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="d8af2-102">SignalR Scaleout with SQL Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="d8af2-103">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="d8af2-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="d8af2-104">En este tutorial, utilizará SQL Server para distribuir mensajes en una aplicación de SignalR que se implementa en dos instancias independientes de IIS.</span><span class="sxs-lookup"><span data-stu-id="d8af2-104">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="d8af2-105">También puede ejecutar este tutorial en un equipo de prueba individual, pero para obtener el efecto completo, debe implementar la aplicación de SignalR en dos o más servidores.</span><span class="sxs-lookup"><span data-stu-id="d8af2-105">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="d8af2-106">También debe instalar a SQL Server en uno de los servidores o en un servidor dedicado independiente.</span><span class="sxs-lookup"><span data-stu-id="d8af2-106">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="d8af2-107">Otra opción consiste en ejecutar el tutorial usando máquinas virtuales en Azure.</span><span class="sxs-lookup"><span data-stu-id="d8af2-107">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="d8af2-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="d8af2-108">Prerequisites</span></span>

<span data-ttu-id="d8af2-109">Microsoft SQL Server 2005 o posterior.</span><span class="sxs-lookup"><span data-stu-id="d8af2-109">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="d8af2-110">El backplane es compatible con las ediciones de escritorio y servidores de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d8af2-110">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="d8af2-111">No es compatible con SQL Server Compact Edition o base de datos de SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="d8af2-111">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="d8af2-112">(Si la aplicación se hospeda en Azure, se recomienda el backplane de Bus de servicio en su lugar.)</span><span class="sxs-lookup"><span data-stu-id="d8af2-112">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="d8af2-113">Información general</span><span class="sxs-lookup"><span data-stu-id="d8af2-113">Overview</span></span>

<span data-ttu-id="d8af2-114">Antes de entrar en el tutorial detallado, presentamos una introducción rápida de lo va a hacer.</span><span class="sxs-lookup"><span data-stu-id="d8af2-114">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="d8af2-115">Crear una nueva base de datos vacía.</span><span class="sxs-lookup"><span data-stu-id="d8af2-115">Create a new empty database.</span></span> <span data-ttu-id="d8af2-116">El backplane creará las tablas necesarias en esta base de datos.</span><span class="sxs-lookup"><span data-stu-id="d8af2-116">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="d8af2-117">Agregue estos paquetes de NuGet para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="d8af2-117">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="d8af2-118">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="d8af2-118">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="d8af2-119">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="d8af2-119">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="d8af2-120">Cree una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="d8af2-120">Create a SignalR application.</span></span>
4. <span data-ttu-id="d8af2-121">Agregue el código siguiente a Global.asax para configurar el plano posterior:</span><span class="sxs-lookup"><span data-stu-id="d8af2-121">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a><span data-ttu-id="d8af2-122">Configurar la base de datos</span><span class="sxs-lookup"><span data-stu-id="d8af2-122">Configure the Database</span></span>

<span data-ttu-id="d8af2-123">Decidir si la aplicación utilizará la autenticación de Windows o autenticación de SQL Server para tener acceso a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d8af2-123">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="d8af2-124">En cualquier caso, asegúrese de que el usuario de base de datos tiene permisos para iniciar sesión, crear esquemas y crear tablas.</span><span class="sxs-lookup"><span data-stu-id="d8af2-124">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="d8af2-125">Crear una nueva base de datos para el plano posterior a la utilice.</span><span class="sxs-lookup"><span data-stu-id="d8af2-125">Create a new database for the backplane to use.</span></span> <span data-ttu-id="d8af2-126">Puede utilizar cualquier nombre para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d8af2-126">You can give the database any name.</span></span> <span data-ttu-id="d8af2-127">No es necesario crear ninguna tabla en la base de datos; el backplane creará las tablas necesarias.</span><span class="sxs-lookup"><span data-stu-id="d8af2-127">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="d8af2-128">Habilitar Service Broker</span><span class="sxs-lookup"><span data-stu-id="d8af2-128">Enable Service Broker</span></span>

<span data-ttu-id="d8af2-129">Se recomienda habilitar Service Broker para la base de datos de backplane.</span><span class="sxs-lookup"><span data-stu-id="d8af2-129">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="d8af2-130">Service Broker proporciona compatibilidad nativa para la mensajería y puesta en cola en SQL Server, lo que permite el backplane recibir actualizaciones de forma más eficaz.</span><span class="sxs-lookup"><span data-stu-id="d8af2-130">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="d8af2-131">(Sin embargo, el plano posterior también funciona sin el Service Broker).</span><span class="sxs-lookup"><span data-stu-id="d8af2-131">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="d8af2-132">Para comprobar si está habilitado Service Broker, consultar la **es\_broker\_habilitado** columna en el **sys.databases** vista de catálogo.</span><span class="sxs-lookup"><span data-stu-id="d8af2-132">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="d8af2-133">Para habilitar a Service Broker, use la siguiente consulta SQL:</span><span class="sxs-lookup"><span data-stu-id="d8af2-133">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="d8af2-134">Si aparece esta consulta generar un interbloqueo, asegúrese de que no hay ninguna aplicación conectada a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d8af2-134">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>


<span data-ttu-id="d8af2-135">Si ha habilitado la traza, los seguimientos también mostrará si Service Broker está habilitado.</span><span class="sxs-lookup"><span data-stu-id="d8af2-135">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="d8af2-136">Crear una aplicación de SignalR</span><span class="sxs-lookup"><span data-stu-id="d8af2-136">Create a SignalR Application</span></span>

<span data-ttu-id="d8af2-137">Crear una aplicación de SignalR, siga cualquiera de estos tutoriales:</span><span class="sxs-lookup"><span data-stu-id="d8af2-137">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="d8af2-138">Introducción a SignalR</span><span class="sxs-lookup"><span data-stu-id="d8af2-138">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="d8af2-139">Introducción a SignalR y MVC 4</span><span class="sxs-lookup"><span data-stu-id="d8af2-139">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="d8af2-140">A continuación, modificaremos la aplicación de chat para admitir la ampliación con SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d8af2-140">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="d8af2-141">En primer lugar, agregue el paquete de SignalR.SqlServer NuGet al proyecto.</span><span class="sxs-lookup"><span data-stu-id="d8af2-141">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="d8af2-142">En Visual Studio, desde la **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="d8af2-142">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="d8af2-143">En la ventana de la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="d8af2-143">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="d8af2-144">A continuación, abra el archivo Global.asax.</span><span class="sxs-lookup"><span data-stu-id="d8af2-144">Next, open the Global.asax file.</span></span> <span data-ttu-id="d8af2-145">Agregue el código siguiente a la **aplicación\_iniciar** método:</span><span class="sxs-lookup"><span data-stu-id="d8af2-145">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="d8af2-146">Implementar y ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="d8af2-146">Deploy and Run the Application</span></span>

<span data-ttu-id="d8af2-147">Preparar las instancias del servidor de Windows para implementar la aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="d8af2-147">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="d8af2-148">Agregue el rol IIS.</span><span class="sxs-lookup"><span data-stu-id="d8af2-148">Add the IIS role.</span></span> <span data-ttu-id="d8af2-149">Incluye características de "Desarrollo de aplicaciones", incluido el protocolo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d8af2-149">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="d8af2-150">También incluye el servicio de administración (aparecen en "Herramientas de administración").</span><span class="sxs-lookup"><span data-stu-id="d8af2-150">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="d8af2-151">**Instalar Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="d8af2-151">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="d8af2-152">Cuando se ejecuta el Administrador de IIS, se le pedirá que instale la plataforma Web de Microsoft, o bien puede [descargar el intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="d8af2-152">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="d8af2-153">En el instalador de plataforma, buscar Web Deploy e instalar Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="d8af2-153">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="d8af2-154">Compruebe que se está ejecutando el servicio de administración de Web.</span><span class="sxs-lookup"><span data-stu-id="d8af2-154">Check that the Web Management Service is running.</span></span> <span data-ttu-id="d8af2-155">Si no es así, inicie el servicio.</span><span class="sxs-lookup"><span data-stu-id="d8af2-155">If not, start the service.</span></span> <span data-ttu-id="d8af2-156">(Si no ve el servicio de administración Web en la lista de servicios de Windows, asegúrese de que instala el servicio de administración cuando se agrega el rol IIS.)</span><span class="sxs-lookup"><span data-stu-id="d8af2-156">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="d8af2-157">Por último, abrir el puerto 8172 para TCP.</span><span class="sxs-lookup"><span data-stu-id="d8af2-157">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="d8af2-158">Éste es el puerto que utiliza la herramienta de implementación Web.</span><span class="sxs-lookup"><span data-stu-id="d8af2-158">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="d8af2-159">Ahora está listo para implementar el proyecto de Visual Studio desde el equipo de desarrollo en el servidor.</span><span class="sxs-lookup"><span data-stu-id="d8af2-159">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="d8af2-160">En el Explorador de soluciones, haga clic en la solución y haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="d8af2-160">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="d8af2-161">Para obtener información más detallada acerca de la implementación de web, consulte [mapa de contenido de implementación Web para Visual Studio y ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="d8af2-161">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="d8af2-162">Si implementa la aplicación en dos servidores, puede abrir cada instancia en otra ventana del explorador y vea que reciban mensajes de SignalR desde la otra.</span><span class="sxs-lookup"><span data-stu-id="d8af2-162">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="d8af2-163">(Por supuesto, en un entorno de producción, los dos servidores se colocan detrás de un equilibrador de carga.)</span><span class="sxs-lookup"><span data-stu-id="d8af2-163">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="d8af2-164">Después de ejecutar la aplicación, puede ver que SignalR crea automáticamente tablas en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="d8af2-164">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="d8af2-165">SignalR administra las tablas.</span><span class="sxs-lookup"><span data-stu-id="d8af2-165">SignalR manages the tables.</span></span> <span data-ttu-id="d8af2-166">Siempre y cuando se implementa la aplicación, no eliminar filas, modificar la tabla y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="d8af2-166">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
