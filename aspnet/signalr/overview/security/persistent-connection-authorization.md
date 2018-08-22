---
uid: signalr/overview/security/persistent-connection-authorization
title: Autenticación y autorización para las conexiones persistentes de SignalR | Microsoft Docs
author: pfletcher
description: Este tema describe cómo aplicar la autorización en una conexión persistente. Para obtener información general sobre la integración de seguridad en una aplicación de SignalR,...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: e7ae160cbe4c5f6cdb393768758f5bdec4203dbf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838498"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections"></a>Autenticación y autorización para las conexiones persistentes de SignalR
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> Este tema describe cómo aplicar la autorización en una conexión persistente. Para obtener información general sobre la integración de seguridad en una aplicación de SignalR, consulte [Introducción a la seguridad](introduction-to-security.md). 
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


## <a name="enforce-authorization"></a>Exigir la autorización

Para aplicar las reglas de autorización cuando se usa un [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) debe invalidar el `AuthorizeRequest` método. No puede usar el `Authorize` atributo con las conexiones persistentes. El `AuthorizeRequest` método es llamado por el marco de SignalR antes de cada solicitud para comprobar que el usuario está autorizado para realizar la acción solicitada. El `AuthorizeRequest` no se llama al método desde el cliente; en su lugar, autenticar al usuario a través del mecanismo de autenticación estándar de la aplicación.

El ejemplo siguiente muestra cómo limitar las solicitudes a los usuarios autenticados.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Puede agregar cualquier lógica de autorización personalizada en el método AuthorizeRequest; Por ejemplo, comprobando si un usuario pertenece a un rol determinado.
