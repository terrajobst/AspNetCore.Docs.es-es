---
title: Consideraciones de seguridad en ASP.NET Core SignalR
author: bradygaster
description: Obtenga información sobre cómo utilizar la autenticación y autorización en ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/security
ms.openlocfilehash: 52cfac6be8e61572acdf0b19dab574b607314d97
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836069"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a>Consideraciones de seguridad en ASP.NET Core SignalR

Por [Andrew Stanton-Nurse](https://twitter.com/anurse)

En este artículo se proporciona información sobre la protección de SignalR.

## <a name="cross-origin-resource-sharing"></a>Uso compartido de recursos entre orígenes

[Los recursos entre orígenes (CORS) de uso compartido](https://www.w3.org/TR/cors/) puede utilizarse para permitir que las conexiones de origen cruzado SignalR en el explorador. Si el código de JavaScript está hospedado en un dominio distinto de la aplicación de SignalR, [middleware CORS](xref:security/cors) debe habilitarse para permitir que el código JavaScript para conectarse a la aplicación de SignalR. Permitir solicitudes entre orígenes solo desde el control o dominios que confía. Por ejemplo:

* El sitio se hospeda en `http://www.example.com`
* La aplicación de SignalR se hospeda en `http://signalr.example.com`

Debe configurarse la CORS en la aplicación de SignalR para permitir únicamente el origen `www.example.com`.

Para obtener más información sobre cómo configurar la CORS, vea [solicitudes habilitar de origen cruzado (CORS)](xref:security/cors). SignalR **requiere** las siguientes directivas CORS:

* Permitir los orígenes específicos esperados. Permitir cualquier origen, es posible pero es **no** seguro o recomendada.
* Métodos HTTP `GET` y `POST` debe estar permitido.
* Deben habilitarse las credenciales, incluso cuando no se usa la autenticación.

Por ejemplo, la siguiente directiva CORS permite hospedado en un cliente del explorador SignalR `https://example.com` para tener acceso a la aplicación de SignalR hospedada en `https://signalr.example.com`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

> [!NOTE]
> SignalR no es compatible con la característica CORS integrada en Azure App Service.

## <a name="websocket-origin-restriction"></a>Restricción de origen de WebSocket

::: moniker range=">= aspnetcore-2.2"

Las protecciones proporcionadas por CORS no se aplican a WebSockets. Para la restricción de origen de WebSockets, lea [restricción de origen de WebSockets](xref:fundamentals/websockets#websocket-origin-restriction).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Las protecciones proporcionadas por CORS no se aplican a WebSockets. Los exploradores **no** hacen lo siguiente:

* Efectúan solicitudes preparatorias CORS.
* Respetan las restricciones especificadas en los encabezados `Access-Control` al efectuar solicitudes de WebSocket.

En cambio, sí que envían el encabezado `Origin` al emitir solicitudes de WebSocket. Las aplicaciones deben configurarse para validar estos encabezados a fin de garantizar que solo se permitan los WebSockets procedentes de los orígenes esperados.

En ASP.NET Core 2.1 y versiones posteriores, la validación del encabezado puede lograrse mediante un middleware personalizado colocarlo **antes `UseSignalR`y el middleware de autenticación** en `Configure`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> El encabezado `Origin` está controlado por el cliente y, al igual que el encabezado `Referer`, se puede falsificar. Estos encabezados deben **no** utilizarse como mecanismo de autenticación.

::: moniker-end

## <a name="access-token-logging"></a>Registro de token de acceso

Al usar WebSockets o los eventos, el explorador del cliente envía el token de acceso en la cadena de consulta. Recibir el token de acceso a través de la cadena de consulta es normalmente tan seguro como usar el estándar `Authorization` encabezado. Siempre debe usar HTTPS para garantizar una conexión segura de extremo a otro entre el cliente y el servidor. La dirección URL para cada solicitud, incluida la cadena de consulta de registro de muchos servidores web. Las direcciones URL de registro, es posible que registre el token de acceso. ASP.NET Core, se registra la dirección URL para cada solicitud de forma predeterminada, que incluirá la cadena de consulta. Por ejemplo:

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

Si le preocupa sobre el registro de estos datos con los registros de servidor, puede deshabilitar este registro mediante la configuración de la `Microsoft.AspNetCore.Hosting` registrador para el `Warning` nivel o superior (estos mensajes se escriben en `Info` nivel). Consulte la documentación sobre [filtrado del registro](xref:fundamentals/logging/index#log-filtering) para obtener más información. Si aún desea cierta información de solicitud de registro, puede [escribir un middleware](xref:fundamentals/middleware/index#write-middleware) para registrar los datos que necesita y filtra el `access_token` el valor de cadena de consulta (si existe).

## <a name="exceptions"></a>Excepciones

Los mensajes de excepción normalmente se consideran información confidencial que no debe mostrarse a un cliente. De forma predeterminada, SignalR no envía los detalles de una excepción producida por un método de concentrador al cliente. En su lugar, el cliente recibe un mensaje genérico que indica que un error. Entrega de mensajes de excepción al cliente se puede invalidar (por ejemplo, en el desarrollo o pruebas) con [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options). Los mensajes de excepción no se deben exponer al cliente en aplicaciones de producción.

## <a name="buffer-management"></a>Administración de búfer

SignalR usa búferes por conexión para administrar los mensajes entrantes y salientes. De forma predeterminada, SignalR limita estos búferes a 32 KB. El mensaje más grande que puede enviar un cliente o servidor es 32 KB. La memoria máxima utilizada por una conexión para los mensajes es 32 KB. Si los mensajes siempre son menores que 32 KB, puede reducir el límite, que:

* Impide que un cliente puede enviar un mensaje mayor.
* El servidor nunca tendrá que asignar los búferes grandes para aceptar mensajes.

Si los mensajes son superiores a 32 KB, puede aumentar el límite. Aumentar este límite significa:

* El cliente puede hacer que el servidor asignar búferes de memoria de gran tamaño.
* Asignación de servidor de los búferes grandes puede reducir el número de conexiones simultáneas.

Hay límites para los mensajes entrantes y salientes, ambos se pueden configurar en el [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) configurado en el objeto `MapHub`:

* `ApplicationMaxBufferSize` representa el número máximo de bytes desde el cliente que los búferes del servidor. Si el cliente intenta enviar un mensaje supere ese límite, se puede cerrar la conexión.
* `TransportMaxBufferSize` representa el número máximo de bytes que el servidor puede enviar. Si el servidor intenta enviar un mensaje (incluidos los valores devueltos de métodos de concentrador) supere ese límite, se producirá una excepción.

Establecer el límite en `0` desactivar el límite. Quitar el límite permite que un cliente enviar un mensaje de cualquier tamaño. Los clientes malintencionados enviar mensajes de gran tamaño pueden producir un exceso de memoria que se va a asignar. Uso de memoria excesivo puede reducir significativamente el número de conexiones simultáneas.
