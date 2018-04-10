---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Este documento describe la versión de ASP.NET MVC 4 Beta para Visual Studio 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: d29f09d726e835c1eb1fc38e643a4bfe7f00f61c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2018
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Este documento describe la versión de ASP.NET MVC 4 Beta para Visual Studio 2010.
> 
> > [!NOTE]
> > Esto no es la versión más reciente. Están disponibles las notas de la versión RC de ASP.NET MVC 4 [aquí](mvc4-release-notes.md).


- [Notas sobre la instalación](#_Toc303253802)
- [Documentación](#_Toc303253803)
- [Soporte técnico](#_Toc303253804)
- [Requisitos de software](#_Toc303253805)
- [Actualizar un proyecto de ASP.NET MVC 3 a ASP.NET MVC 4](#_Toc303253806)
- [Nuevas características de ASP.NET MVC 4 Beta](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197) (Más información sobre ASP.NET Web API)
    - [Aplicación de una página ASP.NET sencilla](#_Toc317096198)
    - [Mejoras en las plantillas de proyecto predeterminadas](#_Toc303253808)
    - [Plantilla de proyecto móvil](#_Toc303253809)
    - [Modos de presentación](#_Toc303253810)
    - [jQuery Mobile, el modificador de vista e invalidar el explorador](#_Toc303253811)
    - [Recetas para la generación de código en Visual Studio](#_Toc303253812)
    - [Asistencia para las tareas para los controladores asincrónicos](#_Toc303253813)
    - [SDK de Azure](#_Toc303253814)
    - [Problemas conocidos y los cambios recientes](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Notas sobre la instalación

ASP.NET MVC 4 Beta para Visual Studio 2010 puede instalarse desde el [página principal de ASP.NET MVC 4](../mvc/mvc4.md) mediante el instalador de plataforma Web.

Debe desinstalar cualquier vistas previas previamente instaladas de ASP.NET MVC 4 antes de instalar la versión Beta de ASP.NET MVC 4.

Esta versión no es compatible con la vista previa de desarrollador de .NET Framework 4.5. Debe desinstalar la versión preliminar para desarrolladores de .NET 4.5 antes de instalar la versión Beta de ASP.NET MVC 4.

ASP.NET MVC 4 puede instalarse y se puede ejecutar en paralelo con ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Documentación

Documentación para ASP.NET MVC está disponible en el sitio Web MSDN en la dirección URL siguiente:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Tutoriales y otra información sobre ASP.NET MVC están disponibles en la página de MVC 4 del sitio Web ASP.NET ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Compatibilidad

Esto es una versión preliminar y no se admite oficialmente. Si tiene alguna pregunta sobre cómo trabajar con esta versión, registrarlas en el foro de ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), donde los miembros de la Comunidad de ASP.NET son normalmente pueden ofrecer soporte técnico informal.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Requisitos de software

Los componentes de ASP.NET MVC 4 para Visual Studio requieren PowerShell 2.0 y Visual Studio 2010 con Service Pack 1 o Visual Web Developer Express 2010 con Service Pack 1.

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Actualizar un proyecto de ASP.NET MVC 3 a ASP.NET MVC 4

ASP.NET MVC 4 se puede instalar paralelo con ASP.NET MVC 3 en el mismo equipo, lo cual ofrece flexibilidad de elegir cuándo se debe actualizar una aplicación de ASP.NET MVC 3 a ASP.NET MVC 4.

La manera más sencilla de actualización es para crear un nuevo proyecto de ASP.NET MVC 4 y copie todas las vistas, controladores, código y contenido de archivos desde el proyecto de MVC 3 existente al nuevo proyecto y, a continuación, actualizar el ensamblado hace referencia en el nuevo proyecto para que coincida con el proyecto anterior. Si ha realizado cambios en el archivo Web.config en el proyecto de MVC 3, también debe combinar los cambios en el archivo Web.config en el proyecto de MVC 4.

Para actualizar manualmente una aplicación existente de ASP.NET MVC 3 a la versión 4, haga lo siguiente:

1. En todos los archivos Web.config en el proyecto (no hay uno en la raíz del proyecto, uno en la carpeta Views y uno en la carpeta Views de cada área del proyecto), reemplace todas las instancias del texto siguiente:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    con el siguiente texto correspondiente:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. En el archivo raíz Web.config, actualice el *webPages:Version* elemento a "2.0.0.0" y agregue un nuevo *PreserveLoginUrl* clave que tiene el valor "true":

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. En el Explorador de soluciones, elimine la referencia a *System.Web.Mvc* (que señala a la versión 3 DLL). A continuación, agregue una referencia a *System.Web.Mvc* (v4.0.0.0). En concreto, realice los cambios siguientes para actualizar las referencias de ensamblado. A continuación se indican los detalles:

    1. En el Explorador de soluciones, elimine las referencias a los siguientes ensamblados: 

        - *System.Web.Mvc*(v3.0.0.0)
        - *System.Web.WebPages*(v1.0.0.0)
        - *System.Web.Razor*(v1.0.0.0)
        - *System.Web.WebPages.Deployment*(v1.0.0.0)
        - *System.Web.WebPages.Razor*(v1.0.0.0)
    2. Agregue una referencias a los ensamblados siguientes: 

        - *System.Web.Mvc*(v4.0.0.0)
        - *System.Web.WebPages*(v2.0.0.0)
        - *System.Web.Razor*(v2.0.0.0)
        - *System.Web.WebPages.Deployment*(v2.0.0.0)
        - *System.Web.WebPages.Razor*(v2.0.0.0)
4. En el Explorador de soluciones, haga clic en el nombre del proyecto y, a continuación, seleccione Descargar proyecto. A continuación, haga clic en el nombre nuevo y seleccione Editar *ProjectName*.csproj.
5. Busque la *ProjectTypeGuids* elemento y reemplace {E53F8FEA-EAE0-44A6-8774-FFD645390401} con {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Guardar los cambios, cierre el archivo de proyecto (.csproj) que estaba editando, haga clic en el proyecto y, a continuación, seleccione recargar el proyecto.
7. Si el proyecto hace referencia a las bibliotecas de terceros que están compiladas con versiones anteriores de ASP.NET MVC, abra el archivo raíz Web.config y agregue las siguientes *bindingRedirect* elementos en la  *configuración* sección: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>Nuevas características de ASP.NET MVC 4 Beta

Esta sección describen las características que se han introducido en la versión Beta de ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

Ahora, ASP.NET MVC 4 incluye ASP.NET Web API, un nuevo marco para crear servicios HTTP puede alcanzar una amplia variedad de clientes como exploradores y dispositivos móviles. ASP.NET Web API es también una plataforma ideal para compilar servicios RESTful.

ASP.NET Web API incluye compatibilidad para las siguientes características:

- **Modelo de programación HTTP moderna:** directamente tener acceso y manipular las solicitudes HTTP y las respuestas en las API Web con un nuevo modelo de objeto HTTP fuertemente tipado. El mismo modelo y la canalización HTTP de programación está simétricamente disponible en el cliente a través del nuevo tipo HttpClient.
- **Compatibilidad completa para las rutas**: las API Web ahora admite el conjunto completo de capacidades de ruta que siempre ha sido una parte de la pila de Web, incluidos los parámetros de ruta y las restricciones. Además, la asignación de acciones es totalmente compatible con convenciones, por lo que ya no se necesita aplicar atributos como [HttpPost] a las clases y métodos.
- **Negociación de contenido**: el cliente y el servidor pueden trabajar juntos para determinar el formato correcto para los datos que se devuelven desde una API. Ofrecemos compatibilidad predeterminada para XML, JSON y formatos codificados de dirección URL de formulario, y puede ampliar esta compatibilidad agregando sus propia formateadores, o incluso reemplazar la estrategia de negociación de contenido de forma predeterminada.
- **Enlace y validación de modelos:** enlazadores de modelos proporcionan una manera sencilla de extraer datos de distintas partes de una solicitud HTTP y convertir las partes de mensaje en objetos de .NET que se pueden usar con las acciones de API Web.
- **Filtros:** Web API ahora es compatible con los filtros, incluidos los filtros conocidos como el atributo [Authorize]. Puede crear y conectar sus propios filtros para las acciones, autorización y control de excepciones.
- **La composición de consulta:** devolver simplemente un valor IQueryable&lt;T&gt;, la API Web será compatible con consultas a través de direcciones URL de OData.
- **Mejora la capacidad de prueba de detalles HTTP:** en lugar de establecer detalles HTTP en objetos de contexto estático, acciones de Web API ahora pueden trabajar con instancias de HttpRequestMessage y HttpResponseMessage. También existen versiones genéricas de estos objetos para permitirle trabajar con sus tipos personalizados además de los tipos HTTP.
- **Mejora la inversión de Control (IoC) mediante DependencyResolver:** Web API ahora usa el patrón de localizador de servicio implementado por la resolución de dependencias de MVC para obtener instancias de para muchas funciones diferentes.
- **Configuración basada en código:** configuración de Web API se realiza únicamente a través de código, dejando la configuración de limpieza de archivos.
- **Autohospedaje:** las API Web se pueden hospedar en su propio proceso además de IIS mientras se sigue usando toda la funcionalidad de rutas y otras características de API Web.

Para obtener más detalles sobre la API Web de ASP.NET, visitan [ https://www.asp.net/web-api ](../web-api/index.md).

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>Aplicación de una página ASP.NET sencilla

ASP.NET MVC 4 incluye ahora un anticipo de la experiencia para compilar aplicaciones de una página con significativas interacciones del lado cliente con JavaScript y las API Web. Esta compatibilidad incluye:

- Un conjunto de bibliotecas de JavaScript más enriquecida interacciones local con datos almacenados en caché
- Otros componentes de API Web de unidad de trabajo y soporte técnico DAL
- Una plantilla de proyecto MVC con scaffolding para empezar a trabajar rápidamente

Para obtener más detalles sobre la aplicación de una página solo se admiten en ASP.NET MVC 4, visite [ https://www.asp.net/single-page-application ](../single-page-application/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Mejoras en las plantillas de proyecto predeterminadas

Se ha actualizado la plantilla que se usa para crear nuevos proyectos de ASP.NET MVC 4 para crear un sitio Web de aspecto más moderno:

![](mvc4-beta-release-notes/_static/image1.png)

Además de mejoras cosméticas, mejoró la funcionalidad de la nueva plantilla. La plantilla emplea una técnica denominada representación adaptable para que se muestren correctamente en los exploradores de escritorio y exploradores móviles sin ninguna personalización.

![](mvc4-beta-release-notes/_static/image2.png)

Para ver la representación adaptable en acción, puede usar un emulador de dispositivos móvil o intente cambiar el tamaño de la ventana del explorador de escritorio para que sea menor. Cuando la ventana del explorador obtiene lo suficientemente pequeña, cambiará el diseño de la página.

Otra mejora en la plantilla de proyecto predeterminada es el uso de JavaScript para proporcionar una interfaz de usuario más enriquecida. Los vínculos de inicio de sesión y de registro que se usan en la plantilla son ejemplos de cómo usar el cuadro de diálogo de interfaz de usuario de jQuery para presentar una pantalla de inicio de sesión completo:

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Plantilla de proyecto móvil

Si está empezando a un nuevo proyecto y desea crear un sitio específicamente para dispositivos móviles y exploradores de Tablet PC, puede usar la nueva plantilla de proyecto de aplicación móvil. Esto se basa en jQuery Mobile, una biblioteca de código abierto para la creación de la interfaz de usuario táctil optimizada:

![](mvc4-beta-release-notes/_static/image4.png)

Esta plantilla contiene la misma estructura de aplicación que la plantilla de aplicación de Internet (y el código del controlador es prácticamente idéntico), pero es un estilo mediante jQuery Mobile que aparezcan correctamente y se comporten correctamente en dispositivos móviles basados en la entrada táctil. Para obtener más información sobre la estructura y el estilo de la interfaz de usuario móvil, consulte el [sitio Web de proyecto móvil de jQuery](http://jquerymobile.com/).

Si ya tiene un sitio orientada a servicios de escritorio que le interese agregar vistas optimizadas para el móvil a, o si desea crear un único sitio que actúa de vistas con estilo de manera diferente a los exploradores de escritorio y mobile, puede usar la nueva característica de modos de presentación. (Vea la sección siguiente).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Modos de presentación

La nueva característica de modos de presentación permite que una aplicación seleccionar vistas dependiendo del explorador que está realizando la solicitud. Por ejemplo, si un explorador de escritorio, solicita la página de inicio, la aplicación podría usar la plantilla Views\Home\Index.cshtml. Si un explorador móvil solicita la página de inicio, la aplicación podría devolver la plantilla Views\Home\Index.mobile.cshtml.

También se pueden reemplazar diseños y parciales para tipos de explorador determinado. Por ejemplo:

- Si la carpeta Views\Shared contiene tanto el \_Layout.cshtml y \_Layout.mobile.cshtml plantillas, de forma predeterminada, la aplicación usará \_Layout.mobile.cshtml durante las solicitudes de exploradores móviles y \_Layout.cshtml durante otras solicitudes.
- Si una carpeta contiene \_MyPartial.cshtml y \_MyPartial.mobile.cshtml, la instrucción @Html.Partial("\_MyPartial") se representarán \_MyPartial.mobile.cshtml durante las solicitudes de mobile exploradores, y \_MyPartial.cshtml durante otras solicitudes.

Si desea crear más determinadas vistas, diseños o vistas parciales para otros dispositivos, puede registrar un nuevo *DefaultDisplayMode* instancia para especificar el nombre que se busca cuando una solicitud cumple las condiciones específicas. Por ejemplo, podría agregar el código siguiente a la *aplicación\_iniciar* método en el archivo Global.asax para registrar la cadena "iPhone" como un modo de presentación que se aplica cuando el Explorador de iPhone de Apple realiza una solicitud:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

Después de ejecuta este código, cuando un explorador de iPhone de Apple realiza una solicitud, la aplicación utilizará la Views\Shared\\_Layout.iPhone.cshtml diseño (si existe).

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile, el modificador de vista e invalidar el explorador

jQuery Mobile es una biblioteca de código abierto para la creación de la interfaz de usuario web táctil optimizada. Si desea usar jQuery Mobile con una aplicación de ASP.NET MVC 4, puede descargar e instalar un paquete de NuGet que le ayuda a empezar a trabajar. Para instalarlo desde la consola de administrador de paquetes de Visual Studio, escriba el siguiente comando:

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

Esto instala jQuery Mobile y algunos archivos de aplicación auxiliar, incluido lo siguiente:

- Vistas/compartida/\_Layout.Mobile.cshtml, que es un diseño basado en Mobile de jQuery.
- Un componente de modificador de vista, que consta de lavistas/compartida/\_ViewSwitcher.cshtml de vista parcial y el controlador ViewSwitcherController.cs.

Después de instalar el paquete, ejecutar su aplicación mediante un explorador móvil (o equivalente, como el Firefox [modificador de agente de usuario](http://chrispederick.com/work/user-agent-switcher/) complemento). Verá que las páginas tienen un aspecto muy diferentes, porque jQuery Mobile controla el diseño y aplicación de estilos. Para aprovechar estas características, puede hacer lo siguiente:

- Crear invalidaciones de la vista específica para equipos móviles tal y como se describe en la sección [modos de presentación](#_Toc303253810) anteriormente (por ejemplo, crear Views\Home\Index.mobile.cshtml para invalidar Views\Home\Index.cshtml para los exploradores móviles).
- Leer la [jQuery móvil documentación](http://jquerymobile.com/) para obtener más información sobre cómo agregar elementos de interfaz de usuario táctil optimizada en las vistas móviles.

Es una convención para las páginas de web móvil optimizada agregar un vínculo cuyo texto está algo como vista de escritorio o en modo de sitio completo que permite a los usuarios cambiar a una versión de escritorio de la página. El paquete de jQuery.Mobile.MVC incluye un componente de modificador de vista de ejemplo para este propósito. Se utiliza en el valor predeterminado Views\Shared\\_Layout.Mobile.cshtml vista, y tiene un aspecto similar al siguiente cuando se representa la página:

![](mvc4-beta-release-notes/_static/image5.png)

Si los visitantes, haga clic en el vínculo, está cambiados a la versión de escritorio de la misma página.

Dado que el diseño del escritorio no incluye a un modificador de vista de forma predeterminada, los visitantes no tendrán una forma de acceder a modo de móvil. Para habilitar esta opción, agregue la siguiente referencia a  *\_ViewSwitcher* al diseño del escritorio, just dentro de la *&lt;cuerpo&gt;* elemento:

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

El modificador de vista utiliza una nueva característica denominada Explorador reemplazar. Esta característica permite que su aplicación trate las solicitudes como si procediesen de un explorador distinto (agente de usuario) a la realmente están desde. En la tabla siguiente enumera los métodos proporcionados por la invalidación de explorador.

| `HttpContext.SetOverriddenBrowser(userAgentString)` | Invalida el valor de agente de usuario real de la solicitud mediante el agente de usuario especificado. |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | Devuelve el valor de invalidación de agente de usuario de la solicitud o la cadena de agente de usuario actual si se ha especificado ninguna omisión. |
| `HttpContext.GetOverriddenBrowser()` | Devuelve un *HttpBrowserCapabilitiesBase* instancia que se corresponde con el agente de usuario establecido actualmente para la solicitud (real o reemplazada). Puede usar este valor para obtener propiedades como *IsMobileDevice*. |
| `HttpContext.ClearOverriddenBrowser()` | Quita a cualquier agente de usuario invalidado para la solicitud actual. |

Reemplazando explorador es una característica fundamental de ASP.NET MVC 4 y está disponible incluso si no instala el paquete jQuery.Mobile.MVC. Sin embargo, afecta a solo vista, diseño y selección de vista parcial, no afecta a todas las características ASP.NET que dependen la *Request.Browser* objeto.

De forma predeterminada, la invalidación de agente de usuario se almacena utilizando una cookie. Si desea almacenar la invalidación en otra ubicación (por ejemplo, en una base de datos), puede reemplazar el proveedor predeterminado (*BrowserOverrideStores.Current*). Documentación de este proveedor estará disponible como acompañamiento de una versión posterior de ASP.NET MVC.

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Recetas para la generación de código en Visual Studio

La nueva característica de recetas permite que Visual Studio generar código de específica de la solución basado en los paquetes que se pueden instalar con NuGet. El marco de trabajo de recetas facilita a los desarrolladores escribir complementos de generación de código, que también puede usar para reemplazar los generadores de código integrados para agregar área y Agregar controlador, agregar vista. Dado que recetas se implementan como paquetes de NuGet, se pueden fácilmente los elementos verificados a control de código fuente y comparten con todos los desarrolladores en el proyecto automáticamente. También están disponibles en una base por la solución.

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Asistencia para las tareas para los controladores asincrónicos

Ahora puede escribir métodos de acción asincrónicos como una sola métodos que devuelven un objeto del tipo *tarea* o *tarea&lt;ActionResult&gt;*.

Por ejemplo, si utiliza Visual C# 5 (o mediante la [Async CTP](https://msdn.microsoft.com/vstudio/async.aspx)), puede crear un método de acción asincrónico que sea similar al siguiente:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

En el método de acción anterior, las llamadas a *newsService.GetHeadlinesAsync* y *sportsService.GetScoresAsync* se denominan de forma asincrónica y no bloquean un subproceso del grupo de subprocesos.

Métodos de acción asincrónicos que devuelven *tarea* instancias también admiten los tiempos de espera. Para hacer que el método de acción cancelable, agregue un parámetro de tipo *CancellationToken* para la firma del método de acción. En el ejemplo siguiente se muestra un método de acción asincrónico que tiene un tiempo de espera de 2500 milisegundos y que muestra un *TimedOut* ver al cliente si se produce un tiempo de espera.

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>SDK de Azure

Versión Beta de ASP.NET MVC 4 es compatible con la versión 1.5 de septiembre de 2011 de Windows Azure SDK.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conocidos y los cambios recientes

- **Después de instalar la versión Beta de ASP.NET MVC 4, puede pausar el editor CSHTML y VBHTML en el editor de Visual Studio 2010 Service Pack 1 CSHTML y VBHTML durante mucho tiempo después de escribir el fragmento de código o JavaScript archivos cshtml y vbhtml.** Esto sucede únicamente en las aplicaciones de ASP.NET MVC 4 que se han creado recientemente y todavía no se ha compilado.

    La solución consiste en compilar el proyecto para obtener los ensamblados en la carpeta bin. Tenga en cuenta que si se limpie el proyecto que quita los ensamblados de la carpeta bin, se devolverán el problema del editor.

    Este problema se corregirá en una próxima versión.
- **Plantillas de proyecto de C# para Visual Studio 11 Beta contienen una cadena de conexión incorrecta en Global.asax.cs.** La conexión predeterminada especificada en la aplicación\_Start (método) para los proyectos creados en Visual Studio 11 Beta contienen una cadena de conexión de LocalDB que contiene una barra diagonal inversa sin escape (\) caracteres. Esto produce un error de conexión cuando intenta obtener acceso a un DbContext de Framework de entidad, que genera una excepción SqlException.

    Para corregir este problema, escape del carácter de barra diagonal inversa en la aplicación\_Start (método) de Global.asax.cs para que quede como sigue:

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **Las aplicaciones de ASP.NET MVC 4 que se dirigen a .NET 4.5, producirán un FileLoadException al intentar obtener acceso al ensamblado de System.Net.Http.dll cuando se ejecuta en .NET 4.0.** Las aplicaciones de ASP.NET MVC 4 creadas en .NET 4.5 contienen un enlace de redirección que dará como resultado un FileLoadException lo que indica que "no pudo cargar archivo o ensamblado 'System.Net.Http' o uno de sus dependencias." Cuando la aplicación se ejecuta en un sistema con .NET 4.0 instalado. Para corregir este problema, quite la siguiente redirección de enlace de web.config:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    El elemento de enlace de ensamblado en el archivo web.config modificado debería aparecer como sigue:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- <strong>La plantilla de elemento de "Agregar controlador" en proyectos de Visual Basic genera un espacio de nombres incorrecto cuando se invoca</strong><strong>desde dentro de un área.</strong> Cuando se agrega un controlador a un área en un proyecto de MVC de ASP.NET que utiliza Visual Basic, la plantilla de elemento inserta el espacio de nombres incorrecto en el controlador. El resultado es un error de "archivo no encontrado" al navegar a cualquier acción en el controlador.  
  
  El espacio de nombres generado omite todo el contenido después de espacio de nombres raíz. Por ejemplo, el espacio de nombres generado es *RootNamespace* pero debe ser *RootNamespace.Areas.AreaName.Controllers* .
- **Cambios importantes en el motor de vista Razor.** Como parte de una reescritura del analizador Razor, se quitaron los siguientes tipos de *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  También se han quitado los siguientes métodos: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Cuando WebMatrix.WebData.dll se incluye en el directorio/bin de las aplicaciones de ASP.NET MVC 4, asume la dirección URL para la autenticación de formularios.** Agrega el ensamblado WebMatrix.WebData.dll a la aplicación (por ejemplo, seleccionando "Páginas Web ASP.NET con la sintaxis de Razor" cuando se usa el cuadro de diálogo Agregar dependencias implementables) invalidará la redirección de inicio de sesión de autenticación al inicio de sesión/account/en lugar de / cuenta o inicio de sesión según lo esperado de forma predeterminada la cuenta de controlador de ASP.NET MVC. Para evitar este comportamiento y la dirección URL especificada ya en la sección de autenticación del archivo web.config, puede agregar un appSetting denominado PreserveLoginUrl y establézcalo en true: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **Se produce un error en el Administrador de paquetes de NuGet instalar al intentar instalar ASP.NET MVC 4 para instalaciones en paralelo de Visual Studio 2010 y Visual Web Developer 2010.** Para ejecutar Visual Studio 2010 y Visual Web Developer 2010 paralelo con ASP.NET MVC 4 debe instalar ASP.NET MVC 4 después de haberse instalados ambas versiones de Visual Studio.
- **Se produce un error al desinstalar ASP.NET MVC 4 si ya se han desinstalado los requisitos previos.** Para desinstalar correctamente ASP.NET MVC 4you debe desinstalar ASP.NET MVC 4 antes de desinstalar Visual Studio.
- **Ejecutar un proyecto de API Web de manera predeterminada, muestra instrucciones que le indiquen de forma incorrecta al usuario agregar rutas mediante el método RegisterApis, que no existe.** Las rutas deben agregarse en el método RegisterRoutes utilizando la tabla de rutas ASP.NET.
- **Instalación de ASP.NET MVC 4 Beta interrumpe las aplicaciones de ASP.NET MVC 3 RTM.** Las aplicaciones de ASP.NET MVC 3 que se crearon con la versión RTM versión (no con la versión ASP.NET MVC 3 Tools Update) requieren los siguientes cambios para poder funcionar en paralelo con ASP.NET MVC 4 Beta. Compilar el proyecto sin realizar estos resultados de las actualizaciones en errores de compilación. 

    **Actualizaciones necesarias**

  1. En el archivo raíz Web.config, agregue un nuevo *&lt;appSettings&gt;* entrada con la clave *webPages:Version* y el valor *1.0.0.0*.

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
  2. En el Explorador de soluciones, haga clic en el nombre del proyecto y, a continuación, seleccione Descargar proyecto. A continuación, haga clic en el nombre nuevo y seleccione Editar *ProjectName*.csproj.
  3. Busque las siguientes referencias de ensamblado: 

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

      Reemplácelas con lo siguiente:

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
  4. Guardar los cambios, cierre el archivo de proyecto (.csproj) estaba editando y, a continuación, haga clic en el proyecto y seleccione volver a cargar.
