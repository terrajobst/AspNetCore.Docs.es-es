---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: "Qué no hacer en ASP.NET y qué hacer en su lugar | Documentos de Microsoft"
author: tfitzmac
description: "Este tema describe varios errores comunes que se pueden cometer dentro de los proyectos web ASP.NET. Proporciona recomendaciones para lo que debe hacer para evitar estos cargándola..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/08/2014
ms.topic: article
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 829f3a024bc15bec8b60b91193ba9bca37b78009
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a>Qué no hacer en ASP.NET y qué hacer en su lugar
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tema describe varios errores comunes que se pueden cometer dentro de los proyectos web ASP.NET. Proporciona recomendaciones para lo que debe hacer para evitar estos errores comunes. Se basa en un [presentación](http://vimeo.com/68390507) por **Damian Edwards** en noruego conferencia de desarrolladores.


## <a name="disclaimer"></a>Declinación de responsabilidades

En este tema no está diseñada como una guía completa para garantizar que su aplicación sea segura y eficaz. Deberá seguir las prácticas recomendadas para la seguridad y rendimiento que no se describen en este tema. Solo se sugiere cómo evitar los errores más comunes relacionados con procesos y las clases. NET.

## <a name="overview"></a>Información general

Este tema contiene las siguientes secciones:

- [Cumplimiento de estándares](#standards)

    - [Adaptadores de control](#adapters)
    - [Propiedades de estilo de los controles](#styleprop)
    - [Página y las devoluciones de llamada de Control](#callback)
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
    - [Fire-and-Forget Work](#fire)
    - [Cuerpo de la entidad de solicitud](#requestentity)
    - [Response.Redirect y Response.End](#redirect)
    - [EnableViewState y ViewStateMode](#viewstatemode)
    - [SqlMembershipProvider](#sqlprovider)
    - [Las solicitudes de ejecución largas (> 110 segundos)](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a>Cumplimiento de estándares

<a id="adapters"></a>

### <a name="control-adapters"></a>Adaptadores de control

Recomendación: Deje de usar adaptadores de control para la representación adaptable y, en su lugar, use las consultas de medios CSS y compatible con los estándares HTML.

Adaptadores de controles se introdujeron en .NET 2.0 para representar el código de presentación que se ha personalizado para diferentes dispositivos y entornos. Ahora, esta representación adaptable puede realizarse con CSS y HTML. Debe dejar de usar adaptadores de Control y convertir a los adaptadores existentes en CSS y HTML.

Para obtener más información, consulte [consultas de medios](http://www.w3.org/TR/css3-mediaqueries/) y [Cómo: agregar páginas móviles a los formularios ASP.NET Web Forms / aplicación MVC](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a>Propiedades de estilo de los controles

Recomendación: Detenga establecer valores de estilo en el marcado del control y, en su lugar, establezca los valores de formato en las hojas de estilos CSS.

Controles de servidor Web contienen docenas de propiedades que se pueden usar para establecer las propiedades de estilo en línea. Por ejemplo, la propiedad de color de primer plano establece el color del texto de un control. Puede realizar este mismo efecto más eficazmente a través de las hojas de estilos CSS. Hojas de estilos permiten centralizar los valores de estilo y no es necesario establecer estos valores a lo largo de la aplicación.

En el ejemplo siguiente se muestra una clase CSS el texto de los conjuntos a rojo.

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

En el ejemplo siguiente se muestra cómo aplicar dinámicamente la clase CSS.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a>Página y las devoluciones de llamada de Control

Recomendación: Deje de usar devoluciones de llamada de página y de control y, en su lugar, use cualquiera de los siguientes: AJAX, UpdatePanel, métodos de acción de MVC, Web API o SignalR.

En versiones anteriores de ASP.NET, los métodos de devolución de llamada de página y un Control habilitan actualizar parte de la página web sin necesidad de actualizar una página completa. Ahora puede realizar actualizaciones parciales de página a través de [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [API Web](../../../web-api/index.md) o [SignalR](../../../signalr/index.md). Debe detener mediante los métodos de devolución de llamada porque pueden causar problemas con direcciones URL descriptivas y enrutamiento. De forma predeterminada, los controles no habilitar métodos de devolución de llamada, pero si habilita esta característica en un control, debe deshabilitarlo.

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a>Detección de capacidad del explorador

Recomendación: Detenga usando la detección de la capacidad de explorador estático y, en su lugar, utilizar la detección de función dinámica.

En versiones anteriores de ASP.NET, las características admitidas para cada explorador que se almacenaban en un archivo XML. Compatibilidad de la característica de detección a través de una búsqueda estática no es el mejor método. Ahora, puede detectar dinámicamente un explorador la admite características mediante un marco de trabajo de detección de función, como [Modernizr](http://modernizr.com/). Detección de características determina la compatibilidad con cualquier intento de usar un método o propiedad y, a continuación, comprueba si el explorador genera el resultado deseado. De forma predeterminada, Modernizr se incluye en las plantillas de aplicación Web.

<a id="security"></a>

## <a name="security"></a>Seguridad

<a id="validation"></a>

### <a name="request-validation"></a>La validación de solicitudes

Recomendación: Validar proporcionados por el usuario y codifique la salida de los usuarios.

Validación de solicitud es una característica de ASP.NET que inspecciona cada solicitud y detiene la solicitud si no se encuentra una amenaza percibida. No dependen de la validación de solicitudes para proteger la aplicación frente a ataques de scripts entre sitios. En su lugar, validar todas las entradas de los usuarios y codificar la salida. En algunos casos limitados, puede usar expresiones regulares para validar la entrada, pero en casos más complicados que debe validar proporcionados por el usuario mediante el uso de clases de .NET que determinan si el valor coincide con valores permiten.

En el ejemplo siguiente se muestra cómo utilizar un método estático en la clase Uri para determinar si el Uri proporcionado por el usuario es válido.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

Sin embargo, para comprobar lo suficientemente el Uri, también debe comprobar para asegurarse de que especifica `http` o `https`. En el ejemplo siguiente se usa métodos de instancia para comprobar que el Uri es válido.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

Antes de presentar proporcionados por el usuario como HTML o incluir proporcionados por el usuario en una consulta SQL, codificar los valores para asegurarse de que no se incluye el código malintencionado.

Puede HTML codificar el valor en el marcado con el &lt;%: %&gt; sintaxis, tal y como se muestra a continuación.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

O bien, en la sintaxis de Razor, puede HTML codificar con @, tal y como se muestra a continuación.

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

El siguiente ejemplo se muestra cómo a HTML codifica un valor en el código subyacente.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

Para codificar de forma segura un valor para los comandos SQL, use los parámetros de comando como el [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx). <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a>Sesión y autenticación de formularios sin cookies

Recomendación: Necesita cookies.

No es seguro pasar información de autenticación en la cadena de consulta. Por lo tanto, requieren cookies cuando la aplicación incluye la autenticación. Si la cookie almacena información confidencial, considere la posibilidad de exigir SSL para la cookie.

En el ejemplo siguiente se muestra cómo especificar en el archivo Web.config que la autenticación de formularios requiere una cookie que se transmite a través de SSL.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a>EnableViewStateMac

Recomendación: Nunca establecida en false.

De forma predeterminada, se establece EnbableViewStateMac en true. Incluso si la aplicación no utiliza el estado de vista, no establezca EnableViewStateMac en false. Establecer este valor en false, la aplicación hará que sea vulnerable a scripting entre sitios.

A partir de ASP.NET 4.5.2, el tiempo de ejecución impone **EnableViewStateMac = true**. Incluso si se establece en false, el tiempo de ejecución omite este valor y continúa con el valor establecido en true. Para obtener más información, consulte [ASP.NET 4.5.2 y EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).

En el ejemplo siguiente se muestra cómo establecer EnableViewStateMac en true. No es necesario establecer este valor en true porque es true de forma predeterminada. Sin embargo, si se configura en false en cualquier página en la aplicación, debe corregir inmediatamente este valor.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a>Nivel de confianza medio

Recomendación: No dependen de nivel de confianza medio (o cualquier otro nivel de confianza) como un límite de seguridad.

Confianza parcial no protegen adecuadamente la aplicación y no debe usarse. En su lugar, use la plena confianza y aislar las aplicaciones que no se confía en grupos de aplicaciones independientes. Asimismo, ejecutar bajo una identidad única para cada grupo de aplicaciones. Para obtener más información, consulte [confianza parcial de ASP.NET no garantiza el aislamiento de aplicaciones](https://support.microsoft.com/kb/2698981).

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a>&lt;appSettings&gt;

Recomendación: Deshabilitar la configuración de seguridad en &lt;appSettings&gt; elemento.

El elemento appSettings contiene muchos valores que son necesarios para las actualizaciones de seguridad. No debe cambiar o deshabilitar estos valores. Si debe deshabilitar estos valores cuando se implementa una actualización, inmediatamente volver a habilitar después de completar la implementación.

Para obtener más información, consulte [appSettings ASP.NET elemento](https://msdn.microsoft.com/library/hh975440.aspx).

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a>UrlPathEncode

Recomendación: Utilizar [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) en su lugar.

El método UrlPathEncode se ha agregado a .NET Framework para resolver un problema de compatibilidad de explorador muy específico. No realiza ninguna codificación adecuadamente una dirección URL y no protege la aplicación de scripting entre sitios. Nunca debería utilizarlo en la aplicación. En su lugar, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).

En el ejemplo siguiente se muestra cómo pasar una dirección URL codificada como un parámetro de cadena de consulta para un control de hipervínculo.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a>Confiabilidad y rendimiento

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a>PreSendRequestHeaders y PreSendRequestContent

Recomendación: No use estos eventos con los módulos administrados. En su lugar, escribir un módulo nativo de IIS para llevar a cabo la tarea requerida. Vea [crear módulos HTTP de código nativo](https://msdn.microsoft.com/library/ms693629.aspx).

Puede usar el [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) y [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) eventos con los módulos nativos de IIS.
> [!WARNING]
> No utilice `PreSendRequestHeaders` y `PreSendRequestContent` con módulos administrados que implementan `IHttpModule`. Al establecer estas propiedades puede causar problemas con solicitudes asincrónicas. La combinación de enrutamiento solicitado aplicaciones (ARR) y websockets podría provocar excepciones de infracción de acceso que pueden causar w3wp se bloquee. ¡Por ejemplo, iiscore! W3_CONTEXT_BASE::GetIsLastNotification + 68 en iiscore.dll ha provocado una excepción de infracción de acceso (0xC0000005).

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a>Eventos de página asincrónica con formularios Web Forms

Recomendación: En formularios Web Forms, evite escribir async void métodos para eventos de ciclo de vida de la página y en su lugar, utilice [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) para código asincrónico.

Cuando marca un evento de página con **async** y **void**, no se puede determinar cuándo ha finalizado el código asincrónico. En su lugar, utilice Page.RegisterAsyncTask para ejecutar el código asincrónico de forma que le permite realizar un seguimiento de su finalización.

El siguiente ejemplo se muestra un botón, haga clic en controlador que contiene código asincrónico. Este ejemplo incluye leer de forma asincrónica, el valor de cadena que se proporciona únicamente como un ejemplo simplificado de una tarea asincrónica y no como un procedimiento recomendado.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

Si está utilizando las tareas asincrónicas, establecer la plataforma de destino en tiempo de ejecución de Http a 4.5 en el archivo Web.config. Establecer la plataforma de destino en 4.5 activa en el nuevo contexto de sincronización que se agregó en .NET 4.5. Este valor se establece de forma predeterminada en los nuevos proyectos en Visual Studio 2012, pero no es puede ser establecido si está trabajando con un proyecto existente.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a>Enviar y olvidarse de trabajo

Recomendación: Al procesar una solicitud de ASP.NET, evite iniciar trabajo enviar y olvidarse (este tipo llamando al método ThreadPool.QueueUserWorkItem o creando un temporizador que llama repetidamente a un delegado).

Si la aplicación tiene trabajo enviar y olvidarse que se ejecuta en ASP.NET, la aplicación puede obtener la sincronización. En cualquier momento, el dominio de aplicación se puedan destruir lo que significa que el proceso actual ya no puede coincidir con el estado actual de la aplicación.

Debe mover este tipo de trabajo fuera de ASP.NET. Puede utilizar los trabajos Web, servicio de Windows o un rol de trabajo en Azure para realizar el trabajo en curso y ejecutar ese código desde otro proceso.

Si va a realizar este trabajo en ASP.NET, puede agregar el paquete de Nuget denominado [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) para ejecutar el código.

<a id="requestentity"></a>

### <a name="request-entity-body"></a>Cuerpo de la entidad de solicitud

Recomendación: Evite leer Request.Form o Request.InputStream antes de que el controlador ejecute eventos.

El más antiguo debe leer desde Request.Form o Request.InputStream es durante el controlador ejecute eventos. En MVC, el controlador es el controlador y el evento de ejecución es cuando se ejecuta el método de acción. En formularios Web Forms, la página es el controlador y el evento de ejecución es cuando se desencadene el evento Page.Init. Si lee el cuerpo de la entidad de solicitud antes que el evento execute, interferir con el procesamiento de la solicitud.

Si tiene que leer el cuerpo de entidad de solicitud antes del evento execute, use [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) o [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx). Cuando usas GetBufferlessInputStream, obtener la secuencia sin formato de la solicitud y asuma la responsabilidad de procesamiento de la solicitud completa. Después de llamar a GetBufferlessInputStream, Request.Form y Request.InputStream no están disponibles porque no se han rellenado por ASP.NET. Cuando usas GetBufferedInputStream, obtendrá una copia de la secuencia de la solicitud. Request.Form y Request.InputStream siguen estando disponibles más adelante en la solicitud porque ASP.NET rellena la otra copia.

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a>Response.Redirect y Response.End

Recomendación: Tener en cuenta las diferencias en cómo se controla el subproceso después de llamar a [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).

El [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) método llama al método Response.End. En un proceso sincrónico, una llamada a Request.Redirect hace que el subproceso actual se anula inmediatamente. Sin embargo, en un proceso asincrónico, la llamada a Response.Redirect no anula el subproceso actual, por lo que continúe la ejecución del código para la solicitud. En un proceso asincrónico, se debe devolver la tarea desde el método para detener la ejecución del código.

En un proyecto MVC, no debería llamar a Response.Redirect. En su lugar, devuelven un RedirectResult.

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a>EnableViewState y ViewStateMode

Recomendación: Use ViewStateMode, en lugar de EnableViewState, para proporcionar un control granular sobre el que los controles utilizan el estado de vista.

Cuando EnableViewState se establece en false en la directiva de página, el estado de vista está deshabilitado para todos los controles en la página y no se puede habilitar. Si desea habilitar el estado de vista de solo ciertos controles en la página, establezca ViewStateMode en deshabilitado para la página.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

A continuación, establezca ViewStateMode habilitado sólo en los controles que realmente necesitan el estado de vista.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

Al habilitar el estado de vista de solo los controles que lo necesitan, puede reducir el tamaño del estado de vista para las páginas web.

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a>SqlMembershipProvider

Recomendación: Utilizar proveedores universales.

En las plantillas de proyecto actual, SqlMembershipProvider se ha reemplazado por [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), que está disponible como un paquete de NuGet. Si usas SqlMembershipProvider en un proyecto que se compiló con una versión anterior de las plantillas, debería pasar a los proveedores universales. Los proveedores universales funcionan con todas las bases de datos que son compatibles con Entity Framework.

Para obtener más información, consulte [Introducción a ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a>Las solicitudes de ejecución prolongada (> 110 segundos)

Recomendación: Utilizar [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) o [SignalR](../../../signalr/index.md) para los clientes conectados y operaciones asincrónicas de E/S de uso.

Las solicitudes de ejecución prolongada pueden provocar resultados imprevisibles y un rendimiento bajo en la aplicación web. El valor de tiempo de espera predeterminado para una solicitud es 110 segundos. Si usas el estado de sesión con una solicitud de ejecución prolongada, ASP.NET volverá a liberar el bloqueo en el objeto de sesión después de 110 segundos. Sin embargo, la aplicación puede estar en el medio de una operación en el objeto de sesión cuando se libere el bloqueo y la operación no se puede completar correctamente. Si una segunda solicitud del usuario se bloquea mientras se ejecuta la primera solicitud, la segunda solicitud puede tener acceso al objeto de sesión en un estado incoherente.

Si la aplicación incluye las operaciones de E/S de bloqueo (o sincrónicas), la aplicación estará no responde.

Para mejorar el rendimiento, utilice las operaciones de E/S asincrónicas en .NET Framework. Además, usar WebSockets o SignalR para conectar los clientes al servidor. Estas características están diseñadas para administrar eficazmente las solicitudes de ejecución prolongada.
