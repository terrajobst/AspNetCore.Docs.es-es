---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: "Autenticación y autorización para las conexiones persistentes de SignalR (SignalR 1.x) | Documentos de Microsoft"
author: pfletcher
description: "En este tema se describe cómo exigir una autorización en una conexión persistente. Para obtener información general sobre la integración de seguridad en una aplicación de SignalR,..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 2e97dfd03c61b110325c41a992b4af490fcd17de
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>Autenticación y autorización para las conexiones persistentes de SignalR (SignalR 1.x)
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> En este tema se describe cómo exigir una autorización en una conexión persistente. Para obtener información general sobre la integración de seguridad en una aplicación de SignalR, consulte [Introducción a la seguridad](index.md).


## <a name="enforce-authorization"></a>Exigir una autorización

Para aplicar las reglas de autorización cuando se usa un [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) debe invalidar el `AuthorizeRequest` método. No se puede utilizar el `Authorize` atributo con las conexiones persistentes. El `AuthorizeRequest` método es llamado por el marco de SignalR antes de cada solicitud para comprobar que el usuario está autorizado para realizar la acción solicitada. El `AuthorizeRequest` método no se llama desde el cliente; en su lugar, autenticar al usuario a través del mecanismo de autenticación estándar de la aplicación.

El ejemplo siguiente muestra cómo limitar las solicitudes a los usuarios autenticados.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Puede agregar cualquier lógica de autorización personalizada en el método AuthorizeRequest; Comprobando como por ejemplo, si un usuario pertenece a un rol determinado.
