---
uid: signalr/overview/security/persistent-connection-authorization
title: "Autenticación y autorización para las conexiones persistentes de SignalR | Documentos de Microsoft"
author: pfletcher
description: "En este tema se describe cómo exigir una autorización en una conexión persistente. Para obtener información general sobre la integración de seguridad en una aplicación de SignalR,..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 9c6fff86ae6b1b65e6ba9922b6b8448643ef1f15
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-persistent-connections"></a><span data-ttu-id="da8a4-104">Autenticación y autorización para las conexiones persistentes de SignalR</span><span class="sxs-lookup"><span data-stu-id="da8a4-104">Authentication and Authorization for SignalR Persistent Connections</span></span>
====================
<span data-ttu-id="da8a4-105">por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="da8a4-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="da8a4-106">En este tema se describe cómo exigir una autorización en una conexión persistente.</span><span class="sxs-lookup"><span data-stu-id="da8a4-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="da8a4-107">Para obtener información general sobre la integración de seguridad en una aplicación de SignalR, consulte [Introducción a la seguridad](introduction-to-security.md).</span><span class="sxs-lookup"><span data-stu-id="da8a4-107">For general information about integrating security into a SignalR application, see [Introduction to Security](introduction-to-security.md).</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="da8a4-108">Versiones de software que se usa en este tema</span><span class="sxs-lookup"><span data-stu-id="da8a4-108">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="da8a4-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="da8a4-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="da8a4-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="da8a4-110">.NET 4.5</span></span>
> - <span data-ttu-id="da8a4-111">SignalR versión 2</span><span class="sxs-lookup"><span data-stu-id="da8a4-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="da8a4-112">Versiones anteriores de este tema</span><span class="sxs-lookup"><span data-stu-id="da8a4-112">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="da8a4-113">Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="da8a4-113">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="da8a4-114">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="da8a4-114">Questions and comments</span></span>
> 
> <span data-ttu-id="da8a4-115">Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="da8a4-115">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="da8a4-116">Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="da8a4-116">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="da8a4-117">Exigir una autorización</span><span class="sxs-lookup"><span data-stu-id="da8a4-117">Enforce authorization</span></span>

<span data-ttu-id="da8a4-118">Para aplicar las reglas de autorización cuando se usa un [PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) debe invalidar el `AuthorizeRequest` método.</span><span class="sxs-lookup"><span data-stu-id="da8a4-118">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="da8a4-119">No se puede utilizar el `Authorize` atributo con las conexiones persistentes.</span><span class="sxs-lookup"><span data-stu-id="da8a4-119">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="da8a4-120">El `AuthorizeRequest` método es llamado por el marco de SignalR antes de cada solicitud para comprobar que el usuario está autorizado para realizar la acción solicitada.</span><span class="sxs-lookup"><span data-stu-id="da8a4-120">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="da8a4-121">El `AuthorizeRequest` método no se llama desde el cliente; en su lugar, autenticar al usuario a través del mecanismo de autenticación estándar de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="da8a4-121">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="da8a4-122">El ejemplo siguiente muestra cómo limitar las solicitudes a los usuarios autenticados.</span><span class="sxs-lookup"><span data-stu-id="da8a4-122">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="da8a4-123">Puede agregar cualquier lógica de autorización personalizada en el método AuthorizeRequest; Comprobando como por ejemplo, si un usuario pertenece a un rol determinado.</span><span class="sxs-lookup"><span data-stu-id="da8a4-123">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
