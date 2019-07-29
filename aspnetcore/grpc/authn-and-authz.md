---
title: Autenticación y autorización en gRPC para ASP.NET Core
author: jamesnk
description: Obtenga información sobre cómo usar la autenticación y la autorización en gRPC para ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 07/26/2019
uid: grpc/authn-and-authz
ms.openlocfilehash: 34f7f8a5a22159329b3d6c4524943434c460c7fb
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602430"
---
# <a name="authentication-and-authorization-in-grpc-for-aspnet-core"></a><span data-ttu-id="b0776-103">Autenticación y autorización en gRPC para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b0776-103">Authentication and authorization in gRPC for ASP.NET Core</span></span>

<span data-ttu-id="b0776-104">Por [James Newton-King](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="b0776-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="b0776-105">[Ver o descargar el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(Cómo descargar)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="b0776-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-calling-a-grpc-service"></a><span data-ttu-id="b0776-106">Autenticar a los usuarios que llaman a un servicio gRPC</span><span class="sxs-lookup"><span data-stu-id="b0776-106">Authenticate users calling a gRPC service</span></span>

<span data-ttu-id="b0776-107">gRPC se puede usar con la [autenticación ASP.net Core](xref:security/authentication/identity) para asociar un usuario a cada llamada.</span><span class="sxs-lookup"><span data-stu-id="b0776-107">gRPC can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each call.</span></span>

<span data-ttu-id="b0776-108">En el siguiente ejemplo `Startup.Configure` se usa gRPC y la autenticación ASP.net Core:</span><span class="sxs-lookup"><span data-stu-id="b0776-108">The following is an example of `Startup.Configure` which uses gRPC and ASP.NET Core authentication:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseRouting();
    
    app.UseAuthentication();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        routes.MapGrpcService<GreeterService>();
    });
}
```

> [!NOTE]
> <span data-ttu-id="b0776-109">El orden en el que se registra el middleware de autenticación ASP.NET Core es importante.</span><span class="sxs-lookup"><span data-stu-id="b0776-109">The order in which you register the ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="b0776-110">Llame `UseAuthentication` siempre a `UseAuthorization` y `UseRouting` después de `UseEndpoints`y antes de.</span><span class="sxs-lookup"><span data-stu-id="b0776-110">Always call `UseAuthentication` and `UseAuthorization` after `UseRouting` and before `UseEndpoints`.</span></span>

<span data-ttu-id="b0776-111">Una vez que se ha configurado la autenticación, se puede tener acceso al usuario en un método de `ServerCallContext`servicio gRPC a través de.</span><span class="sxs-lookup"><span data-stu-id="b0776-111">Once authentication has been setup, the user can be accessed in a gRPC service methods via the `ServerCallContext`.</span></span>

```csharp
public override Task<BuyTicketsResponse> BuyTickets(
    BuyTicketsRequest request, ServerCallContext context)
{
    var user = context.GetHttpContext().User;

    // ... access data from ClaimsPrincipal ...
}

```

### <a name="bearer-token-authentication"></a><span data-ttu-id="b0776-112">Autenticación de token de portador</span><span class="sxs-lookup"><span data-stu-id="b0776-112">Bearer token authentication</span></span>

<span data-ttu-id="b0776-113">El cliente puede proporcionar un token de acceso para la autenticación.</span><span class="sxs-lookup"><span data-stu-id="b0776-113">The client can provide an access token for authentication.</span></span> <span data-ttu-id="b0776-114">El servidor valida el token y lo usa para identificar al usuario.</span><span class="sxs-lookup"><span data-stu-id="b0776-114">The server validates the token and uses it to identify the user.</span></span>

<span data-ttu-id="b0776-115">En el servidor, la autenticación de token de portador se configura mediante el [middleware portador de JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="b0776-115">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="b0776-116">En el cliente gRPC de .NET, el token se puede enviar con llamadas como encabezado:</span><span class="sxs-lookup"><span data-stu-id="b0776-116">In the .NET gRPC client, the token can be sent with calls as a header:</span></span>

```csharp
public bool DoAuthenticatedCall(
    Ticketer.TicketerClient client, string token)
{
    var headers = new Metadata();
    headers.Add("Authorization", $"Bearer {token}");

    var request = new BuyTicketsRequest { Count = 1 };
    var response = await client.BuyTicketsAsync(request, headers);

    return response.Success;
}
```

### <a name="client-certificate-authentication"></a><span data-ttu-id="b0776-117">Autenticación de certificado de cliente</span><span class="sxs-lookup"><span data-stu-id="b0776-117">Client certificate authentication</span></span>

<span data-ttu-id="b0776-118">Un cliente podría proporcionar alternativamente un certificado de cliente para la autenticación.</span><span class="sxs-lookup"><span data-stu-id="b0776-118">A client could alternatively provide a client certificate for authentication.</span></span> <span data-ttu-id="b0776-119">La [autenticación de certificados](https://tools.ietf.org/html/rfc5246#section-7.4.4) se produce en el nivel de TLS, mucho antes de que llegue a ASP.net Core.</span><span class="sxs-lookup"><span data-stu-id="b0776-119">[Certificate authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="b0776-120">Cuando la solicitud entra en ASP.NET Core, el [paquete de autenticación de certificados de cliente](xref:security/authentication/certauth) permite resolver el certificado `ClaimsPrincipal`en un.</span><span class="sxs-lookup"><span data-stu-id="b0776-120">When the request enters ASP.NET Core, the [client certificate authentication package](xref:security/authentication/certauth) allows you to resolve the certificate to a `ClaimsPrincipal`.</span></span>

> [!NOTE]
> <span data-ttu-id="b0776-121">El host debe estar configurado para aceptar certificados de cliente.</span><span class="sxs-lookup"><span data-stu-id="b0776-121">The host needs to be configured to accept client certificates.</span></span> <span data-ttu-id="b0776-122">Consulte [configuración del host para requerir certificados](xref:security/authentication/certauth#configure-your-host-to-require-certificates) para obtener información sobre la aceptación de certificados de cliente en KESTREL, IIS y Azure.</span><span class="sxs-lookup"><span data-stu-id="b0776-122">See [configure your host to require certificates](xref:security/authentication/certauth#configure-your-host-to-require-certificates) for information on accepting client certificates in Kestrel, IIS and Azure.</span></span>

<span data-ttu-id="b0776-123">En el cliente gRPC de .net, se agrega el certificado de `HttpClientHandler` cliente a que se usa para crear el cliente de gRPC:</span><span class="sxs-lookup"><span data-stu-id="b0776-123">In the .NET gRPC client, the client certificate is added to `HttpClientHandler` that is then used to create the gRPC client:</span></span>

```csharp
public Ticketer.TicketerClient CreateClientWithCert(
    string baseAddress,
    X509Certificate2 certificate)
{
    // Add client cert to the handler
    var handler = new HttpClientHandler();
    handler.ClientCertificates.Add(certificate);

    // Create the gRPC client
    var httpClient = new HttpClient(handler);
    httpClient.BaseAddress = new Uri(baseAddress);

    return GrpcClient.Create<Ticketer.TicketerClient>(httpClient);
}
```

### <a name="other-authentication-mechanisms"></a><span data-ttu-id="b0776-124">Otros mecanismos de autenticación</span><span class="sxs-lookup"><span data-stu-id="b0776-124">Other authentication mechanisms</span></span>

<span data-ttu-id="b0776-125">Muchos ASP.NET Core mecanismos de autenticación admitidos funcionan con gRPC:</span><span class="sxs-lookup"><span data-stu-id="b0776-125">Many ASP.NET Core supported authentication mechanisms work with gRPC:</span></span>

* <span data-ttu-id="b0776-126">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b0776-126">Azure Active Directory</span></span>
* <span data-ttu-id="b0776-127">Certificado de cliente</span><span class="sxs-lookup"><span data-stu-id="b0776-127">Client Certificate</span></span>
* <span data-ttu-id="b0776-128">IdentityServer</span><span class="sxs-lookup"><span data-stu-id="b0776-128">IdentityServer</span></span>
* <span data-ttu-id="b0776-129">Token de JWT</span><span class="sxs-lookup"><span data-stu-id="b0776-129">JWT Token</span></span>
* <span data-ttu-id="b0776-130">OAuth 2,0</span><span class="sxs-lookup"><span data-stu-id="b0776-130">OAuth 2.0</span></span>
* <span data-ttu-id="b0776-131">OpenID Connect</span><span class="sxs-lookup"><span data-stu-id="b0776-131">OpenID Connect</span></span>
* <span data-ttu-id="b0776-132">WS-Federation</span><span class="sxs-lookup"><span data-stu-id="b0776-132">WS-Federation</span></span>

<span data-ttu-id="b0776-133">Para obtener más información sobre la configuración de la autenticación en el servidor, consulte [ASP.net Core la autenticación](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="b0776-133">For more information on configuring authentication on the server, see [ASP.NET Core authentication](xref:security/authentication/identity).</span></span>

<span data-ttu-id="b0776-134">La configuración del cliente de gRPC para usar la autenticación dependerá del mecanismo de autenticación que se use.</span><span class="sxs-lookup"><span data-stu-id="b0776-134">Configuring the gRPC client to use authentication will depend on the authentication mechanism you are using.</span></span> <span data-ttu-id="b0776-135">El token de portador anterior y los ejemplos de certificados de cliente muestran un par de formas en que se puede configurar el cliente de gRPC para enviar metadatos de autenticación con llamadas gRPC:</span><span class="sxs-lookup"><span data-stu-id="b0776-135">The previous bearer token and client certificate examples show a couple of ways the gRPC client can be configured to send authentication metadata with gRPC calls:</span></span>

* <span data-ttu-id="b0776-136">Los clientes de gRPC fuertemente tipados usan `HttpClient` internamente.</span><span class="sxs-lookup"><span data-stu-id="b0776-136">Strongly typed gRPC clients use `HttpClient` internally.</span></span> <span data-ttu-id="b0776-137">Se puede configurar la [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler) `HttpClient`autenticación en o agregando instancias [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) personalizadas a.</span><span class="sxs-lookup"><span data-stu-id="b0776-137">Authentication can be configured on [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler), or by adding custom [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) instances to the `HttpClient`.</span></span>
* <span data-ttu-id="b0776-138">Cada llamada a gRPC tiene un `CallOptions` argumento opcional.</span><span class="sxs-lookup"><span data-stu-id="b0776-138">Each gRPC call has an optional `CallOptions` argument.</span></span> <span data-ttu-id="b0776-139">Los encabezados personalizados se pueden enviar mediante la colección de encabezados de la opción.</span><span class="sxs-lookup"><span data-stu-id="b0776-139">Custom headers can be sent using the option's headers collection.</span></span>

> [!NOTE]
> <span data-ttu-id="b0776-140">La autenticación de Windows (NTLM/Kerberos/Negotiate) no se puede usar con gRPC.</span><span class="sxs-lookup"><span data-stu-id="b0776-140">Windows Authentication (NTLM/Kerberos/Negotiate) can't be used with gRPC.</span></span> <span data-ttu-id="b0776-141">gRPC requiere HTTP/2 y HTTP/2 no admite la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="b0776-141">gRPC requires HTTP/2, and HTTP/2 doesn't support Windows Authentication.</span></span>

## <a name="authorize-users-to-access-services-and-service-methods"></a><span data-ttu-id="b0776-142">Autorizar a los usuarios para obtener acceso a servicios y métodos de servicio</span><span class="sxs-lookup"><span data-stu-id="b0776-142">Authorize users to access services and service methods</span></span>

<span data-ttu-id="b0776-143">De forma predeterminada, los usuarios no autenticados pueden llamar a todos los métodos de un servicio.</span><span class="sxs-lookup"><span data-stu-id="b0776-143">By default, all methods in a service can be called by unauthenticated users.</span></span> <span data-ttu-id="b0776-144">Para requerir autenticación, aplique el atributo [[Authorize]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) al servicio:</span><span class="sxs-lookup"><span data-stu-id="b0776-144">To require authentication, apply the [[Authorize]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute to the service:</span></span>

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="b0776-145">Puede usar los argumentos de constructor y las propiedades del `[Authorize]` atributo para restringir el acceso solo a los usuarios que coincidan con [las directivas de autorización](xref:security/authorization/policies)específicas.</span><span class="sxs-lookup"><span data-stu-id="b0776-145">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="b0776-146">Por ejemplo, si tiene una directiva de autorización personalizada denominada `MyAuthorizationPolicy`, asegúrese de que solo los usuarios que coincidan con esa Directiva puedan tener acceso al servicio mediante el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="b0776-146">For example, if you have a custom authorization policy called `MyAuthorizationPolicy`, ensure that only users matching that policy can access the service using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="b0776-147">Los métodos de servicio individuales también `[Authorize]` pueden tener el atributo aplicado.</span><span class="sxs-lookup"><span data-stu-id="b0776-147">Individual service methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="b0776-148">Si el usuario actual no coincide con las directivas que se aplican **al método** y a la clase, se devuelve un error al autor de la llamada:</span><span class="sxs-lookup"><span data-stu-id="b0776-148">If the current user doesn't match the policies applied to **both** the method and the class, an error is returned to the caller:</span></span>

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
    public override Task<AvailableTicketsResponse> GetAvailableTickets(
        Empty request, ServerCallContext context)
    {
        // ... buy tickets for the current user ...
    }

    [Authorize("Administrators")]
    public override Task<BuyTicketsResponse> RefundTickets(
        BuyTicketsRequest request, ServerCallContext context)
    {
        // ... refund tickets (something only Administrators can do) ..
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="b0776-149">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b0776-149">Additional resources</span></span>

* [<span data-ttu-id="b0776-150">Autenticación de token de portador en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b0776-150">Bearer Token authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [<span data-ttu-id="b0776-151">Configurar la autenticación de certificados de cliente en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b0776-151">Configure Client Certificate authentication in ASP.NET Core</span></span>](xref:security/authentication/certauth)
