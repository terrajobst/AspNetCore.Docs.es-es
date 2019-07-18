---
title: Consideraciones de seguridad en gRPC para ASP.NET Core
author: jamesnk
description: Obtenga información sobre las consideraciones de seguridad para gRPC para ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 07/07/2019
uid: grpc/security
ms.openlocfilehash: 4a70cb16d8397dbc69a626435fb72a0512788b14
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308758"
---
# <a name="security-considerations-in-grpc-for-aspnet-core"></a>Consideraciones de seguridad en gRPC para ASP.NET Core

Por [James Newton-King](https://twitter.com/jamesnk)

En este artículo se proporciona información sobre cómo proteger gRPC con .NET Core.

## <a name="transport-security"></a>Seguridad de transporte

Los mensajes gRPC se envían y se reciben mediante HTTP/2. Se recomienda:

* La seguridad de la [capa de transporte (TLS)](https://tools.ietf.org/html/rfc5246) se usa para proteger los mensajes en aplicaciones gRPC de producción.
* gRPC Services solo debe escuchar y responder a través de puertos seguros.

TLS está configurado en Kestrel. Para obtener más información sobre la configuración de puntos de conexión de Kestrel, consulte [configuración del punto de conexión de Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).

## <a name="exceptions"></a>Excepciones

Los mensajes de excepción generalmente se consideran datos confidenciales que no se deben revelar a un cliente. De forma predeterminada, gRPC no envía los detalles de una excepción producida por un servicio de gRPC al cliente. En su lugar, el cliente recibe un mensaje genérico que indica que se ha producido un error. La entrega de mensajes de excepción al cliente se puede invalidar (por ejemplo, en desarrollo o prueba) con [EnableDetailedErrors](xref:grpc/configuration#configure-services-options). Los mensajes de excepción no se deben exponer al cliente en las aplicaciones de producción.

## <a name="message-size-limits"></a>Límites de tamaño de mensaje

Los mensajes entrantes a los clientes y servicios de gRPC se cargan en la memoria. Los límites de tamaño de los mensajes son un mecanismo que ayuda a evitar que gRPC consuma demasiados recursos.

gRPC usa los límites de tamaño por mensaje para administrar los mensajes entrantes y salientes. De forma predeterminada, gRPC limita los mensajes entrantes a 4 MB. No hay ningún límite en los mensajes salientes.

En el servidor, los límites de mensajes gRPC se pueden configurar para todos los servicios de `AddGrpc` una aplicación con:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.ReceiveMaxMessageSize = 1 * 1024 * 1024;  // 1 megabyte
        options.SendMaxMessageSize = 1 * 1024 * 1024;     // 1 megabyte
    });
}
```

Los límites también se pueden configurar para un servicio individual `AddServiceOptions<TService>` mediante. Para obtener más información sobre cómo configurar los límites de tamaño de los mensajes, vea [configuración de gRPC](xref:grpc/configuration).

## <a name="client-certificate-validation"></a>Validación de certificados de cliente

Los [certificados de cliente](https://tools.ietf.org/html/rfc5246#section-7.4.4) se validan inicialmente cuando se establece la conexión. De forma predeterminada, Kestrel no realiza ninguna validación adicional del certificado de cliente de una conexión.

Se recomienda que los servicios de gRPC protegidos por certificados de cliente usen el paquete [Microsoft. AspNetCore. Authentication. Certificate](xref:security/authentication/certauth) . La autenticación de ASP.NET Core certificación realizará una validación adicional en un certificado de cliente, lo que incluye:

* El certificado tiene un uso mejorado de clave (EKU) válido
* Está dentro de su período de validez
* Comprobar la revocación de certificados
