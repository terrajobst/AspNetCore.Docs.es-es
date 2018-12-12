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
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a><span data-ttu-id="a97cc-103">Autenticación y autorización en ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="a97cc-103">Authentication and authorization in ASP.NET Core SignalR</span></span>

<span data-ttu-id="a97cc-104">Por [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="a97cc-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="a97cc-105">[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(cómo descargar)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="a97cc-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a><span data-ttu-id="a97cc-106">Autenticar a los usuarios conectarse a un concentrador SignalR</span><span class="sxs-lookup"><span data-stu-id="a97cc-106">Authenticate users connecting to a SignalR hub</span></span>

<span data-ttu-id="a97cc-107">Se puede usar SignalR con [autenticación de ASP.NET Core](xref:security/authentication/identity) para asociar un usuario a cada conexión.</span><span class="sxs-lookup"><span data-stu-id="a97cc-107">SignalR can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each connection.</span></span> <span data-ttu-id="a97cc-108">En, un centro de datos de autenticación se pueden acceder desde el [ `HubConnectionContext.User` ](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) propiedad.</span><span class="sxs-lookup"><span data-stu-id="a97cc-108">In a hub, authentication data can be accessed from the [`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) property.</span></span> <span data-ttu-id="a97cc-109">La autenticación permite que el concentrador llamar a métodos en todas las conexiones asociadas a un usuario (consulte [administrar usuarios y grupos en SignalR](xref:signalr/groups) para obtener más información).</span><span class="sxs-lookup"><span data-stu-id="a97cc-109">Authentication allows the hub to call methods on all connections associated with a user (See [Manage users and groups in SignalR](xref:signalr/groups) for more information).</span></span> <span data-ttu-id="a97cc-110">Varias conexiones pueden asociarse con un solo usuario.</span><span class="sxs-lookup"><span data-stu-id="a97cc-110">Multiple connections may be associated with a single user.</span></span>

### <a name="cookie-authentication"></a><span data-ttu-id="a97cc-111">Autenticación con cookies</span><span class="sxs-lookup"><span data-stu-id="a97cc-111">Cookie authentication</span></span>

<span data-ttu-id="a97cc-112">Autenticación con cookies en una aplicación basada en explorador, permite que sus credenciales de usuario existente fluya automáticamente a las conexiones de SignalR.</span><span class="sxs-lookup"><span data-stu-id="a97cc-112">In a browser-based app, cookie authentication allows your existing user credentials to automatically flow to SignalR connections.</span></span> <span data-ttu-id="a97cc-113">Cuando se usa el explorador del cliente, no se necesita ninguna configuración adicional.</span><span class="sxs-lookup"><span data-stu-id="a97cc-113">When using the browser client, no additional configuration is needed.</span></span> <span data-ttu-id="a97cc-114">Si el usuario inicia sesión en su aplicación, la conexión de SignalR hereda automáticamente esta autenticación.</span><span class="sxs-lookup"><span data-stu-id="a97cc-114">If the user is logged in to your app, the SignalR connection automatically inherits this authentication.</span></span>

<span data-ttu-id="a97cc-115">Las cookies son una manera específica del explorador para enviar tokens de acceso, pero los clientes sin explorador pueden enviarlos.</span><span class="sxs-lookup"><span data-stu-id="a97cc-115">Cookies are a browser-specific way to send access tokens, but non-browser clients can send them.</span></span> <span data-ttu-id="a97cc-116">Cuando se usa el [cliente.NET](xref:signalr/dotnet-client), el `Cookies` propiedad se puede configurar en el `.WithUrl` llamada con el fin de proporcionar una cookie.</span><span class="sxs-lookup"><span data-stu-id="a97cc-116">When using the [.NET Client](xref:signalr/dotnet-client), the `Cookies` property can be configured in the `.WithUrl` call in order to provide a cookie.</span></span> <span data-ttu-id="a97cc-117">Sin embargo, requiere la aplicación para proporcionar una API para intercambiar datos de autenticación para una cookie mediante la autenticación de cookies del cliente. NET.</span><span class="sxs-lookup"><span data-stu-id="a97cc-117">However, using cookie authentication from the .NET Client requires the app to provide an API to exchange authentication data for a cookie.</span></span>

### <a name="bearer-token-authentication"></a><span data-ttu-id="a97cc-118">Autenticación de token de portador</span><span class="sxs-lookup"><span data-stu-id="a97cc-118">Bearer token authentication</span></span>

<span data-ttu-id="a97cc-119">El cliente puede proporcionar un token de acceso en lugar de usar una cookie.</span><span class="sxs-lookup"><span data-stu-id="a97cc-119">The client can provide an access token instead of using a cookie.</span></span> <span data-ttu-id="a97cc-120">El servidor valida el token y lo usa para identificar al usuario.</span><span class="sxs-lookup"><span data-stu-id="a97cc-120">The server validates the token and uses it to identify the user.</span></span> <span data-ttu-id="a97cc-121">Esta validación se realiza solo cuando se establece la conexión.</span><span class="sxs-lookup"><span data-stu-id="a97cc-121">This validation is done only when the connection is established.</span></span> <span data-ttu-id="a97cc-122">Durante la vida de la conexión, el servidor no validar automáticamente para comprobar la revocación del token.</span><span class="sxs-lookup"><span data-stu-id="a97cc-122">During the life of the connection, the server doesn't automatically revalidate to check for token revocation.</span></span>

<span data-ttu-id="a97cc-123">En el servidor, la autenticación de token de portador se configura mediante el [middleware de portador JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="a97cc-123">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="a97cc-124">En el cliente de JavaScript, se puede proporcionar el token mediante el [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) opción.</span><span class="sxs-lookup"><span data-stu-id="a97cc-124">In the JavaScript client, the token can be provided using the [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) option.</span></span>

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

<span data-ttu-id="a97cc-125">En el cliente. NET, hay un proceso similar [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) propiedad que se puede usar para configurar el token:</span><span class="sxs-lookup"><span data-stu-id="a97cc-125">In the .NET client, there is a similar [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) property that can be used to configure the token:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="a97cc-126">Se llama a la función de token de acceso que proporcione antes **cada** solicitud HTTP de SignalR.</span><span class="sxs-lookup"><span data-stu-id="a97cc-126">The access token function you provide is called before **every** HTTP request made by SignalR.</span></span> <span data-ttu-id="a97cc-127">Si necesita renovar el token con el fin de mantener la conexión activa (porque lo puede expirar durante la conexión), hacerlo desde esta función y devolver el token actualizado.</span><span class="sxs-lookup"><span data-stu-id="a97cc-127">If you need to renew the token in order to keep the connection active (because it may expire during the connection), do so from within this function and return the updated token.</span></span>

<span data-ttu-id="a97cc-128">En las API web estándar, se envían los tokens de portador en un encabezado HTTP.</span><span class="sxs-lookup"><span data-stu-id="a97cc-128">In standard web APIs, bearer tokens are sent in an HTTP header.</span></span> <span data-ttu-id="a97cc-129">Sin embargo, SignalR es no se puede establecer estos encabezados en los exploradores cuando se usa algunos transportes.</span><span class="sxs-lookup"><span data-stu-id="a97cc-129">However, SignalR is unable to set these headers in browsers when using some transports.</span></span> <span data-ttu-id="a97cc-130">Cuando se usa WebSockets y los eventos, el token se transmite como un parámetro de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="a97cc-130">When using WebSockets and Server-Sent Events, the token is transmitted as a query string parameter.</span></span> <span data-ttu-id="a97cc-131">Para admitir esto en el servidor, se requiere configuración adicional:</span><span class="sxs-lookup"><span data-stu-id="a97cc-131">In order to support this on the server, additional configuration is required:</span></span>

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="cookies-vs-bearer-tokens"></a><span data-ttu-id="a97cc-132">Cookies frente a los tokens de portador</span><span class="sxs-lookup"><span data-stu-id="a97cc-132">Cookies vs. bearer tokens</span></span> 

<span data-ttu-id="a97cc-133">Dado que las cookies son específicas de los exploradores, enviarlos de otros tipos de clientes agrega complejidad al comparado con el envío de tokens de portador.</span><span class="sxs-lookup"><span data-stu-id="a97cc-133">Because cookies are specific to browsers, sending them from other kinds of clients adds complexity compared to sending bearer tokens.</span></span> <span data-ttu-id="a97cc-134">Por este motivo, la autenticación de cookies no se recomienda a menos que la aplicación solo necesita autenticar a los usuarios desde el explorador del cliente.</span><span class="sxs-lookup"><span data-stu-id="a97cc-134">For this reason, cookie authentication isn't recommended unless the app only needs to authenticate users from the browser client.</span></span> <span data-ttu-id="a97cc-135">Autenticación de token de portador es el enfoque recomendado al usar a los clientes que no sea el explorador del cliente.</span><span class="sxs-lookup"><span data-stu-id="a97cc-135">Bearer token authentication is the recommended approach when using clients other than the browser client.</span></span>

### <a name="windows-authentication"></a><span data-ttu-id="a97cc-136">Autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="a97cc-136">Windows authentication</span></span>

<span data-ttu-id="a97cc-137">Si [autenticación de Windows](xref:security/authentication/windowsauth) está configurado en la aplicación, SignalR puede usar esa identidad para proteger los concentradores.</span><span class="sxs-lookup"><span data-stu-id="a97cc-137">If [Windows authentication](xref:security/authentication/windowsauth) is configured in your app, SignalR can use that identity to secure hubs.</span></span> <span data-ttu-id="a97cc-138">Sin embargo, para enviar mensajes a usuarios individuales, deberá agregar un proveedor de Id. de usuario personalizado.</span><span class="sxs-lookup"><span data-stu-id="a97cc-138">However, in order to send messages to individual users, you need to add a custom User ID provider.</span></span> <span data-ttu-id="a97cc-139">Esto es porque el sistema de autenticación de Windows no proporciona la notificación "NameIdentifier" que usa SignalR para determinar el nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="a97cc-139">This is because the Windows authentication system doesn't provide the "Name Identifier" claim that SignalR uses to determine the user name.</span></span>

<span data-ttu-id="a97cc-140">Agregue una nueva clase que implementa `IUserIdProvider` y recuperar una de las notificaciones del usuario que se usará como el identificador.</span><span class="sxs-lookup"><span data-stu-id="a97cc-140">Add a new class that implements `IUserIdProvider` and retrieve one of the claims from the user to use as the identifier.</span></span> <span data-ttu-id="a97cc-141">Por ejemplo, para usar la notificación "Name" (que es el nombre de usuario de Windows en el formulario `[Domain]\[Username]`), cree la siguiente clase:</span><span class="sxs-lookup"><span data-stu-id="a97cc-141">For example, to use the "Name" claim (which is the Windows username in the form `[Domain]\[Username]`), create the following class:</span></span>

```csharp
public class NameUserIdProvider : IUserIdProvider
{
    public string GetUserId(HubConnectionContext connection)
    {
        return connection.User?.FindFirst(ClaimTypes.Name)?.Value;
    }
}
```

<span data-ttu-id="a97cc-142">En lugar de `ClaimTypes.Name`, puede usar cualquier valor de la `User` (por ejemplo, el identificador SID de Windows, etcetera.).</span><span class="sxs-lookup"><span data-stu-id="a97cc-142">Rather than `ClaimTypes.Name`, you can use any value from the `User` (such as the Windows SID identifier, etc.).</span></span>

> [!NOTE]
> <span data-ttu-id="a97cc-143">El valor que elija debe ser único entre todos los usuarios en el sistema.</span><span class="sxs-lookup"><span data-stu-id="a97cc-143">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="a97cc-144">En caso contrario, un mensaje destinado a un usuario podría acabar va a un usuario diferente.</span><span class="sxs-lookup"><span data-stu-id="a97cc-144">Otherwise, a message intended for one user could end up going to a different user.</span></span>

<span data-ttu-id="a97cc-145">Registrar este componente en su `Startup.ConfigureServices` método **después** la llamada a `.AddSignalR`</span><span class="sxs-lookup"><span data-stu-id="a97cc-145">Register this component in your `Startup.ConfigureServices` method **after** the call to `.AddSignalR`</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

<span data-ttu-id="a97cc-146">En el cliente. NET, la autenticación de Windows debe estar habilitada estableciendo el [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) propiedad:</span><span class="sxs-lookup"><span data-stu-id="a97cc-146">In the .NET Client, Windows Authentication must be enabled by setting the [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) property:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

<span data-ttu-id="a97cc-147">Autenticación de Windows solo es compatible con el explorador del cliente cuando se usa Microsoft Internet Explorer o Microsoft Edge.</span><span class="sxs-lookup"><span data-stu-id="a97cc-147">Windows Authentication is only supported by the browser client when using Microsoft Internet Explorer or Microsoft Edge.</span></span>

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a><span data-ttu-id="a97cc-148">Autorizar a los usuarios acceso a concentradores y métodos de concentrador</span><span class="sxs-lookup"><span data-stu-id="a97cc-148">Authorize users to access hubs and hub methods</span></span>

<span data-ttu-id="a97cc-149">De forma predeterminada, se pueden llamar a todos los métodos en un centro de un usuario autenticado.</span><span class="sxs-lookup"><span data-stu-id="a97cc-149">By default, all methods in a hub can be called by an unauthenticated user.</span></span> <span data-ttu-id="a97cc-150">Con el fin de solicitar la autenticación, se aplican la [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) al concentrador de atributo:</span><span class="sxs-lookup"><span data-stu-id="a97cc-150">In order to require authentication, apply the [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attribute to the hub:</span></span>

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

<span data-ttu-id="a97cc-151">Puede usar los argumentos de constructor y propiedades de la `[Authorize]` atributo para restringir el acceso solo a los usuarios de coincidencia específico [las directivas de autorización](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="a97cc-151">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="a97cc-152">Por ejemplo, si tiene una directiva de autorización personalizada denominada `MyAuthorizationPolicy` puede asegurarse de que solo los usuarios que coinciden con dicha directiva pueden tener acceso a centro con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a97cc-152">For example, if you have a custom authorization policy called `MyAuthorizationPolicy` you can ensure that only users matching that policy can access the hub using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub: Hub
{
}
```

<span data-ttu-id="a97cc-153">Los métodos de concentrador individuales pueden tener el `[Authorize]` también aplicado el atributo.</span><span class="sxs-lookup"><span data-stu-id="a97cc-153">Individual hub methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="a97cc-154">Si el usuario actual no coincide con la directiva se aplica al método, se devuelve un error al llamador:</span><span class="sxs-lookup"><span data-stu-id="a97cc-154">If the current user doesn't match the policy applied to the method, an error is returned to the caller:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="a97cc-155">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="a97cc-155">Additional resources</span></span>

* [<span data-ttu-id="a97cc-156">Autenticación de Token de portador en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a97cc-156">Bearer Token Authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
