---
title: Autenticación y autorización en gRPC para ASP.NET Core
author: jamesnk
description: Obtenga información sobre cómo usar la autenticación y la autorización en gRPC para ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/13/2019
uid: grpc/authn-and-authz
ms.openlocfilehash: 19018c4ffae1228055a4858b496f135d015625b4
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "68993283"
---
# <a name="authentication-and-authorization-in-grpc-for-aspnet-core"></a>Autenticación y autorización en gRPC para ASP.NET Core

Por [James Newton-King](https://twitter.com/jamesnk)

[Ver o descargar el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(Cómo descargar)](xref:index#how-to-download-a-sample)

## <a name="authenticate-users-calling-a-grpc-service"></a>Autenticar a los usuarios que llaman a un servicio gRPC

gRPC se puede usar con la [autenticación ASP.net Core](xref:security/authentication/identity) para asociar un usuario a cada llamada.

En el siguiente ejemplo `Startup.Configure` se usa gRPC y la autenticación ASP.net Core:

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
> El orden en el que se registra el middleware de autenticación ASP.NET Core es importante. Llame `UseAuthentication` siempre a `UseAuthorization` y `UseRouting` después de `UseEndpoints`y antes de.

El mecanismo de autenticación que usa la aplicación durante una llamada debe configurarse. La configuración de autenticación se `Startup.ConfigureServices` agrega en y será diferente en función del mecanismo de autenticación que use la aplicación. Para ver ejemplos de cómo proteger ASP.NET Core aplicaciones, consulte [ejemplos de autenticación](xref:security/authentication/samples).

Una vez que se ha configurado la autenticación, se puede tener acceso al usuario en un método de `ServerCallContext`servicio gRPC a través de.

```csharp
public override Task<BuyTicketsResponse> BuyTickets(
    BuyTicketsRequest request, ServerCallContext context)
{
    var user = context.GetHttpContext().User;

    // ... access data from ClaimsPrincipal ...
}

```

### <a name="bearer-token-authentication"></a>Autenticación de token de portador

El cliente puede proporcionar un token de acceso para la autenticación. El servidor valida el token y lo usa para identificar al usuario.

En el servidor, la autenticación de token de portador se configura mediante el [middleware portador de JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).

En el cliente gRPC de .NET, el token se puede enviar con llamadas como encabezado:

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

### <a name="client-certificate-authentication"></a>Autenticación de certificado de cliente

Un cliente podría proporcionar alternativamente un certificado de cliente para la autenticación. La [autenticación de certificados](https://tools.ietf.org/html/rfc5246#section-7.4.4) se produce en el nivel de TLS, mucho antes de que llegue a ASP.net Core. Cuando la solicitud entra en ASP.NET Core, el [paquete de autenticación de certificados de cliente](xref:security/authentication/certauth) permite resolver el certificado `ClaimsPrincipal`en un.

> [!NOTE]
> El host debe estar configurado para aceptar certificados de cliente. Consulte [configuración del host para requerir certificados](xref:security/authentication/certauth#configure-your-host-to-require-certificates) para obtener información sobre la aceptación de certificados de cliente en KESTREL, IIS y Azure.

En el cliente gRPC de .net, se agrega el certificado de `HttpClientHandler` cliente a que se usa para crear el cliente de gRPC:

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

### <a name="other-authentication-mechanisms"></a>Otros mecanismos de autenticación

Muchos ASP.NET Core mecanismos de autenticación admitidos funcionan con gRPC:

* Azure Active Directory
* Certificado de cliente
* IdentityServer
* Token de JWT
* OAuth 2,0
* OpenID Connect
* WS-Federation

Para obtener más información sobre la configuración de la autenticación en el servidor, consulte [ASP.net Core la autenticación](xref:security/authentication/identity).

La configuración del cliente de gRPC para usar la autenticación dependerá del mecanismo de autenticación que se use. El token de portador anterior y los ejemplos de certificados de cliente muestran un par de formas en que se puede configurar el cliente de gRPC para enviar metadatos de autenticación con llamadas gRPC:

* Los clientes de gRPC fuertemente tipados usan `HttpClient` internamente. Se puede configurar la [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler) `HttpClient`autenticación en o agregando instancias [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) personalizadas a.
* Cada llamada a gRPC tiene un `CallOptions` argumento opcional. Los encabezados personalizados se pueden enviar mediante la colección de encabezados de la opción.

> [!NOTE]
> La autenticación de Windows (NTLM/Kerberos/Negotiate) no se puede usar con gRPC. gRPC requiere HTTP/2 y HTTP/2 no admite la autenticación de Windows.

## <a name="authorize-users-to-access-services-and-service-methods"></a>Autorizar a los usuarios para obtener acceso a servicios y métodos de servicio

De forma predeterminada, los usuarios no autenticados pueden llamar a todos los métodos de un servicio. Para requerir autenticación, aplique el atributo [[Authorize]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) al servicio:

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
}
```

Puede usar los argumentos de constructor y las propiedades del `[Authorize]` atributo para restringir el acceso solo a los usuarios que coincidan con [las directivas de autorización](xref:security/authorization/policies)específicas. Por ejemplo, si tiene una directiva de autorización personalizada denominada `MyAuthorizationPolicy`, asegúrese de que solo los usuarios que coincidan con esa Directiva puedan tener acceso al servicio mediante el código siguiente:

```csharp
[Authorize("MyAuthorizationPolicy")]
public class TicketerService : Ticketer.TicketerBase
{
}
```

Los métodos de servicio individuales también `[Authorize]` pueden tener el atributo aplicado. Si el usuario actual no coincide con las directivas que se aplican al método y a la clase, se devuelve un error al autor de la llamada:

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

## <a name="additional-resources"></a>Recursos adicionales

* [Autenticación de token de portador en ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [Configurar la autenticación de certificados de cliente en ASP.NET Core](xref:security/authentication/certauth)
