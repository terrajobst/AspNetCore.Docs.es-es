---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Uso de contadores de rendimiento de SignalR en un rol Web de Azure | Microsoft Docs
author: guardrex
description: Cómo instalar y usar contadores de rendimiento de SignalR en un rol Web de Azure.
keywords: Contador ASP.NET,signalr,performance, rol web de azure
ms.author: riande
ms.date: 10/03/2018
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: bdd875201895c6eaf155b54582d0898c2570d93c
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287721"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="173a2-104">Uso de contadores de rendimiento de SignalR en un rol Web de Azure</span><span class="sxs-lookup"><span data-stu-id="173a2-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="173a2-105">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="173a2-105">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="173a2-106">Contadores de rendimiento de SignalR se usan para supervisar el rendimiento de la aplicación en un rol Web de Azure.</span><span class="sxs-lookup"><span data-stu-id="173a2-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="173a2-107">Los contadores se capturan por diagnósticos de Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="173a2-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="173a2-108">Instalar los contadores de rendimiento de SignalR en Azure con *signalr.exe*, la misma herramienta que se usa para las aplicaciones independientes o en el entorno local.</span><span class="sxs-lookup"><span data-stu-id="173a2-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="173a2-109">Puesto que las funciones de Azure son transitorias, configurar una aplicación para instalar y registrar los contadores de rendimiento de SignalR tras el inicio.</span><span class="sxs-lookup"><span data-stu-id="173a2-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="173a2-110">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="173a2-110">Prerequisites</span></span>

* <span data-ttu-id="173a2-111">Visual Studio 2015 o [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span><span class="sxs-lookup"><span data-stu-id="173a2-111">Visual Studio 2015 or [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span></span>
* <span data-ttu-id="173a2-112">[Microsoft Azure SDK para Visual Studio](https://azure.microsoft.com/downloads/) **Nota: Reinicie el equipo después de instalar el SDK.**</span><span class="sxs-lookup"><span data-stu-id="173a2-112">[Microsoft Azure SDK for Visual Studio](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="173a2-113">Suscripción de Microsoft Azure: Para registrarse para una cuenta de prueba gratuita de Azure, consulte [evaluación gratuita de Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="173a2-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="173a2-114">Crear una aplicación de rol Web de Azure que expone los contadores de rendimiento de SignalR</span><span class="sxs-lookup"><span data-stu-id="173a2-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="173a2-115">Abra Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="173a2-115">Open Visual Studio.</span></span>

2. <span data-ttu-id="173a2-116">En Visual Studio, seleccione **Archivo** > **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="173a2-116">In Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="173a2-117">En el **nuevo proyecto** cuadro de diálogo, seleccione el **Visual C#** > **en la nube** categoría de la izquierda y, a continuación, seleccione el **deserviciodenubedeAzure** plantilla.</span><span class="sxs-lookup"><span data-stu-id="173a2-117">In the **New Project** dialog box, select the **Visual C#** > **Cloud** category on the left, and then select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="173a2-118">Nombre de la aplicación **SignalRPerfCounters** y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="173a2-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![Nueva aplicación en la nube](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)

   > [!NOTE]
   > <span data-ttu-id="173a2-120">Si no ve el **en la nube** categoría de plantilla o la **Azure Cloud Service** plantilla, deberá instalar el **desarrollo de Azure** carga de trabajo para Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="173a2-120">If you don't see the **Cloud** template category or the **Azure Cloud Service** template, you need to install the **Azure development** workload for Visual Studio 2017.</span></span> <span data-ttu-id="173a2-121">Elija la **abrir el instalador de Visual Studio** vínculo en la parte inferior izquierda de la **nuevo proyecto** cuadro de diálogo para abrir el instalador de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="173a2-121">Choose the **Open Visual Studio Installer** link on the bottom-left side of the **New Project** dialog to open Visual Studio Installer.</span></span> <span data-ttu-id="173a2-122">Seleccione el **desarrollo de Azure** carga de trabajo y, a continuación, elija **modificar** para iniciar la instalación de la carga de trabajo.</span><span class="sxs-lookup"><span data-stu-id="173a2-122">Select the **Azure development** workload, and then choose **Modify** to start installing the workload.</span></span>
   >
   > ![Carga de trabajo de desarrollo de Azure en el instalador de Visual Studio](using-signalr-performance-counters-in-an-azure-web-role/_static/azure-development-workload.png)

4. <span data-ttu-id="173a2-124">En el **nuevo servicio en la nube de Microsoft Azure** cuadro de diálogo, seleccione **rol Web de ASP.NET** y seleccione el > botón para agregar el rol al proyecto.</span><span class="sxs-lookup"><span data-stu-id="173a2-124">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the > button to add the role to the project.</span></span> <span data-ttu-id="173a2-125">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="173a2-125">Select **OK**.</span></span>

   ![Agregar el rol Web de ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)

5. <span data-ttu-id="173a2-127">En el **nueva aplicación Web de ASP.NET: WebRole1** cuadro de diálogo, seleccione el **MVC** plantilla y, a continuación, seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="173a2-127">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![Incorporación de MVC y Web API](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)

6. <span data-ttu-id="173a2-129">En **el Explorador de soluciones**, abra el *diagnostics.wadcfgx* archivo **WebRole1**.</span><span class="sxs-lookup"><span data-stu-id="173a2-129">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![Diagnostics.wadcfgx del explorador de soluciones](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)

7. <span data-ttu-id="173a2-131">Reemplace el contenido del archivo con la siguiente configuración y guarde el archivo:</span><span class="sxs-lookup"><span data-stu-id="173a2-131">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]

8. <span data-ttu-id="173a2-132">Abra el **Package Manager Console** desde **herramientas** > **Administrador de paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="173a2-132">Open the **Package Manager Console** from **Tools** > **NuGet Package Manager**.</span></span> <span data-ttu-id="173a2-133">Escriba los comandos siguientes para instalar la versión más reciente de SignalR y el paquete de utilidades de SignalR:</span><span class="sxs-lookup"><span data-stu-id="173a2-133">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]

9. <span data-ttu-id="173a2-134">Configure la aplicación para instalar los contadores de rendimiento de SignalR en la instancia de rol cuando se inicia o se recicla.</span><span class="sxs-lookup"><span data-stu-id="173a2-134">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="173a2-135">En **el Explorador de soluciones**, haga doble clic en el **WebRole1** del proyecto y seleccione **agregar** > **nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="173a2-135">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="173a2-136">Nombre de la nueva carpeta *inicio*.</span><span class="sxs-lookup"><span data-stu-id="173a2-136">Name the new folder *Startup*.</span></span>

   ![Agregar carpeta de inicio](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)

10. <span data-ttu-id="173a2-138">Copia el *signalr.exe* archivo (agregado con el **Microsoft.AspNet.SignalR.Utils** paquete) de \<carpeta del proyecto > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\< versión > / herramientas para el *inicio* carpeta que creó en el paso anterior.</span><span class="sxs-lookup"><span data-stu-id="173a2-138">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from \<project folder>/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\<version>/tools to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="173a2-139">En **el Explorador de soluciones**, haga clic en el *inicio* carpeta y seleccione **agregar** > **elemento existente**.</span><span class="sxs-lookup"><span data-stu-id="173a2-139">In **Solution Explorer**, right-click the *Startup* folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="173a2-140">En el cuadro de diálogo que aparece, seleccione *signalr.exe* y seleccione **agregar**.</span><span class="sxs-lookup"><span data-stu-id="173a2-140">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    ![Agregar signalr.exe al proyecto](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)

12. <span data-ttu-id="173a2-142">Haga doble clic en el *inicio* carpeta que ha creado.</span><span class="sxs-lookup"><span data-stu-id="173a2-142">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="173a2-143">Seleccione **Agregar** > **Nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="173a2-143">Select **Add** > **New Item**.</span></span> <span data-ttu-id="173a2-144">Seleccione el **General** nodo, seleccione **archivo de texto**y el nombre del nuevo elemento *SignalRPerfCounterInstall.cmd*.</span><span class="sxs-lookup"><span data-stu-id="173a2-144">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="173a2-145">Este archivo de comandos instala los contadores de rendimiento de SignalR en el rol web.</span><span class="sxs-lookup"><span data-stu-id="173a2-145">This command file will install the SignalR performance counters into the web role.</span></span>

    ![Crear archivo de proceso por lotes de instalación del contador de rendimiento de SignalR](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)

13. <span data-ttu-id="173a2-147">Cuando Visual Studio crea el *SignalRPerfCounterInstall.cmd* archivo, abrirá automáticamente en la ventana principal.</span><span class="sxs-lookup"><span data-stu-id="173a2-147">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="173a2-148">Reemplace el contenido del archivo con el siguiente script, a continuación, guarde y cierre el archivo.</span><span class="sxs-lookup"><span data-stu-id="173a2-148">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="173a2-149">Este script se ejecuta *signalr.exe*, que agrega los contadores de rendimiento de SignalR a la instancia de rol.</span><span class="sxs-lookup"><span data-stu-id="173a2-149">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]

14. <span data-ttu-id="173a2-150">Seleccione el *signalr.exe* archivo **el Explorador de soluciones**.</span><span class="sxs-lookup"><span data-stu-id="173a2-150">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="173a2-151">En el archivo **propiedades**, establezca **Copy to Output Directory** a **copiar siempre**.</span><span class="sxs-lookup"><span data-stu-id="173a2-151">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![Conjunto de copia al directorio de salida para copiar siempre](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)

15. <span data-ttu-id="173a2-153">Repita el paso anterior para el *SignalRPerfCounterInstall.cmd* archivo.</span><span class="sxs-lookup"><span data-stu-id="173a2-153">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>


16. <span data-ttu-id="173a2-154">Haga doble clic en el *SignalRPerfCounterInstall.cmd* de archivo y seleccione **abrir con**.</span><span class="sxs-lookup"><span data-stu-id="173a2-154">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="173a2-155">En el cuadro de diálogo que aparece, seleccione **Editor binario** y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="173a2-155">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![Abrir con el Editor binario](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)

17. <span data-ttu-id="173a2-157">En el editor binario, seleccione todos los bytes iniciales en el archivo y eliminarlos.</span><span class="sxs-lookup"><span data-stu-id="173a2-157">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="173a2-158">Guarde y cierre el archivo.</span><span class="sxs-lookup"><span data-stu-id="173a2-158">Save and close the file.</span></span>

    ![Eliminar los bytes iniciales](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)

18. <span data-ttu-id="173a2-160">Abra *ServiceDefinition.csdef* y agregue una tarea de inicio que se ejecuta el *SignalrPerfCounterInstall.cmd* cuando se inicia el servicio de archivos:</span><span class="sxs-lookup"><span data-stu-id="173a2-160">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]

19. <span data-ttu-id="173a2-161">Abra `Views/Shared/_Layout.cshtml` y quitar la secuencia de comandos del paquete jQuery desde el final del archivo.</span><span class="sxs-lookup"><span data-stu-id="173a2-161">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]

20. <span data-ttu-id="173a2-162">Agregar un cliente de JavaScript que continuamente llama el `increment` método en el servidor.</span><span class="sxs-lookup"><span data-stu-id="173a2-162">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="173a2-163">Abra `Views/Home/Index.cshtml` y reemplace el contenido con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="173a2-163">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]

21. <span data-ttu-id="173a2-164">Cree una nueva carpeta en el **WebRole1** proyecto denominado *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="173a2-164">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="173a2-165">Haga clic en el *Hubs* carpeta **el Explorador de soluciones** y seleccione **agregar** > **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="173a2-165">Right-click the *Hubs* folder in **Solution Explorer** and select **Add** > **New Item**.</span></span> <span data-ttu-id="173a2-166">En el **Agregar nuevo elemento** cuadro de diálogo, seleccione el **Web** > **SignalR** categoría y, a continuación, seleccione el **clase de concentrador de SignalR (v2)** plantilla de elemento.</span><span class="sxs-lookup"><span data-stu-id="173a2-166">In the **Add New Item** dialog box, select the **Web** > **SignalR** category, and then select the **SignalR Hub Class (v2)** item template.</span></span> <span data-ttu-id="173a2-167">Nombre del nuevo centro *MyHub.cs* y seleccione **agregar**.</span><span class="sxs-lookup"><span data-stu-id="173a2-167">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![Agregar clase de concentrador de SignalR a la carpeta de concentradores en el cuadro de diálogo Agregar nuevo elemento](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="173a2-169">*MyHub.cs* se abrirá automáticamente en la ventana principal.</span><span class="sxs-lookup"><span data-stu-id="173a2-169">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="173a2-170">Reemplace el contenido por el código siguiente, a continuación, guarde y cierre el archivo:</span><span class="sxs-lookup"><span data-stu-id="173a2-170">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]

23. <span data-ttu-id="173a2-171">*[Crank.exe](signalr-connection-density-testing-with-crank.md)*  es probar herramienta proporcionada con la base de código de SignalR una densidad de la conexión.</span><span class="sxs-lookup"><span data-stu-id="173a2-171">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="173a2-172">Como manivela requiere una conexión persistente, agrega uno a su sitio para su uso al probar.</span><span class="sxs-lookup"><span data-stu-id="173a2-172">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="173a2-173">Agregar una nueva carpeta para el **WebRole1** proyecto denominado *PersistentConnections*.</span><span class="sxs-lookup"><span data-stu-id="173a2-173">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="173a2-174">Haga clic en esta carpeta y seleccione **agregar** > **clase**.</span><span class="sxs-lookup"><span data-stu-id="173a2-174">Right-click this folder and select **Add** > **Class**.</span></span> <span data-ttu-id="173a2-175">Asigne al nuevo archivo de clase *MyPersistentConnections.cs* y seleccione **agregar**.</span><span class="sxs-lookup"><span data-stu-id="173a2-175">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="173a2-176">Visual Studio abrirá el *MyPersistentConnections.cs* archivo en la ventana principal.</span><span class="sxs-lookup"><span data-stu-id="173a2-176">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="173a2-177">Reemplace el contenido por el código siguiente, a continuación, guarde y cierre el archivo:</span><span class="sxs-lookup"><span data-stu-id="173a2-177">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]

25. <span data-ttu-id="173a2-178">Mediante el `Startup` (clase), los objetos de SignalR se iniciará cuando se inicie OWIN.</span><span class="sxs-lookup"><span data-stu-id="173a2-178">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="173a2-179">Abra o cree *Startup.cs* y reemplace el contenido con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="173a2-179">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]

    <span data-ttu-id="173a2-180">En el código anterior, el `OwinStartup` atributo marca esta clase para el inicio de OWIN.</span><span class="sxs-lookup"><span data-stu-id="173a2-180">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="173a2-181">El `Configuration` método comienza a SignalR.</span><span class="sxs-lookup"><span data-stu-id="173a2-181">The `Configuration` method starts SignalR.</span></span>

26. <span data-ttu-id="173a2-182">Probar la aplicación en el emulador de Microsoft Azure presionando **F5**.</span><span class="sxs-lookup"><span data-stu-id="173a2-182">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="173a2-183">Si se produce un **FileLoadException** en **MapSignalR**, cambie las redirecciones de enlace en *web.config* al siguiente:</span><span class="sxs-lookup"><span data-stu-id="173a2-183">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]

27. <span data-ttu-id="173a2-184">Espere aproximadamente un minuto.</span><span class="sxs-lookup"><span data-stu-id="173a2-184">Wait about one minute.</span></span> <span data-ttu-id="173a2-185">Abra la ventana de herramientas de Cloud Explorer en Visual Studio (**vista** > **Cloud Explorer**) y expanda la ruta de acceso `(Local)/Storage Accounts/(Development)/Tables`.</span><span class="sxs-lookup"><span data-stu-id="173a2-185">Open the Cloud Explorer tool window in Visual Studio (**View** > **Cloud Explorer**) and expand the path `(Local)/Storage Accounts/(Development)/Tables`.</span></span> <span data-ttu-id="173a2-186">Haga doble clic en **WADPerformanceCountersTable**.</span><span class="sxs-lookup"><span data-stu-id="173a2-186">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="173a2-187">Debería ver los contadores de SignalR en la tabla de datos.</span><span class="sxs-lookup"><span data-stu-id="173a2-187">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="173a2-188">Si no ve la tabla, deberá volver a escribir sus credenciales de Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="173a2-188">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="173a2-189">Es posible que deba seleccionar el **actualizar** botón para ver la tabla de **Cloud Explorer** o seleccione el **actualizar** botón en la ventana Abrir tabla para ver los datos de la tabla.</span><span class="sxs-lookup"><span data-stu-id="173a2-189">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![Seleccione la tabla de contadores de rendimiento de WAD en Visual Studio Cloud Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Que muestra los contadores recopilados en la tabla de contadores de rendimiento de WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)

28. <span data-ttu-id="173a2-192">Para probar la aplicación en la nube, actualice el **ServiceConfiguration.Cloud.cscfg** de archivos y establecer el `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` a una cadena de conexión de cuenta de almacenamiento de Azure válida.</span><span class="sxs-lookup"><span data-stu-id="173a2-192">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="173a2-193">Implementar la aplicación en su suscripción de Azure.</span><span class="sxs-lookup"><span data-stu-id="173a2-193">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="173a2-194">Para obtener más información sobre cómo implementar una aplicación en Azure, consulte [cómo crear e implementar un servicio de nube](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="173a2-194">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="173a2-195">Espere unos minutos.</span><span class="sxs-lookup"><span data-stu-id="173a2-195">Wait a few minutes.</span></span> <span data-ttu-id="173a2-196">En **Cloud Explorer**, busque la cuenta de almacenamiento que configuró anteriormente y busque el `WADPerformanceCountersTable` tabla en ella.</span><span class="sxs-lookup"><span data-stu-id="173a2-196">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="173a2-197">Debería ver los contadores de SignalR en la tabla de datos.</span><span class="sxs-lookup"><span data-stu-id="173a2-197">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="173a2-198">Si no ve la tabla, deberá volver a escribir sus credenciales de Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="173a2-198">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="173a2-199">Es posible que deba seleccionar el **actualizar** botón para ver la tabla de **Cloud Explorer** o seleccione el **actualizar** botón en la ventana Abrir tabla para ver los datos de la tabla.</span><span class="sxs-lookup"><span data-stu-id="173a2-199">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="173a2-200">Agradecimientos especiales a [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) para el contenido original utilizado en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="173a2-200">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
