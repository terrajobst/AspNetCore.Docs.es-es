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
# <a name="aspnet-core-signalr-net-client"></a>Cliente de .NET de ASP.NET Core SignalR

La biblioteca de cliente de ASP.NET Core SignalR .NET le permite comunicarse con los concentradores de SignalR desde aplicaciones de. NET.

> [!NOTE]
> Xamarin tiene requisitos especiales para la versión de Visual Studio. Para obtener más información, consulte [2.1.1 de cliente de SignalR en Xamarin](https://github.com/aspnet/Announcements/issues/305).

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

El ejemplo de código en este artículo es una aplicación WPF que usa al cliente de .NET de ASP.NET Core SignalR.

## <a name="install-the-signalr-net-client-package"></a>Instale el paquete de cliente .NET de SignalR

El `Microsoft.AspNetCore.SignalR.Client` paquete es necesario para que los clientes de .NET para conectarse a los concentradores de SignalR. Para instalar la biblioteca de cliente, ejecute el siguiente comando en el **Package Manager Console** ventana:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Conectarse a un concentrador

Para establecer una conexión, cree un `HubConnectionBuilder` y llamar a `Build`. Durante la compilación de una conexión se pueden configurar la URL del centro, protocolo, tipo de transporte, nivel de registro, los encabezados y otras opciones. Configurar las opciones necesarias mediante la inserción de cualquiera de los `HubConnectionBuilder` métodos en `Build`. Iniciar la conexión con `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a>Controlar la pérdida de conexión

Use el <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> eventos para responder a una pérdida de conexión. Por ejemplo, es posible que desee automatizar la reconexión.

El `Closed` evento requiere un delegado que devuelve un `Task`, lo que permite ejecutar sin usar código asincrónico `async void`. Para cumplir con la firma del delegado en un `Closed` controlador de eventos que se ejecuta de forma sincrónica, devolver `Task.CompletedTask`:

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

La razón principal para el soporte para asincronía es por lo que puede reiniciar la conexión. A partir de una conexión es una acción asincrónica.

En un `Closed` controlador que se reinicia la conexión, considere la posibilidad de esperar a cierto retraso aleatorio evitar la sobrecarga del servidor, como se muestra en el ejemplo siguiente:

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a>Llamar a métodos de concentrador de cliente

`InvokeAsync` llama a métodos en el concentrador. Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `InvokeAsync`. SignalR es asincrónica, así que use `async` y `await` al realizar las llamadas.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a>Llamar a métodos cliente desde el concentrador

Definir métodos que el centro de llamadas utilizando `connection.On` tras su creación, pero antes de iniciar la conexión.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

El código anterior en `connection.On` se ejecuta cuando se llama al código del lado servidor mediante el `SendAsync` método.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a>Registro y control de errores

Controlar los errores con una instrucción try-catch. Inspeccionar el `Exception` para determinar la acción adecuada que deben realizar después de que se produce un error.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a>Recursos adicionales

* [Concentradores](xref:signalr/hubs)
* [Cliente de JavaScript](xref:signalr/javascript-client)
* [Publicar en Azure](xref:signalr/publish-to-azure-web-app)
