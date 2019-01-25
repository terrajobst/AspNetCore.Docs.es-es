---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Tutorial: Creación de aplicaciones de alta frecuencia en tiempo real con SignalR 2 | Microsoft Docs'
author: bradygaster
description: Este tutorial muestra cómo crear una aplicación web que usa SignalR de ASP.NET para proporcionar funcionalidad de mensajería de alta frecuencia.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 44aaa2b0c059de310e963f642fa56c2f00a7e443
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836732"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a><span data-ttu-id="a815d-103">Tutorial: Creación de aplicaciones de alta frecuencia en tiempo real con SignalR 2</span><span class="sxs-lookup"><span data-stu-id="a815d-103">Tutorial: Create high-frequency real-time app with SignalR 2</span></span>

<span data-ttu-id="a815d-104">Este tutorial muestra cómo crear una aplicación web que usa ASP.NET SignalR 2 para proporcionar funcionalidad de mensajería de alta frecuencia.</span><span class="sxs-lookup"><span data-stu-id="a815d-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span> <span data-ttu-id="a815d-105">En este caso, "mensajería de alta frecuencia" significa que el servidor envía actualizaciones a una tarifa fija.</span><span class="sxs-lookup"><span data-stu-id="a815d-105">In this case, "high-frequency messaging" means the server sends updates at a fixed rate.</span></span> <span data-ttu-id="a815d-106">Enviar hasta 10 mensajes por segundo.</span><span class="sxs-lookup"><span data-stu-id="a815d-106">You send up to 10 messages a second.</span></span>

<span data-ttu-id="a815d-107">La aplicación que crea muestra una forma que los usuarios pueden arrastrar.</span><span class="sxs-lookup"><span data-stu-id="a815d-107">The application you create displays a shape that users can drag.</span></span> <span data-ttu-id="a815d-108">El servidor actualiza la posición de la forma en todos los exploradores conectados para que coincida con la posición de la forma arrastrada mediante actualizaciones periódicas.</span><span class="sxs-lookup"><span data-stu-id="a815d-108">The server updates the position of the shape in all connected browsers to match the position of the dragged shape using timed updates.</span></span>

<span data-ttu-id="a815d-109">Los conceptos presentados en este tutorial tienen aplicaciones de juegos en tiempo real y otras aplicaciones de la simulación.</span><span class="sxs-lookup"><span data-stu-id="a815d-109">Concepts introduced in this tutorial have applications in real-time gaming and other simulation applications.</span></span>

<span data-ttu-id="a815d-110">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="a815d-110">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a815d-111">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="a815d-111">Set up the project</span></span>
> * <span data-ttu-id="a815d-112">Crear la aplicación básica</span><span class="sxs-lookup"><span data-stu-id="a815d-112">Create the base application</span></span>
> * <span data-ttu-id="a815d-113">Cuando se inicia la aplicación se asignan al concentrador</span><span class="sxs-lookup"><span data-stu-id="a815d-113">Map to the hub when app starts</span></span>
> * <span data-ttu-id="a815d-114">Agregar el cliente</span><span class="sxs-lookup"><span data-stu-id="a815d-114">Add the client</span></span>
> * <span data-ttu-id="a815d-115">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="a815d-115">Run the app</span></span>
> * <span data-ttu-id="a815d-116">Agregar el bucle de cliente</span><span class="sxs-lookup"><span data-stu-id="a815d-116">Add the client loop</span></span>
> * <span data-ttu-id="a815d-117">Agregar bucle de servidor</span><span class="sxs-lookup"><span data-stu-id="a815d-117">Add the server loop</span></span>
> * <span data-ttu-id="a815d-118">Agregar una animación fluida</span><span class="sxs-lookup"><span data-stu-id="a815d-118">Add smooth animation</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="a815d-119">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="a815d-119">Prerequisites</span></span>

* <span data-ttu-id="a815d-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con el **ASP.NET y desarrollo web** carga de trabajo.</span><span class="sxs-lookup"><span data-stu-id="a815d-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="a815d-121">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="a815d-121">Set up the project</span></span>

<span data-ttu-id="a815d-122">En esta sección, creará el proyecto en Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="a815d-122">In this section, you create the project in Visual Studio 2017.</span></span>

<span data-ttu-id="a815d-123">En esta sección se muestra cómo usar Visual Studio 2017 para crear una aplicación Web de ASP.NET vacía y agregar las bibliotecas de SignalR y jQuery.UI.</span><span class="sxs-lookup"><span data-stu-id="a815d-123">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application and add the SignalR and jQuery.UI libraries.</span></span>

1. <span data-ttu-id="a815d-124">En Visual Studio, cree una aplicación Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a815d-124">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Creación de web](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. <span data-ttu-id="a815d-126">En el **nueva aplicación Web de ASP.NET - MoveShapeDemo** ventana, deje **vacía** seleccionado y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="a815d-126">In the **New ASP.NET Web Application - MoveShapeDemo** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="a815d-127">En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="a815d-127">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="a815d-128">En **Agregar nuevo elemento - MoveShapeDemo**, seleccione **instalado** > **Visual C#**   >  **Web**  >  **SignalR** y, a continuación, seleccione **clase de concentrador de SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="a815d-128">In **Add New Item - MoveShapeDemo**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="a815d-129">Nombre de la clase *MoveShapeHub* y agréguelo al proyecto.</span><span class="sxs-lookup"><span data-stu-id="a815d-129">Name the class *MoveShapeHub* and add it to the project.</span></span>

    <span data-ttu-id="a815d-130">Este paso se crea el *MoveShapeHub.cs* archivo de clase.</span><span class="sxs-lookup"><span data-stu-id="a815d-130">This step creates the *MoveShapeHub.cs* class file.</span></span> <span data-ttu-id="a815d-131">Al mismo tiempo, agrega un conjunto de archivos de script y las referencias de ensamblado que admiten SignalR al proyecto.</span><span class="sxs-lookup"><span data-stu-id="a815d-131">Simultaneously, it adds  a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="a815d-132">Seleccione **herramientas** > **Administrador de paquetes de NuGet** > **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="a815d-132">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

1. <span data-ttu-id="a815d-133">En **Package Manager Console**, ejecute este comando:</span><span class="sxs-lookup"><span data-stu-id="a815d-133">In **Package Manager Console**, run this command:</span></span>

    ```console
    Install-Package jQuery.UI.Combined
    ```

    <span data-ttu-id="a815d-134">El comando instala la biblioteca de interfaz de usuario de jQuery.</span><span class="sxs-lookup"><span data-stu-id="a815d-134">The command installs the jQuery UI library.</span></span> <span data-ttu-id="a815d-135">Usa para animar la forma.</span><span class="sxs-lookup"><span data-stu-id="a815d-135">You use it to animate the shape.</span></span>

1. <span data-ttu-id="a815d-136">En **el Explorador de soluciones**, expanda el nodo secuencias de comandos.</span><span class="sxs-lookup"><span data-stu-id="a815d-136">In **Solution Explorer**, expand the Scripts node.</span></span>

    ![Referencias a bibliotecas de script](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    <span data-ttu-id="a815d-138">Bibliotecas de scripts de jQuery, jQueryUI y SignalR están visibles en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="a815d-138">Script libraries for jQuery, jQueryUI, and SignalR are visible in the project.</span></span>

## <a name="create-the-base-application"></a><span data-ttu-id="a815d-139">Crear la aplicación básica</span><span class="sxs-lookup"><span data-stu-id="a815d-139">Create the base application</span></span>

<span data-ttu-id="a815d-140">En esta sección, creará una aplicación de explorador.</span><span class="sxs-lookup"><span data-stu-id="a815d-140">In this section, you create a browser application.</span></span> <span data-ttu-id="a815d-141">La aplicación envía la ubicación de la forma en el servidor durante cada evento de mover el mouse.</span><span class="sxs-lookup"><span data-stu-id="a815d-141">The app sends the location of the shape to the server during each mouse move event.</span></span> <span data-ttu-id="a815d-142">El servidor transmite esta información para todos los clientes conectados en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="a815d-142">The server broadcasts this information to all other connected clients in real time.</span></span> <span data-ttu-id="a815d-143">Aprenderá más acerca de esta aplicación en secciones posteriores.</span><span class="sxs-lookup"><span data-stu-id="a815d-143">You learn more about this application in later sections.</span></span>

1. <span data-ttu-id="a815d-144">Abra el *MoveShapeHub.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="a815d-144">Open the *MoveShapeHub.cs* file.</span></span>

1. <span data-ttu-id="a815d-145">Reemplace el código en el *MoveShapeHub.cs* archivo con este código:</span><span class="sxs-lookup"><span data-stu-id="a815d-145">Replace the code in the *MoveShapeHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="a815d-146">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="a815d-146">Save the file.</span></span>

<span data-ttu-id="a815d-147">La `MoveShapeHub` clase es una implementación de un concentrador SignalR.</span><span class="sxs-lookup"><span data-stu-id="a815d-147">The `MoveShapeHub` class is an implementation of a SignalR hub.</span></span> <span data-ttu-id="a815d-148">Como en el [Introducción a SignalR](tutorial-getting-started-with-signalr.md) tutorial, el concentrador tiene un método que llaman directamente los clientes.</span><span class="sxs-lookup"><span data-stu-id="a815d-148">As in the [Getting Started with SignalR](tutorial-getting-started-with-signalr.md) tutorial, the hub has a method that the clients call directly.</span></span> <span data-ttu-id="a815d-149">En este caso, el cliente envía un objeto con la nueva X y las coordenadas de la forma en el servidor.</span><span class="sxs-lookup"><span data-stu-id="a815d-149">In this case, the client sends an object with the new X and Y coordinates of the shape to the server.</span></span> <span data-ttu-id="a815d-150">Esas coordenadas obtengan difunden a todos los demás clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="a815d-150">Those coordinates get broadcasted to all other connected clients.</span></span> <span data-ttu-id="a815d-151">SignalR serializa automáticamente este objeto mediante JSON.</span><span class="sxs-lookup"><span data-stu-id="a815d-151">SignalR automatically serializes this object using JSON.</span></span>

<span data-ttu-id="a815d-152">Los envíos de la aplicación la `ShapeModel` objeto al cliente.</span><span class="sxs-lookup"><span data-stu-id="a815d-152">The app sends the `ShapeModel` object to the client.</span></span> <span data-ttu-id="a815d-153">Tiene miembros para almacenar la posición de la forma.</span><span class="sxs-lookup"><span data-stu-id="a815d-153">It has members to store the position of the shape.</span></span> <span data-ttu-id="a815d-154">La versión del objeto en el servidor también tiene un miembro para realizar un seguimiento de los datos del cliente que se va a almacenar.</span><span class="sxs-lookup"><span data-stu-id="a815d-154">The version of the object on the server also has a member to track which client's data is being stored.</span></span> <span data-ttu-id="a815d-155">Este objeto impide que el servidor envíe datos de un cliente a sí mismo.</span><span class="sxs-lookup"><span data-stu-id="a815d-155">This object prevents the server from sending a client's data back to itself.</span></span> <span data-ttu-id="a815d-156">Este miembro se usa el `JsonIgnore` atributo para mantener la aplicación de serializar los datos y se envían al cliente.</span><span class="sxs-lookup"><span data-stu-id="a815d-156">This member uses the `JsonIgnore` attribute to keep the application from serializing the data and sending it back to the client.</span></span>

## <a name="map-to-the-hub-when-app-starts"></a><span data-ttu-id="a815d-157">Cuando se inicia la aplicación se asignan al concentrador</span><span class="sxs-lookup"><span data-stu-id="a815d-157">Map to the hub when app starts</span></span>

<span data-ttu-id="a815d-158">A continuación, configurar la asignación al concentrador cuando se inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a815d-158">Next, you set up mapping to the hub when the application starts.</span></span> <span data-ttu-id="a815d-159">En SignalR 2, la adición de una clase de inicio OWIN crea la asignación.</span><span class="sxs-lookup"><span data-stu-id="a815d-159">In SignalR 2, adding an OWIN startup class creates the mapping.</span></span>

1. <span data-ttu-id="a815d-160">En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="a815d-160">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="a815d-161">En **Agregar nuevo elemento - MoveShapeDemo** seleccione **instalado** > **Visual C#**   >  **Web** y, a continuación, Seleccione **clase de inicio OWIN**.</span><span class="sxs-lookup"><span data-stu-id="a815d-161">In **Add New Item - MoveShapeDemo** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="a815d-162">Nombre de la clase *inicio* y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="a815d-162">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="a815d-163">Reemplace el código predeterminado en el *Startup.cs* archivo con este código:</span><span class="sxs-lookup"><span data-stu-id="a815d-163">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<span data-ttu-id="a815d-164">La clase de inicio OWIN llama `MapSignalR` cuando se ejecuta la aplicación la `Configuration` método.</span><span class="sxs-lookup"><span data-stu-id="a815d-164">The OWIN startup class calls `MapSignalR` when the app executes the `Configuration` method.</span></span> <span data-ttu-id="a815d-165">La aplicación agrega la clase de inicio del OWIN proceso utilizando el `OwinStartup` atributo de ensamblado.</span><span class="sxs-lookup"><span data-stu-id="a815d-165">The app adds the class to OWIN's startup process using the `OwinStartup` assembly attribute.</span></span>

## <a name="add-the-client"></a><span data-ttu-id="a815d-166">Agregar el cliente</span><span class="sxs-lookup"><span data-stu-id="a815d-166">Add the client</span></span>

<span data-ttu-id="a815d-167">Agregue la página HTML para el cliente.</span><span class="sxs-lookup"><span data-stu-id="a815d-167">Add the HTML page for the client.</span></span>

1. <span data-ttu-id="a815d-168">En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **página HTML**.</span><span class="sxs-lookup"><span data-stu-id="a815d-168">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="a815d-169">Nombre de la página **predeterminado** y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="a815d-169">Name the page **Default** and select **OK**.</span></span>

1. <span data-ttu-id="a815d-170">En **el Explorador de soluciones**, haga clic en *Default.html* y seleccione **establecer como página de inicio**.</span><span class="sxs-lookup"><span data-stu-id="a815d-170">In **Solution Explorer**, right-click *Default.html* and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="a815d-171">Reemplace el código predeterminado en el *Default.html* archivo con este código:</span><span class="sxs-lookup"><span data-stu-id="a815d-171">Replace the default code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. <span data-ttu-id="a815d-172">En **el Explorador de soluciones**, expanda **Scripts**.</span><span class="sxs-lookup"><span data-stu-id="a815d-172">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="a815d-173">Bibliotecas de scripts de jQuery y SignalR están visibles en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="a815d-173">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="a815d-174">El Administrador de paquetes instala una versión posterior de los scripts de SignalR.</span><span class="sxs-lookup"><span data-stu-id="a815d-174">The package manager installs a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="a815d-175">Actualizar las referencias de script en el bloque de código para que coincida con las versiones de los archivos de script en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="a815d-175">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

<span data-ttu-id="a815d-176">Este código HTML y JavaScript crea una red `div` llamado `shape`.</span><span class="sxs-lookup"><span data-stu-id="a815d-176">This HTML and JavaScript code creates a red `div` called `shape`.</span></span> <span data-ttu-id="a815d-177">Se habilita el comportamiento de arrastrar la forma mediante la biblioteca de jQuery y se usa el `drag` eventos a la posición de la forma de envío al servidor.</span><span class="sxs-lookup"><span data-stu-id="a815d-177">It enables the shape's dragging behavior using the jQuery library and uses the `drag` event to send the shape's position to the server.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="a815d-178">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="a815d-178">Run the app</span></span>

<span data-ttu-id="a815d-179">Puede ejecutar la aplicación a se'e funciona.</span><span class="sxs-lookup"><span data-stu-id="a815d-179">You can run the app to se\`e it work.</span></span> <span data-ttu-id="a815d-180">Cuando arrastre la forma en torno a una ventana del explorador, la forma se desplaza demasiado en los otros exploradores.</span><span class="sxs-lookup"><span data-stu-id="a815d-180">When you drag the shape around a browser window, the shape moves in the other browsers too.</span></span>

1. <span data-ttu-id="a815d-181">En la barra de herramientas activar **depuración de scripts** y, a continuación, seleccione el botón Reproducir para ejecutar la aplicación en modo de depuración.</span><span class="sxs-lookup"><span data-stu-id="a815d-181">In the toolbar, turn on **Script Debugging** and then select the play button to run the application in Debug mode.</span></span>

    ![Captura de pantalla del usuario activar el modo de depuración y seleccione Reproducir.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    <span data-ttu-id="a815d-183">Se abre una ventana del explorador con la forma de color rojo en la esquina superior derecha.</span><span class="sxs-lookup"><span data-stu-id="a815d-183">A browser window opens with the red shape in the upper-right corner.</span></span>

1. <span data-ttu-id="a815d-184">Copie la dirección URL de la página.</span><span class="sxs-lookup"><span data-stu-id="a815d-184">Copy the page's URL.</span></span>

1. <span data-ttu-id="a815d-185">Abra un explorador y pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="a815d-185">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="a815d-186">Arrastre la forma de una de las ventanas del explorador.</span><span class="sxs-lookup"><span data-stu-id="a815d-186">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="a815d-187">Sigue la forma en la ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="a815d-187">The shape in the other browser window follows.</span></span>

<span data-ttu-id="a815d-188">Mientras que la aplicación de las funciones con este método, no es un modelo de programación recomendado.</span><span class="sxs-lookup"><span data-stu-id="a815d-188">While the application functions using this method, it's not a recommended programming model.</span></span> <span data-ttu-id="a815d-189">No hay ningún límite superior para el número de mensajes que se envían.</span><span class="sxs-lookup"><span data-stu-id="a815d-189">There's no upper limit to the number of messages getting sent.</span></span> <span data-ttu-id="a815d-190">Como resultado, los clientes y el servidor obtengan experimentando con mensajes y degrada el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="a815d-190">As a result, the clients and server get overwhelmed with messages and performance degrades.</span></span> <span data-ttu-id="a815d-191">Además, la aplicación muestra una animación separada en el cliente.</span><span class="sxs-lookup"><span data-stu-id="a815d-191">Also, the app displays a disjointed animation on the client.</span></span> <span data-ttu-id="a815d-192">Esta animación irregular se produce debido a la forma se mueve al instante por cada método.</span><span class="sxs-lookup"><span data-stu-id="a815d-192">This jerky animation happens because the shape moves instantly by each method.</span></span> <span data-ttu-id="a815d-193">Es mejor si la forma se moverá instantáneamente a cada nueva ubicación.</span><span class="sxs-lookup"><span data-stu-id="a815d-193">It's better if the shape moves smoothly to each new location.</span></span> <span data-ttu-id="a815d-194">A continuación, aprenderá a corregir esos problemas.</span><span class="sxs-lookup"><span data-stu-id="a815d-194">Next, you learn how to fix those issues.</span></span>

## <a name="add-the-client-loop"></a><span data-ttu-id="a815d-195">Agregar el bucle de cliente</span><span class="sxs-lookup"><span data-stu-id="a815d-195">Add the client loop</span></span>

<span data-ttu-id="a815d-196">Envío de la ubicación de la forma en cada evento de mover el mouse, crea un volumen de tráfico de red innecesario.</span><span class="sxs-lookup"><span data-stu-id="a815d-196">Sending the location of the shape on every mouse move event creates an unnecessary amount of network traffic.</span></span> <span data-ttu-id="a815d-197">La aplicación debe limitar los mensajes desde el cliente.</span><span class="sxs-lookup"><span data-stu-id="a815d-197">The app needs to throttle the messages from the client.</span></span>

<span data-ttu-id="a815d-198">Utilice el código javascript `setInterval` función para establecer un bucle que envía información de posición de nuevo al servidor con una tarifa fija.</span><span class="sxs-lookup"><span data-stu-id="a815d-198">Use the javascript `setInterval` function to set up a loop that sends new position information to the server at a fixed rate.</span></span> <span data-ttu-id="a815d-199">Este bucle es una representación básica de un "bucle de juego".</span><span class="sxs-lookup"><span data-stu-id="a815d-199">This loop is a basic representation of a "game loop."</span></span> <span data-ttu-id="a815d-200">Es una función varias veces a que controla toda la funcionalidad de un juego.</span><span class="sxs-lookup"><span data-stu-id="a815d-200">It's a repeatedly called function that drives all the functionality of a game.</span></span>

1. <span data-ttu-id="a815d-201">Reemplace el código de cliente en el *Default.html* archivo con este código:</span><span class="sxs-lookup"><span data-stu-id="a815d-201">Replace the client code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > <span data-ttu-id="a815d-202">Tiene que reemplazar las referencias de script nuevo.</span><span class="sxs-lookup"><span data-stu-id="a815d-202">You have to replace the script references again.</span></span> <span data-ttu-id="a815d-203">Deben coincidir con las versiones de las secuencias de comandos en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="a815d-203">They must match the versions of the scripts in the project.</span></span>

    <span data-ttu-id="a815d-204">Este nuevo código agrega el `updateServerModel` función.</span><span class="sxs-lookup"><span data-stu-id="a815d-204">This new code adds the `updateServerModel` function.</span></span> <span data-ttu-id="a815d-205">Se llama a una frecuencia fija.</span><span class="sxs-lookup"><span data-stu-id="a815d-205">It gets called on a fixed frequency.</span></span> <span data-ttu-id="a815d-206">La función envía los datos de la posición en el servidor cada vez que el `moved` marca indica que hay nuevos datos de posición para enviar.</span><span class="sxs-lookup"><span data-stu-id="a815d-206">The function sends the position data to the server whenever the `moved` flag indicates that there's new position data to send.</span></span>

1. <span data-ttu-id="a815d-207">Seleccione el botón Reproducir para iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="a815d-207">Select the play button to start the application</span></span>

1. <span data-ttu-id="a815d-208">Copie la dirección URL de la página.</span><span class="sxs-lookup"><span data-stu-id="a815d-208">Copy the page's URL.</span></span>

1. <span data-ttu-id="a815d-209">Abra un explorador y pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="a815d-209">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="a815d-210">Arrastre la forma de una de las ventanas del explorador.</span><span class="sxs-lookup"><span data-stu-id="a815d-210">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="a815d-211">Sigue la forma en la ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="a815d-211">The shape in the other browser window follows.</span></span>

<span data-ttu-id="a815d-212">Puesto que la aplicación limita al número de mensajes que se envían al servidor, la animación no aparece nítida como hizo al principio.</span><span class="sxs-lookup"><span data-stu-id="a815d-212">Since the app throttles the number of messages that get sent to the server, the animation won't appear as smooth did at first.</span></span>

## <a name="add-the-server-loop"></a><span data-ttu-id="a815d-213">Agregar bucle de servidor</span><span class="sxs-lookup"><span data-stu-id="a815d-213">Add the server loop</span></span>

<span data-ttu-id="a815d-214">En la aplicación actual, los mensajes enviados desde el servidor al cliente salen tan a menudo cuando se reciben.</span><span class="sxs-lookup"><span data-stu-id="a815d-214">In the current application, messages sent from the server to the client go out as often as they're received.</span></span> <span data-ttu-id="a815d-215">Este tráfico de red presenta un problema similar, como veremos en el cliente.</span><span class="sxs-lookup"><span data-stu-id="a815d-215">This network traffic presents a similar problem as we see on the client.</span></span>

<span data-ttu-id="a815d-216">La aplicación puede enviar mensajes con más frecuencia son necesarios.</span><span class="sxs-lookup"><span data-stu-id="a815d-216">The app can send messages more often than they're needed.</span></span> <span data-ttu-id="a815d-217">La conexión puede inundarse como resultado.</span><span class="sxs-lookup"><span data-stu-id="a815d-217">The connection can become flooded as a result.</span></span> <span data-ttu-id="a815d-218">En esta sección se describe cómo actualizar el servidor para agregar un temporizador que limita la velocidad de los mensajes salientes.</span><span class="sxs-lookup"><span data-stu-id="a815d-218">This section describes how to update the server to add a timer that throttles the rate of the outgoing messages.</span></span>

1. <span data-ttu-id="a815d-219">Reemplace el contenido de `MoveShapeHub.cs` con este código:</span><span class="sxs-lookup"><span data-stu-id="a815d-219">Replace the contents of `MoveShapeHub.cs` with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. <span data-ttu-id="a815d-220">Seleccione el botón Reproducir para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a815d-220">Select the play button to start the application.</span></span>

1. <span data-ttu-id="a815d-221">Copie la dirección URL de la página.</span><span class="sxs-lookup"><span data-stu-id="a815d-221">Copy the page's URL.</span></span>

1. <span data-ttu-id="a815d-222">Abra un explorador y pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="a815d-222">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="a815d-223">Arrastre la forma de una de las ventanas del explorador.</span><span class="sxs-lookup"><span data-stu-id="a815d-223">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="a815d-224">Este código expande el cliente para agregar la `Broadcaster` clase.</span><span class="sxs-lookup"><span data-stu-id="a815d-224">This code expands the client to add the `Broadcaster` class.</span></span> <span data-ttu-id="a815d-225">La nueva clase limita los mensajes salientes con el `Timer` clase a partir de .NET framework.</span><span class="sxs-lookup"><span data-stu-id="a815d-225">The new class throttles the outgoing messages using the `Timer` class from the .NET framework.</span></span>

<span data-ttu-id="a815d-226">Es bueno saber que el propio centro es transitorio.</span><span class="sxs-lookup"><span data-stu-id="a815d-226">It's good to learn that the hub itself is transitory.</span></span> <span data-ttu-id="a815d-227">Se crea cada vez que sea necesario.</span><span class="sxs-lookup"><span data-stu-id="a815d-227">It's created every time it's needed.</span></span> <span data-ttu-id="a815d-228">Por lo que la aplicación crea el `Broadcaster` como un singleton.</span><span class="sxs-lookup"><span data-stu-id="a815d-228">So the app creates the `Broadcaster` as a singleton.</span></span> <span data-ttu-id="a815d-229">Usa la inicialización diferida para aplazar la `Broadcaster`de creación hasta que sea necesario.</span><span class="sxs-lookup"><span data-stu-id="a815d-229">It uses lazy initialization to defer the `Broadcaster`'s creation until it's needed.</span></span> <span data-ttu-id="a815d-230">Que garantiza que la aplicación crea la primera instancia de concentrador por completo antes de iniciar el temporizador.</span><span class="sxs-lookup"><span data-stu-id="a815d-230">That guarantees that the app creates the first hub instance completely before starting the timer.</span></span>

<span data-ttu-id="a815d-231">La llamada a los clientes `UpdateShape` función, a continuación, se mueve fuera del centro `UpdateModel` método.</span><span class="sxs-lookup"><span data-stu-id="a815d-231">The call to the clients' `UpdateShape` function is then moved out of the hub's `UpdateModel` method.</span></span> <span data-ttu-id="a815d-232">Ya no se llama inmediatamente siempre que la aplicación recibe los mensajes entrantes.</span><span class="sxs-lookup"><span data-stu-id="a815d-232">It's no longer called immediately whenever the app receives incoming messages.</span></span> <span data-ttu-id="a815d-233">En su lugar, la aplicación envía los mensajes a los clientes a una velocidad de 25 llamadas por segundo.</span><span class="sxs-lookup"><span data-stu-id="a815d-233">Instead, the app sends the messages to the clients at a rate of 25 calls per second.</span></span> <span data-ttu-id="a815d-234">El proceso es administrado por el `_broadcastLoop` temporizador desde el `Broadcaster` clase.</span><span class="sxs-lookup"><span data-stu-id="a815d-234">The process is  managed by the `_broadcastLoop` timer from within the `Broadcaster` class.</span></span>

<span data-ttu-id="a815d-235">Por último, en lugar de llamar al método de cliente desde el centro directamente, la `Broadcaster` clase necesita para obtener una referencia a opera actualmente `_hubContext` concentrador.</span><span class="sxs-lookup"><span data-stu-id="a815d-235">Lastly, instead of calling the client method from the hub directly, the `Broadcaster` class needs to get a reference to the currently operating `_hubContext` hub.</span></span> <span data-ttu-id="a815d-236">Obtiene la referencia con la `GlobalHost`.</span><span class="sxs-lookup"><span data-stu-id="a815d-236">It gets the reference with the `GlobalHost`.</span></span>

## <a name="add-smooth-animation"></a><span data-ttu-id="a815d-237">Agregar una animación fluida</span><span class="sxs-lookup"><span data-stu-id="a815d-237">Add smooth animation</span></span>

<span data-ttu-id="a815d-238">La aplicación es casi hemos terminada, pero podemos hacer una sola mejora más.</span><span class="sxs-lookup"><span data-stu-id="a815d-238">The application is almost finished, but we could make one more improvement.</span></span> <span data-ttu-id="a815d-239">La aplicación mueve la forma en el cliente en respuesta a los mensajes del servidor.</span><span class="sxs-lookup"><span data-stu-id="a815d-239">The app moves the shape on the client in response to server messages.</span></span> <span data-ttu-id="a815d-240">En lugar de establecer la posición de la forma a la nueva ubicación proporcionada por el servidor, utilice la biblioteca de JQuery UI `animate` función.</span><span class="sxs-lookup"><span data-stu-id="a815d-240">Instead of setting the position of the shape to the new location given by the server, use the JQuery UI library's `animate` function.</span></span> <span data-ttu-id="a815d-241">La forma se puede mover sin problemas entre su posición actual y el nuevo.</span><span class="sxs-lookup"><span data-stu-id="a815d-241">It can move the shape smoothly between its current and new position.</span></span>

1. <span data-ttu-id="a815d-242">Actualizar el cliente `updateShape` método en el *Default.html* archivo para examinar como el código resaltado:</span><span class="sxs-lookup"><span data-stu-id="a815d-242">Update the client's `updateShape` method in the *Default.html* file to look like the highlighted code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. <span data-ttu-id="a815d-243">Seleccione el botón Reproducir para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a815d-243">Select the play button to start the application.</span></span>

1. <span data-ttu-id="a815d-244">Copie la dirección URL de la página.</span><span class="sxs-lookup"><span data-stu-id="a815d-244">Copy the page's URL.</span></span>

1. <span data-ttu-id="a815d-245">Abra un explorador y pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="a815d-245">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="a815d-246">Arrastre la forma de una de las ventanas del explorador.</span><span class="sxs-lookup"><span data-stu-id="a815d-246">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="a815d-247">El movimiento de la forma en la ventana aparece menos irregular.</span><span class="sxs-lookup"><span data-stu-id="a815d-247">The movement of the shape in the other window appears less jerky.</span></span> <span data-ttu-id="a815d-248">La aplicación interpola su movimiento a través del tiempo, en lugar de que se va a establecer una vez por cada mensaje entrante.</span><span class="sxs-lookup"><span data-stu-id="a815d-248">The app interpolates its movement over time rather than being set once per incoming message.</span></span>

<span data-ttu-id="a815d-249">Este código mueve la forma de la ubicación anterior a la nueva.</span><span class="sxs-lookup"><span data-stu-id="a815d-249">This code moves the shape from the old location to the new one.</span></span> <span data-ttu-id="a815d-250">El servidor proporciona la posición de la forma en el transcurso del intervalo de animación.</span><span class="sxs-lookup"><span data-stu-id="a815d-250">The server gives the position of the shape over the course of the animation interval.</span></span> <span data-ttu-id="a815d-251">En este caso, es 100 milisegundos.</span><span class="sxs-lookup"><span data-stu-id="a815d-251">In this case, that's 100 milliseconds.</span></span> <span data-ttu-id="a815d-252">La aplicación borra cualquier animación anterior que se ejecutan en la forma antes de que comience la nueva animación.</span><span class="sxs-lookup"><span data-stu-id="a815d-252">The app clears any previous animation running on the shape before the new animation starts.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="a815d-253">Obtención del código</span><span class="sxs-lookup"><span data-stu-id="a815d-253">Get the code</span></span>

[<span data-ttu-id="a815d-254">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="a815d-254">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

## <a name="additional-resources"></a><span data-ttu-id="a815d-255">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="a815d-255">Additional resources</span></span>

<span data-ttu-id="a815d-256">Es útil para el desarrollo de juegos en línea y otras simulaciones, al igual que el paradigma de comunicación que acaba de aprender sobre [el juego ShootR creado con SignalR](https://shootr.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="a815d-256">The communication paradigm you just learned about is useful for developing online games and other simulations, like [the ShootR game created with SignalR](https://shootr.azurewebsites.net/).</span></span>

<span data-ttu-id="a815d-257">Para obtener más información acerca de SignalR, consulte los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="a815d-257">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="a815d-258">SignalR Project</span><span class="sxs-lookup"><span data-stu-id="a815d-258">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="a815d-259">SignalR GitHub y ejemplos</span><span class="sxs-lookup"><span data-stu-id="a815d-259">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="a815d-260">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="a815d-260">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="a815d-261">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="a815d-261">Next steps</span></span>

<span data-ttu-id="a815d-262">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="a815d-262">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a815d-263">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="a815d-263">Set up the project</span></span>
> * <span data-ttu-id="a815d-264">Crea la aplicación básica</span><span class="sxs-lookup"><span data-stu-id="a815d-264">Created the base application</span></span>
> * <span data-ttu-id="a815d-265">Asignado al concentrador cuando se inicia la aplicación</span><span class="sxs-lookup"><span data-stu-id="a815d-265">Mapped to the hub when app starts</span></span>
> * <span data-ttu-id="a815d-266">Agregó el cliente</span><span class="sxs-lookup"><span data-stu-id="a815d-266">Added the client</span></span>
> * <span data-ttu-id="a815d-267">Se ejecutó la aplicación</span><span class="sxs-lookup"><span data-stu-id="a815d-267">Ran the app</span></span>
> * <span data-ttu-id="a815d-268">Agrega el bucle de cliente</span><span class="sxs-lookup"><span data-stu-id="a815d-268">Added the client loop</span></span>
> * <span data-ttu-id="a815d-269">Agrega el bucle de servidor</span><span class="sxs-lookup"><span data-stu-id="a815d-269">Added the server loop</span></span>
> * <span data-ttu-id="a815d-270">Se ha agregado una animación fluida</span><span class="sxs-lookup"><span data-stu-id="a815d-270">Added smooth animation</span></span>

<span data-ttu-id="a815d-271">Avance al siguiente artículo para obtener información sobre cómo crear una aplicación web que usa ASP.NET SignalR 2 para proporcionar funcionalidad de difusión de servidor.</span><span class="sxs-lookup"><span data-stu-id="a815d-271">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="a815d-272">SignalR 2 y difusión de servidor</span><span class="sxs-lookup"><span data-stu-id="a815d-272">SignalR 2 and server broadcasting</span></span>](tutorial-server-broadcast-with-signalr.md)