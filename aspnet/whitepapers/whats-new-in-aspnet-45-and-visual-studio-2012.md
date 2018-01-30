---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: Novedades de ASP.NET 4.5 y Visual Studio 2012 | Documentos de Microsoft
author: rick-anderson
description: "Este documento describe las nuevas características y mejoras que se plantean en ASP.NET 4.5. También se describen las mejoras que se va a realizar para el desarrollo web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/29/2012
ms.topic: article
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 16a2fa4b1b6617430703853543cbf29e18ba1103
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
<a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>Novedades de ASP.NET 4.5 y Visual Studio 2012
====================
> Este documento describe las nuevas características y mejoras que se plantean en ASP.NET 4.5. También describen las mejoras que se va a realizar para el desarrollo web en Visual Studio 2012. Este documento se publicó originalmente en 29 de febrero de 2012.


- [Marco de trabajo y en tiempo de ejecución de ASP.NET Core](#_Toc318097372)

    - [Asincrónicamente leer y escribir solicitudes y respuestas HTTP](#_Toc318097373)
    - [Mejoras para el control de HttpRequest](#_Toc318097374)
    - [El vaciado de forma asincrónica una respuesta](#_Toc318097375)
    - [Compatibilidad con *await* y *tarea*-según asincrónica módulos y controladores](#_Toc318097376)
    - [Módulos HTTP asincrónicos](#_Toc318097377)
    - [Controladores HTTP asincrónicos](#_Toc318097378)
    - [Nuevas características de validación de solicitud de ASP.NET](#_Toc318097379)
    - [Aplazado la validación de solicitud ("perezoso")](#_Toc318097380)
    - [Soporte técnico para las solicitudes no validadas](#_Toc318097381)
    - [Biblioteca de AntiXSS](#_Toc318097382)
    - [Compatibilidad con el protocolo WebSockets](#_Toc318097383)
    - [Unión y minificación](#_Toc318097384)
    - [Mejoras de rendimiento para el hospedaje Web](#_Toc_perf)

        - [Factores clave de rendimiento](#_Toc_perf_1)
        - [Requisitos para las nuevas características de rendimiento](#_Toc_perf_2)
        - [Compartir ensamblados comunes](#_Toc_perf_3)
        - [Uso de la compilación JIT varios núcleos para el inicio rápido](#_Toc_perf_4)
        - [Colección de elementos no utilizados para optimizar la memoria para la optimización](#_Toc_perf_5)
        - [Precarga de aplicaciones web](#_Toc_perf_6)
- [Formularios Web Forms ASP.NET](#_Toc318097385)

    - [Controles de datos fuertemente tipados](#_Toc318097386)
    - [Enlace de modelos](#_Toc318097387)

        - [Seleccionar datos](#_Toc318097388)
        - [Proveedores de valores](#_Toc318097389)
        - [Filtrar por valores de un control](#_Toc318097390)
    - [Expresiones de enlace de datos codificado en HTML](#_Toc318097391)
    - [Validación discreto](#_Toc318097392)
    - [Actualizaciones de HTML5](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [ASP.NET Web Pages 2](#_Toc318097395)
- [Visual Studio 2012 Release Candidate](#_Toc318097396)

    - [Proyecto de uso compartido entre Visual Studio 2010 y Visual Studio 2012 Release Candidate (compatibilidad de proyectos)](#project-compatibility)
    - [Cambios de configuración en las plantillas de sitio Web 4.5 de ASP.NET](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [Compatibilidad nativa con en IIS 7 para el enrutamiento de ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [Editor de HTML](#_Toc318097397)

        - [Tareas inteligentes](#_Toc318097398)
        - [Compatibilidad con ARIA WAI](#_Toc318097399)
        - [Nuevos fragmentos de código de HTML5](#_Toc318097400)
        - [Extraer a control de usuario](#_Toc318097401)
        - [IntelliSense para fragmentos de código en atributos](#_Toc318097402)
        - [Cambio de nombre automático de la coincidencia de etiqueta al cambiar el nombre de una etiqueta inicial o final](#_Toc318097403)
        - [Generación de controlador de eventos](#_Toc318097404)
        - [Sangría automática](#_Toc318097405)
        - [Reducir automáticamente la finalización de instrucciones](#_Toc318097406)
    - [JavaScript Editor](#_Toc318097407)

        - [Esquematización de código](#_Toc318097408)
        - [Coincidencia de llaves](#_Toc318097409)
        - [Ir a definición](#_Toc318097410)
        - [Soporte técnico de ECMAScript5](#_Toc318097411)
        - [IntelliSense de DOM](#_Toc318097412)
        - [Sobrecargas de firma VSDOC](#_Toc318097413)
        - [Referencias implícitas](#_Toc318097414)
    - [Editor CSS](#_Toc318097415)

        - [Reducir automáticamente la finalización de instrucciones](#_Toc318097416)
        - [Sangría jerárquica.](#_Toc318097417)
        - [Soporte técnico de ataques de CSS](#_Toc318097418)
        - [Los esquemas específicos de proveedor (- moz-, - webkit)](#_Toc318097419)
        - [Compatibilidad con comentario y eliminar el comentario](#_Toc318097420)
        - [Selector de colores](#_Toc318097421)
        - [Fragmentos de código](#_Toc318097422)
        - [Regiones personalizadas](#_Toc318097423)
    - [Inspector de página](#_Toc318097424)
    - [Publicar](#_Toc318097425)

        - [Perfiles de publicación](#_Toc318097426)
        - [Mezclar y precompilación de ASP.NET](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [Disclaimer](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>Marco de trabajo y en tiempo de ejecución de ASP.NET Core

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>Asincrónicamente leer y escribir solicitudes y respuestas HTTP

ASP.NET 4 incorpora la capacidad de leer una entidad de solicitud HTTP como una secuencia utilizando el *HttpRequest.GetBufferlessInputStream* método. Este método proporciona acceso de transmisión por secuencias a la entidad de solicitud. Sin embargo, ejecutan sincrónicamente, que acumular la un subproceso para la duración de una solicitud.

ASP.NET 4.5 admite la capacidad de leer secuencias de forma asincrónica en una entidad de solicitud HTTP y la capacidad de vaciado de forma asincrónica. ASP.NET 4.5 también ofrece la capacidad de una entidad de solicitud HTTP, que proporciona una integración más sencilla con los controladores HTTP, como controladores de la página .aspx y ASP.NET MVC controladores el doble búfer.

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>Mejoras para el control de HttpRequest

La referencia de la secuencia devuelta por ASP.NET 4.5 de *HttpRequest.GetBufferlessInputStream* es compatible con métodos de lectura sincrónicos y asincrónicos. El *flujo* objeto devuelto desde *GetBufferlessInputStream* ahora implementa métodos el BeginRead y EndRead. Asincrónico *flujo* métodos permiten leer de forma asincrónica la entidad de solicitud en fragmentos, mientras que ASP.NET libera el subproceso actual entre cada iteración de un bucle de lectura asincrónico.

ASP.NET 4.5 también agregó un método complementaria para leer la entidad de solicitud de una manera almacenado en búfer: *HttpRequest.GetBufferedInputStream*. Esta nueva sobrecarga funciona como *GetBufferlessInputStream*, compatibilidad con Lecturas sincrónicas y asincrónicas. Sin embargo, tal y como lee, *GetBufferedInputStream* también copia los bytes de la entidad en búferes internos de ASP.NET para que los módulos y los controladores aún pueden acceder a la entidad de solicitud. Por ejemplo, aunque algunas en dirección ascendente código en la canalización ya leído la entidad de solicitud con *GetBufferedInputStream*, todavía puede usar *HttpRequest.Form* o *HttpRequest.Files*. Esto le permite realizar un procesamiento asincrónico en una solicitud (por ejemplo, una carga de archivos grandes a una base de datos de transmisión por secuencias), pero las páginas .aspx todavía ejecución y controladores de ASP.NET MVC posteriormente.

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>El vaciado de forma asincrónica una respuesta

Enviar las respuestas a un cliente HTTP puede tardar bastante tiempo cuando el cliente se encuentra lejos o tiene una conexión de ancho de banda bajo. Normalmente ASP.NET almacena en búfer los bytes de respuesta como se crean mediante una aplicación. ASP.NET, a continuación, realiza una sola operación de envío de los búferes acumuladas al final del procesamiento de la solicitud.

Si la respuesta almacenada en búfer es grande (por ejemplo, un archivo grande a un cliente de transmisión por secuencias), debe llamar periódicamente *HttpResponse.Flush* para enviar la salida del búfer al cliente y mantener el uso de memoria bajo el control. Sin embargo, dado que *vaciar* es una llamada sincrónica, llamar de forma iterativa *vaciar* todavía consume un subproceso para la duración de las solicitudes de larga duración.

ASP.NET 4.5 agrega compatibilidad para realizar vaciados de forma asincrónica utilizando el *BeginFlush* y *EndFlush* métodos de la *HttpResponse* clase. Uso de estos métodos, puede crear módulos asincrónicos y controladores asincrónicos que envían datos de forma incremental a un cliente sin afectar a subprocesos del sistema operativo. Entre *BeginFlush* y *EndFlush* llamadas, ASP.NET libera el subproceso actual. Este modo reduce el número total de subprocesos activos que son necesarios para admitir la descarga HTTP de ejecución prolongada.

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>Compatibilidad con *await* y *tarea* -según asincrónica módulos y controladores

.NET Framework 4 se introdujo un concepto de programación asincrónico conocido como un *tarea*. Las tareas se representan por el *tarea* tipo y los tipos relacionados en el *System.Threading.Tasks* espacio de nombres. .NET Framework 4.5 se basa en esto con las mejoras de compilador que facilitan el trabajo con *tarea* simple de objetos. En .NET Framework 4.5, los compiladores admiten dos nuevas palabras clave: *await* y *async*. El *await* palabra clave es una abreviatura sintáctica para indicar que debe esperar un fragmento de código de forma asincrónica en alguna otra parte del código. El *async* palabra clave representa una sugerencia que puede usar para marcar los métodos como métodos asincrónicos basado en tareas.

La combinación de *await*, *async*y el *tarea* objeto, resulta más fácil de escribir código asincrónico en .NET 4.5. ASP.NET 4.5 es compatible con estas medidas de simplificación con nuevas API que le permiten escribir asincrónicos módulos HTTP y controladores HTTP asincrónicos mediante las nuevas mejoras del compilador.

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>Módulos HTTP asincrónicos

Suponga que desea realizar el trabajo asincrónico dentro de un método que devuelve un *tarea* objeto. En el ejemplo de código siguiente se define un método asincrónico que realiza una llamada asincrónica para descargar página principal de Microsoft. Observe el uso de la *async* palabra clave en la firma del método y la *await* la llamada a *DownloadStringTaskAsync*.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

Eso es todo lo que tiene que escribir: .NET Framework controlará automáticamente desenredar la pila de llamadas mientras se espera a que finalice la descarga, así como restaurar automáticamente la pila de llamadas después de realiza la descarga.

Ahora suponga que desea usar este método asincrónico en un módulo HTTP de ASP.NET asincrónico. ASP.NET 4.5 incluye un método auxiliar (*EventHandlerTaskAsyncHelper*) y un nuevo tipo de delegado (*TaskEventHandler*) que puede usar para integrar métodos asincrónicos basados en la tarea con la versión más antigua modelo de programación asincrónica expuesto por la canalización HTTP de ASP.NET. Este ejemplo se muestra cómo:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>Controladores HTTP asincrónicos

El enfoque tradicional para escribir controladores asincrónicos en ASP.NET consiste en implementar la *IHttpAsyncHandler* interfaz. ASP.NET 4.5 presenta la *HttpTaskAsyncHandler* asincrónica tipo base que se puede derivar de, lo que facilita la escritura de controladores asincrónicos.

El *HttpTaskAsyncHandler* tipo es abstracto y debe invalidar el *ProcessRequestAsync* método. Internamente ASP.NET se encarga de la integración de la firma del valor devuelta (un *tarea* objeto) de *ProcessRequestAsync* con el modelo de programación asincrónico anterior utilizado por la canalización ASP.NET.

En el ejemplo siguiente se muestra cómo se puede utilizar *tarea* y *await* como parte de la implementación de un controlador HTTP asincrónico:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>Nuevas características de validación de solicitud de ASP.NET

De forma predeterminada, ASP.NET realiza la validación de solicitud: examina las solicitudes para buscar el marcado o el script en campos, encabezados, cookies y así sucesivamente. Si se detecta alguno, ASP.NET produce una excepción. Esto actúa como una primera línea de defensa contra posibles ataques de scripts entre sitios.

ASP.NET 4.5 facilita la lectura selectivamente datos de la solicitud no validados. ASP.NET 4.5 también se integra a la biblioteca de AntiXSS popular, que antes era una biblioteca externa.

Los desarrolladores han solicitado con frecuencia la capacidad de desactivar selectivamente la validación de solicitudes para sus aplicaciones. Por ejemplo, si la aplicación es un software de foro, puede permitir a los usuarios enviar mensajes con formato HTML foro y comentarios, pero aun así, asegúrese de que la validación de solicitudes está comprobando todo lo demás.

ASP.NET 4.5 presenta dos características que facilitan la selectivamente trabajar con entradas no validados: deferred validación de solicitudes ("perezoso") y el acceso a datos de la solicitud no validados.

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>Aplazado la validación de solicitud ("perezoso")

En ASP.NET 4.5, que de forma predeterminada todos los datos de la solicitud está sujeto a validación de solicitudes. Sin embargo, puede configurar la aplicación para aplazar la validación de solicitudes hasta que realmente tener acceso a datos de la solicitud. (Esto se conoce a veces como una validación diferida de solicitud, basada en términos como la carga diferida para algunos escenarios de datos.) También puede configurar la aplicación para usar la validación diferida en el archivo Web.config estableciendo el *requestValidationMode* atribuir a 4.5 en el *httpRUntime* elemento, como en el ejemplo siguiente:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

Cuando el modo de validación de solicitud se establece en 4.5, se desencadena la validación de solicitudes solo para un valor de solicitud específico y únicamente cuando el código tiene acceso a ese valor. Por ejemplo, si el código obtiene el valor de Request.Form["forum\_registrar"], se invoca la validación de solicitud solo para ese elemento de la colección de formulario. Ninguno de los demás elementos de la *formulario* se validan la colección. En versiones anteriores de ASP.NET, la validación de solicitudes se activó para la colección de solicitud completo cuando se tiene acceso a cualquier elemento de la colección. El nuevo comportamiento resulta más fácil para los componentes de aplicación diferente buscar en diferentes partes de datos de la solicitud sin que se desencadene la validación de solicitudes en otras partes.

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>Soporte técnico para las solicitudes no validadas

Validación de solicitud diferida por sí sola no resuelve el problema de forma selectiva omitir la validación de solicitudes. La llamada a Request.Form["forum\_registrar"] todavía desencadenadores validación de solicitudes de ese valor de solicitud específicos. Sin embargo, puede tener acceso a este campo sin desencadenar la validación porque desea permitir marcado en ese campo.

Para permitir esto, ASP.NET 4.5 admite ahora no validado acceso a datos de la solicitud. ASP.NET 4.5 incluye un nuevo *Unvalidated* propiedad de colección en el *HttpRequest* clase. Esta colección proporciona acceso a todos los valores comunes de datos de la solicitud, como *formulario*, *QueryString*, *Cookies*, y *Url*.

Con el ejemplo de foro, para poder leer los datos de la solicitud no validados, primero debe configurar la aplicación para utilizar el nuevo modo de validación de solicitud:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

A continuación, puede usar el *HttpRequest.Unvalidated* propiedad que se va a leer el valor de formulario no validados:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]


> [!WARNING]
> Seguridad - *usar datos de la solicitud no validados con cuidado!* ASP.NET 4.5 agrega las propiedades de la solicitud no validados y recopilaciones para que sea más fácil para que pueda tener acceso a datos de la solicitud no validados muy específico. Sin embargo, todavía debe realizar la validación personalizada en los datos de solicitud sin formato para asegurarse de que no se representa texto peligroso a los usuarios.


<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>Biblioteca de AntiXSS

Debido a la popularidad de Microsoft AntiXSS Library, ASP.NET 4.5 incorpora ahora rutinas de codificación de núcleo de la versión 4.0 de la biblioteca.

Las rutinas de codificación se implementan mediante la *AntiXssEncoder* tipo en el nuevo *System.Web.Security.AntiXss* espacio de nombres. Puede usar el *AntiXssEncoder* tipo directamente mediante una llamada a cualquiera de los métodos estáticos de codificación que se implementan en el tipo. Sin embargo, es el enfoque más sencillo para el uso de las rutinas de anti-XSS nueva configurar una aplicación ASP.NET para que use la *AntiXssEncoder* clase de forma predeterminada. Para ello, agregue el siguiente atributo en el archivo Web.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

Cuando el *encoderType* atributo está configurado para usar el *AntiXssEncoder* tipo, toda la salida automáticamente la codificación en ASP.NET usa las rutinas de codificación de nuevo.

Estas son las partes de la biblioteca de AntiXSS externa que se han incorporado a ASP.NET 4.5:

- *HtmlEncode*, *HtmlFormUrlEncode*, y *HtmlAttributeEncode*
- *XmlAttributeEncode* y *XmlEncode*
- *UrlEncode* y *UrlPathEncode* (nuevo)
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>Compatibilidad con el protocolo WebSockets

Protocolo WebSockets es un protocolo de red basado en estándares que define cómo establecer comunicaciones bidireccionales seguro y en tiempo real entre un cliente y un servidor a través de HTTP. Microsoft ha trabajado con organismos de estándares IETF tanto de W3C para ayudar a definir el protocolo. El protocolo WebSockets es compatible con cualquier cliente (no solo los exploradores) con Microsoft invertir bastantes recursos de la ejecución de protocolo WebSockets en el cliente y sistemas operativos móviles.

Protocolo WebSockets hace mucho más fácil crear las transferencias de datos de ejecución prolongada entre un cliente y un servidor. Por ejemplo, es mucho más fácil escribir una aplicación de chat, dado que puede establecer una conexión de larga duración true entre un cliente y un servidor. No tiene que recurrir a soluciones alternativas como sondeo periódico o sondeo largo HTTP para simular el comportamiento de un socket.

ASP.NET 4.5 y IIS 8 incluyen compatibilidad con bajo nivel de WebSockets, permitiendo a los programadores ASP.NET usar las API administradas para leer y escribir datos binarios y cadena en un objeto de WebSockets de forma asincrónica. Para ASP.NET 4.5, hay una nueva *System.Web.WebSockets* espacio de nombres que contiene tipos para trabajar con el protocolo WebSockets.

Un explorador del cliente establece una conexión de WebSockets mediante la creación de un DOM *WebSocket* objeto que apunta a una dirección URL en una aplicación de ASP.NET, como en el ejemplo siguiente:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

Puede crear extremos de WebSockets en ASP.NET utilizando cualquier tipo de módulo o controlador. En el ejemplo anterior, se usó un archivo .ashx, porque .ashx (archivos) son una forma rápida de crear un controlador.

Según el protocolo WebSockets, una aplicación ASP.NET acepta la solicitud de WebSockets de un cliente indicando que la solicitud se debe actualizar de una solicitud HTTP GET a una solicitud de WebSockets. Por ejemplo:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

El *AcceptWebSocketRequest* método acepta un delegado de función porque ASP.NET desenreda la solicitud HTTP actual y, a continuación, transfiere el control al delegado de función. Conceptualmente, este enfoque es similar a cómo usar *System.Threading.Thread*, donde defina un delegado de inicio del subproceso en qué plano se realiza el trabajo.

Después de que ASP.NET y el cliente han completado correctamente un protocolo de enlace de WebSockets, ASP.NET llama a un delegado y la aplicación de WebSockets empieza a ejecutarse. En el ejemplo de código siguiente se muestra una aplicación de eco simple que utiliza la compatibilidad con WebSockets en ASP.NET:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

La compatibilidad de .NET 4.5 para el *await* palabra clave y operaciones asincrónicas basadas en tareas es una opción natural para escribir aplicaciones de WebSockets. El ejemplo de código se muestra que una solicitud de WebSockets completamente de forma asincrónica se ejecuta dentro de ASP.NET. La aplicación de forma asincrónica espera un mensaje se envíe desde un cliente mediante una llamada a *await socket. ReceiveAsync*. De forma similar, puede enviar un mensaje asincrónico a un cliente mediante una llamada a *await socket. SendAsync*.

En el explorador, una aplicación recibe mensajes de WebSockets a través de un *onmessage* (función). Para enviar un mensaje desde un explorador, se llama a la *enviar* método de la *WebSocket* tipo de DOM, como se muestra en este ejemplo:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

En el futuro, tengamos que publicamos las actualizaciones de esta funcionalidad que abstracta inmediatamente algunos de los que es de bajo nivel codificación necesitan en esta versión websockets aplicaciones.

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>Agrupar y Minificar

Cómo agrupar le permite combinar archivos individuales de JavaScript y CSS en un paquete que se pueden tratar como un único archivo. Reducción condensa archivos JavaScript y CSS mediante la eliminación de espacio en blanco y otros caracteres que no sean necesarios. Estas características funcionan con formularios Web Forms, MVC de ASP.NET y páginas Web.

Los paquetes se crean mediante la clase de paquete o uno de sus clases secundarias, ScriptBundle y StyleBundle. Después de configurar una instancia de un paquete, el paquete estará disponible a las solicitudes entrantes simplemente agregándolo a una instancia de BundleCollection global. En las plantillas predeterminadas, la configuración de agrupación se realiza en un archivo BundleConfig. Esta configuración predeterminada crea agrupaciones para todos los scripts de núcleo y archivos css utilizados por las plantillas.

Agrupaciones se hace referencia desde dentro de las vistas mediante uno de dos métodos auxiliares posibles. Para admitir la representación de marcado diferente para una agrupación en la depuración frente a modo de lanzamiento, ScriptBundle StyleBundle tienen las clases y el método auxiliar, representar. Cuando se encuentra en modo de depuración, representación generará marcado para cada recurso de la agrupación. Cuando se encuentra en modo de lanzamiento, representación generará un elemento único marcado para toda la agrupación. Alternar entre debug y release modo puede realizarse si modifica el atributo de depuración del elemento de compilación en el archivo web.config tal y como se muestra a continuación:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

Además, habilitar o deshabilitar la optimización se puede establecer directamente a través de la propiedad BundleTable.EnableOptimizations.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

Cuando los archivos están agrupados, primero se ordenan alfabéticamente (la forma en que se muestran en **el Explorador de soluciones**). Que estén organizadas, a continuación, para que reconozcan las bibliotecas y sus extensiones personalizadas (por ejemplo, jQuery, MooTools y Dojo) se cargan en primer lugar. Por ejemplo, el orden final para la agrupación de la carpeta de secuencias de comandos como se indicó anteriormente será:

1. jquery-1.6.2.js
2. jQuery ui.js
3. jQuery.Tools.js
4. a.js

Archivos CSS son también se ordenan alfabéticamente y, a continuación, se reorganizan para que reset.css y normalize.css preceden a cualquier otro archivo. La ordenación final de la agrupación de la carpeta Styles que se muestra arriba, será esto:

1. Reset.CSS
2. Content.CSS
3. Forms.CSS
4. globals.css
5. Menu.CSS
6. Styles.CSS

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Mejoras de rendimiento para el hospedaje Web

.NET Framework 4.5 y Windows 8 incorporan características que pueden ayudarle a lograr un aumento significativo del rendimiento para cargas de trabajo de servidor web. Esto incluye una reducción (hasta un 35%) en ambos tiempo de inicio y en la superficie de memoria de hospedaje de sitios que usen ASP.NET.

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>Factores clave de rendimiento

Idealmente, todos los sitios Web deben estar activos y en la memoria para garantizar una respuesta rápida a la solicitud siguiente, cada vez que se trata. Entre los factores que pueden afectar a la capacidad de respuesta de sitio se incluyen:

- El tiempo necesario para que un sitio que reiniciar después de un grupo de aplicaciones se recicla. Este es el tiempo necesario para iniciar un proceso de servidor web para el sitio cuando los ensamblados de sitio ya no están en memoria. (Los ensamblados de plataforma siguen estando en memoria, ya que se utilizan otros sitios). Esta situación se conoce como "sitio frío, inicio de framework semiactiva" o simplemente "frío el inicio del sitio."
- La cantidad de memoria que ocupa el sitio. Términos de esto son "consumo de memoria de cada sitio" o "uso no compartido espacio de trabajo".

Las nuevas mejoras de rendimiento se centran en ambos de estos factores.

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>Requisitos para las nuevas características de rendimiento

Los requisitos para las nuevas características pueden dividirse en las siguientes categorías:

- Mejoras en la que se ejecutan en .NET Framework 4.
- Mejoras que requieren .NET Framework 4.5 pero que pueden ejecutar en cualquier versión de Windows.
- Mejoras en el que están disponibles solo con .NET Framework 4.5 con Windows 8.

El rendimiento aumenta con cada nivel de mejora que puede habilitar.

Algunas de las mejoras de .NET Framework 4.5 sacar partido de las características de rendimiento más amplias que se aplican a otros escenarios también.

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>Compartir ensamblados comunes

**Requisito**: .NET Framework 4 y Visual Studio 11 Developer Preview SDK

Sitios diferentes en un servidor a menudo utilizan los mismos ensamblados de aplicación auxiliar (por ejemplo, los ensamblados desde una aplicación de starter kit o ejemplo). Cada sitio tiene su propia copia de estos ensamblados en el directorio Bin. Aunque el código de objeto para los ensamblados es idéntico, que están físicamente separado de los ensamblados, por lo que cada ensamblado tiene que leerse por separado durante el inicio del sitio frío y se mantienen por separado en la memoria.

La nueva funcionalidad interning resuelve esta ineficacia y reduce los requisitos de RAM y carga de tiempo. Grupo interno de cadenas permite Windows tenga una copia única de cada ensamblado en el sistema de archivos y ensamblados individuales en las carpetas de la ubicación de sitio se reemplazan con los vínculos simbólicos a la copia única. Si un sitio en particular necesita una versión distinta del ensamblado, el vínculo simbólico se reemplaza por la nueva versión del ensamblado y se ve afectado solo a ese sitio.

Compartir ensamblados con vínculos simbólicos requiere una nueva herramienta denominada aspnet\_intern.exe, que le permite crear y administrar el almacén de ensamblados aplicado el método Intern. Se proporciona como parte de los SDK de Visual Studio 11 Developer Preview. (Sin embargo, funcionará en un sistema que tiene solo el instalado .NET Framework 4, suponiendo que haya instalado la versión más reciente [actualizar](https://support.microsoft.com/kb/2468871).)

Para asegurarse de que todos los ensamblados válidos se ha aplicado el método Intern, ejecute aspnet\_intern.exe periódicamente (por ejemplo, una vez por semana como una tarea programada). Un uso típico es como sigue:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

Para ver todas las opciones, ejecute la herramienta sin argumentos.

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>Uso de la compilación JIT varios núcleos para el inicio rápido

**Requisito**: .NET Framework 4.5

Para el inicio de un sitio frío, no solo es necesario ensamblados que se leen del disco, pero el sitio debe ser compilado JIT. Para un sitio complejo, esto puede agregar retrasos significativos. Una nueva técnica de uso general en .NET Framework 4.5 reduce estos retrasos al repartir la compilación JIT entre núcleos de procesador disponibles. Esto consigue tanta y tan pronto como sea posible mediante el uso de la información recopilada durante la anterior inicia del sitio. Esta funcionalidad implementada por el [System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx) método.

Con varios núcleos de compilación JIT está habilitada de forma predeterminada en ASP.NET, por lo que no es necesario hacer nada para aprovechar las ventajas de esta característica. Si desea deshabilitar esta característica, realice la siguiente configuración en el archivo Web.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>Colección de elementos no utilizados para optimizar la memoria para la optimización

**Requisito**: .NET Framework 4.5

Una vez que se ejecuta un sitio, el uso del montón del recolector de elementos no utilizados (GC) puede ser un factor importante en su consumo de memoria. Al igual que cualquier recolector de elementos no utilizados, el GC de .NET Framework hace contrapartidas tiempo de CPU (frecuencia y la importancia de las colecciones) y el consumo de memoria (espacio adicional que se utiliza para los objetos nuevos, liberados o pueda liberar). Para las versiones anteriores, se proporcionan instrucciones sobre cómo configurar el catálogo global para lograr el equilibrio adecuado (por ejemplo, vea [configuración de hospedaje de compartida de ASP.NET 2.0 y 3.5](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).

Para .NET Framework 4.5, en lugar de varias opciones de configuración independiente, un valor de configuración definido por la carga de trabajo está disponible que permite a todos los de la configuración de GC previamente recomendada así como para la optimización nueva que ofrece un rendimiento adicional para el sitio por espacio de trabajo.

Para habilitar la optimización de la memoria de GC, agregue la siguiente configuración al archivo Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(Si está familiarizado con las instrucciones anteriores para los cambios en el archivo aspnet.config, tenga en cuenta que esta configuración reemplaza la configuración anterior, por ejemplo, no hay ninguna necesidad de establecer gcServer, gcConcurrent, etcetera. No es necesario quitar los antiguos valores.)

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>Precarga de aplicaciones web

**Requisito**: .NET Framework 4.5 que se ejecutan en Windows 8

Para varias versiones de Microsoft ha incluido una tecnología conocida como la [precapturador](http://en.wikipedia.org/wiki/Prefetcher) que reduce el costo de lectura del disco de inicio de la aplicación. Dado que el inicio en frío es un problema principalmente para las aplicaciones cliente, esta tecnología no se ha incluido en Windows Server, que incluye solo los componentes que son esenciales para un servidor. La captura previa ahora está disponible en la versión más reciente de Windows Server, donde puede optimizar el inicio de sitios Web individuales.

Para Windows Server, el precapturador no está habilitado de forma predeterminada. Para habilitar y configurar el precapturador para el hospedaje web de alta densidad, ejecute el siguiente conjunto de comandos en la línea de comandos:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

A continuación, para integrar el precapturador con aplicaciones ASP.NET, agregue lo siguiente al archivo Web.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>Formularios Web Forms de ASP.NET

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>Controles de datos fuertemente tipados

En ASP.NET 4.5, formularios Web Forms incluye algunas mejoras para trabajar con datos. La primera mejora es controles de datos fuertemente tipados. Para los controles de formularios Web Forms en versiones anteriores de ASP.NET, se muestra un valor enlazado a datos mediante *Eval* y una expresión de enlace de datos:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

Para el enlace de datos bidireccional, use *enlazar*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

En tiempo de ejecución, estas llamadas usar la reflexión para leer el valor del miembro especificado y, a continuación, mostrar el resultado en el marcado. Este enfoque resulta muy sencillo enlazar los datos con datos arbitrarios, unshaped.

Sin embargo, las expresiones de enlace de datos así no admiten características como IntelliSense para los nombres de miembro, navegación (por ejemplo, ir a definición) o comprobación de estos nombres en el tiempo de compilación.

Para solucionar este problema, ASP.NET 4.5 agrega la capacidad de declarar el tipo de datos de los datos que está enlazado un control. Esto se hace mediante el nuevo *ItemType* propiedad. Cuando se establece esta propiedad, dos nuevas variables con tipo están disponibles en el ámbito de las expresiones de enlace de datos: *elemento* y *BindItem*. Dado que las variables tienen establecimiento inflexible de tipos, obtener todos los beneficios de la experiencia de desarrollo de Visual Studio.


Para las expresiones de enlace de datos bidireccionales, use la *BindItem* variable:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]


Mayoría de los controles en el marco de trabajo de formularios Web Forms de ASP.NET que admite enlace de datos se han actualizado para admitir la *ItemType* propiedad.

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>Enlace de modelos

Enlace de modelos extiende el enlace de datos en controles de formularios Web Forms de ASP.NET para que funcione con acceso a datos centrada en el código. Incorpora también conceptos de la *ObjectDataSource* control y del enlace de modelo de ASP.NET MVC.

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>Seleccionar datos

Para configurar un control de datos para utilizar el enlace de modelos para seleccionar datos, debe establecer el control *SelectMethod* propiedad en el nombre de un método en código de la página. El control de datos llama al método en el momento adecuado en el ciclo de vida de la página y enlaza automáticamente los datos devueltos. No es necesario llamar explícitamente a la *enlazar* método.

En el ejemplo siguiente, la *GridView* control está configurado para usar un método denominado *GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

Crear el *GetCategories* método en código de la página. Para una operación de selección simple, el método no necesita ningún parámetro y debe devolver un *IEnumerable* o *IQueryable* objeto. Si el nuevo *ItemType* se establece la propiedad (lo que permite fuertemente tipadas expresiones de enlace de datos, como se explica en [controles datos fuertemente tipados](#_Toc318097386) anteriormente), las versiones de estas interfaces genéricas se debe devolver: *IEnumerable&lt;T&gt;*  o *IQueryable&lt;T&gt;*, con el *T* parámetro que coincida con el tipo de la *ItemType* propiedad (por ejemplo, *IQueryable&lt;categoría&gt;*).

En el ejemplo siguiente se muestra el código de un *GetCategories* método. En este ejemplo se usa el modelo de Entity Framework Code First con la base de datos de ejemplo Northwind. El código se asegura de que la consulta devuelve los detalles de los productos relacionados para cada categoría por medio de la *Include* método. (Esto garantiza que la *TemplateField* elemento en el marcado muestra el recuento de productos en cada categoría sin necesidad de un [n + 1 Seleccione](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem).)

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

Cuando se ejecuta la página, el *GridView* controlar las llamadas de la *GetCategories* método automáticamente y representa los datos devueltos con los campos configurados:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Dado que el método select devuelve un *IQueryable* objeto, el *GridView* control puede manipular la consulta antes de ejecutarlo. Por ejemplo, el *GridView* control puede agregar expresiones de consulta para ordenar y paginar en el valor devuelto *IQueryable* objeto antes de que se ejecute, por lo que estas operaciones se realizan por subyacente Proveedor LINQ. En este caso, Entity Framework asegurará que esas operaciones se realizan en la base de datos.

El siguiente ejemplo se muestra la *GridView* control modificado para permitir la ordenación y paginación:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

Ahora cuando se ejecuta la página, el control puede asegurarse que se muestra solo la página actual de datos y que está ordenado por la columna seleccionada:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

Para filtrar los datos devueltos, parámetros tienen que agregarse para el método select. Estos parámetros se llenará con el enlace del modelo en tiempo de ejecución y se puede usar para modificar la consulta antes de devolver los datos.

Por ejemplo, suponga que desea permitir que los usuarios filtrar productos escribiendo una palabra clave en la cadena de consulta. Puede agregar un parámetro al método y actualice el código para usar el valor del parámetro:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

Este código incluye un *donde* expresión si se proporciona un valor para *palabra clave* y, a continuación, devuelve los resultados de la consulta.

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>Proveedores de valores

El ejemplo anterior no era específico sobre dónde el valor de la *palabra clave* parámetro era procedentes de. Para indicar esta información, puede usar un atributo de parámetro. En este ejemplo, puede usar el *QueryStringAttribute* clase que está en el *System.Web.ModelBinding* espacio de nombres:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

Esto indica el enlace de modelos para intentar enlazar un valor de la cadena de consulta a la *palabra clave* parámetro en tiempo de ejecución. (Esto puede implicar realizar la conversión de tipos, aunque no es así en este caso.) Si no se puede proporcionar un valor y el tipo es que no aceptan valores NULL, se produce una excepción.

Los orígenes de valores para estos métodos se conocen como proveedores de valores y los atributos de parámetro que indican un proveedor de valor que desea usar se conocen como atributos de proveedor de valor. Formularios Web Forms incluirá proveedores de valores y atributos correspondientes para todos los orígenes habituales de proporcionados por el usuario en una aplicación de formularios Web Forms, como la cadena de consulta, cookies, valores de formulario, controles, estado de vista, estado de sesión y propiedades de perfil. También puede escribir proveedores de valores personalizados.

De forma predeterminada, el nombre del parámetro se utiliza como clave para buscar un valor en la colección de proveedores de valor. En el ejemplo, el código será un valor de cadena de consulta denominado palabra clave (por ejemplo, ~ / default.aspx?keyword=chef). Puede especificar una clave personalizada si se pasa como argumento para el atributo de parámetro. Por ejemplo, para usar el valor de la variable de cadena de consulta denominada preguntas, podría hacer esto:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

Si este método está en el código de la página, los usuarios pueden filtrar los resultados pasando una palabra clave con la cadena de consulta:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

Enlace de modelos lleva a cabo muchas tareas que de lo contrario, tendría código a mano: leer el valor, comprobación de un valor null, cualquier intento de convertir al tipo adecuado, comprobando si la conversión tuvo éxito y por último, con el valor en el consulta. Modelo de enlace de resultados en mucho menos código y en la capacidad de volver a usar la funcionalidad a lo largo de la aplicación.

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>Filtrar por valores de un control

Imagine que desea ampliar el ejemplo que permita al usuario elegir un valor de filtro en una lista desplegable. Agregue la siguiente lista desplegable al código y configúrelo para obtener sus datos de otro método para utilizar el *SelectMethod* propiedad:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

Normalmente también agregaría un *EmptyDataTemplate* elemento a la *GridView* controlar de manera que el control mostrará un mensaje si no se encuentra ningún producto coincidente:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

En el código de la página, agregue que el nuevo selecciona método de la lista desplegable:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

Por último, actualizaremos el *GetProducts* seleccione el método para tomar un parámetro nuevo que contiene el identificador de la categoría seleccionada en la lista desplegable:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

Ahora cuando se ejecuta la página, los usuarios pueden seleccionar una categoría en la lista desplegable y la *GridView* control es volver a enlazados automáticamente para mostrar los datos filtrados. Esto es posible porque el enlace de modelos realiza un seguimiento de los valores de parámetros para los métodos de selección y detecta si algún valor de parámetro ha cambiado después de un postback. Si es así, el enlace de modelos obliga al control de datos asociados a volver a enlazar a los datos.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>Expresiones de enlace de datos codificado en HTML

Puede codificar en HTML ahora el resultado de las expresiones de enlace de datos. Agregue un signo de dos puntos (:) al final de la &lt;% # prefijo que marca la expresión de enlace de datos:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>Validación discreto

Ahora puede configurar los controles de validación integradas para usar JavaScript discreto para la lógica de validación del lado cliente. Esto reduce la cantidad de JavaScript que se representan en línea en el marcado de la página significativamente y reduce el tamaño de página general. Puede configurar el JavaScript discreto para controles de validación en cualquiera de las siguientes maneras:

- Global agregando la siguiente configuración para el  *&lt;appSettings&gt;*  elemento en el archivo Web.config: 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- Global estableciendo el método estático *System.Web.UI.ValidationSettings.UnobtrusiveValidationMode* propiedad *UnobtrusiveValidationMode.WebForms* (normalmente, en la *aplicación \_Iniciar* método en el archivo Global.asax).
- Individualmente para una página estableciendo la nueva *UnobtrusiveValidationMode* propiedad de la *página* clase a *UnobtrusiveValidationMode.WebForms*.

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>Actualizaciones de HTML5

Se realizaron algunas mejoras en formularios Web Forms controles de servidor para aprovechar las nuevas características de HTML5:

- El *TextMode* propiedad de la *cuadro de texto* control se ha actualizado para admitir los nuevos tipos de entrada de HTML5 como *correo electrónico*, *datetime*, y así sucesivamente.
- El *FileUpload* controlar ahora es compatible con cargan de archivos varios de los exploradores que admiten esta característica de HTML5.
- Validador controla ahora compatibilidad con validación HTML5 elementos de entrada.
- Nuevos elementos de HTML5 que tienen atributos que representan una dirección URL ahora admiten runat = "server". Como resultado, puede utilizar las convenciones de ASP.NET en las rutas de acceso de dirección URL, como el ~ (operador) para representar la raíz de la aplicación (por ejemplo, &lt;vídeo runat = "server" src="~/myVideo.wmv" /&gt;).
- El *UpdatePanel* se ha corregido el control para admitir campos de entrada de registro HTML5.

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

ASP.NET MVC 4 Beta estará incluido con Visual Studio 11 Beta. ASP.NET MVC es un marco para el desarrollo de aplicaciones Web de alta de probar y mantener aprovechando el modelo Model-View-Controller (MVC). ASP.NET MVC 4 resulta muy sencillo crear aplicaciones para mobile Web e incluye ASP.NET Web API, lo que ayuda a crear servicios HTTP que pueden llegar a cualquier dispositivo. Para obtener más información, consulte el [notas de la versión de ASP.NET MVC 4](mvc4-release-notes.md).

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>ASP.NET Web Pages 2

Nuevas características incluyen lo siguiente:

- Plantillas de sitios nuevos y actualizados.
- Agregar servidor y realizar una validación de cliente utilizando la *validación* auxiliar.
- La capacidad de registrar scripts mediante un administrador de activos.
- Habilitar los inicios de sesión de Facebook y otros sitios con OAuth y OpenID.
- Agregar mapas mediante la *asigna* auxiliar.
- Ejecutar aplicaciones de páginas Web en paralelo.
- Representar páginas para dispositivos móviles.

Para obtener más información sobre estas características y ejemplos de código de página completa, consulte [características de la parte superior en la versión Beta 2 de páginas Web](https://go.microsoft.com/fwlink/?LinkID=227824).

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Versión Beta de Visual Web Developer 11

Esta sección proporciona información acerca de las mejoras para el desarrollo web en Visual Web Developer 11 Beta y Visual Studio 2012 Release Candidate.

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Proyecto de uso compartido entre Visual Studio 2010 y Visual Studio 2012 Release Candidate (compatibilidad de proyectos)

Hasta que Visual Studio 2012 Release Candidate, abrir un proyecto existente en una versión más reciente de Visual Studio inicia al Asistente para conversión. Esto fuerza una actualización del contenido (activos) de un proyecto y la solución a nuevos formatos que no eran compatibles con versiones anteriores. Por lo tanto, después de la conversión no pudo abrir el proyecto en la versión anterior de Visual Studio.

Muchos clientes nos han dicho que no era el enfoque correcto. En Visual Studio 11 Beta, ahora se admiten para compartir proyectos y soluciones con Visual Studio 2010 SP1. Esto significa que si abre un proyecto de 2010 en Visual Studio 2012 Release Candidate, aún podrá abrir el proyecto en Visual Studio 2010 SP1.

> [!NOTE]
> Algunos tipos de proyectos no puede compartirse entre Visual Studio 2010 SP1 y Visual Studio 2012 Release Candidate. Éstas incluyen algunos proyectos anteriores (como los proyectos de ASP.NET MVC 2) o proyectos para fines especiales (por ejemplo, proyectos de instalación).

Cuando abre un proyecto de Visual Studio 2010 SP1 Web por primera vez en Visual Studio 11 Beta, las propiedades siguientes se agregan al archivo de proyecto:

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

FileUpgradeFlags, UpgradeBackupLocation y OldToolsVersion se usan por el proceso que actualiza el archivo de proyecto. No tienen ningún impacto sobre cómo trabajar con el proyecto en Visual Studio 2010.

VisualStudioVersion es una propiedad nueva utilizada por MSBuild 4.5 que indica la versión de Visual Studio para el proyecto actual. Dado que esta propiedad no existe en MSBuild 4.0 (la versión de MSBuild que usa Visual Studio 2010 SP1), se inserta un valor predeterminado en el archivo de proyecto.

La propiedad VSToolsPath se usa para determinar el archivo .targets correcto para importar desde la ruta de acceso representada por el valor de MSBuildExtensionsPath32.

También hay algunos cambios relacionados a elementos Import. Estos cambios son necesarios para admitir la compatibilidad entre las dos versiones de Visual Studio.

> [!NOTE]
> Si un proyecto se comparte entre Visual Studio 2010 SP1 y Visual Studio 11 Beta en dos equipos diferentes, y si el proyecto incluye una base de datos local en la aplicación\_carpeta de datos, debe asegurarse de que es la versión de SQL Server utilizado por la base de datos instalar en ambos equipos.

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>Cambios de configuración en las plantillas de sitio Web 4.5 de ASP.NET

Los siguientes cambios se realizaron en el valor predeterminado *Web.config* archivo para el sitio que se crean mediante plantillas de sitios Web en Visual Studio 2012 Release Candidate:

- En el `<httpRuntime>` elemento, el `encoderType` atributo ahora se establece de forma predeterminada para utilizar los tipos de AntiXSS que se agregaron a ASP.NET. Para obtener más información, consulte [AntiXSS biblioteca](#_Toc318097382).
- También en el `<httpRuntime>` elemento, el `requestValidationMode` atributo está establecido en "4.5". Esto significa que, de forma predeterminada, la validación de solicitudes está configurada para utilizar la validación diferida de ("perezoso"). Para obtener más información, consulte [características de validación de solicitud de ASP.NET nuevo](#_Toc318097379).
- El `<modules>` elemento de la `<system.webServer>` sección no contiene un `runAllManagedModulesForAllRequests` atributo. (Su valor predeterminado es false). Esto significa que si se usa una versión de IIS 7 que no se ha actualizado a SP1, puede que tenga problemas con el enrutamiento en un sitio nuevo. Para obtener más información, consulte [compatibilidad nativa con en IIS 7 para el enrutamiento de ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine).

Estos cambios no afectan a las aplicaciones existentes. Sin embargo, puede representar una diferencia de comportamiento entre los sitios Web existentes y nuevos sitios Web que se crea para ASP.NET 4.5 utilizando las nuevas plantillas.

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>Compatibilidad nativa con en IIS 7 para el enrutamiento de ASP.NET

Esto no es un cambio a ASP.NET como tal, sino un cambio en las plantillas para los nuevos proyectos de sitios Web que pueden afectar a si está trabajando una versión de IIS 7 que no tenga aplicada la actualización de SP1.

En ASP.NET, puede agregar la siguiente opción de configuración para las aplicaciones con el fin de admitir el enrutamiento:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

Cuando **runAllManagedModulesForAllRequests** es true, una dirección URL como `http://mysite/myapp/home` entra en ASP.NET, aunque no hay ningún *.aspx*, *.mvc*, o extensión similar en el DIRECCIÓN URL.

Realiza una actualización que se realizó en IIS 7 el **runAllManagedModulesForAllRequests** innecesario y es compatible con ASP.NET enrutamiento de forma nativa. (Para obtener información acerca de la actualización, vea el artículo de Microsoft Support [hay una actualización disponible que permite determinados controladores de IIS 7.0 o IIS 7.5 para controlar las solicitudes de direcciones URL no termina con un período](https://support.microsoft.com/kb/980368).)

Si está ejecutando el sitio Web en IIS 7 y si IIS se ha actualizado, no es necesario establecer **runAllManagedModulesForAllRequests** en true. De hecho, si se establece en true no se recomienda, ya que agrega sobrecarga de procesamiento necesario para la solicitud. Si este valor es true, todas las solicitudes, los de incluidos *.htm*, *.jpg*, y otros archivos estáticos, ir a través de la canalización de solicitudes ASP.NET.

Si crea un nuevo sitio Web de ASP.NET 4.5 con las plantillas que se proporcionan en Visual Studio 2012 RC, la configuración para el sitio Web no incluye el **runAllManagedModulesForAllRequests** configuración. Esto significa que, de forma predeterminada el valor es false.

Si, a continuación, se ejecuta el sitio Web en Windows 7 sin SP1 instalado, IIS 7 no incluirá la actualización necesaria. En consecuencia, el enrutamiento no funcionará y verá errores. Si tiene un problema donde el enrutamiento no funciona, puede hacer lo siguiente:

- Actualice Windows 7 con SP1, que se agregará la actualización a IIS 7.
- Instale la actualización que se describe en el artículo de Microsoft Support indicado anteriormente.
- Establecer **runAllManagedModulesForAllRequests** en true en el archivo Web.config de ese sitio Web. Tenga en cuenta que esto se agrega cierta sobrecarga a las solicitudes.

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>Editor de HTML

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>Tareas inteligentes

En la vista de diseño, las propiedades complejas de controles de servidor a menudo tienen asociados cuadros de diálogo y asistentes que resulte sencillo establecerlos. Por ejemplo, puede usar un cuadro de diálogo especial para agregar un origen de datos para un *repetidor* controlar o agregar columnas a una *GridView* control.

Sin embargo, este tipo de Ayuda de la interfaz de usuario para propiedades complejas no ha sido disponible en la vista de origen. Por lo tanto, en Visual Studio 11 se presentan tareas inteligente para la vista del origen. Tareas inteligentes son accesos directos en contexto para las características usadas habitualmente en los editores de C# y Visual Basic.

Para los controles de formularios Web Forms de ASP.NET, tareas inteligentes aparecen en las etiquetas de servidor como un glifo pequeño cuando el punto de inserción está dentro del elemento:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

La tarea inteligentes se expande cuando haga clic en el glifo o presione CTRL +. (punto), al igual que en los editores de código. A continuación, muestra los métodos abreviados que son similares a las tareas inteligentes en la vista Diseño.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

Por ejemplo, la tarea inteligente en la ilustración anterior muestra las opciones de tareas de GridView. Si decide editar columnas, se muestra el cuadro de diálogo siguiente:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

Rellenar el cuadro de diálogo establece las mismas propiedades puede establecer en la vista Diseño. Al hacer clic en Aceptar, el marcado para el control se actualiza con la nueva configuración:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>Compatibilidad con ARIA WAI

Escribir sitios Web accesibles está adquiriendo cada vez más importancia. El [accesibilidad WAI ARIA estándar](http://www.w3.org/WAI/intro/aria) define cómo los desarrolladores deben escribir sitios Web accesibles. Este estándar ahora es totalmente compatible en Visual Studio.

Por ejemplo, el *rol* atributo ahora tiene todas las características IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

El estándar WAI ARIA también introduce los atributos que tienen el prefijo *aria -* que le permiten agregar semántica en un documento de HTML5. Visual Studio también es totalmente compatible con estas *aria -* atributos:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>Nuevos fragmentos de código de HTML5

Para que sea más rápida y sencilla escribir el marcado de HTML5 utilizada, Visual Studio incluye un número de fragmentos de código. Un ejemplo es el fragmento de vídeo:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

Para invocar el fragmento de código, presione la tecla Tab dos veces cuando el elemento está seleccionado en IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

Esto genera un fragmento de código que se puede personalizar.

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>Extraer a control de usuario

En las páginas web grandes, puede ser una buena idea para mover las piezas individuales en controles de usuario. Esta forma de refactorización puede ayudar a aumentar la legibilidad de la página y puede simplificar la estructura de la página.

Para facilitar esta tarea, cuando edita páginas de formularios Web Forms en la vista de origen, puede seleccionar ahora el texto en una página, haga clic en y, a continuación, elija Extraer a Control de usuario:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>IntelliSense para fragmentos de código en atributos

Visual Studio siempre ha proporcionado IntelliSense para fragmentos de código del lado servidor en cualquier página o el control. Ahora Visual Studio incluye IntelliSense para fragmentos de código en también los atributos HTML.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

Esto resulta más sencillo crear expresiones de enlace de datos:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>Cambio de nombre automático de la coincidencia de etiqueta al cambiar el nombre de una etiqueta inicial o final

Si cambia el nombre de un elemento HTML (por ejemplo, cambiar un *div* etiqueta sea un *encabezado* etiqueta), la apertura o una etiqueta de cierre correspondiente también cambia en tiempo real.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

Esto ayuda a evitar el error donde se olvida de cambiar una etiqueta de cierre o uno erróneo.

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>Generación de controlador de eventos

Ahora Visual Studio incorpora características de vista del origen para ayudarle a escribir controladores de eventos y enlazarlas manualmente. Si va a modificar un nombre de evento en la vista código fuente, IntelliSense muestra &lt;crear un nuevo evento&gt;, que creará un controlador de eventos en código de la página que tiene la firma correcta:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

De forma predeterminada, el controlador de eventos usará el identificador del control para el nombre del método de control de eventos:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

El controlador de eventos resultante tendrá un aspecto similar al siguiente (en este caso, en C#):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>Sangría automática

Al presionar ENTRAR dentro de un elemento HTML vacío, el editor colocará el punto de inserción en el lugar correcto:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

Si presiona ENTRAR en esta ubicación, la etiqueta de cierre se desplaza hacia abajo y se aplica una sangría para que coincida con la etiqueta de apertura. También se aplica sangría al punto de inserción:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>Reducir automáticamente la finalización de instrucciones

La lista de IntelliSense en Visual Studio ahora filtros en función de lo que escribe para que muestre solo es pertinentes opciones:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

IntelliSense también filtros basados en el caso de título de las palabras individuales en la lista de IntelliSense. Por ejemplo, si escribe "dl", se muestran dl y asp: DataList:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

Esta característica resulta más rápido para obtener la finalización de instrucciones para los elementos conocidos.

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>editor de JavaScript

El editor de JavaScript en Visual Studio 2012 Release Candidate es completamente nuevo y mejora considerablemente la experiencia de trabajar con el código JavaScript en Visual Studio.

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>Esquematización de código

Regiones de esquematización ahora se crean automáticamente para todas las funciones, lo que le permite contraer partes del archivo que no son pertinentes para el foco actual.

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>Coincidencia de llaves

Cuando se coloca el punto de inserción en una llave de cierre o de apertura, el editor resalta los que coincide con uno.

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>Ir a definición

El comando para ir a definición permite saltar al origen de una función o variable.

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>Soporte técnico de ECMAScript5

El editor admite las API y la nueva sintaxis en ECMAScript5, la versión más reciente del estándar que describe el lenguaje de JavaScript.

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>IntelliSense de DOM

Se ha mejorado IntelliSense para las API de DOM, con compatibilidad para muchas nuevas API de HTML5 incluidos *querySelector*, almacenamiento DOM, mensajería entre documentos y *lienzo*. IntelliSense de DOM ahora está controlada por un único archivo de JavaScript simple, en lugar de una definición de la biblioteca de tipo nativo. Esto hace fácil ampliar o reemplazar.

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>Sobrecargas de firma VSDOC

Comentarios detallados de IntelliSense ahora se pueden declarar para independiente de las sobrecargas de las funciones de JavaScript mediante los nuevos  *&lt;firma&gt;*  elemento, tal como se muestra en este ejemplo:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>Referencias implícitas

Ahora puede agregar archivos de JavaScript a una lista central que implícitamente incluirán en la lista de archivos que cualquier determinadas referencias de archivo o bloque de JavaScript, lo que significa que obtendrá IntelliSense para su contenido. Por ejemplo, puede agregar archivos de jQuery a la lista de archivos central, y obtendrá IntelliSense para las funciones de jQuery en cualquier bloque de JavaScript del archivo, si se ha hecho referencia, explícitamente (con / / / &lt;referencia /&gt;) o no.

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>Editor CSS

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>Reducir automáticamente la finalización de instrucciones

La lista de IntelliSense para los valores admitidos por el esquema seleccionado y filtros CSS ahora basados en las propiedades CSS.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

IntelliSense también admite búsquedas mayúsculas de título:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>Aplicación de sangría jerárquica

El editor de CSS usa sangría para mostrar reglas jerárquicas, que proporciona una visión general de cómo están organizadas lógicamente las reglas en cascada. En el ejemplo siguiente, la #list un selector es un elemento secundario en cascada de lista y, por tanto, se aplica una sangría.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

En el ejemplo siguiente se muestra la herencia más compleja:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

La sangría de una regla viene determinada por las reglas de su primario. Sangría jerárquica está habilitada de forma predeterminada, pero se puede deshabilitar el cuadro de diálogo Opciones (herramientas, opciones de la barra de menús):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>Soporte técnico de ataques de CSS

Análisis de cientos de archivos CSS reales muestra que piratas informáticos CSS son muy comunes, y ahora Visual Studio es compatible con las más usadas. Esta compatibilidad incluye IntelliSense y la validación de la estrella (\*) y caracteres de subrayado (\_) piratas informáticos de propiedad:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

Ataques de selector típico también son compatibles para que sangría jerárquica se mantiene incluso cuando se aplican. Un hack de selector típico utilizado para Internet Explorer 7 de destino es anteponer un selector con  *\*: primera primario-secundario + html*. Uso de esa regla mantendrá la sangría jerárquica:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>Los esquemas específicos de proveedor (- moz - webkit-)

CSS3 presenta muchas propiedades que se han implementado por exploradores diferentes en momentos diferentes. Anteriormente se forzaban a los desarrolladores de código para los exploradores específicos mediante la sintaxis específica del proveedor. Estas propiedades específicas del explorador se incluyen ahora en IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>Compatibilidad con comentario y eliminar el comentario

Ahora puede comentar y elimine las reglas de CSS mediante los mismos métodos abreviados que se utilizan en el editor de código (Ctrl + K, C para comentar y Ctrl + K, que para quitar el comentario).

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>Selector de colores

En versiones anteriores de Visual Studio, IntelliSense para los atributos relacionados con el color consistió en una lista desplegable de valores de color con nombre. Esa lista se ha reemplazado por un selector de colores completa.

Al especificar un valor de color, el selector de colores se muestra automáticamente y presenta una lista de colores usadas anteriormente seguido de una paleta de colores predeterminada. Puede seleccionar un color mediante el mouse o el teclado.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

La lista se puede expandir en un selector de colores completa. El selector de le permite controlar el canal alfa convirtiendo automáticamente cualquier color en RGBA cuando se mueve el control deslizante de opacidad:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>Fragmentos de código

Fragmentos de código en el editor de CSS que sea más fácil y rápido para crear estilos de varios exploradores. Muchas propiedades de CSS3 que requieren una configuración específica del explorador ahora se han revertido en fragmentos de código.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

Fragmentos de código CSS admiten escenarios avanzados (por ejemplo, las consultas de medios de CSS3) escribiendo el símbolo de arroba (@), que muestra la lista de IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

Cuando se selecciona @media valor y presione la tecla Tab, el editor de CSS inserta el fragmento de código siguiente:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

Al igual que con los fragmentos de código, puede crear sus propios fragmentos de código CSS.

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>Regiones personalizadas

Con el nombre de las regiones de código, que ya están disponibles en el editor de código, ahora están disponibles para su edición de CSS. Esto le permite agrupar con facilidad los bloques de estilo relacionados.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

Cuando se contrae una región muestra el nombre de la región:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Inspector de página

Inspector de página es una herramienta que presenta una página web (HTML, formularios Web Forms, ASP.NET MVC o páginas Web) en el IDE de Visual Studio y le permite examinar el código fuente y el resultado. Para las páginas ASP.NET, Inspector de página le permite determinar qué código del lado servidor ha generado el marcado HTML que se representa en el explorador.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Para obtener más información acerca de Inspector de página, vea los siguientes tutoriales:

- Con el Inspector de página en [MVC de ASP.NET](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- Con el Inspector de página en [de ASP.NET Web Forms](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>Publicación

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>Publicar los perfiles

En Visual Studio 2010, información de publicación para los proyectos de aplicación Web no se almacena en el control de versiones y no está diseñado para compartir con otros usuarios. En Visual Studio 2012 Release Candidate, se cambió el formato del perfil de publicación. Se ha realizado un artefacto de equipo, y ahora es fácil aprovechar las ventajas de compilaciones que se basa en MSBuild. Información de configuración de compilación está en el cuadro de diálogo de publicación para que se pueden cambiar fácilmente entre las configuraciones de compilación antes de la publicación.

Publicar perfiles se almacenan en la carpeta PublishProfiles. Depende de la ubicación de la carpeta de lenguaje de programación que esté utilizando:

- C#: Properties\PublishProfiles
- Visual Basic: Mi Project\PublishProfiles

Cada perfil es un archivo de MSBuild. Durante la publicación, este archivo se importa en el archivo del proyecto MSBuild. En Visual Studio 2010, si desea realizar cambios en el proceso de publicación o el paquete, tendrá que coloque las personalizaciones en un archivo denominado **ProjectName**. wpp.targets. Esto se sigue admitiendo, pero ahora puede colocar sus personalizaciones en el perfil de publicación propio. De este modo, las personalizaciones se usará únicamente para dicho perfil.

Ahora puede utilizar publicar perfiles de MSBuild. Para ello, use el siguiente comando al compilar el proyecto:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

El valor de project.csproj es la ruta de acceso del proyecto y ProfileName es el nombre del perfil que desea publicar. O bien, en lugar de pasar el nombre del perfil para la *PublishProfile* propiedad, se puede pasar en la ruta de acceso completa para el perfil de publicación.

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>Mezclar y precompilación de ASP.NET

Para los proyectos de aplicación Web, Visual Studio 2012 Release Candidate agrega una opción en la página de propiedades Empaquetar/Publicar Web que permite precompilar y combinar el contenido de su sitio cuando se publica o el paquete del proyecto. Para ver estas opciones, haga clic en el proyecto en el Explorador de soluciones, elija Propiedades y, a continuación, elija la página de propiedades Empaquetar/Publicar Web. La siguiente ilustración muestra el Precompile esta aplicación antes de la opción de publicación.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

Cuando se selecciona esta opción, Visual Studio precompila la aplicación siempre que publique o el paquete de la aplicación web. Si desea controlar cómo precompilar el sitio o cómo se combinan los ensamblados, haga clic en el botón Avanzado para configurar estas opciones.

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>IIS Express

El servidor web predeterminado para los proyectos de prueba web en Visual Studio ahora es IIS Express. El servidor de desarrollo de Visual Studio sigue siendo una opción de servidor web local durante el desarrollo, pero IIS Express es ahora el servidor recomendado. La experiencia de uso de IIS Express en Visual Studio 11 Beta es muy similar a usar en Visual Studio 2010 SP1.

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>Declinación de responsabilidades

Este documento es preliminar y puede cambiar considerablemente antes de la versión comercial final del software que se describe en él.

La información que incluye este documento representa el punto de vista actual de Microsoft Corporation sobre las cuestiones descritas en la fecha de la publicación. Debido a que Microsoft debe responder a las condiciones de mercado cambiantes, no se debe interpretar como un compromiso por parte de Microsoft y Microsoft no puede garantizar la precisión de la información presentada después de la fecha de publicación.

Estas notas del producto solo tienen fines informativos. MICROSOFT NO OTORGA NINGUNA GARANTÍA, EXPRESA, IMPLÍCITA O ESTUTARIAS, EN LA INFORMACIÓN DE ESTE DOCUMENTO.

Es responsabilidad del usuario el cumplimiento de toda la legislación aplicable en materia de copyright. Sin limitación de los derechos protegidos por copyright, ninguna parte del presente documento podrá ser reproducida, almacenada o introducida en un sistema de recuperación, o bien transmitida en ninguna forma o medio (electrónico, mecánico, mediante fotocopia o grabación, etc.), ni con ningún propósito, sin la autorización expresa y por escrito de Microsoft Corporation.

Microsoft puede ser titular de patentes, solicitudes de patentes, marcas, derechos de autor u otros derechos de propiedad intelectual sobre los contenidos de este documento. Este documento no otorga ninguna licencia sobre estas patentes, marcas, derechos de autor u otros derechos de propiedad intelectual, a menos que se indique expresamente en un contrato escrito de licencia de Microsoft.

A menos que se indique lo contrario, los nombres de las compañías, organizaciones, productos, nombres de dominio, direcciones de correo electrónico, logotipos, personas, lugares y acontecimientos aquí mencionados son ficticio y ninguna asociación con ninguna compañía real, organización, producto, nombre de dominio, correo electrónico dirección, logotipo, persona, lugar o evento pretende ni debe deducirse.

© 2012 Microsoft Corporation. Todos los derechos reservados.

Microsoft y Windows son marcas registradas o marcas comerciales de Microsoft Corporation en Estados Unidos y en otros países.

Los nombres de las compañías y productos reales mencionados en el presente documento pueden ser marcas comerciales de sus respectivos propietarios.
