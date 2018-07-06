---
uid: web-api/overview/security/integrated-windows-authentication
title: Autenticación de Windows integrada | Microsoft Docs
author: MikeWasson
description: Describe cómo usar la autenticación de Windows integrada en ASP.NET Web API.
ms.author: aspnetcontent
ms.date: 12/18/2012
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: c15677b7aa66619f1ff32819585340ff497b937d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820411"
---
<a name="integrated-windows-authentication"></a>Autenticación integrada de Windows
====================
por [Mike Wasson](https://github.com/MikeWasson)

La autenticación integrada de Windows permite a los usuarios inicien sesión con sus credenciales de Windows, utilizando Kerberos o NTLM. El cliente envía las credenciales en el encabezado de autorización. Autenticación de Windows es más adecuada para un entorno de intranet. Para obtener más información, consulte [Windows autenticación](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).

| Ventajas | Desventajas |
| --- | --- |
| -Integrada en IIS. -No envía las credenciales de usuario en la solicitud. -Si el equipo cliente pertenece al dominio (por ejemplo, la aplicación de intranet), el usuario no es necesario que escriba las credenciales. | -No se recomienda para aplicaciones de Internet. -Requiere soporte técnico de Kerberos o NTLM en el cliente. -Cliente debe estar en el dominio de Active Directory. |

> [!NOTE]
> Si la aplicación está hospedada en Azure y tiene un dominio de Active Directory local, considere la posibilidad de federar su AD local con Azure Active Directory. De este modo, los usuarios pueden iniciar sesión con sus credenciales de forma local, pero la autenticación se realiza por Azure AD. Para obtener más información, consulte [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).


Para crear una aplicación que usa la autenticación de Windows integrada, seleccione la plantilla "Aplicación de Intranet" en el Asistente para proyectos de MVC 4. Esta plantilla de proyecto coloca la siguiente configuración en el archivo Web.config:

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

En el lado cliente, la autenticación de Windows integrada funciona con cualquier explorador que admita la [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) esquema de autenticación, que incluye la mayoría de los exploradores principal. Para las aplicaciones cliente. NET, el **HttpClient** clase admite la autenticación de Windows:

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

Autenticación de Windows es vulnerable a ataques de falsificación (CSRF) de solicitud entre sitios. Consulte [prevención de ataques de solicitud entre sitios (CSRF) falsificación](preventing-cross-site-request-forgery-csrf-attacks.md).
