---
uid: web-api/overview/security/basic-authentication
title: Autenticación básica en ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: Describe cómo usar la autenticación básica en ASP.NET Web API.
ms.author: aspnetcontent
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 035baec7c56c0bf6eaacd26ea5192faf2ed6e932
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829592"
---
<a name="basic-authentication-in-aspnet-web-api"></a>Autenticación básica en ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

Autenticación básica se define en [RFC 2617, HTTP Authentication: Basic and Digest Authentication de acceso](http://www.ietf.org/rfc/rfc2617.txt).

Desventajas

- Las credenciales de usuario se envían en la solicitud.
- Las credenciales se envían como texto sin formato.
- Las credenciales se envían con cada solicitud.
- No se puede cerrar la sesión, excepto al finalizar la sesión del explorador.
- Vulnerable a entre sitios (CSRF); la falsificación de solicitudes requiere las medidas de anti-CSRF.

Ventajas

- Estándar de Internet.
- Compatible con todos los exploradores principales.
- Protocolo relativamente sencillo.

Autenticación básica funciona del siguiente modo:

1. Si una solicitud requiere autenticación, el servidor devuelve 401 (no autorizado). La respuesta incluye un encabezado WWW-Authenticate, que indica que el servidor admite la autenticación básica.
2. El cliente envía otra solicitud, con las credenciales del cliente en el encabezado de autorización. Las credenciales tienen el formato de la cadena "nombre: password", con codificación base64. No se cifran las credenciales.

Se realiza la autenticación básica en el contexto de un dominio "Kerberos". El servidor incluye el nombre del territorio en el encabezado WWW-Authenticate. Las credenciales del usuario son válidas dentro de ese dominio. El alcance exacto de un dominio Kerberos se define por el servidor. Por ejemplo, podría definir varios dominios con el fin de dividir los recursos.

![](basic-authentication/_static/image1.png)

Dado que las credenciales se envían sin cifradas, autenticación básica solo es segura a través de HTTPS. Consulte [trabajar con SSL en Web API](working-with-ssl-in-web-api.md).

La autenticación básica también es vulnerable a ataques CSRF. Después de que el usuario escribe las credenciales, el explorador envía automáticamente les en solicitudes posteriores al mismo dominio, para la duración de la sesión. Esto incluye las solicitudes AJAX. Consulte [prevención de ataques de solicitud entre sitios (CSRF) falsificación](preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="basic-authentication-with-iis"></a>Autenticación básica con IIS

IIS admite la autenticación básica, pero hay una advertencia: el usuario se autentica con sus credenciales de Windows. Esto significa que el usuario debe tener una cuenta en el dominio del servidor. Para un sitio web público, normalmente desea autenticarse en un proveedor de pertenencia ASP.NET.

Para habilitar la autenticación básica con IIS, establezca el modo de autenticación en "Windows" en el archivo Web.config de su proyecto de ASP.NET:

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

En este modo, IIS usa credenciales de Windows para autenticarse. Además, debe habilitar la autenticación básica en IIS. En el Administrador de IIS, vaya a la vista de características, seleccione autenticación y habilitar la autenticación básica.

![](basic-authentication/_static/image2.png)

En el proyecto Web API, agregue el `[Authorize]` atributo para todas las acciones de controlador que necesita la autenticación.

Un cliente se autentica mediante el establecimiento del encabezado de autorización en la solicitud. Los clientes de explorador realizan este paso automáticamente. Los clientes nonbrowser deberá establecer el encabezado.

## <a name="basic-authentication-with-custom-membership"></a>Autenticación básica con pertenencia personalizado

Como se mencionó, la autenticación básica integrada en IIS utiliza credenciales de Windows. Esto significa que deberá crear cuentas para los usuarios en el servidor de hospedaje. Pero para una aplicación de internet, las cuentas de usuario suelen almacenarse en una base de datos externo.

El siguiente código cómo un módulo HTTP que realiza la autenticación básica. Puede conectar fácilmente en un proveedor de pertenencia ASP.NET si se reemplaza el `CheckPassword` método, que es un método ficticio en este ejemplo.

En la Web API 2, puede escribir un [filtro de autenticación](authentication-filters.md) o [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), en lugar de un módulo HTTP.

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

Para habilitar el módulo HTTP, agregue lo siguiente al archivo web.config en el **system.webServer** sección:

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

Reemplace "Nombre_del_ensamblado" por el nombre del ensamblado (sin incluir la extensión "dll").

Debe deshabilitar otros esquemas de autenticación, como autenticación de formularios o Windows.
