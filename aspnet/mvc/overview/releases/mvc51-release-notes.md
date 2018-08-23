---
uid: mvc/overview/releases/mvc51-release-notes
title: Novedades de ASP.NET MVC 5.1 | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 6cdf5ae42457ab10e30693a1b77b4aee65570be5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828640"
---
<a name="whats-new-in-aspnet-mvc-51"></a>Novedades de ASP.NET MVC 5.1
====================
por [Microsoft](https://github.com/microsoft)

Este tema describe cuáles son las novedades para ASP.NET Web MVC 5.1.

- [Requisitos de software](#SoftwareRequirements)
- [Descarga](#download)
- [Documentación](#documentation)
- [Nuevas características de ASP.NET MVC 5.1](#new-features)

    - [Mejoras de enrutamiento de atributo](#AttributeRouting)
    - [Compatibilidad de arranque para plantillas de editor](#Bootstrap)
    - [Compatibilidad de enumeraciones en las vistas](#Enum)
    - [Validación discreta de los atributos de MinLength/MaxLength](#Unobtrusive)
    - [Compatibilidad con el contexto 'this' en Ajax discreto](#thisContext)
- [Problemas conocidos y cambios importantes](#KnownBreakingChanges)- [correcciones de errores](#bug-fixes)

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a>Requisitos de software

- Visual Studio 2012: Descargar [ASP.NET y Web Tools 2013.1 para Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013: Descarga [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064). Esta actualización es necesaria para editar las vistas de Razor de ASP.NET MVC 5.1.

<a id="download"></a>
## <a name="download"></a>Descargar

Las características en tiempo de ejecución se publican como paquetes de NuGet en la Galería de NuGet. Siguen todos los paquetes en tiempo de ejecución el [Versionamiento semántico](http://semver.org/) especificación. El paquete más reciente de ASP.NET MVC 5.1 RTM tiene la siguiente versión: "5.1.2". Puede instalar o actualizar estos paquetes a través de [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). La versión también incluye los correspondientes paquetes localizados en NuGet.

Puede instalar o actualizar a los paquetes de NuGet publicados mediante la consola de administrador de paquetes de NuGet:

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentación

Tutoriales y otra información sobre ASP.NET MVC 5.1 RTM están disponibles desde el sitio web ASP.NET ( https://www.asp.net). 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a>Nuevas características de ASP.NET MVC 5.1

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a>Mejoras de enrutamiento de atributo

 Enrutamiento ahora es compatible con restricciones, habilitar el control de versiones y el encabezado de atributos en función de selección de la ruta. Muchos aspectos de las rutas de atributo ahora son personalizables a través de la `IDirectRouteFactory` interfaz y `RouteFactoryAttribute` clase. El prefijo de ruta ahora es extensible a través de la `IRoutePrefix` interfaz y `RoutePrefixAttribute` clase. 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a>Compatibilidad de enumeraciones en las vistas

1. Nuevo `@Html.EnumDropDownListFor()` métodos auxiliares. Deben utilizarse como la mayoría de las aplicaciones auxiliares HTML con la salvedad de que la expresión debe evaluarse como un [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) tipo o un [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) donde `T` es un [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) tipo. Use `EnumHelper.IsValidForEnumHelper()` para comprobar estos requisitos.
2. Nuevo `EnumHelper.GetSelectList()` métodos que devuelven un `IList<SelectListItem>`. Esto es útil cuando deba manipular una lista de selección antes de llamar a, por ejemplo, `@Html.DropDownListFor()`, o cuando desee mostrar los nombres que `@Html.EnumDropDownListFor()` muestra.

El código siguiente muestra estas API.

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

Puede ver un ejemplo completo [aquí](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a>Compatibilidad de arranque para plantillas de editor

Ahora permitimos pasar en los atributos HTML en [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) como un [objeto anónimo](https://msdn.microsoft.com/en-us/library/bb397696.aspx).

Por ejemplo:

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a>Validación discreta para MinLengthAttribute y MaxLengthAttribute

Ahora, se admitirá la validación del lado cliente para los tipos string y array para Propiedades decoran con el [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) y [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) atributos.

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a>Compatibilidad con el contexto 'this' en Ajax discreto

Las funciones de devolución de llamada (`OnBegin, OnComplete, OnFailure, OnSuccess`) ahora podrán localizar el elemento de invocación a través de la `this` contexto. Por ejemplo:

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a>Problemas conocidos y cambios importantes

### <a name="attribute-routing"></a>Enrutamiento mediante atributos

Ambigüedades en coincidencias de enrutamiento de atributos ahora notificará un error en lugar de elegir a la primera coincidencia.

Las rutas de atributo se prohíbe la `{controller}` parámetro y del uso de la `{action}` parámetro en las rutas se coloca en las acciones. Usos de estos parámetros es muy probable que daría lugar a ambigüedades. 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Scaffolding de MVC o API Web en un proyecto con 5.1 da como resultado paquetes 5.0 paquetes para las que aún no existen en el proyecto

Actualizar paquetes de NuGet para ASP.NET MVC 5.1 RTM no actualiza las herramientas de Visual Studio como el scaffolding de ASP.NET o la plantilla de proyecto de aplicación Web ASP.NET. Usan la versión anterior de los paquetes en tiempo de ejecución ASP.NET (5.0.0.0). Como resultado, el scaffolding de ASP.NET se instalará la versión anterior (5.0.0.0) de los paquetes necesarios, si aún no están disponibles en los proyectos. Sin embargo, el scaffolding de ASP.NET en Visual Studio 2013 RTM o Update 1 no sobrescribe los paquetes más recientes en sus proyectos. Si usa scaffolding de ASP.NET después de actualizar los paquetes de los proyectos de Web API 2.1 o ASP.NET MVC 5.1, asegúrese de que las versiones de API Web y ASP.NET MVC son coherentes. 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Resaltado de sintaxis para las vistas de Razor en Visual Studio 2013

Si actualiza a ASP.NET MVC 5.1 RTM sin actualizar Visual Studio 2013, no obtendrá compatibilidad con el editor Visual Studio para resaltar la sintaxis mientras se editan las vistas de Razor. Deberá actualizar Visual Studio 2013 para obtener esta compatibilidad. 

### <a name="type-renames"></a>Cambia el nombre de tipo

Algunos de los tipos utilizados para la extensibilidad de enrutamiento de atributos se cambia el nombre 5.1 RTM.

| **Nombre de tipo anterior (5.1 RC)** | **Nuevo tipo de nombre (5.1 RTM)** |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Correcciones de errores

Esta versión también incluye varias correcciones de errores. Puede encontrar una lista completa aquí:

- [5.1.0 paquete](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 paquete](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

El 5.1.2 paquete contiene actualizaciones de IntelliSense, pero no hay correcciones de errores.
