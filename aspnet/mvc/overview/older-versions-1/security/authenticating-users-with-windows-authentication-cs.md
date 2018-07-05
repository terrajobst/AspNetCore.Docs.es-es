---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
title: Autenticar a los usuarios con la autenticación de Windows (C#) | Microsoft Docs
author: microsoft
description: Obtenga información sobre cómo usar la autenticación de Windows en el contexto de una aplicación MVC. Aprenda a habilitar la autenticación de Windows dentro de co de su aplicación web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 418bb07e-f369-4119-b4b0-08f890f7abb2
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: 0261f3b989d721e6e9aae8fa5766e02c8bca7b63
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397816"
---
<a name="authenticating-users-with-windows-authentication-c"></a>Autenticar a los usuarios con la autenticación de Windows (C#)
====================
por [Microsoft](https://github.com/microsoft)

> Obtenga información sobre cómo usar la autenticación de Windows en el contexto de una aplicación MVC. Obtenga información sobre cómo habilitar la autenticación de Windows en el archivo de configuración de la aplicación web y cómo configurar la autenticación con IIS. Por último, aprenderá a usar el atributo [Authorize] para restringir el acceso a las acciones de controlador a grupos o usuarios de Windows determinados.


El objetivo de este tutorial es explicar cómo puede aprovechar las ventajas de la seguridad de características integradas en Internet Information Services para la contraseña de protegen las vistas en las aplicaciones MVC. Obtendrá información sobre cómo permitir que las acciones de controlador se invoca sólo determinados usuarios de Windows o los usuarios que son miembros de grupos de Windows determinados.

Mediante la autenticación de Windows que tiene sentido cuando está creando un sitio Web de empresa interna (un sitio de intranet) y desea que los usuarios puedan usar sus nombres de usuario de Windows estándar y contraseñas al acceso al sitio Web. Si está creando un hacia afuera accesible desde el sitio Web (un sitio Web de Internet), considere la posibilidad de usar la autenticación de formularios en su lugar.

#### <a name="enabling-windows-authentication"></a>Habilitar la autenticación de Windows

Cuando se crea una nueva aplicación MVC de ASP.NET, autenticación de Windows no está habilitada de forma predeterminada. Autenticación mediante formularios es el tipo de autenticación predeterminada habilitado para las aplicaciones MVC. Debe habilitar la autenticación de Windows modificando el archivo de configuración (web.config) de la aplicación MVC web. Buscar el &lt;autenticación&gt; sección y modifíquelo para usar Windows en lugar de la autenticación de formularios similar al siguiente:

[!code-xml[Main](authenticating-users-with-windows-authentication-cs/samples/sample1.xml)]

Al habilitar la autenticación de Windows, el servidor web pasa a ser responsable de autenticar a los usuarios. Normalmente, hay dos tipos diferentes de servidores web que utilizan al crear e implementar una aplicación ASP.NET MVC.

En primer lugar, al desarrollar una aplicación de MVC, use el servidor de Web de desarrollo de ASP.NET incluido con Visual Studio. De forma predeterminada, el servidor Web de desarrollo de ASP.NET ejecuta todas las páginas en el contexto de la cuenta de Windows actual (es decir, cualquier cuenta que usó para iniciar sesión en Windows).

El servidor Web de desarrollo de ASP.NET también admite la autenticación NTLM. Puede habilitar la autenticación NTLM haciendo clic en el nombre del proyecto en la ventana Explorador de soluciones y seleccione Propiedades. A continuación, seleccione la pestaña Web y Active la casilla NTLM (consulte la figura 1).

**Ilustración 1: habilitar la autenticación NTLM para el servidor Web de desarrollo de ASP.NET**

![clip_image002](authenticating-users-with-windows-authentication-cs/_static/image1.jpg)

Para una aplicación web de producción, en la mano, utiliza IIS como servidor web. IIS admite varios tipos de autenticación, incluidos:

- Autenticación básica: se define como parte del protocolo HTTP 1.0. Envía los nombres de usuario y contraseñas en texto no cifrado (con codificación Base64) a través de Internet. -La autenticación implícita: envía un hash de una contraseña, en lugar de la contraseña, a través de internet. -Autenticación de Windows (NTLM) integrado: el mejor tipo de autenticación que desea usar en entornos de intranet con windows. -Certificado de autenticación: habilita la autenticación mediante un certificado de cliente. El certificado se asigna a una cuenta de usuario de Windows.

> [!NOTE] 
> 
> Para obtener una descripción más detallada de estos diferentes tipos de autenticación, consulte [ https://msdn.microsoft.com/library/aa292114(VS.71).aspx ](https://msdn.microsoft.com/library/aa292114(VS.71).aspx).


Puede usar el Administrador de Internet Information Services para habilitar a un determinado tipo de autenticación. Tenga en cuenta que todos los tipos de autenticación no están disponibles en el caso de cada sistema operativo. Además, si usa IIS 7.0 con Windows Vista, deberá habilitar a los diferentes tipos de autenticación de Windows antes de que aparezcan en el Administrador de Internet Information Services. Abra **Panel de Control, programas, programas y características, activar o desactivar las características de Windows Active**y expanda el nodo de Internet Information Services (consulte la figura 2).

**Ilustración 2: características de habilitar IIS de Windows**

![clip_image004](authenticating-users-with-windows-authentication-cs/_static/image2.jpg)

Mediante Internet Information Services, puede habilitar o deshabilitar a distintos tipos de autenticación. Por ejemplo, la figura 3 ilustra deshabilitar autenticación anónima y habilitar la autenticación integrada de Windows (NTLM) cuando se usa IIS 7.0.

**Ilustración 3: habilitación de la autenticación integrada de Windows**

![clip_image006](authenticating-users-with-windows-authentication-cs/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Autorización de Windows a los usuarios y grupos

Después de habilitar la autenticación de Windows, puede usar el atributo [Authorize] para controlar el acceso a controladores o acciones de controlador. Este atributo puede aplicarse a un controlador MVC completo o una acción de controlador determinado.

Por ejemplo, el controlador Home en el listado 1 expone tres acciones denominadas Index() CompanySecrets() y StephenSecrets(). Cualquier usuario puede invocar la acción de Index(). Sin embargo, solo los miembros del grupo de administradores locales de Windows pueden invocar la acción CompanySecrets(). Por último, solo el usuario de dominio de Windows denominado a Stephen (en el dominio Redmond) puede invocar la acción StephenSecrets().

**Listado 1 – controllers\homecontroller. cs**

[!code-csharp[Main](authenticating-users-with-windows-authentication-cs/samples/sample2.cs)]

> [!NOTE] 
> 
> Debido a Windows cuentas de usuario Control (UAC), cuando se trabaja con Windows Vista o Windows Server 2008, el grupo de administradores locales se comportará de forma diferente a otros grupos. El atributo [Authorize] no reconoce correctamente un miembro del grupo Administradores local a menos que modifique la configuración del equipo UAC.


Exactamente lo que sucede cuando se intenta invocar una acción de controlador sin que se va a los permisos adecuados depende del tipo de autenticación habilitado. De forma predeterminada, cuando se usa el servidor de desarrollo de ASP.NET, obtener simplemente una página en blanco. Se sirve la página con un **401 no autorizado** estado de respuesta HTTP.

Si, por otro lado, está utilizando IIS con deshabilitada la autenticación anónima y la autenticación básica habilitada, a continuación, sigue apareciendo un mensaje del cuadro de diálogo de inicio de sesión cada vez que solicite la página protegida (consulte la figura 4).

**Figura 4: cuadro de diálogo de inicio de sesión de autenticación básica**

![clip_image008](authenticating-users-with-windows-authentication-cs/_static/image4.jpg)

#### <a name="summary"></a>Resumen

En este tutorial se explica cómo puede usar la autenticación de Windows en el contexto de una aplicación ASP.NET MVC. Ha aprendido cómo habilitar la autenticación de Windows en el archivo de configuración de la aplicación web y cómo configurar la autenticación con IIS. Por último, ha aprendido cómo utilizar el atributo [Authorize] para restringir el acceso a las acciones de controlador a grupos o usuarios de Windows determinados.

> [!div class="step-by-step"]
> [Anterior](authenticating-users-with-forms-authentication-cs.md)
> [Siguiente](preventing-javascript-injection-attacks-cs.md)
