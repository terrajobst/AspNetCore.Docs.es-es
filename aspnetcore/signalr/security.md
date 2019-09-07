---
title: Consideraciones de seguridad en ASP.NET Core Signalr
author: bradygaster
description: Aprenda a usar la autenticación y la autorización en ASP.NET Core Signalr.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/security
ms.openlocfilehash: a52db2ff51c55f7299d63aa3c7398f99727e0694
ms.sourcegitcommit: 387cf29f5d5addef2cbc70670a11d612806b36b2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2019
ms.locfileid: "70746556"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a>Consideraciones de seguridad en ASP.NET Core Signalr

Por [Andrew Stanton-enfermera](https://twitter.com/anurse)

En este artículo se proporciona información sobre la protección de Signalr.

## <a name="cross-origin-resource-sharing"></a>Uso compartido de recursos entre orígenes

El [uso compartido de recursos entre orígenes (CORS)](https://www.w3.org/TR/cors/) se puede usar para permitir conexiones de signalr entre orígenes en el explorador. Si el código de JavaScript se hospeda en un dominio diferente de la aplicación Signalr, el [middleware de CORS](xref:security/cors) debe estar habilitado para permitir que JavaScript se conecte a la aplicación signalr. Permita solicitudes entre orígenes únicamente desde dominios en los que confíe o controle. Por ejemplo:

* Su sitio se hospeda en`http://www.example.com`
* La aplicación Signalr se hospeda en`http://signalr.example.com`

CORS debe configurarse en la aplicación Signalr para permitir solo el `www.example.com`origen.

Para obtener más información sobre la configuración de CORS, consulte [habilitación de solicitudes entre orígenes (CORS)](xref:security/cors). Signalr **requiere** las siguientes directivas de CORS:

* Permite los orígenes esperados específicos. Permitir cualquier origen es posible, pero **no** es seguro ni recomendado.
* Los métodos `GET` http `POST` y deben ser permitidos.
* Las credenciales deben estar habilitadas, incluso cuando no se use la autenticación.

Por ejemplo, la siguiente directiva de CORS permite que un cliente del explorador de `https://example.com` signalr hospedado en acceda a la `https://signalr.example.com`aplicación signalr hospedada en:

::: moniker range=">= aspnetcore-3.0"

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder =>
    {
        builder.WithOrigins("https://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<ChatHub>("/chatHub");
    });

    // ... other middleware ...
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

::: moniker-end

> [!NOTE]
> Signalr no es compatible con la característica de CORS integrada en Azure App Service.

## <a name="websocket-origin-restriction"></a>Restricción de origen de WebSocket

::: moniker range=">= aspnetcore-2.2"

Las protecciones proporcionadas por CORS no se aplican a WebSockets. En el caso de la restricción de origen en WebSockets, lea [restricción de origen de WebSockets](xref:fundamentals/websockets#websocket-origin-restriction).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Las protecciones proporcionadas por CORS no se aplican a WebSockets. Los exploradores **no** hacen lo siguiente:

* Efectúan solicitudes preparatorias CORS.
* Respetan las restricciones especificadas en los encabezados `Access-Control` al efectuar solicitudes de WebSocket.

En cambio, sí que envían el encabezado `Origin` al emitir solicitudes de WebSocket. Las aplicaciones deben configurarse para validar estos encabezados a fin de garantizar que solo se permitan los WebSockets procedentes de los orígenes esperados.

En ASP.net Core 2,1 y versiones posteriores, la validación de encabezados se puede lograr mediante un middleware personalizado colocado **antes `UseSignalR`y el middleware de autenticación** en: `Configure`

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> El encabezado `Origin` está controlado por el cliente y, al igual que el encabezado `Referer`, se puede falsificar. Estos encabezados **no** deben usarse como mecanismo de autenticación.

::: moniker-end

## <a name="access-token-logging"></a>Registro de tokens de acceso

Al utilizar WebSockets o eventos enviados por el servidor, el cliente del explorador envía el token de acceso en la cadena de consulta. La recepción del token de acceso a través de la cadena de consulta suele ser `Authorization` tan segura como usar el encabezado estándar. Siempre debe usar HTTPS para garantizar una conexión segura de un extremo a otro entre el cliente y el servidor. Muchos servidores web registran la dirección URL para cada solicitud, incluida la cadena de consulta. El registro de las direcciones URL puede registrar el token de acceso. ASP.NET Core registra la dirección URL de cada solicitud de forma predeterminada, que incluirá la cadena de consulta. Por ejemplo:

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

Si tiene dudas sobre el registro de estos datos con los registros del servidor, puede deshabilitar este registro por completo `Microsoft.AspNetCore.Hosting` configurando `Warning` el registrador en el nivel o superior ( `Info` estos mensajes se escriben en el nivel). Consulte la documentación sobre el [filtrado de registros](xref:fundamentals/logging/index#log-filtering) para obtener más información. Si todavía desea registrar determinada información de la solicitud, puede [escribir un middleware](xref:fundamentals/middleware/write) para registrar los datos que necesita y filtrar el valor `access_token` de la cadena de consulta (si está presente).

## <a name="exceptions"></a>Excepciones

Los mensajes de excepción generalmente se consideran datos confidenciales que no se deben revelar a un cliente. De forma predeterminada, Signalr no envía al cliente los detalles de una excepción producida por un método de concentrador. En su lugar, el cliente recibe un mensaje genérico que indica que se ha producido un error. La entrega de mensajes de excepción al cliente se puede invalidar (por ejemplo, en desarrollo o [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options)prueba) con. Los mensajes de excepción no deben exponerse al cliente en las aplicaciones de producción.

## <a name="buffer-management"></a>Administración de búfer

Signalr usa búferes por conexión para administrar los mensajes entrantes y salientes. De forma predeterminada, Signalr limita estos búferes a 32 KB. El mensaje más grande que un cliente o servidor puede enviar es 32 KB. La memoria máxima consumida por una conexión para los mensajes es de 32 KB. Si los mensajes son siempre menores que 32 KB, puede reducir el límite, que:

* Impide que un cliente pueda enviar un mensaje más grande.
* El servidor nunca tendrá que asignar búferes grandes para aceptar mensajes.

Si los mensajes son mayores de 32 KB, puede aumentar el límite. Aumentar este límite significa:

* El cliente puede hacer que el servidor asigne grandes búferes de memoria.
* La asignación de servidores de búferes de gran tamaño puede reducir el número de conexiones simultáneas.

Hay límites para los mensajes entrantes y salientes, ambos se pueden configurar [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) en el objeto `MapHub`configurado en:

* `ApplicationMaxBufferSize`representa el número máximo de bytes del cliente que el servidor almacena en búfer. Si el cliente intenta enviar un mensaje mayor que este límite, es posible que se cierre la conexión.
* `TransportMaxBufferSize`representa el número máximo de bytes que el servidor puede enviar. Si el servidor intenta enviar un mensaje (incluidos los valores devueltos de los métodos de concentrador) más grande que este límite, se producirá una excepción.

Establecer el límite en `0` deshabilita el límite. Al quitar el límite, un cliente puede enviar un mensaje de cualquier tamaño. Los clientes malintencionados que envían mensajes grandes pueden provocar que se asigne exceso de memoria. El uso excesivo de memoria puede reducir significativamente el número de conexiones simultáneas.
