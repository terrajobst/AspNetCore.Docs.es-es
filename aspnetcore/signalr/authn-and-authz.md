---
title: Autenticación y autorización en ASP.NET Core Signalr
author: bradygaster
description: Aprenda a usar la autenticación y la autorización en ASP.NET Core Signalr.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 07/15/2019
uid: signalr/authn-and-authz
ms.openlocfilehash: e7e7a9fd537ba89b64c15594652a290357a00038
ms.sourcegitcommit: f30b18442ed12831c7e86b0db249183ccd749f59
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2019
ms.locfileid: "68412533"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a><span data-ttu-id="918f3-103">Autenticación y autorización en ASP.NET Core Signalr</span><span class="sxs-lookup"><span data-stu-id="918f3-103">Authentication and authorization in ASP.NET Core SignalR</span></span>

<span data-ttu-id="918f3-104">Por [Andrew Stanton-enfermera](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="918f3-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="918f3-105">[Ver o descargar el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(Cómo descargar)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="918f3-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a><span data-ttu-id="918f3-106">Autenticar a los usuarios que se conectan a un concentrador de Signalr</span><span class="sxs-lookup"><span data-stu-id="918f3-106">Authenticate users connecting to a SignalR hub</span></span>

<span data-ttu-id="918f3-107">Signalr se puede usar con la [autenticación de ASP.net Core](xref:security/authentication/identity) para asociar un usuario a cada conexión.</span><span class="sxs-lookup"><span data-stu-id="918f3-107">SignalR can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each connection.</span></span> <span data-ttu-id="918f3-108">En un concentrador, se puede tener acceso a los datos [`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) de autenticación desde la propiedad.</span><span class="sxs-lookup"><span data-stu-id="918f3-108">In a hub, authentication data can be accessed from the [`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) property.</span></span> <span data-ttu-id="918f3-109">La autenticación permite que el concentrador llame a métodos en todas las conexiones asociadas a un usuario (consulte [Administración de usuarios y grupos en signalr](xref:signalr/groups) para obtener más información).</span><span class="sxs-lookup"><span data-stu-id="918f3-109">Authentication allows the hub to call methods on all connections associated with a user (See [Manage users and groups in SignalR](xref:signalr/groups) for more information).</span></span> <span data-ttu-id="918f3-110">Es posible que varias conexiones estén asociadas a un solo usuario.</span><span class="sxs-lookup"><span data-stu-id="918f3-110">Multiple connections may be associated with a single user.</span></span>

<span data-ttu-id="918f3-111">A continuación se facilita un ejemplo `Startup.Configure` de uso de signalr y la autenticación ASP.net Core:</span><span class="sxs-lookup"><span data-stu-id="918f3-111">The following is an example of `Startup.Configure` which uses SignalR and ASP.NET Core authentication:</span></span>

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
> <span data-ttu-id="918f3-112">El orden en el que se registra el middleware de Signalr y ASP.NET Core es importante.</span><span class="sxs-lookup"><span data-stu-id="918f3-112">The order in which you register the SignalR and ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="918f3-113">Llame siempre `UseAuthentication` a `UseSignalR` antes de para que signalr `HttpContext`tenga un usuario en.</span><span class="sxs-lookup"><span data-stu-id="918f3-113">Always call `UseAuthentication` before `UseSignalR` so that SignalR has a user on the `HttpContext`.</span></span>

### <a name="cookie-authentication"></a><span data-ttu-id="918f3-114">Autenticación de cookies</span><span class="sxs-lookup"><span data-stu-id="918f3-114">Cookie authentication</span></span>

<span data-ttu-id="918f3-115">En una aplicación basada en explorador, la autenticación de cookies permite que las credenciales de usuario existentes fluyan automáticamente a las conexiones de Signalr.</span><span class="sxs-lookup"><span data-stu-id="918f3-115">In a browser-based app, cookie authentication allows your existing user credentials to automatically flow to SignalR connections.</span></span> <span data-ttu-id="918f3-116">Cuando se usa el cliente del explorador, no se necesita ninguna configuración adicional.</span><span class="sxs-lookup"><span data-stu-id="918f3-116">When using the browser client, no additional configuration is needed.</span></span> <span data-ttu-id="918f3-117">Si el usuario ha iniciado sesión en la aplicación, la conexión de Signalr hereda automáticamente esta autenticación.</span><span class="sxs-lookup"><span data-stu-id="918f3-117">If the user is logged in to your app, the SignalR connection automatically inherits this authentication.</span></span>

<span data-ttu-id="918f3-118">Las cookies son una forma específica del explorador de enviar tokens de acceso, pero los clientes que no son de explorador pueden enviarlas.</span><span class="sxs-lookup"><span data-stu-id="918f3-118">Cookies are a browser-specific way to send access tokens, but non-browser clients can send them.</span></span> <span data-ttu-id="918f3-119">Al utilizar el [cliente .net](xref:signalr/dotnet-client), la `Cookies` propiedad se puede configurar en la `.WithUrl` llamada para proporcionar una cookie.</span><span class="sxs-lookup"><span data-stu-id="918f3-119">When using the [.NET Client](xref:signalr/dotnet-client), the `Cookies` property can be configured in the `.WithUrl` call in order to provide a cookie.</span></span> <span data-ttu-id="918f3-120">Sin embargo, el uso de la autenticación de cookies del cliente .NET requiere que la aplicación proporcione una API para intercambiar los datos de autenticación de una cookie.</span><span class="sxs-lookup"><span data-stu-id="918f3-120">However, using cookie authentication from the .NET Client requires the app to provide an API to exchange authentication data for a cookie.</span></span>

### <a name="bearer-token-authentication"></a><span data-ttu-id="918f3-121">Autenticación de token de portador</span><span class="sxs-lookup"><span data-stu-id="918f3-121">Bearer token authentication</span></span>

<span data-ttu-id="918f3-122">El cliente puede proporcionar un token de acceso en lugar de usar una cookie.</span><span class="sxs-lookup"><span data-stu-id="918f3-122">The client can provide an access token instead of using a cookie.</span></span> <span data-ttu-id="918f3-123">El servidor valida el token y lo usa para identificar al usuario.</span><span class="sxs-lookup"><span data-stu-id="918f3-123">The server validates the token and uses it to identify the user.</span></span> <span data-ttu-id="918f3-124">Esta validación se realiza solo cuando se establece la conexión.</span><span class="sxs-lookup"><span data-stu-id="918f3-124">This validation is done only when the connection is established.</span></span> <span data-ttu-id="918f3-125">Durante la vida de la conexión, el servidor no se vuelve a validar automáticamente para comprobar la revocación de tokens.</span><span class="sxs-lookup"><span data-stu-id="918f3-125">During the life of the connection, the server doesn't automatically revalidate to check for token revocation.</span></span>

<span data-ttu-id="918f3-126">En el servidor, la autenticación de token de portador se configura mediante el [middleware portador de JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="918f3-126">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="918f3-127">En el cliente de JavaScript, el token se puede proporcionar mediante la opción [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) .</span><span class="sxs-lookup"><span data-stu-id="918f3-127">In the JavaScript client, the token can be provided using the [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) option.</span></span>

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

<span data-ttu-id="918f3-128">En el cliente de .NET, hay una propiedad [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) similar que se puede usar para configurar el token:</span><span class="sxs-lookup"><span data-stu-id="918f3-128">In the .NET client, there is a similar [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) property that can be used to configure the token:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => Task.FromResult(_myAccessToken);
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="918f3-129">Se llama a la función de token de acceso que se proporciona antes de **cada** solicitud HTTP realizada por signalr.</span><span class="sxs-lookup"><span data-stu-id="918f3-129">The access token function you provide is called before **every** HTTP request made by SignalR.</span></span> <span data-ttu-id="918f3-130">Si necesita renovar el token para mantener la conexión activa (porque puede expirar durante la conexión), hágalo desde esta función y devuelva el token actualizado.</span><span class="sxs-lookup"><span data-stu-id="918f3-130">If you need to renew the token in order to keep the connection active (because it may expire during the connection), do so from within this function and return the updated token.</span></span>

<span data-ttu-id="918f3-131">En las API Web estándar, los tokens de portador se envían en un encabezado HTTP.</span><span class="sxs-lookup"><span data-stu-id="918f3-131">In standard web APIs, bearer tokens are sent in an HTTP header.</span></span> <span data-ttu-id="918f3-132">Sin embargo, Signalr no puede establecer estos encabezados en exploradores cuando se usan algunos transportes.</span><span class="sxs-lookup"><span data-stu-id="918f3-132">However, SignalR is unable to set these headers in browsers when using some transports.</span></span> <span data-ttu-id="918f3-133">Cuando se usan WebSockets y eventos enviados por el servidor, el token se transmite como un parámetro de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="918f3-133">When using WebSockets and Server-Sent Events, the token is transmitted as a query string parameter.</span></span> <span data-ttu-id="918f3-134">Para admitir esto en el servidor, se requiere una configuración adicional:</span><span class="sxs-lookup"><span data-stu-id="918f3-134">In order to support this on the server, additional configuration is required:</span></span>

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="cookies-vs-bearer-tokens"></a><span data-ttu-id="918f3-135">Cookies frente a tokens de portador</span><span class="sxs-lookup"><span data-stu-id="918f3-135">Cookies vs. bearer tokens</span></span> 

<span data-ttu-id="918f3-136">Dado que las cookies son específicas de los exploradores, enviarlos desde otros tipos de clientes agrega complejidad en comparación con el envío de tokens de portador.</span><span class="sxs-lookup"><span data-stu-id="918f3-136">Because cookies are specific to browsers, sending them from other kinds of clients adds complexity compared to sending bearer tokens.</span></span> <span data-ttu-id="918f3-137">Por esta razón, no se recomienda la autenticación con cookies a menos que la aplicación solo necesite autenticar a los usuarios desde el cliente del explorador.</span><span class="sxs-lookup"><span data-stu-id="918f3-137">For this reason, cookie authentication isn't recommended unless the app only needs to authenticate users from the browser client.</span></span> <span data-ttu-id="918f3-138">La autenticación de token de portador es el enfoque recomendado al usar clientes que no sean el cliente del explorador.</span><span class="sxs-lookup"><span data-stu-id="918f3-138">Bearer token authentication is the recommended approach when using clients other than the browser client.</span></span>

### <a name="windows-authentication"></a><span data-ttu-id="918f3-139">Autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="918f3-139">Windows authentication</span></span>

<span data-ttu-id="918f3-140">Si la [autenticación de Windows](xref:security/authentication/windowsauth) está configurada en la aplicación, signalr puede usar esa identidad para proteger los concentradores.</span><span class="sxs-lookup"><span data-stu-id="918f3-140">If [Windows authentication](xref:security/authentication/windowsauth) is configured in your app, SignalR can use that identity to secure hubs.</span></span> <span data-ttu-id="918f3-141">Sin embargo, para poder enviar mensajes a usuarios individuales, debe agregar un proveedor de ID. de usuario personalizado.</span><span class="sxs-lookup"><span data-stu-id="918f3-141">However, in order to send messages to individual users, you need to add a custom User ID provider.</span></span> <span data-ttu-id="918f3-142">Esto se debe a que el sistema de autenticación de Windows no proporciona la petición de "identificador de nombre" que Signalr usa para determinar el nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="918f3-142">This is because the Windows authentication system doesn't provide the "Name Identifier" claim that SignalR uses to determine the user name.</span></span>

<span data-ttu-id="918f3-143">Agregue una nueva clase que implemente `IUserIdProvider` y recupere una de las notificaciones del usuario que se va a usar como identificador.</span><span class="sxs-lookup"><span data-stu-id="918f3-143">Add a new class that implements `IUserIdProvider` and retrieve one of the claims from the user to use as the identifier.</span></span> <span data-ttu-id="918f3-144">Por ejemplo, para usar la notificaciones "Name" (que es el nombre de usuario de `[Domain]\[Username]`Windows en el formulario), cree la clase siguiente:</span><span class="sxs-lookup"><span data-stu-id="918f3-144">For example, to use the "Name" claim (which is the Windows username in the form `[Domain]\[Username]`), create the following class:</span></span>

[!code-csharp[Name based provider](authn-and-authz/sample/nameuseridprovider.cs?name=NameUserIdProvider)]

<span data-ttu-id="918f3-145">En lugar `ClaimTypes.Name`de, puede usar cualquier valor `User` de (como el identificador de SID de Windows, etc.).</span><span class="sxs-lookup"><span data-stu-id="918f3-145">Rather than `ClaimTypes.Name`, you can use any value from the `User` (such as the Windows SID identifier, etc.).</span></span>

> [!NOTE]
> <span data-ttu-id="918f3-146">El valor que elija debe ser único entre todos los usuarios del sistema.</span><span class="sxs-lookup"><span data-stu-id="918f3-146">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="918f3-147">De lo contrario, un mensaje destinado a un usuario podría acabar pasando a otro usuario.</span><span class="sxs-lookup"><span data-stu-id="918f3-147">Otherwise, a message intended for one user could end up going to a different user.</span></span>

<span data-ttu-id="918f3-148">Registre este componente en el `Startup.ConfigureServices` método.</span><span class="sxs-lookup"><span data-stu-id="918f3-148">Register this component in your `Startup.ConfigureServices` method.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

<span data-ttu-id="918f3-149">En el cliente de .NET, la autenticación de Windows debe habilitarse estableciendo la propiedad [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) :</span><span class="sxs-lookup"><span data-stu-id="918f3-149">In the .NET Client, Windows Authentication must be enabled by setting the [UseDefaultCredentials](/dotnet/api/microsoft.aspnetcore.http.connections.client.httpconnectionoptions.usedefaultcredentials) property:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    {
        options.UseDefaultCredentials = true;
    })
    .Build();
```

<span data-ttu-id="918f3-150">La autenticación de Windows solo es compatible con el cliente del explorador al usar Microsoft Internet Explorer o Microsoft Edge.</span><span class="sxs-lookup"><span data-stu-id="918f3-150">Windows Authentication is only supported by the browser client when using Microsoft Internet Explorer or Microsoft Edge.</span></span>

### <a name="use-claims-to-customize-identity-handling"></a><span data-ttu-id="918f3-151">Usar notificaciones para personalizar el control de identidades</span><span class="sxs-lookup"><span data-stu-id="918f3-151">Use claims to customize identity handling</span></span>

<span data-ttu-id="918f3-152">Una aplicación que autentica a los usuarios puede derivar los identificadores de usuario de Signalr de las notificaciones de usuario.</span><span class="sxs-lookup"><span data-stu-id="918f3-152">An app that authenticates users can derive SignalR user IDs from user claims.</span></span> <span data-ttu-id="918f3-153">Para especificar cómo signalr crea los ID. de `IUserIdProvider` usuario, implemente y registre la implementación.</span><span class="sxs-lookup"><span data-stu-id="918f3-153">To specify how SignalR creates user IDs, implement `IUserIdProvider` and register the implementation.</span></span>

<span data-ttu-id="918f3-154">En el código de ejemplo se muestra cómo usar las notificaciones para seleccionar la dirección de correo electrónico del usuario como la propiedad que identifica.</span><span class="sxs-lookup"><span data-stu-id="918f3-154">The sample code demonstrates how you would use claims to select the user's email address as the identifying property.</span></span> 

> [!NOTE]
> <span data-ttu-id="918f3-155">El valor que elija debe ser único entre todos los usuarios del sistema.</span><span class="sxs-lookup"><span data-stu-id="918f3-155">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="918f3-156">De lo contrario, un mensaje destinado a un usuario podría acabar pasando a otro usuario.</span><span class="sxs-lookup"><span data-stu-id="918f3-156">Otherwise, a message intended for one user could end up going to a different user.</span></span>

[!code-csharp[Email provider](authn-and-authz/sample/EmailBasedUserIdProvider.cs?name=EmailBasedUserIdProvider)]

<span data-ttu-id="918f3-157">El registro de la cuenta agrega una demanda `ClaimsTypes.Email` con el tipo a la base de datos de identidad de ASP.net.</span><span class="sxs-lookup"><span data-stu-id="918f3-157">The account registration adds a claim with type `ClaimsTypes.Email` to the ASP.NET identity database.</span></span>

[!code-csharp[Adding the email to the ASP.NET identity claims](authn-and-authz/sample/pages/account/Register.cshtml.cs?name=AddEmailClaim)]

<span data-ttu-id="918f3-158">Registre este componente en su `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="918f3-158">Register this component in your `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSingleton<IUserIdProvider, EmailBasedUserIdProvider>();
```

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a><span data-ttu-id="918f3-159">Autorización para que los usuarios accedan a los concentradores y métodos de concentrador</span><span class="sxs-lookup"><span data-stu-id="918f3-159">Authorize users to access hubs and hub methods</span></span>

<span data-ttu-id="918f3-160">De forma predeterminada, los usuarios no autenticados pueden llamar a todos los métodos de un concentrador.</span><span class="sxs-lookup"><span data-stu-id="918f3-160">By default, all methods in a hub can be called by an unauthenticated user.</span></span> <span data-ttu-id="918f3-161">Para requerir la autenticación, aplique el atributo [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) al centro:</span><span class="sxs-lookup"><span data-stu-id="918f3-161">In order to require authentication, apply the [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attribute to the hub:</span></span>

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

<span data-ttu-id="918f3-162">Puede usar los argumentos de constructor y las propiedades del `[Authorize]` atributo para restringir el acceso solo a los usuarios que coincidan con [las directivas de autorización](xref:security/authorization/policies)específicas.</span><span class="sxs-lookup"><span data-stu-id="918f3-162">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="918f3-163">Por ejemplo, si tiene una directiva de autorización personalizada denominada `MyAuthorizationPolicy` , puede asegurarse de que solo los usuarios que coincidan con esa Directiva puedan tener acceso al centro mediante el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="918f3-163">For example, if you have a custom authorization policy called `MyAuthorizationPolicy` you can ensure that only users matching that policy can access the hub using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub : Hub
{
}
```

<span data-ttu-id="918f3-164">Los métodos de concentrador individuales `[Authorize]` también pueden tener el atributo aplicado.</span><span class="sxs-lookup"><span data-stu-id="918f3-164">Individual hub methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="918f3-165">Si el usuario actual no coincide con la Directiva que se aplica al método, se devuelve un error al autor de la llamada:</span><span class="sxs-lookup"><span data-stu-id="918f3-165">If the current user doesn't match the policy applied to the method, an error is returned to the caller:</span></span>

```csharp
[Authorize]
public class ChatHub : Hub
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

::: moniker range=">= aspnetcore-3.0"

### <a name="use-authorization-handlers-to-customize-hub-method-authorization"></a><span data-ttu-id="918f3-166">Usar controladores de autorización para personalizar la autorización del método de concentrador</span><span class="sxs-lookup"><span data-stu-id="918f3-166">Use authorization handlers to customize hub method authorization</span></span>

<span data-ttu-id="918f3-167">Signalr proporciona un recurso personalizado a los controladores de autorización cuando un método de concentrador requiere autorización.</span><span class="sxs-lookup"><span data-stu-id="918f3-167">SignalR provides a custom resource to authorization handlers when a hub method requires authorization.</span></span> <span data-ttu-id="918f3-168">El recurso es una instancia de `HubInvocationContext`.</span><span class="sxs-lookup"><span data-stu-id="918f3-168">The resource is an instance of `HubInvocationContext`.</span></span> <span data-ttu-id="918f3-169">`HubInvocationContext` Incluye,elnombredelmétododeconcentradorquesevaainvocarylosargumentosparael`HubCallerContext`método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="918f3-169">The `HubInvocationContext` includes the `HubCallerContext`, the name of the hub method being invoked, and the arguments to the hub method.</span></span>

<span data-ttu-id="918f3-170">Considere el ejemplo de un salón de chat que permite el inicio de sesión en varias organizaciones a través de Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="918f3-170">Consider the example of a chat room allowing multiple organization sign-in via Azure Active Directory.</span></span> <span data-ttu-id="918f3-171">Cualquier persona con un cuenta de Microsoft puede iniciar sesión en el chat, pero solo los miembros de la organización propietaria deben ser capaces de prohibir a los usuarios o ver el historial de chat de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="918f3-171">Anyone with a Microsoft account can sign in to chat, but only members of the owning organization should be able to ban users or view users' chat histories.</span></span> <span data-ttu-id="918f3-172">Además, es posible que quiera restringir ciertas funciones de determinados usuarios.</span><span class="sxs-lookup"><span data-stu-id="918f3-172">Furthermore, we might want to restrict certain functionality from certain users.</span></span> <span data-ttu-id="918f3-173">El uso de las características actualizadas en ASP.NET Core 3,0 es todo lo posible.</span><span class="sxs-lookup"><span data-stu-id="918f3-173">Using the updated features in ASP.NET Core 3.0, this is entirely possible.</span></span> <span data-ttu-id="918f3-174">Observe cómo `DomainRestrictedRequirement` actúa como un personalizado `IAuthorizationRequirement`.</span><span class="sxs-lookup"><span data-stu-id="918f3-174">Note how the `DomainRestrictedRequirement` serves as a custom `IAuthorizationRequirement`.</span></span> <span data-ttu-id="918f3-175">Ahora que se `HubInvocationContext` pasa el parámetro de recurso, la lógica interna puede inspeccionar el contexto en el que se llama al centro y tomar decisiones sobre cómo permitir al usuario ejecutar métodos de concentrador individuales.</span><span class="sxs-lookup"><span data-stu-id="918f3-175">Now that the `HubInvocationContext` resource parameter is being passed in, the internal logic can inspect the context in which the Hub is being called and make decisions on allowing the user to execute individual Hub methods.</span></span>

```csharp
[Authorize]
public class ChatHub : Hub
{
    public void SendMessage(string message)
    {
    }

    [Authorize("DomainRestricted")]
    public void BanUser(string username)
    {
    }

    [Authorize("DomainRestricted")]
    public void ViewUserHistory(string username)
    {
    }
}

public class DomainRestrictedRequirement : 
    AuthorizationHandler<DomainRestrictedRequirement, HubInvocationContext>, 
    IAuthorizationRequirement
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context,
        DomainRestrictedRequirement requirement, 
        HubInvocationContext resource)
    {
        if (IsUserAllowedToDoThis(resource.HubMethodName, context.User.Identity.Name) && 
            context.User.Identity.Name.EndsWith("@microsoft.com"))
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }

    private bool IsUserAllowedToDoThis(string hubMethodName,
        string currentUsername)
    {
        return !(currentUsername.Equals("asdf42@microsoft.com") && 
            hubMethodName.Equals("banUser", StringComparison.OrdinalIgnoreCase));
    }
}
```

<span data-ttu-id="918f3-176">En `Startup.ConfigureServices`, agregue la nueva Directiva y proporcione el requisito `DomainRestrictedRequirement` personalizado como parámetro para crear la `DomainRestricted` Directiva.</span><span class="sxs-lookup"><span data-stu-id="918f3-176">In `Startup.ConfigureServices`, add the new policy, providing the custom `DomainRestrictedRequirement` requirement as a parameter to create the `DomainRestricted` policy.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services
        .AddAuthorization(options =>
        {
            options.AddPolicy("DomainRestricted", policy =>
            {
                policy.Requirements.Add(new DomainRestrictedRequirement());
            });
        });
}
```

<span data-ttu-id="918f3-177">En el ejemplo anterior, la `DomainRestrictedRequirement` clase es `IAuthorizationRequirement` y la suya propia `AuthorizationHandler` para ese requisito.</span><span class="sxs-lookup"><span data-stu-id="918f3-177">In the preceding example, the `DomainRestrictedRequirement` class is both an `IAuthorizationRequirement` and its own `AuthorizationHandler` for that requirement.</span></span> <span data-ttu-id="918f3-178">Es aceptable dividir estos dos componentes en clases independientes para separar los problemas.</span><span class="sxs-lookup"><span data-stu-id="918f3-178">It's acceptable to split these two components into separate classes to separate concerns.</span></span> <span data-ttu-id="918f3-179">Una ventaja del enfoque del ejemplo es que no es necesario insertar `AuthorizationHandler` durante el inicio, ya que el requisito y el controlador son los mismos.</span><span class="sxs-lookup"><span data-stu-id="918f3-179">A benefit of the example's approach is there's no need to inject the `AuthorizationHandler` during startup, as the requirement and the handler are the same thing.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="918f3-180">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="918f3-180">Additional resources</span></span>

* [<span data-ttu-id="918f3-181">Autenticación de token de portador en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="918f3-181">Bearer Token Authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [<span data-ttu-id="918f3-182">Autorización basada en recursos</span><span class="sxs-lookup"><span data-stu-id="918f3-182">Resource-based Authorization</span></span>](xref:security/authorization/resourcebased)
