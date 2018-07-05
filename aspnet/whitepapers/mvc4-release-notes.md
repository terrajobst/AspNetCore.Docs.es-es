---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Este documento describe la versión de ASP.NET MVC 4.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
ms.technology: ''
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: f86220d296e0058b0d482cef066c33e5474d51ef
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369462"
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Este documento describe la versión de ASP.NET MVC 4.


- [Notas de instalación](#_Toc303253802)
- [Documentación](#_Toc303253803)
- [Soporte técnico](#_Toc303253804)
- [Requisitos de software](#_Toc303253805)
- [Nuevas características de ASP.NET MVC 4](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197) (Más información sobre ASP.NET Web API)
    - [Mejoras en las plantillas de proyecto predeterminadas](#_Toc303253808)
    - [Plantilla de proyecto móvil](#_Toc303253809)
    - [Modos de presentación](#_Toc303253810)
    - [jQuery Mobile, el modificador de vista y la invalidación de explorador](#_Toc303253811)
    - [Compatibilidad con la tarea para los controladores asincrónicos](#_Toc303253813)
    - [SDK de Azure](#_Toc303253814)
    - [Migraciones de base de datos](#_Toc303253818)
    - [Plantilla proyecto vacío](#_Toc303253819)
    - [Agregar controlador a cualquier carpeta del proyecto](#_Toc303253820)
    - [Unión y minificación](#_Toc303253821)
    - [Habilitar los inicios de sesión de Facebook y otros sitios con OAuth y OpenID](#_Toc303253822)
- [Actualizar un proyecto de ASP.NET MVC 3 a ASP.NET MVC 4](#_Toc303253806)
- [Cambios desde el candidato de versión comercial de ASP.NET MVC 4](#_Toc303253817)
- [Problemas conocidos y cambios importantes](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Notas de instalación

ASP.NET MVC 4 para Visual Studio 2010 puede instalarse desde el [página principal de ASP.NET MVC 4](../mvc/mvc4.md) mediante el instalador de plataforma Web.

Se recomienda desinstalar las versiones preliminares instaladas previamente de ASP.NET MVC 4 antes de instalar ASP.NET MVC 4. Puede actualizar la versión Beta de ASP.NET MVC 4 y Release Candidate a ASP.NET MVC 4 sin desinstalar.

Esta versión no es compatible con las versiones preliminares de .NET Framework 4.5. Por separado, debe actualizar el cualquier versión instalada de versión preliminar de .NET Framework 4.5 a la versión final antes de instalar ASP.NET MVC 4.

ASP.NET MVC 4 pueden instalarse y ejecutarse en paralelo con ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Documentación

Documentación de ASP.NET MVC está disponible en el sitio Web MSDN en la siguiente URL:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Tutoriales y otra información sobre ASP.NET MVC están disponibles en la página de MVC 4 del sitio Web ASP.NET ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Compatibilidad

ASP.NET MVC 4 es totalmente compatible. Si tiene preguntas sobre cómo trabajar con esta versión puede publicarlos en el foro de ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), donde los miembros de la Comunidad de ASP.NET son normalmente pueden ofrecer soporte técnico informal.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Requisitos de software

Los componentes de ASP.NET MVC 4 para Visual Studio requieren PowerShell 2.0 y Visual Studio 2010 con Service Pack 1 o Visual Web Developer Express 2010 con Service Pack 1.

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>Nuevas características de ASP.NET MVC 4

Esta sección describen las características que se han introducido en la versión de ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET MVC 4 incluye ASP.NET Web API, un nuevo marco para crear servicios HTTP que pueden llegar a una amplia gama de clientes, incluidos exploradores y dispositivos móviles. ASP.NET Web API también es una plataforma ideal para crear servicios RESTful.

ASP.NET Web API incluye compatibilidad con las características siguientes:

- **Modelo de programación moderna de HTTP:** directamente, obtener acceso y manipular las solicitudes HTTP y respuestas en las API Web con un nuevo modelo de objeto HTTP fuertemente tipado. El mismo modelo de programación canalización HTTP está disponible simétricamente en el cliente a través del nuevo *HttpClient* tipo.
- **Compatibilidad completa para las rutas:** ASP.NET Web API es compatible con el conjunto completo de funcionalidades de la ruta de enrutamiento de ASP.NET, incluidos los parámetros de ruta y las restricciones. Además, puede usar convenciones simples para asignar acciones a los métodos HTTP.
- **Negociación de contenido:** el cliente y el servidor pueden trabajar juntos para determinar el formato correcto para los datos devueltos de una API web. ASP.NET Web API proporciona compatibilidad predeterminada para XML, JSON, y formatos codificados de dirección URL del formulario y se puede ampliar esta compatibilidad agregando sus propios formateadores, o incluso reemplazar la estrategia de negociación de contenido predeterminada.
- **Enlace de modelos y validación:** enlazadores de modelos proporcionan una manera fácil para extraer datos de varias partes de una solicitud HTTP y convertir esas partes de mensaje en objetos de .NET que se pueden usar con las acciones de API Web. También se realiza la validación en los parámetros de acción en función de las anotaciones de datos.
- **Filtros:** ASP.NET Web API es compatible con los filtros incluidos filtros conocidos como el *[Authorize]* atributo. Puede crear y conectar sus propios filtros de acciones, autorización y control de excepciones.
- **La composición de consulta:** uso el *[Queryable]* atributo de filtro en una acción que devuelve *IQueryable* para habilitar la compatibilidad para consultar la API web a través de las convenciones de la consulta de OData.
- **Mejora la capacidad de prueba:** en lugar de establecer los detalles HTTP en objetos de contexto estático, web API que funcione con instancias de acciones *HttpRequestMessage* y *HttpResponseMessage*. Cree un proyecto de prueba unitaria, junto con el proyecto Web API para empezar a escribir rápidamente pruebas unitarias para la funcionalidad de API Web.
- **Configuración basada en código:** configuración de ASP.NET Web API se lleva a cabo únicamente a través de código, dejar la configuración de limpiar los archivos. Usar el patrón de localizador de servicio proporcionado para configurar puntos de extensibilidad.
- **Compatibilidad mejorada para los contenedores de inversión de Control (IoC):** ASP.NET Web API proporciona una excelente compatibilidad para contenedores de IoC a través de una abstracción de resolución de dependencias mejorada
- **Autohospedaje:** las API Web se pueden hospedar en su propio proceso, además de IIS mientras sigue usando toda la eficacia de las rutas y otras características de la API Web.
- **Crear ayuda personalizada y probar las páginas:** ahora pueden fácilmente compilar ayuda personalizada y probar las páginas de la API web con el nuevo *IApiExplorer* servicio para obtener una descripción completa en tiempo de ejecución de sus API web.
- **Supervisión y diagnóstico:** ASP.NET Web API ahora proporciona la infraestructura de seguimiento de poco peso que facilita la integración con soluciones de registro existente como System.Diagnostics, ETW y plataformas de registro de terceros. Puede habilitar el seguimiento proporcionando un *ITraceWriter* implementación y se agrega a la configuración de web API.
- **Vincular generación:** utilizar ASP.NET Web API *UrlHelper* para generar vínculos a recursos relacionados en la misma aplicación.
- **Plantilla de proyecto Web API:** seleccione el formulario de proyecto Web API nuevo el Asistente de nuevo proyecto de MVC 4 para ponerse rápidamente en marcha con ASP.NET Web API.
- **Scaffolding:** uso el **Agregar controlador** cuadro de diálogo para aplicar la técnica scaffolding un controlador de API web basado en un Entity Framework rápidamente según el tipo de modelo.

Para obtener más información sobre ASP.NET Web API, visite [ https://www.asp.net/web-api ](../web-api/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Mejoras en las plantillas de proyecto predeterminadas

Se ha actualizado la plantilla que se usa para crear nuevos proyectos de ASP.NET MVC 4 para crear un sitio Web de aspecto más moderno:

![](mvc4-release-notes/_static/image1.png)

Además de mejoras de cosméticas, ha mejorado la funcionalidad de la nueva plantilla. La plantilla emplea una técnica denominada procesamiento adaptable para mostrarse correctamente en exploradores de escritorio y los exploradores móviles sin ninguna personalización.

![](mvc4-release-notes/_static/image2.png)

Para ver la representación adaptable en acción, puede usar un emulador de dispositivos móvil o intente cambiar el tamaño de la ventana del explorador de escritorio para que sea menor. Cuando se obtiene lo suficientemente pequeña como la ventana del explorador, cambiará el diseño de la página.

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Plantilla de proyecto móvil

Si está empezando un nuevo proyecto y desea crear un sitio específicamente para dispositivos móviles y exploradores de Tablet PC, puede usar la nueva plantilla de proyecto de aplicación móvil. Esto se basa en jQuery Mobile, una biblioteca de código abierto para la creación de la interfaz de usuario táctil optimizada:

![](mvc4-release-notes/_static/image3.png)

Esta plantilla contiene la misma estructura de aplicación que la plantilla de aplicación de Internet (y el código de controlador es prácticamente idéntico), pero es un estilo mediante jQuery Mobile que aparezcan correctamente y se comporten bien en dispositivos móviles basados en funcionalidad táctil. Para obtener más información acerca de cómo estructurar y estilo de la interfaz de usuario móvil, consulte el [sitio Web de proyecto móvil de jQuery](http://jquerymobile.com/).

Si ya tiene un sitio orientada a servicios de escritorio que desea agregar vistas móviles optimizadas en, o si desea crear un único sitio que actúa de vistas con estilo diferente a los exploradores de escritorio y móviles, puede usar la nueva característica de modos de presentación. (Consulte la sección siguiente).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Modos de presentación

La nueva característica de modos de presentación permite que una aplicación seleccionar vistas dependiendo del explorador que realiza la solicitud. Por ejemplo, si un explorador de escritorio, solicita la página principal, la aplicación podría usar la plantilla Views\Home\Index.cshtml. Si un explorador móvil solicita la página principal, la aplicación podría devolver la plantilla Views\Home\Index.mobile.cshtml.

También se pueden invalidar los diseños y parciales para los tipos de explorador determinado. Por ejemplo:

- Si la carpeta Views\Shared contiene tanto el \_Layout.cshtml y \_Layout.mobile.cshtml plantillas, de forma predeterminada, la aplicación usará \_Layout.mobile.cshtml durante las solicitudes de los exploradores móviles y \_Layout.cshtml durante otras solicitudes.
- Si una carpeta contiene ambos \_MyPartial.cshtml y \_MyPartial.mobile.cshtml, la instrucción @Html.Partial("\_MyPartial") se representarán \_MyPartial.mobile.cshtml durante las solicitudes de mobile los exploradores, y \_MyPartial.cshtml durante otras solicitudes.

Si desea crear vistas más específicas, diseños o vistas parciales para otros dispositivos, puede registrar un nuevo *DefaultDisplayMode* instancia para especificar el nombre que se busca cuando una solicitud cumple determinadas condiciones. Por ejemplo, podría agregar el código siguiente a la *aplicación\_iniciar* método en el archivo Global.asax para registrar la cadena "iPhone" como modo de presentación que se aplica cuando el Explorador de iPhone de Apple realiza una solicitud:

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

Después de ejecutar este código, cuando un explorador de iPhone de Apple realiza una solicitud, la aplicación usará el Views\Shared\\_Layout.iPhone.cshtml diseño (si existe). Para obtener más información sobre el modo de presentación, consulte [características móviles de ASP.NET MVC 4](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md). Deben instalar las aplicaciones que usan DisplayModeProvider el [DisplayModes fijo](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) paquete NuGet. El [ASP.NET otoño de 2012 actualización](https://go.microsoft.com/fwlink/?LinkID=271322) incluye la [DisplayModes fijo](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) paquete NuGet en las nuevas plantillas de proyecto. Consulte [Fixedd de errores de almacenamiento en caché de ASP.NET MVC 4 Mobile](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) para obtener más información sobre la corrección.

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>jQuery Mobile y las características de Mobile

Para obtener información sobre la creación de aplicaciones móviles con ASP.NET MVC 4 mediante jQuery Mobile, consulte el tutorial [características móviles de ASP.NET MVC 4](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md).

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Compatibilidad con la tarea para los controladores asincrónicos

Ahora puede escribir métodos de acción asincrónicos tan solo los métodos que devuelven un objeto de tipo *tarea* o *tarea&lt;ActionResult&gt;*.

 Para obtener más información, consulte [utilizando los métodos asincrónicos en ASP.NET MVC 4](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>SDK de Azure

ASP.NET MVC 4 es compatible con las versiones 1.6 y versiones más recientes de Windows Azure SDK.

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>Migraciones de base de datos

Proyectos de ASP.NET MVC 4 ahora incluyen Entity Framework 5. Una de las excelentes características de Entity Framework 5 es para las migraciones de base de datos. Esta característica le permite desarrollar fácilmente su esquema de base de datos mediante una migración centrada en el código al tiempo que conserva los datos de la base de datos. Para obtener más información sobre las migraciones de base de datos, vea [agregando un nuevo campo a la tabla y el modelo de película](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md) en el [Introducción al tutorial de ASP.NET MVC 4](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>Plantilla proyecto vacío

La plantilla de proyecto MVC vacío ahora es totalmente vacía para que pueda empezar desde una pizarra limpia por completo. Se ha cambiado la versión anterior de la plantilla de proyecto vacía al nivel básico.

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>Agregar controlador a cualquier carpeta del proyecto

Ahora puede haga clic y seleccione **Agregar controlador** desde cualquier carpeta del proyecto de MVC. Esto proporciona más flexibilidad para organizar los controladores como quiera, incluidos mantener los controladores MVC y Web API en carpetas independientes.

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>Unión y Minificación

El marco de trabajo de unión y minificación permite reducir el número de solicitudes HTTP que una página Web necesita realizar mediante la combinación de archivos individuales en un único archivo integrado para los scripts y CSS. A continuación, puede reducir el tamaño total de esas solicitudes por minificar el contenido de la agrupación. Minificar puede incluir actividades como eliminar espacio en blanco para acortar los nombres de variable incluso selectores CSS según su semántica de contracción. Agrupaciones se declaran y se configura en código y son fácilmente hace referencia en vistas a través de métodos auxiliares que pueden generar uno un único vínculo a la agrupación o, al depurar, varios vínculos a contenido individual de la agrupación. Para obtener más información, consulte [unión y Minificación](../mvc/overview/performance/bundling-and-minification.md).

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Habilitar los inicios de sesión de Facebook y otros sitios con OAuth y OpenID

Las plantillas predeterminadas en la plantilla de proyecto de Internet de ASP.NET MVC 4 ahora incluye compatibilidad con inicio de sesión de OAuth y OpenID mediante la biblioteca de DotNetOpenAuth. Para obtener información sobre cómo configurar un proveedor de OAuth u OpenID, consulte [OAuth/OpenID compatibilidad con formularios Web Forms, MVC y páginas Web](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx) y [en ASP.NET Web Pages de la documentación de características de OAuth y OpenID](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup).

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Actualizar un proyecto de ASP.NET MVC 3 a ASP.NET MVC 4

ASP.NET MVC 4 se puede instalar en paralelo con ASP.NET MVC 3 en el mismo equipo, lo que le ofrece flexibilidad para elegir cuándo se debe actualizar una aplicación de ASP.NET MVC 3 a ASP.NET MVC 4.

Actualización de la manera más sencilla es para crear un nuevo proyecto de ASP.NET MVC 4 y copia todas las vistas, controladores, código y contenido de archivos desde el proyecto de MVC 3 existente al nuevo proyecto y, a continuación actualizar el ensamblado hace referencia en el nuevo proyecto para que coincida con cualquier plantilla no MVC en inspiración assembiles que usa. Si ha realizado cambios en el archivo Web.config en el proyecto de MVC 3, también debe combinar los cambios en el archivo Web.config en el proyecto de MVC 4.

Para actualizar manualmente una aplicación existente de ASP.NET MVC 3 a la versión 4, realice lo siguiente:

1. En el archivo Web.config de todos los archivos del proyecto (no hay uno en la raíz del proyecto, uno en la carpeta Views y uno en la carpeta Views de cada área del proyecto), reemplazar todas las instancias del texto siguiente (Nota: System.Web.WebPages, Version = 1.0.0.0 no se encuentra en proyectos creados con Visual Studio 2012): 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    con el siguiente texto correspondiente:

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. En el archivo Web.config raíz, actualice el *webPages:Version* elemento a "2.0.0.0" y agregue un nuevo *PreserveLoginUrl* clave que tiene el valor "true": 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. En el Explorador de soluciones, haga doble clic en las referencias y seleccione Administrar paquetes de NuGet. En el panel izquierdo, seleccione **origen de paquete oficial Online\NuGet**, a continuación, actualice la siguiente:

    - ASP.NET MVC 4
    - (Opcional) jQuery, jQuery UI de jQuery y validación
    - (Opcional) Entity Framework
    - (Opcional) Modernizr
4. En el Explorador de soluciones, haga clic en el nombre del proyecto y, a continuación, seleccione Descargar proyecto. A continuación, haga clic en el nombre nuevo y seleccione Editar *ProjectName*.csproj.
5. Busque el *ProjectTypeGuids* elemento y reemplace {E53F8FEA-EAE0-44A6-8774-FFD645390401} con {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Guarde los cambios, cierre el archivo de proyecto (.csproj) que estaba editando, haga clic en el proyecto y, a continuación, seleccione recargar el proyecto.
7. Si el proyecto hace referencia a las bibliotecas de terceros que están compiladas con versiones anteriores de ASP.NET MVC, abra el archivo raíz Web.config y agregue los siguientes tres *bindingRedirect* elementos bajo el  *configuración* sección: 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>Cambios desde el candidato de versión comercial de ASP.NET MVC 4

Las notas de la versión Release Candidate de ASP.NET MVC 4 pueden encontrarse aquí:

A continuación se resumen los cambios más importantes de ASP.NET MVC 4 Release Candidate en esta versión:

- **Por la configuración del controlador:** controladores de ASP.NET Web API se pueden atribuir con un atributo personalizado que implementa *IControllerConfiguration* para configurar sus propias formateadores, selector de acción y los enlazadores de parámetro . El *HttpControllerConfigurationAttribute* se ha quitado.
- **Por controladores de mensajes de ruta:** ahora puede especificar el controlador de mensaje final de la cadena de solicitud para una ruta determinada. Esto habilita la compatibilidad con marcos andar junto a usar el enrutamiento para enviar a su propia (que no sean de*IHttpController*) puntos de conexión.
- **Las notificaciones de progreso:** el *ProgressMessageHandler* genera notificaciones de progreso para las entidades de la solicitud que se está cargadas y entidades de respuesta que se va a descargar. Mediante este controlador es posible realizar un seguimiento de cuánto está cargando un cuerpo de solicitud o descargando un cuerpo de respuesta.
- **Inserción de contenido:** el *PushStreamContent* clase permite escenarios donde un productor de datos desea escribir directamente en la solicitud o respuesta (sincrónica o asincrónicamente) mediante una secuencia. Cuando el *PushStreamContent* está listo para aceptar los datos que llama a un delegado de acción con el flujo de salida. El desarrollador, a continuación, puede escribir en el flujo para siempre cuando sea necesario y cerrar ha finalizado la secuencia al escribir. El *PushStreamContent* detecta el cierre de la secuencia y se completa asincrónica subyacente *tarea* para escribir el contenido.
- **Creación de las respuestas de error:** uso el *HttpError* tipo para representar constantemente información de error como errores de validación y las excepciones mientras siga teniendo en cuenta la *IncludeErrorDetailPolicy*. Use la nueva *createerrorresponse, tal y* métodos de extensión para crear fácilmente las respuestas de error con *HttpError* como contenido. El *HttpError* contenido está totalmente contenido negociado.
- **MediaRangeMapping quitado:** intervalos de tipos de medios se pueden controlar mediante el negociador de contenido predeterminado.
- **Ahora es el enlace de parámetro predeterminado para los parámetros de tipo simple [FromUri]:** en versiones anteriores de ASP.NET Web API, el enlace de parámetros predeterminados para parámetros de tipo simple utilizan el enlace de modelos. El enlace de parámetro predeterminado para los parámetros de tipo simple es ahora *[FromUri]*.
- **Selección de acción respeta los parámetros necesarios:** selección de acción en ASP.NET Web API ahora seleccionará solo una acción si se proporcionan todos los parámetros necesarios que proceden de la dirección URI. Puede especificarse un parámetro como opcional, ya que proporciona un valor predeterminado para el argumento en la firma del método de acción.
- **Personalizar los enlaces de parámetros HTTP:** utilizar el *ParameterBindingAttribute* para personalizar el enlazado de parámetros para un parámetro de acción específica o usar el *ParameterBindingRules* en el *HttpConfiguration* para personalizar los enlaces de parámetro más ampliamente.
- **Mejoras del elemento MediaTypeFormatter:** formateadores ahora tienen acceso a toda *HttpContent* instancia.
- **Selección de directiva de almacenamiento en búfer de host:** implemente y configure el *IHostBufferPolicySelector* servicio en ASP.NET Web API para permitir a los hosts determinar la directiva para cuando el búfer es que se usará.
- **Acceso a los certificados de cliente de forma independiente del host:** uso el *GetClientCertificate* método de extensión para obtener el certificado de cliente proporcionado en el mensaje de solicitud.
- **Extensibilidad de negociación de contenido:** personalizar la negociación de contenido mediante la derivación de la *DefaultContentNegotiator* e invalidar cualquier aspecto de la negociación de contenido que le gustaría.
- **Soporte técnico para la devolución de 406 respuestas no aceptable:** ahora puede devolver de 406 respuestas no aceptable en ASP.NET Web API cuando no se encuentra un formateador adecuado mediante la creación de un *DefaultContentNegotiator* con el *excludeMatchOnTypeOnly* parámetro establecido en *true*.
- **Leer datos de formulario como NameValueCollection o JToken:** puede leer datos del formulario en la cadena de consulta URI o en el cuerpo de solicitud como un *NameValueCollection* utilizando el *ParseQueryString* y  *ReadAsFormDataAsync* métodos de extensión, respectivamente. De forma similar, puede leer los datos del formulario en la cadena de consulta URI o en el cuerpo de solicitud como un *JToken* utilizando el *TryReadQueryAsJson* y *ReadAsAsync*&lt;T&gt; métodos de extensión, respectivamente.
- **Mejoras de varias partes:** ahora es posible escribir un *MultipartStreamProvider* que se adapta por completo para el tipo de datos de varias partes MIME que puede leer y presentará el resultado de la forma óptima para el usuario. También puede enlazar un paso de procesamiento posterior en el *MultipartStreamProvider* que permite la implementación realizar cualquier posteriores al procesamiento desea en las partes del cuerpo de varias partes MIME. Por ejemplo, el *MultipartFormDataStreamProvider* implementación lee el formulario HTML de los elementos de datos y los agrega a un *NameValueCollection* para que sean fáciles de obtener desde el llamador.
- **Vincular las mejoras de generación:** el *UrlHelper* ya no depende *HttpControllerContext*. Ahora puede tener acceso a la *UrlHelper* desde cualquier contexto donde el *HttpRequestMessage* está disponible.
- **Cambio del orden de ejecución de controlador de mensajes:** controladores de mensajes ahora se ejecutan en el orden en que se configuran en lugar de en orden inverso.
- **Aplicación auxiliar para la inserción de controladores de mensajes:** nuevo *HttpClientFactory* que puede conectar *DelegatingHandlers* y crear un *HttpClient* con el canalización que desee ir. También proporciona funcionalidad para conectar con los controladores internos alternativos (el valor predeterminado es *HttpClientHandler*), así como realizar el cableado con *HttpMessageInvoker* u otro  *DelegatingHandler* en lugar de *HttpClient* como el invocador de la parte superior.
- **Compatibilidad con CDN de optimización Web de ASP.NET:** optimización Web de ASP.NET ahora proporciona la compatibilidad con una dirección URL adicional que apunta a ese mismo recurso en una red de entrega de contenido de CDN rutas de acceso alternativas lo que le permite especificar para cada lote. Compatibilidad con las redes CDN le permite obtener las uniones de script y estilo geográficamente más cercano a los consumidores finales de las aplicaciones Web. Las aplicaciones de producción deben implementar una acción de reserva cuando la red CDN no está disponible. Probar la reserva.
- **Rutas de ASP.NET Web API y configuración se mueve a *WebApiConfig.Register* método estático que puede ser resused en código de prueba.** Las rutas de ASP.NET Web API se agregaron anteriormente en *RouteConfig.RegisterRoutes* junto con el estándar de MVC enruta. La ASP.NET Web API se enruta de forma predeterminada y la configuración ahora se controlan en otro *WebApiConfig.Register* método para facilitar las pruebas.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conocidos y cambios importantes

- **La versión RC y RTM de ASP.NET MVC 4 devuelve incorrectamente las vistas de escritorio en caché cuando se deben devolver vistas móviles.**

    - Consulte [ASP.NET MVC 4 Mobile almacenamiento en caché errores fijo](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) para obtener más información sobre la corrección. La corrección se puede instalar desde el [DisplayModes fijo](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) paquete NuGet.
- **Cambios importantes en el motor de vistas Razor**. Se han quitado los siguientes tipos de *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  También se han quitado los métodos siguientes: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Cuando WebMatrix.WebData.dll se incluye en el directorio/bin de un aplicaciones ASP.NET MVC 4, asume la dirección URL para la autenticación de formularios.** Agregar el ensamblado WebMatrix.WebData.dll a la aplicación (por ejemplo, seleccionando "ASP.NET Web Pages con Razor Syntax" cuando se usa el cuadro de diálogo Agregar dependencias implementables) invalidará el redireccionamiento de inicio de sesión de autenticación/cuenta/inicio de sesión en lugar de / cuenta/inicio de sesión según lo previsto, el valor predeterminado de cuenta de controlador de ASP.NET MVC. Para evitar este comportamiento y la dirección URL especificada ya en la sección de autenticación del archivo web.config, puede agregar un appSetting denominado PreserveLoginUrl y establézcalo en true: 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **Se produce un error en el Administrador de paquetes de NuGet instalar al intentar instalar ASP.NET MVC 4 para las instalaciones en paralelo de Visual Studio 2010 y Visual Web Developer 2010.** Para ejecutar Visual Studio 2010 y Visual Web Developer 2010 en paralelo con ASP.NET MVC 4 debe instalar ASP.NET MVC 4 después de que ya se han instalado las dos versiones de Visual Studio.
- **Se produce un error al desinstalar ASP.NET MVC 4 si ya se han desinstalado los requisitos previos.** Para desinstalar correctamente ASP.NET MVC 4you debe desinstalar ASP.NET MVC 4 antes de desinstalar Visual Studio.
- **Instalar ASP.NET MVC 4 interrumpe las aplicaciones de ASP.NET MVC 3 RTM.** Las aplicaciones de ASP.NET MVC 3 que se crearon con la versión RTM de versión (no con el [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/download/details.aspx?id=1491) release) requieren los siguientes cambios para poder trabajar en paralelo con ASP.NET MVC 4. Compilar el proyecto sin tener que realizar estos resultados de las actualizaciones en los errores de compilación. 

    **Actualizaciones necesarias**

  1. En el archivo raíz Web.config, agregue un nuevo *&lt;appSettings&gt;* entrada con la clave *webPages:Version* y el valor *1.0.0.0*. 

      [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
  2. En el Explorador de soluciones, haga clic en el nombre del proyecto y, a continuación, seleccione Descargar proyecto. A continuación, haga clic en el nombre nuevo y seleccione Editar *ProjectName*.csproj.
  3. Busque las siguientes referencias de ensamblado: 

      [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

      ¿Desea reemplazarlos con lo siguiente:

      [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
  4. Guarde los cambios, cierre el archivo de proyecto (.csproj) estaba editando y, a continuación, haga clic en el proyecto y seleccione volver a cargar.

- **Cambiar un proyecto ASP.NET MVC 4 a destino 4.0 desde 4.5 no actualiza la referencia de ensamblado de EntityFramework:** si cambia un proyecto ASP.NET MVC 4 a destino 4.0 después dirigidos a 4.5 seguirán apuntando hacia la referencia al ensamblado de Entity Framework la versión 4.5. Para corregir esta desinstalación del problema y vuelva a instalar el paquete EntityFramework NuGet.
- **403 Prohibido cuando se ejecuta una aplicación ASP.NET MVC 4 en Azure después de cambiar al destino 4.0 en 4.5:** si cambia un proyecto ASP.NET MVC 4 a destino 4.0 después 4.5 tiene como destino y, a continuación, implementar en Azure es posible que vea un error 403 Prohibido en tiempo de ejecución. Para solucionar este problema, agregue lo siguiente al archivo web.config: `<modules runAllManagedModulesForAllRequests="true" />`
- **Visual Studio 2012 se bloquea cuando se escribe un '\' en un literal de cadena en un archivo de Razor.** Para que funcione el problema, escriba la comilla de cierre de la cadena literal en primer lugar.
- <strong>Vaya a &quot;cuenta/administrar&quot; en los resultados de la plantilla de Internet en un error en tiempo de ejecución para los idiomas chino simplificado, TRK y chino tradicional.</strong> Para corregir el problema de modificar la página para separar <em>@User.Identity.Name</em> colocando como el único contenido dentro de la <em>&lt;seguro&gt;</em> etiqueta.
- **No se admiten proveedores de Google y LinkedIn dentro de sitios Web de Azure.** Usar proveedores de autenticación alternativo al implementar en sitios Web de Azure.
- **Cuando se usa UriPathExtensionMapping 8 con IIS Express o IIS, recibiría 404 errores no encontrado al intentar usar la extensión.** El controlador de archivos estáticos interferirá con las solicitudes de API web que usan *UriPathExtensionMappings*. Establecer *runAllManagedModulesForAllRequests = true* en el archivo web.config para solucionar el problema.
- **Ya no se llama el método Controller.Execute.** Todos los controladores MVC ahora siempre se ejecutan asincrónicamente.
