---
uid: signalr/overview/older-versions/troubleshooting
title: Solución de problemas de SignalR (SignalR 1.x) | Documentos de Microsoft
author: pfletcher
description: Este artículo describen problemas comunes con el desarrollo de aplicaciones SignalR.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2013
ms.topic: article
ms.assetid: 347210ba-c452-4feb-886f-b51d89f58971
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: eb739f806eb57d09008fa0bf04b5a40721dab73d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="signalr-troubleshooting-signalr-1x"></a>Solución de problemas de SignalR (SignalR 1.x)
====================
por [Patrick Fletcher](https://github.com/pfletcher)

> Este documento describen los problemas más comunes con SignalR.


Este documento contiene las siguientes secciones.

- [Llamar a métodos entre el cliente y el servidor en modo silencioso se produce un error](#connection)
- [Otros problemas de conexión](#other)
- [Errores de compilación y el lado servidor](#server)
- [Problemas de Visual Studio](#vs)
- [Problemas de Internet Information Services](#iis)
- [Problemas de Azure](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>Llamar a métodos entre el cliente y el servidor en modo silencioso se produce un error

Esta sección describen las posibles causas de una llamada al método entre cliente y servidor para producir un error sin un mensaje de error significativo. En una aplicación de SignalR, el servidor no tiene información sobre los métodos que implementa el cliente; Cuando el servidor, invoca un método de cliente, los datos de nombre y los parámetros de método se envían al cliente y el método se ejecuta sólo si se encuentra en el formato que el servidor especificado. Si no se encuentra ningún método coincidente en el cliente, no ocurre nada, y no se genera ningún mensaje de error en el servidor.

Para investigar más sobre los métodos de cliente no esté llamando, puede activar el registro antes de llamar al método start en el concentrador para ver qué llamadas se procedente del servidor. Para habilitar el registro en una aplicación de JavaScript, consulte [cómo habilitar el registro de cliente (versión de cliente de JavaScript)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging). Para habilitar el registro en una aplicación de cliente. NET, vea [cómo habilitar el registro de cliente (versión de cliente. NET)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>Método mal escrito, firma de método incorrecto o nombre del concentrador incorrecto

Si el nombre o la firma de un método llamado coincide exactamente un método adecuado en el cliente, se producirá un error en la llamada. Compruebe que el nombre del método llamado por el servidor coincide con el nombre del método en el cliente. Además, SignalR crea el proxy de concentrador mediante métodos de grafía de camel, según resulte adecuado en JavaScript, por lo que un método llamado `SendMessage` en el servidor se denominaría `sendMessage` en el proxy de cliente. Si usas el `HubName` de atributo en el código de servidor, compruebe que el nombre utilizado coincide con el nombre utilizado para crear el concentrador en el cliente. Si no usa el `HubName` atributo, compruebe que el nombre del concentrador en un cliente de JavaScript es la grafía camel, por ejemplo, chatHub en lugar de ChatHub.

### <a name="duplicate-method-name-on-client"></a>Nombre de método en el cliente está duplicado

Compruebe que no tiene un método duplicado en el cliente que se diferencia sólo por mayúsculas o minúsculas. Si la aplicación cliente tiene un método denominado `sendMessage`, asegúrese de que también hay un método llamado `SendMessage` así.

### <a name="missing-json-parser-on-the-client"></a>Analizador JSON que falta en el cliente

SignalR requiere un analizador JSON que esté presente para serializar las llamadas entre el servidor y el cliente. Si el cliente no tiene un analizador JSON integrado (por ejemplo, Internet Explorer 7), debe incluir en la aplicación. Puede descargar el analizador JSON [aquí](http://nuget.org/packages/json2).

### <a name="mixing-hub-and-persistentconnection-syntax"></a>Mezclar la sintaxis de tipo Hub- and PersistentConnection

SignalR usa dos modelos de comunicación: concentradores y PersistentConnections. La sintaxis para llamar a estos modelos de dos comunicación es diferente en el código de cliente. Si ha agregado un concentrador en el código del servidor, compruebe que todo el código de cliente utiliza la sintaxis correcta de concentrador.

**Código de cliente de JavaScript que crea una PersistentConnection en un cliente de JavaScript**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**Código de cliente de JavaScript que crea a un Proxy de concentrador en un cliente de Javascript**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**Código de servidor de C# que se asigna una ruta a un PersistentConnection**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**Código de servidor de C# que se asigna una ruta a un concentrador o a los centros de múltiples si tiene varias aplicaciones**

[!code-csharp[Main](troubleshooting/samples/sample4.cs)]

### <a name="connection-started-before-subscriptions-are-added"></a>Conexión que se inicia antes de que se agregan las suscripciones

Si se inicia conexión del concentrador antes de que los métodos que se pueda llamar desde el servidor se agregan al proxy, no se recibirán los mensajes. El siguiente código de JavaScript no iniciará el concentrador correctamente:

**Código de cliente de JavaScript incorrecta que no permitirá que van a recibir los mensajes de concentradores**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

En su lugar, agregue las suscripciones del método antes de llamar a Start:

**Código de cliente de JavaScript que se agrega correctamente las suscripciones a un concentrador**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>Falta el nombre de método en el proxy de concentrador

Compruebe que el método definido en el servidor se ha suscrito en el cliente. Aunque el servidor define el método, todavía debe agregarse al proxy de cliente. Métodos pueden agregarse para el proxy de cliente de las maneras siguientes (tenga en cuenta que el método se agrega a la `client` miembro del concentrador, no directamente el concentrador):

**Código de cliente de JavaScript que se agrega los métodos a un proxy de concentrador**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>Concentrador o métodos de concentrador no declarados como público

Para que esté visible en el cliente, la implementación del concentrador y métodos deben declararse como `public`.

### <a name="accessing-hub-from-a-different-application"></a>Al obtener acceso al centro de la otra aplicación

Concentradores de SignalR sólo puede tener acceso a través de las aplicaciones que implementan a los clientes de SignalR. SignalR no puede interoperar con otras bibliotecas de comunicación (por ejemplo, SOAP o WCF servicios web.) Si no hay ningún cliente de SignalR disponible para la plataforma de destino, no se puede tener acceso a punto de conexión del servidor directamente.

### <a name="manually-serializing-data"></a>Serializar los datos manualmente

SignalR no usarán automáticamente JSON para serializar el método necesidad del allí parámetros de hacerlo manualmente.

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>Método de concentrador remoto que no se ejecuta en el cliente en función de OnDisconnected

Este comportamiento está diseñado así. Cuando `OnDisconnected` es llama, el concentrador ya ha escrito la `Disconnected` estado, que no permite más métodos de concentrador llamarlo.

**Código de servidor de C# que se ejecuta correctamente el código en el evento OnDisconnected**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="connection-limit-reached"></a>Ha alcanzado el límite de conexión

Al utilizar la versión completa de IIS en un sistema operativo de cliente como Windows 7, se impone un límite de 10 conexiones. Cuando se usa a un sistema operativo cliente, use IIS Express en su lugar, para evitar este límite.

### <a name="cross-domain-connection-not-set-up-properly"></a>Conexión entre dominios no configuró correctamente

Si una conexión entre dominios (una conexión para el que no está en el mismo dominio que la página donde se hospeda la dirección URL de SignalR) no está configurada correctamente, la conexión puede producir un error sin mostrar un mensaje de error. Para obtener información acerca de cómo habilitar la comunicación entre dominios, consulte [cómo establecer una conexión entre dominios](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>Conexión mediante NTLM (Active Directory) no funciona en el cliente de .NET

Una conexión en una aplicación de cliente de .NET que utiliza la seguridad de dominio puede producir un error si la conexión no está configurada correctamente. Para utilizar SignalR en un entorno de dominio, establezca la propiedad de conexión necesaria como se indica a continuación:

**Código de cliente de C# que implementa las credenciales de conexión**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="other"></a>

## <a name="other-connection-issues"></a>Otros problemas de conexión

Esta sección describe las causas y soluciones para los síntomas específicos o mensajes de error que se producen durante una conexión.

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>Error de "Inicio se debe llamar antes de que se pueden enviar datos"

Este error se suele producir si el código hace referencia a objetos de SignalR antes de inicia la conexión. La conexión para los controladores y similares que se llaman a los métodos definidos en el servidor debe agregarse una vez completada la conexión. Tenga en cuenta que la llamada a `Start` es asincrónica, por lo que el código después de la llamada se puede ejecutar antes de que se completa. La mejor manera de agregar controladores después de iniciar una conexión completamente es colocarlos en una función de devolución de llamada que se pasa como parámetro al método start:

**Código de cliente de JavaScript que se agrega correctamente los controladores de eventos que hacen referencia a objetos de SignalR**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

Este error también se verán si una conexión se detiene mientras todavía se hace referencia objetos de SignalR.

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>Error "302 movido temporalmente" o "301 mueve de forma permanente"

Este error puede aparecer si el proyecto contiene una carpeta denominada SignalR, que se va a interferir con el proxy crea automáticamente. Para evitar este error, no use una carpeta denominada `SignalR` en sus aplicaciones o la generación de automática de proxy turn off. Vea [el Proxy generado y lo que hace que](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) para obtener más detalles.

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>Error "403 Prohibido" en el cliente de .NET o Silverlight

Este error puede producirse en entornos entre dominios y la comunicación entre dominios no está habilitada correctamente. Para obtener información acerca de cómo habilitar la comunicación entre dominios, consulte [cómo establecer una conexión entre dominios](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain). Para establecer una conexión entre dominios en un cliente de Silverlight, vea [las conexiones entre dominios desde clientes Silverlight](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).

### <a name="404-not-found-error"></a>Error "404 no encontrado"

Hay varias causas para este problema. Compruebe que todos los elementos siguientes:

- **Referencia de dirección de proxy de concentrador no tiene el formato correcto:** este error se suele producir si la referencia a la dirección de proxy de concentrador generado no tiene el formato correcto. Compruebe que la referencia a la dirección de la base de datos central se ha creado correctamente. Vea [cómo hacen referencia al proxy generado dinámicamente](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) para obtener más información.
- **Agregar rutas a la aplicación antes de agregar la ruta de concentrador:** si la aplicación utiliza otras rutas, compruebe que la primera ruta agrega es la llamada a `MapHubs`.

### <a name="500-internal-server-error"></a>"500 Internal Server Error"

Se trata de un error muy genérico que podría tener una gran variedad de causas. Los detalles del error deben aparecer en el registro de eventos del servidor, o pueden encontrarse a través de depurar el servidor. Puede obtenerse información más detallada del error al activar errores detallados en el servidor. Para obtener más información, consulte [cómo controlar los errores en la clase de base de datos central](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>"TypeError: &lt;hubType&gt; no está definido" error

Este error se producirá si la llamada a `MapHubs` no se realiza correctamente. Vea [cómo registrar la ruta de SignalR y configurar las opciones de SignalR](../guide-to-the-api/hubs-api-guide-server.md#route) para obtener más información.

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>JsonSerializationException no estaba controlada por código de usuario

Compruebe que los parámetros que se envía a los métodos no incluyen tipos no son serializables (por ejemplo, identificadores de archivo o las conexiones de base de datos). Si necesita usar miembros en un objeto de servidor que no desea que se envía al cliente (ya sea por motivos de serialización o de seguridad), use el `JSONIgnore` atributo.

### <a name="protocol-error-unknown-transport-error"></a>"Error de protocolo: transporte desconocido" error

Este error puede producirse si el cliente no es compatible con los transportes que usa SignalR. Vea [transportes y reservas](../getting-started/introduction-to-signalr.md#transports) para obtener información en el que se pueden utilizar los exploradores con SignalR.

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>"Se ha deshabilitado generación de proxy del JavaScript Hub."

Este error se producirá si `DisableJavaScriptProxies` mientras también incluye una referencia al proxy generado dinámicamente en `signalr/hubs`. Para obtener más información acerca de cómo crear el proxy manualmente, consulte [el proxy generado y lo que hace que](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>"El identificador de conexión está en un formato incorrecto" o "no se puede cambiar la identidad del usuario durante una conexión de SignalR activa" error

Este error puede aparecer si se utiliza la autenticación y el cliente se cerró la sesión antes de que la conexión se ha detenido. La solución consiste en detener la conexión de SignalR antes de cerrar la sesión del cliente.

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>"No detectada Error: SignalR: no se encontró de jQuery. Asegúrese de que se hace referencia a jQuery antes del archivo SignalR.js"error

El cliente de SignalR JavaScript requiere jQuery para ejecutar. Compruebe que la referencia a jQuery es correcta, que usa la ruta de acceso es válida y que la referencia a jQuery es antes de la referencia de SignalR.

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>"No detectada TypeError: no se puede leer la propiedad '&lt;propiedad&gt;" undefined "error

Este error resultante de no tener jQuery o el proxy de concentradores al que hace referencia correctamente. Compruebe que la referencia a jQuery y el proxy de concentradores es correcta, que usa la ruta de acceso es válida y que es la referencia a jQuery antes que la referencia al proxy de concentradores. La referencia predeterminada para el proxy de concentradores debe ser similar al siguiente:

**Código de cliente HTML que hace referencia correctamente al proxy de concentradores**

[!code-html[Main](troubleshooting/samples/sample11.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>Error "RuntimeBinderException no estaba controlada por código de usuario"

Este error puede producirse cuando la sobrecarga incorrecta de `Hub.On` se utiliza. Si el método tiene un valor devuelto, el tipo de valor devuelto debe especificarse como un parámetro de tipo genérico:

**Método definido en el cliente (sin proxy generado)**

[!code-html[Main](troubleshooting/samples/sample12.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>Identificador de conexión no es coherente o saltos de conexión entre la carga de página

Este comportamiento está diseñado así. Puesto que el objeto de base de datos central se hospeda en el objeto de página, el concentrador se destruye cuando actualice la página. Se necesita una aplicación de varias página mantener la asociación entre los usuarios y los identificadores de conexión para que estén coherentes entre las cargas de página. La identificadores de conexión puede almacenarse en el servidor en la vista una `ConcurrentDictionary` objeto o una base de datos.

### <a name="value-cannot-be-null-error"></a>Error "El valor no puede ser nulo"

Métodos de servidor con los parámetros opcionales no se admiten actualmente; Si se omite el parámetro opcional, se producirá un error en el método. Para obtener más información, consulte [parámetros opcionales](https://github.com/SignalR/SignalR/issues/324).

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>"Firefox no puede establecer una conexión al servidor a &lt;dirección&gt;" error en Firebug

Este mensaje de error puede verse en Firebug si se produce un error en la negociación del transporte de WebSocket y otro transporte se utiliza en su lugar. Este comportamiento está diseñado así.

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>Error "el certificado remoto no es válido según el procedimiento de validación" en la aplicación cliente de .NET

Si el servidor requiere certificados de cliente personalizada, a continuación, puede agregar un x509certificate a la conexión antes de que se realiza la solicitud. Agregar el certificado para la conexión con `Connection.AddClientCertificate`.

### <a name="connection-drops-after-authentication-times-out"></a>Fallo de conexión una vez agota el tiempo de espera de autenticación

Este comportamiento está diseñado así. Las credenciales de autenticación no se puede modificar mientras una conexión está activa; Para actualizar las credenciales, debe detener y reiniciar la conexión.

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>OnConnected se invoca dos veces al uso de jQuery Mobile

jQuery Mobile `initializePage` función fuerza las secuencias de comandos en cada página para volver a ejecutar, creando así una segunda conexión. Soluciones para este problema incluyen:

- Incluir la referencia a jQuery Mobile antes de que el archivo de JavaScript.
- Deshabilitar la `initializePage` función estableciendo `$.mobile.autoInitializePage = false`.
- Espere a que la página para finalizar la inicialización antes de iniciar la conexión.

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>Los mensajes están retrasados en aplicaciones de Silverlight con eventos enviados del servidor

Los mensajes están retrasados cuando usa servidor envía eventos de Silverlight. Para forzar el tiempo de sondeo para usarse en su lugar, utilice lo siguiente al iniciar la conexión:

[!code-css[Main](troubleshooting/samples/sample13.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>Uso de "Permiso denegado" Enmarcar indefinidamente protocolo

Se trata de un problema conocido, se describe [aquí](https://github.com/SignalR/SignalR/issues/1963). Este síntoma puede verse mediante la biblioteca de JQuery más reciente; la solución consiste en cambiar la aplicación para JQuery 1.8.2.

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>Errores de compilación y el lado servidor

 La sección siguiente contiene posibles soluciones al compilador y errores en tiempo de ejecución del servidor. 

### <a name="reference-to-hub-instance-is-null"></a>Referencia a la instancia de base de datos central es null

Puesto que se crea una instancia de base de datos central para cada conexión, no se puede crear una instancia de un concentrador en el código usted mismo. Para llamar a métodos en un centro de fuera del concentrador propio, vea [cómo llamar a los métodos de cliente y administrar grupos desde fuera de la clase de base de datos central](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) para saber cómo obtener una referencia para el contexto del concentrador.

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext.Current.Session es null

Este comportamiento está diseñado así. SignalR no es compatible con el estado de sesión ASP.NET, puesto que habilitar el estado de sesión, se interrumpiría mensajería dúplex.

### <a name="no-suitable-method-to-override"></a>Ningún método adecuado para invalidar

Puede ver este error si se utiliza el código de la documentación anterior o blogs. Compruebe que no se hace referencia a nombres de métodos que se han cambiado o en desuso (como `OnConnectedAsync`).

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions.WebSocketServerUrl es nulo

Este comportamiento está diseñado así. Este miembro está en desuso y no debe usarse.

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>Error "una ruta denominada 'signalr.hubs' ya está en la colección de rutas"

Verá este error si `MapHubs` se llama dos veces a la aplicación. Algunas aplicaciones de ejemplo llamada `MapHubs` directamente en el archivo de aplicación global; otros usuarios realicen la llamada en una clase contenedora. Asegúrese de que la aplicación no ambas cosas.

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Problemas de Visual Studio

Esta sección describe problemas que pueden producirse en Visual Studio.

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>Nodo de documentos de script no aparece en el Explorador de soluciones

Algunos de nuestros tutoriales le llevan al nodo "Documentos de Script" en el Explorador de soluciones durante la depuración. Este nodo se genera mediante el depurador de JavaScript y solo se mostrará durante la depuración de los clientes de explorador en Internet Explorer; el nodo no aparecerá si se utiliza Chrome o Firefox. El depurador de JavaScript no ejecutará si está ejecutando otro depurador de cliente, por ejemplo, el depurador de Silverlight.

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>SignalR no funciona en Visual Studio 2008 o versiones anteriores

Este comportamiento está diseñado así. SignalR requiere .NET Framework 4 o posterior; Esto requiere que se desarrollan aplicaciones SignalR en Visual Studio 2010 o posterior.

<a id="iis"></a>

## <a name="iis-issues"></a>Problemas IIS

Esta sección contiene problemas con Internet Information Services.

### <a name="web-site-crashes-after-maphubs-call"></a>Sitio Web se bloquea después de llamar MapHubs

Este problema se corrigió en la versión más reciente de SignalR. Compruebe que está utilizando la versión publicada más reciente de SignalR mediante la actualización de la instalación mediante NuGet.

<a id="azure"></a>

## <a name="azure-issues"></a>Problemas de Azure

Esta sección contiene problemas con Microsoft Azure.

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>No se reciben los mensajes a través de la placa de Azure después de modificar los nombres de temas

Los temas que se usa el backplane de Azure no pretenden ser configurables por el usuario.
