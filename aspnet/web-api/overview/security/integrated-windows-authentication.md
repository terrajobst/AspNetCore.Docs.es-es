---
uid: web-api/overview/security/integrated-windows-authentication
title: Autenticación de Windows integrada | Documentos de Microsoft
author: MikeWasson
description: Describe cómo utilizar la autenticación integrada de Windows en ASP.NET Web API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/18/2012
ms.topic: article
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: bf5f55d98d61cdfdd246a847f41a6f1c65f00bfc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508164"
---
<a name="integrated-windows-authentication"></a><span data-ttu-id="43074-103">Autenticación integrada de Windows</span><span class="sxs-lookup"><span data-stu-id="43074-103">Integrated Windows Authentication</span></span>
====================
<span data-ttu-id="43074-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="43074-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="43074-105">Autenticación integrada de Windows permite a los usuarios iniciar sesión con sus credenciales de Windows, utilizando Kerberos o NTLM.</span><span class="sxs-lookup"><span data-stu-id="43074-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="43074-106">El cliente envía las credenciales en el encabezado de autorización.</span><span class="sxs-lookup"><span data-stu-id="43074-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="43074-107">Autenticación de Windows es más apropiada para un entorno de intranet.</span><span class="sxs-lookup"><span data-stu-id="43074-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="43074-108">Para obtener más información, consulte [autenticación de Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span><span class="sxs-lookup"><span data-stu-id="43074-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="43074-109">Ventajas</span><span class="sxs-lookup"><span data-stu-id="43074-109">Advantages</span></span> | <span data-ttu-id="43074-110">Desventajas</span><span class="sxs-lookup"><span data-stu-id="43074-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="43074-111">-Integrado en IIS.</span><span class="sxs-lookup"><span data-stu-id="43074-111">- Built into IIS.</span></span> <span data-ttu-id="43074-112">-No envía las credenciales de usuario en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="43074-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="43074-113">-Si el equipo cliente pertenece al dominio (por ejemplo, la aplicación de intranet), el usuario no es necesario que escriba las credenciales.</span><span class="sxs-lookup"><span data-stu-id="43074-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="43074-114">-No se recomienda para las aplicaciones de Internet.</span><span class="sxs-lookup"><span data-stu-id="43074-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="43074-115">-Requiere compatibilidad con Kerberos o NTLM en el cliente.</span><span class="sxs-lookup"><span data-stu-id="43074-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="43074-116">-Cliente debe encontrarse en el dominio de Active Directory.</span><span class="sxs-lookup"><span data-stu-id="43074-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="43074-117">Si la aplicación se hospeda en Azure y tiene un dominio de Active Directory local, considere la posibilidad de federación de AD local con Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="43074-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="43074-118">De este modo, los usuarios pueden iniciar sesión con sus credenciales locales, pero se realiza la autenticación de Azure AD.</span><span class="sxs-lookup"><span data-stu-id="43074-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="43074-119">Para obtener más información, consulte [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="43074-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>


<span data-ttu-id="43074-120">Para crear una aplicación que utiliza la autenticación integrada de Windows, seleccione la plantilla de "Aplicación de Intranet" en el Asistente para proyecto de MVC 4.</span><span class="sxs-lookup"><span data-stu-id="43074-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="43074-121">Esta plantilla de proyecto establece la siguiente configuración en el archivo Web.config:</span><span class="sxs-lookup"><span data-stu-id="43074-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="43074-122">En el lado del cliente, la autenticación integrada de Windows funciona con cualquier explorador que admita la [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) esquema de autenticación, que incluye la mayoría de los exploradores principal.</span><span class="sxs-lookup"><span data-stu-id="43074-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="43074-123">Para aplicaciones de cliente. NET, el **HttpClient** clase admite la autenticación de Windows:</span><span class="sxs-lookup"><span data-stu-id="43074-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="43074-124">Autenticación de Windows es vulnerable a ataques de falsificación (CSRF) de solicitud entre sitios.</span><span class="sxs-lookup"><span data-stu-id="43074-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="43074-125">Vea [prevención de ataques de falsificación (CSRF) de solicitud entre sitios](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="43074-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>
