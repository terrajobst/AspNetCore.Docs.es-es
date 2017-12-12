---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Seguimiento en ASP.NET Web API 2 | Documentos de Microsoft
author: MikeWasson
description: "Muestra cómo habilitar el seguimiento en ASP.NET Web API."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f35c8a10018ce796e2d905d6ee839ff09bb380a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="tracing-in-aspnet-web-api-2"></a>Seguimiento en ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

> Cuando se intenta depurar una aplicación basada en web, no hay ningún sustituto de un buen conjunto de registros de seguimiento. Este tutorial muestra cómo habilitar el seguimiento en ASP.NET Web API. Puede usar esta característica para realizar un seguimiento lo que hace el marco Web API antes y después invoca el controlador. También puede usar para realizar el seguimiento de su propio código.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - [Visual Studio de 2017](https://www.visualstudio.com/downloads/) (también funciona con Visual Studio 2015)
> - Web API 2
> - [Microsoft.AspNet.WebApi.Tracing](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a>Habilitar System.Diagnostics seguimiento de API Web

En primer lugar, vamos a crear un nuevo proyecto de aplicación Web ASP.NET. En Visual Studio, desde la **archivo** menú, seleccione **New**, a continuación, **proyecto**. En **plantillas**, **Web**, seleccione **aplicación Web ASP.NET**.

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

Elija la plantilla de proyecto de API Web.

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, **consola Administrar del paquete**.

En la ventana de la consola de administrador de paquetes, escriba los siguientes comandos.

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

El primer comando instala el paquete de seguimiento de API Web más reciente. También actualiza los paquetes de Web API core. El segundo comando actualiza el paquete de WebApi.WebHost a la versión más reciente.

> [!NOTE]
> Si desea tener como destino una versión específica de la API Web, use-marca de versión cuando se instala el paquete de seguimiento.


Abra el archivo WebApiConfig.cs en la aplicación\_carpeta de inicio. Agregue el código siguiente a la **registrar** método.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

Este código agrega el [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) clase a la canalización de Web API. El **SystemDiagnosticsTraceWriter** clase escribe seguimientos a [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace).

Para ver los seguimientos, ejecute la aplicación en el depurador. En el explorador, vaya a `/api/values`.

![](tracing-in-aspnet-web-api/_static/image5.png)

Las instrucciones de seguimiento se escriben en la ventana Resultados de Visual Studio. (Desde el **vista** menú, seleccione **salida**).

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

Dado que **SystemDiagnosticsTraceWriter** escribe seguimientos a **System.Diagnostics.Trace**, puede registrar los agentes de escucha de seguimiento adicionales; por ejemplo escribir seguimientos en un archivo de registro. Para obtener más información acerca de los escritores de seguimiento, consulte el [los agentes de escucha de seguimiento](https://msdn.microsoft.com/en-us/library/4y5y10s7.aspx) tema en MSDN.

### <a name="configuring-systemdiagnosticstracewriter"></a>Configuración de SystemDiagnosticsTraceWriter

El código siguiente muestra cómo configurar el sistema de escritura de seguimiento.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

Hay dos opciones de configuración que se pueden controlar:

- IsVerbose: Si es false, cada seguimiento contiene cierta información mínima. Si es true, los seguimientos incluir más información.
- MinimumLevel: Establece el nivel de seguimiento mínimo. Niveles de seguimiento, en orden, son depurar, información, advertencia, Error y Fatal.

## <a name="adding-traces-to-your-web-api-application"></a>Agrega seguimientos a la aplicación de API Web

Agregar un autor de seguimiento proporciona acceso inmediato a los seguimientos creados por la canalización Web API. También puede usar el escritor de seguimiento para realizar el seguimiento de su propio código:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

Para obtener el escritor de seguimiento, llame a **HttpConfiguration.Services.GetTraceWriter**. Desde un controlador, este método es accesible a través de la **ApiController.Configuration** propiedad.

Para escribir un seguimiento, puede llamar a la **ITraceWriter.Trace** método directamente, pero la [ITraceWriterExtensions](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.itracewriterextensions.aspx) clase define algunos métodos de extensión que están más descriptivos. Por ejemplo, el **información** método mostrado anteriormente, crea un seguimiento con nivel de seguimiento **información**.

## <a name="web-api-tracing-infrastructure"></a>Infraestructura de seguimiento de API Web

En esta sección se describe cómo escribir un escritor de seguimiento personalizado de API Web.

El paquete Microsoft.AspNet.WebApi.Tracing se basa en una infraestructura de seguimiento más general en Web API. En lugar de usar Microsoft.AspNet.WebApi.Tracing, también puede conectar en alguna otra biblioteca de seguimiento y registro, como [NLog](http://nlog-project.org/) o [log4net](http://logging.apache.org/log4net/).

Para recopilar seguimientos, implementar la **ITraceWriter** interfaz. Este es un ejemplo simple:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

El **ITraceWriter.Trace** método crea un seguimiento. El autor de la llamada especifica un nivel de categoría y seguimiento. La categoría puede ser cualquier cadena definida por el usuario. La implementación de **seguimiento** debería hacer lo siguiente:

1. Crear un nuevo **TraceRecord**. Inicializar con la solicitud, la categoría y el nivel de seguimiento, tal como se muestra. Estos valores se proporcionan por el llamador.
2. Invocar la *traceAction* delegar. Dentro de este delegado, se espera que el llamador para rellenar el resto de la **TraceRecord**.
3. Escribir el **TraceRecord**, usando cualquier técnica de registro que desee. En el ejemplo se muestra aquí simplemente llama a **System.Diagnostics.Trace**.

## <a name="setting-the-trace-writer"></a>Establecer el escritor de seguimiento

Para habilitar el seguimiento, debe configurar la API de Web que se utilizará la **ITraceWriter** implementación. Lo hace a través de la **HttpConfiguration** de objeto, como se muestra en el código siguiente:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

Escritor de seguimiento solo una puede estar activa. De forma predeterminada, conjuntos de API de Web un &quot;inefectiva&quot; seguimiento que no hace nada. (El &quot;inefectiva&quot; seguimiento existe para que no tengan código de seguimiento comprobar si el autor de seguimiento es **null** antes de escribir un seguimiento.)

## <a name="how-web-api-tracing-works"></a>Cómo Web API Tracing funciona

Seguimiento en usos de las API Web utiliza una API Web en un *fachada* patrón: cuando el seguimiento está habilitado, Web API salta distintas partes de la canalización de solicitudes con las clases que realizan llamadas de seguimiento.

Por ejemplo, al seleccionar un controlador, la canalización usa el **IHttpControllerSelector** interfaz. Con el seguimiento habilitado, la canalización de inserta una clase que implementa **IHttpControllerSelector** pero llamadas a través de la implementación real:

![Seguimiento de API Web usa el patrón de fachada.](tracing-in-aspnet-web-api/_static/image8.png)

Las ventajas de este diseño incluyen:

- Si no se agrega un escritor de seguimiento, los componentes de seguimiento no se crean instancias y no tienen ningún impacto de rendimiento.
- Si reemplaza servicios predeterminados como **IHttpControllerSelector** con su propia implementación personalizada, el seguimiento no se ve afectado, porque el seguimiento se realiza mediante el objeto de contenedor.

También puede reemplazar el marco de trabajo de seguimiento de API Web completo con su propio marco de trabajo personalizado, sustituyendo el valor predeterminado **ITraceManager** servicio:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

Implemente **ITraceManager.Initialize** para inicializar el sistema de seguimiento. Tenga en cuenta que esto reemplazará la *todo* framework de seguimiento, incluido todo el código de seguimiento que se integra en API Web.
