---
uid: web-api/overview/security/integrated-windows-authentication
title: Autenticación de Windows integrada | Microsoft Docs
author: MikeWasson
description: Describe cómo usar la autenticación de Windows integrada en ASP.NET Web API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/18/2012
ms.topic: article
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: f11b9fe5d98118a252c6c00dd2997b2ee9a3da7a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381608"
---
<a name="integrated-windows-authentication"></a><span data-ttu-id="7be56-103">Autenticación integrada de Windows</span><span class="sxs-lookup"><span data-stu-id="7be56-103">Integrated Windows Authentication</span></span>
====================
<span data-ttu-id="7be56-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7be56-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="7be56-105">La autenticación integrada de Windows permite a los usuarios inicien sesión con sus credenciales de Windows, utilizando Kerberos o NTLM.</span><span class="sxs-lookup"><span data-stu-id="7be56-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="7be56-106">El cliente envía las credenciales en el encabezado de autorización.</span><span class="sxs-lookup"><span data-stu-id="7be56-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="7be56-107">Autenticación de Windows es más adecuada para un entorno de intranet.</span><span class="sxs-lookup"><span data-stu-id="7be56-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="7be56-108">Para obtener más información, consulte [Windows autenticación](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span><span class="sxs-lookup"><span data-stu-id="7be56-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="7be56-109">Ventajas</span><span class="sxs-lookup"><span data-stu-id="7be56-109">Advantages</span></span> | <span data-ttu-id="7be56-110">Desventajas</span><span class="sxs-lookup"><span data-stu-id="7be56-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="7be56-111">-Integrada en IIS.</span><span class="sxs-lookup"><span data-stu-id="7be56-111">- Built into IIS.</span></span> <span data-ttu-id="7be56-112">-No envía las credenciales de usuario en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="7be56-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="7be56-113">-Si el equipo cliente pertenece al dominio (por ejemplo, la aplicación de intranet), el usuario no es necesario que escriba las credenciales.</span><span class="sxs-lookup"><span data-stu-id="7be56-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="7be56-114">-No se recomienda para aplicaciones de Internet.</span><span class="sxs-lookup"><span data-stu-id="7be56-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="7be56-115">-Requiere soporte técnico de Kerberos o NTLM en el cliente.</span><span class="sxs-lookup"><span data-stu-id="7be56-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="7be56-116">-Cliente debe estar en el dominio de Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7be56-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="7be56-117">Si la aplicación está hospedada en Azure y tiene un dominio de Active Directory local, considere la posibilidad de federar su AD local con Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7be56-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="7be56-118">De este modo, los usuarios pueden iniciar sesión con sus credenciales de forma local, pero la autenticación se realiza por Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7be56-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="7be56-119">Para obtener más información, consulte [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="7be56-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>


<span data-ttu-id="7be56-120">Para crear una aplicación que usa la autenticación de Windows integrada, seleccione la plantilla "Aplicación de Intranet" en el Asistente para proyectos de MVC 4.</span><span class="sxs-lookup"><span data-stu-id="7be56-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="7be56-121">Esta plantilla de proyecto coloca la siguiente configuración en el archivo Web.config:</span><span class="sxs-lookup"><span data-stu-id="7be56-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="7be56-122">En el lado cliente, la autenticación de Windows integrada funciona con cualquier explorador que admita la [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) esquema de autenticación, que incluye la mayoría de los exploradores principal.</span><span class="sxs-lookup"><span data-stu-id="7be56-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="7be56-123">Para las aplicaciones cliente. NET, el **HttpClient** clase admite la autenticación de Windows:</span><span class="sxs-lookup"><span data-stu-id="7be56-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="7be56-124">Autenticación de Windows es vulnerable a ataques de falsificación (CSRF) de solicitud entre sitios.</span><span class="sxs-lookup"><span data-stu-id="7be56-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="7be56-125">Consulte [prevención de ataques de solicitud entre sitios (CSRF) falsificación](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="7be56-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>
