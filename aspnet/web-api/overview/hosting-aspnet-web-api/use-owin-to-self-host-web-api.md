---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Use OWIN para autohospedaje de ASP.NET Web API | Microsoft Docs
author: rick-anderson
description: Este tutorial muestra cómo hospedar ASP.NET Web API en una aplicación de consola, el uso de OWIN para autohospedaje el marco API Web. Interfaz Web abierta para .NET (OWIN) d...
ms.author: riande
ms.date: 07/09/2013
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 59ce24aa47ca590fbe9b617dbbe8bc6b3711849e
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667393"
---
<a name="use-owin-to-self-host-aspnet-web-api"></a>Use OWIN para autohospedaje de ASP.NET Web API 
====================

> Este tutorial muestra cómo hospedar ASP.NET Web API en una aplicación de consola, el uso de OWIN para autohospedaje el marco API Web.
>
> [Interfaz Web abierta para .NET](http://owin.org) (OWIN) define una abstracción entre los servidores web de .NET y aplicaciones web. OWIN desacopla la aplicación web desde el servidor, lo que hace que OWIN ideal para el autohospedaje de una aplicación web en su propio proceso, fuera de IIS.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
>
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) 
> - Web API 5.2.7


> [!NOTE]
> Puede encontrar el código fuente completo para este tutorial en [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).


## <a name="create-a-console-application"></a>Creación de una aplicación de consola

En el **archivo** menú, **New**, a continuación, seleccione **proyecto**. Desde **instalado**, en **Visual C#** , seleccione **Windows Desktop** y, a continuación, seleccione **aplicación de consola (.Net Framework)**. Denomine el proyecto "OwinSelfhostSample" y seleccione **Aceptar**.

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a>Agregue los paquetes de Web API y OWIN

Desde el **herramientas** menú, seleccione **Administrador de paquetes de NuGet**, a continuación, seleccione **Package Manager Console**. En la ventana de consola de administrador de paquetes, escriba el siguiente comando:

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Esto instalará el paquete de selfhost WebAPI OWIN y todos los paquetes necesarios de OWIN.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Configurar Web API para autohospedar

En el Explorador de soluciones, haga clic en el proyecto y seleccione **agregar** / **clase** para agregar una nueva clase. Asigne a la clase el nombre `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Reemplace todo el código repetitivo en este archivo por lo siguiente:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Agregar un controlador Web API

A continuación, agregue una clase de controlador Web API. En el Explorador de soluciones, haga clic en el proyecto y seleccione **agregar** / **clase** para agregar una nueva clase. Asigne a la clase el nombre `ValuesController`.

Reemplace todo el código repetitivo en este archivo por lo siguiente:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a>Iniciar al Host OWIN y realizar una solicitud con HttpClient

Reemplace todo el código reutilizable en el archivo Program.cs por el siguiente:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a>Ejecutar la aplicación

Para ejecutar la aplicación, presione F5 en Visual Studio. La salida debe tener un aspecto parecido al siguiente:

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Recursos adicionales

[Información general del proyecto Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Hospedar ASP.NET Web API en un rol de trabajo de Azure](host-aspnet-web-api-in-an-azure-worker-role.md)
