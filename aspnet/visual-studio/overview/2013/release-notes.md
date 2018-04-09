---
uid: visual-studio/overview/2013/release-notes
title: ASP.NET y herramientas Web para Visual Studio 2013 notas | Documentos de Microsoft
author: microsoft
description: Este documento describe la versión de ASP.NET y herramientas Web para Visual Studio 2013.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: e9ddd96f186564834ff6bb2c30cf0ed5444cbf1b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>ASP.NET y herramientas Web para Visual Studio 2013 notas
====================
por [Microsoft](https://github.com/microsoft)

> Este documento describe la versión de ASP.NET y herramientas Web para Visual Studio 2013.


## <a name="contents"></a>Contenido

- [Notas sobre la instalación](#TOC1)
- [Documentación](#TOC2)
- [Requisitos de software](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nuevas características de ASP.NET y herramientas Web para Visual Studio 2013

- [Uno de ASP.NET](#TOC6)
- [Nueva experiencia de proyecto Web](#newproj)
- [Scaffolding de ASP.NET](#scaffold)
- [Vínculo con exploradores](#browser-link)
- [Mejoras del Editor de Visual Studio Web](#web-editor)
- [Compatibilidad con aplicaciones de Azure de aplicación de servicio Web en Visual Studio](#waws)
- [Mejoras de publicación de Web](#publish)
- [NuGet 2.7](#nuget)
- [Formularios Web Forms ASP.NET](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [ASP.NET Web API 2](#TOC11)
- [ASP.NET SignalR](#TOC13)
- [Identidad de ASP.NET](#TOC8)
- [Componentes de Microsoft OWIN](#TOC7)
- [Entity Framework 6](#ef6)
- [ASP.NET Razor 3](#TOC14)
- [Suspender la aplicación ASP.NET](#TOC15)
- [Problemas conocidos y los cambios recientes](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>Notas sobre la instalación

ASP.NET y herramientas Web para Visual Studio 2013 se empaquetan en el programa de instalación principal y se puede descargar [aquí](https://www.asp.net/downloads).

<a id="TOC2"></a>
## <a name="documentation"></a>Documentación

Tutoriales y otra información sobre ASP.NET y herramientas Web para Visual Studio 2013 están disponibles en la [sitio web de ASP.NET](https://www.asp.net/).

<a id="TOC4"></a>
## <a name="software-requirements"></a>Requisitos de software

ASP.NET y herramientas Web requiere Visual Studio 2013.

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nuevas características de ASP.NET y herramientas Web para Visual Studio 2013

En las siguientes secciones se describen las características que se han introducido en la versión.

<a id="TOC6"></a>
## <a name="one-aspnet"></a>Uno de ASP.NET

Con la versión de Visual Studio 2013, le hemos llevado un paso hacia unificar la experiencia de uso de tecnologías ASP.NET, por lo que fácilmente puede mezclar y coincidir con las que desea. Por ejemplo, puede iniciar un proyecto de MVC y agregar páginas de formularios Web Forms al proyecto más adelante o fácilmente aplicar la técnica scaffolding API Web en un proyecto de formularios Web Forms. Uno ASP.NET consiste en lo que facilita para usted como desarrollador para realizar las acciones que le encanta en ASP.NET. Con independencia de qué tecnología que elija, puede tener confianza que se está compilando en el marco subyacente confianza de One ASP.NET.

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>Nueva experiencia de proyecto Web

Se ha mejorado la experiencia de creación de nuevos proyectos web en Visual Studio 2013. En el **nuevo proyecto Web de ASP.NET** cuadro de diálogo puede seleccionar el tipo de proyecto desea, configure cualquier combinación de tecnologías (formularios Web Forms, MVC, Web API), configurar opciones de autenticación y agregar un proyecto de prueba unitaria.

![Nuevo proyecto de ASP.NET](release-notes/_static/image1.png)

El nuevo cuadro de diálogo permite cambiar las opciones de autenticación predeterminadas para muchas de las plantillas. Por ejemplo, cuando se crea un proyecto de formularios Web Forms de ASP.NET puede seleccionar cualquiera de las siguientes opciones:

- Sin autenticación
- Cuentas de usuario individuales (pertenencia a ASP.NET o registro del proveedor de redes sociales en)
- Cuentas de organización (Active Directory en una aplicación de internet)
- Autenticación de Windows (Active Directory en una aplicación de intranet)

![Opciones de autenticación](release-notes/_static/image2.png)

Para obtener más información acerca del nuevo proceso para crear proyectos web, vea [Creating ASP.NET Web Projects in Visual Studio 2013](creating-web-projects-in-visual-studio.md). Para obtener más información acerca de las nuevas opciones de autenticación, vea [ASP.NET Identity](#TOC8) más adelante en este documento.

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>Scaffolding de ASP.NET

La técnica de Scaffolding de ASP.NET es un marco de trabajo de generación de código para aplicaciones Web ASP.NET. Resulta muy sencillo agregar código reutilizable en el proyecto que interactúa con un modelo de datos.

En versiones anteriores de Visual Studio, scaffolding estaba limitado a los proyectos de ASP.NET MVC. Con Visual Studio 2013, ahora puede usar el scaffolding para cualquier proyecto de ASP.NET, incluidos los formularios Web Forms. Visual Studio 2013 no admite actualmente la generación de páginas para un proyecto de formularios Web Forms, pero todavía puede usar scaffolding con formularios Web Forms mediante la adición de las dependencias de MVC al proyecto. Se agregará compatibilidad para generar páginas de formularios Web Forms en una futura actualización.

Cuando se utiliza la técnica scaffolding, es asegurarse de que las necesarias están instaladas las dependencias en el proyecto. Por ejemplo, si empieza con un proyecto de formularios Web Forms de ASP.NET y, a continuación, usar el scaffolding para agregar un controlador de API Web, los paquetes de NuGet necesarios y las referencias se agregan al proyecto automáticamente.

Para agregar scaffolding de MVC a un proyecto de formularios Web Forms, agregue un **nuevo elemento de scaffolding** y seleccione **las dependencias de MVC 5** en el cuadro de diálogo. Hay dos opciones de scaffolding MVC; Mínimo y completa. Si selecciona mínimo, solo los paquetes de NuGet y referencias para ASP.NET MVC se agregan al proyecto. Si selecciona la opción completa, se agregan las dependencias mínimas, así como los archivos de contenido necesarios para un proyecto de MVC.

Compatibilidad con scaffolding controladores asincrónicos usa las nuevas características asincrónicas de Entity Framework 6.

Para obtener más información y ver tutoriales, vea [información general sobre la técnica Scaffolding de ASP.NET](aspnet-scaffolding-overview.md).

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>Vínculo de explorador: canal de SignalR entre el explorador y Visual Studio

El nuevo [vínculo de explorador](using-browser-link.md) característica le permite conectarse varios exploradores a Visual Studio y actualizarlos todos, haga clic en un botón en la barra de herramientas. Puede conectar varios exploradores para el sitio de desarrollo, incluidos los emuladores de Windows mobile y haga clic en Actualizar para actualizar todos los exploradores todos al mismo tiempo. Vínculo de explorador también expone una API para permitir a los desarrolladores escribir extensiones de vínculo de explorador.

![](release-notes/_static/image3.png)

Permitiendo a los programadores aprovechar las ventajas de la API de vínculo de explorador, es posible crear escenarios muy avanzados que cruza los límites entre Visual Studio y cualquier explorador que está conectado. Web Essentials aprovecha las ventajas de la API para crear una experiencia integrada entre Visual Studio y herramientas de desarrollo del explorador, remotos controlar los emuladores de Windows mobile y mucho más.

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Mejoras del Editor de Visual Studio Web

Visual Studio 2013 incluye un nuevo editor de HTML para los archivos de Razor y archivos HTML en las aplicaciones web. El nuevo editor de HTML proporciona un único esquema unificado basado en HTML5. Tiene finalización de llave automática, jQuery UI y AngularJS atributo IntelliSense, agrupación de IntelliSense, Id. y nombre de clase Intellisense y otras mejoras incluidas mejorar el rendimiento, formato de atributo y etiquetas inteligentes.

Captura de pantalla siguiente muestra cómo utilizar atributos arranque IntelliSense en el editor HTML.

![IntelliSense en el editor de HTML](release-notes/_static/image4.png)

Visual Studio 2013 también se incluye con ambos CoffeeScript y menos editores integrados. El editor de LESS incluye todas las características interesantes en el editor de CSS y tiene de Intellisense específica de las variables y mixins en el menor de los documentos en la @import cadena.

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Compatibilidad con aplicaciones de Azure de aplicación de servicio Web en Visual Studio

En Visual Studio 2013 con Azure SDK para .NET 2.2, puede usar **Explorador de servidores** para interactuar directamente con las aplicaciones web remoto. Puede iniciar sesión en su cuenta de Azure, crear nuevas aplicaciones web, configurar aplicaciones, ver registros en tiempo real y mucho más. Se libera estará disponible pronto después SDK 2.2, podrá ejecutarse en modo de depuración remota en Azure. La mayoría de las nuevas características para aplicaciones de Web del servicio de aplicación de Azure también funcionan en Visual Studio 2012 cuando se instala la versión actual de Azure SDK para .NET.

Para obtener más información, vea los siguientes recursos:

- [Crear una aplicación web ASP.NET en el servicio de aplicación de Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Solucionar problemas de una aplicación web en el servicio de aplicaciones de Azure con Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Mejoras de publicación de Web

Visual Studio 2013 incluye características nuevas y mejoradas de publicación Web. Estos son algunos de ellos:

- Fácilmente [automatizar el cifrado de archivos Web.config](https://go.microsoft.com/fwlink/?LinkId=325529). (Este vínculo y las dos siguientes apuntan documentación en MSDN que podría no estar disponible hasta que el tiempo de ejecución en el día en el 10/17).
- Fácilmente [automatizar poner una aplicación sin conexión durante la implementación](https://go.microsoft.com/fwlink/?LinkId=325530).
- Configurar Web Deploy para [utilizar la suma de comprobación de archivo en lugar de cambiar a la última fecha](https://go.microsoft.com/fwlink/?LinkId=325531) para determinar qué archivos deben copiarse al servidor.
- Publicar rápidamente archivos seleccionados individuales (incluido el archivo Web.config) cuando se usa FTP o métodos de publicación de sistema de archivos así como con Web Deploy.

Para obtener más información sobre la implementación de web ASP.NET, vea [el sitio de ASP.NET](https://go.microsoft.com/fwlink/?LinkId=322027).

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 incluye un amplio conjunto de nuevas características que se describen en detalle en [notas de la versión 2.7 NuGet](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Esta versión de NuGet también elimina la necesidad de dar su consentimiento explícito para la característica de restauración de paquetes de NuGet descargar los paquetes. Consentimiento (y la casilla de verificación asociada en el cuadro de diálogo de preferencias de NuGet) ahora se conceden mediante la instalación de NuGet. Restauración del paquete simplemente funciona ahora de forma predeterminada.

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>Formularios Web Forms de ASP.NET

### <a name="one-aspnet"></a>Uno de ASP.NET

Las plantillas de proyecto de formularios Web Forms se integren perfectamente con la nueva experiencia de One ASP.NET. Puede agregar MVC y API Web admiten a su proyecto de formularios Web Forms, y puede configurar la autenticación mediante el Asistente para la creación del proyecto de One ASP.NET. Para obtener más información, consulte [Creating ASP.NET Web Projects in Visual Studio 2013](creating-web-projects-in-visual-studio.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Las plantillas de proyecto de formularios Web Forms admiten el nuevo marco de ASP.NET Identity. Además, las plantillas ahora admiten la creación de un proyecto de intranet de formularios Web Forms. Para obtener más información, consulte [métodos de autenticación](creating-web-projects-in-visual-studio.md#auth) en **Creating ASP.NET Web Projects in Visual Studio 2013**.

### <a name="bootstrap"></a>bootstrap

Usan las plantillas de formularios Web Forms [arranque](http://twitter.github.io/bootstrap/) para proporcionar un sensibles y elegante aspecto y diseño que se puede personalizar fácilmente. Para obtener más información, consulte [Bootstrap en las plantillas de proyecto web de Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>Uno de ASP.NET

Las plantillas de proyecto Web MVC se integren perfectamente con la nueva experiencia de One ASP.NET. Puede personalizar el proyecto de MVC y configurar la autenticación mediante el Asistente para la creación del proyecto de One ASP.NET. Encontrará un tutorial de introducción a ASP.NET MVC 5 en [Introducción a ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Para obtener información sobre cómo actualizar proyectos MVC 4 a 5 de MVC, vea [cómo actualizar un ASP.NET MVC 4 y el proyecto de API Web para ASP.NET MVC 5 y API Web 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Las plantillas de proyecto MVC se han actualizado para usar la identidad de ASP.NET para la autenticación y administración de identidades. Encontrará un tutorial que incluye autenticación de Facebook y Google y la nueva API de pertenencia en [crear una aplicación de ASP.NET MVC 5 con Facebook y Google OAuth2 y OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) y [crear una aplicación de MVC de ASP.NET con la autenticación y Base de datos SQL e implementar al servicio de aplicaciones de Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).

### <a name="bootstrap"></a>bootstrap

La plantilla de proyecto MVC se ha actualizado para usar [arranque](http://getbootstrap.com/) para proporcionar un sensibles y elegante aspecto y diseño que se puede personalizar fácilmente. Para obtener más información, consulte [Bootstrap en las plantillas de proyecto web de Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtros de autenticación

Los filtros de autenticación son un nuevo tipo de filtro de MVC de ASP.NET que se ejecute antes que los filtros de autorización en la canalización de ASP.NET MVC y le permiten especificar la lógica de los autenticación por cada acción, por controlador, o globalmente para todos los controladores. Los filtros de autenticación procesan las credenciales en la solicitud y proporcionan una entidad de seguridad correspondiente. Filtros de autenticación también pueden agregar los desafíos de autenticación en respuesta a solicitudes no autorizadas.

### <a name="filter-overrides"></a>Invalidaciones de filtro

Ahora puede invalidar los filtros se aplican a un método de acción determinado o un controlador mediante la especificación de un filtro de invalidación. Filtros de reemplazo especifican un conjunto de tipos de filtro que no se debe ejecutar para un ámbito determinado (acción o controlador). Esto le permite configurar los filtros que se aplican globalmente pero, a continuación, excluirán determinados filtros globales de la aplicación a acciones específicas o controladores.

### <a name="attribute-routing"></a>Enrutamiento mediante atributos

Ahora, ASP.NET MVC admite el enrutamiento del atributo, gracias a una contribución por Tim McCall, el autor de [ http://attributerouting.net ](http://attributerouting.net). Con el enrutamiento de atributo puede especificar sus rutas anotando las acciones y los controladores.

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>ASP.NET Web API 2

### <a name="attribute-routing"></a>Enrutamiento mediante atributos

ASP.NET Web API ahora admite el enrutamiento del atributo, gracias a una contribución por Tim McCall, el autor de [ http://attributerouting.net ](http://attributerouting.net). Con el enrutamiento de atributo puede especificar sus rutas de API Web anotando las acciones y los controladores similar al siguiente:

[!code-csharp[Main](release-notes/samples/sample1.cs)]

Ruta de atributo proporciona mayor control sobre los URI de la API web. Por ejemplo, puede definir fácilmente una jerarquía de recursos mediante un único controlador de API:

[!code-csharp[Main](release-notes/samples/sample2.cs)]

Atributo enrutamiento también proporciona una sintaxis adecuada para especificar los parámetros opcionales, valores predeterminados y restricciones de la ruta:

[!code-csharp[Main](release-notes/samples/sample3.cs)]

Para obtener más información acerca del enrutamiento de atributo, vea [atributo enrutamiento en API Web 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).

### <a name="oauth-20"></a>OAuth 2.0

Las plantillas de proyecto de API Web y aplicación de una sola página ahora admiten la autorización mediante OAuth 2.0. OAuth 2.0 es un marco para autorizar el acceso de cliente a los recursos protegidos. Funciona para una variedad de clientes como exploradores y dispositivos móviles.

Compatibilidad con OAuth 2.0 se basa en el nuevo software intermedio de seguridad proporcionada por los componentes de Microsoft OWIN para la autenticación de portador e implementar el rol de servidor de autorización. Como alternativa, los clientes se pueden autorizar mediante un servidor de autorización de organización, como Azure Active Directory o ADFS en Windows Server 2012 R2.

### <a name="odata-improvements"></a>Mejoras de OData

**Expandir la compatibilidad para $select, $, $batch y $value**

ASP.NET Web API OData ahora es totalmente compatible con para $select, $expand y $value. También puede utilizar $batch para solicitud de procesamiento por lotes y el procesamiento de conjuntos de cambios.

El $select y $expanden opciones le permiten cambiar la forma de los datos que se devuelven desde un extremo de OData. Para obtener más información, consulte [Introducción $select y $amplían la compatibilidad en Web API OData](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).

**Extensibilidad mejorada**

Los formateadores de OData ahora son extensibles. Puede agregar metadatos de la entrada de Atom, admitir entradas de vínculo de secuencia y los medios con nombre, agregar anotaciones de instancia y personalizar cómo se generan los vínculos.

**Compatibilidad con sin tipo**

Ahora puede compilar servicios OData sin necesidad de definir tipos CLR para los tipos de entidad. En su lugar, pueden tener o devuelven instancias de los controladores de OData **IEdmObject**, que son los formateadores de OData para serializar y deserializar.

**Reutilizar un modelo existente**

Si ya tiene un entity data model (EDM) existente, puede ahora volver a usarla directamente, en lugar de tener que crear uno nuevo. Por ejemplo, si usa Entity Framework, puede usar el modelo EDM que EF genera automáticamente.

### <a name="request-batching"></a>Solicitud de procesamiento por lotes

Solicitud de procesamiento por lotes combina varias operaciones en una única solicitud HTTP POST, para reducir el tráfico de red y proporcionar un Suavizador, menos interfaz de usuario locuaz. ASP.NET Web API ahora es compatible con varias estrategias para el procesamiento por lotes de solicitud:

- Utilice el punto de conexión de $batch de un servicio OData.
- Empaquetar varias solicitudes en una única solicitud de varias partes MIME.
- Usar un formato de procesamiento por lotes personalizado.

Para habilitar la solicitud de procesamiento por lotes, basta con agregar una ruta con un controlador de procesamiento por lotes a la configuración de Web API:

[!code-csharp[Main](release-notes/samples/sample4.cs)]

También puede controlar si las solicitudes o ejecutar de forma secuencial o en cualquier orden.

### <a name="portable-aspnet-web-api-client"></a>Portable ASP.NET Web API de cliente

Ahora puede usar al cliente de API Web de ASP.NET para crear bibliotecas de clases portables que funcionan a través de las aplicaciones de la tienda Windows y Windows Phone 8. También puede crear a formateadores portátiles que se pueden compartir entre cliente y servidor.

### <a name="improved-testability"></a>Capacidad para realizar pruebas mejorada

Web API 2 hace mucho más fácil de unidad que los controladores de API de pruebas. Simplemente crear una instancia de su controlador de API con el mensaje de solicitud y la configuración y, a continuación, llame al método de acción que se va a probar. También es fácil simular la **UrlHelper** (clase), para los métodos de acción que realizar la generación de vínculo.

### <a name="ihttpactionresult"></a>IHttpActionResult

Ahora puede implementar IHttpActionResult para encapsular el resultado de los métodos de acción de la API Web. El tiempo de ejecución de ASP.NET Web API para generar el mensaje de respuesta resultante se ejecuta un IHttpActionResult devuelto de un método de acción de la API Web. Se puede devolver un IHttpActionResult desde cualquier acción de Web API para simplificar la unidad de las pruebas de la implementación de la API de Web. Por comodidad que se proporcionan varias implementaciones de IHttpActionResult de fábrica, incluidos los resultados para devolver códigos de estado específico, con formato respuestas contenidas o se negocia de contenido.

### <a name="httprequestcontext"></a>HttpRequestContext

El nuevo **HttpRequestContext** realiza un seguimiento de cualquier estado que está asociado a la solicitud pero no está inmediatamente disponible en la solicitud. Por ejemplo, puede usar el **HttpRequestContext** para obtener datos de ruta, la entidad de seguridad asociado a la solicitud, el certificado de cliente, el **UrlHelper** y la raíz de la ruta de acceso virtual. Puede crear fácilmente un **HttpRequestContext** de unidad con fines de prueba.

Dado que fluye a la entidad de seguridad para la solicitud con la solicitud en lugar de basarse en **Thread.CurrentPrincipal**, la entidad de seguridad ahora está disponible durante el ciclo de vida de la solicitud mientras se encuentra en la canalización Web API.

### <a name="cors"></a>CORS

Gracias a otro contribución excelente de Brock Allen, ASP.NET ahora es totalmente compatible con entre orígenes de solicitud de uso compartido (CORS).

Seguridad del explorador impide que una página web que realiza las solicitudes AJAX a otro dominio. [CORS](http://www.w3.org/TR/cors/) es un estándar de W3C que permite que un servidor relajar la directiva de mismo origen. Con CORS, un servidor puede permitir explícitamente algunas solicitudes entre orígenes y rechazar los otros usuarios.

Web API 2 ahora es compatible con CORS, incluido el control automático de las solicitudes preparatorias. Para obtener más información, consulte [habilitar solicitudes Cross-Origin en ASP.NET Web API](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).

### <a name="authentication-filters"></a>Filtros de autenticación

Los filtros de autenticación son un nuevo tipo de filtro de ASP.NET Web API que se ejecute antes que los filtros de autorización en la canalización de ASP.NET Web API y le permiten especificar la lógica de los autenticación por cada acción, por controlador, o globalmente para todos los controladores. Los filtros de autenticación procesan las credenciales en la solicitud y proporcionan una entidad de seguridad correspondiente. Filtros de autenticación también pueden agregar los desafíos de autenticación en respuesta a solicitudes no autorizadas.

### <a name="filter-overrides"></a>Invalidaciones de filtro

Ahora puede invalidar los filtros se aplican a un método de acción determinado o un controlador, mediante la especificación de un filtro de invalidación. Filtros de reemplazo especifican un conjunto de tipos de filtro que no se debe ejecutar para un ámbito determinado (acción o controlador). Esto permite agregar filtros globales, pero, a continuación, excluir algunas de las acciones específicas o controladores.

### <a name="owin-integration"></a>Integración de OWIN

ASP.NET Web API ahora totalmente es compatible con OWIN y se pueden ejecutar en cualquier host compatible con OWIN. También se incluye un **HostAuthenticationFilter** que proporciona integración con el sistema de autenticación OWIN.

Con la integración de OWIN, puede probarlo internamente API Web en su propio proceso junto con otro middleware OWIN, por ejemplo, SignalR. Para obtener más información, consulte [OWIN de uso con la API de Web de ASP.NET Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>SignalR de ASP.NET 2.0

En las siguientes secciones se describen características de SignalR 2.0.

- [Basado en OWIN](#builtonowin)
- [MapHubs y MapConnection ahora son MapSignalR](#MapSignalR)
- [Compatibilidad entre dominios](#crossdomain)
- [soporte de iOS y Android mediante MonoTouch y MonoDroid](#mobile)
- [Cliente .NET portátil](#portable)
- [Nuevo paquete de autohospedaje](#selfhost)
- [Compatibilidad con el servidor compatible con versiones anteriores](#backwardcompat)
- [Quitado la compatibilidad con servidor para .NET 4.0](#remove40)
- [Enviar un mensaje a una lista de clientes y grupos](#messagelist)
- [Enviar un mensaje a un usuario específico](#sendtouser)
- [Una mejor compatibilidad con control de errores](#errorhandling)
- [Unidad más fácil de pruebas de concentradores](#unittesting)
- [Control de errores de JavaScript](#javascripterror)

Para obtener un ejemplo de cómo actualizar un proyecto existente de 1.x a 2.0 SignalR, consulte [actualizar un SignalR 1.x proyecto](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>Basado en OWIN

SignalR 2.0 se basa completamente en [OWIN (la interfaz Web abierta para. NET)](http://owin.org/). Este cambio hace que el proceso de instalación para SignalR mucho más coherente entre las aplicaciones SignalR hospedado en web y hospedadas por sí mismo, pero también necesita un número de cambios en la API.

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs y MapConnection ahora son MapSignalR

Por compatibilidad con los estándares OWIN, estos métodos se ha cambiado a `MapSignalR`. `MapSignalR` llama sin parámetros asignarán todos los concentradores (como `MapHubs` en versión 1.x); para asignar individuales **PersistentConnection** objetos, especifique el tipo de conexión como parámetro de tipo y la extensión de dirección URL para la conexión como la primer argumento.

El `MapSignalR` método se llama en una clase de inicio de Owin. Visual Studio 2013 contiene una nueva plantilla para una clase de inicio de Owin; Para usar esta plantilla, haga lo siguiente:

1. Haga doble clic en el proyecto
2. Seleccione **agregar**, **nuevo elemento...**
3. Seleccione **clase de inicio de Owin**. La nueva clase el nombre **Startup.cs**.

En un **la aplicación Web,** la clase de inicio Owin que contiene el `MapSignalR` método, a continuación, se agrega al proceso de inicio de Owin mediante una entrada en el nodo de configuración de aplicación del archivo Web.Config, tal y como se muestra a continuación.

En un **aplicación hospeda a sí mismo**, la clase de inicio se pasa como parámetro de tipo de la `WebApp.Start` método.

**Asignación de los concentradores y conexiones de SignalR 1.x (desde el archivo de aplicación global en una aplicación web):** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**Los centros de asignación y las conexiones SignalR 2.0 (desde un archivo de clase de inicio de Owin):** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

En un **aplicación hospeda a sí mismo**, la clase de inicio se pasa como el parámetro de tipo para el `WebApp.Start` método, tal y como se muestra a continuación.

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>Compatibilidad entre dominios

En SignalR 1.x solicitudes entre dominios se controla mediante una sola marca de EnableCrossDomain. Este indicador controla las solicitudes de JSONP y CORS. Para una mayor flexibilidad, compatibilidad con todos los CORS se ha quitado del componente de servidor de SignalR (JavaScript lients seguir usando CORS normalmente si se detecta que el explorador admite), y nueva middleware OWIN pone a su disposición admitir estos escenarios.

SignalR 2.0, si JSONP es obligatorio en el cliente (para admitir las solicitudes entre dominios en exploradores de versiones anteriores), será necesario habilitar de forma explícita mediante el establecimiento `EnableJSONP` en el `HubConfiguration` el objeto a `true`, tal y como se muestra a continuación. JSONP está deshabilitado de forma predeterminada, ya que es menos segura que CORS.

Para agregar el nuevo middleware CORS en SignalR 2.0, agregue el `Microsoft.Owin.Cors` biblioteca para su proyecto y llame `UseCors` antes el middleware de SignalR, tal como se muestra en la siguiente sección.

**Agregar Microsoft.Owin.Cors a un proyecto**: para instalar esta biblioteca, ejecute el siguiente comando en la consola de administrador de paquetes:

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

Este comando agregará la 2.0.0 versión del paquete a su proyecto.

**Al llamar a UseCors**

Los siguientes fragmentos de código muestran cómo implementar las conexiones entre dominios en SignalR 1.x y 2.0.

**Implementación de las solicitudes entre dominios en SignalR 1.x (desde el archivo de aplicación global)**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**Implementación de las solicitudes entre dominios en SignalR 2.0 (desde un archivo de código de C#)**

El código siguiente muestra cómo habilitar CORS o JSONP en un proyecto de SignalR 2.0. Este ejemplo de código usa `Map` y `RunSignalR` en lugar de `MapSignalR`, de modo que se ejecuta el middleware CORS solo para las solicitudes de SignalR que requieren compatibilidad con CORS (en lugar de para todo el tráfico en la ruta de acceso especificada en `MapSignalR`.) `Map` también puede usarse para cualquier otro middleware que necesite ejecutar para un prefijo de dirección URL concreto, en lugar de en toda la aplicación.

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>soporte de iOS y Android mediante MonoTouch y MonoDroid

Se ha agregado compatibilidad para clientes de iOS y Android mediante componentes MonoTouch y MonoDroid desde el [Xamarin biblioteca](https://xamarin.com/). Para obtener más información sobre cómo utilizarlas, vea [utilización de componentes de Xamarin](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln). Estos componentes estarán disponibles en la [Xamarin almacén](https://store.xamarin.com/) cuando está disponible la versión RTW de SignalR.

<a id="portable"></a> ### Cliente de .NET portátil

Mejor facilitar el desarrollo multiplataforma, la Silverlight, WinRT y los clientes de Windows Phone se han reemplazado por un solo cliente .NET portátil que admite las siguientes plataformas:

- NET 4.5
- Silverlight 5
- WinRT (.NET para aplicaciones de la tienda de Windows)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>Nuevo paquete de autohospedaje

Ahora hay un paquete de NuGet para que sea más fácil empezar a trabajar con SignalR autohospedaje (aplicaciones de SignalR que se hospedan en un proceso u otra aplicación, en lugar de que se aloja en un servidor web). Para actualizar un proyecto de autohospedaje compilado con SignalR 1.x, quite el paquete Microsoft.AspNet.SignalR.Owin y agregue el paquete Microsoft.AspNet.SignalR.SelfHost. Para obtener más información sobre cómo empezar a usar el paquete de autohospedaje, consulte [Tutorial: SignalR autohospedaje](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>Compatibilidad con el servidor compatible con versiones anteriores

En versiones anteriores de SignalR, las versiones del paquete de SignalR que se usa en el cliente y el servidor debía ser idénticos. Para admitir aplicaciones de cliente grueso que serían difíciles de actualizar, SignalR 2.0 ahora admite el uso de una versión más reciente del servidor con un cliente anterior. **Nota: SignalR 2.0 no admite servidores creados con versiones anteriores con clientes más recientes.**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>Quitado la compatibilidad con servidor para .NET 4.0

SignalR 2.0 quitó la compatibilidad para la interoperabilidad de servidor con .NET 4.0. .NET 4.5 se debe utilizar con servidores de SignalR 2.0. Sigue siendo un cliente .NET 4.0 para SignalR 2.0.

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>Enviar un mensaje a una lista de clientes y grupos

En SignalR 2.0, es posible enviar un mensaje con una lista de identificadores de grupo y cliente. Los siguientes fragmentos de código muestran cómo hacerlo.

**Enviar un mensaje a una lista de clientes y grupos mediante PersistentConnection**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**Enviar un mensaje a una lista de clientes y grupos, usando para ello centros**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>Enviar un mensaje a un usuario específico

Esta característica permite a los usuarios especificar ¿qué es el identificador de usuario en función de un objeto IRequest a través de una nueva interfaz IUserIdProvider:

**La interfaz de IUserIdProvider**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

De forma predeterminada será una implementación que usa IPrincipal.Identity.Name del usuario como el nombre de usuario.

En los concentradores, podrá enviar mensajes a estos usuarios a través de una API nueva:

**Uso de la API de Clients.User**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>Una mejor compatibilidad con control de errores

Los usuarios ahora pueden producir **HubException** de cualquier invocación de concentrador. El constructor de la **HubException** puede tardar un mensaje de cadena y un objeto de datos de error adicionales. SignalR se auto-serializar la excepción y enviarlo al cliente que se utilizará para rechazar o no la invocación del método de concentrador.

El **mostrar excepciones detallados de concentrador** configuración no repercute sobre **HubException** que se va a enviar al cliente o no; se envía siempre.

**Código de servidor que muestra el envío de un HubException al cliente**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**Código de cliente de JavaScript que muestra responde a un HubException enviado desde el servidor**

[!code-html[Main](release-notes/samples/sample16.html)]

**Código de cliente de .NET que muestra responde a un HubException enviado desde el servidor**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>Unidad más fácil de pruebas de concentradores

SignalR 2.0 incluye una interfaz denominada `IHubCallerConnectionContext` en concentradores que resulta más fácil crear las invocaciones de lado de cliente ficticia. Fragmentos de código siguientes muestran cómo utilizar esta interfaz con pruebas populares [xUnit.net](https://github.com/xunit/xunit) y [moq](https://code.google.com/p/moq/).

**Pruebas unitarias SignalR con xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**Pruebas unitarias de SignalR con moq**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>Control de errores de JavaScript

En SignalR 2.0, todas las devoluciones de llamada de control de errores de JavaScript devuelven objetos de error de JavaScript en lugar de cadenas sin formato. Esto permite SignalR fluir la información más completa a los controladores de error. Puede obtener la excepción interna de la `source` propiedad del error.

**Código de cliente de JavaScript que controla la excepción Start.Fail**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET Identity

### <a name="new-aspnet-membership-system"></a>Nuevo sistema de pertenencia ASP.NET

ASP.NET Identity es el nuevo sistema de pertenencia para las aplicaciones ASP.NET. ASP.NET Identity resulta muy sencillo integrar datos de perfil de usuario específica con datos de la aplicación. Identidad de ASP.NET también permite elegir el modelo de persistencia para los perfiles de usuario de la aplicación. Puede almacenar los datos en una base de datos de SQL Server o en otro almacén de datos, incluidos los almacenes de datos NoSQL, como las tablas de almacenamiento de Azure. Para obtener más información, consulte [cuentas de usuario individuales](creating-web-projects-in-visual-studio.md#indauth) en **Creating ASP.NET Web Projects in Visual Studio 2013**.

### <a name="claims-based-authentication"></a>Autenticación basada en notificaciones

Ahora, ASP.NET admite la autenticación basada en notificaciones, que la identidad del usuario se representa como un conjunto de notificaciones de un emisor de confianza. Los usuarios pueden ser autenticados mediante un nombre de usuario y una contraseña que se mantienen en una base de datos de aplicación o con proveedores de identidades sociales (por ejemplo: Accounts de Microsoft, Facebook, Google, Twitter), o mediante las cuentas organizativas a través de Azure Active Directory o Servicios de federación de Active Directory (ADFS).

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Integración con Azure Active Directory y Active Directory de Windows Server

Ahora puede crear proyectos ASP.NET que utilizan Azure Active Directory o Windows Server Active Directory (AD) para la autenticación. Para obtener más información, consulte [cuentas organizativas](creating-web-projects-in-visual-studio.md#orgauth) en **Creating ASP.NET Web Projects in Visual Studio 2013**.

### <a name="owin-integration"></a>Integración de OWIN

Autenticación de ASP.NET se basa ahora en middleware OWIN que se puede usar en cualquier host basado en OWIN. Para obtener más información acerca de OWIN, vea el siguiente [componentes de Microsoft OWIN](#TOC7) sección.

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Componentes de Microsoft OWIN

[Abrir la interfaz Web para .NET](http://owin.org/) (OWIN) define una abstracción entre servidores web de .NET y las aplicaciones web. OWIN separa la aplicación web desde el servidor, realizar independiente de host de las aplicaciones web. Por ejemplo, puede hospedar una aplicación web basada en OWIN en IIS u hospedarlo en un proceso personalizado.

Cambios introducidos en los componentes de Microsoft OWIN (también conocido como el proyecto Katana) incluyen nuevos componentes de servidor y el host, nuevas bibliotecas de aplicación auxiliar y middleware y middleware de autenticación de nuevo.

Para obtener más información acerca de OWIN y Katana, consulte [What's new en OWIN y Katana](../../../aspnet/overview/owin-and-katana/index.md).

**Nota: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) aplicaciones no pueden ejecutarse en modo clásico de IIS; se debe ejecutar en el modo integrado.**

**Nota: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) aplicaciones se deben ejecutar con plena confianza.**

### <a name="new-servers-and-hosts"></a>Hosts y servidores nuevos

Con esta versión, se agregaron nuevos componentes para habilitar escenarios de host propio. Estos componentes incluyen los siguientes paquetes de NuGet:

- **Microsoft.Owin.Host.HttpListener**. Proporciona un servidor OWIN que utiliza **HttpListener** para escuchar las solicitudes HTTP y dirigirlas a la canalización OWIN.
- **Microsoft.Owin.Hosting** proporciona una biblioteca para los desarrolladores que deseen autohospedaje una canalización OWIN en un proceso personalizado, como una aplicación de consola o el servicio de Windows.
- **OwinHost**. Proporciona un archivo ejecutable independiente que contenga `Microsoft.Owin.Hosting` y le permite probar internamente una canalización OWIN sin tener que escribir una aplicación de host personalizado.

Además, el `Microsoft.Owin.Host.SystemWeb` paquete permite ahora middleware para proporcionar sugerencias para la **SystemWeb** server, lo que indica que el middleware debe llamarse durante una fase determinada de canalización ASP.NET. Esta característica es especialmente útil para el middleware de autenticación, que se debe ejecutar al principio de la canalización ASP.NET.

### <a name="helper-libraries-and-middleware"></a>El software intermedio y bibliotecas auxiliares

Aunque puede escribir componentes OWIN con solo las definiciones de función y el tipo de la especificación de OWIN, el nuevo `Microsoft.Owin` paquete proporciona un conjunto de abstracciones más fácil de usar. Este paquete combina varios paquetes anteriores (p. ej., `Owin.Extensions`, `Owin.Types`) en un modelo único objeto estructurado que, a continuación, se puede usar fácilmente con otros componentes OWIN. De hecho, la mayoría de los componentes de Microsoft OWIN usar ahora este paquete.

> [!NOTE]
> [OWIN](http://www.owin.org) aplicaciones no pueden ejecutarse en modo clásico de IIS; se debe ejecutar en el modo integrado.

> [!NOTE]
> [OWIN](http://www.owin.org) aplicaciones se deben ejecutar con plena confianza.

Esta versión también incluye el paquete Microsoft.Owin.Diagnostics, que incluye middleware para validar una aplicación en ejecución OWIN, además de middleware de página de errores para ayudar a investigar los errores.

### <a name="authentication-components"></a>Componentes de autenticación

Los siguientes componentes de autenticación están disponibles.

- **Microsoft.Owin.Security.ActiveDirectory**. Habilita la autenticación mediante servicios de directorio local o en la nube.
- **Microsoft.Owin.Security.Cookies** habilita la autenticación con cookies. Este paquete se denominaba anteriormente `Microsoft.Owin.Security.Forms`.
- **Microsoft.Owin.Security.Facebook** habilita la autenticación con el servicio basado en OAuth de Facebook.
- **Microsoft.Owin.Security.Google** habilita la autenticación mediante el servicio de basados en OpenID de Google.
- **Microsoft.Owin.Security.Jwt** habilita la autenticación mediante tokens JWT.
- **Microsoft.Owin.Security.MicrosoftAccount** habilita la autenticación mediante cuentas de Microsoft.
- **Microsoft.Owin.Security.OAuth**. Proporciona un servidor de autorización de OAuth, así como de middleware para autenticar tokens de portador.
- **Microsoft.Owin.Security.Twitter** habilita la autenticación con el servicio de basado en OAuth de Twitter.

Esta versión también incluye el `Microsoft.Owin.Cors` paquete, que contiene el middleware para procesar las solicitudes HTTP entre orígenes.

> [!NOTE]
> Se quitó la compatibilidad para la firma JWT en la versión final de Visual Studio 2013.

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

Para obtener una lista de características nuevas y otros cambios de Entity Framework 6, consulte [historial de versiones de Entity Framework](https://msdn.com/data/jj574253).

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>ASP.NET Razor 3

ASP.NET Razor 3 incluye las siguientes características nuevas:

- Compatibilidad con la edición de pestaña. Preivously, la **dar formato al documento** comando, aplicar sangría automática y automáticamente el formato de Visual Studio no funcionaron correctamente cuando se usa el **Mantener tabulaciones** opción. Este cambio corrige el formato de código Razor para el formato de tabulación de Visual Studio.
- Compatibilidad con las reglas de reescritura de direcciones URL al generar vínculos.
- Eliminación de atributos de seguridad transparente.
  > [!NOTE]
  > Esto es un cambio brusco y tiene 3 Razor incompatible con MVC4 y versiones anteriores, mientras que Razor 2 no es compatible con MVC5 o los ensamblados compilados con MVC5.

Pueden encontrar problemas de Razor 3 corregidos en Visual Studio 2013 de versiones preliminares [aquí](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>Suspender la aplicación ASP.NET

ASP.NET App Suspend es una característica innovadoras en .NET Framework 4.5.1 que cambia radicalmente la experiencia del usuario y el modelo económico para hospedar el gran número de sitios ASP.NET en un único equipo. Para obtener más información, consulte [ASP.NET App Suspend: capacidad de respuesta de hospedaje de .NET web compartido](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conocidos y los cambios recientes

Esta sección describen problemas conocidos y cambios importantes en las herramientas de Web y ASP.NET para Visual Studio 2013.

### <a name="nuget"></a>NuGet

- [Nueva restauración del paquete no funciona en Mono cuando se usa el archivo SLN](https://nuget.codeplex.com/workitem/3596) – se corregirá en una descarga de las próximas nuget.exe y [NuGet.CommandLine paquete](http://www.nuget.org/packages/NuGet.CommandLine/) actualizar.
- [La restauración del paquete nuevo no funciona con proyectos de Wix](https://nuget.codeplex.com/workitem/3598) – se corregirá en una descarga de las próximas nuget.exe y [NuGet.CommandLine paquete](http://www.nuget.org/packages/NuGet.CommandLine/) actualizar.
- [La restauración del paquete automática no funciona para los proyectos en una carpeta de soluciones](https://nuget.codeplex.com/workitem/3625) – se corregirá en NuGet 2.8.

### <a name="aspnet-web-api"></a>ASP.NET Web API

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)` no devuelve `IQueryable<T>` siempre, tal y como se agregó compatibilidad para `$select` y `$expand`.

    Nuestros ejemplos anteriores para `ODataQueryOptions<T>` convertir siempre el valor devuelto de `ApplyTo` a `IQueryable<T>`. Esto funcionaba anteriormente como opciones de la consulta que se admitían anteriormente (`$filter`, `$orderby`, `$skip`, `$top`) no cambian la forma de la consulta. Ahora que admitimos `$select` y `$expand` el valor devuelto de `ApplyTo` no estará `IQueryable<T>` siempre.

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    Si usas el código de ejemplo de antes, continuará trabajando si el cliente no envía `$select` y `$expand`. Sin embargo, si desea admitir `$select` y `$expand` tendrá que cambiar el código para esto.

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request.Url o RequestContext.Url es null durante una solicitud por lotes**

    En un escenario de procesamiento por lotes, **UrlHelper** es null cuando se tiene acceso desde **Request.Url** o **RequestContext.Url**.

    Este problema actualmente se realiza un seguimiento aquí: [BatchRequestContext.Url es nula para la solicitud de procesamiento por lotes](http://aspnetwebstack.codeplex.com/workitem/1301).

    Para solucionar este problema consiste en crear una nueva instancia de **UrlHelper**, como en el ejemplo siguiente:

    **Crear una nueva instancia de UrlHelper**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. Al utilizar MVC5 y OrgAuth, si tiene vistas que realizar la validación de AntiForgerToken, podría realizar en el siguiente error al enviar datos a la vista:

    **Error**:

    *Error del servidor en la aplicación '/'.*

    <em>Una notificación de tipo '<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>'o'<http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' no estaba presente en el valor de ClaimsIdentity proporcionado. Para habilitar la compatibilidad de token antifalsificación con autenticación basada en notificaciones, compruebe que el proveedor de notificaciones configurada proporciona ambas de estas notificaciones en las instancias de ClaimsIdentity que genera. Si el proveedor de notificaciones configurada en su lugar, utiliza un tipo de notificación diferente como un identificador único, se puede configurar estableciendo la propiedad estática AntiForgeryConfig.UniqueClaimTypeIdentifier.</em>

    **Solución alternativa**:

    Agregue la siguiente línea en Global.asax para corregirlo:

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    Este problema se solucionará en la versión siguiente.
2. Después de actualizar una aplicación MVC4 a MVC5, compile la solución y ábrala. Debería ver el error siguiente:

    [A] No se puede convertir System.Web.WebPages.Razor.Configuration.HostSection [B]System.Web.WebPages.Razor.Configuration.HostSection. Tipo A se origina en ' System.Web.WebPages.Razor, Version = 2.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' en el contexto 'Default' en la ubicación ' C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\ v4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll'. Tipo B se origina en ' System.Web.WebPages.Razor, Version = versión 3.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' en el contexto 'Default' en la ubicación ' C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\ e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll'.

    Para corregir el error anterior, abra *todos los* los archivos Web.config (incluidas las de la carpeta Views) en el proyecto y haga lo siguiente:

   1. Actualizar todas las apariciones de la versión "4.0.0.0" de "System.Web.Mvc" a "5.0.0.0".
   2. Actualizar todas las apariciones de la versión "2.0.0.0" de "System.Web.Helpers", &quot;System.Web.WebPages&quot; y &quot;System.Web.WebPages.Razor&quot; a "3.0.0.0"

      Por ejemplo, después de realizar los cambios mencionados anteriormente, los enlaces de ensamblado deben ser similar al siguiente:

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      Para obtener información sobre cómo actualizar proyectos MVC 4 a 5 de MVC, vea [cómo actualizar un ASP.NET MVC 4 y el proyecto de API Web para ASP.NET MVC 5 y API Web 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).
3. Al utilizar la validación del lado cliente con jQuery validación discreto, el mensaje de validación a veces es correcto para un elemento input de HTML con el tipo = 'number'. El error de validación para un valor requerido ("edad el campo es obligatorio") se muestran cuando se escribe un número no válido en lugar del mensaje correcto que se requiere un número válido.

    Este problema normalmente se encuentra con código con scaffolding para un modelo con una propiedad de entero en las vistas de creación y edición.

    Para solucionar este problema, cambie la aplicación auxiliar de editor de:

    `@Html.EditorFor(person => person.Age)`

    A:

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 ya no es compatible con confianza parcial. Deben quitar proyectos vinculando los archivos binarios de MVC o WebAPI el [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) atributo y el [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) atributo. Cómo quitar estos atributos eliminará errores del compilador como el siguiente.

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > Tenga en cuenta que como un efecto secundario de, no puede utilizar ensamblados 4.0 y 5.0 en la misma aplicación. Debe actualizar todos ellos a 5.0.

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>Plantilla SPA con Facebook autorización puede causar inestabilidad en Internet Explorer mientras el sitio web se hospeda en la zona intranet

La plantilla SPA proporciona registro externo con Facebook. Cuando el proyecto creado con la plantilla se ejecuta localmente, inicio de sesión puede hacer que Internet Explorer que se bloquee.

Solución:

1. Hospedar el sitio web en la zona de internet; o

2. El escenario de prueba en un explorador que no sea Internet Explorer.

### <a name="web-forms-scaffolding"></a>Formularios Web Forms Scaffolding

Scaffolding de Web Forms se ha quitado de VS2013 y estará disponible en una futura actualización a Visual Studio. Sin embargo, todavía puede usar scaffolding dentro de un proyecto de formularios Web Forms agregando las dependencias de MVC y generar la técnica scaffolding para MVC. El proyecto contendrá una combinación de formularios Web Forms y MVC.

Para agregar MVC al proyecto de formularios Web Forms, agregar un nuevo elemento con scaffolding y seleccione **las dependencias de MVC 5**. Seleccione completa según si tiene todos los archivos de contenido, como scripts o mínimo. A continuación, agregue un elemento con scaffolding para MVC, lo cual creará vistas y un controlador en el proyecto.

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC y Web API Scaffolding - 404 de HTTP, no se encontró

Si se produce un error al agregar un elemento con scaffolding para un proyecto, es posible que el proyecto se quedará en un estado incoherente. Algunos de los cambios realizados pueden scaffolding se revertirá pero otros cambios, como los paquetes de NuGet instalados, no se revertirá. Si se deshacen los cambios de configuración de enrutamiento, los usuarios recibirán un error HTTP 404 cuando vaya a elementos de scaffolding.

Solución:

- Para corregir este error para MVC, agregar un nuevo elemento con scaffolding y seleccionar las dependencias de MVC 5 (básica o completa). Este proceso agregará todos los cambios necesarios al proyecto.
- Para corregir este error para la API Web:

  1. Agregue la clase WebApiConfig al proyecto.

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. Configure WebApiConfig.Register en la aplicación\_Start (método) en Global.asax, como se indica a continuación:

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
