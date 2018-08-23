---
uid: mvc/mvc5
title: ASP.NET MVC 5 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 5 ASP.NET MVC 5 es un marco para crear aplicaciones web escalable y basada en estándares con los patrones de diseño bien establecido y la eficacia de AS....
ms.author: riande
ms.date: 01/20/2014
ms.assetid: f79fbf7f-59e5-4279-a832-c1a0294630f4
msc.legacyurl: /mvc/mvc5
msc.type: content
ms.openlocfilehash: c837560e0ad9618decaba9761da9cf35e0f03f08
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41839166"
---
<a name="aspnet-mvc-5"></a>ASP.NET MVC 5
====================
## <a name="whats-new-in-aspnet-mvc-5"></a>Novedades de ASP.NET MVC 5

### <a name="one-aspnet"></a>One ASP.NET

Las plantillas de proyecto Web MVC se integran perfectamente con la nueva experiencia de One ASP.NET. Puede personalizar el proyecto MVC y configurar la autenticación mediante el Asistente para la creación del proyecto de One ASP.NET. Encontrará un tutorial de introducción a ASP.NET MVC 5 en [Introducción a ASP.NET MVC 5](overview/getting-started/introduction/getting-started.md).

Para obtener información sobre cómo actualizar proyectos de MVC 4 a MVC 5, vea [cómo actualizar un ASP.NET MVC 4 y un proyecto de API Web a ASP.NET MVC 5 y Web API 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Las plantillas de proyecto MVC se actualizaron para usar ASP.NET Identity para la autenticación y administración de identidades. Encontrará un tutorial que incluye autenticación de Facebook y Google y la nueva API de pertenencia en [crear una aplicación de ASP.NET MVC 5 con Facebook y Google OAuth2 y OpenID Sign-on](overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) y [implementar una aplicación ASP.NET MVC segura con Pertenencia, OAuth y SQL Database a un sitio Web de Azure de Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

### <a name="bootstrap"></a>Bootstrap

La plantilla de proyecto MVC se ha actualizado para usar [Bootstrap](http://getbootstrap.com/) para proporcionar una capacidad de respuesta y elegante apariencia que se puede personalizar fácilmente. Para obtener más información, consulte [Bootstrap en las plantillas de proyecto web de Visual Studio 2013](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Filtros de autenticación

[Los filtros de autenticación](http://www.dotnetcurry.com/showarticle.aspx?ID=957) son un nuevo tipo de filtro de ASP.NET MVC que se ejecutan antes de los filtros de autorización en la canalización de ASP.NET MVC y le permiten especificar la lógica de los autenticación por acción, por controlador o globalmente para todos los controladores. Los filtros de autenticación procesan credenciales en la solicitud y proporcionan una entidad de seguridad correspondiente. Los filtros de autenticación también pueden complicar el desafío de autenticación en respuesta a las solicitudes no autorizadas. Consulte [los filtros de autenticación de ASP.NET MVC 5](http://www.dotnetcurry.com/showarticle.aspx?ID=957), [los filtros de autenticación en ASP.NET MVC 5](http://theshravan.net/blog/authentication-filters-in-asp-net-mvc-5/).

### <a name="filter-overrides"></a>Invalidaciones de filtro

Ahora puede reemplazar los filtros que se aplican a un método de acción determinado o un controlador mediante la especificación de un [invalidar filtro](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5). Filtros de reemplazo especifican un conjunto de tipos de filtro que no se debe ejecutar para un ámbito determinado (acción o controlador). Esto le permite configurar los filtros que se aplican globalmente y, a continuación, exclusión determinados filtros globales de aplicar a controladores o acciones específicas. Consulte [invalidaciones de filtro nueva característica de ASP.NET MVC 5 y ASP.NET Web API 2](https://weblogs.asp.net/imranbaloch/archive/2013/09/25/new-filter-overrides-in-asp-net-mvc-5-and-asp-net-web-api-2.aspx), [cómo usar la característica de invalidaciones de filtro de ASP.NET MVC 5](http://hackwebwith.net/how-to-use-the-asp-net-mvc-5-filter-overrides-feature/), y [invalida el filtro en ASP.NET MVC 5](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)

### <a name="attribute-routing"></a>Enrutamiento mediante atributos

ASP.NET MVC ahora admite [enrutamiento mediante atributos](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx), gracias a una contribución de Tim McCall, el autor de [ http://attributerouting.net ](http://attributerouting.net). Con el enrutamiento mediante atributos puede especificar sus rutas anotando las acciones y los controladores.

## <a name="new-web-project-experience"></a>Nueva experiencia de proyecto Web

Hemos mejorado la experiencia de creación de nuevos proyectos web en Visual Studio 2013. En el **nuevo proyecto Web de ASP.NET** cuadro de diálogo puede seleccionar el tipo de proyecto que desee, configura cualquier combinación de tecnologías (formularios Web Forms, MVC, Web API), configura opciones de autenticación y agrega un proyecto de prueba unitaria.

![Nuevo proyecto de ASP.NET](mvc5/_static/image1.png)

El nuevo cuadro de diálogo le permite cambiar las opciones de autenticación predeterminado para muchas de las plantillas. Por ejemplo, cuando se crea un proyecto de formularios Web Forms ASP.NET puede seleccionar cualquiera de las siguientes opciones:

- Sin autenticación
- Cuentas de usuario individuales (pertenencia a ASP.NET o registro de proveedor social en)
- Cuentas de organización (Active Directory en una aplicación de internet)
- Autenticación de Windows (Active Directory en una aplicación de intranet)

![Opciones de autenticación](mvc5/_static/image2.png)

Para obtener más información acerca del nuevo proceso para crear proyectos web, consulte [Creating ASP.NET Web Projects in Visual Studio 2013](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md). Para obtener más información acerca de las nuevas opciones de autenticación, consulte [ASP.NET Identity](../identity/overview/index.md).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>Scaffolding de ASP.NET

Scaffolding de ASP.NET es un marco de generación de código para aplicaciones Web ASP.NET. Resulta muy sencillo agregar código reutilizable para el proyecto que interactúa con un modelo de datos.

En versiones anteriores de Visual Studio, estaba limitado a los proyectos de MVC de ASP.NET scaffolding. Con Visual Studio 2013, ahora puede usar el scaffolding para un proyecto ASP.NET, incluidos formularios Web Forms. Visual Studio 2013 no admite actualmente genera páginas para un proyecto de formularios Web Forms, pero todavía puede usar scaffolding con formularios Web Forms mediante la adición de dependencias de MVC al proyecto. Se agregará compatibilidad para generar páginas de formularios Web Forms en una futura actualización.

Cuando se usa la técnica scaffolding, nos aseguramos de que todos los necesarios a las dependencias se instalan en el proyecto. Por ejemplo, si se inicia con un proyecto de formularios Web Forms de ASP.NET y, a continuación, usar el scaffolding para agregar un controlador Web API, los paquetes de NuGet necesarios y las referencias se agregan al proyecto automáticamente.

Para agregar scaffolding de MVC a un proyecto de formularios Web Forms, agregue un **nuevo elemento de scaffolding** y seleccione **dependencias de MVC 5** en la ventana de cuadro de diálogo. Hay dos opciones para scaffolding de MVC; Mínimo y completa. Si selecciona mínima, sólo los paquetes de NuGet y referencias de ASP.NET MVC se agregan al proyecto. Si selecciona la opción completa, se agregan las dependencias mínimas, así como los archivos de contenido necesarios para un proyecto de MVC.

Compatibilidad con controladores asincrónicos de scaffolding usa las nuevas características asincrónicas de Entity Framework 6.

Para obtener más información y tutoriales, consulte [información general sobre el Scaffolding de ASP.NET](../visual-studio/overview/2013/aspnet-scaffolding-overview.md).

### <a name="getting-help-and-reporting-issues"></a>Obtención de ayuda y los informes de problemas

- [Problemas conocidos y la lista de los cambios de última hora](../visual-studio/overview/2013/release-notes.md#knownissues)
- Obtener ayuda y comente ASP.NET MVC 5 en el [foros](https://forums.asp.net/1146.aspx)
- [Notificar un error en ASP.NET MVC 5](https://github.com/aspnet/AspNetWebStack/issues)
- [Realizar una solicitud de característica](http://aspnet.uservoice.com/forums/41201-asp-net-mvc)

### <a name="upgrading-from-aspnet-mvc-4"></a>Actualización desde ASP.NET MVC 4

Vea [cómo actualizar ASP.NET MVC 4 y Web de proyecto de API a ASP.NET MVC 5 y Web API 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)
