---
title: Cliente de .NET Core de ASP.NET SignalR
author: rachelappel
description: Información sobre el cliente de .NET Core de ASP.NET SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/dotnet-client
ms.openlocfilehash: faa4368988971a3e7fcdcd1b044971e16d70f19a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273300"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="9957e-103">Cliente de .NET Core de ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="9957e-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="9957e-104">Por [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="9957e-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="9957e-105">El cliente de ASP.NET Core SignalR .NET puede utilizarse en aplicaciones de Xamarin, WPF, Windows Forms, consola y .NET Core.</span><span class="sxs-lookup"><span data-stu-id="9957e-105">The ASP.NET Core SignalR .NET client can be used by Xamarin, WPF, Windows Forms, Console, and .NET Core apps.</span></span> <span data-ttu-id="9957e-106">Al igual que el [cliente JavaScript](xref:signalr/javascript-client), el cliente de .NET permite recibir y enviar y recibir mensajes a un concentrador en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="9957e-106">Like the [JavaScript client](xref:signalr/javascript-client), the .NET client enables you to receive and send and receive messages to a hub in real time.</span></span>

<span data-ttu-id="9957e-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9957e-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9957e-108">El ejemplo de código de este artículo es una aplicación WPF que utiliza al cliente de ASP.NET Core SignalR. NET.</span><span class="sxs-lookup"><span data-stu-id="9957e-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="9957e-109">Instale el paquete de cliente .NET de SignalR</span><span class="sxs-lookup"><span data-stu-id="9957e-109">Install the SignalR .NET client package</span></span>

<span data-ttu-id="9957e-110">El `Microsoft.AspNetCore.SignalR.Client` paquete es necesario para que los clientes de .NET para conectarse a los concentradores SignalR.</span><span class="sxs-lookup"><span data-stu-id="9957e-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="9957e-111">Para instalar la biblioteca de cliente, ejecute el siguiente comando el **Package Manager Console** ventana:</span><span class="sxs-lookup"><span data-stu-id="9957e-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="9957e-112">Conectarse a un concentrador</span><span class="sxs-lookup"><span data-stu-id="9957e-112">Connect to a hub</span></span>

<span data-ttu-id="9957e-113">Para establecer una conexión, cree un `HubConnectionBuilder` y llame a `Build`.</span><span class="sxs-lookup"><span data-stu-id="9957e-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="9957e-114">La dirección URL del concentrador, protocolo, tipo de transporte, nivel de registro, encabezados y otras opciones se pueden configurar durante la creación de una conexión.</span><span class="sxs-lookup"><span data-stu-id="9957e-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="9957e-115">Configurar las opciones necesarias mediante la inserción de cualquiera de los `HubConnectionBuilder` métodos en `Build`.</span><span class="sxs-lookup"><span data-stu-id="9957e-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="9957e-116">Iniciar la conexión con `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="9957e-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="9957e-117">Llamar a métodos de concentrador de cliente</span><span class="sxs-lookup"><span data-stu-id="9957e-117">Call hub methods from client</span></span>

<span data-ttu-id="9957e-118">`InvokeAsync` llama a métodos en el concentrador.</span><span class="sxs-lookup"><span data-stu-id="9957e-118">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="9957e-119">Pasar el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="9957e-119">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="9957e-120">SignalR es asincrónica, por lo que usar `async` y `await` cuando se realizan las llamadas.</span><span class="sxs-lookup"><span data-stu-id="9957e-120">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="9957e-121">Llamar a métodos de cliente desde el concentrador</span><span class="sxs-lookup"><span data-stu-id="9957e-121">Call client methods from hub</span></span>

<span data-ttu-id="9957e-122">Define el centro de llamadas mediante el método `connection.On` tras su creación, pero antes de iniciar la conexión.</span><span class="sxs-lookup"><span data-stu-id="9957e-122">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

<span data-ttu-id="9957e-123">El código anterior en `connection.On` se ejecuta cuando llama a código del lado servidor mediante el `SendAsync` método.</span><span class="sxs-lookup"><span data-stu-id="9957e-123">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="9957e-124">Registro y control de errores</span><span class="sxs-lookup"><span data-stu-id="9957e-124">Error handling and logging</span></span>

<span data-ttu-id="9957e-125">Controlar los errores con una instrucción try-catch.</span><span class="sxs-lookup"><span data-stu-id="9957e-125">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="9957e-126">Inspeccionar el `Exception` para determinar la acción apropiada deben realizar después de que se produce un error.</span><span class="sxs-lookup"><span data-stu-id="9957e-126">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a><span data-ttu-id="9957e-127">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="9957e-127">Additional resources</span></span>

* [<span data-ttu-id="9957e-128">Concentradores</span><span class="sxs-lookup"><span data-stu-id="9957e-128">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="9957e-129">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="9957e-129">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="9957e-130">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="9957e-130">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)