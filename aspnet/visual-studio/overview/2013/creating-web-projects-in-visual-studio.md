---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: Crear proyectos Web ASP.NET en Visual Studio 2013 | Documentos de Microsoft
author: tdykstra
description: Este tema se explican las opciones para crear proyectos web ASP.NET en Visual Studio 2013 con Update 3 aquí son algunas de las nuevas características de c de desarrollo de web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/01/2014
ms.topic: article
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: aacae7a9ccf483b21d3c6796c0411d558fa3c75b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "28038870"
---
<a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Crear proyectos Web ASP.NET en Visual Studio 2013
====================
por [Tom Dykstra](https://github.com/tdykstra)

> En este tema se explica las opciones para crear proyectos web ASP.NET en Visual Studio 2013 con Update 3
> 
> Estas son algunas de las nuevas características para el desarrollo web en comparación con versiones anteriores de Visual Studio:
> 
> - Una interfaz de usuario simple para crear proyectos de dicha oferta [admite varios marcos de ASP.NET](#add) (formularios Web Forms, MVC y API Web).
> - [ASP.NET Identity](#indauth), un nuevo sistema de pertenencia ASP.NET que funciona igual en todos los marcos de trabajo ASP.NET y funciona con software que no sean IIS de hospedaje web.
> - El uso de [arranque](#bootstrap) para proporcionar capacidades de diseño y creación de temas con capacidad de respuesta.
> - Nuevas características de formularios Web Forms que solía ofrecerá sólo para MVC, como [creación del proyecto de prueba automáticas](#testproj) y [plantilla de sitio de Intranet](#winauth).
> 
> Para obtener información acerca de cómo crear un proyecto web de servicios de nube de Azure o servicios móviles de Azure, consulte [empezar a trabajar con servicios de nube de Azure y ASP.NET](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/) y [crear un Leaderboard App con .NET de servicios móviles de Azure Back-end](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/).


<a id="prerequisites"></a>
## <a name="prerequisites"></a>Requisitos previos

En este artículo se aplica a [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) con [Update 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) instalado.

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Proyectos de aplicación Web frente a proyectos de sitios web

ASP.NET permite elegir entre dos tipos de proyectos web: *proyectos de aplicación web* y *proyectos de sitios web*. Recomendamos que los proyectos de aplicación web para los nuevos desarrollos y este artículo se aplica solo a los proyectos de aplicación web. Para obtener más información, consulte [Web Application Projects versus Web Site Projects en Visual Studio](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx) en el sitio MSDN.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Información general sobre la creación del proyecto de aplicación web

Los pasos siguientes muestran cómo crear un proyecto web:

1. Haga clic en **nuevo proyecto** en el **iniciar** página o en la **archivo** menú.
2. En el **nuevo proyecto** cuadro de diálogo, haga clic en **Web** en el panel izquierdo y **aplicación Web ASP.NET** en el panel central.

    ![Cuadro de diálogo Nuevo proyecto](creating-web-projects-in-visual-studio/_static/image1.png)

    Puede elegir **nube** en el panel izquierdo para crear un [servicio de nube de Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), [servicios móviles de Azure](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx), o [WebJob de Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs). En este tema no se trata las plantillas.
3. En el panel derecho, haga clic en el **agregar Application Insights al proyecto** casilla de verificación si desea realizar la supervisión del estado y uso de la aplicación. Para obtener más información, consulte [supervisar el rendimiento en aplicaciones web](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/).
4. Especifica el proyecto **nombre**, **ubicación**y otras opciones y, a continuación, haga clic en **Aceptar**.

    El **nuevo proyecto ASP.NET** aparece el cuadro de diálogo.

    ![Cuadro de diálogo Nuevo proyecto](creating-web-projects-in-visual-studio/_static/image2.png)
5. Haga clic en una plantilla.

    ![Seleccione una plantilla](creating-web-projects-in-visual-studio/_static/image3.png)
6. Si desea agregar compatibilidad para los marcos adicionales no incluidas en la plantilla, haga clic en la casilla correspondiente. (En este ejemplo, se podría agregar MVC o API Web a un proyecto de formularios Web Forms.)

    ![Agregar marcos](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Si desea agregar un proyecto de prueba unitaria, haga clic en **agregar pruebas unitarias**.

    ![Agregar pruebas unitarias](creating-web-projects-in-visual-studio/_static/image5.png)
8. Si desea que un método de autenticación diferente que el que ofrece la plantilla predeterminada, haga clic en **Cambiar autenticación**.

    ![Autenticación botón configurar](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Configurar el cuadro de diálogo autenticación](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Crear una aplicación web o máquina virtual en Azure

Visual Studio incluye características que facilitan el trabajo con los servicios de Azure para hospedar aplicaciones web. Por ejemplo, puede hacer todo lo siguiente desde el IDE de Visual Studio:

- Crear y administrar las aplicaciones web o máquinas virtuales que forman la aplicación disponible a través de Internet.
- Ver los registros creados por la aplicación mientras se ejecuta en la nube.
- Trabajar en modo de depuración remota mientras la aplicación se ejecuta en la nube.
- Viiew y administrar otros servicios de Azure como bases de datos SQL.

También puede [crear una cuenta de Azure](https://www.windowsazure.com/pricing/free-trial/) que incluye los servicios básicos como aplicaciones web de forma gratuita, y si es un suscriptor MSDN puede [activar los beneficios](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) que le proporcionan los créditos mensuales que Azure adicionales servicios. 

De forma predeterminada el **nuevo proyecto ASP.NET** cuadro de diálogo le permite crear una aplicación web o una máquina virtual para un nuevo proyecto web. Si no desea crear una nueva aplicación web o una máquina virtual, desactive la **Host en la nube** casilla de verificación.

![Crear recursos remotos](creating-web-projects-in-visual-studio/_static/image8.png)

El título de la casilla de verificación podría ser **Host en la nube** o **crear recursos remotos**, y en cualquier caso, el efecto es el mismo. Si deja activada la casilla, Visual Studio crea una aplicación web en el servicio de aplicación de Azure de forma predeterminada. Puede usar el cuadro de lista desplegable para cambiar a un **Máquina Virtual** si lo prefiere. Si aún no ha iniciado sesión en Azure, se le pide las credenciales de Azure. Después de iniciar la sesión, un cuadro de diálogo permite configurar los recursos que va a crear para el proyecto de Visual Studio. La ilustración siguiente muestra el cuadro de diálogo para una aplicación web; Si decide crear una máquina virtual, aparecen distintas opciones.

![Configurar la aplicación de Azure](creating-web-projects-in-visual-studio/_static/image9.png)

Para obtener más información acerca de cómo utilizar este proceso para crear recursos de Azure, consulte [Introducción a Azure y ASP.NET](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) y [crear una máquina virtual para un sitio web con Visual Studio](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).

El resto de este artículo proporciona más información acerca de las plantillas disponibles y sus opciones. El artículo también introduce Bootstrap, el diseño y la creación de temas framework usada en las plantillas.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Plantillas de proyecto Web de Visual Studio 2013

Visual Studio usa plantillas para crear un proyecto web. Una plantilla de proyecto puede crear archivos y carpetas en el nuevo proyecto, instalar paquetes de NuGet y proporciona código de ejemplo para una aplicación operativa rudimentario. Plantillas de implementan los estándares web más recientes y están diseñadas para demostrar las prácticas recomendadas sobre cómo usar tecnologías ASP.NET, además de proporcionarle un salto iniciar acerca de cómo crear su propia aplicación.

Visual Studio 2013 proporciona las siguientes opciones para las plantillas de proyecto web para los proyectos destinados a .NET 4.5 o versiones posteriores de .NET framework:

- [Plantilla vacía](#empty)
- [Plantilla de Web Forms](#wf)
- [Plantilla de MVC](#mvc)
- [Plantilla de Web API](#webapi)
- [Plantilla de aplicación de página única](#spa)
- [Plantilla de servicio móvil de Azure](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Plantillas de Visual Studio 2012](#vs2012)

También puede instalar una extensión de Visual Studio que proporciona un [plantilla de Facebook](#facebook).

Para obtener información sobre cómo crear proyectos que tienen como destino .NET 4, consulte [plantillas de Visual Studio 2012](#vs2012) más adelante en este tema.

Para obtener información sobre cómo crear aplicaciones de ASP.NET para clientes móviles, consulte [Mobile Support en ASP.NET](../../../mobile/index.md).

<a id="empty"></a>
### <a name="empty-template"></a>Plantilla vacía

La plantilla vacía proporciona la reconstrucción mínimo de carpetas y archivos para una aplicación web ASP.NET, como un archivo de proyecto (*.csproj* o. *vbproj*) y un *Web.config* archivo. Puede agregar compatibilidad con formularios Web Forms, MVC o API Web mediante las casillas de verificación en la **agregar carpetas y principales de referencias para:** etiqueta.

Para la plantilla vacía opciones de autenticación no están disponibles. Funcionalidad de autenticación se implementa en aplicaciones de ejemplo y la plantilla vacía no crea una aplicación de ejemplo.

<a id="wf"></a>
### <a name="web-forms-template"></a>Plantilla de Web Forms

Los formularios Web Forms framework proporciona las siguientes características que permiten rápidamente compilación sitios web que tienen una gran en los datos y la interfaz de usuario tener acceso a características:

- Un diseñador WYSIWYG en Visual Studio.
- Controles de servidor que representan HTML y que puede personalizar mediante el establecimiento de propiedades y estilos.
- Una gran variedad de controles de acceso a datos y presentación de datos.
- Un modelo de eventos que expone eventos que se pueden programar como debe programar una aplicación cliente, como WPF.
- Mantenimiento automático de estado (datos) entre las solicitudes HTTP.

En general, la creación de una aplicación de formularios Web Forms requiere menos esfuerzo de programación de la creación de la misma aplicación mediante el marco de MVC de ASP.NET. Sin embargo, formularios Web Forms no es solo para el desarrollo rápido de aplicaciones. Hay muchas aplicaciones comerciales complejas y marcos de trabajo que se basa en formularios Web Forms.

Dado que una página de formularios Web Forms y los controles en la página generan automáticamente gran parte de la marca que se envía al explorador, no tiene el tipo de control más preciso sobre el código HTML que ofrece ASP.NET MVC. El modelo declarativo para la configuración de páginas y controles minimiza la cantidad de código que tiene que escribir pero oculta parte del comportamiento de HTML y HTTP. Por ejemplo, no siempre es posible especificar exactamente qué marcado podría generarse un control.

El marco de trabajo de formularios Web Forms no se presta tan fácilmente como MVC de ASP.NET con las prácticas de desarrollo basados en patrones como [desarrollo controlado por pruebas](http://en.wikipedia.org/wiki/Test-driven_development), [separación de intereses](http://en.wikipedia.org/wiki/Separation_of_concerns), [inversión de control](http://en.wikipedia.org/wiki/Inversion_of_control), y [inyección de dependencia](http://en.wikipedia.org/wiki/Dependency_injection). Si desea escribir código tenerse en cuenta este modo, puede hacerlo; pero no es tan automático porque está en el marco de MVC de ASP.NET. El [MVP de ASP.NET Web Forms](http://webformsmvp.com/) project muestra un enfoque que facilita la separación de aspectos y capacidad de prueba al tiempo que mantiene el rápido desarrollo formularios Web Forms se ha diseñado para entregar. Microsoft SharePoint está basado en Web Forms MVP.

La plantilla de formularios Web Forms crea una aplicación de formularios Web Forms de ejemplo que usa [arranque](#bootstrap) para proporcionar características de diseño y creación de temas con capacidad de respuesta. En la siguiente ilustración muestra la página principal.

![Página de inicio de aplicación de Web Forms plantilla](creating-web-projects-in-visual-studio/_static/image10.png)

Para obtener más información acerca de formularios Web Forms, vea [formularios Web Forms de ASP.NET](https://asp.net/web-forms). Para obtener información sobre lo que hace la plantilla de formularios Web Forms para usted, consulte [generar una aplicación básica de formularios Web Forms con Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>Plantilla de MVC

ASP.NET MVC se diseñó para facilitar las prácticas de desarrollo basada en modelos como [desarrollo controlado por pruebas](http://en.wikipedia.org/wiki/Test-driven_development), [separación de intereses](http://en.wikipedia.org/wiki/Separation_of_concerns), [inversión de control](http://en.wikipedia.org/wiki/Inversion_of_control), y [inyección de dependencia](http://en.wikipedia.org/wiki/Dependency_injection). El marco anima a separar la capa de lógica empresarial de una aplicación web desde su nivel de presentación. Al dividir la aplicación en modelos (M), vistas (V) y controladores (C), ASP.NET MVC puede ser de ayuda administrar la complejidad de aplicaciones de mayor tamaño.

Con ASP.NET MVC, trabajar más fácilmente con HTML y HTTP que en formularios Web Forms. Por ejemplo, formularios Web Forms automáticamente puede conservar el estado entre las solicitudes HTTP, pero tiene codificar de forma explícita en MVC. La ventaja del modelo MVC es que le permite tomar el control completo sobre exactamente lo que está haciendo la aplicación y el comportamiento en el entorno web. La desventaja es que tiene que escribir más código.

MVC se diseñó para ser extensible, ofrece a los desarrolladores de power la capacidad de personalizar el marco de trabajo para sus necesidades de aplicación. Además, el código fuente de ASP.NET MVC está disponible en una licencia OSI.

La plantilla MVC crea una aplicación de MVC 5 de ejemplo que usa [arranque](#bootstrap) para proporcionar características de diseño y creación de temas con capacidad de respuesta. En la siguiente ilustración muestra la página principal.

![Aplicación de ejemplo MVC](creating-web-projects-in-visual-studio/_static/image11.png)

Para obtener más información sobre MVC, vea [ASP.NET MVC](https://asp.net/mvc). Para obtener información sobre cómo seleccionar la plantilla de MVC 4, consulte [plantillas de Visual Studio 2012](#vs2012) más adelante en este artículo.

<a id="webapi"></a>
### <a name="web-api-template"></a>Plantilla de la API de Web

La plantilla API Web crea un servicio web de ejemplo basado en API Web, incluidas las páginas de Ayuda de API basadas en MVC.

ASP.NET Web API es un marco que facilita la creación de servicios HTTP que llegan a una amplia gama de clientes, incluidos los exploradores y dispositivos móviles. ASP.NET Web API es una plataforma ideal para compilar servicios RESTful en .NET Framework.

La plantilla API Web crea un servicio web de ejemplo. Las ilustraciones siguientes muestran las páginas de Ayuda de ejemplo.

![Página de Ayuda de Web API](creating-web-projects-in-visual-studio/_static/image12.png)

![Página de Ayuda de Web API para obtener API](creating-web-projects-in-visual-studio/_static/image13.png)

Para obtener más información sobre la API Web, consulte [ASP.NET Web API](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Plantilla de aplicación de página única

La plantilla de aplicación de página única (SPA) crea una aplicación de ejemplo que usa JavaScript, HTML 5, y [KnockoutJS](http://knockoutjs.com/) en el cliente y ASP.NET Web API en el servidor.

Es la única opción de autenticación para la plantilla SPA [cuentas de usuario individuales](#indauth).

En la siguiente ilustración muestra el estado inicial de la aplicación de ejemplo que crea la plantilla SPA.

![Aplicación de ejemplo SPA](creating-web-projects-in-visual-studio/_static/image14.png)

Para obtener información sobre cómo crear una aplicación mediante la plantilla SPA, consulte [API Web - Servicios de autenticación externos](../../../web-api/overview/security/external-authentication-services.md).

Para obtener más información acerca de las aplicaciones de ASP.NET única página y plantillas adicionales de SPA que utilicen marcos de JavaScript que no sea KnockoutJS, vea los siguientes recursos:

- [Página de ASP.NET sencilla aplicación](../../../single-page-application/index.md).
- [Descripción de las características de seguridad de la plantilla SPA de RC de VS2013](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Página de aplicaciones: Compilar aplicaciones Web modernas y capacidad de respuesta con ASP.NET](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Plantilla de Facebook

Puede instalar un [extensión de Visual Studio que proporciona una plantilla de Facebook](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409). Esta plantilla crea una aplicación de ejemplo está diseñada para ejecutarse en el sitio web de Facebook. Se basa en ASP.NET MVC y utiliza la API de Web para la funcionalidad de actualización en tiempo real.

No hay opciones de autenticación están disponibles para la plantilla de Facebook debido a las aplicaciones de Facebook se ejecutan en el sitio de Facebook y se basan en la autenticación de Facebook.

Para obtener más información acerca de las aplicaciones de Facebook de ASP.NET, vea [actualizar la API de Facebook MVC](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Plantillas de Visual Studio 2012

El cuadro de diálogo de creación de proyectos de Visual Studio 2013 web no proporciona acceso a algunas plantillas que estaban disponibles en Visual Studio 2012. Si desea utilizar una de estas plantillas, puede hacer clic en el nodo de Visual Studio 2012 en el panel izquierdo del cuadro de diálogo nuevo proyecto de Visual Studio.

![Plantillas de Visual Studio 2012](creating-web-projects-in-visual-studio/_static/image15.png)

El **Visual Studio 2012** nodo le permite seleccionar las siguientes plantillas de web que no tienen acceso a en la lista predeterminada de plantillas de Visual Studio 2013:

- Aplicación Web de ASP.NET MVC 4
- Aplicación Web de entidades de datos dinámicos de ASP.NET
- Control de servidor de AJAX de ASP.NET
- Extensor del Control de servidor de AJAX de ASP.NET
- Control de servidor ASP.NET

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Arrancar en las plantillas de proyecto web de Visual Studio 2013

Usan las plantillas de proyecto de Visual Studio 2013 [arranque](http://getbootstrap.com/), un marco de trabajo de diseño y creación de temas creado por Twitter. Bootstrap usa CSS3 para proporcionar un diseño dinámico, lo que significa diseños dinámicamente pueden adaptarse a los tamaños de ventana de explorador diferente. Por ejemplo, en una ventana del explorador amplia la página de inicio creada por la plantilla de formularios Web Forms es similar a la siguiente ilustración:

![Página de inicio de aplicación de Web Forms plantilla](creating-web-projects-in-visual-studio/_static/image16.png)

Asegúrese de la ventana más estrechas y mover las columnas horizontalmente organizadas en disposición vertical:

![Organización de arranque columna vertical](creating-web-projects-in-visual-studio/_static/image17.png)

Restringir un poco más de la ventana y el menú superior horizontal se convierte en un icono que puede hacer clic para expandir en un menú una orientación vertical:

![Icono de menú de arranque](creating-web-projects-in-visual-studio/_static/image18.png)

![Arranque menú vertical](creating-web-projects-in-visual-studio/_static/image19.png)

También puede utilizar la característica de temas del arranque para llevar a cabo fácilmente un cambio en la apariencia y funcionamiento de la aplicación. Por ejemplo, puede hacer lo siguiente para cambiar el tema.

1. En el explorador, vaya a [ http://Bootswatch.com ](http://Bootswatch.com), elija un tema y, a continuación, haga clic en **descargar**. (Este paquete descargará *bootstrap.min.css* de forma predeterminada; si desea examinar el código CSS, obtener *bootstrap.css* en lugar de la versión reducida.)
2. Copie el contenido del archivo CSS descargado.
3. En Visual Studio, cree un nuevo **hoja de estilos** archivo denominado *bootstrap-theme.css* en el *contenido* carpeta y pegue el código CSS descargado de código en él.
4. Abra *aplicación\_Start/Bundle.config* y cambiar *bootstrap.css* a *bootstrap-theme.css*.

Vuelva a ejecutar el proyecto y la aplicación tiene un nuevo aspecto. En la siguiente ilustración muestra el efecto del tema Amelia:

![Tema Amelia arranque](creating-web-projects-in-visual-studio/_static/image20.png)

Existen muchos temas de arranque, versiones gratuitas y premium. Bootstrap también ofrece una amplia variedad de componentes de interfaz de usuario, como [listas desplegables](http://twitter.github.io/bootstrap/components.html#dropdowns), [botón grupos](http://twitter.github.io/bootstrap/components.html#buttonGroups), y [iconos](http://twitter.github.io/bootstrap/base-css.html#images). Para obtener más información acerca de arranque, consulte [el sitio de arranque](http://twitter.github.io/bootstrap/).

Si utiliza el Diseñador de formularios Web Forms en Visual Studio, tenga en cuenta que el diseñador no es compatible con CSS3, por lo que no muestra con precisión todos los efectos de arranque temas o cambios de diseño dinámico. Sin embargo, las páginas de formularios Web Forms mostrará correctamente cuando se ven con un explorador.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Agregar compatibilidad para marcos de trabajo adicionales

Cuando selecciona una plantilla, automáticamente se selecciona la casilla de verificación para el framework(s) utilizado por la plantilla. Por ejemplo, si selecciona el **formularios Web Forms** plantilla, el **formularios Web Forms** casilla está activada y no se puede desactivar.

![Seleccione una plantilla](creating-web-projects-in-visual-studio/_static/image21.png)

![Agregar marcos](creating-web-projects-in-visual-studio/_static/image22.png)

Puede activar la casilla de verificación para un marco de trabajo que no se incluye en la plantilla con el fin de agregar compatibilidad para ese marco de trabajo cuando se crea el proyecto. Por ejemplo, para habilitar el uso de formularios Web Forms *.aspx* páginas cuando haya seleccionado la plantilla MVC, seleccione la **formularios Web Forms** casilla de verificación. O para habilitar MVC cuando se usa la plantilla de formularios Web Forms, haga clic en el **MVC** casilla de verificación. Agregar un marco de trabajo habilita la compatibilidad de tiempo de diseño como en tiempo de ejecución. Por ejemplo, si agrega compatibilidad de MVC a un proyecto de formularios Web Forms, podrá aplicar la técnica scaffolding controladores y vistas.

Si combinar formularios Web Forms y MVC en un proyecto y habilite [direcciones URL descriptivas](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) en formularios Web Forms, puede haber inesperado enrutamiento problemas en una dirección URL tiene varios destinos posibles. Las rutas que se definen en primer lugar tendrá prioridad. Por ejemplo, si tiene un `Home` controlador y un *Home.aspx* página, el `http://contoso.com/home` dirección URL será dirigido a *Home.aspx* si se llama a la `EnableFriendlyUrls` método antes de llamar a la `MapRoute`método *RouteConfig.cs*, o la misma dirección URL será dirigido a la vista predeterminada para su `Home` controlador si se llama a `MapRoute` antes de `EnableFriendlyUrls`.

Agregar un marco de trabajo no agrega ninguna funcionalidad de la aplicación de ejemplo. Por ejemplo, si agrega formularios Web Forms admiten cuando haya seleccionado la plantilla MVC, ya no *Default.aspx* se crea el archivo de página principal. Se agregan las carpetas, archivos y referencias necesarias para admitir el marco de trabajo. Por esta razón, agregar marcos no cambia las opciones de autenticación, que se implementan mediante código en las aplicaciones de ejemplo creados mediante las plantillas. Por ejemplo, si selecciona la plantilla vacía y agregar formularios Web Forms o MVC admite, la **configurar autenticación** todavía se deshabilitará el botón.

Las secciones siguientes describen brevemente el efecto de cada casilla de verificación.

### <a name="add-web-forms-support"></a>Agregar compatibilidad de Web Forms

Crea vacía *aplicación\_datos* y *modelos* carpetas y una *Global.asax* archivo. Estos ya están creados por todas las plantillas que no sea la plantilla vacía, por lo que si se activa la casilla de formularios Web Forms produce ninguna diferencia de otras plantillas.

La plantilla de formularios Web Forms habilita URL preparadas de forma predeterminada, pero al agregar que formularios Web Forms admiten con otras plantillas activando la casilla de verificación de formularios Web Forms que direcciones URL descriptivas no se habilitan automáticamente.

### <a name="add-mvc-support"></a>Agregar compatibilidad con MVC

Instala los paquetes MVC, Razor y WebPages NuGet, crea vacía *aplicación\_datos*, *controladores*, *modelos*, y *vistas*carpetas, crea *aplicación\_iniciar* carpeta con *RouteConfig.cs* archivo y crea *Global.asax* archivo.

### <a name="add-web-api-support"></a>Agregar compatibilidad con la API Web

Instala los paquetes de Newtonsoft.Json NuGet y de WebApi, crea vacía *aplicación\_datos*, *controladores*, y *modelos* carpetas, crea  *Aplicación\_iniciar* carpeta con *WebApiConfig.cs* archivo y crea *Global.asax* archivo.

<a id="auth"></a>
## <a name="authentication-methods"></a>Métodos de autenticación

Visual Studio 2013 ofrece varias opciones de autenticación para las plantillas de formularios Web Forms, MVC y API Web:

- [Sin autenticación](#noauth)
- [Cuentas de usuario individuales](#indauth) (identidad de ASP.NET, anteriormente conocido como la pertenencia a ASP.NET)
- [Las cuentas organizativas](#orgauth) (Windows Server Active Directory o Azure Active Directory)
- [Autenticación de Windows](#winauth) (Intranet)

![Configurar el cuadro de diálogo autenticación](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Sin autenticación

Si selecciona **sin autenticación**, la aplicación de ejemplo no contendrá ninguna página web de inicio de sesión, no que indica que se ha registrado en la interfaz de usuario, clases de ninguna entidad para una base de datos de pertenencia y ninguna cadena de conexión para una base de datos de pertenencia.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Cuentas de usuario individuales

Si selecciona **cuentas de usuario individuales**, la aplicación de ejemplo se configurará para usar ASP.NET Identity (anteriormente conocido como la pertenencia a ASP.NET) para la autenticación de usuario. Identidad de ASP.NET permite a un usuario registrar una cuenta, mediante la creación de un nombre de usuario y una contraseña en el sitio o iniciando sesión con proveedores de redes sociales, como Facebook, Google, Microsoft Account o Twitter. El almacén de datos predeterminado para los perfiles de usuario en ASP.NET Identity es una base de datos de SQL Server LocalDB, que puede implementar en SQL Server o base de datos de SQL Azure para el sitio de producción.

En Visual Studio 2013, estas características son el mismo que en Visual Studio 2012, pero ha vuelto a escribir el código subyacente para el sistema de pertenencia ASP.NET. Ventajas de la nueva base de código incluyen lo siguiente:

- El nuevo sistema de pertenencia se basa en [OWIN](http://owin.org/) en lugar del módulo de autenticación de formularios de ASP.NET. Esto significa que puede usar el mismo mecanismo de autenticación si está usando formularios Web Forms o MVC en IIS, u hospeda a sí mismo Web API SignalR.
- La nueva base de datos de pertenencia está administrado por Entity Framework Code First y todas las tablas se representan mediante las clases de entidad que se pueden modificar. Esto significa que se pueden personalizar fácilmente el esquema de base de datos y web relacionados con el perfil de interfaz de usuario para que se adapte a sus necesidades, y puede implementar con facilidad las actualizaciones con Code First Migrations.

El nuevo sistema de pertenencia se implementa automáticamente en las nuevas plantillas y se puede implementar manualmente en cualquier proyecto cuyo destino es .NET 4.5 o posterior.

Identidad de ASP.NET es una buena opción si va a crear un sitio web de Internet que es principalmente para clientes externos. Si su organización usa Active Directory o Office 365 y desea crear un proyecto que permite inicio de sesión único para los empleados y socios comerciales, la **cuentas organizativas** opción podría ser una opción mejor.

Para obtener más información acerca de la opción de cuentas de usuario individuales, vea los siguientes recursos:

- [www.ASP.NET/Identity](../../../identity/index.md). Documentación acerca de la identidad de ASP.NET en el sitio web ASP.NET.
- [Crear una aplicación de ASP.NET MVC 5 con Facebook y Google OAuth2 y OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). También muestra cómo personalizar los datos de perfil de usuario.
- [Web API - Servicios de autenticación externos](../../../web-api/overview/security/external-authentication-services.md)
- [Agregar inicios de sesión externos a la aplicación de ASP.NET en Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Cuentas organizativas

Si selecciona **cuentas organizativas**, se configurará la aplicación de ejemplo para usar Windows Identity Foundation (WIF) para la autenticación basada en cuentas de usuario en Azure Active Directory (Azure AD, que incluye Office 365) o Windows Server Active Directory. Para obtener más información, consulte [opciones de autenticación de cuenta profesional](#orgauthoptions) más adelante en este tema.

<a id="winauth"></a>
### <a name="windows-authentication"></a>Autenticación de Windows

Si selecciona **autenticación de Windows**, se configurará la aplicación de ejemplo para usar el módulo de autenticación de Windows IIS para la autenticación. La aplicación mostrará el identificador de usuario y dominio de Active directory o la cuenta del equipo local que ha iniciado sesión en Windows, pero no incluyen el registro de usuario o registro en interfaz de usuario. Esta opción está pensada para sitios web de Intranet.

Como alternativa, puede crear un sitio de Intranet que usa autenticación de Active Directory eligiendo la [opción situada debajo de las cuentas organizativas local](#orgauthonprem). La opción de On-Premises usa Windows Identity Foundation (WIF) en lugar del módulo de autenticación de Windows. Algunos pasos adicionales necesarios para configurar la opción On-Premises, pero WIF habilita características que no están disponibles con el módulo de autenticación de Windows. Por ejemplo, con WIF puede configurar el acceso a la aplicación en Active Directory y consultar datos de directorio.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Opciones de autenticación de cuenta profesional

El **configurar autenticación** cuadro de diálogo ofrece varias opciones para la autenticación de cuentas de Windows Server Active Directory (AD) o Azure Active Directory (Azure AD, que incluye Office 365):

- [En la nube - única organización](#orgauthsingle) (Azure AD o AD mediante la integración de directorios con Azure AD)
- [En la nube - organización de varias](#orgauthmulti) (Azure AD o AD mediante la integración de directorios con Azure AD)
- [Local](#orgauthonprem) (AD)

Si desea probar con una de las opciones de Azure AD, pero no tiene una cuenta, [haga clic aquí para registrarse en una cuenta de Azure AD](https://go.microsoft.com/fwlink/?LinkId=309942).

> [!NOTE]
> Si elige una de las opciones de Azure AD, su proyecto requiere una base de datos y tiene que iniciar sesión en una cuenta de administrador global para el inquilino de Azure AD. Escriba el nombre y la contraseña de una cuenta profesional (por ejemplo, admin@contoso.onmicrosoft.com) que tiene permisos administrativos para el inquilino de Azure AD.
> 
> **No escriba las credenciales de una cuenta de Microsoft (por ejemplo, contoso@hotmail.com) en el cuadro de diálogo de inicio de sesión.**


<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>En la nube - autenticación de la organización único

![Autenticación de la organización único](creating-web-projects-in-visual-studio/_static/image24.png)

Elija esta opción si desea habilitar la autenticación de cuentas de usuario que se definen en un anuncio de Azure [inquilino](https://technet.microsoft.com/library/jj573650.aspx). Por ejemplo, el sitio es contoso.com y estará disponible para los empleados de la compañía de Contoso que se encuentran en el inquilino contoso.onmicrosoft.com. No podrá configurar Azure AD para permitir a los usuarios de otros inquilinos para tener acceso a la aplicación.

#### <a name="domain"></a>Dominio

Escriba el dominio de Azure AD que desea configurar la aplicación, por ejemplo: `contoso.onmicrosoft.com`. Si tiene una [dominio personalizado](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/), como `contoso.com` en lugar de `contoso.onmicrosoft.com`, puede especificar ese aquí.

#### <a name="access-level"></a>Nivel de acceso

Si la aplicación necesita consultar o actualizar información de directorio mediante la API Graph, elija **inicio de sesión único, leer datos de directorio** o **inicio de sesión único, leer y escribir datos de directorio**. En caso contrario, elija **Single Sign-On**. Para obtener más información, consulte [niveles de acceso de la aplicación](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) y [mediante la API de Graph para consultar Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx).

#### <a name="application-id-uri"></a>URI de Id. de la aplicación

De forma predeterminada, la plantilla crea una aplicación identificador URI automáticamente anexando el nombre del proyecto al dominio de Azure AD. Por ejemplo, si el nombre del proyecto es `Example` y el dominio es `contoso.onmicrosoft.com`, la aplicación se convierte en el URI de Id. de `https://contoso.onmicrosoft.com/Example`. Si desea especificar manualmente el identificador URI de la aplicación, expanda la **más opciones** sección y escriba el identificador URI de la aplicación en el cuadro de texto. La aplicación de identificador URI debe comenzar por `https://`.

De forma predeterminada, si una aplicación que ya se ha aprovisionado en Azure AD tiene la misma aplicación identificador URI como uno que usa Visual Studio para el proyecto, el proyecto se conectarán a la aplicación existente en lugar de aprovisionamiento de una nueva. Si desea que una nueva aplicación para aprovisionar en ese caso, desactive el **sobrescribir la entrada de la aplicación si ya existe uno con el mismo identificador** casilla de verificación.

Si el **sobrescribir** casilla de verificación está desactivada y Visual Studio busca una aplicación existente con la misma aplicación URI del Id., crea un nuevo URI anexando un número para el URI que se va a usar. Por ejemplo, suponga que es el nombre del proyecto `Example`, deja en blanco el cuadro de texto, desactiva la **sobrescribir** casilla de verificación y el inquilino de Azure AD ya tiene una aplicación con el identificador URI `https://contoso.onmicrosoft.com/Example`. En ese caso, se aprovisionará una nueva aplicación con una aplicación como identificador URI `https://contoso.onmicrosoft.com/Example_20130619330903`.

#### <a name="provisioning-the-application-in-azure-ad"></a>Aprovisionamiento de la aplicación en Azure AD

Para aprovisionar la aplicación en Azure AD o conectar el proyecto a una aplicación existente, Visual Studio necesita las credenciales de administrador global para el dominio. Al hacer clic en **Aceptar** en el **configurar autenticación** cuadro de diálogo, se le solicitará que escriba el nombre de usuario y la contraseña de administrador global para el dominio especificado. Más adelante, al hacer clic en **crear proyecto** en el **nuevo proyecto ASP.NET** cuadro de diálogo, Visual Studio aprovisiona la aplicación en Azure AD. Tenga en cuenta que como parte de este proceso de Visual Studio incrusta valores secreto de cliente en el archivo Web.config que expire un año después de su creación.

Para obtener información acerca de cómo crear aplicaciones que usan **Cloud - única organización** autenticación, vea los siguientes recursos:

- [Autenticación de Azure](../2012/windows-azure-authentication.md)
- [Agregar inicio de sesión a la aplicación Web con Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Desarrollo de aplicaciones ASP.NET con Azure Active Directory](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Proteger ASP.NET Web API con Azure AD y componentes de Microsoft OWIN](https://msdn.microsoft.com/magazine/dn463788.aspx)

Los tutoriales aún no se han actualizado para Visual Studio 2013; en Visual Studio 2013 proceso se automatiza algunos de los tutoriales de qué ayudará a hacer de forma manual.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>En la nube - autenticación de la organización de múltiples

![Autenticación múltiple de organización](creating-web-projects-in-visual-studio/_static/image25.png)

Elija esta opción si desea habilitar la autenticación de cuentas de usuario que se definen en Azure AD varios [inquilinos](https://technet.microsoft.com/library/jj573650.aspx). Por ejemplo, el sitio es contoso.com y estará disponible para los empleados de la compañía de Contoso que se encuentran en el inquilino contoso.onmicrosoft.com y empleados de la compañía de Fabrikam que están en el inquilino fabrikam.onmicrosoft.com.

La configuración que especifique y el paso de aprovisionamiento de aplicaciones son similares a [autenticación de la organización solo](#orgauthsingle).

Para obtener información acerca de cómo crear aplicaciones que usan **Cloud - organización múltiples** autenticación, vea los siguientes recursos:

- [Fácil integración de aplicaciones Web con Azure Active Directory, ASP.NET &amp; Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) en el blog del equipo de Active Directory.
- [Desarrollo de aplicaciones Web de varios inquilinos con Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx) tutorial. El tutorial aún no se ha actualizado para Visual Studio 2013; algunos de lo que el tutorial le lleva a cabo manualmente se automatiza en Visual Studio 2013.
- [Tiene que registrarse con su propia aplicación de ASP.NET varias organizaciones antes de poder firmar](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/). Blog de Vittorio Bertocci que explica cómo resolver un personas problema común encontrar al crear un proyecto que usa la autenticación de la organización.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>Autenticación de organización local

![Autenticación de organización local](creating-web-projects-in-visual-studio/_static/image26.png)

Elija esta opción si desea habilitar la autenticación de cuentas de usuario que se definen en servidor de Active Directory (AD) de Windows y no desea usar Azure AD. Puede usar esta opción para crear un sitio de Intranet o un sitio de Internet. Para un sitio de Internet, utilice servicios de federación de Active Directory (ADFS) para proporcionar acceso a AD. Para obtener más información, consulte [usar la opción de autenticación profesional (ADFS) local con ASP.NET en Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

Para un sitio de Intranet, como alternativa puede elegir [autenticación de Windows](#winauth) en lugar de esta opción. Para la opción de autenticación de Windows no tendrá que proporcionar una dirección URL de documento de metadatos. Sin embargo, la autenticación de Windows no le otorga la capacidad para controlar el acceso de aplicación en Active Directory o para consultar los datos de directorio.

#### <a name="on-premises-authority"></a>Autoridad local

Escriba una dirección URL que señala al documento de metadatos. El documento de metadatos contiene las coordenadas de la entidad. La aplicación usará esas coordenadas para dirigir el flujo del inicio de sesión web.

#### <a name="application-id-uri"></a>URI de Id. de la aplicación

Proporcione un URI único que AD puede usar para identificar esta aplicación, o déjela en blanco para permitir que Visual Studio cree uno.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Pasos siguientes

Este documento proporciona Ayuda básica para crear un nuevo proyecto web ASP.NET en Visual Studio 2013. Para obtener más información acerca del uso de Visual Studio para el desarrollo web, consulte [ https://www.asp.net/visual-studio/ ](../../index.md).
