---
uid: mvc/mvc5
title: ASP.NET MVC 5 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 5 ASP.NET MVC 5 es un marco para crear aplicaciones web escalable, basada en estándares con hacia patrones de diseño y la eficacia de AS....
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: f79fbf7f-59e5-4279-a832-c1a0294630f4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/mvc5
msc.type: content
ms.openlocfilehash: 1b3f920b51a70757ec0e20e36fa8e7dc329e663d
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
ms.locfileid: "30077925"
---
<a name="aspnet-mvc-5"></a>ASP.NET MVC 5
====================
## <a name="whats-new-in-aspnet-mvc-5"></a>Novedades de MVC de ASP.NET 5

### <a name="one-aspnet"></a>Uno de ASP.NET

Las plantillas de proyecto Web MVC se integren perfectamente con la nueva experiencia de One ASP.NET. Puede personalizar el proyecto de MVC y configurar la autenticación mediante el Asistente para la creación del proyecto de One ASP.NET. Encontrará un tutorial de introducción a ASP.NET MVC 5 en [Introducción a ASP.NET MVC 5](overview/getting-started/introduction/getting-started.md).

Para obtener información sobre cómo actualizar proyectos MVC 4 a 5 de MVC, vea [cómo actualizar un ASP.NET MVC 4 y el proyecto de API Web para ASP.NET MVC 5 y API Web 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Las plantillas de proyecto MVC se han actualizado para usar la identidad de ASP.NET para la autenticación y administración de identidades. Encontrará un tutorial que incluye autenticación de Facebook y Google y la nueva API de pertenencia en [crear una aplicación de ASP.NET MVC 5 con Facebook y Google OAuth2 y OpenID Sign-on](overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) y [implementar una aplicación de MVC de ASP.NET seguros con Base de datos SQL a un sitio Web de Azure de Windows, pertenencia y OAuth](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

### <a name="bootstrap"></a>bootstrap

La plantilla de proyecto MVC se ha actualizado para usar [arranque](http://getbootstrap.com/) para proporcionar un sensibles y elegante aspecto y diseño que se puede personalizar fácilmente. Para obtener más información, consulte [Bootstrap en las plantillas de proyecto web de Visual Studio 2013](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtros de autenticación

[Filtros de autenticación](http://www.dotnetcurry.com/showarticle.aspx?ID=957) son un nuevo tipo de filtro de MVC de ASP.NET que se ejecute antes que los filtros de autorización en la canalización de ASP.NET MVC y le permiten especificar la lógica de los autenticación por cada acción, por controlador, o globalmente para todos los controladores. Los filtros de autenticación procesan las credenciales en la solicitud y proporcionan una entidad de seguridad correspondiente. Filtros de autenticación también pueden agregar los desafíos de autenticación en respuesta a solicitudes no autorizadas. Vea [filtros de autenticación de ASP.NET MVC 5](http://www.dotnetcurry.com/showarticle.aspx?ID=957), [filtros de autenticación en ASP.NET MVC 5](http://theshravan.net/blog/authentication-filters-in-asp-net-mvc-5/).

### <a name="filter-overrides"></a>Invalidaciones de filtro

Ahora puede invalidar los filtros se aplican a un método de acción determinado o un controlador mediante la especificación de un [invalidar filtro](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5). Filtros de reemplazo especifican un conjunto de tipos de filtro que no se debe ejecutar para un ámbito determinado (acción o controlador). Esto le permite configurar los filtros que se aplican globalmente pero, a continuación, excluirán determinados filtros globales de la aplicación a acciones específicas o controladores. Vea [invalidaciones de filtro nueva característica de ASP.NET MVC 5 y ASP.NET Web API 2](https://weblogs.asp.net/imranbaloch/archive/2013/09/25/new-filter-overrides-in-asp-net-mvc-5-and-asp-net-web-api-2.aspx), [cómo usar la característica de invalidaciones de filtro de ASP.NET MVC 5](http://hackwebwith.net/how-to-use-the-asp-net-mvc-5-filter-overrides-feature/), y [filtro invalidaciones en ASP.NET MVC 5](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)

### <a name="attribute-routing"></a>Enrutamiento mediante atributos

ASP.NET MVC ahora admite [atributo enrutamiento](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx), gracias a una contribución por Tim McCall, el autor de [ http://attributerouting.net ](http://attributerouting.net). Con el enrutamiento de atributo puede especificar sus rutas anotando las acciones y los controladores.

## <a name="new-web-project-experience"></a>Nueva experiencia de proyecto Web

Se ha mejorado la experiencia de creación de nuevos proyectos web en Visual Studio 2013. En el **nuevo proyecto Web de ASP.NET** cuadro de diálogo puede seleccionar el tipo de proyecto desea, configure cualquier combinación de tecnologías (formularios Web Forms, MVC, Web API), configurar opciones de autenticación y agregar un proyecto de prueba unitaria.

![Nuevo proyecto de ASP.NET](mvc5/_static/image1.png)

El nuevo cuadro de diálogo permite cambiar las opciones de autenticación predeterminadas para muchas de las plantillas. Por ejemplo, cuando se crea un proyecto de formularios Web Forms de ASP.NET puede seleccionar cualquiera de las siguientes opciones:

- Sin autenticación
- Cuentas de usuario individuales (pertenencia a ASP.NET o registro del proveedor de redes sociales en)
- Cuentas de organización (Active Directory en una aplicación de internet)
- Autenticación de Windows (Active Directory en una aplicación de intranet)

![Opciones de autenticación](mvc5/_static/image2.png)

Para obtener más información acerca del nuevo proceso para crear proyectos web, vea [Creating ASP.NET Web Projects in Visual Studio 2013](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md). Para obtener más información acerca de las nuevas opciones de autenticación, vea [ASP.NET Identity](../identity/overview/index.md).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>Scaffolding de ASP.NET

La técnica de Scaffolding de ASP.NET es un marco de trabajo de generación de código para aplicaciones Web ASP.NET. Resulta muy sencillo agregar código reutilizable en el proyecto que interactúa con un modelo de datos.

En versiones anteriores de Visual Studio, scaffolding estaba limitado a los proyectos de ASP.NET MVC. Con Visual Studio 2013, ahora puede usar el scaffolding para cualquier proyecto de ASP.NET, incluidos los formularios Web Forms. Visual Studio 2013 no admite actualmente la generación de páginas para un proyecto de formularios Web Forms, pero todavía puede usar scaffolding con formularios Web Forms mediante la adición de las dependencias de MVC al proyecto. Se agregará compatibilidad para generar páginas de formularios Web Forms en una futura actualización.

Cuando se utiliza la técnica scaffolding, es asegurarse de que las necesarias están instaladas las dependencias en el proyecto. Por ejemplo, si empieza con un proyecto de formularios Web Forms de ASP.NET y, a continuación, usar el scaffolding para agregar un controlador de API Web, los paquetes de NuGet necesarios y las referencias se agregan al proyecto automáticamente.

Para agregar scaffolding de MVC a un proyecto de formularios Web Forms, agregue un **nuevo elemento de scaffolding** y seleccione **las dependencias de MVC 5** en el cuadro de diálogo. Hay dos opciones de scaffolding MVC; Mínimo y completa. Si selecciona mínimo, solo los paquetes de NuGet y referencias para ASP.NET MVC se agregan al proyecto. Si selecciona la opción completa, se agregan las dependencias mínimas, así como los archivos de contenido necesarios para un proyecto de MVC.

Compatibilidad con scaffolding controladores asincrónicos usa las nuevas características asincrónicas de Entity Framework 6.

Para obtener más información y ver tutoriales, vea [información general sobre la técnica Scaffolding de ASP.NET](../visual-studio/overview/2013/aspnet-scaffolding-overview.md).

### <a name="getting-help-and-reporting-issues"></a>Informes de problemas y obtención de ayuda

- [Problemas conocidos y lista de los cambios importantes](../visual-studio/overview/2013/release-notes.md#knownissues)
- Obtener ayuda y analizar 5 de MVC de ASP.NET en el [foros](https://forums.asp.net/1146.aspx)
- [Informar de un error en ASP.NET MVC 5](https://github.com/aspnet/AspNetWebStack/issues)
- [Realizar una solicitud de característica](http://aspnet.uservoice.com/forums/41201-asp-net-mvc)

### <a name="upgrading-from-aspnet-mvc-4"></a>Actualización de ASP.NET MVC 4

Vea [cómo actualizar un MVC de ASP.NET 4 y Web de proyecto de API para ASP.NET MVC 5 y Web API 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)
