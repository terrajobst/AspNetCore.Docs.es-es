---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Habilitar la autenticación de Windows en Katana | Microsoft Docs
author: MikeWasson
description: 'En este artículo se muestra cómo habilitar la autenticación de Windows en Katana. Trata dos escenarios: usar IIS para hospedar Katana y con HttpListener autohospedaje Kat...'
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 0aa578020a1f02fa68c74e758014c642219b4265
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827962"
---
<a name="enabling-windows-authentication-in-katana"></a>Habilitar la autenticación de Windows en Katana
====================
por [Mike Wasson](https://github.com/MikeWasson)

> En este artículo se muestra cómo habilitar la autenticación de Windows en Katana. Trata dos escenarios: usar IIS para hospedar Katana y con HttpListener autohospedaje Katana en un proceso personalizado. Gracias a Chris Ross, David Matson y Barry Dorrans por revisar este artículo.


Katana es la implementación de Microsoft [OWIN](http://owin.org/), la interfaz Web abierta para. NET. Puede leer una introducción a OWIN y Katana [aquí](an-overview-of-project-katana.md). La arquitectura OWIN consta de varios niveles:

- Host: Administra el proceso en el que se ejecuta la canalización de OWIN.
- Servidor: Abre un socket de red y escucha las solicitudes.
- Middleware: Procesa la solicitud y respuesta HTTP.

Katana ofrece actualmente dos servidores, los cuales admiten la autenticación integrada de Windows:

- **Microsoft.Owin.Host.SystemWeb**. Usa IIS con la canalización de ASP.NET.
- **Microsoft.Owin.Host.HttpListener**. Usa [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx). Este servidor actualmente es la opción predeterminada cuando se autohospeda Katana.

> [!NOTE]
> Katana no actualmente proporciona middleware de OWIN para la autenticación de Windows, porque esta funcionalidad ya está disponible en los servidores.


## <a name="windows-authentication-in-iis"></a>Autenticación de Windows en IIS

Usar Microsoft.Owin.Host.SystemWeb, simplemente puede habilitar la autenticación de Windows en IIS.

Comencemos por crear una nueva aplicación de ASP.NET mediante la plantilla de proyecto "Aplicación Web ASP.NET vacía".

![](enabling-windows-authentication-in-katana/_static/image1.png)

A continuación, agregue los paquetes de NuGet. Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**. En la ventana de consola de administrador de paquetes, escriba el siguiente comando:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

Ahora agregue una clase denominada `Startup` con el código siguiente:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

Eso es todo lo que necesita para crear una aplicación "Hello world" para OWIN, que se ejecutan en IIS. Presione F5 para depurar la aplicación. Debería ver "Hello World!" en la ventana del explorador.

![](enabling-windows-authentication-in-katana/_static/image2.png)

A continuación, permitimos la autenticación de Windows en IIS Express. Desde el **vista** menú, seleccione **propiedades**. Haga clic en el nombre del proyecto en el Explorador de soluciones para ver las propiedades del proyecto.

En el **propiedades** ventana, establezca **autenticación anónima** a **deshabilitado** y establecer **Windows autenticación** a  **Habilitado**.

![](enabling-windows-authentication-in-katana/_static/image3.png)

Al ejecutar la aplicación desde Visual Studio, IIS Express requiere credenciales de Windows del usuario. Puede ver esto mediante el uso de [Fiddler](http://fiddler2.com/home) o HTTP otra herramienta de depuración. Este es un ejemplo de respuesta HTTP:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

Los encabezados en esta respuesta WWW-Authenticate indican que el servidor admite el [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocolo, que se usa Kerberos o NTLM.

Más adelante, al implementar la aplicación en un servidor, siga [estos pasos](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) para habilitar la autenticación de Windows en IIS en ese servidor.

## <a name="windows-authentication-in-httplistener"></a>Autenticación de Windows en HttpListener

Si usas Microsoft.Owin.Host.HttpListener autohospedaje Katana, puede habilitar la autenticación de Windows directamente en el **HttpListener** instancia.

En primer lugar, cree una nueva aplicación de consola. A continuación, agregue los paquetes de NuGet. Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**. En la ventana de consola de administrador de paquetes, escriba el siguiente comando:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

Ahora agregue una clase denominada `Startup` con el código siguiente:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

Esta clase implementa el mismo ejemplo "Hello world" de antes, pero también establece la autenticación de Windows como el esquema de autenticación.

Dentro de la `Main` de función, el inicio de la canalización OWIN:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

Puede enviar una solicitud en Fiddler para confirmar que la aplicación utiliza la autenticación de Windows:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>Temas relacionados

[Información general del proyecto Katana](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[Introducción a la autenticación de formularios OWIN en MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
