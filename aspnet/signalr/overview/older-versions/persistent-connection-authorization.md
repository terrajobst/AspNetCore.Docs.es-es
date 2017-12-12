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
ms.openlocfilehash: 4c036ddf1e20e3a3be7b043d90b594292013f6c2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="4eac6-104">Autenticación y autorización para las conexiones persistentes de SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="4eac6-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>
====================
<span data-ttu-id="4eac6-105">por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4eac6-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4eac6-106">En este tema se describe cómo exigir una autorización en una conexión persistente.</span><span class="sxs-lookup"><span data-stu-id="4eac6-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="4eac6-107">Para obtener información general sobre la integración de seguridad en una aplicación de SignalR, consulte [Introducción a la seguridad](index.md).</span><span class="sxs-lookup"><span data-stu-id="4eac6-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="4eac6-108">Exigir una autorización</span><span class="sxs-lookup"><span data-stu-id="4eac6-108">Enforce authorization</span></span>

<span data-ttu-id="4eac6-109">Para aplicar las reglas de autorización cuando se usa un [PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) debe invalidar el `AuthorizeRequest` método.</span><span class="sxs-lookup"><span data-stu-id="4eac6-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="4eac6-110">No se puede utilizar el `Authorize` atributo con las conexiones persistentes.</span><span class="sxs-lookup"><span data-stu-id="4eac6-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="4eac6-111">El `AuthorizeRequest` método es llamado por el marco de SignalR antes de cada solicitud para comprobar que el usuario está autorizado para realizar la acción solicitada.</span><span class="sxs-lookup"><span data-stu-id="4eac6-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="4eac6-112">El `AuthorizeRequest` método no se llama desde el cliente; en su lugar, autenticar al usuario a través del mecanismo de autenticación estándar de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4eac6-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="4eac6-113">El ejemplo siguiente muestra cómo limitar las solicitudes a los usuarios autenticados.</span><span class="sxs-lookup"><span data-stu-id="4eac6-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="4eac6-114">Puede agregar cualquier lógica de autorización personalizada en el método AuthorizeRequest; Comprobando como por ejemplo, si un usuario pertenece a un rol determinado.</span><span class="sxs-lookup"><span data-stu-id="4eac6-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
