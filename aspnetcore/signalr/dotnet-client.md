---
title: Cliente de .NET de ASP.NET Core SignalR
author: bradygaster
description: Información sobre el cliente de .NET de ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 25b618f7a424b217c0fb55417754ea358280b95a
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836311"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="c8bd7-103">Cliente de .NET de ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="c8bd7-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="c8bd7-104">La biblioteca de cliente de ASP.NET Core SignalR .NET le permite comunicarse con los concentradores de SignalR desde aplicaciones de. NET.</span><span class="sxs-lookup"><span data-stu-id="c8bd7-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="c8bd7-105">Xamarin tiene requisitos especiales para la versión de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8bd7-105">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="c8bd7-106">Para obtener más información, consulte [2.1.1 de cliente de SignalR en Xamarin](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="c8bd7-106">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="c8bd7-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c8bd7-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c8bd7-108">El ejemplo de código en este artículo es una aplicación WPF que usa al cliente de .NET de ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="c8bd7-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="c8bd7-109">Instale el paquete de cliente .NET de SignalR</span><span class="sxs-lookup"><span data-stu-id="c8bd7-109">Install the SignalR .NET client package</span></span>

<span data-ttu-id="c8bd7-110">El `Microsoft.AspNetCore.SignalR.Client` paquete es necesario para que los clientes de .NET para conectarse a los concentradores de SignalR.</span><span class="sxs-lookup"><span data-stu-id="c8bd7-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="c8bd7-111">Para instalar la biblioteca de cliente, ejecute el siguiente comando en el **Package Manager Console** ventana:</span><span class="sxs-lookup"><span data-stu-id="c8bd7-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="c8bd7-112">Conectarse a un concentrador</span><span class="sxs-lookup"><span data-stu-id="c8bd7-112">Connect to a hub</span></span>

<span data-ttu-id="c8bd7-113">Para establecer una conexión, cree un `HubConnectionBuilder` y llamar a `Build`.</span><span class="sxs-lookup"><span data-stu-id="c8bd7-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="c8bd7-114">Durante la compilación de una conexión se pueden configurar la URL del centro, protocolo, tipo de transporte, nivel de registro, los encabezados y otras opciones.</span><span class="sxs-lookup"><span data-stu-id="c8bd7-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="c8bd7-115">Configurar las opciones necesarias mediante la inserción de cualquiera de los `HubConnectionBuilder` métodos en `Build`.</span><span class="sxs-lookup"><span data-stu-id="c8bd7-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="c8bd7-116">Iniciar la conexión con `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="c8bd7-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="c8bd7-117">Controlar la pérdida de conexión</span><span class="sxs-lookup"><span data-stu-id="c8bd7-117">Handle lost connection</span></span>

<span data-ttu-id="c8bd7-118">Use el <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> eventos para responder a una pérdida de conexión.</span><span class="sxs-lookup"><span data-stu-id="c8bd7-118">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="c8bd7-119">Por ejemplo, es posible que desee automatizar la reconexión.</span><span class="sxs-lookup"><span data-stu-id="c8bd7-119">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="c8bd7-120">El `Closed` evento requiere un delegado que devuelve un `Task`, lo que permite ejecutar sin usar código asincrónico `async void`.</span><span class="sxs-lookup"><span data-stu-id="c8bd7-120">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="c8bd7-121">Para cumplir con la firma del delegado en un `Closed` controlador de eventos que se ejecuta de forma sincrónica, devolver `Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="c8bd7-121">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="c8bd7-122">La razón principal para el soporte para asincronía es por lo que puede reiniciar la conexión.</span><span class="sxs-lookup"><span data-stu-id="c8bd7-122">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="c8bd7-123">A partir de una conexión es una acción asincrónica.</span><span class="sxs-lookup"><span data-stu-id="c8bd7-123">Starting a connection is an async action.</span></span>

<span data-ttu-id="c8bd7-124">En un `Closed` controlador que se reinicia la conexión, considere la posibilidad de esperar a cierto retraso aleatorio evitar la sobrecarga del servidor, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c8bd7-124">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="c8bd7-125">Llamar a métodos de concentrador de cliente</span><span class="sxs-lookup"><span data-stu-id="c8bd7-125">Call hub methods from client</span></span>

<span data-ttu-id="c8bd7-126">`InvokeAsync` llama a métodos en el concentrador.</span><span class="sxs-lookup"><span data-stu-id="c8bd7-126">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="c8bd7-127">Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="c8bd7-127">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="c8bd7-128">SignalR es asincrónica, así que use `async` y `await` al realizar las llamadas.</span><span class="sxs-lookup"><span data-stu-id="c8bd7-128">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="c8bd7-129">Llamar a métodos cliente desde el concentrador</span><span class="sxs-lookup"><span data-stu-id="c8bd7-129">Call client methods from hub</span></span>

<span data-ttu-id="c8bd7-130">Definir métodos que el centro de llamadas utilizando `connection.On` tras su creación, pero antes de iniciar la conexión.</span><span class="sxs-lookup"><span data-stu-id="c8bd7-130">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="c8bd7-131">El código anterior en `connection.On` se ejecuta cuando se llama al código del lado servidor mediante el `SendAsync` método.</span><span class="sxs-lookup"><span data-stu-id="c8bd7-131">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="c8bd7-132">Registro y control de errores</span><span class="sxs-lookup"><span data-stu-id="c8bd7-132">Error handling and logging</span></span>

<span data-ttu-id="c8bd7-133">Controlar los errores con una instrucción try-catch.</span><span class="sxs-lookup"><span data-stu-id="c8bd7-133">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="c8bd7-134">Inspeccionar el `Exception` para determinar la acción adecuada que deben realizar después de que se produce un error.</span><span class="sxs-lookup"><span data-stu-id="c8bd7-134">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="c8bd7-135">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="c8bd7-135">Additional resources</span></span>

* [<span data-ttu-id="c8bd7-136">Concentradores</span><span class="sxs-lookup"><span data-stu-id="c8bd7-136">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="c8bd7-137">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="c8bd7-137">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="c8bd7-138">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="c8bd7-138">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
