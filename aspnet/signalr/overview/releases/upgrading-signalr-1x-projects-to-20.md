---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: Actualizar proyectos de 1.x de SignalR a la versión 2 | Documentos de Microsoft
author: pfletcher
description: Este tema describe cómo actualizar un proyecto existente de 1.x de SignalR a SignalR 2.x y cómo solucionar problemas que pueden surgir durante el proceso de actualización...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: e372275ae5dd4bbf354db2d02e4407f8c513b7a3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26505744"
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a>Actualización de proyectos de 1.x SignalR a la versión 2
====================
por [Patrick Fletcher](https://github.com/pfletcher)

> Este tema describe cómo actualizar un proyecto existente de 1.x de SignalR a SignalR 2.x y cómo solucionar problemas que pueden surgir durante el proceso de actualización.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Versiones 1 y 2 de SignalR
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Uso de Visual Studio 2012 con este tutorial
> 
> 
> Para usar Visual Studio 2012 con este tutorial, realice lo siguiente:
> 
> - Actualización de su [el Administrador de paquetes](http://docs.nuget.org/docs/start-here/installing-nuget) a la versión más reciente.
> - Instalar el [instalador de plataforma Web](https://www.microsoft.com/web/downloads/platform.aspx).
> - El instalador de plataforma Web, buscar e instalar **2013.1 de herramientas Web para Visual Studio 2012 y ASP.NET**. Esto instalará como plantillas de Visual Studio para las clases de SignalR **concentrador**.
> - Algunas plantillas (como **clase de inicio de OWIN**) no estará disponible; para ello, utilice un archivo de clase en su lugar.
> 
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


SignalR 2 ofrece una experiencia de desarrollo coherente entre plataformas de servidor mediante [OWIN](http://owin.org). Este artículo describen los pasos necesarios para actualizar una aplicación de 1.x SignalR a la versión 2.

Aunque se recomienda actualizar las aplicaciones al 2 de SignalR, SignalR sigue 1.x admitirse.

Este tutorial describe cómo actualizar una aplicación hospedada en web al 2 de SignalR. Las aplicaciones autohospedadas (aquellas que hospedan un servidor en una aplicación de consola, servicio de Windows u otro proceso) ahora se admiten en SignalR 2. Para obtener información sobre cómo empezar a crear una aplicación autohospedada con SignalR 2, consulte [Tutorial: SignalR autohospedaje](../deployment/tutorial-signalr-self-host.md).

## <a name="contents"></a>Contenido

Las secciones siguientes describen las tareas relacionadas con la actualización de proyectos de SignalR y cómo solucionar problemas que pueden surgir.

- [Ejemplo: Actualizar el tutorial de introducción a SignalR 2](#example)
- [Solución de problemas de errores encontrados durante la actualización](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a>Ejemplo: Actualizar la aplicación de tutorial de introducción para SignalR 2

En esta sección, actualizará la aplicación creada en el [SignalR 1.x versión del Tutorial de introducción](../older-versions/index.md) utilizar 2 de SignalR.

1. Una vez que haya finalizado el tutorial de introducción, haga doble clic en el proyecto y seleccione **propiedades**. Compruebe que la **.NET framework de destino** está establecido en **.NET Framework 4.5.**
2. Abra la consola de administrador de paquetes. Quitar SignalR 1.x desde el proyecto con el comando siguiente:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. Instalar el 2 de SignalR mediante el siguiente comando:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. En la página HTML, actualice la referencia de script para SignalR para que coincida con la versión de la secuencia de comandos que se incluye ahora en el proyecto.

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. En la clase de aplicación global, quite la llamada a MapHubs.

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. Haga clic en la solución y seleccione **agregar**, **nuevo elemento...** . En el cuadro de diálogo, seleccione **clase de inicio de Owin**. La nueva clase el nombre **Startup.cs**.

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. Reemplace el contenido de Startup.cs con el código siguiente:

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    El atributo de ensamblado agrega la clase al proceso de inicio de Owin, que se ejecuta el `Configuration` método cuando se inicia Owin. Esto llama a su vez el `MapSignalR` método, que crea rutas para todos los concentradores de SignalR en la aplicación.
8. Ejecute el proyecto y copie la dirección URL de la página principal en otro explorador o el panel del explorador, como antes. Cada página le pedirá un nombre de usuario y los mensajes enviados desde cada página deben ser visibles en los paneles del explorador.

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a>Solución de problemas de errores encontrados durante la actualización

Esta sección describe problemas que pueden surgir durante la actualización. Para obtener una lista más completa de los errores y problemas que pueden producirse con una aplicación de SignalR, consulte [solución de problemas de SignalR](../testing-and-debugging/troubleshooting.md).

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a>'La llamada es ambigua entre los siguientes métodos o propiedades'

Este error se producirá si una referencia a `Microsoft.AspNet.SignalR.Owin` no se quita. Este paquete está en desuso; la referencia debe quitarse y se debe desinstalar la versión 1.x del paquete SelfHost.

### <a name="hub-methods-fail-silently"></a>Métodos de concentrador producirá un error en modo silencioso

Compruebe que las referencias de script en el cliente son hasta fecha y que la `OwinStartup` atributo para la clase de inicio tiene la clase correcto y nombres de ensamblado para el proyecto. Además, pruebe a abrir la dirección de los concentradores (hubs/signalr /) en el explorador; cualquier error que aparece le ofrece más información sobre la causa del error.
