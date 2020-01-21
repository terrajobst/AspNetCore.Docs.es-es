---
title: Consideraciones de seguridad en ASP.NET Core SignalR
author: bradygaster
description: Aprenda a usar la autenticación y autorización en ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- SignalR
uid: signalr/security
ms.openlocfilehash: 4b27d9abb36938ed8161ff0d3535204e3fa68765
ms.sourcegitcommit: f259889044d1fc0f0c7e3882df0008157ced4915
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294702"
---
# <a name="security-considerations-in-aspnet-core-opno-locsignalr"></a>Consideraciones de seguridad en ASP.NET Core SignalR

Por [Andrew Stanton-enfermera](https://twitter.com/anurse)

En este artículo se proporciona información sobre cómo proteger SignalR.

## <a name="cross-origin-resource-sharing"></a>Uso compartido de recursos entre orígenes

[El uso compartido de recursos entre orígenes (CORS)](https://www.w3.org/TR/cors/) se puede usar para permitir conexiones SignalR entre orígenes en el explorador. Si el código de JavaScript está hospedado en un dominio diferente del SignalR aplicación, el [middleware de CORS](xref:security/cors) debe estar habilitado para permitir que JavaScript se conecte a la aplicación SignalR. Permita solicitudes entre orígenes únicamente desde dominios en los que confíe o controle. Por ejemplo:

* El sitio se hospeda en `http://www.example.com`
* La aplicación SignalR se hospeda en `http://signalr.example.com`

CORS debe configurarse en la aplicación SignalR para permitir solo el origen `www.example.com`.

Para obtener más información sobre la configuración de CORS, consulte [habilitación de solicitudes entre orígenes (CORS)](xref:security/cors). SignalR **requiere** las siguientes directivas de CORS:

* Permite los orígenes esperados específicos. Permitir cualquier origen es posible, pero **no** es seguro ni recomendado.
* Se deben permitir los métodos HTTP `GET` y `POST`.
* Se deben permitir las credenciales para que las sesiones permanentes basadas en cookies funcionen correctamente. Deben habilitarse incluso cuando no se utiliza la autenticación.

<!--
::: moniker range=">= aspnetcore-5.0"  // Moniker here just to make sure this doesn't get missed in the 5.0 version update.
However, in 5.0 we have provided an option in the TypeScript client to not use credentials.
The not to use credentials option should only be used when you know 100% that credentials like Cookies are not needed in your app (cookies are used by azure app service when using multiple servers)

For more info, see https://github.com/aspnet/AspNetCore.Docs/issues/16003
.-->

Por ejemplo, la siguiente directiva de CORS permite a un cliente de explorador de SignalR hospedado en `https://example.com` acceder a la aplicación de SignalR hospedada en `https://signalr.example.com`:

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
> SignalR no es compatible con la característica de CORS integrada en Azure App Service.

## <a name="websocket-origin-restriction"></a>Restricción de origen de WebSocket

::: moniker range=">= aspnetcore-2.2"

Las protecciones proporcionadas por CORS no se aplican a WebSockets. En el caso de la restricción de origen en WebSockets, lea [restricción de origen de WebSockets](xref:fundamentals/websockets#websocket-origin-restriction).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Las protecciones proporcionadas por CORS no se aplican a WebSockets. Los exploradores **no** hacen lo siguiente:

* Efectúan solicitudes preparatorias CORS.
* Respetan las restricciones especificadas en los encabezados `Access-Control` al efectuar solicitudes de WebSocket.

En cambio, sí que envían el encabezado `Origin` al emitir solicitudes de WebSocket. Las aplicaciones deben configurarse para validar estos encabezados a fin de garantizar que solo se permitan los WebSockets procedentes de los orígenes esperados.

En ASP.NET Core 2,1 y versiones posteriores, la validación de encabezados se puede lograr mediante un middleware personalizado colocado **antes de `UseSignalR`y el middleware de autenticación** en `Configure`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> El encabezado `Origin` está controlado por el cliente y, al igual que el encabezado `Referer`, se puede falsificar. Estos encabezados **no** deben usarse como mecanismo de autenticación.

::: moniker-end

## <a name="connectionid"></a>ConnectionId

Exponer `ConnectionId` puede provocar la suplantación malintencionada si la versión del servidor o del cliente de SignalR es ASP.NET Core 2,2 o anterior. Si el servidor de SignalR y la versión del cliente son ASP.NET Core 3,0 o posterior, el `ConnectionToken` en lugar del `ConnectionId` debe mantenerse en secreto. El `ConnectionToken` no se expone intencionadamente en ninguna API.  Puede ser difícil asegurarse de que los clientes de SignalR más antiguos no se conecten al servidor, por lo que, incluso si la versión del servidor de SignalR es ASP.NET Core 3,0 o posterior, la `ConnectionId` no debe exponerse.

## <a name="access-token-logging"></a>Registro de tokens de acceso

Al utilizar WebSockets o eventos enviados por el servidor, el cliente del explorador envía el token de acceso en la cadena de consulta. La recepción del token de acceso a través de la cadena de consulta suele ser segura con el encabezado de `Authorization` estándar. Use siempre HTTPS para garantizar una conexión segura de un extremo a otro entre el cliente y el servidor. Muchos servidores web registran la dirección URL para cada solicitud, incluida la cadena de consulta. El registro de las direcciones URL puede registrar el token de acceso. ASP.NET Core registra la dirección URL de cada solicitud de forma predeterminada, que incluirá la cadena de consulta. Por ejemplo:

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

Si tiene dudas sobre el registro de estos datos con los registros de servidor, puede deshabilitar este registro por completo configurando el registrador de `Microsoft.AspNetCore.Hosting` en el nivel de `Warning` o superior (estos mensajes se escriben en `Info` nivel). Para obtener más información, consulte [filtrado de registros](xref:fundamentals/logging/index#log-filtering) para obtener más información. Si todavía desea registrar determinada información de la solicitud, puede [escribir un middleware](xref:fundamentals/middleware/write) para registrar los datos que necesita y filtrar el valor de la cadena de consulta `access_token` (si está presente).

## <a name="exceptions"></a>Excepciones

Los mensajes de excepción generalmente se consideran datos confidenciales que no se deben revelar a un cliente. De forma predeterminada, SignalR no envía al cliente los detalles de una excepción producida por un método de concentrador. En su lugar, el cliente recibe un mensaje genérico que indica que se ha producido un error. La entrega de mensajes de excepción al cliente se puede invalidar (por ejemplo, en desarrollo o prueba) con [EnableDetailedErrors](xref:signalr/configuration#configure-server-options). Los mensajes de excepción no deben exponerse al cliente en las aplicaciones de producción.

## <a name="buffer-management"></a>Administración de búfer

SignalR usa búferes por conexión para administrar los mensajes entrantes y salientes. De forma predeterminada, SignalR limita estos búferes a 32 KB. El mensaje más grande que un cliente o servidor puede enviar es 32 KB. La memoria máxima consumida por una conexión para los mensajes es de 32 KB. Si los mensajes son siempre menores que 32 KB, puede reducir el límite, que:

* Impide que un cliente pueda enviar un mensaje más grande.
* El servidor nunca tendrá que asignar búferes grandes para aceptar mensajes.

Si los mensajes son mayores de 32 KB, puede aumentar el límite. Aumentar este límite significa:

* El cliente puede hacer que el servidor asigne grandes búferes de memoria.
* La asignación de servidores de búferes de gran tamaño puede reducir el número de conexiones simultáneas.

Hay límites para los mensajes entrantes y salientes, ambos se pueden configurar en el objeto [HttpConnectionDispatcherOptions](xref:signalr/configuration#configure-server-options) configurado en `MapHub`:

* `ApplicationMaxBufferSize` representa el número máximo de bytes del cliente que el servidor almacena en búfer. Si el cliente intenta enviar un mensaje mayor que este límite, es posible que se cierre la conexión.
* `TransportMaxBufferSize` representa el número máximo de bytes que el servidor puede enviar. Si el servidor intenta enviar un mensaje (incluidos los valores devueltos de los métodos de concentrador) más grande que este límite, se producirá una excepción.

Al establecer el límite en `0`, se deshabilita el límite. Al quitar el límite, un cliente puede enviar un mensaje de cualquier tamaño. Los clientes malintencionados que envían mensajes grandes pueden provocar que se asigne exceso de memoria. El uso excesivo de memoria puede reducir significativamente el número de conexiones simultáneas.
