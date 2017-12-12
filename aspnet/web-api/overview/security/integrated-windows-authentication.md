---
uid: web-api/overview/security/integrated-windows-authentication
title: "Autenticación de Windows integrada | Documentos de Microsoft"
author: MikeWasson
description: "Describe cómo utilizar la autenticación integrada de Windows en ASP.NET Web API."
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
---
<a name="integrated-windows-authentication"></a>Autenticación integrada de Windows
====================
por [Mike Wasson](https://github.com/MikeWasson)

Autenticación integrada de Windows permite a los usuarios iniciar sesión con sus credenciales de Windows, utilizando Kerberos o NTLM. El cliente envía las credenciales en el encabezado de autorización. Autenticación de Windows es más apropiada para un entorno de intranet. Para obtener más información, consulte [autenticación de Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).

| Ventajas | Desventajas |
| --- | --- |
| -Integrado en IIS. -No envía las credenciales de usuario en la solicitud. -Si el equipo cliente pertenece al dominio (por ejemplo, la aplicación de intranet), el usuario no es necesario que escriba las credenciales. | -No se recomienda para las aplicaciones de Internet. -Requiere compatibilidad con Kerberos o NTLM en el cliente. -Cliente debe encontrarse en el dominio de Active Directory. |

> [!NOTE]
> Si la aplicación se hospeda en Azure y tiene un dominio de Active Directory local, considere la posibilidad de federación de AD local con Azure Active Directory. De este modo, los usuarios pueden iniciar sesión con sus credenciales locales, pero se realiza la autenticación de Azure AD. Para obtener más información, consulte [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).


Para crear una aplicación que utiliza la autenticación integrada de Windows, seleccione la plantilla de "Aplicación de Intranet" en el Asistente para proyecto de MVC 4. Esta plantilla de proyecto establece la siguiente configuración en el archivo Web.config:

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

En el lado del cliente, la autenticación integrada de Windows funciona con cualquier explorador que admita la [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) esquema de autenticación, que incluye la mayoría de los exploradores principal. Para aplicaciones de cliente. NET, el **HttpClient** clase admite la autenticación de Windows:

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

Autenticación de Windows es vulnerable a ataques de falsificación (CSRF) de solicitud entre sitios. Vea [prevención de ataques de falsificación (CSRF) de solicitud entre sitios](preventing-cross-site-request-forgery-csrf-attacks.md).
