---
uid: signalr/overview/security/hub-authorization
title: Autenticación y autorización para los concentradores de SignalR | Microsoft Docs
author: pfletcher
description: Este tema describe cómo restringir qué usuarios o roles pueden tener acceso a los métodos de concentrador. Las versiones de software usan en este tema Visual Studio 2013 .NET 4.5 SignalR ve...
ms.author: riande
ms.date: 01/05/2015
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 21946b9fa0406de79af7b83809869d90e6e8234e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829747"
---
<a name="authentication-and-authorization-for-signalr-hubs"></a>Autenticación y autorización para los concentradores de SignalR
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> Este tema describe cómo restringir qué usuarios o roles pueden tener acceso a los métodos de concentrador. 
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


## <a name="overview"></a>Información general

Este tema contiene las siguientes secciones:

- [Atributo de autorización](#authorizeattribute)
- [Requerir autenticación para todos los centros](#requireauth)
- [Autorización personalizada](#custom)
- [Pasar información de autenticación a los clientes](#passauth)
- [Opciones de autenticación para los clientes de .NET](#authoptions)

    - [Cookie de autenticación de formularios](#cookie)
    - [Autenticación de Windows](#windows)
    - [Encabezado de conexión](#header)
    - [Certificado](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a>Atributo de autorización

SignalR proporciona el [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) atributo para especificar qué usuarios o roles tienen acceso a un concentrador o un método. Este atributo se encuentra en la `Microsoft.AspNet.SignalR` espacio de nombres. Aplica el `Authorize` atributo a un concentrador o determinados métodos en un concentrador. Al aplicar el `Authorize` atributo a una clase de hub, el requisito de autorización especificada se aplica a todos los métodos del concentrador. En este tema se proporciona ejemplos de los distintos tipos de los requisitos de autorización que se pueden aplicar. Sin el `Authorize` atributo, una conexión cliente puede tener acceso a cualquier método público en el concentrador.

Si ha definido un rol denominado "Admin" en la aplicación web, puede especificar que solo los usuarios de ese rol pueden tener acceso a un concentrador con el código siguiente.

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

O bien, puede especificar que un concentrador contiene un método que está disponible para todos los usuarios y un segundo método solo está disponible para los usuarios autenticados, como se muestra a continuación.

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

Los ejemplos siguientes abordan los escenarios de autorización diferentes:

- `[Authorize]` : solo los usuarios autenticados
- `[Authorize(Roles = "Admin,Manager")]` : solo autenticados los usuarios de los roles especificados
- `[Authorize(Users = "user1,user2")]` : solo autenticados los usuarios con los nombres de usuario especificado
- `[Authorize(RequireOutgoing=false)]` : solo los usuarios autenticados pueden invocar el concentrador, pero las llamadas desde el servidor a los clientes no están limitadas por la autorización, como por ejemplo, cuando sólo determinados usuarios pueden enviar un mensaje, pero todos los demás pueden recibir el mensaje. La propiedad RequireOutgoing sólo puede aplicarse al centro de todo, no en los métodos de las personas en el centro. Cuando RequireOutgoing no se establece en false, solo los usuarios que cumplen el requisito de autorización se llaman desde el servidor.

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a>Requerir autenticación para todos los centros

Puede requerir autenticación para todos los concentradores y métodos de concentrador en la aplicación mediante una llamada a la [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) método cuando se inicia la aplicación. Puede usar este método cuando haya varios centros y desea aplicar un requisito de autenticación para todas ellas. Con este método, no se puede especificar los requisitos de autorización saliente, usuario o rol. Solo se puede especificar que está restringido a los usuarios autenticados acceso a los métodos de concentrador. Sin embargo, todavía puede aplicar el atributo Authorize a concentradores o métodos para especificar los requisitos adicionales. Cualquier requisito que especifique en un atributo se agrega al requisito de autenticación básico.

El ejemplo siguiente muestra un archivo de inicio que restringe todos los métodos de concentrador a los usuarios autenticados.

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

Si se llama a la `RequireAuthentication()` método se ha procesado una solicitud de SignalR, SignalR producirá un `InvalidOperationException` excepción. SignalR produce esta excepción porque no se puede agregar un módulo a la HubPipeline después de que se ha invocado la canalización. El ejemplo anterior muestra la llamada a la `RequireAuthentication` método en el `Configuration` método que se ejecuta una vez antes de tratar la primera solicitud.

<a id="custom"></a>

## <a name="customized-authorization"></a>Autorización personalizada

Si necesita personalizar cómo se determina la autorización, puede crear una clase que derive de `AuthorizeAttribute` e invalidar la [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) método. Para cada solicitud, SignalR invoca este método para determinar si el usuario está autorizado para completar la solicitud. En el método reemplazado, se proporciona la lógica necesaria para su escenario de autorización. El ejemplo siguiente muestra cómo aplicar la autorización a través de la identidad basada en notificaciones.

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a>Pasar información de autenticación a los clientes

Es posible que deba usar información de autenticación en el código que se ejecuta en el cliente. Pasar la información necesaria al llamar a los métodos en el cliente. Por ejemplo, un método de aplicación de chat podría pasar como parámetro el nombre de usuario de la persona que publica un mensaje, como se muestra a continuación.

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

O bien, puede crear un objeto para representar la información de autenticación y pasar ese objeto como un parámetro, tal como se muestra a continuación.

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

No debe pasar nunca Id. de conexión de un cliente a otros clientes, como un usuario malintencionado podría utilizar para imitar una solicitud de ese cliente.

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a>Opciones de autenticación para los clientes de .NET

Cuando haya un cliente. NET, como una aplicación de consola que interactúa con un concentrador que se limita a los usuarios autenticados, puede pasar las credenciales de autenticación en una cookie, el encabezado de conexión o un certificado. Los ejemplos en esta sección muestran cómo usar esos métodos diferentes para autenticar un usuario. No son aplicaciones plenamente funcionales de SignalR. Para obtener más información acerca de los clientes de .NET con SignalR, consulte [Guía de la API Hubs: cliente .NET](../guide-to-the-api/hubs-api-guide-net-client.md).

<a id="cookie"></a>

### <a name="cookie"></a>Cookie

Cuando el cliente de .NET interactúa con un concentrador que utiliza la autenticación de formularios de ASP.NET, deberá establecer manualmente la cookie de autenticación en la conexión. Agregue la cookie a la `CookieContainer` propiedad en el [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) objeto. El ejemplo siguiente muestra una aplicación de consola que recupera una cookie de autenticación de una página web y agrega dicha cookie para la conexión.

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

Las credenciales para entradas de la aplicación de consola <strong>www.contoso.com/RemoteLogin</strong> que podría hacer referencia a una página vacía que contiene el siguiente archivo de código subyacente.

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Autenticación de Windows

Al utilizar la autenticación de Windows, puede pasar las credenciales del usuario actual mediante el [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) propiedad. Establecer las credenciales para la conexión en el valor de la DefaultCredentials.

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>Encabezado de conexión

Si la aplicación no usa cookies, puede pasar información de usuario en el encabezado de la conexión. Por ejemplo, puede pasar un token en el encabezado de la conexión.

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

A continuación, en el centro, debe comprobar el token del usuario.

<a id="certificate"></a>

### <a name="certificate"></a>Certificado

Puede pasar un certificado de cliente para comprobar que el usuario. Agregue el certificado al crear la conexión. El ejemplo siguiente muestra solo cómo agregar un certificado de cliente a la conexión; no se muestra la aplicación de consola completa. Usa el [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) clase que proporciona varias formas de crear el certificado.

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
