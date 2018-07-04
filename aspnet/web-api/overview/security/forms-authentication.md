---
uid: web-api/overview/security/forms-authentication
title: Autenticación de formularios en ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: Describe cómo usar la autenticación de formularios en ASP.NET Web API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 7642a30816a04a88a25ef8bf4f01e766fda362ce
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365076"
---
<a name="forms-authentication-in-aspnet-web-api"></a>Autenticación de formularios en ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

Autenticación de formularios usa un formulario HTML para enviar las credenciales del usuario al servidor. No es un estándar de Internet. Autenticación mediante formularios sólo es adecuada para las API web que se llaman desde una aplicación web, para que el usuario puede interactuar con el formulario HTML.

| Ventajas | Desventajas |
| --- | --- |
| -Fácil de implementar: integrado en ASP.NET. : Usa el proveedor de pertenencia ASP.NET, lo que facilita la administración de cuentas de usuario. | -No un estándar mecanismo de autenticación HTTP; usa las cookies HTTP en lugar del encabezado de autorización estándar. -Requiere un explorador del cliente. -Las credenciales se envían como texto sin formato. -Vulnerable la falsificación de solicitud entre sitios (CSRF); requiere las medidas de anti-CSRF. -Difíciles de usar desde clientes nonbrowser. Inicio de sesión requiere un explorador. : Las credenciales de usuario se envían en la solicitud. -Algunos usuarios deshabilitan las cookies. |

En pocas palabras, la autenticación de formularios en ASP.NET funciona del siguiente modo:

1. El cliente solicita un recurso que requiere autenticación.
2. Si el usuario no está autenticado, el servidor devuelve HTTP 302 (encontrado) y redirige a una página de inicio de sesión.
3. El usuario escriba las credenciales y envía el formulario.
4. El servidor devuelve otro HTTP 302 que redirige a la dirección URI original. Esta respuesta incluye una cookie de autenticación.
5. El cliente solicita el recurso nuevo. La solicitud incluye la cookie de autenticación, por lo que el servidor concede la solicitud.

![](forms-authentication/_static/image1.png)

Para obtener más información, consulte [una visión general de autenticación mediante formularios.](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>Mediante la autenticación de formularios con la API Web

Para crear una aplicación que utiliza la autenticación de formularios, seleccione la plantilla "Aplicación de Internet" en el Asistente para proyectos de MVC 4. Esta plantilla crea controladores MVC para la administración de cuenta. También puede usar la plantilla "Aplicación de página única", disponible en la actualización de 2012 de otoño de ASP.NET.

En los controladores web API, puede restringir el acceso mediante el uso de la `[Authorize]` de atributo, como se describe en [utilizando el atributo [Authorize]](authentication-and-authorization-in-aspnet-web-api.md#auth3).

Autenticación de formularios utiliza una cookie de sesión para autenticar las solicitudes. Los exploradores envían automáticamente todas las cookies relevantes al sitio web de destino. Esta característica hace que la autenticación de formularios potencialmente vulnerables a forgery (CSRF) attacks vea de solicitud entre sitios [ataques de falsificación de solicitud de entre sitios (CSRF) impide](preventing-cross-site-request-forgery-csrf-attacks.md).

Autenticación de formularios no cifra las credenciales del usuario. Por lo tanto, la autenticación de formularios no es segura a menos que se puede usar con SSL. Consulte [trabajar con SSL en Web API](working-with-ssl-in-web-api.md).
