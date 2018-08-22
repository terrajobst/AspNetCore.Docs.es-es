---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Lo que no se debe hacer en ASP.NET y qué hacer en su lugar | Microsoft Docs
author: tfitzmac
description: En este tema se describe varios errores comunes que las personas dentro de los proyectos web ASP.NET. Se proporcionan recomendaciones sobre lo que debe hacer para evitar estos cargándola...
ms.author: riande
ms.date: 05/08/2014
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 7f2366ef8cb258a08c3cb96407f605864e5ec9a9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838197"
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a>Lo que no se debe hacer en ASP.NET y qué hacer en su lugar
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este tema se describe varios errores comunes que las personas dentro de los proyectos web ASP.NET. Se proporcionan recomendaciones sobre lo que debe hacer para evitar estos errores comunes. Se basa en un [presentación](http://vimeo.com/68390507) por **Damian Edwards** en corona conferencia para desarrolladores.


## <a name="disclaimer"></a>Declinación de responsabilidades

En este tema no está pensada como una guía completa para asegurarse de que la aplicación es segura y eficaz. Aun así deberá seguir los procedimientos recomendados de seguridad y rendimiento que no se describen en este tema. Solo sugiere cómo evitar errores comunes relacionados con los procesos y las clases. NET.

## <a name="overview"></a>Información general

Este tema contiene las siguientes secciones:

- [Cumplimiento de estándares](#standards)

    - [Adaptadores de control](#adapters)
    - [Propiedades de estilo de los controles](#styleprop)
    - [Página y devoluciones de llamada de Control](#callback)
    - [Detección de capacidad del explorador](#browsercap)
- [Seguridad](#security)

    - [La validación de solicitudes](#validation)
    - [Sesión y autenticación de formularios sin cookies](#cookieless)
    - [EnableViewStateMac](#viewstatemac)
    - [Nivel de confianza medio](#medium)
    - [&lt;appSettings&gt;](#appsettings)
    - [UrlPathEncode](#urlpathencode)
- [Confiabilidad y rendimiento](#performance)

    - [PreSendRequestHeaders y PreSendRequestContent](#presend)
    - [Eventos de página asincrónica con formularios Web Forms](#asyncevents)
    - [Enviar y olvidarse de trabajo](#fire)
    - [Cuerpo de la entidad de solicitud](#requestentity)
    - [Response.Redirect y Response.End](#redirect)
    - [EnableViewState y ViewStateMode](#viewstatemode)
    - [SqlMembershipProvider](#sqlprovider)
    - [Las solicitudes de ejecución largo (> 110 segundos)](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a>Cumplimiento de estándares

<a id="adapters"></a>

### <a name="control-adapters"></a>Adaptadores de control

Recomendación: Dejar de usar adaptadores de control para procesamiento adaptable y, en su lugar, use las consultas de medios CSS y HTML estándar.

Adaptadores de controles se introdujeron en .NET 2.0 para representar el código de presentación que se ha personalizado para dispositivos diferentes y entornos. Ahora, se puede lograr este procesamiento adaptable con CSS y HTML. Debe dejar de usar adaptadores de Control y convertir a todos los adaptadores existentes en CSS y HTML.

Para obtener más información, consulte [las consultas de medios](http://www.w3.org/TR/css3-mediaqueries/) y [Cómo: agregar páginas móviles a la de formularios Web Forms ASP.NET / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a>Propiedades de estilo de los controles

Recomendación: Detenga al establecer los valores de estilo de marcado del control y, en su lugar, establezca los valores de formato en las hojas de estilo CSS.

Controles de servidor Web contienen docenas de propiedades que se pueden usar para establecer las propiedades de estilo en línea. Por ejemplo, la propiedad ForeColor establece el color del texto de un control. Puede realizar este mismo efecto más eficazmente a través de las hojas de estilo CSS. Las hojas de estilo le permiten centralizar los valores de estilo y evite establecer estos valores en toda la aplicación.

En el ejemplo siguiente se muestra una clase CSS el texto de los conjuntos a rojo.

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

En el ejemplo siguiente se muestra cómo se aplican dinámicamente a la clase CSS.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a>Página y devoluciones de llamada de Control

Recomendación: Dejar de utilizar las devoluciones de llamada de la página y el control y, en su lugar, use cualquiera de las siguientes acciones: AJAX, UpdatePanel, los métodos de acción de MVC, Web API o SignalR.

En versiones anteriores de ASP.NET, los métodos de devolución de llamada de página y Control habilitado para actualizar parte de la página web sin actualizar una página completa. Ahora puede realizar actualizaciones parciales de página a través de [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [API Web](../../../web-api/index.md) o [SignalR](../../../signalr/index.md). Debe detenerse mediante los métodos de devolución de llamada porque pueden causar problemas con direcciones URL descriptivas y enrutamiento. De forma predeterminada, los controles no permiten a los métodos de devolución de llamada, pero si habilita esta característica en un control, debe deshabilitarlo.

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a>Detección de capacidad del explorador

Recomendación: Dejar de usar detección de capacidad del explorador estático y, en su lugar, use la detección dinámica de características.

En versiones anteriores de ASP.NET, las características admitidas para cada explorador se almacenaban en un archivo XML. Detectar compatibilidad con las características a través de una búsqueda estática no es el mejor enfoque. Ahora, puede detectar dinámicamente un explorador las características compatibles con un marco de trabajo de detección de características, como [Modernizr](http://modernizr.com/). Detección de características determina la compatibilidad con cualquier intento de usar un método o propiedad y, a continuación, comprueba si el explorador produce el resultado deseado. De forma predeterminada, Modernizr se incluye en las plantillas de aplicación Web.

<a id="security"></a>

## <a name="security"></a>Seguridad

<a id="validation"></a>

### <a name="request-validation"></a>La validación de solicitudes

Recomendación: Validar la entrada del usuario y codifique la salida de los usuarios.

Validación de la solicitud es una característica de ASP.NET que inspecciona cada solicitud y se detiene la solicitud si no se encuentra una amenaza percibida. No dependen de la validación de solicitudes para proteger la aplicación frente a ataques de scripting entre sitios. En su lugar, validar todas las entradas de los usuarios y codifique la salida. En algunos casos limitados, puede usar expresiones regulares para validar la entrada, pero en casos más complicados que debe validar entrada de usuario mediante las clases de .NET que determinan si el valor coincide con los valores permitidos.

El ejemplo siguiente muestra cómo usar un método estático en la clase Uri para determinar si el Uri proporcionado por el usuario es válido.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

Sin embargo, para comprobar lo suficientemente el Uri, también debe comprobar para asegurarse de que especifica `http` o `https`. El ejemplo siguiente utiliza los métodos de instancia para comprobar que el Uri es válido.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

Antes de representar la entrada del usuario como HTML o como entrada del usuario en una consulta SQL, codificar los valores para asegurarse de que no se incluye el código malintencionado.

Puede HTML codificar el valor en el marcado con el &lt;%: %&gt; sintaxis, como se muestra a continuación.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

O bien, en la sintaxis de Razor, puede HTML codificar con @, tal y como se muestra a continuación.

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

El ejemplo siguiente se muestra cómo a HTML codifica un valor en el código subyacente.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

Para codificar de forma segura un valor para los comandos SQL, use los parámetros de comando como el [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx). <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a>Sesión y autenticación de formularios sin cookies

Recomendación: Necesita cookies.

No es seguro pasar información de autenticación en la cadena de consulta. Por lo tanto, necesita cookies cuando la aplicación incluye la autenticación. Si la cookie que almacena información confidencial, considere la posibilidad de que se requiere SSL para la cookie.

El ejemplo siguiente muestra cómo se especifica en el archivo Web.config que la autenticación de formularios requiere una cookie que se transmite a través de SSL.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a>EnableViewStateMac

Recomendación: Nunca se establece en false.

De forma predeterminada, EnbableViewStateMac se establece en true. Incluso si la aplicación no usa el estado de vista, no establezca EnableViewStateMac en false. Establecer este valor en false para que su aplicación sea vulnerable a XSS.

A partir de ASP.NET 4.5.2, el tiempo de ejecución impone **EnableViewStateMac = true**. Incluso si se establece en false, el tiempo de ejecución omite este valor y continúa con el valor establecido en true. Para obtener más información, consulte [ASP.NET 4.5.2 y EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).

El ejemplo siguiente muestra cómo establecer EnableViewStateMac en true. No es necesario establecer este valor en true porque es true de forma predeterminada. Sin embargo, si se se establece en false en cualquier página en la aplicación, debe corregir inmediatamente este valor.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a>Nivel de confianza medio

Recomendación: No dependen de nivel de confianza medio (o cualquier otro nivel de confianza) como un límite de seguridad.

Confianza parcial no protegen adecuadamente la aplicación y no debe usarse. En su lugar, use la plena confianza y aislar las aplicaciones que no se confía en grupos de aplicaciones independientes. Además, ejecutar cada grupo de aplicaciones con una identidad única. Para obtener más información, consulte [ASP.NET de confianza parcial no garantiza el aislamiento de la aplicación](https://support.microsoft.com/kb/2698981).

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a>&lt;appSettings&gt;

Recomendación: Deshabilitar la configuración de seguridad en &lt;appSettings&gt; elemento.

El elemento appSettings contiene muchos valores que son necesarios para las actualizaciones de seguridad. No debe cambiar o deshabilitar estos valores. Si debe deshabilitar estos valores al implementar una actualización, inmediatamente volver a habilitar después de completar la implementación.

Para obtener más información, consulte [elemento appSettings de ASP.NET](https://msdn.microsoft.com/library/hh975440.aspx).

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a>UrlPathEncode

Recomendación: Use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) en su lugar.

Se agregó el método UrlPathEncode a .NET Framework para resolver un problema de compatibilidad de explorador muy específico. No realiza ninguna codificación adecuadamente una dirección URL y no protege la aplicación de scripting entre sitios. Nunca se debe usar en la aplicación. En su lugar, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).

El ejemplo siguiente muestra cómo pasar una dirección URL codificada como un parámetro de cadena de consulta para un control de hipervínculo.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a>Confiabilidad y rendimiento

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a>PreSendRequestHeaders y PreSendRequestContent

Recomendación: No use estos eventos con módulos administrados. En su lugar, escribir un módulo nativo de IIS para llevar a cabo la tarea requerida. Consulte [crear módulos HTTP de código nativo](https://msdn.microsoft.com/library/ms693629.aspx).

Puede usar el [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) y [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) eventos con los módulos nativos de IIS.
> [!WARNING]
> No use `PreSendRequestHeaders` y `PreSendRequestContent` con módulos administrados que implementan `IHttpModule`. Establecer estas propiedades puede causar problemas con las solicitudes asincrónicas. La combinación de enrutamiento solicitado aplicaciones (ARR) y websockets podría provocar excepciones de infracción de acceso que pueden causar w3wp se bloquee. ¡Por ejemplo, iiscore! W3_CONTEXT_BASE::GetIsLastNotification + 68 en iiscore.dll ha provocado una excepción de infracción de acceso (0xC0000005).

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a>Eventos de página asincrónica con formularios Web Forms

Recomendación: En formularios Web Forms, evitar la escritura de async void métodos para los eventos de ciclo de vida de página y, en su lugar use [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) para código asincrónico.

Al marcar un evento de página con **async** y **void**, no se puede determinar cuándo ha finalizado el código asincrónico. En su lugar, utilice Page.RegisterAsyncTask para ejecutar el código asincrónico de forma que permite realizar un seguimiento de su finalización.

El ejemplo siguiente se muestra un botón, haga clic en el controlador que contiene el código asincrónico. Este ejemplo incluye la lectura asincrónica, de un valor de cadena que se proporciona sólo como un ejemplo simplificado de una tarea asincrónica y no como una práctica recomendada.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

Si usa tareas asincrónicas, establezca la plataforma de destino en tiempo de ejecución de Http a la versión 4.5 en el archivo Web.config. Establecer la plataforma de destino en 4.5 activa en el nuevo contexto de sincronización que se agregó en .NET 4.5. Este valor se establece de forma predeterminada en los nuevos proyectos en Visual Studio 2012, pero no es estar establecida si está trabajando con un proyecto existente.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a>Enviar y olvidarse de trabajo

Recomendación: Al procesar una solicitud de ASP.NET, evitará iniciar trabajo fire and forget (este tipo llamando al método ThreadPool.QueueUserWorkItem o crear un temporizador que llama repetidamente a un delegado).

Si la aplicación tiene trabajo fire and forget que se ejecuta en ASP.NET, la aplicación puede obtener sincronizada. En cualquier momento, se puede destruir el dominio de aplicación lo que significa que el proceso continuo ya no puede coincidir con el estado actual de la aplicación.

Debe mover este tipo de trabajo fuera de ASP.NET. Puede usar los trabajos Web, servicio de Windows o un rol de trabajo en Azure para realizar el trabajo en curso y ejecutar ese código de otro proceso.

Si se debe realizar este trabajo en ASP.NET, puede agregar el paquete Nuget llamado [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) para ejecutar el código.

<a id="requestentity"></a>

### <a name="request-entity-body"></a>Cuerpo de la entidad de solicitud

Recomendación: Evitar la lectura Request.Form o Request.InputStream antes de ejecuta el evento al controlador.

Lo antes posible, debe leer desde Request.Form o Request.InputStream es durante el controlador ejecutar el evento. En MVC, el controlador es el controlador y el evento de ejecución es cuando se ejecuta el método de acción. En formularios Web Forms, la página es el controlador y el evento de ejecución es cuando se desencadene el evento Page.Init. Si lee el cuerpo de entidad de solicitud antes que el evento de ejecución, interferir con el procesamiento de la solicitud.

Si tiene que leer el cuerpo de entidad de solicitud antes del evento execute, usar [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) o [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx). Cuando usas GetBufferlessInputStream, obtener la cadena sin formato de la solicitud y asuma la responsabilidad para procesar la solicitud completa. Después de llamar a GetBufferlessInputStream, Request.Form y Request.InputStream no están disponibles porque no se ha rellenado por ASP.NET. Cuando usas GetBufferedInputStream, obtener una copia de la secuencia de la solicitud. Request.Form y Request.InputStream siguen estando disponibles más adelante en la solicitud porque ASP.NET rellena la otra copia.

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a>Response.Redirect y Response.End

Recomendación: Tenga en cuenta las diferencias en cómo se controla el subproceso después de llamar a [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).

El [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) método llama al método Response.End. En un proceso sincrónico, una llamada a Request.Redirect hace que el subproceso actual se anula inmediatamente. Sin embargo, en un proceso asincrónico, la llamada a Response.Redirect no anula el subproceso actual, por lo que continúa la ejecución de código para la solicitud. En un proceso asincrónico, debe devolver la tarea desde el método para detener la ejecución del código.

En un proyecto MVC, no debe llamar a Response.Redirect. En su lugar, devuelven un RedirectResult.

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a>EnableViewState y ViewStateMode

Recomendación: Use ViewStateMode, en lugar de EnableViewState para proporcionar control granular sobre qué controles utilizar el estado de vista.

Cuando EnableViewState se establece en false en la directiva de página, el estado de vista está deshabilitado para todos los controles dentro de la página y no se puede habilitar. Si desea habilitar el estado de vista para solo ciertos controles en la página, establezca ViewStateMode en deshabilitado para la página.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

A continuación, establezca ViewStateMode habilitado sólo en los controles que necesitan realmente el estado de vista.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

Al habilitar el estado de vista de solo los controles que lo necesiten, puede reducir el tamaño del estado de vista para las páginas web.

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a>SqlMembershipProvider

Recomendación: Use proveedores universales.

En las plantillas de proyecto actual, SqlMembershipProvider ha sido reemplazado por [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), que está disponible como un paquete de NuGet. Si utilizas SqlMembershipProvider en un proyecto que se creó con una versión anterior de las plantillas, debe cambiar a los proveedores universales. Los proveedores universales funcionan con todas las bases de datos que son compatibles con Entity Framework.

Para obtener más información, consulte [Introducción a ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a>Las solicitudes de ejecución prolongada (> 110 segundos)

Recomendación: Use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) o [SignalR](../../../signalr/index.md) para los clientes conectados y use operaciones asincrónicas de E/S.

Solicitudes de ejecución prolongada pueden causar resultados imprevisibles y un rendimiento deficiente en la aplicación web. El valor de tiempo de espera predeterminado para una solicitud es de 110 segundos. Si usa el estado de sesión con una solicitud de ejecución prolongada, ASP.NET volverá a liberar el bloqueo en el objeto de sesión después de 110 segundos. Sin embargo, la aplicación puede estar en el medio de una operación en el objeto de sesión cuando se libere el bloqueo y la operación no se puede completar correctamente. Si una segunda solicitud del usuario se bloquea mientras se ejecuta la primera solicitud, la segunda solicitud puede tener acceso al objeto de sesión en un estado incoherente.

Si la aplicación incluye las operaciones de E/S de bloqueo (o sincrónicas), no responderá la aplicación.

Para mejorar el rendimiento, utilice las operaciones de E/S asincrónicas en .NET Framework. Además, usar SignalR o WebSockets para conectar clientes al servidor. Estas características están diseñadas para manejar de manera eficaz las solicitudes de ejecución prolongada.
