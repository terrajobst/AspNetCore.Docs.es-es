---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: "Este documento describe la versión de ASP.NET MVC 4."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: bea6f6112388290a2c6b5ed267626ba28fc36671
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Este documento describe la versión de ASP.NET MVC 4.


- [Notas sobre la instalación](#_Toc303253802)
- [Documentación](#_Toc303253803)
- [Support](#_Toc303253804)
- [Requisitos de software](#_Toc303253805)
- [Nuevas características de ASP.NET MVC 4](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197) (Más información sobre ASP.NET Web API)
    - [Mejoras en las plantillas de proyecto predeterminadas](#_Toc303253808)
    - [Plantilla de proyecto móvil](#_Toc303253809)
    - [Modos de presentación](#_Toc303253810)
    - [jQuery Mobile, el modificador de vista e invalidar el explorador](#_Toc303253811)
    - [Asistencia para las tareas para los controladores asincrónicos](#_Toc303253813)
    - [SDK de Azure](#_Toc303253814)
    - [Migraciones de base de datos](#_Toc303253818)
    - [Plantilla de proyecto vacía](#_Toc303253819)
    - [Agregar controlador a cualquier carpeta del proyecto](#_Toc303253820)
    - [Unión y minificación](#_Toc303253821)
    - [Habilitar los inicios de sesión de Facebook y otros sitios con OAuth y OpenID](#_Toc303253822)
- [Actualizar un proyecto de ASP.NET MVC 3 a ASP.NET MVC 4](#_Toc303253806)
- [Cambios de candidato de versión comercial de ASP.NET MVC 4](#_Toc303253817)
- [Problemas conocidos y los cambios recientes](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Notas sobre la instalación

ASP.NET MVC 4 para Visual Studio 2010 puede instalarse desde el [página principal de ASP.NET MVC 4](../mvc/mvc4.md) mediante el instalador de plataforma Web.

Se recomienda desinstalar las vistas previas de previamente instaladas de ASP.NET MVC 4 antes de instalar ASP.NET MVC 4. Puede actualizar la versión Beta de ASP.NET MVC 4 y Release Candidate a ASP.NET MVC 4 sin desinstalar.

Esta versión no es compatible con las versiones preliminares de .NET Framework 4.5. Por separado, debe actualizar el cualquier versión de vista previa instalada de .NET Framework 4.5 a la versión final antes de instalar ASP.NET MVC 4.

ASP.NET MVC 4 pueden instalarse y ejecutarse en paralelo con ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Documentación

Documentación para ASP.NET MVC está disponible en el sitio Web MSDN en la dirección URL siguiente:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Tutoriales y otra información sobre ASP.NET MVC están disponibles en la página de MVC 4 del sitio Web ASP.NET ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Compatibilidad

ASP.NET MVC 4 es totalmente compatible. Si tiene alguna pregunta sobre cómo trabajar con esta versión se puede registrarlas en el foro de ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), donde los miembros de la Comunidad de ASP.NET son normalmente pueden ofrecer soporte técnico informal.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Requisitos de software

Los componentes de ASP.NET MVC 4 para Visual Studio requieren PowerShell 2.0 y Visual Studio 2010 con Service Pack 1 o Visual Web Developer Express 2010 con Service Pack 1.

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>Nuevas características de ASP.NET MVC 4

Esta sección describen las características que se han introducido en la versión de ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET MVC 4 incluye ASP.NET Web API, un nuevo marco para crear servicios HTTP que pueden alcanzar una amplia gama de clientes, incluidos los exploradores y dispositivos móviles. ASP.NET Web API es también una plataforma ideal para compilar servicios RESTful.

ASP.NET Web API incluye compatibilidad para las siguientes características:

- **Modelo de programación HTTP moderna:** directamente tener acceso y manipular las solicitudes HTTP y las respuestas en las API Web con un nuevo modelo de objeto HTTP fuertemente tipado. El mismo modelo y la canalización HTTP de programación está simétricamente disponible en el cliente a través del nuevo *HttpClient* tipo.
- **Compatibilidad completa para las rutas:** ASP.NET Web API es compatible con el conjunto completo de capacidades de ruta de enrutamiento de ASP.NET, incluidos los parámetros de ruta y las restricciones. Además, usar convenciones simples para asignar acciones a los métodos HTTP.
- **Negociación de contenido:** el cliente y el servidor pueden trabajar juntos para determinar el formato correcto para los datos que se devuelven desde una API web. ASP.NET Web API proporciona compatibilidad predeterminada para XML, JSON, y formatos codificados de dirección URL de formulario y se puede ampliar esta compatibilidad agregando sus propia formateadores, o incluso reemplazar la estrategia de negociación de contenido de forma predeterminada.
- **Enlace y validación de modelos:** enlazadores de modelos proporcionan una manera sencilla de extraer datos de distintas partes de una solicitud HTTP y convertir las partes de mensaje en objetos de .NET que se pueden usar con las acciones de API Web. También se realiza la validación de parámetros de acción en función de las anotaciones de datos.
- **Filtros:** ASP.NET Web API es compatible con filtros incluido filtros bien conocidos, como el *[Authorize]* atributo. Puede crear y conectar sus propios filtros para las acciones, autorización y control de excepciones.
- **La composición de consulta:** Use la *[Queryable]* atributo de filtro en una acción que devuelve *IQueryable* para habilitar la compatibilidad para consultar la API web a través de las convenciones de la consulta de OData.
- **Mejora la capacidad de prueba:** en lugar de establecer detalles HTTP en objetos de contexto estático, web API que funcione con instancias de acciones *HttpRequestMessage* y *HttpResponseMessage*. Crear un proyecto de prueba unitaria junto con su proyecto de Web API para empezar a trabajar rápidamente escribir pruebas unitarias para la funcionalidad de la API Web.
- **Configuración basada en código:** configuración de ASP.NET Web API se realiza únicamente a través de código, dejando la configuración de limpieza de archivos. Usar el patrón de localizador de servicio proporcionado para configurar puntos de extensibilidad.
- **Compatibilidad mejorada para los contenedores de inversión de Control (IoC):** API Web de ASP.NET proporciona una excelente compatibilidad para los contenedores de IoC a través de una abstracción de resolución de dependencia mejorada
- **Autohospedaje:** las API Web se pueden hospedar en su propio proceso además de IIS mientras se sigue usando toda la funcionalidad de rutas y otras características de API Web.
- **Crear ayuda personalizada y probar las páginas:** ahora puede fácilmente Ayuda personalizada de compilar y probar las páginas de la API de web con el nuevo *IApiExplorer* servicio para obtener una descripción completa en tiempo de ejecución de la API web.
- **Supervisión y diagnóstico:** ASP.NET Web API ahora proporciona la infraestructura de seguimiento ligero que facilita la integración con soluciones de registro existente como System.Diagnostics, ETW y marcos de registro de otros fabricantes. Puede habilitar el seguimiento proporcionando un *ITraceWriter* implementación y lo agrega a la configuración de web API.
- **Vincular generación:** Use ASP.NET Web API *UrlHelper* para generar vínculos a recursos relacionados en la misma aplicación.
- **Plantilla de proyecto Web API:** seleccione el nuevo formato de proyecto de Web API el Asistente nuevo proyecto MVC 4 para obtener rápidamente a trabajar con ASP.NET Web API.
- **Scaffolding:** Use la **Agregar controlador** cuadro de diálogo para aplicar la técnica scaffolding un controlador de web API basado en un Entity Framework rápidamente según el tipo de modelo.

Para obtener más detalles sobre la API Web de ASP.NET, visitan [https://www.asp.net/web-api](../web-api/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Mejoras en las plantillas de proyecto predeterminadas

Se ha actualizado la plantilla que se usa para crear nuevos proyectos de ASP.NET MVC 4 para crear un sitio Web de aspecto más moderno:

![](mvc4-release-notes/_static/image1.png)

Además de mejoras cosméticas, mejoró la funcionalidad de la nueva plantilla. La plantilla emplea una técnica denominada representación adaptable para que se muestren correctamente en los exploradores de escritorio y exploradores móviles sin ninguna personalización.

![](mvc4-release-notes/_static/image2.png)

Para ver la representación adaptable en acción, puede usar un emulador de dispositivos móvil o intente cambiar el tamaño de la ventana del explorador de escritorio para que sea menor. Cuando la ventana del explorador obtiene lo suficientemente pequeña, cambiará el diseño de la página.

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Plantilla de proyecto móvil

Si está empezando a un nuevo proyecto y desea crear un sitio específicamente para dispositivos móviles y exploradores de Tablet PC, puede usar la nueva plantilla de proyecto de aplicación móvil. Esto se basa en jQuery Mobile, una biblioteca de código abierto para la creación de la interfaz de usuario táctil optimizada:

![](mvc4-release-notes/_static/image3.png)

Esta plantilla contiene la misma estructura de aplicación que la plantilla de aplicación de Internet (y el código del controlador es prácticamente idéntico), pero es un estilo mediante jQuery Mobile que aparezcan correctamente y se comporten correctamente en dispositivos móviles basados en la entrada táctil. Para obtener más información sobre la estructura y el estilo de la interfaz de usuario móvil, consulte el [sitio Web de proyecto móvil de jQuery](http://jquerymobile.com/).

Si ya tiene un sitio orientada a servicios de escritorio que le interese agregar vistas optimizadas para el móvil a, o si desea crear un único sitio que actúa de vistas con estilo de manera diferente a los exploradores de escritorio y mobile, puede usar la nueva característica de modos de presentación. (Vea la sección siguiente).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Modos de presentación

La nueva característica de modos de presentación permite que una aplicación seleccionar vistas dependiendo del explorador que está realizando la solicitud. Por ejemplo, si un explorador de escritorio, solicita la página de inicio, la aplicación podría usar la plantilla Views\Home\Index.cshtml. Si un explorador móvil solicita la página de inicio, la aplicación podría devolver la plantilla Views\Home\Index.mobile.cshtml.

También se pueden reemplazar diseños y parciales para tipos de explorador determinado. Por ejemplo:

- Si la carpeta Views\Shared contiene tanto el \_Layout.cshtml y \_Layout.mobile.cshtml plantillas, de forma predeterminada, la aplicación usará \_Layout.mobile.cshtml durante las solicitudes de exploradores móviles y \_Layout.cshtml durante otras solicitudes.
- Si una carpeta contiene \_MyPartial.cshtml y \_MyPartial.mobile.cshtml, la instrucción @Html.Partial("\_MyPartial") se representarán \_MyPartial.mobile.cshtml durante las solicitudes de mobile exploradores, y \_MyPartial.cshtml durante otras solicitudes.

Si desea crear más determinadas vistas, diseños o vistas parciales para otros dispositivos, puede registrar un nuevo *DefaultDisplayMode* instancia para especificar el nombre que se busca cuando una solicitud cumple las condiciones específicas. Por ejemplo, podría agregar el código siguiente a la *aplicación\_iniciar* método en el archivo Global.asax para registrar la cadena "iPhone" como un modo de presentación que se aplica cuando el Explorador de iPhone de Apple realiza una solicitud:

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

Después de ejecuta este código, cuando un explorador de iPhone de Apple realiza una solicitud, la aplicación utilizará la Views\Shared\\_Layout.iPhone.cshtml diseño (si existe). Para obtener más información sobre el modo de presentación, consulte [características de ASP.NET MVC 4 Mobile](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md). Deben instalar las aplicaciones que usan DisplayModeProvider el [DisplayModes fijo](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) paquete NuGet. El [ASP.NET otoño de 2012 Update](https://go.microsoft.com/fwlink/?LinkID=271322) incluye la [DisplayModes fijo](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) paquete de NuGet en las nuevas plantillas de proyecto. Vea [Fixedd de error almacenamiento en caché de ASP.NET MVC 4 Mobile](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) para obtener más información sobre la corrección.

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>Características de Mobile y jQuery Mobile

Para obtener información sobre la creación de aplicaciones móviles con ASP.NET MVC 4 mediante jQuery Mobile, consulte el tutorial [características de ASP.NET MVC 4 Mobile](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md).

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Asistencia para las tareas para los controladores asincrónicos

Ahora puede escribir métodos de acción asincrónicos como una sola métodos que devuelven un objeto del tipo *tarea* o *tarea&lt;ActionResult&gt;*.

 Para obtener más información, consulte [mediante los métodos asincrónicos en ASP.NET MVC 4](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>SDK de Azure

ASP.NET MVC 4 es compatible con las versiones 1.6 y versiones más recientes de Windows Azure SDK.

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>Migraciones de base de datos

Proyectos de ASP.NET MVC 4 ahora incluyen Entity Framework 5. Una de las excepcionales características de Entity Framework 5 es compatible con migraciones de base de datos. Esta característica permite desarrollar fácilmente su esquema de base de datos mediante una migración centrada en el código al tiempo que conserva los datos de la base de datos. Para obtener más información acerca de las migraciones de base de datos, vea [agregando un nuevo campo en el modelo de película como tabla](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md) en el [Introducción a ASP.NET MVC 4 tutorial](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>Plantilla de proyecto vacía

La plantilla proyecto vacío de MVC está ahora realmente vacía para que pueda empezar desde una pizarra limpia por completo. La versión anterior de la plantilla proyecto vacío de servidor se ha cambiado al modo básico.

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>Agregar controlador a cualquier carpeta del proyecto

Ahora puede haga clic y seleccione **Agregar controlador** desde cualquier carpeta del proyecto de MVC. Esto proporciona más flexibilidad para organizar los controladores, pero desee, incluido mantener los controladores MVC y API Web en carpetas independientes.

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>Agrupar y Minificar

El marco de trabajo de agrupación y minificación permite reducir el número de solicitudes HTTP que una página Web se debe realizar mediante la combinación de archivos individuales en un único archivo integrado para secuencias de comandos y CSS. A continuación, puede reducir el tamaño total de las solicitudes por minificar el contenido de la agrupación. Minificar puede incluir actividades, como eliminar espacio en blanco para acortar los nombres de variable para contraer incluso selectores CSS basándose en su semántica. Agrupaciones se declaran y se configura en código y se fácilmente al que hace referencia en las vistas a través de los métodos auxiliares que pueden generar en un único vínculo a la agrupación o, al depurar, varios vínculos al contenido individual de la agrupación. Para obtener más información, consulte [unión y Minificación](../mvc/overview/performance/bundling-and-minification.md).

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Habilitar los inicios de sesión de Facebook y otros sitios con OAuth y OpenID

Las plantillas predeterminadas de la plantilla de proyecto de ASP.NET MVC 4 Internet ahora incluye compatibilidad con inicio de sesión de OAuth y OpenID mediante la biblioteca de DotNetOpenAuth. Para obtener información sobre cómo configurar un proveedor de OAuth u OpenID, consulte [OAuth/OpenID admiten formularios Web Forms, MVC y las páginas Web](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx) y [OAuth y OpenID característica documentación en ASP.NET Web Pages](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup).

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Actualizar un proyecto de ASP.NET MVC 3 a ASP.NET MVC 4

ASP.NET MVC 4 se puede instalar paralelo con ASP.NET MVC 3 en el mismo equipo, lo cual ofrece flexibilidad de elegir cuándo se debe actualizar una aplicación de ASP.NET MVC 3 a ASP.NET MVC 4.

La manera más sencilla de actualización es para crear un nuevo proyecto de ASP.NET MVC 4 y copie todas las vistas, controladores, código y contenido de archivos desde el proyecto de MVC 3 existente al nuevo proyecto y, a continuación, actualizar el ensamblado hace referencia en el nuevo proyecto para que coincida con cualquier plantilla de MVC no en incluidos assembiles que usa. Si ha realizado cambios en el archivo Web.config en el proyecto de MVC 3, también debe combinar los cambios en el archivo Web.config en el proyecto de MVC 4.

Para actualizar manualmente una aplicación existente de ASP.NET MVC 3 a la versión 4, haga lo siguiente:

1. En el archivo Web.config de todos los archivos en el proyecto (no hay uno en la raíz del proyecto, uno en la carpeta Views y uno en la carpeta Views de cada área del proyecto), reemplace todas las instancias del texto siguiente (Nota: System.Web.WebPages, Version = 1.0.0.0 no se encuentra en proyectos creados con Visual Studio 2012): 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    con el siguiente texto correspondiente:

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. En el archivo raíz Web.config, actualice el *webPages:Version* elemento a "2.0.0.0" y agregue un nuevo *PreserveLoginUrl* clave que tiene el valor "true": 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. En el Explorador de soluciones, haga doble clic en las referencias y seleccione Administrar paquetes de NuGet. En el panel izquierdo, seleccione **origen de paquete oficial de Online\NuGet**, a continuación, actualice los siguientes:

    - ASP.NET MVC 4
    - (Opcional) jQuery, jQuery validación y jQuery UI
    - (Opcional) Entity Framework
    - (Opcional) Modernizr
4. En el Explorador de soluciones, haga clic en el nombre del proyecto y, a continuación, seleccione Descargar proyecto. A continuación, haga clic en el nombre nuevo y seleccione Editar *ProjectName*.csproj.
5. Busque la *ProjectTypeGuids* elemento y reemplace {E53F8FEA-EAE0-44A6-8774-FFD645390401} con {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Guardar los cambios, cierre el archivo de proyecto (.csproj) que estaba editando, haga clic en el proyecto y, a continuación, seleccione recargar el proyecto.
7. Si el proyecto hace referencia a las bibliotecas de terceros que están compiladas con versiones anteriores de ASP.NET MVC, abra el archivo raíz Web.config y agregue las siguientes *bindingRedirect* elementos en la  *configuración* sección: 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>Cambios de candidato de versión comercial de ASP.NET MVC 4

Las notas de la versión de ASP.NET MVC 4 Release Candidate pueden encontrarse aquí:

A continuación se resumen los cambios más importantes de ASP.NET MVC 4 Release Candidate en esta versión:

- **Por cada configuración de controlador:** controladores de ASP.NET Web API se pueden atribuir con un atributo personalizado que implementa *IControllerConfiguration* para configurar sus propias formateadores, selector de acción y los enlazadores de parámetro . El *HttpControllerConfigurationAttribute* se ha quitado.
- **Por controladores de mensajes de ruta:** ahora puede especificar el controlador de mensaje final de la cadena de solicitud en una determinada ruta. Esto habilita la compatibilidad con marcos de conducción junto a utilizar el enrutamiento para enviar a su propia (no -*IHttpController*) los puntos de conexión.
- **Las notificaciones de progreso:** el *ProgressMessageHandler* genera notificaciones de progreso para entidades de solicitud que se va a cargar y entidades de respuesta que se va a descargar. Con este controlador es posible realizar un seguimiento de hasta qué punto se carga un cuerpo de solicitud o descargar un cuerpo de respuesta.
- **Insertar contenido:** el *PushStreamContent* clase permite escenarios donde un productor de datos desea escribir directamente en la solicitud o respuesta (forma sincrónica o asincrónica) mediante una secuencia. Cuando el *PushStreamContent* está listo para aceptar los datos que llama a un delegado de acción con el flujo de salida. El programador, a continuación, puede escribir en la secuencia para siempre que es necesario y cierre se ha completado la secuencia al escribir. El *PushStreamContent* detecta el cierre de la secuencia y se completa asincrónica subyacente *tarea* para escribir el contenido.
- **Creación de respuestas de error:** Use la *HttpError* tipo para representar información de error como errores de validación y las excepciones sigue respetando al mismo tiempo de forma coherente la *IncludeErrorDetailPolicy* . Use la nueva *CreateErrorResponse* métodos de extensión para crear fácilmente las respuestas de error con *HttpError* como contenido. El *HttpError* contenido está totalmente contenido negociado.
- **MediaRangeMapping quitado:** intervalos de tipos de medios se pueden controlar mediante el negociador de contenido de forma predeterminada.
- **Enlace de parámetro predeterminado para los parámetros de tipo simple es ahora [FromUri]:** en versiones anteriores de ASP.NET Web API, el enlace de parámetros predeterminados para el enlace de modelos de uso de parámetros de tipo simple. El enlace de parámetro predeterminado para los parámetros de tipo simple es ahora *[FromUri]*.
- **Selección de acción respeta los parámetros necesarios:** selección de acción en ASP.NET Web API ahora seleccionará solo una acción, si se proporcionan todos los parámetros necesarios que proceden de la dirección URI. Un parámetro puede especificarse como opcional, proporcionando un valor predeterminado para el argumento de la firma del método de acción.
- **Personalizar los enlaces de parámetro HTTP:** usar la *ParameterBindingAttribute* para personalizar el enlace de parámetro para un parámetro de acción específica o usar el *ParameterBindingRules* en el *HttpConfiguration* personalizar los enlaces de parámetro más amplia.
- **Mejoras de elemento MediaTypeFormatter:** formateadores ahora tienen acceso a toda la *HttpContent* instancia.
- **Selección de directiva de almacenamiento en búfer de host:** implemente y configure el *IHostBufferPolicySelector* servicio en ASP.NET Web API para permitir a los hosts determinar la directiva cuando el almacenamiento en búfer es que se usará.
- **Tener acceso a los certificados de cliente de una manera independiente del host:** Use la *GetClientCertificate* método de extensión para obtener el certificado de cliente desde el mensaje de solicitud.
- **Extensibilidad de negociación de contenido:** personalizar negociación de contenido derivando de la *DefaultContentNegotiator* e invalidar cualquier aspecto de negociación de contenido que le gustaría.
- **Compatibilidad para devolver las respuestas no aceptable 406:** ahora puede devolver 406 respuestas no aceptable en ASP.NET Web API cuando no se encuentra un formateador adecuado mediante la creación de un *DefaultContentNegotiator* con el *excludeMatchOnTypeOnly* parámetro establecido en *true*.
- **Leer datos de formulario como NameValueCollection o JToken:** puede leer datos del formulario en la cadena de consulta URI o en el cuerpo de solicitud como un *NameValueCollection* mediante la *ParseQueryString* y  *ReadAsFormDataAsync* métodos de extensión respectivamente. De forma similar, puede leer los datos del formulario en la cadena de consulta URI o en el cuerpo de solicitud como un *JToken* mediante la *TryReadQueryAsJson* y *ReadAsAsync*&lt;T&gt; métodos de extensión respectivamente.
- **Mejoras de varias partes:** ahora es posible escribir una *MultipartStreamProvider* que se adapta por completo para el tipo de datos de varias partes MIME que pueda leer y presentará el resultado de la forma óptima para el usuario. También puede enlazar un paso de procesamiento posterior en el *MultipartStreamProvider* que permite la implementación realice cualquier posteriores al procesamiento desea en las partes del cuerpo de varias partes MIME. Por ejemplo, el *MultipartFormDataStreamProvider* implementación lee el formulario HTML de los elementos de datos y los agrega a un *NameValueCollection* para que resulte fácil tener acceso desde el llamador.
- **Mejoras de generación de vínculo:** el *UrlHelper* ya no depende *HttpControllerContext*. Ahora puede tener acceso a la *UrlHelper* desde cualquier contexto donde el *HttpRequestMessage* está disponible.
- **Cambio de orden de ejecución de controlador de mensajes:** controladores de mensajes ahora se ejecutan en el orden en que están configurados en lugar de en orden inverso.
- **Aplicación auxiliar para la conexión de controladores de mensajes:** nuevo *HttpClientFactory* que puede conectarlo *DelegatingHandlers* y crear un *HttpClient* con el canalización que desee ir. También proporciona funcionalidad para la conexión de con controladores internos alternativos (el valor predeterminado es *HttpClientHandler*), así como realizar la conexión de con *HttpMessageInvoker* u otro  *DelegatingHandler* en lugar de *HttpClient* como el invocador de la parte superior.
- **Compatibilidad con CDN de optimización Web de ASP.NET:** optimización Web de ASP.NET ahora proporciona compatibilidad con CDN rutas de acceso alternativas lo que le permite especificar para cada paquete una dirección URL adicional que apunta a ese mismo recurso en una red de entrega de contenido. Compatibilidad con CDN permite obtener los paquetes de secuencias de comandos y el estilo geográficamente más cerca a los consumidores de final de las aplicaciones Web.
- **ASP.NET Web API enruta y configuración se mueve a *WebApiConfig.Register* método estático que puede ser resused en el código de prueba.** Rutas de ASP.NET Web API se agregaron previamente en *RouteConfig.RegisterRoutes* junto con el estándar MVC enruta. El predeterminado que se enruta de ASP.NET Web API y la configuración ahora se controlan en otro *WebApiConfig.Register* método para facilitar la prueba.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conocidos y los cambios recientes

- **La versión RC y RTM de ASP.NET MVC 4 devuelve incorrectamente vistas escritorio almacenadas en caché cuando se deben devolver vistas móviles.**

    - Vea [ASP.NET MVC 4 Mobile almacenamiento en caché errores fijo](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) para obtener más información sobre la corrección. La corrección se puede instalar desde el [DisplayModes fijo](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) paquete NuGet.
- **Cambios importantes en el motor de vista Razor**. Se quitaron los siguientes tipos de *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

 También se han quitado los siguientes métodos: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Cuando WebMatrix.WebData.dll se incluye en el directorio/bin de las aplicaciones de ASP.NET MVC 4, asume la dirección URL para la autenticación de formularios.** Agrega el ensamblado WebMatrix.WebData.dll a la aplicación (por ejemplo, seleccionando "Páginas Web ASP.NET con la sintaxis de Razor" cuando se usa el cuadro de diálogo Agregar dependencias implementables) invalidará la redirección de inicio de sesión de autenticación al inicio de sesión/account/en lugar de / cuenta o inicio de sesión según lo esperado de forma predeterminada la cuenta de controlador de ASP.NET MVC. Para evitar este comportamiento y la dirección URL especificada ya en la sección de autenticación del archivo web.config, puede agregar un appSetting denominado PreserveLoginUrl y establézcalo en true: 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **Se produce un error en el Administrador de paquetes de NuGet instalar al intentar instalar ASP.NET MVC 4 para instalaciones en paralelo de Visual Studio 2010 y Visual Web Developer 2010.** Para ejecutar Visual Studio 2010 y Visual Web Developer 2010 paralelo con ASP.NET MVC 4 debe instalar ASP.NET MVC 4 después de haberse instalados ambas versiones de Visual Studio.
- **Se produce un error al desinstalar ASP.NET MVC 4 si ya se han desinstalado los requisitos previos.** Para desinstalar correctamente ASP.NET MVC 4you debe desinstalar ASP.NET MVC 4 antes de desinstalar Visual Studio.
- **Instalar ASP.NET MVC 4 interrumpe las aplicaciones de ASP.NET MVC 3 RTM.** Las aplicaciones de ASP.NET MVC 3 que se crearon con la versión RTM de la versión (no con el [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/download/details.aspx?id=1491) liberar) requieren los siguientes cambios para funcionar en paralelo con ASP.NET MVC 4. Compilar el proyecto sin realizar estos resultados de las actualizaciones en errores de compilación. 

    **Actualizaciones necesarias**

    1. En el archivo raíz Web.config, agregue un nuevo  *&lt;appSettings&gt;*  entrada con la clave *webPages:Version* y el valor *1.0.0.0*. 

        [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
    2. En el Explorador de soluciones, haga clic en el nombre del proyecto y, a continuación, seleccione Descargar proyecto. A continuación, haga clic en el nombre nuevo y seleccione Editar *ProjectName*.csproj.
    3. Busque las siguientes referencias de ensamblado: 

        [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

        Reemplácelas con lo siguiente:

        [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
    4. Guardar los cambios, cierre el archivo de proyecto (.csproj) estaba editando y, a continuación, haga clic en el proyecto y seleccione volver a cargar.
- **Cambiar un proyecto de ASP.NET MVC 4 al destino 4.0 desde 4.5, no actualiza la referencia de ensamblado de EntityFramework:** si cambia un proyecto de ASP.NET MVC 4 a destino 4.0 después como destino 4.5 seguirán apuntando hacia la referencia al ensamblado de Entity Framework la versión 4.5. Para corregir esta desinstalación de problema y vuelva a instalar el paquete EntityFramework NuGet.
- **403 Prohibido cuando se ejecuta una aplicación ASP.NET MVC 4 en Azure después de cambiar al destino 4.0 desde 4.5:** si cambia un proyecto de ASP.NET MVC 4 a destino 4.0 después como destino la 4.5 y, a continuación, implementar en Azure puede ver un error 403 Prohibido en tiempo de ejecución. Para solucionar este problema, agregue lo siguiente al archivo web.config:`<modules runAllManagedModulesForAllRequests="true" />`
- **Visual Studio 2012 se bloquea cuando se escribe un '\' en un literal de cadena en un archivo Razor.** Intentar resolver el problema, escriba la comilla de cierre de la cadena literal en primer lugar.
- **Vaya a &quot;cuenta/administrar&quot; en los resultados de la plantilla de Internet en un error en tiempo de ejecución para los idiomas chino simplificado, TRK y CHT.** Para corregir el problema modificar la página para apartar  *@User.Identity.Name*  colocando como el único contenido dentro de la  *&lt;seguro&gt;*  etiqueta.
- **Proveedores de Google y LinkedIn no se admiten dentro de los sitios Web.** Usar proveedores de autenticación alternativo al implementar sitios Web de Azure.
- **Cuando se utiliza UriPathExtensionMapping con IIS 8 Express o IIS, recibiría 404 errores no encontrado al intentar usar la extensión.** El controlador de archivos estáticos va a interferir con las solicitudes de API web que usan *UriPathExtensionMappings*. Establecer *runAllManagedModulesForAllRequests = true* en el archivo web.config para solucionar el problema.
- **Método Controller.Execute ya no se llama.** Todos los controladores MVC ahora siempre se ejecutan asincrónicamente.
