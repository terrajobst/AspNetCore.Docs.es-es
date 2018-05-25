---
title: Cliente de .NET Core de ASP.NET SignalR
author: rachelappel
description: Información sobre el cliente de .NET Core de ASP.NET SignalR
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/18/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/dotnet-client
ms.openlocfilehash: 412d2362575789f1fb4792940df6d3dd24dbdd5a
ms.sourcegitcommit: 300a1127957dcdbce1b6ad79a7b9dc676f571510
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2018
---
# <a name="aspnet-core-signalr-net-client"></a>Cliente de .NET Core de ASP.NET SignalR

Por [Rachel Appel](http://twitter.com/rachelappel)

El cliente de ASP.NET Core SignalR .NET puede utilizarse en aplicaciones de Xamarin, WPF, Windows Forms, consola y .NET Core. Al igual que el [cliente JavaScript](xref:signalr/javascript-client), el cliente de .NET permite recibir y enviar y recibir mensajes a un concentrador en tiempo real.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

El ejemplo de código de este artículo es una aplicación WPF que utiliza al cliente de ASP.NET Core SignalR. NET.

## <a name="setup-client"></a>Configurar el cliente

El `Microsoft.AspNetCore.SignalR.Client` paquete es necesario para que los clientes de .NET para conectarse a los concentradores SignalR. Para instalar la biblioteca de cliente, ejecute el siguiente comando el **Package Manager Console** ventana:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Conectarse a un concentrador

Para establecer una conexión, cree un `HubConnectionBuilder` y llame a `Build`. La dirección URL del concentrador, protocolo, tipo de transporte, nivel de registro, encabezados y otras opciones se pueden configurar durante la creación de una conexión. Configurar las opciones necesarias mediante la inserción de cualquiera de los `HubConnectionBuilder` métodos en `Build`. Iniciar la conexión con `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a>Llamar a métodos de concentrador de cliente

`InvokeAsync` llama a métodos en el concentrador. Pasar el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `InvokeAsync`. SignalR es asincrónica, por lo que usar `async` y `await` cuando se realizan las llamadas.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a>Llamar a métodos de cliente desde el concentrador

Define el centro de llamadas mediante el método `connection.On` tras su creación, pero antes de iniciar la conexión.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

El código anterior en `connection.On` se ejecuta cuando llama a código del lado servidor mediante el `SendAsync` método.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a>Registro y control de errores

Controlar los errores con una instrucción try-catch. Inspeccionar el `Exception` para determinar la acción apropiada deben realizar después de que se produce un error.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a>Recursos adicionales

* [Concentradores](xref:signalr/hubs)
* [Cliente de JavaScript](xref:signalr/javascript-client)
* [Publicar en Azure](xref:signalr/publish-to-azure-web-app)