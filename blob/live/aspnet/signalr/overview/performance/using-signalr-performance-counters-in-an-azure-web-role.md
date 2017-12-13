---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Mediante los contadores de rendimiento de SignalR en un rol Web de Azure | Documentos de Microsoft
author: guardrex
description: "Cómo instalar y utilizar contadores de rendimiento de SignalR en un rol Web de Azure."
keywords: Contador de ASP.NET,signalr,performance, rol web de azure
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2017
ms.topic: article
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 0d2717eb318d282e21e9aa8622a205f556e3a4ee
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="63f41-104">Uso de contadores de rendimiento de SignalR en un rol Web de Azure</span><span class="sxs-lookup"><span data-stu-id="63f41-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="63f41-105">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="63f41-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="63f41-106">Contadores de rendimiento de SignalR se utilizan para supervisar el rendimiento de la aplicación en un rol Web de Azure.</span><span class="sxs-lookup"><span data-stu-id="63f41-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="63f41-107">Los contadores se capturan por diagnósticos de Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="63f41-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="63f41-108">Instalar los contadores de rendimiento de SignalR en Azure con *signalr.exe*, la misma herramienta que se usa para aplicaciones independientes o de forma local.</span><span class="sxs-lookup"><span data-stu-id="63f41-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="63f41-109">Puesto que las funciones de Azure son transitorias, configurar una aplicación para instalar y registrar los contadores de rendimiento de SignalR durante el inicio.</span><span class="sxs-lookup"><span data-stu-id="63f41-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="63f41-110">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="63f41-110">Prerequisites</span></span>

* [<span data-ttu-id="63f41-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="63f41-111">Visual Studio 2015</span></span>](https://www.visualstudio.com/vs/visual-studio-express/)
* <span data-ttu-id="63f41-112">[SDK de Microsoft Azure para Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Nota: reiniciar el equipo después de instalar el SDK.**</span><span class="sxs-lookup"><span data-stu-id="63f41-112">[Microsoft Azure SDK for Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="63f41-113">Suscripción de Microsoft Azure: para suscribirse a una cuenta de evaluación de Azure gratuita, consulte [prueba gratuita de Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="63f41-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="63f41-114">Crear una aplicación de rol Web de Azure que expone los contadores de rendimiento de SignalR</span><span class="sxs-lookup"><span data-stu-id="63f41-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="63f41-115">Abra Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="63f41-115">Open Visual Studio 2015.</span></span>

2. <span data-ttu-id="63f41-116">En Visual Studio 2015, seleccione **archivo &gt; New &gt; proyecto**.</span><span class="sxs-lookup"><span data-stu-id="63f41-116">In Visual Studio 2015, select **File &gt; New &gt; Project**.</span></span>

3. <span data-ttu-id="63f41-117">En el **plantillas** panel de la **nuevo proyecto** ventana bajo la **Visual C#** nodo, seleccione el **nube** nodo y seleccione el **Servicio de nube de azure** plantilla.</span><span class="sxs-lookup"><span data-stu-id="63f41-117">In the **Templates** pane of the **New Project** window under the **Visual C#** node, select the **Cloud** node and select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="63f41-118">Nombre de la aplicación **SignalRPerfCounters** y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="63f41-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![Nueva aplicación de nube](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. <span data-ttu-id="63f41-120">En el **nuevo servicio de nube de Microsoft Azure** cuadro de diálogo, seleccione **rol Web de ASP.NET** y seleccione la  **&gt;**  botón para agregar el rol para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="63f41-120">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the **&gt;** button to add the role to the project.</span></span> <span data-ttu-id="63f41-121">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="63f41-121">Select **OK**.</span></span>

   ![Agregar rol Web de ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. <span data-ttu-id="63f41-123">En el **nueva aplicación Web de ASP.NET: WebRole1** cuadro de diálogo, seleccione la **MVC** plantilla y, a continuación, seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="63f41-123">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![Agregar API Web y MVC](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. <span data-ttu-id="63f41-125">En **el Explorador de soluciones**, abra el *diagnostics.wadcfgx* de archivos en **WebRole1**.</span><span class="sxs-lookup"><span data-stu-id="63f41-125">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![Diagnostics.wadcfgx del explorador de soluciones](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. <span data-ttu-id="63f41-127">Reemplace el contenido del archivo con la siguiente configuración y guarde el archivo:</span><span class="sxs-lookup"><span data-stu-id="63f41-127">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. <span data-ttu-id="63f41-128">Abra la **consola de administrador de paquetes** de **herramientas &gt; Administrador de paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="63f41-128">Open the **Package Manager Console** from **Tools &gt; NuGet Package Manager**.</span></span> <span data-ttu-id="63f41-129">Escriba los comandos siguientes para instalar la versión más reciente de SignalR y el paquete de utilidades de SignalR:</span><span class="sxs-lookup"><span data-stu-id="63f41-129">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. <span data-ttu-id="63f41-130">Configurar la aplicación para instalar los contadores de rendimiento de SignalR en la instancia de rol cuando inicia o se recicla.</span><span class="sxs-lookup"><span data-stu-id="63f41-130">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="63f41-131">En **el Explorador de soluciones**, haga doble clic en el **WebRole1** de proyecto y seleccione **agregar &gt; nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="63f41-131">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add &gt; New Folder**.</span></span> <span data-ttu-id="63f41-132">Nombre de la nueva carpeta *inicio*.</span><span class="sxs-lookup"><span data-stu-id="63f41-132">Name the new folder *Startup*.</span></span>

   ![Agregar carpeta de inicio](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. <span data-ttu-id="63f41-134">Copia la *signalr.exe* archivo (agregado con la **Microsoft.AspNet.SignalR.Utils** paquete) de  **&lt;carpeta de proyecto&gt;\SignalRPerfCounters\packages\ Microsoft.AspNet.SignalR.Utils. &lt;versión&gt;\tools** a la *inicio* carpeta que creó en el paso anterior.</span><span class="sxs-lookup"><span data-stu-id="63f41-134">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from **&lt;project folder&gt;\SignalRPerfCounters\packages\Microsoft.AspNet.SignalR.Utils.&lt;version&gt;\tools** to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="63f41-135">En **el Explorador de soluciones**, haga clic en el *inicio* carpeta y seleccione **agregar &gt; elemento existente**.</span><span class="sxs-lookup"><span data-stu-id="63f41-135">In **Solution Explorer**, right-click the *Startup* folder and select **Add &gt; Existing Item**.</span></span> <span data-ttu-id="63f41-136">En el cuadro de diálogo que aparece, seleccione *signalr.exe* y seleccione **agregar**.</span><span class="sxs-lookup"><span data-stu-id="63f41-136">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    ![Agregue signalr.exe al proyecto](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. <span data-ttu-id="63f41-138">Haga doble clic en el *inicio* carpeta que ha creado.</span><span class="sxs-lookup"><span data-stu-id="63f41-138">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="63f41-139">Seleccione **agregar &gt; nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="63f41-139">Select **Add &gt; New Item**.</span></span> <span data-ttu-id="63f41-140">Seleccione el **General** nodo, seleccione **archivo de texto**y el nombre del nuevo elemento *SignalRPerfCounterInstall.cmd*.</span><span class="sxs-lookup"><span data-stu-id="63f41-140">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="63f41-141">Este archivo de comandos va a instalar los contadores de rendimiento de SignalR en el rol web.</span><span class="sxs-lookup"><span data-stu-id="63f41-141">This command file will install the SignalR performance counters into the web role.</span></span>

    ![Crear archivo de proceso por lotes de instalación de contadores de rendimiento de SignalR](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. <span data-ttu-id="63f41-143">Cuando Visual Studio crea el *SignalRPerfCounterInstall.cmd* archivo, abrirá automáticamente en la ventana principal.</span><span class="sxs-lookup"><span data-stu-id="63f41-143">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="63f41-144">Reemplace el contenido del archivo con el siguiente script, a continuación, guarde y cierre el archivo.</span><span class="sxs-lookup"><span data-stu-id="63f41-144">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="63f41-145">Este script se ejecuta *signalr.exe*, que agrega los contadores de rendimiento de SignalR a la instancia de rol.</span><span class="sxs-lookup"><span data-stu-id="63f41-145">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. <span data-ttu-id="63f41-146">Seleccione el *signalr.exe* en el archivo **el Explorador de soluciones**.</span><span class="sxs-lookup"><span data-stu-id="63f41-146">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="63f41-147">En el archivo **propiedades**, establezca **copiar en el directorio de salida** a **copiar siempre**.</span><span class="sxs-lookup"><span data-stu-id="63f41-147">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![Establezca copiar en directorio de salida para copiar siempre](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. <span data-ttu-id="63f41-149">Repita el paso anterior para el *SignalRPerfCounterInstall.cmd* archivo.</span><span class="sxs-lookup"><span data-stu-id="63f41-149">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>

    
16. <span data-ttu-id="63f41-150">Haga doble clic en el *SignalRPerfCounterInstall.cmd* de archivos y seleccione **abrir con**.</span><span class="sxs-lookup"><span data-stu-id="63f41-150">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="63f41-151">En el cuadro de diálogo que aparece, seleccione **Editor binario** y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="63f41-151">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![Abrir con el Editor binario](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. <span data-ttu-id="63f41-153">En el editor binario, seleccione los bytes iniciales en el archivo y eliminarlos.</span><span class="sxs-lookup"><span data-stu-id="63f41-153">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="63f41-154">Guarde y cierre el archivo.</span><span class="sxs-lookup"><span data-stu-id="63f41-154">Save and close the file.</span></span>

    ![Eliminar bytes iniciales](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. <span data-ttu-id="63f41-156">Abra *ServiceDefinition.csdef* y agregue una tarea de inicio que se ejecuta el *SignalrPerfCounterInstall.cmd* cuando se inicia el servicio de archivos:</span><span class="sxs-lookup"><span data-stu-id="63f41-156">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. <span data-ttu-id="63f41-157">Abra `Views/Shared/_Layout.cshtml` y quitar la secuencia de comandos de agrupación de jQuery desde el final del archivo.</span><span class="sxs-lookup"><span data-stu-id="63f41-157">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. <span data-ttu-id="63f41-158">Agregar un cliente de JavaScript que continuamente llama el `increment` método en el servidor.</span><span class="sxs-lookup"><span data-stu-id="63f41-158">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="63f41-159">Abra `Views/Home/Index.cshtml` y reemplace el contenido con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="63f41-159">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. <span data-ttu-id="63f41-160">Crear una nueva carpeta en el **WebRole1** proyecto denominado *concentradores*.</span><span class="sxs-lookup"><span data-stu-id="63f41-160">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="63f41-161">Haga clic en el *concentradores* carpeta **el Explorador de soluciones**, seleccione **Web &gt; SignalR**y seleccione **clase de concentrador de SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="63f41-161">Right-click the *Hubs* folder in **Solution Explorer**, select **Web &gt; SignalR**, and select **SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="63f41-162">Nombre del nuevo centro de *MyHub.cs* y seleccione **agregar**.</span><span class="sxs-lookup"><span data-stu-id="63f41-162">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![Agregar clase de concentrador de SignalR a la carpeta de bases de datos centrales en el cuadro de diálogo Agregar nuevo elemento](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="63f41-164">*MyHub.cs* se abrirá automáticamente en la ventana principal.</span><span class="sxs-lookup"><span data-stu-id="63f41-164">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="63f41-165">Reemplace el contenido con el código siguiente, a continuación, guarde y cierre el archivo:</span><span class="sxs-lookup"><span data-stu-id="63f41-165">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. <span data-ttu-id="63f41-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)*  es una densidad de conexión proporcionada con el código de base de SignalR de herramienta de prueba.</span><span class="sxs-lookup"><span data-stu-id="63f41-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="63f41-167">Puesto que cigüeñal requiere una conexión persistente, agrega uno a su sitio para su uso cuando se prueba.</span><span class="sxs-lookup"><span data-stu-id="63f41-167">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="63f41-168">Agregar una nueva carpeta para el **WebRole1** proyecto denominado *PersistentConnections*.</span><span class="sxs-lookup"><span data-stu-id="63f41-168">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="63f41-169">Haga clic en esta carpeta y seleccione **agregar &gt; clase**.</span><span class="sxs-lookup"><span data-stu-id="63f41-169">Right-click this folder and select **Add &gt; Class**.</span></span> <span data-ttu-id="63f41-170">Asigne al nuevo archivo de clase *MyPersistentConnections.cs* y seleccione **agregar**.</span><span class="sxs-lookup"><span data-stu-id="63f41-170">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="63f41-171">Visual Studio abrirá el *MyPersistentConnections.cs* archivo en la ventana principal.</span><span class="sxs-lookup"><span data-stu-id="63f41-171">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="63f41-172">Reemplace el contenido con el código siguiente, a continuación, guarde y cierre el archivo:</span><span class="sxs-lookup"><span data-stu-id="63f41-172">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. <span data-ttu-id="63f41-173">Mediante el `Startup` (clase), los objetos de SignalR se iniciará cuando se inicie OWIN.</span><span class="sxs-lookup"><span data-stu-id="63f41-173">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="63f41-174">Abra o cree *Startup.cs* y reemplace el contenido con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="63f41-174">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    <span data-ttu-id="63f41-175">En el código anterior, el `OwinStartup` atributo marca esta clase para iniciar OWIN.</span><span class="sxs-lookup"><span data-stu-id="63f41-175">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="63f41-176">El `Configuration` método comienza a SignalR.</span><span class="sxs-lookup"><span data-stu-id="63f41-176">The `Configuration` method starts SignalR.</span></span>
    
26. <span data-ttu-id="63f41-177">Probar la aplicación en el emulador de Microsoft Azure presionando **F5**.</span><span class="sxs-lookup"><span data-stu-id="63f41-177">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="63f41-178">Si se produce un **FileLoadException** en **MapSignalR**, cambiar las redirecciones de enlace en *web.config* al siguiente:</span><span class="sxs-lookup"><span data-stu-id="63f41-178">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. <span data-ttu-id="63f41-179">Espere aproximadamente un minuto.</span><span class="sxs-lookup"><span data-stu-id="63f41-179">Wait about one minute.</span></span> <span data-ttu-id="63f41-180">Abra la ventana de herramientas del explorador de la nube en Visual Studio (**vista &gt; Explorer nube**) y expanda la ruta de acceso `(Local)\Storage Accounts\(Development)\Tables`.</span><span class="sxs-lookup"><span data-stu-id="63f41-180">Open the Cloud Explorer tool window in Visual Studio (**View &gt; Cloud Explorer**) and expand the path `(Local)\Storage Accounts\(Development)\Tables`.</span></span> <span data-ttu-id="63f41-181">Haga doble clic en **WADPerformanceCountersTable**.</span><span class="sxs-lookup"><span data-stu-id="63f41-181">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="63f41-182">Debería ver los contadores de SignalR en la tabla de datos.</span><span class="sxs-lookup"><span data-stu-id="63f41-182">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="63f41-183">Si no ve la tabla, debe volver a escribir las credenciales de almacenamiento de Azure.</span><span class="sxs-lookup"><span data-stu-id="63f41-183">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="63f41-184">Debe seleccionar la **actualizar** botón para ver la tabla de **Explorer nube** o seleccione el **actualizar** botón en la ventana Abrir tabla para ver los datos en la tabla.</span><span class="sxs-lookup"><span data-stu-id="63f41-184">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![Al seleccionar la tabla de contadores de rendimiento de WAD en el Explorador de Visual Studio en la nube](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Que muestra los contadores recopilados en la tabla de contadores de rendimiento de WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. <span data-ttu-id="63f41-187">Para probar la aplicación en la nube, actualice el **ServiceConfiguration.Cloud.cscfg** de archivos y establezca la `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` en una cadena de conexión de cuenta de almacenamiento de Azure válida.</span><span class="sxs-lookup"><span data-stu-id="63f41-187">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="63f41-188">Implementar la aplicación en su suscripción de Azure.</span><span class="sxs-lookup"><span data-stu-id="63f41-188">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="63f41-189">Para obtener más información sobre cómo implementar una aplicación en Azure, consulte [cómo crear e implementar un servicio de nube](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="63f41-189">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="63f41-190">Espere unos minutos.</span><span class="sxs-lookup"><span data-stu-id="63f41-190">Wait a few minutes.</span></span> <span data-ttu-id="63f41-191">En **Explorer nube**, busque la cuenta de almacenamiento que configuró anteriormente y busque el `WADPerformanceCountersTable` tabla en ella.</span><span class="sxs-lookup"><span data-stu-id="63f41-191">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="63f41-192">Debería ver los contadores de SignalR en la tabla de datos.</span><span class="sxs-lookup"><span data-stu-id="63f41-192">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="63f41-193">Si no ve la tabla, debe volver a escribir las credenciales de almacenamiento de Azure.</span><span class="sxs-lookup"><span data-stu-id="63f41-193">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="63f41-194">Debe seleccionar la **actualizar** botón para ver la tabla de **Explorer nube** o seleccione el **actualizar** botón en la ventana Abrir tabla para ver los datos en la tabla.</span><span class="sxs-lookup"><span data-stu-id="63f41-194">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="63f41-195">Agradecimientos especiales a [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) para el contenido original utilizado en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="63f41-195">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
