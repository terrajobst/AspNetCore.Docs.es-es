---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: Middleware de OWIN en IIS integrado canalización | Microsoft Docs
author: Praburaj
description: En este artículo se muestra cómo ejecutar componentes de middleware OWIN (OMCs) en la canalización integrada de IIS, y cómo establecer el evento de la canalización un OMC se ejecuta en. Debería...
ms.author: riande
ms.date: 11/07/2013
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 6124bcdaeeb0d4342cbde0d3ca52d55f76a953ab
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576475"
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a>Middleware de OWIN en la canalización integrada de IIS
====================
por [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson]((https://twitter.com/RickAndMSFT))

> En este artículo se muestra cómo ejecutar componentes de middleware OWIN (OMCs) en la canalización integrada de IIS, y cómo establecer el evento de la canalización un OMC se ejecuta en. Debe revisar [una visión general del proyecto Katana](an-overview-of-project-katana.md) y [detección de clase de inicio OWIN](owin-startup-class-detection.md) antes de leer este tutorial. En este tutorial se escribió por Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Howard Dierking, Chris Ross y Praburaj Thiagarajan ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).


Aunque [OWIN](an-overview-of-project-katana.md) componentes de middleware (OMCs) están diseñados principalmente para ejecutarse en una canalización independiente del servidor, es posible ejecutar una OMC en así la canalización integrada de IIS (**es el modo clásico *no* admite**). ¿Se pueden hacer un OMC para trabajar en la canalización integrada de IIS al instalar el paquete siguiente desde la consola de administrador de paquetes (PMC):

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

Esto significa que todos los marcos de aplicaciones, incluso aquellos que no son capaz de ejecutar fuera de IIS o System.Web, pueden beneficiarse de los componentes de middleware OWIN existentes. 

> [!NOTE]
> Todas la `Microsoft.Owin.Security.*` paquetes trasvase de registros con el nuevo sistema de identidad en Visual Studio 2013 (por ejemplo: Cookies, Account de Microsoft, Google, Facebook, Twitter, [el Token de portador](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, servidor de autorización, JWT, Azure Active Directory directorio y servicios de federación de Active Directory) se crean como OMCs y pueden usarse en escenarios autohospedados y hospedado en IIS.

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>Cómo se ejecuta el Middleware de OWIN en la canalización integrada de IIS

OWIN para aplicaciones de consola, la canalización de aplicación creados con el [configuración de inicio](owin-startup-class-detection.md) está establecido por el orden de los componentes se agregan utilizando la `IAppBuilder.Use` método. Es decir, la canalización de OWIN en la [Katana](an-overview-of-project-katana.md) en tiempo de ejecución procesará OMCs en el orden en que se han registrado con `IAppBuilder.Use`. En la canalización integrada de IIS se compone de la canalización de solicitudes [HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) suscrito a un conjunto predefinido de eventos de la canalización como [BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx), etcetera.

Si se compara una OMC a la de un [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) en el mundo ASP.NET, un OMC debe estar registrado en el evento correcto de canalización predefinida. Por ejemplo, HttpModule `MyModule` se invoca cuando llega una solicitud la [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) fase de la canalización:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

En orden para una OMC participar en este orden de ejecución de la misma, basado en eventos, el [Katana](an-overview-of-project-katana.md) examina el código en tiempo de ejecución a través de la [configuración de inicio](owin-startup-class-detection.md) y se suscribe a cada uno de los componentes de middleware a un eventos de canalización integrada. Por ejemplo, el siguiente código OMC y registro permite ver el registro de eventos predeterminados de los componentes de middleware. (Para obtener más instrucciones sobre cómo crear una clase de inicio OWIN, consulte [detección de clase de inicio de OWIN](owin-startup-class-detection.md).)

1. Crear un proyecto de aplicación web vacía y asígnele el nombre **owin2**.
2. Desde la consola de administrador de paquetes (PMC), ejecute el siguiente comando: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. Agregar un `OWIN Startup Class` y asígnele el nombre `Startup`. Reemplace el código generado por lo siguiente (se resaltan los cambios):  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. Presione F5 para ejecutar la aplicación.

Establece la configuración del inicio de una canalización con componentes de middleware de tres, los dos primeros mostrar información de diagnóstico y la última de ellas responde a eventos (y también mostrar información de diagnóstico). El `PrintCurrentIntegratedPipelineStage` método muestra un mensaje y este middleware se invoca en el evento de canalización integrada. Las ventanas de salida muestra lo siguiente:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

El tiempo de ejecución de Katana asigna a cada uno de los componentes de middleware OWIN a [PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) de forma predeterminada, que se corresponde con la canalización IIS [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx).

## <a name="stage-markers"></a>Marcadores de escenario

Puede marcar OMCs para ejecutar en fases específicas de la canalización mediante el uso de la `IAppBuilder UseStageMarker()` método de extensión. Para ejecutar un conjunto de componentes de middleware durante una fase determinada, insertar un marcador de fases justo después del último componente es el conjunto durante el registro. Hay reglas en qué fase de la canalización pueden ejecutar software intermedio y los componentes de orden se deben ejecutar (las reglas se explican más adelante en el tutorial). Agregar el `UseStageMarker` método a la `Configuration` de código como se muestra a continuación:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

El `app.UseStageMarker(PipelineStage.Authenticate)` llamada configura todos los componentes de middleware registrado anteriormente (en este caso, nuestro dos componentes de diagnóstico) para ejecutarse en la fase de autenticación de la canalización. El último componente de middleware (que muestra los diagnósticos y responde a las solicitudes) que se ejecutará en el `ResolveCache` etapa (la [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) eventos).

Presione F5 para ejecutar la aplicación. La ventana de salida muestra lo siguiente:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>Reglas de marcador de fase

Componentes de middleware de Owin (OMC) pueden configurarse para ejecutarse en los siguientes eventos de fase de canalización OWIN:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. De forma predeterminada, OMCs se ejecutan en el último evento (`PreHandlerExecute`). Por eso nuestro primer ejemplo de código muestra "PreExecuteRequestHandler".
2. Puede usar el un `app.UseStageMarker` método para registrar un OMC para ejecutar versiones anteriores, en cualquier etapa de la canalización OWIN que se muestra en el `PipelineStage` enum.
3. La canalización OWIN y la canalización IIS está ordenado, por lo tanto, las llamadas a `app.UseStageMarker` deben estar en orden. No se puede establecer el controlador de eventos a un evento que precede al último evento registrado con en `app.UseStageMarker`. Por ejemplo, *después* llamada:

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   las llamadas a `app.UseStageMarker` pasando `Authenticate` o `PostAuthenticate` no se respetan y no se producirá ninguna excepción. OMCs ejecutar en la fase más reciente, cuyo valor predeterminado es `PreHandlerExecute`. Los marcadores de escenario se usan para hacer que se ejecuten en versiones anteriores. Si especifica los marcadores de escenario de desorden, hemos de ida y vuelta al marcador anterior. En otras palabras, la adición de un marcador de fases dice "Run no posterior a la fase X". Ejecución de OMC en el marcador de fases más antiguo agregado después de ellos en la canalización de OWIN.
4. La fase más temprana de las llamadas a `app.UseStageMarker` wins. Por ejemplo, si cambia el orden de `app.UseStageMarker` llamadas desde nuestro ejemplo anterior:

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   Se mostrará la ventana de salida: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   El OMCs ejecutan todas en el `AuthenticateRequest` fase, porque el último OMC registrado con el `Authenticate` eventos y el `Authenticate` event precede a todos los demás eventos.
