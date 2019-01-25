---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Autenticación y autorización para las conexiones persistentes de SignalR (SignalR 1.x) | Microsoft Docs
author: bradygaster
description: Este tema describe cómo aplicar la autorización en una conexión persistente. Para obtener información general sobre la integración de seguridad en una aplicación de SignalR,...
ms.author: bradyg
ms.date: 10/21/2013
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: c4a2c9b4fa05e43ade98fb521c59f8645b43ffea
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837356"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>Autenticación y autorización para las conexiones persistentes de SignalR (SignalR 1.x)
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este tema describe cómo aplicar la autorización en una conexión persistente. Para obtener información general sobre la integración de seguridad en una aplicación de SignalR, consulte [Introducción a la seguridad](index.md).


## <a name="enforce-authorization"></a>Exigir la autorización

Para aplicar las reglas de autorización cuando se usa un [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) debe invalidar el `AuthorizeRequest` método. No puede usar el `Authorize` atributo con las conexiones persistentes. El `AuthorizeRequest` método es llamado por el marco de SignalR antes de cada solicitud para comprobar que el usuario está autorizado para realizar la acción solicitada. El `AuthorizeRequest` no se llama al método desde el cliente; en su lugar, autenticar al usuario a través del mecanismo de autenticación estándar de la aplicación.

El ejemplo siguiente muestra cómo limitar las solicitudes a los usuarios autenticados.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Puede agregar cualquier lógica de autorización personalizada en el método AuthorizeRequest; Por ejemplo, comprobando si un usuario pertenece a un rol determinado.
