---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
title: "Autenticar a los usuarios con la autenticación de Windows (C#) | Documentos de Microsoft"
author: microsoft
description: "Obtenga información acerca de cómo utilizar la autenticación de Windows en el contexto de una aplicación MVC. Obtenga información acerca de cómo habilitar la autenticación de Windows dentro de co de la aplicación web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 418bb07e-f369-4119-b4b0-08f890f7abb2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: 575fb382cc758efb101485bd5aece461bf995bfc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="authenticating-users-with-windows-authentication-c"></a>Autenticar a los usuarios con la autenticación de Windows (C#)
====================
por [Microsoft](https://github.com/microsoft)

> Obtenga información acerca de cómo utilizar la autenticación de Windows en el contexto de una aplicación MVC. Obtenga información acerca de cómo habilitar la autenticación de Windows en el archivo de configuración de la aplicación web y cómo configurar la autenticación con IIS. Por último, aprenderá cómo utilizar el atributo [Authorize] para restringir el acceso a las acciones de controlador a grupos o usuarios concretos de Windows.


El objetivo de este tutorial es explicar cómo puede aprovechar las ventajas de la seguridad de características integradas en servicios de Internet Information Server para la contraseña de protegen las vistas en las aplicaciones de MVC. Obtenga información acerca de cómo permitir que las acciones de controlador para que invoque solo determinados usuarios de Windows o los usuarios que son miembros de grupos de Windows determinados.

Mediante la autenticación de Windows tiene sentido cuando está creando un sitio Web de empresa interna (un sitio de intranet) y desea que los usuarios puedan usar sus nombres de usuario de Windows estándar y contraseñas al tener acceso a lo sitios Web. Si está generando un hacia afuera orientada hacia el sitio Web (un sitio Web de Internet), considere la posibilidad de usar autenticación de formularios en su lugar.

#### <a name="enabling-windows-authentication"></a>Habilitar la autenticación de Windows

Cuando se crea una nueva aplicación MVC de ASP.NET, autenticación de Windows no está habilitada de forma predeterminada. Autenticación mediante formularios es el tipo de autenticación predeterminado habilitado para las aplicaciones MVC. Debe habilitar la autenticación de Windows mediante la modificación de archivo de configuración (web.config) de web de su aplicación de MVC. Buscar el &lt;autenticación&gt; sección y modifíquela para que utilice Windows en lugar de autenticación de formularios similar al siguiente:

[!code-xml[Main](authenticating-users-with-windows-authentication-cs/samples/sample1.xml)]

Cuando se habilita la autenticación de Windows, el servidor web pasa a ser responsable de autenticar a los usuarios. Normalmente, hay dos tipos diferentes de servidores web que utilizan al crear e implementar una aplicación ASP.NET MVC.

En primer lugar, al desarrollar una aplicación de MVC, use el servidor de Web de desarrollo de ASP.NET incluido con Visual Studio. De forma predeterminada, el servidor Web de desarrollo de ASP.NET ejecuta todas las páginas en el contexto de la cuenta de Windows actual (es decir, la cuenta utilizada para iniciar sesión en Windows).

El servidor Web de desarrollo de ASP.NET también admite la autenticación NTLM. Puede habilitar la autenticación NTLM haciendo clic en el nombre del proyecto en la ventana Explorador de soluciones y seleccione Propiedades. A continuación, seleccione la pestaña Web y Active la casilla NTLM (consulte la figura 1).

**Ilustración 1: habilitar la autenticación NTLM para el servidor Web de desarrollo de ASP.NET**

![clip_image002](authenticating-users-with-windows-authentication-cs/_static/image1.jpg)

Para una aplicación web de producción, en la mano, utiliza IIS como servidor web. IIS admite varios tipos de autenticación, incluidos:

- La autenticación básica: se define como parte del protocolo HTTP 1.0. Envía los nombres de usuario y contraseñas en texto sin cifrar (codificados con Base64) a través de Internet. -La autenticación implícita: envía un hash de una contraseña, en lugar de la propia contraseña a través de internet. -Autenticación de Windows (NTLM) integrada: el mejor tipo de autenticación que desea usar en entornos de intranet con windows. -Autenticación: habilita la autenticación mediante un certificado de cliente de certificados. El certificado se asigna a una cuenta de usuario de Windows.

> [!NOTE] 
> 
> Para obtener una descripción más detallada de estos diferentes tipos de autenticación, vea [https://msdn.microsoft.com/en-us/library/aa292114(VS.71).aspx](https://msdn.microsoft.com/en-us/library/aa292114(VS.71).aspx).


Puede usar el Administrador de Internet Information Services para habilitar a un determinado tipo de autenticación. Tenga en cuenta que todos los tipos de autenticación no están disponibles en el caso de todos los sistemas operativos. Además, si usa IIS 7.0 con Windows Vista, debe habilitar a los diferentes tipos de autenticación de Windows para que aparezcan en el Administrador de Internet Information Services. Abra **el Panel de Control, programas, programas y características, activar o desactivar las características de Windows Active**y expanda el nodo Servicios de Internet Information Server (consulte la figura 2).

**Figura 2: características de Windows si habilita IIS**

![clip_image004](authenticating-users-with-windows-authentication-cs/_static/image2.jpg)

Con servicios de Internet Information Server, puede habilitar o deshabilitar a distintos tipos de autenticación. Por ejemplo, la figura 3 muestra la autenticación anónima, deshabilitar y habilitar la autenticación integrada de Windows (NTLM) cuando se usa IIS 7.0.

**Figura 3: habilitar la autenticación integrada de Windows**

![clip_image006](authenticating-users-with-windows-authentication-cs/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Autorización de Windows a los usuarios y grupos

Después de habilitar la autenticación de Windows, puede utilizar el atributo [Authorize] para controlar el acceso a los controladores o las acciones de controlador. Este atributo se puede aplicar a un controlador MVC completo o una acción de controlador determinado.

Por ejemplo, el controlador Home en el listado 1 expone tres acciones denominadas Index(), CompanySecrets() y StephenSecrets(). Cualquier usuario puede invocar la acción Index(). Sin embargo, solo los miembros del grupo de administradores locales de Windows pueden invocar la acción CompanySecrets(). Por último, solo el usuario de dominio de Windows denominado a Stephen (en el dominio Redmond) puede invocar la acción StephenSecrets().

**Lista 1 – controllers\homecontroller**

[!code-csharp[Main](authenticating-users-with-windows-authentication-cs/samples/sample2.cs)]

> [!NOTE] 
> 
> Debido a Windows cuenta Control usuario (UAC), cuando se trabaja con Windows Vista o Windows Server 2008, el grupo de administradores local se comportará de forma diferente a otros grupos. El atributo [Authorize] no reconoce correctamente un miembro del grupo Administradores local a menos que modifique la configuración del equipo UAC.


Exactamente lo que ocurre cuando se intenta invocar una acción de controlador sin que se va a los permisos adecuados depende del tipo de autenticación habilitado. De forma predeterminada, cuando se usa el servidor de desarrollo de ASP.NET, obtendrá simplemente una página en blanco. Se sirve la página con un **401 no autorizado** estado de respuesta HTTP.

Si, por otro lado, está utilizando IIS con deshabilitada la autenticación anónima y la autenticación básica habilitada y seguir recibiendo un mensaje de diálogo de inicio de sesión cada vez que solicite la página protegida (consulte la figura 4).

**Figura 4: cuadro de diálogo de inicio de sesión de autenticación básica**

![clip_image008](authenticating-users-with-windows-authentication-cs/_static/image4.jpg)

#### <a name="summary"></a>Resumen

Este tutorial explica cómo puede utilizar la autenticación de Windows en el contexto de una aplicación ASP.NET MVC. Aprendió a habilitar la autenticación de Windows en el archivo de configuración de la aplicación web y cómo configurar la autenticación con IIS. Por último, aprendió a utilizar el atributo [Authorize] para restringir el acceso a las acciones de controlador a grupos o usuarios concretos de Windows.

>[!div class="step-by-step"]
[Anterior](authenticating-users-with-forms-authentication-cs.md)
[Siguiente](preventing-javascript-injection-attacks-cs.md)
