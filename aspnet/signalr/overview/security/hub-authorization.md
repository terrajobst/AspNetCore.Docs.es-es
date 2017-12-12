---
uid: signalr/overview/security/hub-authorization
title: "Autenticación y autorización para los concentradores SignalR | Documentos de Microsoft"
author: pfletcher
description: "En este tema se describe cómo restringir qué usuarios o roles pueden tener acceso a métodos de concentrador. Versiones de software usan en este tema ha de Visual Studio 2013 .NET 4.5 SignalR..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/05/2015
ms.topic: article
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: f1538c933ff9e8e680d70ce1e63d24b189be47e5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-hubs"></a>Autenticación y autorización para los concentradores de SignalR
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> En este tema se describe cómo restringir qué usuarios o roles pueden tener acceso a métodos de concentrador. 
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

Este tema contiene las siguientes secciones:

- [Autorizar el atributo](#authorizeattribute)
- [Requerir autenticación para todos los centros](#requireauth)
- [Autorización personalizada](#custom)
- [Pasar información de autenticación a los clientes](#passauth)
- [Opciones de autenticación para los clientes de .NET](#authoptions)

    - [Cookie de autenticación de formularios](#cookie)
    - [Autenticación de Windows](#windows)
    - [Encabezado de conexión](#header)
    - [Certificado](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a>Autorizar el atributo

SignalR proporciona el [Authorize](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) atributo para especificar qué usuarios o roles tienen acceso a un concentrador o un método. Este atributo se encuentra en la `Microsoft.AspNet.SignalR` espacio de nombres. Aplicar el `Authorize` atributo a un concentrador o determinados métodos en un concentrador. Al aplicar el `Authorize` atributo a una clase de base de datos central, el requisito de autorización especificado se aplica a todos los métodos en el concentrador. En este tema se proporciona ejemplos de los distintos tipos de requisitos de autorización que se pueden aplicar. Sin el `Authorize` atributo conectada cliente puede tener acceso a cualquier método público en el concentrador.

Si ha definido un rol denominado "Admin" en la aplicación web, puede especificar que sólo los usuarios con ese rol pueden tener acceso a un concentrador con el código siguiente.

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

O bien, puede especificar que un concentrador contiene un método que está disponible para todos los usuarios y un segundo método solo está disponible para los usuarios autenticados, tal y como se muestra a continuación.

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

Los siguientes ejemplos referentes a los escenarios de autorización diferentes:

- `[Authorize]`: solo los usuarios autenticados
- `[Authorize(Roles = "Admin,Manager")]`: solo autenticados los usuarios de los roles especificados
- `[Authorize(Users = "user1,user2")]`: solo autenticados los usuarios con los nombres de usuario especificado
- `[Authorize(RequireOutgoing=false)]`: solo los usuarios autenticados pueden invocar el concentrador, pero las llamadas desde el servidor a los clientes no están limitadas por la autorización, como por ejemplo, cuando solo ciertos usuarios pueden enviar un mensaje, pero todas las demás pueden recibir el mensaje. La propiedad RequireOutgoing solo puede aplicarse al concentrador completo, no en los métodos de usuarios en el concentrador. Cuando RequireOutgoing no está establecido en false, solo los usuarios que cumplan los requisitos de autorización se llaman desde el servidor.

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a>Requerir autenticación para todos los centros

Puede requerir la autenticación para todos los concentradores y métodos de concentrador en la aplicación mediante una llamada a la [RequireAuthentication](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) método cuando se inicia la aplicación. Puede utilizar este método cuando tiene varios concentradores y desea aplicar un requisito de autenticación para todas ellas. Con este método, no puede especificar los requisitos de autorización saliente, usuario o rol de. Solo se puede especificar que está restringido a los usuarios autenticados acceso a los métodos de concentrador. Sin embargo, todavía puede aplicar el atributo Authorize a concentradores o métodos para especificar requisitos adicionales. Cualquier requisito que especifique en un atributo se agrega al requisito de autenticación básico.

En el ejemplo siguiente se muestra un archivo de inicio que restringe todos los métodos de concentrador a los usuarios autenticados.

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

Si se llama a la `RequireAuthentication()` método se ha procesado una solicitud de SignalR, SignalR producirá un `InvalidOperationException` excepción. SignalR produce esta excepción porque no se puede agregar un módulo a la HubPipeline después de que se ha invocado la canalización. El ejemplo anterior muestra que realiza la llamada la `RequireAuthentication` método en el `Configuration` método que se ejecuta una vez antes de tratar la primera solicitud.

<a id="custom"></a>

## <a name="customized-authorization"></a>Autorización personalizada

Si necesita personalizar cómo se determina la autorización, puede crear una clase que deriva de `AuthorizeAttribute` e invalide el [UserAuthorized](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) método. SignalR para cada solicitud, invoca este método para determinar si el usuario está autorizado para completar la solicitud. En el método invalidado, proporcione la lógica necesaria para su escenario de autorización. En el ejemplo siguiente se muestra cómo aplicar la autorización a través de la identidad basada en notificaciones.

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a>Pasar información de autenticación a los clientes

Debe usar la información de autenticación en el código que se ejecuta en el cliente. Pasar la información necesaria al llamar a los métodos en el cliente. Por ejemplo, un método de aplicación de chat podría pasar como parámetro el nombre de usuario de la persona que publica un mensaje, tal y como se muestra a continuación.

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

O bien, puede crear un objeto para representar la información de autenticación y pasar ese objeto como un parámetro, tal y como se muestra a continuación.

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

No debe pasar nunca Id. de conexión de un cliente a otros clientes, cuando un usuario malintencionado podría utilizarlo para imitar una solicitud de ese cliente.

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a>Opciones de autenticación para los clientes de .NET

Si tiene un cliente. NET, como una aplicación de consola que interactúa con un concentrador que está limitado a los usuarios autenticados, puede pasar las credenciales de autenticación en una cookie, el encabezado de conexión o un certificado. Los ejemplos de esta sección muestran cómo utilizar los distintos métodos para autenticar a un usuario. No son totalmente funcionales aplicaciones SignalR. Para obtener más información acerca de los clientes de .NET con SignalR, consulte [Guía de la API de concentradores - cliente .NET](../guide-to-the-api/hubs-api-guide-net-client.md).

<a id="cookie"></a>

### <a name="cookie"></a>Cookie

Cuando el cliente .NET interactúa con un concentrador que utiliza la autenticación de formularios de ASP.NET, debe establecer manualmente la cookie de autenticación en la conexión. Agregue la cookie a la `CookieContainer` propiedad en el [HubConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) objeto. En el ejemplo siguiente se muestra una aplicación de consola que recupera una cookie de autenticación desde una página web y agrega dicha cookie para la conexión.

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

La aplicación de consola envía las credenciales para **www.contoso.com/RemoteLogin** que podría hacer referencia a una página vacía que contiene el siguiente archivo de código subyacente.

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Autenticación de Windows

Al utilizar la autenticación de Windows, puede pasar las credenciales del usuario actual mediante el [DefaultCredentials](https://msdn.microsoft.com/en-us/library/system.net.credentialcache.defaultcredentials.aspx) propiedad. Establezca las credenciales para la conexión con el valor de la DefaultCredentials.

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>Encabezado de conexión

Si la aplicación no utiliza cookies, puede pasar información de usuario en el encabezado de conexión. Por ejemplo, puede pasar un token en el encabezado de conexión.

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

A continuación, en el concentrador, debe comprobar el token del usuario.

<a id="certificate"></a>

### <a name="certificate"></a>Certificado

Puede pasar un certificado de cliente para comprobar que el usuario. Agregar el certificado al crear la conexión. En el ejemplo siguiente se muestra solamente cómo agregar un certificado de cliente para la conexión; no se muestra la aplicación de consola completa. Usa el [X509Certificate](https://msdn.microsoft.com/en-us/library/system.security.cryptography.x509certificates.x509certificate.aspx) clase que proporciona varias formas de crear el certificado.

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
