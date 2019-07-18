---
title: Autenticación y autorización en gRPC para ASP.NET Core
author: jamesnk
description: Obtenga información sobre cómo usar la autenticación y la autorización en gRPC para ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 06/07/2019
uid: grpc/authn-and-authz
ms.openlocfilehash: 49024295e4db7976924397bb24567d92d6298562
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308778"
---
# <a name="authentication-and-authorization-in-grpc-for-aspnet-core"></a><span data-ttu-id="58c01-103">Autenticación y autorización en gRPC para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="58c01-103">Authentication and authorization in gRPC for ASP.NET Core</span></span>

<span data-ttu-id="58c01-104">Por [James Newton-King](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="58c01-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="58c01-105">[Ver o descargar el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(Cómo descargar)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="58c01-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-calling-a-grpc-service"></a><span data-ttu-id="58c01-106">Autenticar a los usuarios que llaman a un servicio gRPC</span><span class="sxs-lookup"><span data-stu-id="58c01-106">Authenticate users calling a gRPC service</span></span>

<span data-ttu-id="58c01-107">gRPC se puede usar con la [autenticación ASP.net Core](xref:security/authentication/identity) para asociar un usuario a cada llamada.</span><span class="sxs-lookup"><span data-stu-id="58c01-107">gRPC can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each call.</span></span>

<span data-ttu-id="58c01-108">En el siguiente ejemplo `Startup.Configure` se usa gRPC y la autenticación ASP.net Core:</span><span class="sxs-lookup"><span data-stu-id="58c01-108">The following is an example of `Startup.Configure` which uses gRPC and ASP.NET Core authentication:</span></span>

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
> <span data-ttu-id="58c01-109">El orden en el que se registra el middleware de autenticación ASP.NET Core es importante.</span><span class="sxs-lookup"><span data-stu-id="58c01-109">The order in which you register the ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="58c01-110">Llame `UseAuthentication` siempre a `UseAuthorization` y `UseRouting` después de `UseEndpoints`y antes de.</span><span class="sxs-lookup"><span data-stu-id="58c01-110">Always call `UseAuthentication` and `UseAuthorization` after `UseRouting` and before `UseEndpoints`.</span></span>

<span data-ttu-id="58c01-111">Una vez que se ha configurado la autenticación, se puede tener acceso al usuario en un método de `ServerCallContext`servicio gRPC a través de.</span><span class="sxs-lookup"><span data-stu-id="58c01-111">Once authentication has been setup, the user can be accessed in a gRPC service methods via the `ServerCallContext`.</span></span>

```csharp
public override Task<BuyTicketsResponse> BuyTickets(
    BuyTicketsRequest request, ServerCallContext context)
{
    var user = context.GetHttpContext().User;

    // ... access data from ClaimsPrincipal ...
}

```

### <a name="bearer-token-authentication"></a><span data-ttu-id="58c01-112">Autenticación de token de portador</span><span class="sxs-lookup"><span data-stu-id="58c01-112">Bearer token authentication</span></span>

<span data-ttu-id="58c01-113">El cliente puede proporcionar un token de acceso para la autenticación.</span><span class="sxs-lookup"><span data-stu-id="58c01-113">The client can provide an access token for authentication.</span></span> <span data-ttu-id="58c01-114">El servidor valida el token y lo usa para identificar al usuario.</span><span class="sxs-lookup"><span data-stu-id="58c01-114">The server validates the token and uses it to identify the user.</span></span>

<span data-ttu-id="58c01-115">En el servidor, la autenticación de token de portador se configura mediante el [middleware portador de JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="58c01-115">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="58c01-116">En el cliente gRPC de .NET, el token se puede enviar con llamadas como encabezado:</span><span class="sxs-lookup"><span data-stu-id="58c01-116">In the .NET gRPC client, the token can be sent with calls as a header:</span></span>

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

### <a name="client-certificate-authentication"></a><span data-ttu-id="58c01-117">Autenticación de certificado de cliente</span><span class="sxs-lookup"><span data-stu-id="58c01-117">Client certificate authentication</span></span>

<span data-ttu-id="58c01-118">Un cliente podría proporcionar alternativamente un certificado de cliente para la autenticación.</span><span class="sxs-lookup"><span data-stu-id="58c01-118">A client could alternatively provide a client certificate for authentication.</span></span> <span data-ttu-id="58c01-119">La [autenticación de certificados](https://tools.ietf.org/html/rfc5246#section-7.4.4) se produce en el nivel de TLS, mucho antes de que llegue a ASP.net Core.</span><span class="sxs-lookup"><span data-stu-id="58c01-119">[Certificate authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="58c01-120">Cuando la solicitud entra en ASP.NET Core, el [paquete de autenticación de certificados de cliente](xref:security/authentication/certauth) permite resolver el certificado `ClaimsPrincipal`en un.</span><span class="sxs-lookup"><span data-stu-id="58c01-120">When the request enters ASP.NET Core, the [client certificate authentication package](xref:security/authentication/certauth) allows you to resolve the certificate to a `ClaimsPrincipal`.</span></span>

> [!NOTE]
> <span data-ttu-id="58c01-121">El host debe estar configurado para aceptar certificados de cliente.</span><span class="sxs-lookup"><span data-stu-id="58c01-121">The host needs to be configured to accept client certificates.</span></span> <span data-ttu-id="58c01-122">Consulte [configuración del host para requerir certificados](xref:security/authentication/certauth#configure-your-host-to-require-certificates) para obtener información sobre la aceptación de certificados de cliente en KESTREL, IIS y Azure.</span><span class="sxs-lookup"><span data-stu-id="58c01-122">See [configure your host to require certificates](xref:security/authentication/certauth#configure-your-host-to-require-certificates) for information on accepting client certificates in Kestrel, IIS and Azure.</span></span>

<span data-ttu-id="58c01-123">En el cliente gRPC de .net, se agrega el certificado de `HttpClientHandler` cliente a que se usa para crear el cliente de gRPC:</span><span class="sxs-lookup"><span data-stu-id="58c01-123">In the .NET gRPC client, the client certificate is added to `HttpClientHandler` that is then used to create the gRPC client:</span></span>

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

### <a name="other-authentication-mechanisms"></a><span data-ttu-id="58c01-124">Otros mecanismos de autenticación</span><span class="sxs-lookup"><span data-stu-id="58c01-124">Other authentication mechanisms</span></span>

<span data-ttu-id="58c01-125">Además del token de portador y la autenticación de certificado de cliente, todos los ASP.NET Core mecanismos de autenticación admitidos como OAuth, OpenID y Negotiate deben funcionar con gRPC.</span><span class="sxs-lookup"><span data-stu-id="58c01-125">In addition to bearer token and client certificate authentication, all ASP.NET Core supported authentication mechanisms such as OAuth, OpenID and Negotiate should work with gRPC.</span></span> <span data-ttu-id="58c01-126">Visite [autenticación de ASP.net Core](xref:security/authentication/identity) para obtener más información sobre la configuración de la autenticación en el lado del servidor.</span><span class="sxs-lookup"><span data-stu-id="58c01-126">Visit [ASP.NET Core authentication](xref:security/authentication/identity) for more information for configuring authentication on the server side.</span></span>

<span data-ttu-id="58c01-127">La configuración del lado cliente dependerá del mecanismo de autenticación que se use.</span><span class="sxs-lookup"><span data-stu-id="58c01-127">Client side configuration will depend on the authentication mechanism you are using.</span></span> <span data-ttu-id="58c01-128">En los ejemplos de token de portador anterior y autenticación de certificados de cliente se muestran un par de formas en que se puede configurar el cliente de gRPC para enviar metadatos de autenticación con llamadas gRPC:</span><span class="sxs-lookup"><span data-stu-id="58c01-128">The previous bearer token and client certificate authentication examples show a couple of ways the gRPC client can be configured to send authentication metadata with gRPC calls:</span></span>

* <span data-ttu-id="58c01-129">Los clientes de gRPC fuertemente tipados usan `HttpClient` internamente.</span><span class="sxs-lookup"><span data-stu-id="58c01-129">Strongly typed gRPC clients use `HttpClient` internally.</span></span> <span data-ttu-id="58c01-130">Se puede configurar la [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler) `HttpClient`autenticación en o agregando instancias [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) personalizadas a.</span><span class="sxs-lookup"><span data-stu-id="58c01-130">Authentication can be configured on [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler), or by adding custom [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) instances to the `HttpClient`.</span></span>
* <span data-ttu-id="58c01-131">Cada llamada a gRPC tiene un `CallOptions` argumento opcional.</span><span class="sxs-lookup"><span data-stu-id="58c01-131">Each gRPC call has an optional `CallOptions` argument.</span></span> <span data-ttu-id="58c01-132">Los encabezados personalizados se pueden enviar mediante la colección de encabezados de la opción.</span><span class="sxs-lookup"><span data-stu-id="58c01-132">Custom headers can be sent using the option's headers collection.</span></span>

## <a name="authorize-users-to-access-services-and-service-methods"></a><span data-ttu-id="58c01-133">Autorizar a los usuarios para obtener acceso a servicios y métodos de servicio</span><span class="sxs-lookup"><span data-stu-id="58c01-133">Authorize users to access services and service methods</span></span>

<span data-ttu-id="58c01-134">De forma predeterminada, los usuarios no autenticados pueden llamar a todos los métodos de un servicio.</span><span class="sxs-lookup"><span data-stu-id="58c01-134">By default, all methods in a service can be called by unauthenticated users.</span></span> <span data-ttu-id="58c01-135">Para requerir autenticación, aplique el atributo [[Authorize]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) al servicio:</span><span class="sxs-lookup"><span data-stu-id="58c01-135">To require authentication, apply the [[Authorize]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute to the service:</span></span>

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="58c01-136">Puede usar los argumentos de constructor y las propiedades del `[Authorize]` atributo para restringir el acceso solo a los usuarios que coincidan con [las directivas de autorización](xref:security/authorization/policies)específicas.</span><span class="sxs-lookup"><span data-stu-id="58c01-136">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="58c01-137">Por ejemplo, si tiene una directiva de autorización personalizada denominada `MyAuthorizationPolicy`, asegúrese de que solo los usuarios que coincidan con esa Directiva puedan tener acceso al servicio mediante el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="58c01-137">For example, if you have a custom authorization policy called `MyAuthorizationPolicy`, ensure that only users matching that policy can access the service using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="58c01-138">Los métodos de servicio individuales también `[Authorize]` pueden tener el atributo aplicado.</span><span class="sxs-lookup"><span data-stu-id="58c01-138">Individual service methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="58c01-139">Si el usuario actual no coincide con las directivas que se aplican **al método** y a la clase, se devuelve un error al autor de la llamada:</span><span class="sxs-lookup"><span data-stu-id="58c01-139">If the current user doesn't match the policies applied to **both** the method and the class, an error is returned to the caller:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="58c01-140">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="58c01-140">Additional resources</span></span>

* [<span data-ttu-id="58c01-141">Autenticación de token de portador en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="58c01-141">Bearer Token authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [<span data-ttu-id="58c01-142">Configurar la autenticación de certificados de cliente en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="58c01-142">Configure Client Certificate authentication in ASP.NET Core</span></span>](xref:security/authentication/certauth)
