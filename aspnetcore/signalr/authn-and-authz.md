---
title: Autenticación y autorización en ASP.NET Core SignalR
author: bradygaster
description: Obtenga información sobre cómo utilizar la autenticación y autorización en ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 05/09/2019
uid: signalr/authn-and-authz
ms.openlocfilehash: e8f9dc48be780fb91bdec6ea4d579f5e4f16197b
ms.sourcegitcommit: 3376f224b47a89acf329b2d2f9260046a372f924
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/10/2019
ms.locfileid: "65516948"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a><span data-ttu-id="e6adf-103">Autenticación y autorización en ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="e6adf-103">Authentication and authorization in ASP.NET Core SignalR</span></span>

<span data-ttu-id="e6adf-104">Por [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="e6adf-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="e6adf-105">[Ver o descargar el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(cómo descargar)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="e6adf-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a><span data-ttu-id="e6adf-106">Autenticar a los usuarios conectarse a un concentrador SignalR</span><span class="sxs-lookup"><span data-stu-id="e6adf-106">Authenticate users connecting to a SignalR hub</span></span>

<span data-ttu-id="e6adf-107">Se puede usar SignalR con [autenticación de ASP.NET Core](xref:security/authentication/identity) para asociar un usuario a cada conexión.</span><span class="sxs-lookup"><span data-stu-id="e6adf-107">SignalR can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each connection.</span></span> <span data-ttu-id="e6adf-108">En, un centro de datos de autenticación se pueden acceder desde el [ `HubConnectionContext.User` ](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) propiedad.</span><span class="sxs-lookup"><span data-stu-id="e6adf-108">In a hub, authentication data can be accessed from the [`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) property.</span></span> <span data-ttu-id="e6adf-109">La autenticación permite que el concentrador llamar a métodos en todas las conexiones asociadas a un usuario (consulte [administrar usuarios y grupos en SignalR](xref:signalr/groups) para obtener más información).</span><span class="sxs-lookup"><span data-stu-id="e6adf-109">Authentication allows the hub to call methods on all connections associated with a user (See [Manage users and groups in SignalR](xref:signalr/groups) for more information).</span></span> <span data-ttu-id="e6adf-110">Varias conexiones pueden asociarse con un solo usuario.</span><span class="sxs-lookup"><span data-stu-id="e6adf-110">Multiple connections may be associated with a single user.</span></span>

<span data-ttu-id="e6adf-111">El siguiente es un ejemplo de `Startup.Configure` que usa la autenticación de SignalR y ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="e6adf-111">The following is an example of `Startup.Configure` which uses SignalR and ASP.NET Core authentication:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    ...

    app.UseStaticFiles();
    
    app.UseAuthentication();

    app.UseSignalR(hubs =>
    {
        hubs.MapHub<ChatHub>("/chat");
    });

    app.UseMvc(routes =>
    {
        routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
    });
}
```

> [!NOTE]
> <span data-ttu-id="e6adf-112">Es importante el orden en que se registre el middleware de autenticación de SignalR y ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e6adf-112">The order in which you register the SignalR and ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="e6adf-113">Llame siempre a `UseAuthentication` antes `UseSignalR` poder SignalR tiene un usuario en el `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="e6adf-113">Always call `UseAuthentication` before `UseSignalR` so that SignalR has a user on the `HttpContext`.</span></span>

### <a name="cookie-authentication"></a><span data-ttu-id="e6adf-114">Autenticación con cookies</span><span class="sxs-lookup"><span data-stu-id="e6adf-114">Cookie authentication</span></span>

<span data-ttu-id="e6adf-115">Autenticación con cookies en una aplicación basada en explorador, permite que sus credenciales de usuario existente fluya automáticamente a las conexiones de SignalR.</span><span class="sxs-lookup"><span data-stu-id="e6adf-115">In a browser-based app, cookie authentication allows your existing user credentials to automatically flow to SignalR connections.</span></span> <span data-ttu-id="e6adf-116">Cuando se usa el explorador del cliente, no se necesita ninguna configuración adicional.</span><span class="sxs-lookup"><span data-stu-id="e6adf-116">When using the browser client, no additional configuration is needed.</span></span> <span data-ttu-id="e6adf-117">Si el usuario inicia sesión en su aplicación, la conexión de SignalR hereda automáticamente esta autenticación.</span><span class="sxs-lookup"><span data-stu-id="e6adf-117">If the user is logged in to your app, the SignalR connection automatically inherits this authentication.</span></span>

<span data-ttu-id="e6adf-118">Las cookies son una manera específica del explorador para enviar tokens de acceso, pero los clientes sin explorador pueden enviarlos.</span><span class="sxs-lookup"><span data-stu-id="e6adf-118">Cookies are a browser-specific way to send access tokens, but non-browser clients can send them.</span></span> <span data-ttu-id="e6adf-119">Cuando se usa el [cliente.NET](xref:signalr/dotnet-client), el `Cookies` propiedad se puede configurar en el `.WithUrl` llamada con el fin de proporcionar una cookie.</span><span class="sxs-lookup"><span data-stu-id="e6adf-119">When using the [.NET Client](xref:signalr/dotnet-client), the `Cookies` property can be configured in the `.WithUrl` call in order to provide a cookie.</span></span> <span data-ttu-id="e6adf-120">Sin embargo, requiere la aplicación para proporcionar una API para intercambiar datos de autenticación para una cookie mediante la autenticación de cookies del cliente. NET.</span><span class="sxs-lookup"><span data-stu-id="e6adf-120">However, using cookie authentication from the .NET Client requires the app to provide an API to exchange authentication data for a cookie.</span></span>

### <a name="bearer-token-authentication"></a><span data-ttu-id="e6adf-121">Autenticación de token de portador</span><span class="sxs-lookup"><span data-stu-id="e6adf-121">Bearer token authentication</span></span>

<span data-ttu-id="e6adf-122">El cliente puede proporcionar un token de acceso en lugar de usar una cookie.</span><span class="sxs-lookup"><span data-stu-id="e6adf-122">The client can provide an access token instead of using a cookie.</span></span> <span data-ttu-id="e6adf-123">El servidor valida el token y lo usa para identificar al usuario.</span><span class="sxs-lookup"><span data-stu-id="e6adf-123">The server validates the token and uses it to identify the user.</span></span> <span data-ttu-id="e6adf-124">Esta validación se realiza solo cuando se establece la conexión.</span><span class="sxs-lookup"><span data-stu-id="e6adf-124">This validation is done only when the connection is established.</span></span> <span data-ttu-id="e6adf-125">Durante la vida de la conexión, el servidor no validar automáticamente para comprobar la revocación del token.</span><span class="sxs-lookup"><span data-stu-id="e6adf-125">During the life of the connection, the server doesn't automatically revalidate to check for token revocation.</span></span>

<span data-ttu-id="e6adf-126">En el servidor, la autenticación de token de portador se configura mediante el [middleware de portador JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="e6adf-126">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="e6adf-127">En el cliente de JavaScript, se puede proporcionar el token mediante el [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) opción.</span><span class="sxs-lookup"><span data-stu-id="e6adf-127">In the JavaScript client, the token can be provided using the [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) option.</span></span>

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

<span data-ttu-id="e6adf-128">En el cliente. NET, hay un proceso similar [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) propiedad que se puede usar para configurar el token:</span><span class="sxs-lookup"><span data-stu-id="e6adf-128">In the .NET client, there is a similar [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) property that can be used to configure the token:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="e6adf-129">Se llama a la función de token de acceso que proporcione antes **cada** solicitud HTTP de SignalR.</span><span class="sxs-lookup"><span data-stu-id="e6adf-129">The access token function you provide is called before **every** HTTP request made by SignalR.</span></span> <span data-ttu-id="e6adf-130">Si necesita renovar el token con el fin de mantener la conexión activa (porque lo puede expirar durante la conexión), hacerlo desde esta función y devolver el token actualizado.</span><span class="sxs-lookup"><span data-stu-id="e6adf-130">If you need to renew the token in order to keep the connection active (because it may expire during the connection), do so from within this function and return the updated token.</span></span>

<span data-ttu-id="e6adf-131">En las API web estándar, se envían los tokens de portador en un encabezado HTTP.</span><span class="sxs-lookup"><span data-stu-id="e6adf-131">In standard web APIs, bearer tokens are sent in an HTTP header.</span></span> <span data-ttu-id="e6adf-132">Sin embargo, SignalR es no se puede establecer estos encabezados en los exploradores cuando se usa algunos transportes.</span><span class="sxs-lookup"><span data-stu-id="e6adf-132">However, SignalR is unable to set these headers in browsers when using some transports.</span></span> <span data-ttu-id="e6adf-133">Cuando se usa WebSockets y los eventos, el token se transmite como un parámetro de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="e6adf-133">When using WebSockets and Server-Sent Events, the token is transmitted as a query string parameter.</span></span> <span data-ttu-id="e6adf-134">Para admitir esto en el servidor, se requiere configuración adicional:</span><span class="sxs-lookup"><span data-stu-id="e6adf-134">In order to support this on the server, additional configuration is required:</span></span>

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="cookies-vs-bearer-tokens"></a><span data-ttu-id="e6adf-135">Cookies frente a los tokens de portador</span><span class="sxs-lookup"><span data-stu-id="e6adf-135">Cookies vs. bearer tokens</span></span> 

<span data-ttu-id="e6adf-136">Dado que las cookies son específicas de los exploradores, enviarlos de otros tipos de clientes agrega complejidad al comparado con el envío de tokens de portador.</span><span class="sxs-lookup"><span data-stu-id="e6adf-136">Because cookies are specific to browsers, sending them from other kinds of clients adds complexity compared to sending bearer tokens.</span></span> <span data-ttu-id="e6adf-137">Por este motivo, la autenticación de cookies no se recomienda a menos que la aplicación solo necesita autenticar a los usuarios desde el explorador del cliente.</span><span class="sxs-lookup"><span data-stu-id="e6adf-137">For this reason, cookie authentication isn't recommended unless the app only needs to authenticate users from the browser client.</span></span> <span data-ttu-id="e6adf-138">Autenticación de token de portador es el enfoque recomendado al usar a los clientes que no sea el explorador del cliente.</span><span class="sxs-lookup"><span data-stu-id="e6adf-138">Bearer token authentication is the recommended approach when using clients other than the browser client.</span></span>

### <a name="windows-authentication"></a><span data-ttu-id="e6adf-139">Autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="e6adf-139">Windows authentication</span></span>

<span data-ttu-id="e6adf-140">Si [autenticación de Windows](xref:security/authentication/windowsauth) está configurado en la aplicación, SignalR puede usar esa identidad para proteger los concentradores.</span><span class="sxs-lookup"><span data-stu-id="e6adf-140">If [Windows authentication](xref:security/authentication/windowsauth) is configured in your app, SignalR can use that identity to secure hubs.</span></span> <span data-ttu-id="e6adf-141">Sin embargo, para enviar mensajes a usuarios individuales, deberá agregar un proveedor de Id. de usuario personalizado.</span><span class="sxs-lookup"><span data-stu-id="e6adf-141">However, in order to send messages to individual users, you need to add a custom User ID provider.</span></span> <span data-ttu-id="e6adf-142">Esto es porque el sistema de autenticación de Windows no proporciona la notificación "NameIdentifier" que usa SignalR para determinar el nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="e6adf-142">This is because the Windows authentication system doesn't provide the "Name Identifier" claim that SignalR uses to determine the user name.</span></span>

<span data-ttu-id="e6adf-143">Agregue una nueva clase que implementa `IUserIdProvider` y recuperar una de las notificaciones del usuario que se usará como el identificador.</span><span class="sxs-lookup"><span data-stu-id="e6adf-143">Add a new class that implements `IUserIdProvider` and retrieve one of the claims from the user to use as the identifier.</span></span> <span data-ttu-id="e6adf-144">Por ejemplo, para usar la notificación "Name" (que es el nombre de usuario de Windows en el formulario `[Domain]\[Username]`), cree la siguiente clase:</span><span class="sxs-lookup"><span data-stu-id="e6adf-144">For example, to use the "Name" claim (which is the Windows username in the form `[Domain]\[Username]`), create the following class:</span></span>

[!code-csharp[Name based provider](authn-and-authz/sample/nameuseridprovider.cs?name=NameUserIdProvider)]

<span data-ttu-id="e6adf-145">En lugar de `ClaimTypes.Name`, puede usar cualquier valor de la `User` (por ejemplo, el identificador SID de Windows, etcetera.).</span><span class="sxs-lookup"><span data-stu-id="e6adf-145">Rather than `ClaimTypes.Name`, you can use any value from the `User` (such as the Windows SID identifier, etc.).</span></span>

> [!NOTE]
> <span data-ttu-id="e6adf-146">El valor que elija debe ser único entre todos los usuarios en el sistema.</span><span class="sxs-lookup"><span data-stu-id="e6adf-146">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="e6adf-147">En caso contrario, un mensaje destinado a un usuario podría acabar va a un usuario diferente.</span><span class="sxs-lookup"><span data-stu-id="e6adf-147">Otherwise, a message intended for one user could end up going to a different user.</span></span>

<span data-ttu-id="e6adf-148">Registrar este componente en su `Startup.ConfigureServices` método.</span><span class="sxs-lookup"><span data-stu-id="e6adf-148">Register this component in your `Startup.ConfigureServices` method.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

<span data-ttu-id="e6adf-149">En el cliente. NET, la autenticación de Windows debe estar habilitada estableciendo el [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) propiedad:</span><span class="sxs-lookup"><span data-stu-id="e6adf-149">In the .NET Client, Windows Authentication must be enabled by setting the [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) property:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

<span data-ttu-id="e6adf-150">Autenticación de Windows solo es compatible con el explorador del cliente cuando se usa Microsoft Internet Explorer o Microsoft Edge.</span><span class="sxs-lookup"><span data-stu-id="e6adf-150">Windows Authentication is only supported by the browser client when using Microsoft Internet Explorer or Microsoft Edge.</span></span>

### <a name="use-claims-to-customize-identity-handling"></a><span data-ttu-id="e6adf-151">Uso de notificaciones para personalizar el control de identidad</span><span class="sxs-lookup"><span data-stu-id="e6adf-151">Use claims to customize identity handling</span></span>

<span data-ttu-id="e6adf-152">Una aplicación que autentica a los usuarios puede derivar los identificadores de usuario de SignalR de notificaciones de usuario.</span><span class="sxs-lookup"><span data-stu-id="e6adf-152">An app that authenticates users can derive SignalR user IDs from user claims.</span></span> <span data-ttu-id="e6adf-153">Para especificar cómo SignalR crea los identificadores de usuario, implemente `IUserIdProvider` y registre la implementación.</span><span class="sxs-lookup"><span data-stu-id="e6adf-153">To specify how SignalR creates user IDs, implement `IUserIdProvider` and register the implementation.</span></span>

<span data-ttu-id="e6adf-154">El código de ejemplo muestra cómo se podría usar notificaciones para seleccionar la dirección de correo electrónico del usuario como la propiedad de identificación.</span><span class="sxs-lookup"><span data-stu-id="e6adf-154">The sample code demonstrates how you would use claims to select the user's email address as the identifying property.</span></span> 

> [!NOTE]
> <span data-ttu-id="e6adf-155">El valor que elija debe ser único entre todos los usuarios en el sistema.</span><span class="sxs-lookup"><span data-stu-id="e6adf-155">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="e6adf-156">En caso contrario, un mensaje destinado a un usuario podría acabar va a un usuario diferente.</span><span class="sxs-lookup"><span data-stu-id="e6adf-156">Otherwise, a message intended for one user could end up going to a different user.</span></span>

[!code-csharp[Email provider](authn-and-authz/sample/EmailBasedUserIdProvider.cs?name=EmailBasedUserIdProvider)]

<span data-ttu-id="e6adf-157">El registro de la cuenta agrega una notificación con el tipo `ClaimsTypes.Email` a la base de datos de identidad ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e6adf-157">The account registration adds a claim with type `ClaimsTypes.Email` to the ASP.NET identity database.</span></span>

[!code-csharp[Adding the email to the ASP.NET identity claims](authn-and-authz/sample/pages/account/Register.cshtml.cs?name=AddEmailClaim)]

<span data-ttu-id="e6adf-158">Registrar este componente en su `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e6adf-158">Register this component in your `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSingleton<IUserIdProvider, EmailBasedUserIdProvider>();
```

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a><span data-ttu-id="e6adf-159">Autorizar a los usuarios acceso a concentradores y métodos de concentrador</span><span class="sxs-lookup"><span data-stu-id="e6adf-159">Authorize users to access hubs and hub methods</span></span>

<span data-ttu-id="e6adf-160">De forma predeterminada, se pueden llamar a todos los métodos en un centro de un usuario autenticado.</span><span class="sxs-lookup"><span data-stu-id="e6adf-160">By default, all methods in a hub can be called by an unauthenticated user.</span></span> <span data-ttu-id="e6adf-161">Con el fin de solicitar la autenticación, se aplican la [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) al concentrador de atributo:</span><span class="sxs-lookup"><span data-stu-id="e6adf-161">In order to require authentication, apply the [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attribute to the hub:</span></span>

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

<span data-ttu-id="e6adf-162">Puede usar los argumentos de constructor y propiedades de la `[Authorize]` atributo para restringir el acceso solo a los usuarios de coincidencia específico [las directivas de autorización](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="e6adf-162">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="e6adf-163">Por ejemplo, si tiene una directiva de autorización personalizada denominada `MyAuthorizationPolicy` puede asegurarse de que solo los usuarios que coinciden con dicha directiva pueden tener acceso a centro con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="e6adf-163">For example, if you have a custom authorization policy called `MyAuthorizationPolicy` you can ensure that only users matching that policy can access the hub using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub: Hub
{
}
```

<span data-ttu-id="e6adf-164">Los métodos de concentrador individuales pueden tener el `[Authorize]` también aplicado el atributo.</span><span class="sxs-lookup"><span data-stu-id="e6adf-164">Individual hub methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="e6adf-165">Si el usuario actual no coincide con la directiva se aplica al método, se devuelve un error al llamador:</span><span class="sxs-lookup"><span data-stu-id="e6adf-165">If the current user doesn't match the policy applied to the method, an error is returned to the caller:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="e6adf-166">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e6adf-166">Additional resources</span></span>

* [<span data-ttu-id="e6adf-167">Autenticación de Token de portador en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6adf-167">Bearer Token Authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
