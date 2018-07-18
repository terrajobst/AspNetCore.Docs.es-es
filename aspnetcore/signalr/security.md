---
title: Consideraciones de seguridad en ASP.NET Core SignalR
author: tdykstra
description: Obtenga información sobre cómo utilizar la autenticación y autorización en ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/security
ms.openlocfilehash: b66c7fbfbaee4c70a68f3132875fbc81018c3e20
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095137"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a>Consideraciones de seguridad en ASP.NET Core SignalR

Por [Andrew Stanton-Nurse](https://twitter.com/anurse)

## <a name="overview"></a>Información general

SignalR proporciona una serie de protecciones de seguridad de forma predeterminada. Es importante entender cómo configurar estas protecciones.

### <a name="cross-origin-resource-sharing"></a>Uso compartido de recursos entre orígenes

[Los recursos entre orígenes (CORS) de uso compartido](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) puede utilizarse para permitir que las conexiones de origen cruzado SignalR en el explorador. Si el código de JavaScript está hospedado en otro nombre de dominio de la aplicación de SignalR, tendrá que habilitar el [middleware de ASP.NET Core CORS](xref:security/cors) con el fin de permitir la conexión. En general, permitir solicitudes entre orígenes solo desde los dominios que controla. Por ejemplo, si su sitio está hospedado en `http://www.example.com` y la aplicación de SignalR se hospeda en `http://signalr.example.com`, debe configurar CORS en la aplicación de SignalR para permitir únicamente el origen `www.example.com`.

Para obtener más información sobre cómo configurar la CORS, vea [la documentación sobre ASP.NET Core CORS](xref:security/cors). SignalR requiere las siguientes directivas CORS para poder funcionar correctamente:

* La directiva debe permitir los orígenes específicos previsto o permitir que cualquier origen (no recomendado).
* Métodos HTTP `GET` y `POST` debe estar permitido.
* Deben habilitarse las credenciales, incluso cuando no está usando la autenticación.

Por ejemplo, la siguiente directiva CORS permite hospedado en un cliente del explorador SignalR `http://example.com` para tener acceso a la aplicación de SignalR:

```csharp
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder => {
        builder.WithOrigins("http://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> SignalR no es compatible con la característica CORS integrada en Azure App Service.

### <a name="access-token-logging"></a>Registro de token de acceso

Al usar WebSockets o los eventos, el explorador del cliente envía el token de acceso en la cadena de consulta. Esto es generalmente tan seguro como usar el estándar `Authorization` encabezado, sin embargo, la dirección URL para cada solicitud de registro de muchos servidores web, incluida la cadena de consulta. Esto significa que el token de acceso que puede incluirse en los registros. Considere la posibilidad de revisar la configuración de registro del servidor web para evitar esta información de registro.

### <a name="exceptions"></a>Excepciones

Los mensajes de excepción normalmente se consideran información confidencial que no debe mostrarse a un cliente. De forma predeterminada, SignalR no envía los detalles de una excepción producida por un método de concentrador al cliente. En su lugar, el cliente recibe un mensaje genérico que indica que un error. Puede invalidar este comportamiento estableciendo el [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options) configuración.

### <a name="buffer-management"></a>Administración de búfer

SignalR usa búferes por conexión con el fin de administrar los mensajes entrantes y salientes. De forma predeterminada, SignalR limita estos búferes a 32KB. Esto significa que el mensaje posible más grande que puede enviar un cliente o servidor es 32KB. Esto también significa que la cantidad máxima de memoria consumida por una conexión para los mensajes es 32KB. Si conoce que los mensajes siempre son menores que este límite, puede reducir este tamaño para impedir que un cliente puede enviar un mensaje mayor y forzar al servidor para asignar memoria para aceptarla. De forma similar, si sabe que los mensajes son mayores que este límite, se puede aumentar. Sin embargo, tenga en cuenta que al aumentar este límite significa que el cliente es capaz de hacer que el servidor asignar memoria adicional y puede reducir el número de conexiones simultáneas que puede controlar la aplicación.

Hay límites independientes para mensajes entrantes y salientes, ambos se pueden configurar en el [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) configurado en el objeto `MapHub`:

* `ApplicationMaxBufferSize` representa el número máximo de bytes desde el cliente que los búferes del servidor. Si el cliente intenta enviar un mensaje supere ese límite, se puede cerrar la conexión.
* `TransportMaxBufferSize` representa el número máximo de bytes que el servidor puede enviar. Si el servidor intenta enviar un mensaje (incluye los valores devueltos de métodos de concentrador) supera este límite, se producirá una excepción.

Establecer el límite en `0` deshabilita totalmente el límite. Sin embargo, esto debe hacerse con sumo cuidado. Quitar el límite permite que un cliente enviar un mensaje de cualquier tamaño. Esto se puede utilizar un cliente malintencionado para provocar un exceso de memoria que se asignen, lo que podría disminuir considerablemente el número de conexiones simultáneas que puede admitir la aplicación.
