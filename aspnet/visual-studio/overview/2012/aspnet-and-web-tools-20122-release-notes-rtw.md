---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
title: ASP.NET y Web Tools 2012.2 notas de la versión | Microsoft Docs
author: rick-anderson
description: Notas de la versión de ASP.NET y Web Tools 2012.2.
ms.author: riande
ms.date: 02/14/2013
ms.assetid: 9534e58b-1d15-4f1d-b04c-10c79b9d8227
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
msc.type: content
ms.openlocfilehash: 0566a362b36f6cfb73b6479cd490e82c63455459
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828454"
---
<a name="aspnet-and-web-tools-20122-release-notes"></a>Notas de versión ASP.NET y Web Tools 2012.2
====================
> Este documento describe la versión de ASP.NET y Web Tools 2012.2. Es una actualización de herramientas Web de Visual Studio y ASP.NET.


- [Notas de instalación](#_Installation)
- [Documentación](#_Documentation)
- [Soporte técnico](#_Support)
- [Requisitos de software](#_Software_Requirements)
- [Nuevas características de ASP.NET y Web Tools 2012.2](#_New_Features_in)

    - [Herramientas](#_Tooling)
    - [Publicación en Web](#_Web_Publishing)
    - [Plantillas de ASP.NET MVC](#_Templates)
    - [ASP.NET Web API](#_ASP.NET_Web_API) (Más información sobre ASP.NET Web API)

    - [SignalR de ASP.NET](#_ASP.NET_SignalR)
    - [ASP.NET Friendly URLs](#_ASP.NET_Friendly_URLs)
- [Problemas conocidos y cambios importantes](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>Notas de instalación

ASP.NET y Web Tools 2012.2 para Visual Studio 2012 pueden instalarse mediante [instalador de plataforma Web](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2). Se trata de una actualización de Visual Studio 2012 o Visual Studio Express 2012 para Web, que es necesario. Si no tiene instalado Visual Studio, se instalará Visual Studio Express 2012 para Web.

También puede instalar ASP.NET y Web Tools 2012.2 manualmente. Debe tener Visual Studio 2012 o Visual Studio Express 2012 para Web instalado. A continuación, utilice las siguientes instrucciones: 

1. Descargar [ASP.NET y Web Frameworks 2012.2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) instalador desde el centro de descarga.
2. Cuando se ejecuta solicitadas haga clic en. También puede guardar el archivo para ejecutarlo más tarde.
3. Compruebe la versión de Visual Studio actualizará. Para ello, inicie Visual Studio que desea actualizar. A continuación, haga clic en el elemento de menú de ayuda.   
    ![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.jpg)
4. Si ve el elemento de menú &quot;acerca de Microsoft Visual Studio 2012 para Web&quot; , a continuación, descargue [Web Developer Tools 2012.2 - Visual Studio Express 2012 para Web](https://go.microsoft.com/fwlink/?LinkID=282228). En caso contrario, descargue [Web Developer Tools 2012.2 - Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).
5. Cuando se ejecuta solicitadas haga clic en. También puede guardar el archivo para ejecutarlo más tarde.

> [!NOTE]
> El lanzamiento de ASP.NET y Web Tools 2012.2 no incluye SQL Server Data Tools. SQL Server y bases de datos SQL de Windows Azure proporciona un completo conjunto de herramientas que incluyen el desarrollo basado en el proyecto sin conexión, la comparación de esquemas y las capacidades de implementación de base de datos mejorada de la base de datos. Para obtener más información o para instalar SQL Server Data Tools, visite [ https://go.microsoft.com/fwlink/?LinkID=237127 ](https://go.microsoft.com/fwlink/?LinkID=237127).

<a id="_Documentation"></a>
## <a name="documentation"></a>Documentación

Tutoriales y otra información acerca de ASP.NET y Web Tools 2012.2 están disponibles en el sitio web de ASP.NET ( https://www.asp.net).

<a id="_Support"></a>
## <a name="support"></a>Compatibilidad

ASP.NET y Web Tools 2012.2 se lanzó oficialmente y admite. Puede usar el canal de soporte normales. También puede publicar preguntas en los foros ASP.NET ([https://forums.asp.net/](https://forums.asp.net/)), donde los miembros de la Comunidad de ASP.NET son normalmente pueden ofrecer soporte técnico informal.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Requisitos de software

ASP.NET y Web Tools 2012.2 requiere Visual Studio 2012 o Visual Studio Express 2012 para Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>Nuevas características de ASP.NET y Web Tools 2012.2

Esta sección describen las características que se han introducido en la versión ASP.NET y Web Tools 2012.2.

<a id="_Tooling"></a>
### <a name="tooling"></a>Tooling

- Inspector de página 

    - Admite la asignación de selección de JavaScript que permite el Inspector de página asignar los elementos que se han agregado dinámicamente a la página al código JavaScript correspondiente.
    - La capacidad para ver las actualizaciones CSS en tiempo real.
    - Para obtener más información, lea [sincronización automática de CSS y JavaScript de selección de asignación en el Inspector de página](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Editor 

    - Admite el resaltado de sintaxis para JsRender, Handlebars, Mustache y CoffeeScript.
    - El editor de HTML proporciona Intellisense para los enlaces de Knockout.
    - MENOS de edición y el compilador admiten para habilitar la creación dinámica CSS con menor.
    - Pegar JSON como una clase. NET. Con este comando Pegado especial para pegar JSON en un C# o VB.NET archivo de código y Visual Studio generará automáticamente las clases de .NET puede inferir de JSON.
- Compatibilidad con el emulador de Mobile agrega enlaces de extensibilidad para que los emuladores de terceros pueden instalarse como un archivo VSIX. Los emuladores instalados se mostrarán en la lista desplegable de F5, para que los desarrolladores pueden obtener una vista previa sus sitios Web en una gran variedad de dispositivos móviles. Obtenga más información sobre esta característica en la entrada de blog de Scott Hanselman en [la nueva integración BrowserStack con Visual Studio](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>Publicación en Web

- Proyectos de sitio Web ahora tienen la misma experiencia de publicación como proyectos de aplicación Web, incluida la publicación en sitios Web de Windows Azure.
- Publicación selectiva &#8211; para uno o varios archivos puede hacer lo siguiente (después de publicarlo en un punto de conexión de Web Deploy): 

    - Publicar los archivos seleccionados.
    - Ver la diferencia entre un archivo local y un archivo remoto.
    - Actualice el archivo local con el archivo remoto o actualizar el archivo remoto con el archivo local.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>Plantillas de ASP.NET MVC

- La nueva plantilla de aplicación de Facebook facilita la escritura de aplicaciones fáciles de Canvas de Facebook. En unos pocos pasos sencillos, puede crear una aplicación de Facebook que obtiene los datos de un usuario que ha iniciado sesión y se integra con sus amigos. La plantilla incluye una nueva biblioteca para que se encargue de todos los mecanismos implicados en la creación de una aplicación de Facebook, incluida la autenticación, permisos, acceso a datos de Facebook y mucho más. Para obtener más información sobre el uso de la plantilla de aplicación de Facebook, vea [ https://go.microsoft.com/fwlink/?LinkID=269921 ](https://go.microsoft.com/fwlink/?LinkID=269921).
- Una nueva plantilla de MVC de aplicación de página única permite a los desarrolladores crear aplicaciones web de cliente interactivas mediante HTML 5, CSS 3 y Knockout popular y bibliotecas de JavaScript jQuery, sobre ASP.NET Web API. La plantilla incluye una aplicación de lista de "todo" que demuestra prácticas comunes para la creación de una aplicación JavaScript HTML5 que usa una API de servidor RESTful. Puede leer más información en [ https://www.asp.net/single-page-application ](../../../single-page-application/index.md).
- Ahora puede crear un archivo VSIX que agrega nuevas plantillas al cuadro de diálogo nuevo proyecto de ASP.NET MVC. Descubra cómo hacerlo aquí: [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- Paquete FixedDisplayModes &#8211; plantillas de proyecto MVC se han actualizado para incluir el nuevo paquete NuGet 'FixedDisplayModes', que contiene una solución a un error en MVC 4. Para obtener más información sobre la corrección contenida en el paquete, consulte esta entrada de blog ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) desde el equipo de MVC.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET Web API se ha mejorado con varias características nuevas:

- ASP.NET Web API OData
- Seguimiento de ASP.NET Web API
- Página de Ayuda de ASP.NET Web API

#### <a name="aspnet-web-api-odata"></a>ASP.NET Web API OData

ASP.NET Web API OData le ofrece la flexibilidad que necesita para crear puntos de conexión de OData con lógica de negocios enriquecida a través de cualquier origen de datos. Con ASP.NET Web API OData controlar la cantidad de semántica de OData que desea exponer. ASP.NET Web API OData se incluye con las plantillas de proyecto de ASP.NET MVC 4 y también está disponible en NuGet ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)).

Actualmente, ASP.NET Web API OData admite las siguientes características:

- Habilitar la semántica de consulta de OData al aplicar el atributo [Queryable].
- Validar las consultas de OData y restringir el conjunto de opciones de consulta admitidas, operadores y funciones fácilmente.
- Parámetro enlazar ODataQueryOptions directamente para obtener una representación de árbol de sintaxis abstracta de la consulta que, a continuación, se valida y aplica a un IQueryable o IEnumerable.
- Habilitar paginación controlada por servicio y próxima generación de vínculo de página mediante la especificación de los límites del resultado en el atributo [Queryable].
- Solicitar un recuento del número total de recursos coincidentes mediante $inlinecount insertado.
- Controlar la propagación de valores null.
- Cualquier o todos los operadores en $filter.
- Inferir un entity data model por convención o personalizar explícitamente un modelo de forma similar a Entity Framework Code First.
- Conjuntos de entidades de exponer derivando de EntitySetController.
- Convenciones de simple y personalizables para exponer las propiedades de navegación, manipular vínculos e implementar las acciones de OData.
- Simplifica el enrutamiento mediante el método de extensión MapODataRoute.
- Compatibilidad con control de versiones mediante la exposición de varios modelos EDM.
- Exponer $metadata y documento de servicio para que pueda generar a clientes (. NET, Windows Phone, Windows Store, etc.) para la API Web.
- Compatibilidad con los formatos OData Atom, JSON y JSON detallados.
- Crear, actualizar, parcialmente update (PATCH) y eliminar entidades.
- Consultar y manipular las relaciones entre entidades.
- Crear vínculos de relación hasta las rutas de conexión.
- Tipos complejos.
- Herencia de tipo de entidad.
- Propiedades de la colección.
- Enumeraciones.
- Acciones de OData.
- Desarrollado sobre la misma base que WCF Data Services, es decir, ODataLib ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)).

Para obtener más información sobre ASP.NET Web API OData vea [ https://go.microsoft.com/fwlink/?LinkId=271141 ](https://go.microsoft.com/fwlink/?LinkId=271141).

#### <a name="aspnet-web-api-tracing"></a>Seguimiento de ASP.NET Web API

Seguimiento de ASP.NET Web API integra datos de seguimiento de sus API web con el seguimiento. NET. Ahora está habilitado de forma predeterminada en la plantilla de proyecto Web API. Seguimiento de datos para la web API se envían a la ventana de salida y está disponible a través de IntelliTrace. Seguimiento de ASP.NET Web API permite a la información de seguimiento sobre la API Web cuando se hospeda en Windows Azure mediante la integración con [diagnósticos de Windows Azure](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx). También puede instalar y habilitar el seguimiento de ASP.NET Web API en cualquier aplicación que use el paquete NuGet de seguimiento de ASP.NET Web API ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)).

Para obtener más información sobre cómo configurar y usar el seguimiento de ASP.NET Web API, consulte [ https://go.microsoft.com/fwlink/?LinkID=269874 ](https://go.microsoft.com/fwlink/?LinkID=269874).

#### <a name="aspnet-web-api-help-page"></a>Página de Ayuda de ASP.NET Web API

La página de Ayuda de ASP.NET Web API ahora se incluye de forma predeterminada en la plantilla de proyecto Web API. La página de Ayuda de ASP.NET Web API genera automáticamente la documentación de web API, incluidos los puntos de conexión HTTP, los métodos HTTP admitidos, parámetros y las cargas de mensajes de solicitud y respuesta de ejemplo. Automáticamente se extrae la documentación de comentarios en el código. También puede agregar la página de Ayuda de ASP.NET Web API a cualquier aplicación mediante el paquete NuGet de ASP.NET Web API ayuda Page ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)).

Para obtener más información sobre cómo configurar y personalizar el, vea la página de Ayuda de API Web de ASP.NET [ https://go.microsoft.com/fwlink/?LinkId=271140 ](https://go.microsoft.com/fwlink/?LinkId=271140).

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

SignalR de ASP.NET facilita agregar capacidades de web en tiempo real a la aplicación ASP.NET, con WebSockets si está disponible y automáticamente revirtiendo a otras técnicas cuando no lo está.

Para obtener más información sobre el uso de ASP.NET SignalR vea [ https://go.microsoft.com/fwlink/?LinkId=271271 ](https://go.microsoft.com/fwlink/?LinkId=271271).

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>ASP.NET Friendly URLs

FriendlyURLs ASP.NET facilita enormemente a los desarrolladores de web forms generar limpiador busca las direcciones URL (sin la extensión .aspx). No requiere poca o ninguna configuración y puede usarse con aplicaciones de ASP.NET v4.0. La característica FriendlyURLs también facilita a los desarrolladores agregar compatibilidad con dispositivos móvil a sus aplicaciones, al admitir el cambio entre las vistas de escritorio y móviles.

Para obtener más información sobre cómo instalar y usar ASP.NET Friendly URLs vea [ http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx ](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conocidos y cambios importantes

En esta sección se describe problemas conocidos y cambios importantes que se encuentran en la versión ASP.NET y Web Tools 2012.2.

### <a name="installation-issues"></a>Problemas de instalación

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Se instala en el orden correcto de Visual Studio 2012

Instalar un adicionales SKU de Visual Studio 2012 después de instalar ASP.NET y Web Tools 2012.2 requerirá una operación de reparación. Tenga en cuenta la siguiente secuencia:

1. Instalar Visual Studio 2012 Express para Web
2. Instalar ASP.NET y Web Tools 2012.2
3. Instalar Visual Studio 2012 Professional, Premium o Ultimate

Paso 2 solo daría como resultado la instalación de actualizaciones para Express para Web. Para asegurarse de que la SKU adicional instalada todavía durante el paso 3 contiene la actualización deberá reparar ASP.NET y Web Tools 2012.2 para instalar las actualizaciones de la última SKU instalada. Esto también se aplica si se invierten las SKU en el paso 1 y 3.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Instalación de Microsoft ASP.NET y Web Tools 2012.2 cuando se abre Visual Studio

Si está abierto VS durante la instalación de Microsoft ASP.NET y Web Tools 2012.2, Visual Studio podría acabar en mal estado. Se recomienda que los usuarios cerrar todas las instancias de Visual Studio antes de continuar con la instalación.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>Cancelación de la instalación ASP.NET y Web Tools 2012.2 en medio de instalación

Cancelación de ASP.NET y Web Tools 2012.2 el programa de instalación en el medio de instalación dejará de Visual Studio en un estado incorrecto. Para resolver este problema, siga estos pasos: 

- Vaya a agregar o quitar programas
- Desinstale Microsoft ASP.NET y Web Tools 2012.2, si está presente.
- Volver a instalar Microsoft ASP.NET y Web Tools 2012.2

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>Después de desinstalar ASP.NET y Web Tools 2012.2 el ASP.NET MVC 4 plantillas y plantillas de sitio Web de Razor v2 faltan

La desinstalación de ASP.NET y Web Tools 2012.2 también desinstalará todos de ASP.NET MVC 4 y plantillas de sitio Web de Razor v2 desde Visual Studio 2012.

La solución es reparar la instalación de Visual Studio 2012 para reinstalar ASP.NET MVC 4 y plantillas de sitio Web de Razor v2.

### <a name="tooling-issues"></a>Problemas de herramientas

#### <a name="nuget-error-reported-during-project-creation"></a>Error de NuGet notificado durante la creación del proyecto

Después de instalar ASP.NET y Web Tools 2012.2 es posible que vea el siguiente error al crear un proyecto de MVC 4

![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.png)

ASP.NET y Web Tools 2012.2 se distribuye 2.1 de NuGet y actualizará la extensión en Visual Studio 2012. En algunos casos, el instalador de VSIX se producirá un error al actualizar correctamente la extensión VSIX. Los pasos siguientes, podrá abordar este problema:

1. Inicie Visual Studio 2012 como administrador
2. Vaya a herramientas -&gt;extensiones y actualizaciones y desinstalar NuGet.
3. Cierre Visual Studio.
4. Navegue hasta la carpeta de instalación de ASP.NET y Web Tools 2012.2:

    1. Para Visual Studio 2012: **archivos de programa\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio 2012**
    2. Para Visual Studio 2012 Express for Web: **archivos de programa\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio Express 2012 para Web**
5. Haga doble clic en el NuGet.Tools.vsix para volver a instalar NuGet

### <a name="web-api-issues"></a>Problemas de Web API

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>Analizar los problemas de $filter y literales de fecha y hora

Se produce un error en el analizador de URI de OData analizar correctamente los literales de datetime parcial. Por ejemplo, $filter = inicio eq datetime'2012-12-31T12:00' no se puede analizar correctamente. Una solución alternativa es usar el literal completa, $filter = inicio eq datetime'2012-12-31T12:00:00'.

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData no es compatible con los nombres de propiedad entre mayúsculas y minúsculas.

OData no es compatible con los nombres de propiedad entre mayúsculas y minúsculas en las consultas de OData y la ruta de acceso de odata. Ver elementos de trabajo:

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Si los usuarios tienen que distinguen mayúsculas de minúsculas en el lado del cliente de javascript y el lado servidor, probablemente se encuentran este problema. Este problema es así por diseño en el protocolo odata. Sin embargo, muchos usuarios informa de este problema. Para solucionarlo, los usuarios tienen que corregir sus casos de la dirección URL.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>Convenciones de enrutamiento de OData de predeterminado no es compatible con POST o PUT en la propiedad de navegación.

Convenciones de enrutamiento de OData de predeterminado no es compatible con POST o PUT en la propiedad de navegación. Vea el elemento de trabajo [ http://aspnetwebstack.codeplex.com/workitem/366 ](http://aspnetwebstack.codeplex.com/workitem/366). No se encuentra esta convención usa en las convenciones predeterminadas.

Para solucionarlo, los usuarios necesitan ampliar la nueva convención de enrutamiento para admitirlo.

### <a name="facebook-template-issues"></a>Problemas de la plantilla de Facebook

#### <a name="facebook-application-template-only-works-using-net-45"></a>Plantilla de aplicación de Facebook solo funciona con .NET 4.5

Debe seleccionar .NET 4.5 en la lista desplegable de marco de trabajo en el cuadro de diálogo nuevo proyecto para ver la plantilla de aplicación de Facebook en ASP.NET MVC 4.

#### <a name="real-time-update-controller"></a>Controlador de actualización en tiempo real

La plantilla de aplicación de Facebook permite usuario fácilmente la creación de un controlador de API Web para controlar las actualizaciones en tiempo real de Facebook. Si el equipo de desarrollo está detrás de NAT, el controlador puede no funcionar sin configuración de red adicional. Consulte aquí para obtener más información: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>Consulta de valores de cadena entran en conflicto con los parámetros de Facebook OAuth

Los campos siguientes entran en conflicto con la llamada del cuadro de diálogo de Facebook OAuth hacer una copia de la dirección URL. No agregue sus propios valores de cadena de consulta con los siguientes nombres: código de error, error\_descripción, error\_motivo.

#### <a name="using-page-inspector-with-facebook-template"></a>Usar el Inspector de página con la plantilla de Facebook

No se puede usar la característica Inspector de página en Visual Studio 2012 mientras se depura su aplicación de Facebook. El Inspector de página no admite actualmente iframes.

### <a name="single-page-application-template-issues"></a>Problemas de plantillas de aplicación de página única

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>Con JQuery escriba actualización 1.9/Knockout 2.2.1, al ejecutar el proyecto de SPA de MVC predeterminada, nueva edición de elemento de lista de tareas no se controlan correctamente los eventos de foco.

Con JQuery 1.9/Knockout 2.2.1 de actualización, al ejecutar el proyecto de SPA de MVC de forma predeterminada, nueva edición de elemento de lista de tareas escriba ya no foco hasta el cuadro de edición de elemento de lista de tareas nueva después de escribir el nuevo elemento todo en la lista de tareas.

Referencia de la solución alternativa [ http://knockoutjs.com/documentation/hasfocus-binding.html ](http://knockoutjs.com/documentation/hasfocus-binding.html)y realizar la corrección similar en el código de ejemplo siguiente:

Archivo todo.model.js  
 función todolist(data), agregue siguientes:  
 **self.isSelected = ko.observable(false);**

función todoList.prototype.addTodo, agregue el siguiente texto blacked:  
 **self.isSelected(true);**  
 self.newTodoTitle(&quot;&quot;);

Archivo index.cshtml, agregue el siguiente texto blacked:  
 &lt;enlazar datos de formulario =&quot;enviar: addTodo&quot;&gt;  
 &lt;clase de entrada =&quot;addTodo&quot; tipo =&quot;texto&quot; data-bind =&quot;valor: newTodoTitle, marcador de posición: 'Tipo aquí para agregar', blurOnEnter: true, **hasfocus: isSelected**, evento: {Desenfoque: addTodo}&quot; /&gt;  
 &lt;/form&gt;
