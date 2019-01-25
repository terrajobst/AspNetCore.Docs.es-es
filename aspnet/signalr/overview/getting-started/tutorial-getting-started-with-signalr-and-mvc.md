---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Tutorial: Chat en tiempo real con SignalR 2 y MVC 5. | Microsoft Docs'
author: bradygaster
description: Este tutorial muestra cómo usar ASP.NET SignalR 2 para crear una aplicación de chat en tiempo real. Agregar SignalR a una aplicación de MVC 5.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 1b02aecc68a93dbd6373ca5304530e76c9d0b6b5
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837006"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a><span data-ttu-id="2e50f-104">Tutorial: Chat en tiempo real con SignalR 2 y MVC 5</span><span class="sxs-lookup"><span data-stu-id="2e50f-104">Tutorial: Real-time chat with SignalR 2 and MVC 5</span></span>

<span data-ttu-id="2e50f-105">Este tutorial muestra cómo usar ASP.NET SignalR 2 para crear una aplicación de chat en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="2e50f-105">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="2e50f-106">Agregar SignalR a una aplicación de MVC 5 y crear una vista de conversación para enviar y mostrar mensajes.</span><span class="sxs-lookup"><span data-stu-id="2e50f-106">You add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span>

<span data-ttu-id="2e50f-107">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="2e50f-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2e50f-108">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="2e50f-108">Set up the project</span></span>
> * <span data-ttu-id="2e50f-109">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="2e50f-109">Run the sample</span></span>
> * <span data-ttu-id="2e50f-110">Examine el código</span><span class="sxs-lookup"><span data-stu-id="2e50f-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="2e50f-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="2e50f-111">Prerequisites</span></span>

* <span data-ttu-id="2e50f-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con el **ASP.NET y desarrollo web** carga de trabajo.</span><span class="sxs-lookup"><span data-stu-id="2e50f-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="2e50f-113">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="2e50f-113">Set up the Project</span></span>

<span data-ttu-id="2e50f-114">En esta sección se muestra cómo usar Visual Studio 2017 y SignalR 2 para crear una aplicación ASP.NET MVC 5 vacía, agregue la biblioteca SignalR y crear la aplicación de chat.</span><span class="sxs-lookup"><span data-stu-id="2e50f-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="2e50f-115">En Visual Studio, cree una aplicación ASP.NET de C# que tiene como destino .NET Framework 4.5, asígnele el nombre SignalRChat y haga clic en Aceptar.</span><span class="sxs-lookup"><span data-stu-id="2e50f-115">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![Creación de web](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. <span data-ttu-id="2e50f-117">En **nueva aplicación Web de ASP.NET - SignalRMvcChat**, seleccione **MVC** y, a continuación, seleccione **Cambiar autenticación**.</span><span class="sxs-lookup"><span data-stu-id="2e50f-117">In **New ASP.NET Web Application - SignalRMvcChat**, select **MVC** and then select **Change Authentication**.</span></span>

1. <span data-ttu-id="2e50f-118">En **Cambiar autenticación**, seleccione **sin autenticación** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="2e50f-118">In **Change Authentication**, select **No Authentication** and click **OK**.</span></span>

    ![No seleccione autenticación](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. <span data-ttu-id="2e50f-120">En **nueva aplicación Web de ASP.NET - SignalRMvcChat**, seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="2e50f-120">In **New ASP.NET Web Application - SignalRMvcChat**, select **OK**.</span></span>

1. <span data-ttu-id="2e50f-121">En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="2e50f-121">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="2e50f-122">En **Agregar nuevo elemento - SignalRChat**, seleccione **instalado** > **Visual C#**   >  **Web**  >  **SignalR** y, a continuación, seleccione **clase de concentrador de SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="2e50f-122">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="2e50f-123">Nombre de la clase *ChatHub* y agréguelo al proyecto.</span><span class="sxs-lookup"><span data-stu-id="2e50f-123">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="2e50f-124">Este paso se crea el *ChatHub.cs* archivo de clase y agrega un conjunto de archivos de script y las referencias de ensamblado que admiten SignalR al proyecto.</span><span class="sxs-lookup"><span data-stu-id="2e50f-124">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="2e50f-125">Reemplace el código en el nuevo *ChatHub.cs* archivo de clase con este código:</span><span class="sxs-lookup"><span data-stu-id="2e50f-125">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. <span data-ttu-id="2e50f-126">En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **clase**.</span><span class="sxs-lookup"><span data-stu-id="2e50f-126">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="2e50f-127">Nombre de la nueva clase *inicio* y agréguelo al proyecto.</span><span class="sxs-lookup"><span data-stu-id="2e50f-127">Name the new class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="2e50f-128">Reemplace el código en el *Startup.cs* archivo de clase con este código:</span><span class="sxs-lookup"><span data-stu-id="2e50f-128">Replace the code in the *Startup.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. <span data-ttu-id="2e50f-129">En **el Explorador de soluciones**, seleccione **controladores** > **HomeController.cs**.</span><span class="sxs-lookup"><span data-stu-id="2e50f-129">In **Solution Explorer**, select **Controllers** > **HomeController.cs**.</span></span>

1. <span data-ttu-id="2e50f-130">Agregue este método para el *HomeController.cs*.</span><span class="sxs-lookup"><span data-stu-id="2e50f-130">Add this method to the *HomeController.cs*.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    <span data-ttu-id="2e50f-131">Este método devuelve el **Chat** vista que se crea en un paso posterior.</span><span class="sxs-lookup"><span data-stu-id="2e50f-131">This method returns the **Chat** view that you create in a later step.</span></span>

1. <span data-ttu-id="2e50f-132">En **el Explorador de soluciones**, haga clic en **vistas** > **inicio**y seleccione **agregar**  >    **Vista**.</span><span class="sxs-lookup"><span data-stu-id="2e50f-132">In **Solution Explorer**, right-click **Views** > **Home**, and select **Add** >  **View**.</span></span>

1. <span data-ttu-id="2e50f-133">En **agregar vista**, nombre la nueva vista **Chat** y seleccione **agregar**.</span><span class="sxs-lookup"><span data-stu-id="2e50f-133">In **Add View**, name the new view **Chat** and select **Add**.</span></span>

1. <span data-ttu-id="2e50f-134">Reemplace el contenido de **Chat.cshtml** con este código:</span><span class="sxs-lookup"><span data-stu-id="2e50f-134">Replace the contents of **Chat.cshtml** with this code:</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. <span data-ttu-id="2e50f-135">En **el Explorador de soluciones**, expanda **Scripts**.</span><span class="sxs-lookup"><span data-stu-id="2e50f-135">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="2e50f-136">Bibliotecas de scripts de jQuery y SignalR están visibles en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="2e50f-136">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="2e50f-137">El Administrador de paquetes que ha instalado una versión posterior de los scripts de SignalR.</span><span class="sxs-lookup"><span data-stu-id="2e50f-137">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="2e50f-138">Compruebe que las referencias de script en el bloque de código se corresponden con las versiones de los archivos de script en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="2e50f-138">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="2e50f-139">Referencias de script desde el bloque de código original:</span><span class="sxs-lookup"><span data-stu-id="2e50f-139">Script references from the original code block:</span></span>

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. <span data-ttu-id="2e50f-140">Si no coinciden, actualice el *.cshtml* archivo.</span><span class="sxs-lookup"><span data-stu-id="2e50f-140">If they don't match, update the *.cshtml* file.</span></span>

1. <span data-ttu-id="2e50f-141">En la barra de menús, seleccione **archivo** > **guardar todo**.</span><span class="sxs-lookup"><span data-stu-id="2e50f-141">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="2e50f-142">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="2e50f-142">Run the Sample</span></span>

1. <span data-ttu-id="2e50f-143">En la barra de herramientas activar **depuración de scripts** y, a continuación, seleccione el botón Reproducir para ejecutar el ejemplo en modo de depuración.</span><span class="sxs-lookup"><span data-stu-id="2e50f-143">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. <span data-ttu-id="2e50f-145">Cuando se abre el explorador, escriba un nombre de la identidad de chat.</span><span class="sxs-lookup"><span data-stu-id="2e50f-145">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="2e50f-146">Copie la dirección URL desde el explorador, abra dos otros exploradores y pegue las direcciones URL en las barras de dirección.</span><span class="sxs-lookup"><span data-stu-id="2e50f-146">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="2e50f-147">En cada explorador, escriba un nombre único.</span><span class="sxs-lookup"><span data-stu-id="2e50f-147">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="2e50f-148">Ahora, agregue un comentario y seleccione **enviar**.</span><span class="sxs-lookup"><span data-stu-id="2e50f-148">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="2e50f-149">Se repiten en los otros exploradores.</span><span class="sxs-lookup"><span data-stu-id="2e50f-149">Repeat that in the other browsers.</span></span> <span data-ttu-id="2e50f-150">Los comentarios aparecen en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="2e50f-150">The comments appear in real time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2e50f-151">Esta aplicación de chat simple no mantiene el contexto de discusión en el servidor.</span><span class="sxs-lookup"><span data-stu-id="2e50f-151">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="2e50f-152">El concentrador difunde comentarios a todos los usuarios actuales.</span><span class="sxs-lookup"><span data-stu-id="2e50f-152">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="2e50f-153">Los usuarios que se unan a la charla más adelante verá mensajes agregados desde el momento en que se unen a.</span><span class="sxs-lookup"><span data-stu-id="2e50f-153">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="2e50f-154">Vea cómo se ejecuta la aplicación de chat en tres diferentes exploradores.</span><span class="sxs-lookup"><span data-stu-id="2e50f-154">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="2e50f-155">Cuando Tom, Anand y Susan envían mensajes, actualizarán todos los exploradores en tiempo real:</span><span class="sxs-lookup"><span data-stu-id="2e50f-155">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Todos los tres exploradores mostrar al mismo historial de chat](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. <span data-ttu-id="2e50f-157">En **el Explorador de soluciones**, inspeccionar la **documentos de Script** nodo para la aplicación en ejecución.</span><span class="sxs-lookup"><span data-stu-id="2e50f-157">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="2e50f-158">Hay un archivo de script denominado *hubs* que genera la biblioteca SignalR en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="2e50f-158">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="2e50f-159">Este archivo administra la comunicación entre el script de jQuery y el código de servidor.</span><span class="sxs-lookup"><span data-stu-id="2e50f-159">This file manages the communication between jQuery script and server-side code.</span></span>

    ![script de concentradores generado automáticamente en el nodo documentos de Script](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="2e50f-161">Examine el código</span><span class="sxs-lookup"><span data-stu-id="2e50f-161">Examine the Code</span></span>

<span data-ttu-id="2e50f-162">La aplicación de chat de SignalR muestra dos tareas básicas de desarrollo de SignalR.</span><span class="sxs-lookup"><span data-stu-id="2e50f-162">The SignalR chat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="2e50f-163">Muestra cómo crear un concentrador.</span><span class="sxs-lookup"><span data-stu-id="2e50f-163">It shows you how to create a hub.</span></span> <span data-ttu-id="2e50f-164">El servidor usa ese concentrador como el objeto principal de coordinación.</span><span class="sxs-lookup"><span data-stu-id="2e50f-164">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="2e50f-165">El concentrador usa la biblioteca de jQuery de SignalR para enviar y recibir mensajes.</span><span class="sxs-lookup"><span data-stu-id="2e50f-165">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="2e50f-166">Concentradores de SignalR en el ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="2e50f-166">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="2e50f-167">En el ejemplo de código, el `ChatHub` clase se deriva de la `Microsoft.AspNet.SignalR.Hub` clase.</span><span class="sxs-lookup"><span data-stu-id="2e50f-167">In the code sample, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="2e50f-168">Derivar de la `Hub` clase es una forma útil de compilar una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="2e50f-168">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="2e50f-169">Puede crear métodos públicos en la clase de concentrador y, a continuación, obtener acceso a esos métodos al llamarlas desde secuencias de comandos en una página web.</span><span class="sxs-lookup"><span data-stu-id="2e50f-169">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="2e50f-170">En el código de chat, llame los clientes la `ChatHub.Send` método para enviar un mensaje nuevo.</span><span class="sxs-lookup"><span data-stu-id="2e50f-170">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="2e50f-171">El concentrador a su vez envía el mensaje a todos los clientes mediante una llamada a `Clients.All.addNewMessageToPage`.</span><span class="sxs-lookup"><span data-stu-id="2e50f-171">The hub in turn sends the message to all clients by calling `Clients.All.addNewMessageToPage`.</span></span>

<span data-ttu-id="2e50f-172">El `Send` método muestra varios conceptos de hub:</span><span class="sxs-lookup"><span data-stu-id="2e50f-172">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="2e50f-173">Declarar métodos públicos en un centro de modo que los clientes se les pueden llamar.</span><span class="sxs-lookup"><span data-stu-id="2e50f-173">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="2e50f-174">Use la `Microsoft.AspNet.SignalR.Hub.Clients` propiedades dinámicas para comunicarse con todos los clientes conectados a este centro.</span><span class="sxs-lookup"><span data-stu-id="2e50f-174">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="2e50f-175">Llamar a una función en el cliente (como el `addNewMessageToPage` función) para actualizar los clientes.</span><span class="sxs-lookup"><span data-stu-id="2e50f-175">Call a function on the client (like the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a><span data-ttu-id="2e50f-176">SignalR y jQuery Chat.cshtml</span><span class="sxs-lookup"><span data-stu-id="2e50f-176">SignalR and jQuery Chat.cshtml</span></span>

<span data-ttu-id="2e50f-177">El *Chat.cshtml* Ver archivo en el ejemplo de código muestra cómo usar la biblioteca de jQuery SignalR para comunicarse con un concentrador SignalR.</span><span class="sxs-lookup"><span data-stu-id="2e50f-177">The *Chat.cshtml* view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span>  <span data-ttu-id="2e50f-178">El código lleva a cabo muchas tareas importantes.</span><span class="sxs-lookup"><span data-stu-id="2e50f-178">The code carries out many important tasks.</span></span> <span data-ttu-id="2e50f-179">Crea una referencia al proxy generado automáticamente para el concentrador, declara una función que el servidor puede llamar para insertar contenido en clientes y se inicia una conexión para enviar mensajes al concentrador.</span><span class="sxs-lookup"><span data-stu-id="2e50f-179">It creates a reference to the autogenerated proxy for the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="2e50f-180">En JavaScript, la referencia a la clase de servidor y sus miembros se encuentra en camelCase.</span><span class="sxs-lookup"><span data-stu-id="2e50f-180">In JavaScript, the reference to the server class and its members is in camelCase.</span></span> <span data-ttu-id="2e50f-181">Las referencias de ejemplo de código la C# `ChatHub` clases en JavaScript como `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="2e50f-181">The code sample references the C# `ChatHub` class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="2e50f-182">En este bloque de código, cree una función de devolución de llamada en la secuencia de comandos.</span><span class="sxs-lookup"><span data-stu-id="2e50f-182">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="2e50f-183">La clase de hub en el servidor llama a esta función para insertar las actualizaciones de contenido en cada cliente.</span><span class="sxs-lookup"><span data-stu-id="2e50f-183">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="2e50f-184">La llamada opcional para el `htmlEncode` función se muestra un camino hacia HTML codifica el contenido del mensaje antes de mostrarla en la página.</span><span class="sxs-lookup"><span data-stu-id="2e50f-184">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page.</span></span> <span data-ttu-id="2e50f-185">Es una manera de evitar la inyección de script.</span><span class="sxs-lookup"><span data-stu-id="2e50f-185">It's a way to prevent script injection.</span></span>

<span data-ttu-id="2e50f-186">Este código abre una conexión con el centro.</span><span class="sxs-lookup"><span data-stu-id="2e50f-186">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> <span data-ttu-id="2e50f-187">Este enfoque garantiza que establecer una conexión antes de que se ejecuta el controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="2e50f-187">This approach ensures that you establish a connection before the event handler executes.</span></span>

<span data-ttu-id="2e50f-188">El código inicia la conexión y, a continuación, pasa una función para controlar el evento click en la **enviar** botón en la página de Chat.</span><span class="sxs-lookup"><span data-stu-id="2e50f-188">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="2e50f-189">Obtención del código</span><span class="sxs-lookup"><span data-stu-id="2e50f-189">Get the code</span></span>

[<span data-ttu-id="2e50f-190">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="2e50f-190">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

## <a name="additional-resources"></a><span data-ttu-id="2e50f-191">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="2e50f-191">Additional resources</span></span>

<span data-ttu-id="2e50f-192">Para obtener más información acerca de SignalR, consulte los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="2e50f-192">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="2e50f-193">SignalR Project</span><span class="sxs-lookup"><span data-stu-id="2e50f-193">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="2e50f-194">SignalR GitHub y ejemplos</span><span class="sxs-lookup"><span data-stu-id="2e50f-194">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="2e50f-195">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="2e50f-195">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="2e50f-196">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="2e50f-196">Next steps</span></span>

<span data-ttu-id="2e50f-197">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="2e50f-197">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2e50f-198">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="2e50f-198">Set up the project</span></span>
> * <span data-ttu-id="2e50f-199">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="2e50f-199">Ran the sample</span></span>
> * <span data-ttu-id="2e50f-200">Examinar el código</span><span class="sxs-lookup"><span data-stu-id="2e50f-200">Examined the code</span></span>

<span data-ttu-id="2e50f-201">Avance al siguiente artículo para obtener información sobre cómo crear una aplicación web que usa ASP.NET SignalR 2 para proporcionar funcionalidad de mensajería de alta frecuencia.</span><span class="sxs-lookup"><span data-stu-id="2e50f-201">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="2e50f-202">Aplicación Web con la mensajería de alta frecuencia</span><span class="sxs-lookup"><span data-stu-id="2e50f-202">Web app with high-frequency messaging</span></span>](tutorial-high-frequency-realtime-with-signalr.md)