---
uid: web-api/overview/security/forms-authentication
title: Autenticación de formularios en ASP.NET Web API | Documentos de Microsoft
author: MikeWasson
description: Describe cómo utilizar la autenticación de formularios en ASP.NET Web API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 9027d76bcf8854fc85f11906d3651511f350cd32
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508314"
---
<a name="forms-authentication-in-aspnet-web-api"></a>Autenticación de formularios en ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

Autenticación de formularios usa un formulario HTML para enviar las credenciales del usuario al servidor. No es un estándar de Internet. Autenticación de formularios solo es adecuada para las API que se llaman desde una aplicación web, web para que el usuario puede interactuar con el formato HTML.

| Ventajas | Desventajas |
| --- | --- |
| -Fácil de implementar: integrado en ASP.NET. -Utiliza el proveedor de pertenencia ASP.NET, lo que facilita el proceso administrar las cuentas de usuario. | -No un estándar mecanismo de autenticación HTTP; Utilice cookies HTTP en lugar del encabezado de autorización estándar. -Requiere un explorador del cliente. -Las credenciales se envían como texto sin formato. -Vulnerable la falsificación de solicitud entre sitios (CSRF); requiere medidas anti-CSRF. -Difícil su uso de los clientes de nonbrowser. Inicio de sesión requiere un explorador. -Las credenciales de usuario se envían en la solicitud. -Algunos usuarios deshabilitan las cookies. |

En pocas palabras, la autenticación de formularios ASP.NET funciona del siguiente modo:

1. El cliente solicita un recurso que requiere autenticación.
2. Si el usuario no está autenticado, el servidor devuelve HTTP 302 (encontrado) y redirige a una página de inicio de sesión.
3. El usuario introduce credenciales y envía el formulario.
4. El servidor devuelve otro HTTP 302 que redirige a la dirección URI original. Esta respuesta incluye una cookie de autenticación.
5. El cliente solicita el recurso de nuevo. La solicitud incluye la cookie de autenticación, por lo que el servidor concede la solicitud.

![](forms-authentication/_static/image1.png)

Para obtener más información, vea [una visión general de autenticación mediante formularios.](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>Usar autenticación de formularios con la API Web

Para crear una aplicación que utiliza la autenticación de formularios, seleccione la plantilla de "Aplicación de Internet" en el Asistente para proyecto de MVC 4. Esta plantilla crea controladores MVC para la administración de cuentas. También puede usar la plantilla de "Aplicación de página única", disponible en la actualización de ASP.NET otoño de 2012.

En los controladores de la API web, puede restringir el acceso mediante el uso de la `[Authorize]` de atributo, como se describe en [con el atributo [Authorize]](authentication-and-authorization-in-aspnet-web-api.md#auth3).

Autenticación de formularios utiliza una cookie de sesión para autenticar las solicitudes. Exploradores envían automáticamente todas las cookies relevantes para el sitio web de destino. Esta característica hace que la autenticación de formularios potencialmente vulnerable a la solicitud, vea los ataques de falsificación (CSRF) a través de sitios [ataques de falsificación de solicitud entre sitios impide (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).

Autenticación de formularios no cifra las credenciales del usuario. Por lo tanto, la autenticación de formularios no es segura a menos que use con SSL. Vea [trabajar con SSL en Web API](working-with-ssl-in-web-api.md).
