---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Autenticación y autorización para las conexiones persistentes de SignalR (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: Este tema describe cómo aplicar la autorización en una conexión persistente. Para obtener información general sobre la integración de seguridad en una aplicación de SignalR,...
ms.author: riande
ms.date: 10/21/2013
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 6c13a63630948a8b6bd1f6e3a6e752b9695256bf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828555"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="1445d-104">Autenticación y autorización para las conexiones persistentes de SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="1445d-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>
====================
<span data-ttu-id="1445d-105">por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1445d-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1445d-106">Este tema describe cómo aplicar la autorización en una conexión persistente.</span><span class="sxs-lookup"><span data-stu-id="1445d-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="1445d-107">Para obtener información general sobre la integración de seguridad en una aplicación de SignalR, consulte [Introducción a la seguridad](index.md).</span><span class="sxs-lookup"><span data-stu-id="1445d-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="1445d-108">Exigir la autorización</span><span class="sxs-lookup"><span data-stu-id="1445d-108">Enforce authorization</span></span>

<span data-ttu-id="1445d-109">Para aplicar las reglas de autorización cuando se usa un [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) debe invalidar el `AuthorizeRequest` método.</span><span class="sxs-lookup"><span data-stu-id="1445d-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="1445d-110">No puede usar el `Authorize` atributo con las conexiones persistentes.</span><span class="sxs-lookup"><span data-stu-id="1445d-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="1445d-111">El `AuthorizeRequest` método es llamado por el marco de SignalR antes de cada solicitud para comprobar que el usuario está autorizado para realizar la acción solicitada.</span><span class="sxs-lookup"><span data-stu-id="1445d-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="1445d-112">El `AuthorizeRequest` no se llama al método desde el cliente; en su lugar, autenticar al usuario a través del mecanismo de autenticación estándar de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1445d-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="1445d-113">El ejemplo siguiente muestra cómo limitar las solicitudes a los usuarios autenticados.</span><span class="sxs-lookup"><span data-stu-id="1445d-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="1445d-114">Puede agregar cualquier lógica de autorización personalizada en el método AuthorizeRequest; Por ejemplo, comprobando si un usuario pertenece a un rol determinado.</span><span class="sxs-lookup"><span data-stu-id="1445d-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
