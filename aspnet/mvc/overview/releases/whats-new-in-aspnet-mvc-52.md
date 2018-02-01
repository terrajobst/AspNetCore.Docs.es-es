---
uid: mvc/overview/releases/whats-new-in-aspnet-mvc-52
title: "' S New en ASP.NET MVC 5.2 | Documentos de Microsoft"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 97972587-2720-48b4-b158-f35f2e855fbf
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/whats-new-in-aspnet-mvc-52
msc.type: authoredcontent
ms.openlocfilehash: 94f6131fdb86d1694c1f563c5f6806f119c71266
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-mvc-52"></a>' S New en ASP.NET MVC 5.2
====================
por [Microsoft](https://github.com/microsoft)

Este tema describe lo que es nuevo en ASP.NET MVC 5.2, [Microsoft.AspNet.MVC 5.2.2](#52) y [Beta de ASP.NET MVC 5.2.3](#mvc523Beta)

- [Requisitos de software](#softRequire)
- [Descarga](#download)
- [Documentación](#documentation)
- [Nuevas características de ASP.NET MVC 5.2](#new-features)

    - [Atributo mejoras de enrutamiento](#attributerouting)
- [Problemas conocidos y los cambios recientes](#knownbreakingchanges)
- [Correcciones de errores](#bug-fixes)
- [Microsoft.AspNet.MVC 5.2.2](#52)

<a id="softRequire"></a>
## <a name="software-requirements"></a>Requisitos de software

- Visual Studio 2012: Descargar [Web ASP.NET y herramientas 2013.1 para Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013: Descargar [Visual Studio 2013 Update](https://go.microsoft.com/fwlink/?LinkId=390064) o superior. Esta actualización es necesaria para editar las vistas de Razor de ASP.NET MVC 5.2.

<a id="download"></a>
## <a name="download"></a>Descargar

Las características en tiempo de ejecución se publican como paquetes de NuGet en la Galería de NuGet. Siguen todos los paquetes en tiempo de ejecución el [control de versiones semántico](http://semver.org/) especificación. El paquete más reciente de ASP.NET MVC 5.2 tiene la siguiente versión: "5.2.0". Puede instalar o actualizar estos paquetes a través de [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). La versión también incluye paquetes localizados correspondientes en NuGet.

Puede instalar o actualizar a los paquetes de NuGet publicados mediante la consola de administrador de paquetes de NuGet:

Instalar paquete Microsoft.AspNet.Mvc-versión 5.2.0

<a id="documentation"></a>
## <a name="documentation"></a>Documentación

Tutoriales y otra información sobre ASP.NET MVC 5.2 están disponibles desde el sitio web ASP.NET ([https://www.asp.net/mvc](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-52"></a>Nuevas características de ASP.NET MVC 5.2

<a id="attributerouting"></a>
### <a name="attribute-routing-improvements"></a>Atributo mejoras de enrutamiento

Ruta de atributo ahora proporciona un punto de extensibilidad denominado IDirectRouteProvider, lo que permite un control total sobre el proceso de detección y configurar rutas de atributo. Un IDirectRouteProvider es responsable de proporcionar una lista de acciones y controladores junto con la información de ruta asociada para especificar exactamente qué configuración de enrutamiento se deseada para esas acciones. Al llamar a MapAttributes/MapHttpAttributeRoutes, se puede especificar una implementación de IDirectRouteProvider.

Personalizar IDirectRouteProvider será más fácil extendiendo nuestra implementación de forma predeterminada, DefaultDirectRouteProvider. Esta clase proporciona métodos virtuales reemplazables independientes para cambiar la lógica de detección de atributos, crear entradas de ruta y detectar el prefijo de ruta y el prefijo de área.

Con el nuevo atributo enrutamiento de extensibilidad de IDirectRouteProvider, un usuario podría hacer lo siguiente:

1. Admite la herencia de rutas de atributo. Por ejemplo, en el siguiente escenario Blog y el almacén de controladores están utilizando una convención de ruta de atributo definido por el BaseController. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample1.cs)]
2. Generar automáticamente los nombres de ruta para las rutas de atributo. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample2.cs)]
3. Modifique los prefijos de ruta en un lugar central antes de que las rutas se agregan a la tabla de rutas.
4. Filtrar los controladores en el que desea que el enrutamiento de atributo para buscar. Esperamos blog en 3 y 4 pronto.

### <a name="facebook-fixes-for-changed-api-surface"></a>Facebook correcciones para la superficie de la API modificada

El paquete de MVC Facebook [se interrumpió](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All) debido a algunos cambios en la API realizados por Facebook. También vamos a lanzar un nuevo paquete de Facebook (Microsoft.AspNet.Facebook 1.0.0) para solucionar estos problemas.

<a id="knownbreakingchanges"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemas conocidos y los cambios recientes

### <a name="scaffolding-mvcweb-api-into-a-project-with-520-packages-results-in-512-packages-for-ones-that-dont-already-exist-in-the-project"></a>Scaffolding MVC o Web API en un proyecto con 5.2.0 resultados de paquetes en 5.1.2 paquetes para los que ya no existen en el proyecto

Actualizar paquetes de NuGet para ASP.NET MVC 5.2.0 no actualizar las herramientas de Visual Studio como scaffolding de ASP.NET o la plantilla de proyecto de aplicación Web ASP.NET. Usan la versión anterior de los paquetes en tiempo de ejecución ASP.NET (por ejemplo, 5.1.2 en Update 2). Como resultado, el scaffolding de ASP.NET se instalará la versión anterior (por ejemplo, 5.1.2 en Update 2) de los paquetes necesarios, si aún no están disponibles en los proyectos. Sin embargo, el scaffolding de ASP.NET en Visual Studio 2013 RTM o Update 1 sobrescribir los paquetes más recientes de los proyectos. Si utiliza ASP.NET scaffolding después de actualizar los paquetes de los proyectos Web API 2.2 o 5.2 de MVC de ASP.NET, asegúrese de que las versiones de API Web y MVC de ASP.NET son coherentes.

### <a name="microsoftjqueryunobtrusivevalidation-nuget-package-installation-fails-because-it-is-unable-to-find-a-version-of-microsoftjqueryunobtrusivevalidation-compatible-to-jquery-141"></a>Se produce un error en la instalación del paquete de Microsoft.jQuery.Unobtrusive.Validation NuGet porque no puede encontrar una versión de Microsoft.jQuery.Unobtrusive.Validation compatible con jQuery 1.4.1

Microsoft.jQuery.Unobtrusive.Validation requiere jQuery &gt;= 1.8 y jQuery.Validation &gt;= 1.8. But,jQuery.Validation (1.8) necesita jQuery (&#8805; 1.3.2 &amp; &amp; &#8804; 1.6). Por este motivo, cuando NuGet instala los JQuery 1.8 y jQuery.Validation 1.8 al mismo tiempo, se produce un error. Cuando ve este problema, simplemente puede actualizar la versión de jQuery.Validation a &gt; =  [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1) que tiene el límite de jQuery fijo en primer lugar, debe poder instalar Microsoft.jQuery.Unobtrusive.Validation.

### <a name="the-jqueryvalidation-nuget-package-version-1130-does-not-recognize-some-international-email-addresses"></a>Jquery. Versión de paquete de nuget de validación 1.13.0 no reconoce algunas direcciones de correo electrónico internacionales

versión de paquete de nuget jQuery.Validation 1.11.1 es la última versión conocida que reconoce las siguientes direcciones de correo electrónico válida. Pueden que las versiones posteriores no pueda reconocerlos. Por ejemplo:

Estándar de internacionalización de dirección (EAI) de correo electrónico (p. ej., [&#29992; &#25143;@domain.com ](mailto:&#29992;&#25143;@domain.com))   
 EAI + identificadores de recursos internacionalizado (IRI) (p. ej., [&#29992; &#25143; @&#1076; &#1086; &#1084; &#1077; &#1085;. &#1088; &#1092; ](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))

El problema se notifica en [https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Resaltado de sintaxis para las vistas de Razor en Visual Studio 2013

Si actualiza a ASP.NET MVC 5.2 sin actualizar Visual Studio 2013, no obtendrá la compatibilidad con el editor de Visual Studio para resaltar la sintaxis mientras edita las vistas de Razor. Debe actualizar Visual Studio 2013 para obtener esta compatibilidad.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Correcciones de errores y actualizaciones de características secundarias

Esta versión también incluye varias correcciones de errores y una característica secundaria actualizaciones. Puede encontrar la lista completa aquí:

- [paquete 5.2](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<a id="52"></a>
## <a name="microsoftaspnetmvc-522"></a>Microsoft.AspNet.MVC 5.2.2

Esta versión no tiene las características nuevas o correcciones de errores en MVC. Hemos realizado una [cambiar en las páginas Web](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) para una mejora significativa del rendimiento y posteriormente se han actualizado todos los otros paquetes dependientes se posee para que dependa de esta nueva versión de las páginas Web.

<a id="mvc523Beta"></a>
## <a name="aspnet-mvc-523-beta"></a>Versión Beta de ASP.NET MVC 5.2.3

Puede leer acerca de la versión [aquí](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Esta versión contiene solo las correcciones de errores. Puede usar [esta consulta](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) para ver la lista de problemas corregidos en esta versión.
