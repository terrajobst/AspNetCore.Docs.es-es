---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Tutorial: Chat en tiempo real con SignalR 2 | Microsoft Docs'
author: bradygaster
description: En este tutorial se muestra cómo usar SignalR para crear una aplicación de chat en tiempo real. Agregar SignalR a una aplicación web ASP.NET vacía.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 90f2c03fbda522e3a46200bc0132cc74100ce70f
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836797"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a><span data-ttu-id="76909-104">Tutorial: Chat en tiempo real con SignalR 2</span><span class="sxs-lookup"><span data-stu-id="76909-104">Tutorial: Real-time chat with SignalR 2</span></span>

<span data-ttu-id="76909-105">Este tutorial muestra cómo usar SignalR para crear una aplicación de chat en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="76909-105">This tutorial shows you how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="76909-106">Agregar SignalR a una aplicación web ASP.NET vacía y cree una página HTML para enviar y mostrar mensajes.</span><span class="sxs-lookup"><span data-stu-id="76909-106">You add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>

<span data-ttu-id="76909-107">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="76909-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="76909-108">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="76909-108">Set up the project</span></span>
> * <span data-ttu-id="76909-109">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="76909-109">Run the sample</span></span>
> * <span data-ttu-id="76909-110">Examine el código</span><span class="sxs-lookup"><span data-stu-id="76909-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="76909-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="76909-111">Prerequisites</span></span>

* <span data-ttu-id="76909-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con el **ASP.NET y desarrollo web** carga de trabajo.</span><span class="sxs-lookup"><span data-stu-id="76909-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="76909-113">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="76909-113">Set up the Project</span></span>

<span data-ttu-id="76909-114">En esta sección se muestra cómo usar Visual Studio 2017 y SignalR 2 para crear una aplicación web ASP.NET vacía, agregar SignalR y crear la aplicación de chat.</span><span class="sxs-lookup"><span data-stu-id="76909-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

1. <span data-ttu-id="76909-115">En Visual Studio, cree una aplicación Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="76909-115">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Creación de web](tutorial-getting-started-with-signalr/_static/image2.png)

1. <span data-ttu-id="76909-117">En el **nuevo proyecto ASP.NET - SignalRChat** ventana, deje **vacía** seleccionado y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="76909-117">In the **New ASP.NET Project - SignalRChat** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="76909-118">En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="76909-118">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="76909-119">En **Agregar nuevo elemento - SignalRChat**, seleccione **instalado** > **Visual C#**   >  **Web**  >  **SignalR** y, a continuación, seleccione **clase de concentrador de SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="76909-119">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="76909-120">Nombre de la clase *ChatHub* y agréguelo al proyecto.</span><span class="sxs-lookup"><span data-stu-id="76909-120">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="76909-121">Este paso se crea el *ChatHub.cs* archivo de clase y agrega un conjunto de archivos de script y las referencias de ensamblado que admiten SignalR al proyecto.</span><span class="sxs-lookup"><span data-stu-id="76909-121">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="76909-122">Reemplace el código en el nuevo *ChatHub.cs* archivo de clase con este código:</span><span class="sxs-lookup"><span data-stu-id="76909-122">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="76909-123">En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="76909-123">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="76909-124">En **Agregar nuevo elemento - SignalRChat** seleccione **instalado** > **Visual C#**   >  **Web** y, a continuación, Seleccione **clase de inicio OWIN**.</span><span class="sxs-lookup"><span data-stu-id="76909-124">In **Add New Item - SignalRChat** select **Installed** > **Visual C#** > **Web**  and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="76909-125">Nombre de la clase *inicio* y agréguelo al proyecto.</span><span class="sxs-lookup"><span data-stu-id="76909-125">Name the class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="76909-126">En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **página HTML**.</span><span class="sxs-lookup"><span data-stu-id="76909-126">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="76909-127">Nombre de la nueva página *índice* y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="76909-127">Name the new page *index* and select **OK**.</span></span>

1. <span data-ttu-id="76909-128">En **el Explorador de soluciones**, haga clic en la página HTML que creó y seleccione **establecer como página de inicio**.</span><span class="sxs-lookup"><span data-stu-id="76909-128">In **Solution Explorer**, right-click the HTML page you created and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="76909-129">Reemplace el código predeterminado en la página HTML con este código:</span><span class="sxs-lookup"><span data-stu-id="76909-129">Replace the default code in the HTML page with this code:</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. <span data-ttu-id="76909-130">En **el Explorador de soluciones**, expanda **Scripts**.</span><span class="sxs-lookup"><span data-stu-id="76909-130">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="76909-131">Bibliotecas de scripts de jQuery y SignalR están visibles en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="76909-131">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="76909-132">El Administrador de paquetes que ha instalado una versión posterior de los scripts de SignalR.</span><span class="sxs-lookup"><span data-stu-id="76909-132">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="76909-133">Compruebe que las referencias de script en el bloque de código se corresponden con las versiones de los archivos de script en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="76909-133">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="76909-134">Referencias de script desde el bloque de código original:</span><span class="sxs-lookup"><span data-stu-id="76909-134">Script references from the original code block:</span></span>
    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. <span data-ttu-id="76909-135">Si no coinciden, actualice el *.html* archivo.</span><span class="sxs-lookup"><span data-stu-id="76909-135">If they don't match, update the *.html* file.</span></span>

1. <span data-ttu-id="76909-136">En la barra de menús, seleccione **archivo** > **guardar todo**.</span><span class="sxs-lookup"><span data-stu-id="76909-136">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="76909-137">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="76909-137">Run the Sample</span></span>

1. <span data-ttu-id="76909-138">En la barra de herramientas activar **depuración de scripts** y, a continuación, seleccione el botón Reproducir para ejecutar el ejemplo en modo de depuración.</span><span class="sxs-lookup"><span data-stu-id="76909-138">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr/_static/image3.png)

1. <span data-ttu-id="76909-140">Cuando se abre el explorador, escriba un nombre de la identidad de chat.</span><span class="sxs-lookup"><span data-stu-id="76909-140">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="76909-141">Copie la dirección URL desde el explorador, abra dos otros exploradores y pegue las direcciones URL en las barras de dirección.</span><span class="sxs-lookup"><span data-stu-id="76909-141">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="76909-142">En cada explorador, escriba un nombre único.</span><span class="sxs-lookup"><span data-stu-id="76909-142">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="76909-143">Ahora, agregue un comentario y seleccione **enviar**.</span><span class="sxs-lookup"><span data-stu-id="76909-143">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="76909-144">Se repiten en los otros exploradores.</span><span class="sxs-lookup"><span data-stu-id="76909-144">Repeat that in the other browsers.</span></span> <span data-ttu-id="76909-145">Los comentarios aparecen en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="76909-145">The comments appear in real-time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="76909-146">Esta aplicación de chat simple no mantiene el contexto de discusión en el servidor.</span><span class="sxs-lookup"><span data-stu-id="76909-146">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="76909-147">El concentrador difunde comentarios a todos los usuarios actuales.</span><span class="sxs-lookup"><span data-stu-id="76909-147">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="76909-148">Los usuarios que se unan a la charla más adelante verá mensajes agregados desde el momento en que se unen a.</span><span class="sxs-lookup"><span data-stu-id="76909-148">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="76909-149">Vea cómo se ejecuta la aplicación de chat en tres diferentes exploradores.</span><span class="sxs-lookup"><span data-stu-id="76909-149">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="76909-150">Cuando Tom, Anand y Susan envían mensajes, actualizarán todos los exploradores en tiempo real:</span><span class="sxs-lookup"><span data-stu-id="76909-150">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Todos los tres exploradores mostrar al mismo historial de chat](tutorial-getting-started-with-signalr/_static/image4.png)

1. <span data-ttu-id="76909-152">En **el Explorador de soluciones**, inspeccionar la **documentos de Script** nodo para la aplicación en ejecución.</span><span class="sxs-lookup"><span data-stu-id="76909-152">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="76909-153">Hay un archivo de script denominado *hubs* que genera la biblioteca SignalR en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="76909-153">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="76909-154">Este archivo administra la comunicación entre el script de jQuery y el código de servidor.</span><span class="sxs-lookup"><span data-stu-id="76909-154">This file manages the communication between jQuery script and server-side code.</span></span>

    ![script de concentradores generado automáticamente en el nodo documentos de Script](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="76909-156">Examine el código</span><span class="sxs-lookup"><span data-stu-id="76909-156">Examine the Code</span></span>

<span data-ttu-id="76909-157">La aplicación SignalRChat muestra dos tareas básicas de desarrollo de SignalR.</span><span class="sxs-lookup"><span data-stu-id="76909-157">The SignalRChat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="76909-158">Muestra cómo crear un concentrador.</span><span class="sxs-lookup"><span data-stu-id="76909-158">It shows you how to create a hub.</span></span> <span data-ttu-id="76909-159">El servidor usa ese concentrador como el objeto principal de coordinación.</span><span class="sxs-lookup"><span data-stu-id="76909-159">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="76909-160">El concentrador usa la biblioteca de jQuery de SignalR para enviar y recibir mensajes.</span><span class="sxs-lookup"><span data-stu-id="76909-160">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="76909-161">Concentradores de SignalR en el ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="76909-161">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="76909-162">En el ejemplo de código anterior, el `ChatHub` clase se deriva de la `Microsoft.AspNet.SignalR.Hub` clase.</span><span class="sxs-lookup"><span data-stu-id="76909-162">In the code sample above, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="76909-163">Derivar de la `Hub` clase es una forma útil de compilar una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="76909-163">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="76909-164">Puede crear métodos públicos en la clase de concentrador y, a continuación, usar estos métodos al llamarlas desde secuencias de comandos en una página web.</span><span class="sxs-lookup"><span data-stu-id="76909-164">You can create public methods on your hub class and then use those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="76909-165">En el código de chat, llame los clientes la `ChatHub.Send` método para enviar un mensaje nuevo.</span><span class="sxs-lookup"><span data-stu-id="76909-165">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="76909-166">El concentrador, a continuación, envía el mensaje a todos los clientes mediante una llamada a `Clients.All.broadcastMessage`.</span><span class="sxs-lookup"><span data-stu-id="76909-166">The hub then sends the message to all clients by calling `Clients.All.broadcastMessage`.</span></span>

<span data-ttu-id="76909-167">El `Send` método muestra varios conceptos de hub:</span><span class="sxs-lookup"><span data-stu-id="76909-167">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="76909-168">Declarar métodos públicos en un centro de modo que los clientes se les pueden llamar.</span><span class="sxs-lookup"><span data-stu-id="76909-168">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="76909-169">Use la `Microsoft.AspNet.SignalR.Hub.Clients` propiedades dinámicas para comunicarse con todos los clientes conectados a este centro.</span><span class="sxs-lookup"><span data-stu-id="76909-169">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="76909-170">Llamar a una función en el cliente (como el `broadcastMessage` función) para actualizar los clientes.</span><span class="sxs-lookup"><span data-stu-id="76909-170">Call a function on the client (like the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a><span data-ttu-id="76909-171">SignalR y jQuery en el archivo index.html</span><span class="sxs-lookup"><span data-stu-id="76909-171">SignalR and jQuery in the index.html</span></span>

<span data-ttu-id="76909-172">El *index.html* página en el ejemplo de código muestra cómo usar la biblioteca de jQuery SignalR para comunicarse con un concentrador SignalR.</span><span class="sxs-lookup"><span data-stu-id="76909-172">The *index.html* page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="76909-173">El código lleva a cabo muchas tareas importantes.</span><span class="sxs-lookup"><span data-stu-id="76909-173">The code carries out many important tasks.</span></span> <span data-ttu-id="76909-174">Declara un proxy para hacer referencia al concentrador, declara una función que el servidor puede llamar para insertar contenido a los clientes y se inicia una conexión para enviar mensajes al concentrador.</span><span class="sxs-lookup"><span data-stu-id="76909-174">It declares a proxy to reference the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="76909-175">En JavaScript la referencia a la clase de servidor y sus miembros tiene que ser camelCase.</span><span class="sxs-lookup"><span data-stu-id="76909-175">In JavaScript the reference to the server class and its members has to be camelCase.</span></span> <span data-ttu-id="76909-176">Las referencias de ejemplo de código la C# *ChatHub* clases en JavaScript como `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="76909-176">The code sample references the C# *ChatHub* class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="76909-177">En este bloque de código, cree una función de devolución de llamada en la secuencia de comandos.</span><span class="sxs-lookup"><span data-stu-id="76909-177">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="76909-178">La clase de hub en el servidor llama a esta función para insertar las actualizaciones de contenido en cada cliente.</span><span class="sxs-lookup"><span data-stu-id="76909-178">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="76909-179">Las dos líneas que codificar en HTML el contenido antes de mostrarla son opcionales y mostrar una buena forma de evitar la inyección de script.</span><span class="sxs-lookup"><span data-stu-id="76909-179">The two lines that HTML-encode the content before displaying it are optional and show a good way to prevent script injection.</span></span>

<span data-ttu-id="76909-180">Este código abre una conexión con el centro.</span><span class="sxs-lookup"><span data-stu-id="76909-180">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> <span data-ttu-id="76909-181">Este enfoque garantiza que el código establece una conexión antes de que se ejecuta el controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="76909-181">This approach ensures that the code establishes a connection before the event handler executes.</span></span>

<span data-ttu-id="76909-182">El código inicia la conexión y, a continuación, pasa una función para controlar el evento click en la **enviar** botón en la página HTML.</span><span class="sxs-lookup"><span data-stu-id="76909-182">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="76909-183">Obtención del código</span><span class="sxs-lookup"><span data-stu-id="76909-183">Get the code</span></span>

[<span data-ttu-id="76909-184">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="76909-184">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

## <a name="additional-resources"></a><span data-ttu-id="76909-185">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="76909-185">Additional resources</span></span>

<span data-ttu-id="76909-186">Para obtener más información acerca de SignalR, consulte los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="76909-186">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="76909-187">SignalR Project</span><span class="sxs-lookup"><span data-stu-id="76909-187">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="76909-188">SignalR Github y ejemplos</span><span class="sxs-lookup"><span data-stu-id="76909-188">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="76909-189">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="76909-189">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="76909-190">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="76909-190">Next steps</span></span>

<span data-ttu-id="76909-191">En este tutorial le:</span><span class="sxs-lookup"><span data-stu-id="76909-191">In this tutorial you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="76909-192">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="76909-192">Set up the project</span></span>
> * <span data-ttu-id="76909-193">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="76909-193">Ran the sample</span></span>
> * <span data-ttu-id="76909-194">Examinar el código</span><span class="sxs-lookup"><span data-stu-id="76909-194">Examined the code</span></span>

<span data-ttu-id="76909-195">Avance al siguiente artículo para obtener información sobre cómo usar SignalR y MVC 5.</span><span class="sxs-lookup"><span data-stu-id="76909-195">Advance to the next article to learn how to use SignalR and MVC 5.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="76909-196">SignalR 2 y MVC 5</span><span class="sxs-lookup"><span data-stu-id="76909-196">SignalR 2 and MVC 5</span></span>](tutorial-getting-started-with-signalr-and-mvc.md)