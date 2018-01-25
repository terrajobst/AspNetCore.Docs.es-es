---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: "Habilitar la autenticación de Windows en Katana | Documentos de Microsoft"
author: MikeWasson
description: "Este artículo muestra cómo habilitar la autenticación de Windows en Katana. Incluye dos escenarios: usar IIS al host de Katana y usar HttpListener para autohospedaje Kat..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 8a26d356f7abafba021199761f9a49dcb81765c5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="enabling-windows-authentication-in-katana"></a>Habilitar la autenticación de Windows en Katana
====================
por [Mike Wasson](https://github.com/MikeWasson)

> Este artículo muestra cómo habilitar la autenticación de Windows en Katana. Incluye dos escenarios: usar IIS al host de Katana y usar HttpListener para autohospedaje Katana en un proceso personalizado. Gracias a Barry Dorrans, David Matson y Chris Ross de revisión de este artículo.


Katana es la implementación de Microsoft [OWIN](http://owin.org/), la interfaz Web abierta para. NET. Puede leer una introducción a OWIN y Katana [aquí](an-overview-of-project-katana.md). La arquitectura OWIN consta de varios niveles:

- Host: Administra el proceso en el que se ejecuta la canalización OWIN.
- Servidor: Abre un socket de red y escucha las solicitudes.
- Middleware: Procesa la solicitud y respuesta HTTP.

Katana actualmente proporciona dos servidores, los cuales admiten la autenticación integrada de Windows:

- **Microsoft.Owin.Host.SystemWeb**. Usa IIS con la canalización ASP.NET.
- **Microsoft.Owin.Host.HttpListener**. Usa [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx). Este servidor actualmente es la opción predeterminada cuando se hospeda a sí mismo Katana.

> [!NOTE]
> Katana no actualmente proporciona middleware de OWIN para la autenticación de Windows, porque esta funcionalidad ya está disponible en los servidores.


## <a name="windows-authentication-in-iis"></a>Autenticación de Windows en IIS

Con Microsoft.Owin.Host.SystemWeb, simplemente puede habilitar la autenticación de Windows en IIS.

Empecemos creando una nueva aplicación de ASP.NET, mediante la plantilla de proyecto de "Aplicación Web ASP.NET vacía".

![](enabling-windows-authentication-in-katana/_static/image1.png)

A continuación, agregar paquetes de NuGet. Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**. En la ventana de la consola de administrador de paquetes, escriba el siguiente comando:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

Ahora agregue una clase denominada `Startup` con el código siguiente:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

Que es todo lo que necesita para crear una aplicación "Hello world" para OWIN, que se ejecuta en IIS. Presione F5 para depurar la aplicación. Debería ver "¡Hello World!" en la ventana del explorador.

![](enabling-windows-authentication-in-katana/_static/image2.png)

A continuación, se habilitará la autenticación de Windows en IIS Express. Desde el **vista** menú, seleccione **propiedades**. Haga clic en el nombre del proyecto en el Explorador de soluciones para ver las propiedades del proyecto.

En el **propiedades** ventana, establezca **autenticación anónima** a **deshabilitado** y establecer **autenticación de Windows** a  **Habilitado**.

![](enabling-windows-authentication-in-katana/_static/image3.png)

Al ejecutar la aplicación desde Visual Studio, IIS Express requiere credenciales de Windows del usuario. Puede ver mediante el uso de [Fiddler](http://fiddler2.com/home) o HTTP otra herramienta de depuración. Este es un ejemplo de respuesta HTTP:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

Los encabezados WWW-Authenticate en esta respuesta indican que el servidor admite la [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocolo, que se usa Kerberos o NTLM.

Más adelante, cuando se implementa la aplicación en un servidor, realice [estos pasos](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) para habilitar la autenticación de Windows en IIS en ese servidor.

## <a name="windows-authentication-in-httplistener"></a>Autenticación de Windows en HttpListener

Si usas Microsoft.Owin.Host.HttpListener para auto-hospedar Katana, puede habilitar la autenticación de Windows directamente en el **HttpListener** instancia.

En primer lugar, cree una nueva aplicación de consola. A continuación, agregar paquetes de NuGet. Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**. En la ventana de la consola de administrador de paquetes, escriba el siguiente comando:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

Ahora agregue una clase denominada `Startup` con el código siguiente:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

Esta clase implementa el mismo ejemplo de "Hola a todos" de antes, pero también establece la autenticación de Windows como el esquema de autenticación.

Dentro de la `Main` funcionar, iniciar la canalización OWIN:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

Puede enviar una solicitud de Fiddler para confirmar que la aplicación utiliza la autenticación de Windows:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>Temas relacionados

[Información general del proyecto Katana](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[Descripción de la autenticación de formularios OWIN en MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
