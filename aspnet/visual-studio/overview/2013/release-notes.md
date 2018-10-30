---
uid: visual-studio/overview/2013/release-notes
title: ASP.NET y Web Tools para Visual Studio 2013 notas | Microsoft Docs
author: microsoft
description: Este documento describe la versión de ASP.NET y Web Tools para Visual Studio 2013.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: 43878bc101ef97e8bbb6c150f4125707da7660c9
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244962"
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a>ASP.NET y herramientas Web para las notas de la versión de Visual Studio 2013
====================
por [Microsoft](https://github.com/microsoft)

> Este documento describe la versión de ASP.NET y Web Tools para Visual Studio 2013.


## <a name="contents"></a>Contenido

- [Notas de instalación](#TOC1)
- [Documentación](#TOC2)
- [Requisitos de software](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nuevas características de ASP.NET y Web Tools para Visual Studio 2013

- [One ASP.NET](#TOC6)
- [Nueva experiencia de proyecto Web](#newproj)
- [Scaffolding de ASP.NET](#scaffold)
- [Vínculo con exploradores](#browser-link)
- [Mejoras del Editor Web de Visual Studio](#web-editor)
- [Compatibilidad de aplicaciones Web de Azure App Service en Visual Studio](#waws)
- [Mejoras de publicación Web](#publish)
- [NuGet 2.7](#nuget)
- [Formularios Web Forms ASP.NET](#TOC9)
- [ASP.NET MVC 5](#TOC10)
- [ASP.NET Web API 2](#TOC11)
- [SignalR de ASP.NET](#TOC13)
- [ASP.NET Identity](#TOC8)
- [Componentes de Microsoft OWIN](#TOC7)
- [Entity Framework 6](#ef6)
- [3 de Razor de ASP.NET](#TOC14)
- [Suspensión de aplicaciones ASP.NET](#TOC15)
- [Problemas conocidos y cambios importantes](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a>Notas de instalación

ASP.NET y Web Tools para Visual Studio 2013 se agrupan en el programa de instalación principal y puede descargarse [aquí](https://www.asp.net/downloads).

<a id="TOC2"></a>
## <a name="documentation"></a>Documentación

Tutoriales y otra información acerca de ASP.NET y Web Tools para Visual Studio 2013 están disponibles desde el [sitio web de ASP.NET](https://www.asp.net/).

<a id="TOC4"></a>
## <a name="software-requirements"></a>Requisitos de software

ASP.NET y Web Tools requiere Visual Studio 2013.

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a>Nuevas características de ASP.NET y Web Tools para Visual Studio 2013

Las siguientes secciones describen las características que se han introducido en la versión.

<a id="TOC6"></a>
## <a name="one-aspnet"></a>One ASP.NET

Con el lanzamiento de Visual Studio 2013, hemos tomado para asegurarnos de unificar la experiencia de usar tecnologías ASP.NET, por lo que puede mezclar y coincidir con los que desee con facilidad. Por ejemplo, puede iniciar un proyecto de MVC y fácilmente agregar páginas de formularios Web Forms al proyecto más adelante o aplicar la técnica scaffolding las API Web en un proyecto de formularios Web Forms. One ASP.NET es toda la información sobre lo que facilita que los desarrolladores hacer lo que le encanta de ASP.NET. Independientemente de la tecnología que elija, puede tener la confianza que se está compilando en el marco subyacente de confianza de One ASP.NET.

<a id="newproj"></a>
## <a name="new-web-project-experience"></a>Nueva experiencia de proyecto Web

Hemos mejorado la experiencia de creación de nuevos proyectos web en Visual Studio 2013. En el **nuevo proyecto Web de ASP.NET** cuadro de diálogo puede seleccionar el tipo de proyecto que desee, configura cualquier combinación de tecnologías (formularios Web Forms, MVC, Web API), configura opciones de autenticación y agrega un proyecto de prueba unitaria.

![Nuevo proyecto de ASP.NET](release-notes/_static/image1.png)

El nuevo cuadro de diálogo le permite cambiar las opciones de autenticación predeterminado para muchas de las plantillas. Por ejemplo, cuando se crea un proyecto de formularios Web Forms ASP.NET puede seleccionar cualquiera de las siguientes opciones:

- Sin autenticación
- Cuentas de usuario individuales (pertenencia a ASP.NET o registro de proveedor social en)
- Cuentas de organización (Active Directory en una aplicación de internet)
- Autenticación de Windows (Active Directory en una aplicación de intranet)

![Opciones de autenticación](release-notes/_static/image2.png)

Para obtener más información acerca del nuevo proceso para crear proyectos web, consulte [Creating ASP.NET Web Projects in Visual Studio 2013](creating-web-projects-in-visual-studio.md). Para obtener más información acerca de las nuevas opciones de autenticación, consulte [ASP.NET Identity](#TOC8) más adelante en este documento.

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a>Scaffolding de ASP.NET

Scaffolding de ASP.NET es un marco de generación de código para aplicaciones Web ASP.NET. Resulta muy sencillo agregar código reutilizable para el proyecto que interactúa con un modelo de datos.

En versiones anteriores de Visual Studio, estaba limitado a los proyectos de MVC de ASP.NET scaffolding. Con Visual Studio 2013, ahora puede usar el scaffolding para un proyecto ASP.NET, incluidos formularios Web Forms. Visual Studio 2013 no admite actualmente genera páginas para un proyecto de formularios Web Forms, pero todavía puede usar scaffolding con formularios Web Forms mediante la adición de dependencias de MVC al proyecto. Se agregará compatibilidad para generar páginas de formularios Web Forms en una futura actualización.

Cuando se usa la técnica scaffolding, nos aseguramos de que todos los necesarios a las dependencias se instalan en el proyecto. Por ejemplo, si se inicia con un proyecto de formularios Web Forms de ASP.NET y, a continuación, usar el scaffolding para agregar un controlador Web API, los paquetes de NuGet necesarios y las referencias se agregan al proyecto automáticamente.

Para agregar scaffolding de MVC a un proyecto de formularios Web Forms, agregue un **nuevo elemento de scaffolding** y seleccione **dependencias de MVC 5** en la ventana de cuadro de diálogo. Hay dos opciones para scaffolding de MVC; Mínimo y completa. Si selecciona mínima, sólo los paquetes de NuGet y referencias de ASP.NET MVC se agregan al proyecto. Si selecciona la opción completa, se agregan las dependencias mínimas, así como los archivos de contenido necesarios para un proyecto de MVC.

Compatibilidad con controladores asincrónicos de scaffolding usa las nuevas características asincrónicas de Entity Framework 6.

Para obtener más información y tutoriales, consulte [información general sobre el Scaffolding de ASP.NET](aspnet-scaffolding-overview.md).

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a>Vínculo de explorador: SignalR canal entre el explorador y Visual Studio

El nuevo [Browser Link](using-browser-link.md) característica le permite conectar múltiples exploradores con Visual Studio y actualizarlos todos, haga clic en un botón en la barra de herramientas. Puede conectarse a varios exploradores para el sitio de desarrollo, incluidos los emuladores de Windows mobile y haga clic en Actualizar para actualizar todos los exploradores todo al mismo tiempo. Vínculo de explorador también expone una API para permitir a los desarrolladores escribir extensiones de vínculo de explorador.

![](release-notes/_static/image3.png)

Permitiendo a los desarrolladores aprovechar las ventajas de la API de vínculo de explorador, es posible crear escenarios muy avanzados que cruza los límites entre Visual Studio y en cualquier explorador que está conectado. Web Essentials saca partido de la API para crear una experiencia integrada entre Visual Studio y herramientas para desarrolladores del explorador, remotas controlar emuladores móviles y mucho más.

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a>Mejoras del Editor Web de Visual Studio

Visual Studio 2013 incluye un nuevo editor de HTML para los archivos de Razor y HTML en aplicaciones web. El nuevo editor de HTML proporciona un único esquema unificado basado en HTML5. Tiene finalización automática de llaves, la UI de jQuery y AngularJS atributo IntelliSense, la agrupación de IntelliSense, el identificador y nombre de clase de Intellisense y otras mejoras incluyen un mejor rendimiento, formato de atributo y etiquetas inteligentes.

Captura de pantalla siguiente muestra cómo usar Bootstrap atributo IntelliSense en el editor HTML.

![IntelliSense en el editor de HTML](release-notes/_static/image4.png)

Visual Studio 2013 también viene con ambos CoffeeScript y menos editores integrados. El editor LESS viene con todas las increíbles características desde el editor de CSS y tiene Intellisense específico para las variables y mixins en el menor de los documentos de la @import cadena.

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a>Compatibilidad de aplicaciones Web de Azure App Service en Visual Studio

En Visual Studio 2013 con Azure SDK para .NET 2.2, puede usar **Explorador de servidores** interactuar directamente con las aplicaciones web remoto. Puede iniciar sesión en su cuenta de Azure, crear nuevas aplicaciones web, configuración de aplicaciones, ver registros en tiempo real y mucho más. Se libera el próximo poco después de SDK 2.2, podrá ejecutar en modo de depuración remota en Azure. La mayoría de las nuevas características de Azure App Service Web Apps también funciona en Visual Studio 2012 al instalar la versión actual del SDK de Azure para. NET.

Para obtener más información, vea los siguientes recursos:

- [Crear una aplicación web ASP.NET en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [Solución de problemas de una aplicación web en Azure App Service con Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a>Mejoras de publicación Web

Visual Studio 2013 incluye nuevas y mejoradas características de publicación Web. Estos son algunos de ellos:

- Fácilmente [automatizar el cifrado de archivos Web.config](https://go.microsoft.com/fwlink/?LinkId=325529). (Este vínculo y las dos siguientes apuntan a la documentación de MSDN que podría no estar disponible hasta tarde en el día en el 10/17).
- Fácilmente [automatizar poner una aplicación sin conexión durante la implementación](https://go.microsoft.com/fwlink/?LinkId=325530).
- Configurar Web Deploy para [usar checksum de archivo en lugar de cambiar a la última fecha](https://go.microsoft.com/fwlink/?LinkId=325531) para determinar qué archivos deben copiarse al servidor.
- Publicar rápidamente los archivos individuales seleccionados (incluido el archivo Web.config) cuando se usa FTP o métodos de publicación del sistema de archivos así como con Web Deploy.

Para obtener más información sobre la implementación web ASP.NET, vea [el sitio de ASP.NET](https://go.microsoft.com/fwlink/?LinkId=322027).

<a id="nuget"></a>
## <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 incluye un amplio conjunto de nuevas características que se describen en detalle en [notas de la versión de NuGet 2.7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Esta versión de NuGet también elimina la necesidad de dar su consentimiento explícito para la característica de restauración de paquetes de NuGet descargar los paquetes. Ahora se conceden consentimiento (y la casilla de verificación asociada en el cuadro de diálogo de preferencias de NuGet) mediante la instalación de NuGet. Restauración de paquetes simplemente funciona ahora de forma predeterminada.

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a>Formularios Web Forms de ASP.NET

### <a name="one-aspnet"></a>One ASP.NET

Las plantillas de proyecto de formularios Web Forms se integran perfectamente con la nueva experiencia de One ASP.NET. Puede agregar compatibilidad con MVC y Web API a su proyecto de formularios Web Forms, y puede configurar la autenticación mediante el Asistente para la creación del proyecto de One ASP.NET. Para obtener más información, consulte [Creating ASP.NET Web Projects in Visual Studio 2013](creating-web-projects-in-visual-studio.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Las plantillas de proyecto de formularios Web Forms admiten el nuevo marco de ASP.NET Identity. Además, las plantillas ahora admiten la creación de un proyecto de intranet de formularios Web Forms. Para obtener más información, consulte [métodos de autenticación](creating-web-projects-in-visual-studio.md#auth) en **Creating ASP.NET Web Projects in Visual Studio 2013**.

### <a name="bootstrap"></a>Bootstrap

Usan las plantillas de formularios Web Forms [Bootstrap](http://twitter.github.io/bootstrap/) para proporcionar una capacidad de respuesta y elegante apariencia que se puede personalizar fácilmente. Para obtener más información, consulte [Bootstrap en las plantillas de proyecto web de Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a>ASP.NET MVC 5

### <a name="one-aspnet"></a>One ASP.NET

Las plantillas de proyecto Web MVC se integran perfectamente con la nueva experiencia de One ASP.NET. Puede personalizar el proyecto MVC y configurar la autenticación mediante el Asistente para la creación del proyecto de One ASP.NET. Encontrará un tutorial de introducción a ASP.NET MVC 5 en [Introducción a ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Para obtener información sobre cómo actualizar proyectos de MVC 4 a MVC 5, vea [cómo actualizar un ASP.NET MVC 4 y un proyecto de API Web a ASP.NET MVC 5 y Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Las plantillas de proyecto MVC se actualizaron para usar ASP.NET Identity para la autenticación y administración de identidades. Encontrará un tutorial que incluye autenticación de Facebook y Google y la nueva API de pertenencia en [crear una aplicación de ASP.NET MVC 5 con Facebook y Google OAuth2 y OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) y [crear una aplicación MVC de ASP.NET con la autenticación y Base de datos SQL e implementar en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).

### <a name="bootstrap"></a>Bootstrap

La plantilla de proyecto MVC se ha actualizado para usar [Bootstrap](http://getbootstrap.com/) para proporcionar una capacidad de respuesta y elegante apariencia que se puede personalizar fácilmente. Para obtener más información, consulte [Bootstrap en las plantillas de proyecto web de Visual Studio 2013](creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtros de autenticación

Los filtros de autenticación son un nuevo tipo de filtro de ASP.NET MVC que se ejecutan antes de los filtros de autorización en la canalización de ASP.NET MVC y le permiten especificar la lógica de los autenticación por acción, por controlador o globalmente para todos los controladores. Los filtros de autenticación procesan credenciales en la solicitud y proporcionan una entidad de seguridad correspondiente. Los filtros de autenticación también pueden complicar el desafío de autenticación en respuesta a las solicitudes no autorizadas.

### <a name="filter-overrides"></a>Invalidaciones de filtro

Ahora puede reemplazar los filtros que se aplican a un método de acción determinado o un controlador mediante la especificación de un filtro de invalidación. Filtros de reemplazo especifican un conjunto de tipos de filtro que no se debe ejecutar para un ámbito determinado (acción o controlador). Esto le permite configurar los filtros que se aplican globalmente y, a continuación, exclusión determinados filtros globales de aplicar a controladores o acciones específicas.

### <a name="attribute-routing"></a>Enrutamiento mediante atributos

ASP.NET MVC ahora admite el enrutamiento mediante atributos, gracias a una contribución de Tim McCall, el autor de [ http://attributerouting.net ](http://attributerouting.net). Con el enrutamiento mediante atributos puede especificar sus rutas anotando las acciones y los controladores.

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a>ASP.NET Web API 2

### <a name="attribute-routing"></a>Enrutamiento mediante atributos

ASP.NET Web API ahora admite el enrutamiento mediante atributos, gracias a una contribución de Tim McCall, el autor de [ http://attributerouting.net ](http://attributerouting.net). Con el enrutamiento mediante atributos puede especificar las rutas de API Web anotando las acciones y los controladores similar al siguiente:

[!code-csharp[Main](release-notes/samples/sample1.cs)]

Enrutamiento mediante atributos proporciona mayor control sobre los URI de la API web. Por ejemplo, puede definir fácilmente una jerarquía de recursos mediante un único controlador de API:

[!code-csharp[Main](release-notes/samples/sample2.cs)]

Enrutamiento mediante atributos también proporciona una sintaxis adecuada para especificar los parámetros opcionales, valores predeterminados y las restricciones de ruta:

[!code-csharp[Main](release-notes/samples/sample3.cs)]

Para obtener más información sobre el enrutamiento mediante atributos, vea [enrutamiento mediante atributos en Web API 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).

### <a name="oauth-20"></a>OAuth 2.0

Las plantillas de proyecto Web API y aplicación de página única ahora admiten la autorización mediante OAuth 2.0. OAuth 2.0 es un marco para autorizar el acceso de cliente a los recursos protegidos. Funciona con una variedad de clientes, incluidos exploradores y dispositivos móviles.

Compatibilidad con OAuth 2.0 se basa en el nuevo middleware de seguridad proporcionadas por los componentes de OWIN de Microsoft para la autenticación de portador e implementar el rol de servidor de autorización. Como alternativa, los clientes se pueden autorizar mediante un servidor de autorización de organización, como Azure Active Directory o AD FS en Windows Server 2012 R2.

### <a name="odata-improvements"></a>Mejoras de OData

**Expanda el soporte técnico para $select, $, $batch y $value**

ASP.NET Web API OData ahora es totalmente compatible con para $select, $expand y $value. También puede usar $batch de solicitud de procesamiento por lotes y el procesamiento de conjuntos de cambios.

El $select y $expand expanden las opciones le permiten cambiar la forma de los datos que se devuelven desde un punto de conexión de OData. Para obtener más información, consulte [Introducing $select y $expand amplían la compatibilidad de Web API OData](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).

**Extensibilidad mejorada**

Los formateadores de OData ahora son extensibles. Puede agregar metadatos de entrada de Atom, admiten entradas de vínculo multimedia y de secuencia con nombre, agregar anotaciones de instancia y personalizar cómo se generan los vínculos.

**Soporte técnico sin tipo**

Ahora puede crear servicios de OData sin necesidad de definir tipos de CLR para los tipos de entidad. En su lugar, pueden tomar o devuelven instancias de los controladores de OData **IEdmObject**, que son los formateadores de OData serializar/deserializar.

**Volver a usar un modelo existente**

Si ya tiene un entity data model (EDM) existente, ahora reutilizarlo directamente, en lugar de tener que crear uno nuevo. Por ejemplo, si usa Entity Framework, puede usar el EDM que EF crea para usted.

### <a name="request-batching"></a>Solicitud de procesamiento por lotes

Solicitud de procesamiento por lotes combina varias operaciones en una sola solicitud HTTP POST, para reducir el tráfico de red y proporcionar un Suavizador, menos la interfaz de usuario más activas. ASP.NET Web API ahora admite varias estrategias para la solicitud de procesamiento por lotes:

- Utilice el punto de conexión de $batch de un servicio de OData.
- Empaquetar varias solicitudes en una única solicitud de varias partes MIME.
- Usar un formato de procesamiento por lotes personalizado.

Para habilitar la solicitud de procesamiento por lotes, basta con agregar una ruta con un controlador de procesamiento por lotes a la configuración de la API Web:

[!code-csharp[Main](release-notes/samples/sample4.cs)]

También puede controlar si las solicitudes o ejecuta secuencialmente o en cualquier orden.

### <a name="portable-aspnet-web-api-client"></a>Portátil ASP.NET Web API de cliente

Ahora puede usar al cliente de ASP.NET Web API para crear bibliotecas de clases portables que funcionan a través de las aplicaciones de Windows Store y Windows Phone 8. También puede crear a formateadores portátiles que se pueden compartir entre cliente y servidor.

### <a name="improved-testability"></a>Capacidad de prueba mejorada

Web API 2 hace mucho más fácil unidad probar los controladores de API. Apenas el controlador de API con el mensaje de solicitud y la configuración y, a continuación, llamar al método de acción que desea probar. También es fácil simular el **UrlHelper** (clase), para los métodos de acción que realizar la generación del vínculo.

### <a name="ihttpactionresult"></a>IHttpActionResult

Ahora puede implementar IHttpActionResult para encapsular el resultado de los métodos de acción de la API Web. El tiempo de ejecución de ASP.NET Web API para generar el mensaje de respuesta resultante se ejecuta un IHttpActionResult devuelto desde un método de acción de Web API. Se puede devolver un IHttpActionResult desde cualquier acción de la API Web para simplificar la unidad de las pruebas de la implementación de la API Web. Para mayor comodidad que se proporcionan varias implementaciones IHttpActionResult incluyendo resultados para devolver los códigos de estado específicos, con el formato contenidas o contenido que se negocia las respuestas.

### <a name="httprequestcontext"></a>HttpRequestContext

El nuevo **HttpRequestContext** realiza un seguimiento de cualquier estado que está asociado a la solicitud pero no está inmediatamente disponible en la solicitud. Por ejemplo, puede usar el **HttpRequestContext** para obtener datos de ruta, la entidad de seguridad asociado a la solicitud, el certificado de cliente, el **UrlHelper** y la raíz de la ruta de acceso virtual. Puede crear fácilmente un **HttpRequestContext** con fines de prueba de unidad.

Dado que la entidad de seguridad para la solicitud se ajusta con la solicitud en lugar de depender **Thread.CurrentPrincipal**, la entidad de seguridad ahora está disponible durante la vigencia de la solicitud mientras está en la canalización Web API.

### <a name="cors"></a>CORS

Gracias a la otra gran contribución de Brock Allen, ASP.NET ahora es totalmente compatible con origen solicitar uso compartido entre (CORS).

La seguridad del explorador impide que una página web realice solicitudes AJAX a otro dominio. [CORS](http://www.w3.org/TR/cors/) es una norma de W3C que permite que un servidor a ser menos exigentes con la directiva de mismo origen. Mediante CORS, un servidor puede permitir explícitamente algunas solicitudes entre orígenes y rechazar otras.

Web API 2 admite ahora la CORS, incluido el control automático de las solicitudes anteriores. Para obtener más información, consulte [habilitación de solicitudes entre orígenes en ASP.NET Web API](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).

### <a name="authentication-filters"></a>Filtros de autenticación

Los filtros de autenticación son un nuevo tipo de filtro de ASP.NET Web API que se ejecutan antes de los filtros de autorización en la canalización de ASP.NET Web API y le permiten especificar la lógica de los autenticación por acción, por controlador o globalmente para todos los controladores. Los filtros de autenticación procesan credenciales en la solicitud y proporcionan una entidad de seguridad correspondiente. Los filtros de autenticación también pueden complicar el desafío de autenticación en respuesta a las solicitudes no autorizadas.

### <a name="filter-overrides"></a>Invalidaciones de filtro

Ahora puede reemplazar los filtros que se aplican a un método de acción determinado o un controlador, mediante la especificación de un filtro de invalidación. Filtros de reemplazo especifican un conjunto de tipos de filtro que no se debe ejecutar para un ámbito determinado (acción o controlador). Esto le permite agregar filtros globales, pero, a continuación, excluir algunos de los controladores o acciones específicas.

### <a name="owin-integration"></a>Integración de OWIN

ASP.NET Web API ahora totalmente es compatible con OWIN y se puede ejecutar en cualquier host compatible con OWIN. También se incluye un **HostAuthenticationFilter** que proporciona integración con el sistema de autenticación OWIN.

Con la integración de OWIN, puede probarlo internamente API Web en su propio proceso junto con otro middleware OWIN, como SignalR. Para obtener más información, consulte [Use OWIN para Self ASP.NET Web API](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a>SignalR de ASP.NET 2.0

Las secciones siguientes describen las características de SignalR 2.0.

- [Basado en OWIN](#builtonowin)
- [MapHubs y MapConnection ahora son MapSignalR](#MapSignalR)
- [Compatibilidad entre dominios](#crossdomain)
- [soportan de iOS y Android mediante MonoTouch y MonoDroid](#mobile)
- [Cliente .NET portátil](#portable)
- [Nuevo paquete de autohospedaje](#selfhost)
- [Compatibilidad de servidor compatible con versiones anteriores](#backwardcompat)
- [Ha quitado la compatibilidad de servidor para .NET 4.0](#remove40)
- [Enviar un mensaje a una lista de clientes y grupos](#messagelist)
- [Enviar un mensaje a un usuario específico](#sendtouser)
- [Mejor compatibilidad del control de errores](#errorhandling)
- [Unidad más sencillas las pruebas de concentradores](#unittesting)
- [Control de errores de JavaScript](#javascripterror)

Para obtener un ejemplo de cómo actualizar un proyecto existente de 1.x a 2.0 de SignalR, consulte [actualizar un SignalR 1.x proyecto](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).

<a id="builtonowin"></a>
### <a name="built-on-owin"></a>Basado en OWIN

SignalR 2.0 se basa completamente en [OWIN (la interfaz Web abierta para .NET)](http://owin.org/). Este cambio hace que el proceso de instalación para SignalR mucho más coherente entre aplicaciones SignalR autohospedadas y hospedado en web, pero también requiere un número de cambios en la API.

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a>MapHubs y MapConnection ahora son MapSignalR

Por compatibilidad con los estándares OWIN, estos métodos se han cambiado a `MapSignalR`. `MapSignalR` llama sin parámetros asignarán todos los concentradores (como `MapHubs` en la versión 1.x); para asignar individuales **PersistentConnection** objetos, especifique el tipo de conexión como parámetro de tipo y la extensión de dirección URL para la conexión como el primer argumento.

El `MapSignalR` se llama al método en una clase de inicio de Owin. Visual Studio 2013 contiene una nueva plantilla para una clase de inicio Owin; Para usar esta plantilla, haga lo siguiente:

1. Haga doble clic en el proyecto
2. Seleccione **agregar**, **nuevo elemento...**
3. Seleccione **clase Owin Startup**. Nombre de la nueva clase **Startup.cs**.

En un **la aplicación Web,** la clase de inicio Owin que contiene el `MapSignalR` método, a continuación, se agrega al proceso de inicio de Owin mediante una entrada en el nodo de configuración de aplicación del archivo Web.Config, tal como se muestra a continuación.

En un **autohospedado aplicación**, la clase de inicio se pasa como parámetro de tipo de la `WebApp.Start` método.

**Asignación de concentradores y conexiones en SignalR 1.x (desde el archivo de aplicación global en una aplicación web):** 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

**Asignación de concentradores y conexiones en SignalR 2.0 (desde un archivo de clase de inicio Owin):** 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

En un **autohospedado aplicación**, la clase de inicio se pasa como parámetro de tipo para el `WebApp.Start` método tal y como se muestra a continuación.

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a>Compatibilidad entre dominios

En SignalR 1.x solicitudes entre dominios se controla mediante una única marca EnableCrossDomain. Este indicador controla las solicitudes de JSONP y CORS. Para una mayor flexibilidad, compatibilidad con CORS todos los ha quitado el componente de servidor de SignalR (JavaScript lients todavía usa CORS normalmente si se detecta que el explorador lo admite), y middleware OWIN nuevo está disponible admitir estos escenarios.

En SignalR 2.0, es necesario si JSONP en el cliente (para admitir las solicitudes entre dominios en los exploradores más antiguos), deberá habilitarse explícitamente estableciendo `EnableJSONP` en el `HubConfiguration` objeto `true`, tal y como se muestra a continuación. JSONP está deshabilitada de forma predeterminada, ya que es menos segura que la CORS.

Para agregar el nuevo middleware de CORS en SignalR 2.0, agregue el `Microsoft.Owin.Cors` biblioteca al proyecto y llamada `UseCors` antes de su middleware de SignalR, tal como se muestra en la sección siguiente.

**Agregar Microsoft.Owin.Cors a su proyecto**: para instalar esta biblioteca, ejecute el siguiente comando en la consola de administrador de paquetes:

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

Este comando agregará la 2.0.0 versión del paquete al proyecto.

**Llamar a UseCors**

Los fragmentos de código siguientes muestran cómo implementar las conexiones entre dominios en SignalR 1.x y 2.0.

**Implementación de las solicitudes entre dominios en SignalR 1.x (desde el archivo de aplicación global)**

[!code-csharp[Main](release-notes/samples/sample9.cs)]

**Implementación de las solicitudes entre dominios en SignalR 2.0 (desde un archivo de código de C#)**

El código siguiente muestra cómo se habilita la CORS o JSONP en un proyecto de SignalR 2.0. Este ejemplo de código usa `Map` y `RunSignalR` en lugar de `MapSignalR`, de modo que el middleware de CORS se ejecuta solo para las solicitudes de SignalR que requieren compatibilidad con CORS (en lugar de para todo el tráfico en la ruta de acceso especificada en `MapSignalR`.) `Map` también se puede usar para cualquier otro middleware que debe ejecutarse para un prefijo de dirección URL específico, en lugar de toda la aplicación.

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a>soportan de iOS y Android mediante MonoTouch y MonoDroid

Se ha agregado compatibilidad con clientes iOS y Android mediante componentes MonoTouch y MonoDroid desde el [biblioteca Xamarin](https://xamarin.com/). Para obtener más información sobre cómo usarlas, vea [utilizando componentes de Xamarin](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln). Estos componentes estarán disponibles en el [Xamarin Store](https://store.xamarin.com/) cuando está disponible la versión RTW de SignalR.

<a id="portable"></a> ### Portable cliente .NET

Mejor facilitar el desarrollo multiplataforma, el Silverlight, WinRT y los clientes de Windows Phone se han reemplazado con un solo cliente portátil de .NET que admite las siguientes plataformas:

- NET 4.5
- Silverlight 5
- WinRT (.NET para aplicaciones de Windows Store)
- Windows Phone 8

<a id="selfhost"></a>

### <a name="new-self-host-package"></a>Nuevo paquete de autohospedaje

Ahora hay un paquete de NuGet para que resulte más fácil empezar a trabajar con interna de SignalR (SignalR las aplicaciones que se hospedan en un proceso u otra aplicación, en lugar de que se hospeda en un servidor web). Para actualizar un proyecto de autohospedaje con SignalR 1.x, quitar el paquete Microsoft.AspNet.SignalR.Owin y agregue el paquete Microsoft.AspNet.SignalR.SelfHost. Para obtener más información sobre cómo comenzar con el paquete de autohospedaje, vea [Tutorial: interna de SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a>Compatibilidad de servidor compatible con versiones anteriores

En versiones anteriores de SignalR, las versiones de paquete SignalR que se usa en el cliente y el servidor deben ser idénticos. Para admitir aplicaciones de cliente pesada que serían difíciles de actualizar, SignalR 2.0 ahora admite el uso de una versión más reciente del servidor con un cliente anterior. **Nota: SignalR 2.0 no admite servidores creados con versiones anteriores con clientes de versiones más recientes.**

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a>Ha quitado la compatibilidad de servidor para .NET 4.0

SignalR 2.0 quitó la compatibilidad para la interoperabilidad de servidor con .NET 4.0. .NET 4.5 se debe usar con servidores SignalR 2.0. Todavía hay un cliente de .NET 4.0 para SignalR 2.0.

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a>Enviar un mensaje a una lista de clientes y grupos

En SignalR 2.0, es posible enviar un mensaje con una lista de identificadores de grupo y cliente. Los siguientes fragmentos de código muestran cómo hacerlo.

**Enviar un mensaje a una lista de los clientes y grupos mediante PersistentConnection**

[!code-csharp[Main](release-notes/samples/sample11.cs)]

**Enviar un mensaje a una lista de clientes y grupos con concentradores**

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a>Enviar un mensaje a un usuario específico

Esta característica permite a los usuarios especificar lo que es el identificador de usuario en función de un objeto IRequest a través de una nueva interfaz IUserIdProvider:

**La interfaz IUserIdProvider**

[!code-csharp[Main](release-notes/samples/sample13.cs)]

De forma predeterminada será una implementación que usa IPrincipal.Identity.Name del usuario como el nombre de usuario.

En los concentradores, podrá enviar mensajes a estos usuarios a través de una API nueva:

**Uso de la API Clients.User**

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a>Mejor compatibilidad del control de errores

Los usuarios ahora pueden generar **HubException** de cualquier invocación de concentrador. El constructor de la **HubException** puede tardar un mensaje de cadena y un objeto de datos de error adicionales. SignalR se auto-serializar la excepción y enviarlo al cliente donde se usará para la invocación del método de concentrador de rechazo o incorrecto.

El **mostrar excepciones detallados de concentrador** configuración no influye en **HubException** que se envían al cliente o no; siempre se envíe.

**Código de servidor que muestra un HubException enviar al cliente**

[!code-csharp[Main](release-notes/samples/sample15.cs)]

**Código de cliente de JavaScript que muestra responde a un HubException enviado desde el servidor**

[!code-html[Main](release-notes/samples/sample16.html)]

**Código de cliente de .NET que muestra responde a un HubException enviado desde el servidor**

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a>Unidad más sencillas las pruebas de concentradores

SignalR 2.0 incluye una interfaz denominada `IHubCallerConnectionContext` en los centros que resulta más fácil crear las invocaciones de lado de cliente ficticio. Los fragmentos de código siguiente se muestra cómo usar esta interfaz con agentes de pruebas populares [xUnit.net](https://github.com/xunit/xunit) y [moq](https://code.google.com/p/moq/).

**Pruebas unitarias de SignalR con xUnit.net**

[!code-csharp[Main](release-notes/samples/sample18.cs)]

**Pruebas unitarias de SignalR con moq**

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a>Control de errores de JavaScript

En SignalR 2.0, todas las devoluciones de llamada del control de errores de JavaScript devuelven objetos de error de JavaScript en lugar de cadenas sin formato. Esto permite SignalR al flujo de información más completa para los controladores de error. Puede obtener la excepción interna de la `source` propiedad del error.

**Código de cliente de JavaScript que controla la excepción Start.Fail**

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a>ASP.NET Identity

### <a name="new-aspnet-membership-system"></a>Nuevo sistema de pertenencia ASP.NET

ASP.NET Identity es el nuevo sistema de pertenencia para las aplicaciones ASP.NET. ASP.NET Identity facilita integrar datos de perfil de usuario específica con datos de la aplicación. ASP.NET Identity permite elegir el modelo de persistencia para perfiles de usuario en la aplicación. Puede almacenar los datos en una base de datos de SQL Server o en otro almacén de datos, incluidos los almacenes de datos NoSQL, como tablas de Azure Storage. Para obtener más información, consulte [cuentas de usuario individuales](creating-web-projects-in-visual-studio.md#indauth) en **Creating ASP.NET Web Projects in Visual Studio 2013**.

### <a name="claims-based-authentication"></a>Autenticación basada en notificaciones

ASP.NET ahora admite la autenticación basada en notificaciones, donde la identidad del usuario se representa como un conjunto de notificaciones de un emisor de confianza. Los usuarios pueden ser autenticado con un nombre de usuario y la contraseña que se mantienen en una base de datos de aplicación o con proveedores de identidades sociales (por ejemplo: Accounts de Microsoft, Facebook, Google, Twitter), o mediante cuentas profesionales a través de Azure Active Directory o Servicios de federación de Active Directory (ADFS).

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a>Integración con Azure Active Directory y Active Directory de Windows Server

Ahora puede crear proyectos de ASP.NET que usan Azure Active Directory o Windows Server Active Directory (AD) para la autenticación. Para obtener más información, consulte [cuentas organizativas](creating-web-projects-in-visual-studio.md#orgauth) en **Creating ASP.NET Web Projects in Visual Studio 2013**.

### <a name="owin-integration"></a>Integración de OWIN

Autenticación de ASP.NET se basa ahora en middleware OWIN que se puede usar en cualquier host basado en OWIN. Para obtener más información acerca de OWIN, vea el siguiente [componentes de Microsoft OWIN](#TOC7) sección.

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a>Componentes de Microsoft OWIN

[Interfaz Web abierta para .NET](http://owin.org/) (OWIN) define una abstracción entre los servidores web de .NET y aplicaciones web. OWIN desacopla la aplicación web desde el servidor, realizar independiente del host de aplicaciones web. Por ejemplo, puede hospedar una aplicación web basada en OWIN en IIS o autohospedar en un proceso personalizado.

Los cambios introducidos en los componentes de OWIN de Microsoft (también conocido como el proyecto Katana) incluyen nuevos componentes de servidor y el host, nuevas bibliotecas auxiliares y middleware y middleware de autenticación de nuevo.

Para obtener más información acerca de OWIN y Katana, consulte [Novedades de OWIN y Katana](../../../aspnet/overview/owin-and-katana/index.md).

**Nota: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) no se pueden ejecutar aplicaciones en modo clásico de IIS; se debe ejecutar en el modo integrado.**

**Nota: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) las aplicaciones se deben ejecutar con plena confianza.**

### <a name="new-servers-and-hosts"></a>Hosts y servidores nuevos

Con esta versión, se agregaron nuevos componentes para habilitar escenarios de autohospedaje. Estos componentes incluyen los siguientes paquetes NuGet:

- **Microsoft.Owin.Host.HttpListener**. Proporciona un servidor OWIN que utiliza **HttpListener** para escuchar las solicitudes HTTP y dirigirlos a la canalización OWIN.
- **Microsoft.Owin.Hosting** proporciona una biblioteca para los desarrolladores que quieran para hospedar automáticamente una canalización de OWIN en un proceso personalizado, como una aplicación de consola o el servicio de Windows.
- **OwinHost**. Proporciona un archivo ejecutable independiente que encapsula `Microsoft.Owin.Hosting` y le permite hospedar automáticamente una canalización de OWIN sin tener que escribir una aplicación de host personalizado.

Además, el `Microsoft.Owin.Host.SystemWeb` paquete ahora permite middleware para proporcionar sugerencias a la **SystemWeb** server, que indica que el middleware debe llamarse durante una fase de canalización ASP.NET específica. Esta característica es especialmente útil para el middleware de autenticación, que se debe ejecutar al principio de la canalización de ASP.NET.

### <a name="helper-libraries-and-middleware"></a>Middleware y bibliotecas auxiliares

Aunque puede escribir componentes de OWIN mediante sólo las definiciones de función y el tipo de la especificación OWIN, el nuevo `Microsoft.Owin` paquete proporciona un conjunto de abstracciones más fáciles de usar. Este paquete combina varios paquetes anteriores (p. ej., `Owin.Extensions`, `Owin.Types`) en un modelo de objeto único y bien estructurada que, a continuación, se puede usar fácilmente para otros componentes OWIN. De hecho, la mayoría de los componentes de Microsoft OWIN ahora usan este paquete.

> [!NOTE]
> [OWIN](http://www.owin.org) no se pueden ejecutar aplicaciones en modo clásico de IIS; se debe ejecutar en el modo integrado.

> [!NOTE]
> [OWIN](http://www.owin.org) las aplicaciones se deben ejecutar con plena confianza.

Esta versión también incluye el paquete Microsoft.Owin.Diagnostics, que incluye middleware para validar una aplicación OWIN en ejecución, además de middleware de la página de error para ayudar a investigar los errores.

### <a name="authentication-components"></a>Componentes de autenticación

Los siguientes componentes de autenticación están disponibles.

- **Microsoft.Owin.Security.ActiveDirectory**. Habilita la autenticación mediante servicios de directorio local o en la nube.
- **Microsoft.Owin.Security.Cookies** habilita la autenticación con cookies. Este paquete se llamaba `Microsoft.Owin.Security.Forms`.
- **Microsoft.Owin.Security.Facebook** habilita la autenticación mediante el servicio basado en OAuth de Facebook.
- **Microsoft.Owin.Security.Google** habilita la autenticación mediante el servicio de Google OpenID.
- **Microsoft.Owin.Security.Jwt** habilita la autenticación mediante tokens de JWT.
- **Microsoft.Owin.Security.MicrosoftAccount** habilita la autenticación mediante cuentas de Microsoft.
- **Microsoft.Owin.Security.OAuth**. Proporciona un servidor de autorización de OAuth, así como el middleware para autenticar los tokens de portador.
- **Microsoft.Owin.Security.Twitter** habilita la autenticación mediante el servicio de basado en OAuth de Twitter.

Esta versión también incluye el `Microsoft.Owin.Cors` paquete, que contiene el software intermedio para procesar las solicitudes HTTP entre orígenes.

> [!NOTE]
> Se ha quitado la compatibilidad para la firma JWT en la versión final de Visual Studio 2013.

<a id="ef6"></a>
## <a name="entity-framework-6"></a>Entity Framework 6

Para obtener una lista de las nuevas características y otros cambios de Entity Framework 6, consulte [historial de versiones de Entity Framework](https://msdn.com/data/jj574253).

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a>3 de Razor de ASP.NET

ASP.NET Razor 3 incluye las siguientes características nuevas:

- Compatibilidad con la edición de ficha. Anteriormente, el **dar formato al documento** comando sangría automática y automática de formato en Visual Studio no ha funcionado correctamente cuando se usa el **Mantener tabulaciones** opción. Este cambio corrige el formato para el código Razor para el formato de tabulación de Visual Studio.
- Compatibilidad con las reglas de reescritura de direcciones URL al generar vínculos.
- Eliminación de atributo transparente de seguridad.
  > [!NOTE]
  > Esto es un cambio importante y hace que sea Razor 3 incompatible con MVC4 y versiones anteriores, aunque no es compatible con MVC5 o los ensamblados compilados en MVC5 Razor 2.

Pueden encontrar problemas de Razor 3 corregidos en Visual Studio 2013 desde versiones preliminares [aquí](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a>Suspensión de aplicaciones ASP.NET

ASP.NET App Suspend es una característica innovadora en .NET Framework 4.5.1 que cambia radicalmente la experiencia del usuario y el modelo económico para hospedar grandes cantidades de sitios ASP.NET en un equipo único. Para obtener más información, consulte [ASP.NET App Suspend – con capacidad de respuesta de hospedaje de .NET web compartido](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conocidos y cambios importantes

Esta sección describen problemas conocidos y cambios importantes en ASP.NET y Web Tools para Visual Studio 2013.

### <a name="nuget"></a>NuGet

- [Nueva restauración de paquetes no funciona en Mono, cuando se usa el archivo SLN](https://nuget.codeplex.com/workitem/3596) – se corregirá en una descarga de nuget.exe próximos y [NuGet.CommandLine paquete](http://www.nuget.org/packages/NuGet.CommandLine/) actualizar.
- [Nueva restauración de paquetes no funciona con proyectos de Wix](https://nuget.codeplex.com/workitem/3598) – se corregirá en una descarga de nuget.exe próximos y [NuGet.CommandLine paquete](http://www.nuget.org/packages/NuGet.CommandLine/) actualizar.
- [La restauración automática del paquete no funciona para los proyectos en una carpeta de soluciones](https://nuget.codeplex.com/workitem/3625) – se corregirá en NuGet 2.8.

### <a name="aspnet-web-api"></a>ASP.NET Web API

1. `ODataQueryOptions<T>.ApplyTo(IQueryable)` no devuelve `IQueryable<T>` siempre, tal como se ha agregado compatibilidad para `$select` y `$expand`.

    Los ejemplos anteriores para `ODataQueryOptions<T>` convertir siempre el valor devuelto desde `ApplyTo` a `IQueryable<T>`. Esto funcionaba anteriormente porque las opciones de la consulta que se admite anteriormente (`$filter`, `$orderby`, `$skip`, `$top`) no cambian la forma de la consulta. Ahora que admitimos `$select` y `$expand` el valor devuelto de `ApplyTo` no estará `IQueryable<T>` siempre.

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    Si usa el código de ejemplo anterior, continuará trabajando si el cliente no envía `$select` y `$expand`. Sin embargo, si desea admitir `$select` y `$expand` tendrá que cambiar el código para esto.

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. **Request.Url o RequestContext.Url es null durante una solicitud por lotes**

    En un escenario de procesamiento por lotes, **UrlHelper** es null cuando se tiene acceso desde **Request.Url** o **RequestContext.Url**.

    Este problema está realizando un seguimiento actualmente aquí: [BatchRequestContext.Url es null para la solicitud de procesamiento por lotes](http://aspnetwebstack.codeplex.com/workitem/1301).

    La solución para este problema consiste en crear una nueva instancia de **UrlHelper**, como en el ejemplo siguiente:

    **Crear una nueva instancia de UrlHelper**

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a>ASP.NET MVC

1. Al usar MVC5 y OrgAuth, si tiene las vistas que realiza la validación de AntiForgerToken, puede que encuentre el siguiente error al publicar datos a la vista:

    **Error**:

    *Error del servidor en la aplicación '/'.*

    <em>Una notificación del tipo '<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>'o'<http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' no estaba presente en la ClaimsIdentity proporcionado. Para habilitar la compatibilidad de token antifalsificación con la autenticación basada en notificaciones, compruebe que el proveedor de notificaciones configurada proporciona ambas de estas notificaciones en las instancias de ClaimsIdentity genera. Si el proveedor de notificaciones configurada en su lugar, utiliza un tipo de notificación diferente como identificador único, se puede configurar estableciendo la propiedad estática AntiForgeryConfig.UniqueClaimTypeIdentifier.</em>

    **Solución alternativa**:

    Agregue la siguiente línea en Global.asax para corregirlo:

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    Este problema se corregirá en la próxima versión.
2. Después de actualizar una aplicación de MVC4 a MVC5, compile la solución e inícielo. Debería ver el error siguiente:

    [A] No puede convertirse System.Web.WebPages.Razor.Configuration.HostSection [B]System.Web.WebPages.Razor.Configuration.HostSection. Tipo A se origina desde ' System.Web.WebPages.Razor, Version = 2.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' en el contexto "Default" en la ubicación ' C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\ v4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll'. Tipo B se origina en ' System.Web.WebPages.Razor, Version = 3.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' en el contexto "Default" en la ubicación ' C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\ e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll'.

    Para corregir el error anterior, abra *todas* los archivos Web.config (incluidos los que aparecen en la carpeta Views) en el proyecto y haga lo siguiente:

   1. Actualizar todas las apariciones de la versión "4.0.0.0" de "System.Web.Mvc" a "5.0.0.0".
   2. Actualizar todas las apariciones de la versión "2.0.0.0" de "System.Web.Helpers," &quot;System.Web.WebPages&quot; y &quot;System.Web.WebPages.Razor&quot; a "3.0.0.0"

      Por ejemplo, después de realizar los cambios anteriores, los enlaces de ensamblado deben presentar este aspecto:

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      Para obtener información sobre cómo actualizar proyectos de MVC 4 a MVC 5, vea [cómo actualizar un ASP.NET MVC 4 y un proyecto de API Web a ASP.NET MVC 5 y Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).
3. Al usar la validación del lado cliente con validación discreta de jQuery, el mensaje de validación a veces es incorrecto para un elemento input de HTML con el tipo = 'number'. El error de validación para un valor requerido ("campo de la edad es necesario") se muestran cuando se especifica un número no válido en lugar del mensaje correcto que se requiere un número válido.

    Este problema normalmente se encuentra con el código con scaffolding para un modelo con una propiedad de entero en las vistas Create y Edit.

    Para solucionar este problema, cambie la aplicación auxiliar de editor:

    `@Html.EditorFor(person => person.Age)`

    A:

    `@Html.TextBoxFor(person => person.Age)`
4. ASP.NET MVC 5 ya no es compatible con confianza parcial. Los proyectos a los archivos binarios MVC o API Web de vinculación deben quitar la [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) atributo y el [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) atributo. Quitar estos atributos eliminará los errores del compilador como el siguiente.

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > Tenga en cuenta, como un efecto secundario de este que no se puede usar ensamblados 4.0 y 5.0 en la misma aplicación. Deberá actualizar todos ellos a 5.0.

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a>Plantilla SPA con Facebook autorización puede provocar inestabilidad en Internet Explorer mientras el sitio web se hospeda en la zona de intranet

La plantilla SPA proporciona registro externo con Facebook. Cuando el proyecto creado con la plantilla se ejecuta localmente, la firma puede provocar IE se bloquee.

Solución:

1. Hospedar el sitio web en la zona de internet; o

2. El escenario de prueba en un explorador que no sea Internet Explorer.

### <a name="web-forms-scaffolding"></a>Formularios Web Forms Scaffolding

Web Forms Scaffolding se ha quitado de VS2013 y estará disponible en una futura actualización para Visual Studio. Sin embargo, todavía puede usar scaffolding dentro de un proyecto de formularios Web Forms al agregar dependencias de MVC y generar scaffolding de MVC. El proyecto contendrá una combinación de formularios Web Forms y MVC.

Para agregar MVC a su proyecto de formularios Web Forms, agregue un nuevo elemento con scaffolding y seleccione **dependencias de MVC 5**. Seleccione completa según si tiene todos los archivos de contenido, como secuencias de comandos o mínimo. A continuación, agregue un elemento con scaffolding de MVC, lo que creará un controlador y vistas en el proyecto.

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC y Web API Scaffolding - 404 de HTTP, no se encontró

Si se produce un error al agregar un elemento con scaffolding a un proyecto, es posible que el proyecto se quedará en un estado incoherente. Algunos de los cambios realizados a ser scaffolding se revertirá, pero otros cambios, como los paquetes de NuGet instalados, no se revertirán. Si se revierten los cambios de configuración de enrutamiento, los usuarios recibirán un error HTTP 404 cuando vaya a los elementos de scaffolding.

Solución:

- Para corregir este error para MVC, agregue un nuevo elemento con scaffolding y seleccionar las dependencias de MVC 5 (mínimo o completo). Este proceso se agregará todos los cambios necesarios al proyecto.
- Para corregir este error para API Web:

  1. Agregue la clase WebApiConfig al proyecto.

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. Configure WebApiConfig.Register en la aplicación\_Start (método) en Global.asax como sigue:

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
