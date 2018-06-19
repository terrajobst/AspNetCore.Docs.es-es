---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: Middleware de OWIN en IIS integrado canalización | Documentos de Microsoft
author: Praburaj
description: Este artículo muestra cómo ejecutar los componentes de middleware OWIN (OMCs) en la canalización integrada de IIS y cómo establecer el evento de canalización un OMC se ejecuta en. Debe...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2013
ms.topic: article
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 5df70c80084a32c5f61ac9288c8cdbfaaa47f124
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871498"
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a>Middleware de OWIN en la canalización integrada de IIS
====================
por [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> Este artículo muestra cómo ejecutar los componentes de middleware OWIN (OMCs) en la canalización integrada de IIS y cómo establecer el evento de canalización un OMC se ejecuta en. Debe revisar [una información general del proyecto Katana](an-overview-of-project-katana.md) y [detección de clase de inicio de OWIN](owin-startup-class-detection.md) antes de leer este tutorial. Este tutorial se escribió Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Chris Ross, Praburaj Thiagarajan y Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).


Aunque [OWIN](an-overview-of-project-katana.md) componentes de middleware (OMCs) están diseñados principalmente para ejecutarse en una canalización independiente del servidor, es posible ejecutar un OMC en así la canalización integrada de IIS (**modo clásico es *no* admite**). Un OMC puede ponerse a trabajar en la canalización integrada de IIS, deberá instalar el siguiente paquete de consola de administrador de paquete (PMC):

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

Esto significa que todos los marcos de aplicaciones, incluso aquellos que no son capaces de ejecutar fuera de IIS y System.Web, pueden beneficiarse de los componentes de middleware OWIN existentes. 

> [!NOTE]
> Todos los `Microsoft.Owin.Security.*` paquetes de envío con el nuevo sistema de identidad en Visual Studio 2013 (por ejemplo: las Cookies, Account de Microsoft, Google, Facebook, Twitter, [tokens de portador](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, servidor de autorización, JWT, Azure Active directorio y los servicios de federación de Active directory) se crean como OMCs y pueden utilizarse en escenarios hospedados por sí mismo y hospedado en IIS.

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>Cómo se ejecuta el Middleware de OWIN en la canalización integrada de IIS

Para aplicaciones de consola OWIN, la canalización de aplicación creados con el [configuración de inicio](owin-startup-class-detection.md) está establecido por el orden de los componentes se agregan mediante el `IAppBuilder.Use` método. Es decir, la canalización OWIN en la [Katana](an-overview-of-project-katana.md) en tiempo de ejecución procesará OMCs en el orden en que se registraron mediante `IAppBuilder.Use`. En la canalización integrada de IIS se compone de la canalización de solicitudes [HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) suscrito a un conjunto predefinido de los eventos de la canalización como [BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx), etcetera.

Si se compara una OMC a la de un [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) en el mundo ASP.NET, debe registrarse un OMC al evento correcto canalización predefinidos. Por ejemplo, el elemento HttpModule `MyModule` se invoca cuando una solicitud entra el [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) fase de la canalización:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

En orden para un OMC participar en este orden de ejecución mismo, basado en eventos, el [Katana](an-overview-of-project-katana.md) código en tiempo de ejecución recorre la [configuración de inicio](owin-startup-class-detection.md) y se suscribe a cada uno de los componentes de middleware para un eventos de la canalización integrada. Por ejemplo, el siguiente código OMC y registro permite ver el registro de eventos predeterminados de los componentes de middleware. (Para obtener más instrucciones sobre cómo crear una clase de inicio OWIN, consulte [detección de clase de inicio de OWIN](owin-startup-class-detection.md).)

1. Crear un proyecto de aplicación web vacía y asígnele el nombre **owin2**.
2. Desde la consola de Manager de paquete (PMC), ejecute el siguiente comando: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. Agregar un `OWIN Startup Class` y asígnele el nombre `Startup`. Reemplace el código generado por el siguiente (los cambios aparecen resaltados):  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. Presione F5 para ejecutar la aplicación.

La configuración del inicio se configura una canalización con componentes de middleware tres, el primero dos mostrar información de diagnóstico y la última de ellas responder a eventos (y también muestra información de diagnóstico). El `PrintCurrentIntegratedPipelineStage` método muestra este middleware se invoca en el evento de canalización integrada y un mensaje. Las ventanas de salida muestra lo siguiente:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

El tiempo de ejecución de Katana asigna cada uno de los componentes de middleware OWIN para [PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) de forma predeterminada, que corresponde al evento de canalización IIS [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx).

## <a name="stage-markers"></a>Marcadores de fase

Puede marcar OMCs para ejecutar en fases específicas de la canalización mediante el `IAppBuilder UseStageMarker()` método de extensión. Para ejecutar un conjunto de componentes de middleware durante una fase determinada, inserte un marcador de fases justo después del último componente es el conjunto durante el registro. Hay reglas en qué fase de la canalización pueden ejecutar middleware y componentes de la orden se deben ejecutar (las reglas se explican más adelante en el tutorial). Agregar el `UseStageMarker` método a la `Configuration` código tal y como se muestra a continuación:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

El `app.UseStageMarker(PipelineStage.Authenticate)` llamada configura todos los componentes de middleware registrado anteriormente (en este caso, los dos componentes de diagnóstico) para ejecutarse en la fase de autenticación de la canalización. El último componente de middleware (que muestra los diagnósticos y responde a las solicitudes) se ejecutará en el `ResolveCache` fase (la [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) evento).

Presione F5 para ejecutar la aplicación. La ventana de salida muestra lo siguiente:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>Reglas de marcador de fase

Componentes de middleware de Owin (OMC) se pueden configurar para ejecutarse en los siguientes eventos de fase de canalización OWIN:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. De forma predeterminada, OMCs se ejecutan en el último evento (`PreHandlerExecute`). Por eso nuestro primer ejemplo de código muestra "PreExecuteRequestHandler".
2. Puede usar el un `app.UseStageMarker` método para registrar un OMC para ejecutar versiones anteriores, en cualquier fase de la canalización OWIN aparece en la `PipelineStage` enum.
3. La canalización OWIN y la canalización IIS está ordenado, por lo tanto, las llamadas a `app.UseStageMarker` deben estar en orden. No se puede establecer el controlador de eventos a un evento que precede al último evento registrado con en `app.UseStageMarker`. Por ejemplo, *después* de llamada:

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   las llamadas a `app.UseStageMarker` pasar `Authenticate` o `PostAuthenticate` no se aplican y no se producirá ninguna excepción. OMCs ejecutar en la fase más reciente, cuyo valor predeterminado es `PreHandlerExecute`. Los marcadores de fase se utilizan para hacer que se ejecuten versiones anteriores. Si especifica marcadores de fase fuera de servicio, se redondeará al marcador anterior. En otras palabras, agregar un marcador de fases dice "Ejecutar ya no posterior a la fase X". Ejecución del OMC en el marcador de fase más temprano agregado después de ellos en la canalización OWIN.
4. La primera etapa de llamadas a `app.UseStageMarker` wins. Por ejemplo, si cambia el orden de `app.UseStageMarker` llamadas desde nuestro ejemplo anterior:

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   Se mostrará la ventana de salida: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   El OMCs ejecutar en el `AuthenticateRequest` fase, porque el último OMC registrado con el `Authenticate` eventos y el `Authenticate` event precede a todos los demás eventos.
