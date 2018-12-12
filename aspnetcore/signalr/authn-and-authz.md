---
title: Autenticación y autorización en ASP.NET Core SignalR
author: tdykstra
description: Obtenga información sobre cómo utilizar la autenticación y autorización en ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/authn-and-authz
ms.openlocfilehash: 6624b2b4361eb0a5bb6e45108878ee6fe5ecb443
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284438"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a>Autenticación y autorización en ASP.NET Core SignalR

Por [Andrew Stanton-Nurse](https://twitter.com/anurse)

[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(cómo descargar)](xref:index#how-to-download-a-sample)

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a>Autenticar a los usuarios conectarse a un concentrador SignalR

Se puede usar SignalR con [autenticación de ASP.NET Core](xref:security/authentication/identity) para asociar un usuario a cada conexión. En, un centro de datos de autenticación se pueden acceder desde el [ `HubConnectionContext.User` ](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) propiedad. La autenticación permite que el concentrador llamar a métodos en todas las conexiones asociadas a un usuario (consulte [administrar usuarios y grupos en SignalR](xref:signalr/groups) para obtener más información). Varias conexiones pueden asociarse con un solo usuario.

### <a name="cookie-authentication"></a>Autenticación con cookies

Autenticación con cookies en una aplicación basada en explorador, permite que sus credenciales de usuario existente fluya automáticamente a las conexiones de SignalR. Cuando se usa el explorador del cliente, no se necesita ninguna configuración adicional. Si el usuario inicia sesión en su aplicación, la conexión de SignalR hereda automáticamente esta autenticación.

Las cookies son una manera específica del explorador para enviar tokens de acceso, pero los clientes sin explorador pueden enviarlos. Cuando se usa el [cliente.NET](xref:signalr/dotnet-client), el `Cookies` propiedad se puede configurar en el `.WithUrl` llamada con el fin de proporcionar una cookie. Sin embargo, requiere la aplicación para proporcionar una API para intercambiar datos de autenticación para una cookie mediante la autenticación de cookies del cliente. NET.

### <a name="bearer-token-authentication"></a>Autenticación de token de portador

El cliente puede proporcionar un token de acceso en lugar de usar una cookie. El servidor valida el token y lo usa para identificar al usuario. Esta validación se realiza solo cuando se establece la conexión. Durante la vida de la conexión, el servidor no validar automáticamente para comprobar la revocación del token.

En el servidor, la autenticación de token de portador se configura mediante el [middleware de portador JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).

En el cliente de JavaScript, se puede proporcionar el token mediante el [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) opción.

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

En el cliente. NET, hay un proceso similar [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) propiedad que se puede usar para configurar el token:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> Se llama a la función de token de acceso que proporcione antes **cada** solicitud HTTP de SignalR. Si necesita renovar el token con el fin de mantener la conexión activa (porque lo puede expirar durante la conexión), hacerlo desde esta función y devolver el token actualizado.

En las API web estándar, se envían los tokens de portador en un encabezado HTTP. Sin embargo, SignalR es no se puede establecer estos encabezados en los exploradores cuando se usa algunos transportes. Cuando se usa WebSockets y los eventos, el token se transmite como un parámetro de cadena de consulta. Para admitir esto en el servidor, se requiere configuración adicional:

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="cookies-vs-bearer-tokens"></a>Cookies frente a los tokens de portador 

Dado que las cookies son específicas de los exploradores, enviarlos de otros tipos de clientes agrega complejidad al comparado con el envío de tokens de portador. Por este motivo, la autenticación de cookies no se recomienda a menos que la aplicación solo necesita autenticar a los usuarios desde el explorador del cliente. Autenticación de token de portador es el enfoque recomendado al usar a los clientes que no sea el explorador del cliente.

### <a name="windows-authentication"></a>Autenticación de Windows

Si [autenticación de Windows](xref:security/authentication/windowsauth) está configurado en la aplicación, SignalR puede usar esa identidad para proteger los concentradores. Sin embargo, para enviar mensajes a usuarios individuales, deberá agregar un proveedor de Id. de usuario personalizado. Esto es porque el sistema de autenticación de Windows no proporciona la notificación "NameIdentifier" que usa SignalR para determinar el nombre de usuario.

Agregue una nueva clase que implementa `IUserIdProvider` y recuperar una de las notificaciones del usuario que se usará como el identificador. Por ejemplo, para usar la notificación "Name" (que es el nombre de usuario de Windows en el formulario `[Domain]\[Username]`), cree la siguiente clase:

```csharp
public class NameUserIdProvider : IUserIdProvider
{
    public string GetUserId(HubConnectionContext connection)
    {
        return connection.User?.FindFirst(ClaimTypes.Name)?.Value;
    }
}
```

En lugar de `ClaimTypes.Name`, puede usar cualquier valor de la `User` (por ejemplo, el identificador SID de Windows, etcetera.).

> [!NOTE]
> El valor que elija debe ser único entre todos los usuarios en el sistema. En caso contrario, un mensaje destinado a un usuario podría acabar va a un usuario diferente.

Registrar este componente en su `Startup.ConfigureServices` método **después** la llamada a `.AddSignalR`

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

En el cliente. NET, la autenticación de Windows debe estar habilitada estableciendo el [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) propiedad:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

Autenticación de Windows solo es compatible con el explorador del cliente cuando se usa Microsoft Internet Explorer o Microsoft Edge.

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a>Autorizar a los usuarios acceso a concentradores y métodos de concentrador

De forma predeterminada, se pueden llamar a todos los métodos en un centro de un usuario autenticado. Con el fin de solicitar la autenticación, se aplican la [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) al concentrador de atributo:

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

Puede usar los argumentos de constructor y propiedades de la `[Authorize]` atributo para restringir el acceso solo a los usuarios de coincidencia específico [las directivas de autorización](xref:security/authorization/policies). Por ejemplo, si tiene una directiva de autorización personalizada denominada `MyAuthorizationPolicy` puede asegurarse de que solo los usuarios que coinciden con dicha directiva pueden tener acceso a centro con el código siguiente:

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub: Hub
{
}
```

Los métodos de concentrador individuales pueden tener el `[Authorize]` también aplicado el atributo. Si el usuario actual no coincide con la directiva se aplica al método, se devuelve un error al llamador:

```csharp
[Authorize]
public class ChatHub: Hub
{
    public async Task Send(string message)
    {
        // ... send a message to all users ...
    }

    [Authorize("Administrators")]
    public void BanUser(string userName)
    {
        // ... ban a user from the chat room (something only Administrators can do) ...
    }
}
```

## <a name="additional-resources"></a>Recursos adicionales

* [Autenticación de Token de portador en ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
