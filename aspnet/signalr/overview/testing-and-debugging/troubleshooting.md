---
uid: signalr/overview/testing-and-debugging/troubleshooting
title: Solución de problemas de SignalR | Microsoft Docs
author: pfletcher
description: En este artículo se describe problemas comunes con el desarrollo de aplicaciones de SignalR.
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 4b559e6c-4fb0-4a04-9812-45cf08ae5779
msc.legacyurl: /signalr/overview/testing-and-debugging/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 9ef69ece2558260f0409e9388751f77ac84a06f1
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523082"
---
<a name="signalr-troubleshooting"></a>Solución de problemas de SignalR
====================
por [Patrick Fletcher](https://github.com/pfletcher)

> Este documento describe los problemas más comunes con SignalR.
> 
> ## <a name="software-versions-used-in-this-topic"></a>Versiones de software que se usa en este tema
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Versión 2 de SignalR
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Versiones anteriores de este tema.
> 
> Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


Este documento contiene las siguientes secciones.

- [Llamar a métodos entre el cliente y servidor en modo silencioso se produce un error](#connection)
- [Configuración de websockets de IIS a ping/pong para detectar a un cliente inactivo](#pong)
- [Otros problemas de conexión](#other)
- [Errores de compilación y del lado servidor](#server)
- [Problemas de Visual Studio](#vs)
- [Problemas de Internet Information Services](#iis)
- [Problemas de Microsoft Azure](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>Llamar a métodos entre el cliente y servidor en modo silencioso se produce un error

Esta sección describen las posibles causas de una llamada de método entre cliente y servidor genere un error sin un mensaje de error descriptivo. En una aplicación de SignalR, el servidor no tiene información sobre los métodos que implementa el cliente; Cuando el servidor invoca un método de cliente, los datos de nombre y los parámetros de método se envían al cliente y el método se ejecuta solo si existe en el formato que el servidor especificado. Si no se encuentra ningún método coincidente en el cliente, no sucede nada, y no se genera ningún mensaje de error en el servidor.

Para investigar más sobre los métodos de cliente no obtener llamado, puede activar el registro antes de llamar al método start en el concentrador para ver qué llamadas proceden del servidor. Para habilitar el registro en una aplicación de JavaScript, consulte [cómo habilitar el registro del lado cliente (versión de cliente de JavaScript)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging). Para habilitar el registro en una aplicación de cliente. NET, consulte [cómo habilitar el registro del lado cliente (versión de cliente de. NET)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>Método mal escrito, firma de método incorrecto o nombre del concentrador incorrecto

Si el nombre o la firma de un método llamado coincide exactamente un método adecuado en el cliente, se producirá un error en la llamada. Compruebe que el nombre del método llamado por el servidor coincide con el nombre del método en el cliente. Además, SignalR crea el proxy de concentrador mediante métodos de mayúsculas y minúsculas camel, según resulte adecuado en JavaScript, por lo que llama un método `SendMessage` en el servidor se llamaría `sendMessage` en el proxy de cliente. Si usas el `HubName` atributo en el código del lado servidor, compruebe que el nombre usado coincide con el nombre usado para crear el centro en el cliente. Si no usa el `HubName` atributo, compruebe que el nombre del centro en un cliente de JavaScript es con grafía camel, como chatHub en lugar de ChatHub.

### <a name="duplicate-method-name-on-client"></a>Nombre del método duplicada en el cliente

Compruebe que no tiene un método duplicado en el cliente que se diferencia sólo por caso. Si la aplicación cliente tiene un método llamado `sendMessage`, asegúrese de que también hay un método llamado `SendMessage` también.

### <a name="missing-json-parser-on-the-client"></a>Analizador JSON que falta en el cliente

SignalR requiere un analizador JSON esté presente para serializar llamadas entre el servidor y el cliente. Si el cliente no tiene un analizador JSON integrado (por ejemplo, Internet Explorer 7), deberá incluir una en la aplicación. Puede descargar el analizador JSON [aquí](http://nuget.org/packages/json2).

### <a name="mixing-hub-and-persistentconnection-syntax"></a>Mezclar la sintaxis de concentrador y PersistentConnection

SignalR usa dos modelos de comunicación: concentradores y PersistentConnections. La sintaxis para llamar a estos modelos de dos comunicación es diferente en el código de cliente. Si ha agregado un concentrador en el código del servidor, compruebe que todo el código de cliente utiliza la sintaxis correcta del centro.

**Código de cliente de JavaScript que crea un PersistentConnection en un cliente de JavaScript**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**Código de cliente de JavaScript que crea a un Proxy de concentrador en un cliente de Javascript**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**Código de servidor de C# que se asigna una ruta a un PersistentConnection**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**Código de servidor de C# que se asigna una ruta a un concentrador o a múltiples concentradores si tiene varias aplicaciones**

[!code-css[Main](troubleshooting/samples/sample4.css)]

### <a name="connection-started-before-subscriptions-are-added"></a>Conexión que se inicia antes de que las suscripciones se agregan

Si la conexión con el concentrador se inicia antes de que los métodos que pueden llamarse desde el servidor se agregan al proxy, no se recibirán los mensajes. El siguiente código JavaScript no iniciará correctamente el centro:

**Código de cliente JavaScript incorrecto que no le permitirá recibir los mensajes de concentradores**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

En su lugar, agregue las suscripciones del método antes de llamar al inicio:

**Código de cliente de JavaScript que se agrega correctamente las suscripciones a un concentrador**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>Falta el nombre de método en el proxy de concentrador

Compruebe que el método definido en el servidor se ha suscrito en el cliente. Aunque el servidor define el método, todavía debe agregarse al proxy de cliente. Se pueden agregar métodos para el proxy de cliente de las maneras siguientes (tenga en cuenta que el método se agrega a la `client` miembro del concentrador, no directamente del centro):

**Código de cliente de JavaScript que agrega métodos a un proxy de concentrador**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>Concentrador o métodos de concentrador no declarados como público

Para que esté visible en el cliente, la implementación del concentrador y los métodos deben declararse como `public`.

### <a name="accessing-hub-from-a-different-application"></a>Tener acceso al centro de una aplicación diferente

Concentradores de SignalR solo puede obtenerse a través de las aplicaciones que implementan a los clientes de SignalR. SignalR no puede interoperar con otras bibliotecas de comunicación (por ejemplo, SOAP o WCF web services.) Si no hay ningún cliente de SignalR disponibles para la plataforma de destino, no se puede tener acceso a punto de conexión del servidor directamente.

### <a name="manually-serializing-data"></a>Serialización de datos manualmente

SignalR no usarán automáticamente JSON para serializar el método es necesario de ahí los parámetros hacerlo usted mismo.

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>Método de concentrador remoto que no se ejecuta en el cliente en función de OnDisconnected

Este comportamiento está diseñado así. Cuando `OnDisconnected` es llama, el concentrador ya ha entrado en el `Disconnected` estado, lo que no permite más llamar los métodos de concentrador.

**Código de servidor de C# que se ejecuta correctamente el código en el evento OnDisconnected**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="ondisconnect-not-firing-at-consistent-times"></a>No se activa en un momento coherente de OnDisconnect

Este comportamiento está diseñado así. Cuando un usuario intenta salir de una página con una conexión SignalR activa, el cliente de SignalR, a continuación, realizará un intento de mejor esfuerzo para notificar al servidor que se detendrá la conexión de cliente. Si el cliente de SignalR del esfuerzo se produce un error en intento de acceder al servidor, el servidor eliminará la conexión después de un configurable `DisconnectTimeout` más adelante, en qué momento el `OnDisconnected` se desencadenará el evento. Si el cliente de SignalR del esfuerzo intento es correcto, el `OnDisconnected` evento se activará inmediatamente.

Para obtener información sobre cómo el `DisconnectTimeout` , vea [controlar eventos de duración de la conexión: DisconnectTimeout](../guide-to-the-api/handling-connection-lifetime-events.md#disconnecttimeout).

### <a name="connection-limit-reached"></a>Ha alcanzado el límite de conexión

Cuando se usa la versión completa de IIS en un sistema operativo de cliente como Windows 7, se impone un límite de 10 conexiones. Cuando se usa a un sistema operativo cliente, use IIS Express en su lugar, para evitar este límite.

### <a name="cross-domain-connection-not-set-up-properly"></a>Conexión entre dominios no configurada correctamente

Si una conexión entre dominios (una conexión para que la dirección URL de SignalR no está en el mismo dominio que la página de hospedaje) no está configurada correctamente, la conexión puede producir un error sin mostrar un mensaje de error. Para obtener información sobre cómo habilitar la comunicación entre dominios, vea [cómo establecer una conexión entre dominios](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>Conexión mediante NTLM (Active Directory) no funciona en el cliente de .NET

Una conexión en una aplicación de cliente de .NET que usa seguridad de dominio puede producir un error si la conexión no está configurada correctamente. Para usar SignalR en un entorno de dominio, establezca la propiedad de conexión necesaria como se indica a continuación:

**Código de cliente de C# que implementa las credenciales de conexión**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="pong"></a>

## <a name="configuring-iis-websockets-to-pingpong-to-detect-a-dead-client"></a>Configuración de websockets de IIS a ping/pong para detectar a un cliente inactivo

Servidores de SignalR no saben si el cliente está inactiva o no y se basan en la notificación de websocket subyacente para los errores de conexión, es decir, el `OnClose` devolución de llamada. Una solución a este problema consiste en configurar websockets de IIS para hacer el pong de ping por usted. Esto garantiza que la conexión se cerrará si interrumpe inesperadamente. Para obtener más información, consulte [esta entrada de StackOverflow](http://stackoverflow.com/questions/19502755/websocket-clients-state-not-changing-on-network-loss).

<a id="other"></a>

## <a name="other-connection-issues"></a>Otros problemas de conexión

Esta sección describen las causas y soluciones para los síntomas específicos o mensajes de error que se producen durante una conexión.

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>Error de "Inicio se debe llamar antes de que se pueden enviar datos"

Normalmente este error aparece si el código hace referencia a objetos de SignalR antes de inicia la conexión. La conexión para los controladores y similares que llaman a los métodos definidos en el servidor debe se agregará al finalizar la conexión. Tenga en cuenta que la llamada a `Start` es asincrónica, por lo que el código después de la llamada se puede ejecutar antes de que se complete. La mejor manera de agregar controladores después de iniciar una conexión completamente es colocarlos en una función de devolución de llamada que se pasa como parámetro al método start:

**Código de cliente de JavaScript que se agrega correctamente los controladores de eventos que hacen referencia a objetos de SignalR**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

Este error también se verán si una conexión se detiene mientras todavía se hace referencia los objetos de SignalR.

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>"301 movido definitivamente" o "302 movido temporalmente" error

Este error puede aparecer si el proyecto contiene una carpeta denominada SignalR, que podría interferir con el proxy crea automáticamente. Para evitar este error, no use una carpeta denominada `SignalR` en su aplicación o la generación del proxy automática turn off. Consulte [el Proxy generado y lo que hace por usted](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) para obtener más detalles.

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>Error "403 Prohibido" en el cliente de .NET o Silverlight

Este error puede producirse en entornos entre dominios que no está habilitada correctamente la comunicación entre dominios. Para obtener información sobre cómo habilitar la comunicación entre dominios, vea [cómo establecer una conexión entre dominios](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain). Para establecer una conexión entre dominios en un cliente de Silverlight, vea [conexiones entre dominios desde clientes de Silverlight](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).

### <a name="404-not-found-error"></a>Error "404 no encontrado"

Hay varias causas para este problema. Compruebe que todos los elementos siguientes:

- **Referencia de dirección de proxy de concentrador no tiene el formato correcto:** este error suele aparecer si la referencia a la dirección de proxy de concentrador generado no se formateó correctamente. Compruebe que la referencia a la dirección del concentrador se realizará correctamente. Consulte [cómo hacen referencia al proxy generado dinámicamente](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) para obtener más información.
- **Agregar rutas a la aplicación antes de agregar la ruta de concentrador:** si la aplicación utiliza otras rutas, compruebe que la primera ruta agregada es la llamada a `MapSignalR`.
- **Con IIS 7 o 7.5 sin la actualización de direcciones URL sin extensión:** utilizando IIS 7 o 7.5 requiere una actualización para direcciones URL sin extensión para que el servidor puede proporcionar acceso a las definiciones de hub en `/signalr/hubs`. La actualización se puede encontrar [aquí](https://support.microsoft.com/kb/980368).
- **Almacenar en caché no actualizada IIS o está dañado:** para comprobar que el contenido de la caché no actualizada, escriba el siguiente comando en una ventana de PowerShell para borrar la memoria caché:

    [!code-powershell[Main](troubleshooting/samples/sample11.ps1)]

### <a name="500-internal-server-error"></a>"Error de servidor interno 500"

Se trata de un error genérico que podría tener una amplia variedad de causas. Los detalles del error deben aparecer en el registro de eventos del servidor, o se pueden encontrar en el servidor de depuración. Puede obtenerse información más detallada del error al activar errores detallados en el servidor. Para obtener más información, consulte [cómo controlar los errores en la clase Hub](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).

Este error normalmente también aparece si un firewall o proxy no está configurado correctamente, la causa de los encabezados de solicitud se vuelva a escribir. La solución consiste en asegurarse de que el puerto 80 está habilitado en el firewall o proxy.

### <a name="unexpected-response-code-500"></a>"Código de respuesta inesperado: 500"

Este error puede producirse si la versión de .NET framework que se usan en la aplicación no coincide con la versión especificada en el archivo Web.Config. La solución consiste en comprobar que se usa .NET 4.5 en la configuración de la aplicación y el archivo Web.Config.

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>"TypeError: &lt;hubType&gt; es indefinido" error

Este error se producirá si la llamada a `MapSignalR` no se realizará correctamente. Consulte [cómo registrar SignalR Middleware y configurar las opciones de SignalR](../guide-to-the-api/hubs-api-guide-server.md#route) para obtener más información.

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>JsonSerializationException no estaba controlada por código de usuario

Compruebe que los parámetros que se envían a los métodos no incluyen tipos que no son serializables (como identificadores de archivos o las conexiones de base de datos). Si tiene que utilizar los miembros en un objeto de servidor que no desea que se envía al cliente (ya sea por motivos de serialización o de seguridad), use el `JSONIgnore` atributo.

### <a name="protocol-error-unknown-transport-error"></a>"Error de protocolo: transporte desconocido" error

Este error puede producirse si el cliente no es compatible con los transportes que usa SignalR. Consulte [transportes y reservas](../getting-started/introduction-to-signalr.md#transports) para obtener información en el que se pueden usar exploradores con SignalR.

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>"Se ha deshabilitado la generación de proxy de concentrador de JavaScript."

Este error se producirá si `DisableJavaScriptProxies` se establece al tiempo que incluye también una referencia al proxy generado de forma dinámica en `signalr/hubs`. Para obtener más información sobre cómo crear el servidor proxy manualmente, consulte [el proxy generado y lo que hace por usted](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>"El identificador de conexión tiene el formato incorrecto" o "no se puede cambiar la identidad del usuario durante una conexión SignalR activa" error

Este error puede aparecer si se utiliza la autenticación y el cliente está cerrado antes de que la conexión se ha detenido. La solución es detener la conexión de SignalR antes de cerrar la sesión del cliente.

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>"No se detecta errores: SignalR: no se encuentra de jQuery. Asegúrese de que se hace referencia a jQuery antes del archivo SignalR.js"error

El cliente de JavaScript de SignalR requiere jQuery para ejecutar. Compruebe que la referencia a jQuery es correcta, que usa la ruta de acceso es válida y que la referencia a jQuery es antes de la referencia de SignalR.

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>"No se detecta TypeError: no se puede leer la propiedad '&lt;propiedad&gt;" undefined "error

Este error da como resultado de no tener jQuery o el proxy de concentradores hace referencia correctamente. Compruebe que la referencia a jQuery y el proxy de concentradores es correcta, que usa la ruta de acceso es válida y que la referencia a jQuery es antes de la referencia al proxy de concentradores. La referencia predeterminada para el proxy de concentradores debe ser similar al siguiente:

**Código de cliente HTML que hace referencia correctamente el proxy de concentradores**

[!code-html[Main](troubleshooting/samples/sample12.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>Error "RuntimeBinderException no estaba controlada por código de usuario"

Este error puede producirse cuando la sobrecarga incorrecta del `Hub.On` se utiliza. Si el método tiene un valor devuelto, se debe especificar el tipo de valor devuelto como un parámetro de tipo genérico:

**Método definido en el cliente (sin proxy generado)**

[!code-html[Main](troubleshooting/samples/sample13.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>Id. de conexión es incoherente o se interrumpe la conexión entre las cargas de página

Este comportamiento está diseñado así. Puesto que el objeto hub está hospedado en el objeto de página, el concentrador se destruye cuando se actualiza la página. Se necesita una aplicación de varias página mantener la asociación entre los usuarios y los identificadores de conexión para que sean coherentes entre las cargas de página. Los identificadores de conexión se pueden almacenar en el servidor en cualquiera de un `ConcurrentDictionary` objeto o una base de datos.

### <a name="value-cannot-be-null-error"></a>Error "El valor no puede ser null"

Métodos de servidor con los parámetros opcionales no se admiten actualmente; Si se omite el parámetro opcional, se producirá un error en el método. Para obtener más información, consulte [parámetros opcionales](https://github.com/SignalR/SignalR/issues/324).

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>"Firefox no puede establecer una conexión al servidor en &lt;dirección&gt;" error en Firebug

Este mensaje de error puede verse en Firebug si se produce un error en la negociación del transporte de WebSocket y otro transporte se utiliza en su lugar. Este comportamiento está diseñado así.

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>Error "el certificado remoto no es válido según el procedimiento de validación" en la aplicación cliente de .NET

Si el servidor requiere certificados de cliente personalizada, a continuación, puede agregar un x509certificate a la conexión antes de realizar la solicitud. Agregar el certificado para la conexión con `Connection.AddClientCertificate`.

### <a name="connection-drops-after-authentication-times-out"></a>Fallo de conexión después de que el tiempo de espera de autenticación

Este comportamiento está diseñado así. Las credenciales de autenticación no se puede modificar mientras una conexión está activa; Para actualizar las credenciales, debe detener y reiniciar la conexión.

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>OnConnected se llama dos veces al usar jQuery Mobile

jQuery Mobile `initializePage` función fuerza las secuencias de comandos en cada página para volver a ejecutarse, creando así una segunda conexión. Las soluciones para resolver este problema incluyen:

- Incluir la referencia a jQuery Mobile antes de que el archivo JavaScript.
- Deshabilitar la `initializePage` función estableciendo `$.mobile.autoInitializePage = false`.
- Espere a que la página para finalizar la inicialización antes de iniciar la conexión.

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>Los mensajes están retrasados en aplicaciones de Silverlight con eventos enviados del servidor

Los mensajes están retrasados cuando utilizando el servidor envía los eventos de Silverlight. Para forzar el largo de sondeo para usarse en su lugar, use lo siguiente al iniciar la conexión:

[!code-css[Main](troubleshooting/samples/sample14.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>Siempre utilizando "Permiso denegado" Protocolo de marco

Se trata de un problema conocido que se describen [aquí](https://github.com/SignalR/SignalR/issues/1963). Este síntoma puede verse mediante la biblioteca de JQuery más reciente; la solución consiste en cambiar la aplicación para JQuery 1.8.2.

### <a name="invalidoperationexception-not-a-valid-web-socket-request"></a>"InvalidOperationException: no una solicitud de socket web válido.

Este error puede producirse si se usa el protocolo WebSocket, pero el proxy de red está modificando los encabezados de solicitud. La solución consiste en configurar el proxy para permitir el WebSocket en el puerto 80.

### <a name="exception-ltmethod-namegt-method-could-not-be-resolved-when-client-calls-method-on-server"></a>"Excepción: &lt;nombre del método&gt; no se pudo resolver el método" al cliente llama a método en el servidor

Este error puede obtenerse utilizando tipos de datos que no se pueden detectar en una carga JSON, por ejemplo, la matriz. La solución alternativa es usar un tipo de datos que se puede detectar mediante JSON, como IList. Para obtener más información, consulte [cliente .NET no se puede llamar a métodos de concentrador con los parámetros de matriz](https://github.com/SignalR/SignalR/issues/2672).

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>Errores de compilación y del lado servidor

 En la sección siguiente contiene posibles soluciones para el compilador y errores en tiempo de ejecución del servidor. 

### <a name="reference-to-hub-instance-is-null"></a>Referencia a la instancia del concentrador es null

Puesto que se crea una instancia de concentrador para cada conexión, no se puede crear una instancia de un concentrador en el código usted mismo. Para llamar a métodos en un centro desde fuera del propio centro de, consulte [cómo llamar a los métodos de cliente y administrar grupos de fuera de la clase Hub](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) para saber cómo obtener una referencia al contexto del concentrador.

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext.Current.Session es null

Este comportamiento está diseñado así. SignalR no es compatible con el estado de sesión ASP.NET, puesto que habilitar el estado de sesión se interrumpiría mensajería dúplex.

### <a name="no-suitable-method-to-override"></a>Ningún método adecuado para invalidar

Puede ver este error si se utiliza el código de la documentación anterior o blogs. Compruebe que no hacen referencia a nombres de métodos que se han cambiado o en desuso (como `OnConnectedAsync`).

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions.WebSocketServerUrl es null

Este comportamiento está diseñado así. Este miembro está en desuso y no debe usarse.

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>Error "una ruta denominada 'signalr.hubs' ya está en la colección de rutas"

Este error se verán si `MapSignalR` se llama dos veces la aplicación. Algunas aplicaciones de ejemplo llamada `MapSignalR` directamente en la clase de inicio; otros usuarios realicen la llamada en una clase contenedora. Asegúrese de que la aplicación no ambas cosas.

### <a name="websocket-is-not-used"></a>No se usa el WebSocket

Si ha comprobado que el servidor y los clientes cumplan los requisitos de WebSocket (enumerados en el [plataformas admitidas](../getting-started/supported-platforms.md) documento), deberá habilitar WebSocket en el servidor. Puede encontrar instrucciones para realizar esta acción [aquí](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).

### <a name="connection-is-undefined"></a>$.connection no está definido

Este error indica que las secuencias de comandos en una página no se cargan correctamente, o el proxy de concentrador no es accesible o se tiene acceso incorrectamente. Compruebe que las referencias de script en la página se corresponden con los scripts cargados en el proyecto y que se puede acceder/signalr/hubs en un explorador cuando se está ejecutando el servidor.

### <a name="one-or-more-types-required-to-compile-a-dynamic-expression-cannot-be-found"></a>No se encuentra uno o varios de los tipos necesarios para compilar una expresión dinámica

Este error indica que el `Microsoft.CSharp` biblioteca falta. Agréguelo a la **ensamblados -&gt;Framework** ficha.

### <a name="caller-state-cannot-be-accessed-from-clientscaller-in-visual-basic-or-in-a-strongly-typed-hub-conversion-from-type-taskof-object-to-type-string-is-not-valid-error"></a>Estado del autor de llamada no es accesible desde Clients.Caller en Visual Basic o en un concentrador fuertemente tipado; Error de "Conversión de tipo 'Task (Of Object)' al tipo 'String' no es válida"

Para obtener acceso a estado del llamador en Visual Basic o en un concentrador fuertemente tipado, utilice el `Clients.CallerState` propiedad (introducida en SignalR 2.1) en lugar de `Clients.Caller`.

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Problemas de Visual Studio

Esta sección describen los problemas detectados en Visual Studio.

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>Nodo de documentos de script no aparece en el Explorador de soluciones

Algunos de nuestros tutoriales le dirigirán al nodo "Documentos de Script" en el Explorador de soluciones durante la depuración. Este nodo es generado por el depurador de JavaScript y sólo aparecerá durante la depuración de los clientes de explorador en Internet Explorer; el nodo no aparecerá si se utiliza Chrome o Firefox. El depurador de JavaScript no ejecutará si se está ejecutando otro depurador de cliente, por ejemplo, el depurador de Silverlight.

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>SignalR no funciona en Visual Studio 2008 o versiones anteriores

Este comportamiento está diseñado así. SignalR requiere .NET Framework 4 o posterior; Esto requiere que se desarrolle aplicaciones SignalR en Visual Studio 2010 o posterior. El componente de servidor de SignalR requiere .NET Framework 4.5.

<a id="iis"></a>

## <a name="iis-issues"></a>Problemas de IIS

Esta sección contiene problemas relacionados con Internet Information Services.

### <a name="signalr-works-on-visual-studio-development-server-but-not-in-iis"></a>SignalR funciona en el servidor de desarrollo de Visual Studio, pero no en IIS

SignalR es compatible con IIS 7.0 y 7.5, pero la compatibilidad con direcciones URL sin extensión se deben agregar. Para agregar compatibilidad con direcciones URL sin extensión, vea [https://support.microsoft.com/kb/980368](https://support.microsoft.com/kb/980368)

SignalR requiere ASP.NET para instalarse en el servidor (ASP.NET no está instalado en IIS de forma predeterminada). Para instalar ASP.NET, consulte [descargas de ASP.NET](https://www.asp.net/downloads).

<a id="azure"></a>

## <a name="microsoft-azure-issues"></a>Problemas de Microsoft Azure

Esta sección contiene problemas relacionados con Microsoft Azure.

### <a name="fileloadexception-when-hosting-signalr-in-an-azure-worker-role"></a>FileLoadException al hospedar SignalR en un rol de trabajador de Azure

Hospedaje de SignalR en un rol de trabajador de Azure puede dar lugar a la excepción "no se pudo cargar el archivo o ensamblado ' Microsoft.Owin, Version = 2.0.0.0". Se trata de un problema conocido con NuGet; No se agregan automáticamente las redirecciones de enlace en los proyectos de rol de trabajo de Azure. Para solucionar este problema, puede agregar manualmente las redirecciones de enlace. Agregue las líneas siguientes a la `app.config` archivo del proyecto de rol de trabajo.

[!code-xml[Main](troubleshooting/samples/sample15.xml)]

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>No se reciben los mensajes a través de la placa de Azure después de modificar los nombres de tema

Los temas de la placa de Azure se guardan internamente; no pretenden ser configurables por el usuario.
