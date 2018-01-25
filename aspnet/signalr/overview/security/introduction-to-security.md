---
uid: signalr/overview/security/introduction-to-security
title: "Introducción a la seguridad de SignalR | Documentos de Microsoft"
author: pfletcher
description: "Describe los problemas de seguridad que debe tener en cuenta al desarrollar una aplicación de SignalR."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: ed562717-8591-4936-8e10-c7e63dcb570a
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: 1cb9f15a958028822b50decf4b420c36596ce25e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-signalr-security"></a>Introducción a la seguridad de SignalR
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> Este artículo describen los problemas de seguridad que debe tener en cuenta al desarrollar una aplicación de SignalR. 
> 
> ## <a name="software-versions-used-in-this-topic"></a>Versiones de software que se usa en este tema
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versión 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Versiones anteriores de este tema
> 
> Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Información general

Este documento contiene las siguientes secciones:

- [Conceptos de seguridad de SignalR](#concepts)

    - [Autenticación y autorización](#authentication)
    - [Token de conexión](#connectiontoken)
    - [Nueva unión a grupos cuando vuelve a conectar](#rejoingroup)
- [¿Cómo SignalR evita la falsificación de solicitud entre sitios](#csrf)
- [Recomendaciones de seguridad de SignalR](#recommendations)

    - [Protocolo seguro de las capas de Sockets (SSL)](#ssl)
    - [No use los grupos como un mecanismo de seguridad](#groupsecurity)
    - [Controlar de forma segura entrada de los clientes](#input)
    - [Reconciliar un cambio en el estado de usuario con una conexión activa](#reconcile)
    - [Archivos de proxy de JavaScript generados automáticamente](#autogen)
    - [Excepciones](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>Conceptos de seguridad de SignalR

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>Autenticación y autorización

SignalR no proporciona características para autenticar a los usuarios. En su lugar, integrar las características de SignalR en la estructura de autenticación existente de una aplicación. Autenticar a los usuarios como haría normalmente en la aplicación y trabajar con los resultados de la autenticación en su SignalR el código. Por ejemplo, puede autenticar a los usuarios con autenticación de formularios ASP.NET y, a continuación, en el centro, exigir que los usuarios o roles están autorizados para llamar a un método. En el centro, también puede pasar información de autenticación, como el nombre de usuario o si un usuario pertenece a un rol, al cliente.

SignalR proporciona el [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) atributo para especificar qué usuarios tienen acceso a un concentrador o un método. Aplicar el atributo de autorizar a un concentrador o determinados métodos en un concentrador. Sin el atributo Authorize, todos los métodos públicos en el concentrador están disponibles para un cliente que está conectado al concentrador. Para obtener más información acerca de los concentradores, vea [autenticación y autorización para los concentradores de SignalR](hub-authorization.md).

Aplicar el `Authorize` atribuir a los concentradores, pero las conexiones no persistentes. Para aplicar las reglas de autorización cuando se usa un `PersistentConnection` debe invalidar el `AuthorizeRequest` método. Para obtener más información acerca de las conexiones persistentes, consulte [autenticación y autorización para las conexiones persistentes de SignalR](persistent-connection-authorization.md).

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Token de conexión

SignalR reduce el riesgo de ejecutar comandos malintencionados mediante la validación de la identidad del remitente. Para cada solicitud, el cliente y servidor pasan un token de conexión que contiene el identificador de conexión y el nombre de usuario para los usuarios autenticados. El identificador de conexión identifica de forma única cada cliente conectado. El servidor genera aleatoriamente el identificador de conexión cuando se crea una nueva conexión y conserva ese identificador para la duración de la conexión. El mecanismo de autenticación para la aplicación web proporciona el nombre de usuario. SignalR usa una firma digital y cifrado para proteger el token de conexión.

![](introduction-to-security/_static/image2.png)

Para cada solicitud, el servidor valida el contenido del token para asegurarse de que la solicitud procede del usuario especificado. El nombre de usuario debe corresponderse con el identificador de conexión. Al validar el identificador de conexión y el nombre de usuario, SignalR impide que un usuario malintencionado fácilmente suplantación de otro usuario. Si el servidor no puede validar el token de conexión, se produce un error en la solicitud.

![](introduction-to-security/_static/image4.png)

Dado que el identificador de conexión es parte del proceso de comprobación, no debe mostrar el identificador de conexión de un usuario a otros usuarios ni almacenar el valor en el cliente, como en una cookie.

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>Nueva unión a grupos cuando vuelve a conectar

De forma predeterminada, la aplicación de SignalR volver a asignará automáticamente un usuario a los grupos correspondientes cuando vuelve a conectar desde una interrupción temporal, por ejemplo, cuando una conexión se quita y se restablece antes agote el tiempo de espera de la conexión. Cuando vuelve a conectar, el cliente pasa un símbolo (token) de grupo que incluye el identificador de conexión y los grupos asignados. El token de grupo esté firmado y cifrado. El cliente conserva el mismo identificador de conexión después de una reconexión; por lo tanto, el identificador de conexión que se pasa desde el cliente ha vuelto a conectar debe coincidir con el identificador de conexión anterior usado por el cliente. Esta comprobación impide que un usuario malintencionado pasar solicitudes para unirse a grupos no autorizados cuando vuelve a conectar.

Sin embargo, es importante tener en cuenta que el token de grupo no caduca. Si un usuario pertenecía a un grupo en el pasado, pero se prohibió de ese grupo, ese usuario posible imitar un símbolo (token) de grupo que incluye el grupo prohibido. Si tiene que administrar los usuarios que pertenecen a los grupos de forma segura, debe almacenar los datos en el servidor, como en una base de datos. A continuación, agregar lógica a la aplicación que se comprueba en el servidor si un usuario pertenece a un grupo. Para obtener un ejemplo de comprobación de pertenencia a grupos, consulte [trabajar con grupos](../guide-to-the-api/working-with-groups.md).

Automáticamente nueva unión a grupos solo se aplica cuando se vuelve a conectar una conexión después de una interrupción temporal. Si un usuario se desconecta por si sale de la aplicación o se reinicie la aplicación, la aplicación debe controlar cómo agregar ese usuario a los grupos correctos. Para obtener más información, consulte [trabajar con grupos](../guide-to-the-api/working-with-groups.md).

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>¿Cómo SignalR evita la falsificación de solicitud entre sitios

Falsificación de solicitud entre sitios (CSRF) es un ataque en un sitio malintencionado envía una solicitud a un sitio vulnerable donde el usuario actualmente ha iniciado sesión. SignalR evita CSRF realizando muy poco probable de un sitio malintencionado para crear una solicitud válida para la aplicación de SignalR.

### <a name="description-of-csrf-attack"></a>Descripción de ataque CSRF

Este es un ejemplo de un ataque CSRF:

1. Un usuario inicia sesión en www.example.com, utilizando la autenticación de formularios.
2. El servidor autentica al usuario. La respuesta del servidor incluye una cookie de autenticación.
3. Sin cerrar la sesión, el usuario visita un sitio web malintencionado. Este sitio malintencionado contiene el siguiente formulario HTML: 

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

 Tenga en cuenta que la acción de formulario se envía al sitio vulnerable, no en el sitio malintencionado. Esta es la parte "cross-site" de CSRF.
4. El usuario hace clic en el botón Enviar. El explorador incluye la cookie de autenticación con la solicitud.
5. La solicitud se ejecuta en el servidor de ejemplo.com con contexto de autenticación del usuario y puede hacer todo lo que puede hacer un usuario autenticado.

Aunque este ejemplo requiere que el usuario haga clic en el botón del formulario, la página malintencionada podría tal y como ejecutar fácilmente un script que envía una solicitud AJAX a la aplicación de SignalR. Además, con SSL no evita que un ataque CSRF, dado que el sitio malintencionado puede enviar una solicitud de "https://".

Normalmente, los ataques CSRF son posibles con sitios web que usan cookies para la autenticación, porque los exploradores envían todas las cookies relevantes para el sitio web de destino. Sin embargo, los ataques CSRF no están limitados a aprovecharse de las cookies. Por ejemplo, la autenticación básica e implícita también son vulnerables. Una vez que un usuario inicia sesión con autenticación básica o implícita, el explorador envía automáticamente las credenciales hasta que finaliza la sesión.

### <a name="csrf-mitigations-taken-by-signalr"></a>Mitigaciones CSRF realizadas por SignalR

SignalR realiza los pasos siguientes para impedir que un sitio malintencionado crear solicitudes válidas para la aplicación. SignalR lleva a cabo estos pasos de forma predeterminada, no es necesario realizar ninguna acción en el código.

- **Deshabilitar las solicitudes entre dominios**  
 SignalR deshabilita las solicitudes entre dominios para evitar que los usuarios de la llamada a un punto de conexión de SignalR de un dominio externo. SignalR considera que cualquier solicitud de un dominio externo para que no sean válidos y bloquea las solicitudes. Se recomienda que mantenga este comportamiento predeterminado; en caso contrario, un sitio malintencionado podría engañar a los usuarios enviar comandos a su sitio. Si necesita utilizar solicitudes entre dominios, consulte [cómo establecer una conexión entre dominios](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .
- **Pasar el token de conexión en la cadena de consulta, no cookie**  
 SignalR pasa el token de conexión como un valor de cadena de consulta, en lugar de como una cookie. Almacenar el token de conexión en una cookie no es seguro porque el explorador sin darse cuenta puede reenviar el token de conexión cuando se encuentra el código malintencionado. También, al pasar el token de conexión en la cadena de consulta, evita el token de conexión conservación más allá de la conexión actual. Por lo tanto, un usuario malintencionado no puede realizar una solicitud con credenciales de autenticación de otro usuario.
- **Comprobar el token de conexión**  
 Como se describe en el [token de conexión](#connectiontoken) sección, el servidor sabe qué Id. de conexión está asociado a cada usuario autenticado. El servidor no procesa ninguna solicitud de un identificador de conexión que no coincide con el nombre de usuario. Es poco probable que un usuario malintencionado podría suponer una solicitud válida porque el usuario malintencionado tendría que conocer el nombre de usuario y el identificador de conexión generada de forma aleatoria actual. Ese identificador de conexión deja de ser válido en cuanto finaliza la conexión. Los usuarios anónimos no deben tener acceso a información confidencial.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>Recomendaciones de seguridad de SignalR

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>Protocolo seguro de las capas de Sockets (SSL)

El protocolo SSL usa el cifrado para proteger el transporte de datos entre un cliente y servidor. Si la aplicación de SignalR transmite información confidencial entre el cliente y el servidor, use SSL para el transporte. Para obtener más información acerca de cómo configurar SSL, consulte [cómo configurar SSL en IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>No use los grupos como un mecanismo de seguridad

Los grupos son una forma cómoda de recopilación de usuarios relacionados, pero no son un mecanismo de seguridad para limitar el acceso a información confidencial. Esto es especialmente cierto cuando los usuarios pueden volver a unirse automáticamente grupos durante una reconexión. En su lugar, considere la posibilidad de agregar los usuarios con privilegios a un rol y limitar el acceso a un método de concentrador a solo los miembros de ese rol. Para obtener un ejemplo de restringir el acceso basándose en un rol, consulte [autenticación y autorización para los concentradores de SignalR](hub-authorization.md). Para obtener un ejemplo de comprobación de acceso de usuario a grupos cuando vuelve a conectar, vea [trabajar con grupos](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>Controlar de forma segura entrada de los clientes

Para asegurarse de que un usuario malintencionado no envía el script a otros usuarios, primero debe codificar todas las entradas de los clientes que está diseñada para la difusión a otros clientes. Debe codificar los mensajes en los clientes de recepción en lugar de en el servidor, ya que la aplicación de SignalR puede tener muchos tipos diferentes de los clientes. Por lo tanto, la codificación HTML funciona para un cliente web, pero no para otros tipos de clientes. Por ejemplo, un método de cliente web para mostrar un mensaje de chat con seguridad controlaría el nombre de usuario y el mensaje mediante una llamada a la `html()` (función).

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>Reconciliar un cambio en el estado de usuario con una conexión activa

Si el estado de autenticación de un usuario cambia mientras existe una conexión activa, el usuario recibirá un error que indica, "la identidad del usuario no puede cambiar durante una conexión de SignalR activa". En ese caso, la aplicación vuelve a debe conectarse al servidor para asegurarse de que se coordinen el identificador de conexión y el nombre de usuario. Por ejemplo, si la aplicación permite al usuario cerrar la sesión mientras existe una conexión activa, el nombre de usuario para la conexión ya no coincidirá con el nombre que se ha pasado para la solicitud siguiente. Se desea detener la conexión antes de que el usuario cierre sesión y, a continuación, reiniciarlo.

Sin embargo, es importante tener en cuenta que la mayoría de las aplicaciones no necesitarán detener e iniciar la conexión de forma manual. Si la aplicación redirige a los usuarios a otra página después de cerrar la sesión, por ejemplo, el comportamiento predeterminado en una aplicación de formularios Web Forms o aplicación de MVC o actualiza la página actual después de cerrar la sesión, la conexión activa se desconecta automáticamente y no lo hace requerir ninguna acción adicional.

En el ejemplo siguiente se muestra cómo detener e iniciar una conexión cuando ha cambiado el estado de usuario.

[!code-html[Main](introduction-to-security/samples/sample3.html)]

O bien, puede cambiar el estado de autenticación del usuario si el sitio usa la expiración variable con autenticación de formularios, y no hay ninguna actividad para mantener la cookie de autenticación válida. En ese caso, el usuario cerrará la sesión y el nombre de usuario ya no coincidirá con el nombre de usuario en el token de conexión. Puede corregir este problema mediante la adición de un script que periódicamente se solicita un recurso en el servidor web para mantener la cookie de autenticación válida. En el ejemplo siguiente se muestra cómo solicitar un recurso cada 30 minutos.

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>Archivos de proxy de JavaScript generados automáticamente

Si no desea incluir todos los concentradores y métodos en el archivo de proxy de JavaScript para cada usuario, puede deshabilitar la generación automática del archivo. Puede elegir esta opción si tiene varios concentradores y métodos, pero no desea que todos los usuarios a tener en cuenta todos los métodos. Deshabilitar la generación automática estableciendo **EnableJavaScriptProxies** a **false**.

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

Para obtener más información acerca de los archivos de proxy de JavaScript, consulte [el proxy generado y lo que hace que](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy). <a id="exceptions"></a>

### <a name="exceptions"></a>Excepciones

Debe evitar pasar objetos de excepción a los clientes porque los objetos pueden exponer información confidencial a los clientes. En su lugar, llame a un método en el cliente que muestra el mensaje de error relevantes.

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
