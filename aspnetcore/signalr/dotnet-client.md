---
title: Cliente de .NET de ASP.NET Core SignalR
author: tdykstra
description: Información sobre el cliente de .NET de ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 205ca8ca228dcc2cc77f7e9b6431943851a3b152
ms.sourcegitcommit: 1a2fc47fb5d3da0f2a3c3269613ab20eb3b0da2c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2018
ms.locfileid: "44373324"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="a9dc8-103">Cliente de .NET de ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="a9dc8-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="a9dc8-104">Por [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="a9dc8-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="a9dc8-105">La biblioteca de cliente de ASP.NET Core SignalR .NET le permite comunicarse con los concentradores de SignalR desde aplicaciones de. NET.</span><span class="sxs-lookup"><span data-stu-id="a9dc8-105">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="a9dc8-106">Xamarin tiene requisitos especiales para la versión de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a9dc8-106">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="a9dc8-107">Para obtener más información, consulte [2.1.1 de cliente de SignalR en Xamarin](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="a9dc8-107">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="a9dc8-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a9dc8-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a9dc8-109">El ejemplo de código en este artículo es una aplicación WPF que usa al cliente de .NET de ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="a9dc8-109">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="a9dc8-110">Instale el paquete de cliente .NET de SignalR</span><span class="sxs-lookup"><span data-stu-id="a9dc8-110">Install the SignalR .NET client package</span></span>

<span data-ttu-id="a9dc8-111">El `Microsoft.AspNetCore.SignalR.Client` paquete es necesario para que los clientes de .NET para conectarse a los concentradores de SignalR.</span><span class="sxs-lookup"><span data-stu-id="a9dc8-111">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="a9dc8-112">Para instalar la biblioteca de cliente, ejecute el siguiente comando en el **Package Manager Console** ventana:</span><span class="sxs-lookup"><span data-stu-id="a9dc8-112">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="a9dc8-113">Conectarse a un concentrador</span><span class="sxs-lookup"><span data-stu-id="a9dc8-113">Connect to a hub</span></span>

<span data-ttu-id="a9dc8-114">Para establecer una conexión, cree un `HubConnectionBuilder` y llamar a `Build`.</span><span class="sxs-lookup"><span data-stu-id="a9dc8-114">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="a9dc8-115">Durante la compilación de una conexión se pueden configurar la URL del centro, protocolo, tipo de transporte, nivel de registro, los encabezados y otras opciones.</span><span class="sxs-lookup"><span data-stu-id="a9dc8-115">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="a9dc8-116">Configurar las opciones necesarias mediante la inserción de cualquiera de los `HubConnectionBuilder` métodos en `Build`.</span><span class="sxs-lookup"><span data-stu-id="a9dc8-116">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="a9dc8-117">Iniciar la conexión con `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="a9dc8-117">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=14-16,32)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="a9dc8-118">Llamar a métodos de concentrador de cliente</span><span class="sxs-lookup"><span data-stu-id="a9dc8-118">Call hub methods from client</span></span>

<span data-ttu-id="a9dc8-119">`InvokeAsync` llama a métodos en el concentrador.</span><span class="sxs-lookup"><span data-stu-id="a9dc8-119">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="a9dc8-120">Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="a9dc8-120">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="a9dc8-121">SignalR es asincrónica, así que use `async` y `await` al realizar las llamadas.</span><span class="sxs-lookup"><span data-stu-id="a9dc8-121">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="a9dc8-122">Llamar a métodos cliente desde el concentrador</span><span class="sxs-lookup"><span data-stu-id="a9dc8-122">Call client methods from hub</span></span>

<span data-ttu-id="a9dc8-123">Definir métodos que el centro de llamadas utilizando `connection.On` tras su creación, pero antes de iniciar la conexión.</span><span class="sxs-lookup"><span data-stu-id="a9dc8-123">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="a9dc8-124">El código anterior en `connection.On` se ejecuta cuando se llama al código del lado servidor mediante el `SendAsync` método.</span><span class="sxs-lookup"><span data-stu-id="a9dc8-124">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="a9dc8-125">Registro y control de errores</span><span class="sxs-lookup"><span data-stu-id="a9dc8-125">Error handling and logging</span></span>

<span data-ttu-id="a9dc8-126">Controlar los errores con una instrucción try-catch.</span><span class="sxs-lookup"><span data-stu-id="a9dc8-126">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="a9dc8-127">Inspeccionar el `Exception` para determinar la acción adecuada que deben realizar después de que se produce un error.</span><span class="sxs-lookup"><span data-stu-id="a9dc8-127">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="a9dc8-128">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="a9dc8-128">Additional resources</span></span>

* [<span data-ttu-id="a9dc8-129">Concentradores</span><span class="sxs-lookup"><span data-stu-id="a9dc8-129">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="a9dc8-130">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="a9dc8-130">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="a9dc8-131">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="a9dc8-131">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
