---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Este documento describe la versión de ASP.NET MVC 4 Beta para Visual Studio 2010.
ms.author: aspnetcontent
ms.date: 09/09/2011
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: b9d50114a239b67b1adc263f6ea6d3a811bcc8f7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816696"
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Este documento describe la versión de ASP.NET MVC 4 Beta para Visual Studio 2010.
> 
> > [!NOTE]
> > Esto no es la versión más reciente. Están disponibles las notas de la versión RC de ASP.NET MVC 4 [aquí](mvc4-release-notes.md).


- [Notas de instalación](#_Toc303253802)
- [Documentación](#_Toc303253803)
- [Soporte técnico](#_Toc303253804)
- [Requisitos de software](#_Toc303253805)
- [Actualizar un proyecto de ASP.NET MVC 3 a ASP.NET MVC 4](#_Toc303253806)
- [Nuevas características de ASP.NET MVC 4 Beta](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197) (Más información sobre ASP.NET Web API)
    - [Aplicación de página única de ASP.NET](#_Toc317096198)
    - [Mejoras en las plantillas de proyecto predeterminadas](#_Toc303253808)
    - [Plantilla de proyecto móvil](#_Toc303253809)
    - [Modos de presentación](#_Toc303253810)
    - [jQuery Mobile, el modificador de vista y la invalidación de explorador](#_Toc303253811)
    - [Recetas para la generación de código en Visual Studio](#_Toc303253812)
    - [Compatibilidad con la tarea para los controladores asincrónicos](#_Toc303253813)
    - [SDK de Azure](#_Toc303253814)
    - [Problemas conocidos y cambios importantes](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Notas de instalación

Versión Beta de ASP.NET MVC 4 para Visual Studio 2010 puede instalarse desde el [página principal de ASP.NET MVC 4](../mvc/mvc4.md) mediante el instalador de plataforma Web.

Debe desinstalar las versiones preliminares instaladas previamente de ASP.NET MVC 4 antes de instalar la versión Beta de ASP.NET MVC 4.

Esta versión no es compatible con la versión preliminar para desarrolladores de .NET Framework 4.5. Debe desinstalar la versión preliminar para desarrolladores de .NET 4.5 antes de instalar la versión Beta de ASP.NET MVC 4.

ASP.NET MVC 4 se puede instalar y se puede ejecutar en paralelo con ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Documentación

Documentación de ASP.NET MVC está disponible en el sitio Web MSDN en la siguiente URL:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Tutoriales y otra información sobre ASP.NET MVC están disponibles en la página de MVC 4 del sitio Web ASP.NET ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Compatibilidad

Esto es una versión preliminar y no se admite oficialmente. Si tiene preguntas sobre cómo trabajar con esta versión, publíquelos en el foro de ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), donde los miembros de la Comunidad de ASP.NET son normalmente pueden ofrecer soporte técnico informal.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Requisitos de software

Los componentes de ASP.NET MVC 4 para Visual Studio requieren PowerShell 2.0 y Visual Studio 2010 con Service Pack 1 o Visual Web Developer Express 2010 con Service Pack 1.

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Actualizar un proyecto de ASP.NET MVC 3 a ASP.NET MVC 4

ASP.NET MVC 4 se puede instalar en paralelo con ASP.NET MVC 3 en el mismo equipo, lo que le ofrece flexibilidad para elegir cuándo se debe actualizar una aplicación de ASP.NET MVC 3 a ASP.NET MVC 4.

Actualización de la manera más sencilla es para crear un nuevo proyecto de ASP.NET MVC 4 y copia todas las vistas, controladores, código y contenido de archivos desde el proyecto de MVC 3 existente al nuevo proyecto y, a continuación actualizar el ensamblado hace referencia en el nuevo proyecto para que coincida con el proyecto antiguo. Si ha realizado cambios en el archivo Web.config en el proyecto de MVC 3, también debe combinar los cambios en el archivo Web.config en el proyecto de MVC 4.

Para actualizar manualmente una aplicación existente de ASP.NET MVC 3 a la versión 4, realice lo siguiente:

1. En todos los archivos Web.config en el proyecto (no hay uno en la raíz del proyecto, uno en la carpeta Views y uno en la carpeta Views de cada área del proyecto), reemplace todas las instancias del texto siguiente:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    con el siguiente texto correspondiente:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. En el archivo Web.config raíz, actualice el *webPages:Version* elemento a "2.0.0.0" y agregue un nuevo *PreserveLoginUrl* clave que tiene el valor "true":

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. En el Explorador de soluciones, elimine la referencia a *System.Web.Mvc* (que señala a la versión DLL de 3). A continuación, agregue una referencia a *System.Web.Mvc* (v4.0.0.0). En concreto, realice los cambios siguientes para actualizar las referencias de ensamblado. A continuación se indican los detalles:

    1. En el Explorador de soluciones, elimine las referencias a los ensamblados siguientes: 

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
5. Busque el *ProjectTypeGuids* elemento y reemplace {E53F8FEA-EAE0-44A6-8774-FFD645390401} con {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Guarde los cambios, cierre el archivo de proyecto (.csproj) que estaba editando, haga clic en el proyecto y, a continuación, seleccione recargar el proyecto.
7. Si el proyecto hace referencia a las bibliotecas de terceros que están compiladas con versiones anteriores de ASP.NET MVC, abra el archivo raíz Web.config y agregue los siguientes tres *bindingRedirect* elementos bajo el  *configuración* sección: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>Nuevas características de ASP.NET MVC 4 Beta

Esta sección describen las características que se han introducido en la versión Beta de ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET MVC 4 ahora incluye ASP.NET Web API, un nuevo marco para crear servicios HTTP que puede llegar a una amplia gama de clientes, incluidos exploradores y dispositivos móviles. ASP.NET Web API también es una plataforma ideal para crear servicios RESTful.

ASP.NET Web API incluye compatibilidad con las características siguientes:

- **Modelo de programación moderna de HTTP:** directamente, obtener acceso y manipular las solicitudes HTTP y respuestas en las API Web con un nuevo modelo de objeto HTTP fuertemente tipado. El mismo modelo de programación canalización HTTP está disponible simétricamente en el cliente mediante el nuevo tipo HttpClient.
- **Compatibilidad completa para las rutas**: las API Web ahora admiten el conjunto completo de funcionalidades de enrutamiento que siempre han sido una parte de la pila de Web, incluidos los parámetros de ruta y las restricciones. Además, la asignación de acciones es totalmente compatible con las convenciones, por lo que ya no necesitará aplicar atributos como [HttpPost] a las clases y métodos.
- **Negociación de contenido**: el cliente y el servidor pueden trabajar juntos para determinar el formato correcto para los datos que se devuelven desde una API. Proporcionamos soporte técnico de forma predeterminada para XML, JSON y formatos codificados de dirección URL del formulario, y puede ampliar esta compatibilidad agregando sus propios formateadores, o incluso reemplazar la estrategia de negociación de contenido predeterminada.
- **Enlace de modelos y validación:** enlazadores de modelos proporcionan una manera fácil para extraer datos de varias partes de una solicitud HTTP y convertir esas partes de mensaje en objetos de .NET que se pueden usar con las acciones de API Web.
- **Filtros:** las API Web ahora es compatible con los filtros, incluidos los filtros conocidos, como el atributo [Authorize]. Puede crear y conectar sus propios filtros de acciones, autorización y control de excepciones.
- **La composición de consulta:** devolviendo simplemente IQueryable&lt;T&gt;, la API Web será compatible con consultas a través de la dirección URL de OData.
- **Mejora la capacidad de prueba de los detalles HTTP:** en lugar de establecer los detalles HTTP en objetos de contexto estático, las acciones de API Web ahora pueden trabajar con instancias de HttpRequestMessage y HttpResponseMessage. También existen versiones genéricas de estos objetos para que pueda trabajar con sus tipos personalizados además de los tipos HTTP.
- **Mejora de la inversión de Control (IoC) mediante DependencyResolver:** Web API ahora usa el patrón de localizador de servicio implementado por la resolución de dependencias de MVC para obtener instancias de muchos servicios diferentes.
- **Configuración basada en código:** configuración de Web API se lleva a cabo únicamente a través de código, dejar la configuración de limpiar los archivos.
- **Autohospedaje:** las API Web se pueden hospedar en su propio proceso, además de IIS mientras sigue usando toda la eficacia de las rutas y otras características de la API Web.

Para obtener más información sobre ASP.NET Web API, visite [ https://www.asp.net/web-api ](../web-api/index.md).

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>Aplicación de página única de ASP.NET

ASP.NET MVC 4 ahora incluye una versión preliminar de la experiencia de creación de aplicaciones de página única con importantes interacciones del lado cliente con JavaScript y las API Web. Esta compatibilidad incluye:

- Un conjunto de bibliotecas de JavaScript más rico interacciones local con datos almacenados en caché
- Componentes adicionales de Web API para la unidad de trabajo y soporte técnico DAL
- Una plantilla de proyecto MVC con scaffolding para empezar a trabajar rápidamente

Para obtener más detalles sobre la solicitud de página única se admiten en ASP.NET MVC 4, visite [ https://www.asp.net/single-page-application ](../single-page-application/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Mejoras en las plantillas de proyecto predeterminadas

Se ha actualizado la plantilla que se usa para crear nuevos proyectos de ASP.NET MVC 4 para crear un sitio Web de aspecto más moderno:

![](mvc4-beta-release-notes/_static/image1.png)

Además de mejoras de cosméticas, ha mejorado la funcionalidad de la nueva plantilla. La plantilla emplea una técnica denominada procesamiento adaptable para mostrarse correctamente en exploradores de escritorio y los exploradores móviles sin ninguna personalización.

![](mvc4-beta-release-notes/_static/image2.png)

Para ver la representación adaptable en acción, puede usar un emulador de dispositivos móvil o intente cambiar el tamaño de la ventana del explorador de escritorio para que sea menor. Cuando se obtiene lo suficientemente pequeña como la ventana del explorador, cambiará el diseño de la página.

Otra mejora en la plantilla de proyecto predeterminada es el uso de JavaScript para proporcionar una interfaz de usuario más completa. Los vínculos de inicio de sesión y registro que se usan en la plantilla son ejemplos de cómo usar el cuadro de diálogo de interfaz de usuario de jQuery para presentar una pantalla de inicio de sesión completo:

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Plantilla de proyecto móvil

Si está empezando un nuevo proyecto y desea crear un sitio específicamente para dispositivos móviles y exploradores de Tablet PC, puede usar la nueva plantilla de proyecto de aplicación móvil. Esto se basa en jQuery Mobile, una biblioteca de código abierto para la creación de la interfaz de usuario táctil optimizada:

![](mvc4-beta-release-notes/_static/image4.png)

Esta plantilla contiene la misma estructura de aplicación que la plantilla de aplicación de Internet (y el código de controlador es prácticamente idéntico), pero es un estilo mediante jQuery Mobile que aparezcan correctamente y se comporten bien en dispositivos móviles basados en funcionalidad táctil. Para obtener más información acerca de cómo estructurar y estilo de la interfaz de usuario móvil, consulte el [sitio Web de proyecto móvil de jQuery](http://jquerymobile.com/).

Si ya tiene un sitio orientada a servicios de escritorio que desea agregar vistas móviles optimizadas en, o si desea crear un único sitio que actúa de vistas con estilo diferente a los exploradores de escritorio y móviles, puede usar la nueva característica de modos de presentación. (Consulte la sección siguiente).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Modos de presentación

La nueva característica de modos de presentación permite que una aplicación seleccionar vistas dependiendo del explorador que realiza la solicitud. Por ejemplo, si un explorador de escritorio, solicita la página principal, la aplicación podría usar la plantilla Views\Home\Index.cshtml. Si un explorador móvil solicita la página principal, la aplicación podría devolver la plantilla Views\Home\Index.mobile.cshtml.

También se pueden invalidar los diseños y parciales para los tipos de explorador determinado. Por ejemplo:

- Si la carpeta Views\Shared contiene tanto el \_Layout.cshtml y \_Layout.mobile.cshtml plantillas, de forma predeterminada, la aplicación usará \_Layout.mobile.cshtml durante las solicitudes de los exploradores móviles y \_Layout.cshtml durante otras solicitudes.
- Si una carpeta contiene ambos \_MyPartial.cshtml y \_MyPartial.mobile.cshtml, la instrucción @Html.Partial("\_MyPartial") se representarán \_MyPartial.mobile.cshtml durante las solicitudes de mobile los exploradores, y \_MyPartial.cshtml durante otras solicitudes.

Si desea crear vistas más específicas, diseños o vistas parciales para otros dispositivos, puede registrar un nuevo *DefaultDisplayMode* instancia para especificar el nombre que se busca cuando una solicitud cumple determinadas condiciones. Por ejemplo, podría agregar el código siguiente a la *aplicación\_iniciar* método en el archivo Global.asax para registrar la cadena "iPhone" como modo de presentación que se aplica cuando el Explorador de iPhone de Apple realiza una solicitud:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

Después de ejecutar este código, cuando un explorador de iPhone de Apple realiza una solicitud, la aplicación usará el Views\Shared\\_Layout.iPhone.cshtml diseño (si existe).

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile, el modificador de vista y la invalidación de explorador

jQuery Mobile es una biblioteca de código abierto para la creación de la interfaz de usuario web optimizadas para toque. Si desea usar jQuery Mobile con una aplicación ASP.NET MVC 4, puede descargar e instalar un paquete de NuGet que le ayuda a empezar a trabajar. Para instalarlo desde la consola de administrador de paquetes de Visual Studio, escriba el siguiente comando:

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

Esto instala jQuery Mobile y algunos archivos auxiliares, incluidos los siguientes:

- Views/Shared/\_Layout.Mobile.cshtml, que es un diseño de jQuery Mobile.
- Un componente de modificador de vista, que consta de loViews/Shared/\_ViewSwitcher.cshtml de vista parcial y el controlador ViewSwitcherController.cs.

Después de instalar el paquete, ejecute la aplicación con un explorador móvil (o equivalente, como el Firefox [Mezclador de agente de usuario](http://chrispederick.com/work/user-agent-switcher/) complemento). Verá que las páginas se ven bastante diferentes, ya que jQuery Mobile controla el diseño y estilo. Para aprovechar las ventajas de esto, puede hacer lo siguiente:

- Crear invalidaciones de la vista específica para móviles, como se describe en [modos de presentación](#_Toc303253810) anteriormente (por ejemplo, crear Views\Home\Index.mobile.cshtml para invalidar Views\Home\Index.cshtml para exploradores móviles).
- Leer el [jQuery Mobile documentación](http://jquerymobile.com/) para obtener más información sobre cómo agregar elementos de interfaz de usuario táctil optimizada de vistas móviles.

Es una convención para las páginas web optimizada para móviles agregar un vínculo cuyo texto es algo similar a la vista de escritorio o en modo de sitio completa que permite a los usuarios cambiar a una versión de escritorio de la página. El paquete jQuery.Mobile.MVC incluye un componente de modificador de vista de ejemplo para este propósito. Se utiliza en el valor predeterminado Views\Shared\\_Layout.Mobile.cshtml vista, y tiene un aspecto similar al siguiente cuando se procesa la página:

![](mvc4-beta-release-notes/_static/image5.png)

Si los visitantes, haga clic en el vínculo, le cambió a la versión de escritorio de la misma página.

Dado que el diseño de escritorio no incluirá a un modificador de vista de forma predeterminada, los visitantes no tendrán una manera de llegar a modo de móvil. Para ello, agregue la siguiente referencia a  *\_ViewSwitcher* a su diseño de escritorio, justo dentro de la *&lt;cuerpo&gt;* elemento:

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

El modificador de vista utiliza una nueva característica denominada Explorador reemplazar. Esta característica permite que una aplicación trate las solicitudes como si provinieran de un explorador distinto (agente de usuario) a la realmente están desde. En la tabla siguiente se enumera los métodos que proporciona la invalidación de explorador.

| `HttpContext.SetOverriddenBrowser(userAgentString)` | Invalida el valor de agente de usuario real de la solicitud mediante el agente de usuario especificado. |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | Devuelve el valor de invalidación de agente de usuario de la solicitud o la cadena de agente de usuario real si no se ha especificado ninguna omisión. |
| `HttpContext.GetOverriddenBrowser()` | Devuelve un *HttpBrowserCapabilitiesBase* instancia que se corresponde con el agente de usuario establecido actualmente para la solicitud (real o reemplazada). Puede usar este valor para obtener las propiedades como *IsMobileDevice*. |
| `HttpContext.ClearOverriddenBrowser()` | Quita a cualquier agente de usuario invalidado para la solicitud actual. |

Invalidación de explorador es una característica fundamental de ASP.NET MVC 4 y está disponible incluso si no instala el paquete jQuery.Mobile.MVC. Sin embargo, afecta a solo vista, diseño y selección de la vista parcial, no afecta a cualquier otra característica ASP.NET que depende el *Request.Browser* objeto.

De forma predeterminada, la invalidación de agente de usuario se almacena mediante una cookie. Si desea almacenar la invalidación en otro lugar (por ejemplo, en una base de datos), puede reemplazar el proveedor predeterminado (*BrowserOverrideStores.Current*). Documentación de este proveedor estarán disponible para acompañar a una versión posterior de ASP.NET MVC.

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Recetas para la generación de código en Visual Studio

La nueva característica de recetas permite que Visual Studio generar el código específico de la solución en función de los paquetes que se pueden instalar mediante NuGet. El marco de trabajo de recetas facilita a los desarrolladores escribir complementos de generación de código, que también puede usar para reemplazar los generadores de código integradas para agregar una área, Agregar controlador y agregar vista. Dado que se implementan las recetas como paquetes de NuGet, se pueden fácilmente proteger en el control de código fuente y comparten con todos los desarrolladores en el proyecto automáticamente. También están disponibles en una base de cada solución.

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Compatibilidad con la tarea para los controladores asincrónicos

Ahora puede escribir métodos de acción asincrónicos tan solo los métodos que devuelven un objeto de tipo *tarea* o *tarea&lt;ActionResult&gt;*.

Por ejemplo, si está utilizando Visual C# 5 (o mediante el [Async CTP](https://msdn.microsoft.com/vstudio/async.aspx)), puede crear un método de acción asincrónico es similar al siguiente:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

En el método de acción anterior, las llamadas a *newsService.GetHeadlinesAsync* y *sportsService.GetScoresAsync* se denominan de forma asincrónica y no bloquean un subproceso del grupo de subprocesos.

Métodos de acción asincrónicos que devuelven *tarea* instancias también admiten los tiempos de espera. Para hacer que el método de acción del objeto cancelable, agregue un parámetro de tipo *CancellationToken* a la signatura de método de acción. El ejemplo siguiente muestra un método de acción asincrónica que tiene un tiempo de espera de 2500 milisegundos y que muestra un *TimedOut* ver al cliente si se produce un tiempo de espera.

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>SDK de Azure

ASP.NET MVC 4 Beta es compatible con la versión 1.5 de septiembre de 2011 del SDK de Windows Azure.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conocidos y cambios importantes

- **Después de instalar la versión Beta de ASP.NET MVC 4, puede pausar el editor CSHTML y VBHTML en el editor de Visual Studio 2010 Service Pack 1 CSHTML y VBHTML durante mucho tiempo después de escribir el fragmento de código o JavaScript archivos cshtml y vbhtml.** Esto sucede solo en aplicaciones de ASP.NET MVC 4 que simplemente se han creado y que aún no se han compilado.

    La solución consiste en compilar el proyecto para obtener los ensamblados en la carpeta bin. Tenga en cuenta que si limpia el proyecto que quita los ensamblados de la carpeta bin, se devolverá el problema del editor.

    Esto se corregirá en la próxima versión.
- **Plantillas de proyecto de C# para Visual Studio 11 Beta contienen una cadena de conexión incorrecta de Global.asax.cs.** La conexión predeterminada especificada en la aplicación\_Start (método) para los proyectos creados en Visual Studio 11 Beta contienen una cadena de conexión de LocalDB que contiene una barra diagonal inversa sin escape (\) caracteres. Esto da como resultado un error de conexión al que intenta obtener acceso a un DbContext de Entity Framework, que genera una excepción SqlException.

    Para corregir este problema, el escape del carácter de barra diagonal inversa en la aplicación\_Start (método) de Global.asax.cs para que quede como sigue:

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **Las aplicaciones de ASP.NET MVC 4 que como destino .NET 4.5, producirán un FileLoadException tras el intento de obtener acceso al ensamblado System.Net.Http.dll cuando se ejecuta en .NET 4.0.** Las aplicaciones de ASP.NET MVC 4 creadas en .NET 4.5 contienen un enlace de redirección que dará como resultado un FileLoadException lo que indica que "no pudo cargar archivo o ensamblado 'System.Net.Http' o uno de sus dependencias." Cuando la aplicación se ejecuta en un sistema con .NET 4.0 instalado. Para corregir este problema, quite la siguiente redirección de enlace de web.config:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    El elemento de enlace de ensamblado en el archivo web.config modificado debería aparecer como sigue:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- <strong>La plantilla de elemento de "Agregar controlador" en proyectos de Visual Basic genera un espacio de nombres incorrecto cuando se invoca</strong><strong>desde dentro de un área.</strong> Cuando se agrega un controlador a un área en un proyecto de MVC de ASP.NET que utiliza Visual Basic, la plantilla de elemento inserta el espacio de nombres incorrecto en el controlador. El resultado es un error de "archivo no encontrado" al navegar a cualquier acción del controlador.  
  
  El espacio de nombres generado omite todo lo que después el espacio de nombres raíz. Por ejemplo, el espacio de nombres generado es *RootNamespace* pero debería ser *RootNamespace.Areas.AreaName.Controllers* .
- **Cambios importantes en el motor de vistas Razor.** Como parte de una reescritura del analizador de Razor, se han quitado los siguientes tipos de *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  También se han quitado los métodos siguientes: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Cuando WebMatrix.WebData.dll se incluye en el directorio/bin de un aplicaciones ASP.NET MVC 4, asume la dirección URL para la autenticación de formularios.** Agregar el ensamblado WebMatrix.WebData.dll a la aplicación (por ejemplo, seleccionando "ASP.NET Web Pages con Razor Syntax" cuando se usa el cuadro de diálogo Agregar dependencias implementables) invalidará el redireccionamiento de inicio de sesión de autenticación/cuenta/inicio de sesión en lugar de / cuenta/inicio de sesión según lo previsto, el valor predeterminado de cuenta de controlador de ASP.NET MVC. Para evitar este comportamiento y la dirección URL especificada ya en la sección de autenticación del archivo web.config, puede agregar un appSetting denominado PreserveLoginUrl y establézcalo en true: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **Se produce un error en el Administrador de paquetes de NuGet instalar al intentar instalar ASP.NET MVC 4 para las instalaciones en paralelo de Visual Studio 2010 y Visual Web Developer 2010.** Para ejecutar Visual Studio 2010 y Visual Web Developer 2010 en paralelo con ASP.NET MVC 4 debe instalar ASP.NET MVC 4 después de que ya se han instalado las dos versiones de Visual Studio.
- **Se produce un error al desinstalar ASP.NET MVC 4 si ya se han desinstalado los requisitos previos.** Para desinstalar correctamente ASP.NET MVC 4you debe desinstalar ASP.NET MVC 4 antes de desinstalar Visual Studio.
- **Ejecutar un proyecto de Web API predeterminada muestra instrucciones que indican incorrectamente al usuario que agregue rutas mediante el método RegisterApis, que no existe.** Las rutas deben agregarse en el método RegisterRoutes utilizando la tabla de rutas ASP.NET.
- **Instalar la versión Beta de ASP.NET MVC 4 interrumpe las aplicaciones de ASP.NET MVC 3 RTM.** Las aplicaciones de ASP.NET MVC 3 que se crearon con la versión RTM versión (no con el lanzamiento de ASP.NET MVC 3 Tools Update) requieren los siguientes cambios para poder trabajar en paralelo con ASP.NET MVC 4 Beta. Compilar el proyecto sin tener que realizar estos resultados de las actualizaciones en los errores de compilación. 

    **Actualizaciones necesarias**

  1. En el archivo raíz Web.config, agregue un nuevo *&lt;appSettings&gt;* entrada con la clave *webPages:Version* y el valor *1.0.0.0*.

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
  2. En el Explorador de soluciones, haga clic en el nombre del proyecto y, a continuación, seleccione Descargar proyecto. A continuación, haga clic en el nombre nuevo y seleccione Editar *ProjectName*.csproj.
  3. Busque las siguientes referencias de ensamblado: 

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

      ¿Desea reemplazarlos con lo siguiente:

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
  4. Guarde los cambios, cierre el archivo de proyecto (.csproj) estaba editando y, a continuación, haga clic en el proyecto y seleccione volver a cargar.
