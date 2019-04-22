---
title: Cliente de .NET de ASP.NET Core SignalR
author: bradygaster
description: Información sobre el cliente de .NET de ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/dotnet-client
ms.openlocfilehash: 640d75157e42ffa6d78235c5be03e4846e8dcde9
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982951"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="ae209-103">Cliente de .NET de ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="ae209-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="ae209-104">La biblioteca de cliente de ASP.NET Core SignalR .NET le permite comunicarse con los concentradores de SignalR desde aplicaciones de. NET.</span><span class="sxs-lookup"><span data-stu-id="ae209-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="ae209-105">Xamarin tiene requisitos especiales para la versión de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ae209-105">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="ae209-106">Para obtener más información, consulte [2.1.1 de cliente de SignalR en Xamarin](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="ae209-106">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="ae209-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ae209-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ae209-108">El ejemplo de código en este artículo es una aplicación WPF que usa al cliente de .NET de ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="ae209-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="ae209-109">Instale el paquete de cliente .NET de SignalR</span><span class="sxs-lookup"><span data-stu-id="ae209-109">Install the SignalR .NET client package</span></span>

<span data-ttu-id="ae209-110">El `Microsoft.AspNetCore.SignalR.Client` paquete es necesario para que los clientes de .NET para conectarse a los concentradores de SignalR.</span><span class="sxs-lookup"><span data-stu-id="ae209-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="ae209-111">Para instalar la biblioteca de cliente, ejecute el siguiente comando en el **Package Manager Console** ventana:</span><span class="sxs-lookup"><span data-stu-id="ae209-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="ae209-112">Conectarse a un concentrador</span><span class="sxs-lookup"><span data-stu-id="ae209-112">Connect to a hub</span></span>

<span data-ttu-id="ae209-113">Para establecer una conexión, cree un `HubConnectionBuilder` y llamar a `Build`.</span><span class="sxs-lookup"><span data-stu-id="ae209-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="ae209-114">Durante la compilación de una conexión se pueden configurar la URL del centro, protocolo, tipo de transporte, nivel de registro, los encabezados y otras opciones.</span><span class="sxs-lookup"><span data-stu-id="ae209-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="ae209-115">Configurar las opciones necesarias mediante la inserción de cualquiera de los `HubConnectionBuilder` métodos en `Build`.</span><span class="sxs-lookup"><span data-stu-id="ae209-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="ae209-116">Iniciar la conexión con `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="ae209-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="ae209-117">Controlar la pérdida de conexión</span><span class="sxs-lookup"><span data-stu-id="ae209-117">Handle lost connection</span></span>

<span data-ttu-id="ae209-118">Use el <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> eventos para responder a una pérdida de conexión.</span><span class="sxs-lookup"><span data-stu-id="ae209-118">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="ae209-119">Por ejemplo, es posible que desee automatizar la reconexión.</span><span class="sxs-lookup"><span data-stu-id="ae209-119">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="ae209-120">El `Closed` evento requiere un delegado que devuelve un `Task`, lo que permite ejecutar sin usar código asincrónico `async void`.</span><span class="sxs-lookup"><span data-stu-id="ae209-120">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="ae209-121">Para cumplir con la firma del delegado en un `Closed` controlador de eventos que se ejecuta de forma sincrónica, devolver `Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="ae209-121">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="ae209-122">La razón principal para el soporte para asincronía es por lo que puede reiniciar la conexión.</span><span class="sxs-lookup"><span data-stu-id="ae209-122">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="ae209-123">A partir de una conexión es una acción asincrónica.</span><span class="sxs-lookup"><span data-stu-id="ae209-123">Starting a connection is an async action.</span></span>

<span data-ttu-id="ae209-124">En un `Closed` controlador que se reinicia la conexión, considere la posibilidad de esperar a cierto retraso aleatorio evitar la sobrecarga del servidor, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ae209-124">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="ae209-125">Llamar a métodos de concentrador de cliente</span><span class="sxs-lookup"><span data-stu-id="ae209-125">Call hub methods from client</span></span>

<span data-ttu-id="ae209-126">`InvokeAsync` llama a métodos en el concentrador.</span><span class="sxs-lookup"><span data-stu-id="ae209-126">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="ae209-127">Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="ae209-127">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="ae209-128">SignalR es asincrónica, así que use `async` y `await` al realizar las llamadas.</span><span class="sxs-lookup"><span data-stu-id="ae209-128">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

<span data-ttu-id="ae209-129">El `InvokeAsync` método devuelve un `Task` que se completa cuando finaliza el método de servidor.</span><span class="sxs-lookup"><span data-stu-id="ae209-129">The `InvokeAsync` method returns a `Task` which completes when the server method returns.</span></span> <span data-ttu-id="ae209-130">El valor devuelto, si existe, se proporciona como el resultado de la `Task`.</span><span class="sxs-lookup"><span data-stu-id="ae209-130">The return value, if any, is provided as the result of the `Task`.</span></span> <span data-ttu-id="ae209-131">Las excepciones producidas por el método en el servidor generan un error `Task`.</span><span class="sxs-lookup"><span data-stu-id="ae209-131">Any exceptions thrown by the method on the server produce a faulted `Task`.</span></span> <span data-ttu-id="ae209-132">Use `await` sintaxis que espere a que el método de servidor completar y `try...catch` sintaxis para controlar los errores.</span><span class="sxs-lookup"><span data-stu-id="ae209-132">Use `await` syntax to wait for the server method to complete and `try...catch` syntax to handle errors.</span></span>

<span data-ttu-id="ae209-133">El `SendAsync` método devuelve un `Task` que se completa cuando el mensaje se envió al servidor.</span><span class="sxs-lookup"><span data-stu-id="ae209-133">The `SendAsync` method returns a `Task` which completes when the message has been sent to the server.</span></span> <span data-ttu-id="ae209-134">Se proporciona ningún valor devuelto, ya que `Task` no espere hasta que finalice el método de servidor.</span><span class="sxs-lookup"><span data-stu-id="ae209-134">No return value is provided since this `Task` doesn't wait until the server method completes.</span></span> <span data-ttu-id="ae209-135">Las excepciones producidas en el cliente al enviar el mensaje generan un error `Task`.</span><span class="sxs-lookup"><span data-stu-id="ae209-135">Any exceptions thrown on the client while sending the message produce a faulted `Task`.</span></span> <span data-ttu-id="ae209-136">Use `await` y `try...catch` sintaxis para controlar errores de envío.</span><span class="sxs-lookup"><span data-stu-id="ae209-136">Use `await` and `try...catch` syntax to handle send errors.</span></span>

> [!NOTE]
> <span data-ttu-id="ae209-137">Si usa Azure SignalR Service en *modo sin servidor*, no puede llamar a métodos de concentrador desde un cliente.</span><span class="sxs-lookup"><span data-stu-id="ae209-137">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="ae209-138">Para obtener más información, consulte el [documentación de SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="ae209-138">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="ae209-139">Llamar a métodos cliente desde el concentrador</span><span class="sxs-lookup"><span data-stu-id="ae209-139">Call client methods from hub</span></span>

<span data-ttu-id="ae209-140">Definir métodos que el centro de llamadas utilizando `connection.On` tras su creación, pero antes de iniciar la conexión.</span><span class="sxs-lookup"><span data-stu-id="ae209-140">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="ae209-141">El código anterior en `connection.On` se ejecuta cuando se llama al código del lado servidor mediante el `SendAsync` método.</span><span class="sxs-lookup"><span data-stu-id="ae209-141">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="ae209-142">Registro y control de errores</span><span class="sxs-lookup"><span data-stu-id="ae209-142">Error handling and logging</span></span>

<span data-ttu-id="ae209-143">Controlar los errores con una instrucción try-catch.</span><span class="sxs-lookup"><span data-stu-id="ae209-143">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="ae209-144">Inspeccionar el `Exception` para determinar la acción adecuada que deben realizar después de que se produce un error.</span><span class="sxs-lookup"><span data-stu-id="ae209-144">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="ae209-145">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ae209-145">Additional resources</span></span>

* [<span data-ttu-id="ae209-146">Concentradores</span><span class="sxs-lookup"><span data-stu-id="ae209-146">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="ae209-147">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="ae209-147">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="ae209-148">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="ae209-148">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="ae209-149">Documentación sin servidor de Azure SignalR Service</span><span class="sxs-lookup"><span data-stu-id="ae209-149">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
