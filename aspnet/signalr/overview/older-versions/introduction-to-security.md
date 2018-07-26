---
uid: signalr/overview/older-versions/introduction-to-security
title: Introducción a la seguridad de SignalR (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: Describe los problemas de seguridad que debe tener en cuenta al desarrollar una aplicación de SignalR.
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: 715a4059-d307-4631-abbb-c789c95d6eb4
msc.legacyurl: /signalr/overview/older-versions/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: cb705ccb6052297d0214deeaaeb8181e283245f3
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228564"
---
<a name="introduction-to-signalr-security-signalr-1x"></a>Introducción a la seguridad de SignalR (SignalR 1.x)
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se describe los problemas de seguridad que debe tener en cuenta al desarrollar una aplicación de SignalR.


## <a name="overview"></a>Información general

Este documento contiene las siguientes secciones:

- [Conceptos de seguridad de SignalR](#concepts)

    - [Autenticación y autorización](#authentication)
    - [Token de conexión](#connectiontoken)
    - [Unir a grupos al volver a conectarse](#rejoingroup)
- [¿Cómo evita la falsificación de solicitud entre sitios SignalR](#csrf)
- [Recomendaciones de seguridad de SignalR](#recommendations)

    - [Protocolo seguro de las capas de Sockets (SSL)](#ssl)
    - [No use grupos como un mecanismo de seguridad](#groupsecurity)
    - [Controlar entrada desde clientes de forma segura](#input)
    - [Reconciliar un cambio en el estado de usuario con una conexión activa](#reconcile)
    - [Archivos del proxy de JavaScript generados automáticamente](#autogen)
    - [Excepciones](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>Conceptos de seguridad de SignalR

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>Autenticación y autorización

SignalR está diseñado para integrarse en la estructura de autenticación existente de una aplicación. No proporciona ninguna característica para autenticar a los usuarios. En su lugar, autenticar a los usuarios como lo haría normalmente en la aplicación y, a continuación, trabaja con los resultados de la autenticación en el código de SignalR. Por ejemplo, puede autenticar a los usuarios con autenticación de formularios ASP.NET y, a continuación, en su centro, exigir que los usuarios o roles están autorizados para llamar a un método. En su centro, también se puede pasar información de autenticación, como nombre de usuario o si un usuario pertenece a un rol, al cliente.

SignalR proporciona el [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) atributo para especificar qué usuarios tienen acceso a un concentrador o un método. Aplicar el atributo Authorize a un concentrador o determinados métodos en un concentrador. Sin el atributo Authorize, todos los métodos públicos en el centro están disponibles para un cliente que está conectado al concentrador. Para obtener más información sobre los concentradores, vea [autenticación y autorización para los concentradores de SignalR](../security/hub-authorization.md).

El `Authorize` atributo se usa solo con los concentradores. Para aplicar las reglas de autorización cuando se usa un `PersistentConnection` debe invalidar el `AuthorizeRequest` método. Para obtener más información acerca de las conexiones persistentes, consulte [autenticación y autorización para las conexiones persistentes de SignalR](../security/persistent-connection-authorization.md).

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Token de conexión

SignalR reduce el riesgo de ejecutar comandos malintencionados al validar la identidad del remitente. Un token de conexión, que contiene el identificador de conexión y el nombre de usuario para los usuarios autenticados, se pasa entre el cliente y servidor para cada solicitud. El identificador de conexión es un identificador único que se genera aleatoriamente por el servidor cuando se crea una nueva conexión y se conserva para la duración de la conexión. El nombre de usuario proporciona el mecanismo de autenticación para la aplicación web. El token de conexión está protegido con cifrado y una firma digital.

![](introduction-to-security/_static/image2.png)

Para cada solicitud, el servidor valida el contenido del token para asegurarse de que la solicitud procede del usuario especificado. El nombre de usuario debe corresponder con el identificador de conexión. Al validar el identificador de conexión y el nombre de usuario, SignalR impide que un usuario malintencionado fácilmente suplantación de otro usuario. Si el servidor no puede validar el token de conexión, se produce un error en la solicitud.

![](introduction-to-security/_static/image4.png)

Dado que el identificador de conexión es parte del proceso de comprobación, no debe revelar el identificador de conexión de un usuario a otros usuarios o almacenar el valor en el cliente, como en una cookie.

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>Unir a grupos al volver a conectarse

De forma predeterminada, la aplicación de SignalR volver a asignará automáticamente un usuario a los grupos adecuados cuando vuelva a conectar desde una interrupción temporal, por ejemplo, cuando una conexión se quita y vuelve a establecer antes de la conexión agota el tiempo. Al volver a conectarse, el cliente pasa un token de grupo que incluye el identificador de conexión y los grupos asignados. El token de grupo esté firmado y cifrado. El cliente conserva el mismo identificador de conexión después de una reconexión; por lo tanto, el identificador de conexión que se pasa desde el cliente ha vuelto a conectar debe coincidir con el identificador de conexión anterior utilizado por el cliente. Esta comprobación impide que un usuario malintencionado pasar solicitudes para unirse a grupos no autorizados al conectarse de nuevo.

Sin embargo, es importante tener en cuenta que no expira el token de grupo. Si un usuario pertenecía a un grupo en el pasado, pero se prohibió de ese grupo, ese usuario puede imitar un token de grupo que incluye el grupo prohibido. Si necesita administrar los usuarios que pertenecen a qué grupos de forma segura, deberá almacenar esos datos en el servidor, como en una base de datos. A continuación, agregar lógica a la aplicación que se comprueba en el servidor si un usuario pertenece a un grupo. Para obtener un ejemplo de la comprobación de pertenencia a grupos, consulte [trabajar con grupos](../guide-to-the-api/working-with-groups.md).

Unir automáticamente a los grupos solo se aplica cuando se vuelve a conectar una conexión después de una interrupción temporal. Si un usuario se desconecta por salir de la aplicación o se reinicie la aplicación, la aplicación debe controlar cómo agregar ese usuario a los grupos correctos. Para obtener más información, consulte [trabajar con grupos](../guide-to-the-api/working-with-groups.md).

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>¿Cómo evita la falsificación de solicitud entre sitios SignalR

Falsificación de solicitud entre sitios (CSRF) es un ataque en un sitio malintencionado envía una solicitud a un sitio vulnerable donde el usuario ha iniciado en. SignalR evita CSRF realizando muy poco probable de un sitio malintencionado para crear una solicitud válida para la aplicación de SignalR.

### <a name="description-of-csrf-attack"></a>Descripción de ataque CSRF

Este es un ejemplo de un ataque CSRF:

1. Un usuario inicia sesión en `www.example.com`, utilizando la autenticación de formularios.
2. El servidor autentica al usuario. La respuesta del servidor incluye una cookie de autenticación.
3. Sin cerrar la sesión, el usuario visita un sitio web malicioso. Este sitio malintencionado contiene el siguiente formulario HTML: 

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

   Tenga en cuenta que la acción de formulario se publica en el sitio vulnerable, no en el sitio malintencionado. Esta es la parte "cross-site" de CSRF.
4. El usuario hace clic con el botón Enviar. El explorador incluye la cookie de autenticación con la solicitud.
5. La solicitud se ejecuta en el servidor de ejemplo.com con contexto de autenticación del usuario y puede hacer cualquier cosa que puede hacer un usuario autenticado.

Aunque este ejemplo requiere que el usuario haga clic en el botón del formulario, la página malintencionada podría tal como ejecutar fácilmente una secuencia de comandos que envía una solicitud AJAX a la aplicación de SignalR. Además, con SSL no impide que un ataque CSRF, dado que el sitio Web malintencionado puede enviar una solicitud de "https://".

Normalmente, los ataques CSRF son posibles con sitios web que usan cookies para la autenticación, porque los exploradores envían todas las cookies relevantes al sitio web de destino. Sin embargo, los ataques CSRF no están limitados a aprovechar las cookies. Por ejemplo, la autenticación básica e implícita también son vulnerables. Después de que un usuario inicia sesión con la autenticación básica o implícita, el explorador envía automáticamente las credenciales hasta que finaliza la sesión.

### <a name="csrf-mitigations-taken-by-signalr"></a>Mitigaciones de CSRF realizadas por SignalR

SignalR realiza los pasos siguientes para evitar que un sitio malintencionado creen solicitudes válidas para la aplicación de SignalR. Estos pasos se realizan de forma predeterminada y no requieren ninguna acción en el código.

- **Deshabilitar las solicitudes entre dominios**  
 De forma predeterminada, entre dominios se deshabilitan las solicitudes en una aplicación de SignalR para evitar que los usuarios de la llamada a un punto de conexión SignalR desde un dominio externo. Cualquier solicitud que procede de un dominio externo automáticamente se considera no válida y se bloquea. Se recomienda que mantenga este comportamiento predeterminado; en caso contrario, un sitio malintencionado podría engañar a los usuarios enviar comandos a su sitio. Si necesita usar solicitudes entre dominios, consulte [cómo establecer una conexión entre dominios](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .
- **Pasar el token de conexión en la cadena de consulta, no en cookies**  
 SignalR pasa el token de conexión como un valor de cadena de consulta, en lugar de como una cookie. Si no almacena el token de conexión como una cookie, el token de conexión no accidentalmente reenvía el explorador cuando se encuentra el código malintencionado. Además, el token de conexión no se conserva más allá de la conexión actual. Por lo tanto, un usuario malintencionado no puede realizar una solicitud con credenciales de autenticación de otro usuario.
- **Comprobar el token de conexión**  
 Como se describe en el [token de conexión](#connectiontoken) sección, el servidor sabe qué identificador de conexión está asociado a cada usuario autenticado. El servidor no procesa cualquier solicitud de un identificador de conexión que no coincide con el nombre de usuario. No es probable que un usuario malintencionado pueda adivinar una solicitud válida porque el usuario malintencionado tendría que conocer el nombre de usuario y el identificador de conexión generada aleatoriamente actual. Id. de esa conexión deja de ser válido tan pronto como finaliza la conexión. Los usuarios anónimos no deben tener acceso a información confidencial.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>Recomendaciones de seguridad de SignalR

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>Protocolo seguro de las capas de Sockets (SSL)

El protocolo SSL usa cifrado para proteger el transporte de datos entre un cliente y servidor. Si la aplicación de SignalR transmite información confidencial entre el cliente y servidor, use SSL para el transporte. Para obtener más información acerca de cómo configurar SSL, consulte [cómo configurar SSL en IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>No use grupos como un mecanismo de seguridad

Los grupos son una manera cómoda de recopilación de usuarios relacionados, pero no son un mecanismo seguro para limitar el acceso a información confidencial. Esto es especialmente cierto cuando los usuarios pueden volver a unir automáticamente grupos durante una reconexión. En su lugar, considere la posibilidad de agregar los usuarios con privilegios a un rol y limitar el acceso a un método de concentrador a solo los miembros de ese rol. Para obtener un ejemplo de restringir el acceso basándose en una función, vea [autenticación y autorización para los concentradores de SignalR](../security/hub-authorization.md). Para obtener un ejemplo de la comprobación de acceso de usuario a grupos al volver a conectarse, consulte [trabajar con grupos](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>Controlar entrada desde clientes de forma segura

Todas las entradas de los clientes que está pensada para difusión a otros clientes deben codificarse para asegurarse de que un usuario malintencionado no envía la secuencia de comandos a otros usuarios. Es aconsejable codificar los mensajes en los clientes de recepción en lugar de en el servidor, ya que la aplicación de SignalR puede tener muchos tipos diferentes de los clientes. Por lo tanto, la codificación HTML funciona para un cliente web, pero no para otros tipos de clientes. Por ejemplo, un método de cliente web para mostrar un mensaje de chat con seguridad controlaría el nombre de usuario y el mensaje mediante una llamada a la `html()` función.

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>Reconciliar un cambio en el estado de usuario con una conexión activa

Si el estado de la autenticación de un usuario cambia mientras existe una conexión activa, el usuario recibirá un error que indica, "la identidad del usuario no puede cambiar durante una conexión SignalR activa". En ese caso, la aplicación vuelve a debe conectarse al servidor para asegurarse de que el nombre de usuario y el identificador de conexión se coordinan. Por ejemplo, si la aplicación permite al usuario cerrar la sesión mientras exista una conexión activa, el nombre de usuario para la conexión ya no coincidirá con el nombre que se pasa en la siguiente solicitud. Se desea detener la conexión antes de que el usuario cierre sesión y, a continuación, reinícielo.

Sin embargo, es importante tener en cuenta que la mayoría de las aplicaciones no necesitarán detener e iniciar la conexión manualmente. Si la aplicación redirige a los usuarios a una página independiente después de iniciar sesión, por ejemplo, el comportamiento predeterminado en una aplicación de formularios Web Forms o MVC, o actualiza la página actual después de cerrar sesión, la conexión activa se desconecta automáticamente y no lo hace requerir ninguna acción adicional.

El ejemplo siguiente muestra cómo detener e iniciar una conexión cuando ha cambiado el estado de usuario.

[!code-html[Main](introduction-to-security/samples/sample3.html)]

O bien, puede cambiar el estado de la autenticación del usuario si su sitio utiliza una expiración variable con autenticación de formularios, y no hay ninguna actividad para mantener la cookie de autenticación válido. En ese caso, el usuario se cerrará y el nombre de usuario ya no coincidirá con el nombre de usuario en el token de conexión. Puede corregir este problema mediante la adición de un script que solicita periódicamente un recurso en el servidor web para mantener la cookie de autenticación válida. El ejemplo siguiente muestra cómo solicitar un recurso cada 30 minutos.

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>Archivos del proxy de JavaScript generados automáticamente

Si no desea incluir todos los concentradores y métodos en el archivo de proxy de JavaScript para cada usuario, puede deshabilitar la generación automática del archivo. Puede elegir esta opción si tiene varios concentradores y métodos, pero no desea que todos los usuarios a tener en cuenta todos los métodos. Deshabilitar la generación automática estableciendo **EnableJavaScriptProxies** a **false**.

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

Para obtener más información acerca de los archivos del proxy de JavaScript, consulte [el proxy generado y lo que hace por usted](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy). <a id="exceptions"></a>

### <a name="exceptions"></a>Excepciones

Debe evitar pasar objetos de excepción para los clientes porque los objetos pueden exponer información confidencial a los clientes. En su lugar, llame a un método en el cliente que muestra el mensaje de error relevantes.

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
