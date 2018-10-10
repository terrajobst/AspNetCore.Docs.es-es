---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: 'Guía de API de ASP.NET SignalR Hubs: cliente JavaScript | Microsoft Docs'
author: pfletcher
description: Este documento proporciona una introducción al uso de la API de concentradores de SignalR versión 2 en los clientes de JavaScript, como los exploradores y usos prácticos de Windows Store (WinJS)...
ms.author: riande
ms.date: 09/28/2015
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 9edb7fd100a3f4c5331454045ac206d2f7a81961
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912454"
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a>Guía de API de ASP.NET SignalR Hubs: cliente JavaScript
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Este documento proporciona una introducción al uso de la API de concentradores de SignalR versión 2 en los clientes de JavaScript, como los exploradores y aplicaciones de Windows Store (WinJS).
>
> La API de concentradores de SignalR permite realizar llamadas a procedimiento remoto (RPC) de un servidor a los clientes conectados y de clientes en el servidor. En el código de servidor, definir los métodos que se pueden llamar a los clientes y llamar a métodos que se ejecutan en el cliente. En el código de cliente, definir los métodos que pueden llamarse desde el servidor y llamar a métodos que se ejecutan en el servidor. SignalR se ocupa de todos los mecanismos de cliente a servidor para usted.
>
> SignalR también ofrece una API de nivel inferior denominada conexiones persistentes. Para obtener una introducción a SignalR, concentradores y conexiones persistentes, consulte [Introducción a SignalR](../getting-started/introduction-to-signalr.md).
>
> ## <a name="software-versions-used-in-this-topic"></a>Versiones de software que se usa en este tema
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
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


## <a name="overview"></a>Información general

Este documento contiene las siguientes secciones:

- [El proxy generado y lo que hace por usted](#genproxy)

    - [Cuándo se debe usar al proxy generado](#cantusegenproxy)
- [Programa de instalación de cliente](#clientsetup)

    - [Cómo se hacen referencia al proxy generado dinámicamente](#dynamicproxy)
    - [Cómo crear un archivo físico para SignalR genera proxy](#manualproxy)
- [Cómo establecer una conexión](#establishconnection)

    - [$. connection.hub es el mismo objeto crea ese $.hubConnection()](#connequivalence)
    - [Ejecución asincrónica del método de inicio](#asyncstart)
- [Cómo establecer una conexión entre dominios](#crossdomain)
- [Cómo configurar la conexión](#configureconnection)

    - [Cómo especificar parámetros de cadena de consulta](#querystring)
    - [Cómo especificar el modo de transporte](#transport)
- [Cómo obtener a un proxy para una clase de Hub](#getproxy)
- [Cómo definir métodos en el cliente que el servidor puede llamar a](#callclient)
- [Cómo llamar a métodos del servidor desde el cliente](#callserver)
- [Cómo controlar eventos de duración de la conexión](#connectionlifetime)
- [Cómo controlar los errores](#handleerrors)
- [Cómo habilitar el registro del lado cliente](#logging)

Para obtener documentación sobre cómo programar el servidor o los clientes. NET, consulte los siguientes recursos:

- [Guía de la API SignalR Hubs - servidor](hubs-api-guide-server.md)
- [Guía de API de concentradores de SignalR: cliente .NET](hubs-api-guide-net-client.md)

El componente de servidor de SignalR 2 solo está disponible en .NET 4.5 (aunque no hay un cliente de .NET para SignalR 2 en .NET 4.0).

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>El proxy generado y lo que hace por usted

Puede programar un cliente de JavaScript para comunicarse con un servicio de SignalR con o sin un proxy que SignalR genera para usted. Lo que hace el proxy para usted es simplificar la sintaxis del código se usa para conectarse, los métodos de escritura que llama el servidor, y llamar a métodos en el servidor.

Al escribir código para llamar a métodos de servidor, el proxy generado le permite usar la sintaxis que parece ser que se estaban ejecutando una función local: puede escribir `serverMethod(arg1, arg2)` en lugar de `invoke('serverMethod', arg1, arg2)`. La sintaxis de proxy generada también permite un error inmediato e inteligible del lado cliente, si se escribe incorrectamente el nombre de un método de servidor. Y si creas manualmente el archivo que define a los servidores proxy, también puede obtener compatibilidad con IntelliSense para escribir código que llama a métodos de servidor.

Por ejemplo, suponga que tiene la siguiente clase de Hub en el servidor:

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

Los ejemplos de código siguientes muestran JavaScript código aspecto para invocar el `NewContosoChatMessage` método en el servidor y recibir las invocaciones de la `addContosoChatMessageToPage` método desde el servidor.

**Con el proxy generado**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Sin el proxy generado**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Cuándo se debe usar al proxy generado

Si desea registrar varios controladores de eventos para un método de cliente que llama el servidor, no puede usar al proxy generado. En caso contrario, puede optar por usar al proxy generado o no según sus preferencias de codificación. Si decide no usarlo, no debe hacer referencia a la dirección URL de "signalr/hubs" en un `script` elemento en el código de cliente.

<a id="clientsetup"></a>

## <a name="client-setup"></a>Programa de instalación de cliente

Un cliente de JavaScript requiere referencias a jQuery y el archivo de JavaScript de SignalR core. Debe ser la versión de jQuery 1.6.4 o versiones posteriores importantes, como 1.9.1, 1.8.2 o 1.7.2. Si decide usar al proxy generado, también necesita una referencia al proxy de SignalR genera archivos de JavaScript. El ejemplo siguiente muestra el aspecto que es posible que las referencias en una página HTML que usa al proxy generado.

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

Estas referencias deben incluirse en este orden: jQuery, SignalR core después de eso y servidores proxy de SignalR apellidos.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Cómo se hacen referencia al proxy generado dinámicamente

En el ejemplo anterior, la referencia al proxy de SignalR generado es código JavaScript generado dinámicamente, no a un archivo físico. SignalR crea el código de JavaScript para el proxy sobre la marcha y sirve al cliente en respuesta a la dirección URL "/ signalr/hubs". Si especifica una dirección URL base diferente para las conexiones SignalR en el servidor en su `MapSignalR` método, la dirección URL del archivo de proxy generada dinámicamente es la dirección URL personalizada con "/ hubs" anexado a él.

> [!NOTE]
> Para los clientes de JavaScript de Windows 8 (Windows Store), use el archivo de proxy físico en lugar del generada dinámicamente. Para obtener más información, consulte [cómo crear un archivo físico para SignalR genera proxy](#manualproxy) más adelante en este tema.


En ASP.NET MVC 4 o 5 vista de Razor, use la tilde para hacer referencia a la raíz de la aplicación en la referencia del archivo de proxy:

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

Para obtener más información sobre el uso de SignalR en MVC 5, consulte [Introducción a SignalR y MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

En una vista de Razor de ASP.NET MVC 3, use `Url.Content` para su referencia de archivo de proxy:

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

En una aplicación de formularios Web Forms de ASP.NET, utilice `ResolveClientUrl` para servidores proxy de la referencia de archivo o registrarlo mediante ScriptManager mediante una ruta relativa de raíz de aplicación (empezando por una tilde):

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

Como norma general, utilice el mismo método para especificar la dirección URL "/ signalr/hubs" que usa para los archivos CSS o JavaScript. Si especifica una dirección URL sin utilizar una tilde, en algunos escenarios de la aplicación funcionará correctamente cuando se prueba en Visual Studio con IIS Express, pero se producirá un error 404 cuando se implementa en IIS completo. Para obtener más información, consulte **resolver referencias a los recursos de nivel de raíz** en [servidores Web en Visual Studio para proyectos Web de ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) en el sitio MSDN.

Cuando se ejecuta un proyecto web en Visual Studio 2013 en modo de depuración y, si usa Internet Explorer como explorador, puede ver el archivo de proxy en **el Explorador de soluciones** en **documentos de Script**, tal y como se muestra en el ilustración siguiente.

![Archivo de proxy generada de JavaScript en el Explorador de soluciones](hubs-api-guide-javascript-client/_static/image1.png)

Para ver el contenido del archivo, haga doble clic en **hubs**. Si no está utilizando Internet Explorer y Visual Studio 2012 o 2013, o si no está en modo de depuración, también puede obtener el contenido del archivo yendo a la dirección URL "/ signalR/hubs". Por ejemplo, si el sitio se ejecuta en `http://localhost:56699`, vaya a `http://localhost:56699/SignalR/hubs` en el explorador.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Cómo crear un archivo físico para SignalR genera proxy

Como alternativa para el proxy generado de forma dinámica, puede crear un archivo físico que tiene el código proxy y hacer referencia a ese archivo. Puede hacer un control sobre el almacenamiento en caché o su comportamiento de agrupación u obtener IntelliSense cuando codifique las llamadas a métodos de servidor.

Para crear un archivo de proxy, realice los pasos siguientes:

1. Instalar el [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) paquete NuGet.
2. Abra un símbolo del sistema y vaya a la *herramientas* carpeta que contiene el archivo SignalR.exe. La carpeta de herramientas está en la siguiente ubicación:

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. Escriba el comando siguiente:

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    La ruta de acceso a su *.dll* suele ser el *bin* carpeta en la carpeta del proyecto.

    Este comando crea un archivo denominado *server.js* en la misma carpeta que *signalr.exe*.
4. Coloque el *server.js* de archivos en una carpeta adecuada en el proyecto, cambie su nombre según corresponda para su aplicación y agregue una referencia a ella en lugar de la referencia "signalr/hubs".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Cómo establecer una conexión

Antes de establecer una conexión, tendrá que crear un objeto de conexión, crear a un proxy y registrar los controladores de eventos para los métodos que pueden llamarse desde el servidor. Cuando se configuran el proxy y controladores de eventos, establecer la conexión mediante una llamada a la `start` método.

Si usa al proxy generado, no tienes que crear el objeto de conexión en su propio código porque el código proxy generado lo hace por usted.

<a id="nogenconnection"></a>

**Establecer una conexión (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Establecer una conexión (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

El código de ejemplo usa el valor predeterminado "/ signalr" dirección URL para conectarse a su servicio SignalR. Para obtener información sobre cómo especificar una dirección URL base diferente, consulte [Guía de la API de ASP.NET SignalR Hubs - Server - la dirección URL de /signalr](hubs-api-guide-server.md#signalrurl).

De forma predeterminada, la ubicación del concentrador es el servidor actual; Si se conecta a un servidor diferente, especifique la dirección URL antes de llamar a la `start` método, como se muestra en el ejemplo siguiente:

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> Normalmente registra controladores de eventos antes de llamar a la `start` método para establecer la conexión. Si desea registrar algunos controladores de eventos después de establecer la conexión, puede hacerlo, pero debe registrar al menos uno de los controladores de los eventos antes de llamar a la `start` método. Una razón para esto es que puede haber muchos centros en una aplicación, pero que no desea que activen la `OnConnected` eventos en cada centro si solo va a usar para uno de ellos. Cuando se establece la conexión, la presencia de un método de cliente de proxy de un concentrador es que le indica a SignalR para desencadenar la `OnConnected` eventos. Si no registra los controladores de eventos antes de llamar a la `start` método, podrá invocar métodos en el concentrador, pero el concentrador `OnConnected` no se llamará al método y no se invocará ningún método de cliente desde el servidor.


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$. connection.hub es el mismo objeto crea ese $.hubConnection()

Como puede ver en los ejemplos, cuando se usa el proxy generado, `$.connection.hub` hace referencia al objeto de conexión. Este es el mismo objeto que se obtiene mediante una llamada a `$.hubConnection()` cuando no está usando el proxy generado. El código proxy generado, crea la conexión automáticamente al ejecutar la instrucción siguiente:

![Crear una conexión en el archivo de proxy generada](hubs-api-guide-javascript-client/_static/image3.png)

Cuando se usa el proxy generado, puede hacer nada con `$.connection.hub` que puede hacer con un objeto de conexión cuando no usa el proxy generado.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Ejecución asincrónica del método de inicio

El `start` método se ejecuta de forma asincrónica. Devuelve un [jQuery aplazado objeto](http://api.jquery.com/category/deferred-object/), lo que significa que puede agregar funciones de devolución de llamada llamando a métodos como `pipe`, `done`, y `fail`. Si tiene código que desea ejecutar una vez establecida la conexión, como una llamada a un método de servidor, agregue el código en una función de devolución de llamada o llamarlo desde una función de devolución de llamada. El `.done` método de devolución de llamada se ejecuta después de que se ha establecido la conexión y, después de cualquier código que tiene en su `OnConnected` método de controlador de eventos en el servidor termina de ejecutarse.

Si coloca la instrucción "Ahora conectados" del ejemplo anterior, como la siguiente línea de código después la `start` llamada al método (no en un `.done` devolución de llamada), la `console.log` línea se ejecutará antes de establecer la conexión, como se muestra en la siguiente ejemplo:

![Erróneo para escribir código que se ejecuta una vez establecida la conexión](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Cómo establecer una conexión entre dominios

Normalmente, si el explorador carga una página de `http://contoso.com`, la conexión de SignalR está en el mismo dominio, en `http://contoso.com/signalr`. Si la página de `http://contoso.com` realiza una conexión a `http://fabrikam.com/signalr`, que es una conexión entre dominios. Por motivos de seguridad, las conexiones entre dominios están deshabilitadas de forma predeterminada.

En SignalR 1.x solicitudes entre dominios se controla mediante una única marca EnableCrossDomain. Este indicador controla las solicitudes de JSONP y CORS. Para una mayor flexibilidad, compatibilidad con CORS todos los ha quitado el componente de servidor de SignalR (clientes de JavaScript todavía usan CORS normalmente si se detecta que el explorador lo admite), y middleware OWIN nuevo está disponible admitir estos escenarios.

Si se requiere JSONP en el cliente (para admitir las solicitudes entre dominios en los exploradores más antiguos), deberá habilitarse explícitamente estableciendo `EnableJSONP` en el `HubConfiguration` objeto `true`, tal y como se muestra a continuación. JSONP está deshabilitada de forma predeterminada, ya que es menos segura que la CORS.

**Agregar Microsoft.Owin.Cors a su proyecto:** para instalar esta biblioteca, ejecute el siguiente comando en la consola de administrador de paquetes:

`Install-Package Microsoft.Owin.Cors`

Este comando agregará el 2.1.0 versión del paquete al proyecto.

### <a name="calling-usecors"></a>Llamar a UseCors

 El fragmento de código siguiente muestra cómo implementar las conexiones entre dominios en SignalR 2.

**Implementación de las solicitudes entre dominios en SignalR 2**

El código siguiente muestra cómo se habilita la CORS o JSONP en un proyecto de SignalR 2. Este ejemplo de código usa `Map` y `RunSignalR` en lugar de `MapSignalR`, de modo que el middleware de CORS se ejecuta solo para las solicitudes de SignalR que requieren compatibilidad con CORS (en lugar de para todo el tráfico en la ruta de acceso especificada en `MapSignalR`.) Map también puede usarse para cualquier otro middleware que debe ejecutarse para un prefijo de dirección URL específico, en lugar de toda la aplicación.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE]
>
> - No establezca `jQuery.support.cors` en true en el código.
>
>     ![No establezca jQuery.support.cors en true](hubs-api-guide-javascript-client/_static/image7.png)
>
>     SignalR controla el uso de la CORS. Establecer `jQuery.support.cors` a true deshabilita JSONP porque hace que SignalR asumir el explorador admite la CORS.
> - Cuando se conecta a una dirección URL de localhost, Internet Explorer 10 no considerarlo una conexión entre dominios, por lo que la aplicación funcionará localmente con Internet Explorer 10 incluso si no ha habilitado las conexiones entre dominios en el servidor.
> - Para obtener información sobre el uso de las conexiones entre dominios con Internet Explorer 9, consulte [este hilo](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Para obtener información sobre el uso de las conexiones entre dominios con Chrome, consulte [este hilo](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - El código de ejemplo usa el valor predeterminado "/ signalr" dirección URL para conectarse a su servicio SignalR. Para obtener información sobre cómo especificar una dirección URL base diferente, consulte [Guía de la API de ASP.NET SignalR Hubs - Server - la dirección URL de /signalr](hubs-api-guide-server.md#signalrurl).


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Cómo configurar la conexión

Antes de establecer una conexión, puede especificar parámetros de cadena de consulta o especificar el modo de transporte.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Cómo especificar parámetros de cadena de consulta

Si desea enviar datos al servidor cuando el cliente se conecta, puede agregar parámetros de cadena de consulta para el objeto de conexión. Los ejemplos siguientes muestran cómo establecer un parámetro de cadena de consulta en código de cliente.

**Establecer un valor de cadena de consulta antes de llamar al método start (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

**Establecer un valor de cadena de consulta antes de llamar al método start (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

El ejemplo siguiente muestra cómo leer un parámetro de cadena de consulta en código del servidor.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Cómo especificar el modo de transporte

Como parte del proceso de conexión, un cliente de SignalR normalmente negocia con el servidor para determinar el mejor transporte que se admite por servidor y cliente. Si ya conoce el tipo de transporte que desea usar, puede omitir este proceso de negociación de mediante la especificación del método de transporte cuando se llama a la `start` método.

**Código de cliente que especifica el método de transporte (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

**Código de cliente que especifica el método de transporte (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

Como alternativa, puede especificar varios métodos de transporte en el orden en el que desea SignalR para probarlas:

**Código de cliente que especifica un esquema de reserva de transporte personalizado (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**Código de cliente que especifica un esquema de reserva de transporte personalizado (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

Puede usar los siguientes valores para especificar el modo de transporte:

- "webSockets"
- "foreverFrame"
- "serverSentEvents"
- "longPolling"

Los ejemplos siguientes muestran cómo averiguar qué método de transporte que está usando una conexión.

**Código de cliente que se muestra el método de transporte utilizado por una conexión (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

**Código de cliente que se muestra el método de transporte utilizado por una conexión (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

Para obtener información sobre cómo comprobar el modo de transporte en el código de servidor, consulte [Guía de la API de ASP.NET SignalR Hubs - Server - obtener información sobre el cliente de la propiedad de contexto](hubs-api-guide-server.md#contextproperty). Para obtener más información acerca de los transportes y reservas, consulte [Introducción a SignalR - transportes y reservas](../getting-started/introduction-to-signalr.md#transports).

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Cómo obtener a un proxy para una clase de Hub

Cada objeto de conexión que cree encapsula información sobre una conexión a un servicio de SignalR que contiene una o más clases de concentrador. Para comunicarse con una clase de Hub, use un objeto proxy que cree usted mismo (si no usa al proxy generado) o que se genera automáticamente.

En el cliente el nombre del proxy es una versión de mayúsculas y minúsculas camel del nombre de clase del concentrador. SignalR realiza automáticamente este cambio para que el código JavaScript puede cumplir las convenciones de JavaScript.

**Clase de Hub en servidor**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

**Obtener una referencia al proxy de cliente generado para el concentrador**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

**Crear el proxy de cliente para la clase Hub (sin proxy generado)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

Si decorar la clase de Hub con un `HubName` atributo, use el nombre exacto sin cambiar mayúsculas y minúsculas.

**Clase de Hub en el servidor con el atributo HubName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

**Obtener una referencia al proxy de cliente generado para el concentrador**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

**Crear el proxy de cliente para la clase Hub (sin proxy generado)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Cómo definir métodos en el cliente que el servidor puede llamar a

Para definir un método que el servidor puede llamar a un concentrador, agregar un controlador de eventos para el proxy de concentrador mediante el `client` propiedad de la llamada o proxy generado el `on` método si no usa el proxy generado. Los parámetros pueden ser objetos complejos.

Agregar el controlador de eventos antes de llamar a la `start` método para establecer la conexión. (Si desea agregar controladores de eventos después de llamar a la `start` método, vea la nota de [cómo establecer una conexión](#establishconnection) anteriormente en este documento y utilice la sintaxis mostrada para definir un método sin usar el proxy generado.)

Coincidencia de nombres de método distingue mayúsculas de minúsculas. Por ejemplo, `Clients.All.addContosoChatMessageToPage` se ejecutará en el servidor `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, o `addcontosochatmessagetopage` en el cliente.

**Definir el método en el cliente (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

**Forma alternativa para definir el método de cliente (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

**Definir el método en el cliente (sin el proxy generado, o cuando se agrega después de llamar al método start)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

**Código de servidor que llama al método de cliente**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

Los ejemplos siguientes incluyen un objeto complejo como un parámetro de método.

**Definir el método en el cliente que toma un objeto complejo (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

**Definir el método en el cliente que toma un objeto complejo (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

**Código de servidor que define el objeto complejo**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

**Código de servidor que llama al método de cliente mediante un objeto complejo**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Cómo llamar a métodos del servidor desde el cliente

Para llamar a un método de servidor desde el cliente, use el `server` propiedad del proxy generado o `invoke` método en el proxy de concentrador, si no usa el proxy generado. El valor devuelto o los parámetros pueden ser objetos complejos.

Pasar de una versión de mayúscula y minúscula camel del nombre del método del concentrador. SignalR realiza automáticamente este cambio para que el código JavaScript puede cumplir las convenciones de JavaScript.

Los ejemplos siguientes muestran cómo llamar a un método de servidor que no tiene un valor devuelto y cómo llamar a un método de servidor que tiene un valor devuelto.

**Método de servidor con ningún atributo HubMethodName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

**Código de servidor que define el objeto complejo que se pasa en un parámetro**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

**Código de cliente que invoca el método de servidor (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

**Código de cliente que invoca el método de servidor (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

Si decora el método de concentrador con un `HubMethodName` atributo, use ese nombre sin cambiar mayúsculas y minúsculas.

**Método de servidor** con un atributo HubMethodName

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

**Código de cliente que invoca el método de servidor (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

**Código de cliente que invoca el método de servidor (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

Los ejemplos anteriores muestran cómo llamar a un método de servidor que no tiene ningún valor devuelto. Los ejemplos siguientes muestran cómo llamar a un método de servidor que tiene un valor devuelto.

**Código de servidor para un método que tiene un valor devuelto**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

**La clase de acción utilizada para el** devolver valor

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

**Código de cliente que invoca el método de servidor (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

**Código de cliente que invoca el método de servidor (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Cómo controlar eventos de duración de la conexión

SignalR proporciona la siguiente conexión de eventos de duración que puede controlar:

- `starting`: Se genera antes de que se envían los datos a través de la conexión.
- `received`: Se genera cuando se recibe ningún dato en la conexión. Proporciona los datos recibidos.
- `connectionSlow`: Se genera cuando el cliente detecta una conexión lenta o eliminación con frecuencia.
- `reconnecting`: Se genera cuando comienza a volver a conectar el transporte subyacente.
- `reconnected`: Se genera cuando se ha vuelto a conectar el transporte subyacente.
- `stateChanged`: Se genera cuando cambia el estado de conexión. Proporciona el estado antiguo y el estado nueva (conexión, conectado, volviendo a conectarse o desconectado).
- `disconnected`: Se genera cuando se ha desconectado la conexión.

Por ejemplo, si desea mostrar mensajes de advertencia cuando hay problemas de conexión que pueden provocar retrasos evidentes, controlar el `connectionSlow` eventos.

**Controlar el evento connectionSlow (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

**Controlar el evento connectionSlow (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

Para obtener más información, consulte [comprensión y control de eventos de duración de conexión en SignalR](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Cómo controlar los errores

El cliente de JavaScript de SignalR proporciona una `error` eventos que se puede agregar un controlador para. También puede usar el método fail para agregar un controlador de errores que resultan de una invocación de método del servidor.

Si no habilita explícitamente los mensajes de error detallados en el servidor, el objeto de excepción que SignalR devuelve después de un error contiene información mínima sobre el error. Por ejemplo, si una llamada a `newContosoChatMessage` se produce un error, el mensaje de error en el objeto de error contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" enviar mensajes de error detallados a los clientes de producción no se recomienda por motivos de seguridad, pero si desea permitir que los mensajes de error detallados para solucionar problemas, use el código siguiente en el servidor.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

El ejemplo siguiente muestra cómo agregar un controlador para el evento de error.

**Agregar un controlador de errores (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

**Agregar un controlador de errores (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

El ejemplo siguiente muestra cómo controlar un error de una invocación de método.

**Controla un error de una invocación de método (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

**Controla un error de una invocación de método (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

Si se produce un error en una invocación de método, el `error` también se genera el evento, por lo que el código en el `error` controlador de método y en el `.fail` ejecutaría la devolución de llamada de método.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Cómo habilitar el registro del lado cliente

Para habilitar el registro de cliente en una conexión, establezca el `logging` propiedad del objeto de conexión antes de llamar a la `start` método para establecer la conexión.

**Habilitación del registro (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

**Habilitar el registro (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

Para ver los registros, abrir las herramientas de desarrollador de su explorador y vaya a la pestaña de la consola. Para ver un tutorial que muestra instrucciones paso a paso y pantalla capturas que muestran cómo hacerlo, consulte [difusión de servidores con ASP.NET Signalr - habilitar el registro de](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).
