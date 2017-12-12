---
uid: web-api/overview/security/basic-authentication
title: "La autenticación básica en ASP.NET Web API | Documentos de Microsoft"
author: MikeWasson
description: "Describe cómo utilizar la autenticación básica en ASP.NET Web API."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 4b8e6410668b2db289488bb4b6cd26d881e70f4c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="basic-authentication-in-aspnet-web-api"></a>Autenticación básica en ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

La autenticación básica se define en [RFC 2617, HTTP Authentication: Basic and Digest Access Authentication](http://www.ietf.org/rfc/rfc2617.txt).

Desventajas

- Las credenciales de usuario se envían en la solicitud.
- Las credenciales se envían como texto sin formato.
- Las credenciales se envían con cada solicitud.
- Ninguna manera de cerrar la sesión, excepto para finalizar la sesión del explorador.
- Vulnerable a través de sitios (CSRF); la falsificación de solicitudes requiere medidas anti-CSRF.

Ventajas

- Estándar de Internet.
- Compatible con todos los exploradores principales.
- Protocolo relativamente sencilla.

La autenticación básica funciona del siguiente modo:

1. Si una solicitud requiere autenticación, el servidor devuelve 401 (no autorizado). La respuesta incluye un encabezado WWW-Authenticate, que indica que el servidor admite la autenticación básica.
2. El cliente envía otra solicitud, con las credenciales del cliente en el encabezado de autorización. Las credenciales se da formato como la cadena "nombre: password", con codificación base64. No se cifran las credenciales.

La autenticación básica se realiza en el contexto de un "dominio". El servidor incluye el nombre del territorio en el encabezado WWW-Authenticate. Las credenciales del usuario son válidas dentro de ese dominio. El ámbito exacto de un dominio Kerberos se define por el servidor. Por ejemplo, podría definir varios de los dominios en orden a los recursos de la partición.

![](basic-authentication/_static/image1.png)

Dado que las credenciales se envían sin cifrar, la autenticación básica solo es segura a través de HTTPS. Vea [trabajar con SSL en Web API](working-with-ssl-in-web-api.md).

La autenticación básica también es vulnerable a ataques CSRF. Cuando el usuario introduce credenciales, el explorador envía automáticamente su en solicitudes posteriores al mismo dominio, para la duración de la sesión. Esto incluye las solicitudes AJAX. Vea [prevención de ataques de falsificación (CSRF) de solicitud entre sitios](preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="basic-authentication-with-iis"></a>Autenticación básica con IIS

IIS admite la autenticación básica, pero no hay una advertencia: el usuario se autentica con sus credenciales de Windows. Esto significa que el usuario debe tener una cuenta en el dominio del servidor. Para un sitio web de acceso público, normalmente desea autenticarse en un proveedor de pertenencia ASP.NET.

Para habilitar la autenticación básica con IIS, establezca el modo de autenticación en "Windows" en el archivo Web.config de su proyecto de ASP.NET:

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

En este modo, IIS utiliza credenciales de Windows para autenticar. Además, debe habilitar la autenticación básica en IIS. En el Administrador de IIS, vaya a la vista de características, seleccione la autenticación y habilitar la autenticación básica.

![](basic-authentication/_static/image2.png)

En el proyecto Web API, agregue el `[Authorize]` atributo para todas las acciones de controlador que requieran autenticación.

Un cliente se autentica estableciendo el encabezado de autorización en la solicitud. Los clientes de explorador realizan este paso automáticamente. Los clientes de nonbrowser deberá establecer el encabezado.

## <a name="basic-authentication-with-custom-membership"></a>Autenticación básica con pertenencia personalizada

Como se mencionó, la autenticación básica integrado en IIS utiliza credenciales de Windows. Esto significa que necesita crear cuentas para los usuarios en el servidor de hospedaje. Pero para una aplicación de internet, las cuentas de usuario suelen almacenarse en una base de datos externo.

El siguiente código cómo un módulo HTTP que realiza la autenticación básica. Puede conectar fácilmente en un proveedor de pertenencia ASP.NET si se reemplaza el `CheckPassword` método, que es un método ficticio en este ejemplo.

En la API Web 2, puede escribir una [filtro de autenticación](authentication-filters.md) o [middleware de OWIN](../../../aspnet/overview/owin-and-katana/index.md), en lugar de un módulo HTTP.

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

Para habilitar el módulo HTTP, agregue lo siguiente al archivo web.config en el **system.webServer** sección:

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

Reemplace "YourAssemblyName" por el nombre del ensamblado (sin incluir la extensión "dll").

Debe deshabilitar otros esquemas de autenticación, como autenticación de formularios o Windows.
