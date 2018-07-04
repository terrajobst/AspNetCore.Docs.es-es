---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: Creación de proyectos Web de ASP.NET en Visual Studio 2013 | Microsoft Docs
author: tdykstra
description: En este tema se explica las opciones para crear proyectos web de ASP.NET en Visual Studio 2013 con Update 3 aquí son algunas de las nuevas características de c de desarrollo web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/01/2014
ms.topic: article
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 6033163b7b8e9e1d0524935217dc53f938470cb6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363473"
---
<a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Crear proyectos Web ASP.NET en Visual Studio 2013
====================
por [Tom Dykstra](https://github.com/tdykstra)

> En este tema se explica las opciones para crear proyectos web de ASP.NET en Visual Studio 2013 con Update 3
> 
> Estas son algunas de las nuevas características para el desarrollo web en comparación con versiones anteriores de Visual Studio:
> 
> - Una interfaz de usuario simple para crear proyectos de dicha oferta [admite varios marcos de ASP.NET](#add) (formularios Web Forms, MVC y Web API).
> - [ASP.NET Identity](#indauth), un nuevo sistema de pertenencia ASP.NET que funciona igual en todos los marcos de ASP.NET y funciona con el software que no sean IIS de hospedaje web.
> - El uso de [Bootstrap](#bootstrap) para proporcionar capacidades de diseño y creación de temas con capacidad de respuesta.
> - Nuevas características para formularios Web Forms que se proporcionan solo para MVC, tales como [creación del proyecto de pruebas automáticas](#testproj) y un [plantilla de sitio de Intranet](#winauth).
> 
> Para obtener información acerca de cómo crear un proyecto web para Azure Cloud Services o Azure Mobile Services, vea [empezar a trabajar con Azure Cloud Services y ASP.NET](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/) y [creación de un App Leaderboard con .NET de Azure Mobile Services Back-end](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/).


<a id="prerequisites"></a>
## <a name="prerequisites"></a>Requisitos previos

En este artículo se aplica a [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) con [Update 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) instalado.

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Proyectos de aplicación Web frente a proyectos de sitio web

ASP.NET le permite elegir entre dos tipos de proyectos web: *proyectos de aplicación web* y *proyectos de sitios web*. Se recomienda proyectos de aplicación web para el desarrollo nuevo, y este artículo se aplica solo a los proyectos de aplicación web. Para obtener más información, consulte [proyectos de aplicación Web frente a proyectos de sitio Web en Visual Studio](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx) en el sitio MSDN.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Información general de creación del proyecto de aplicación web

Los pasos siguientes muestran cómo crear un proyecto web:

1. Haga clic en **nuevo proyecto** en el **iniciar** página o en el **archivo** menú.
2. En el **nuevo proyecto** cuadro de diálogo, haga clic en **Web** en el panel izquierdo y **aplicación Web ASP.NET** en el panel central.

    ![Cuadro de diálogo Nuevo proyecto](creating-web-projects-in-visual-studio/_static/image1.png)

    Puede elegir **en la nube** en el panel izquierdo para crear un [Azure Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), [Azure Mobile Service](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx), o [WebJob de Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs). En este tema no se trata esas plantillas.
3. En el panel derecho, haga clic en el **agregar Application Insights al proyecto** casilla de verificación si desea mantenimiento y supervisión del uso de la aplicación. Para obtener más información, consulte [supervisar el rendimiento en aplicaciones web](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/).
4. Especifique el proyecto **nombre**, **ubicación**y otras opciones y, a continuación, haga clic en **Aceptar**.

    El **nuevo proyecto ASP.NET** aparece el cuadro de diálogo.

    ![Cuadro de diálogo Nuevo proyecto](creating-web-projects-in-visual-studio/_static/image2.png)
5. Haga clic en una plantilla.

    ![Seleccione una plantilla](creating-web-projects-in-visual-studio/_static/image3.png)
6. Si desea agregar compatibilidad para plataformas adicionales no incluidos en la plantilla, haga clic en la casilla correspondiente. (En el ejemplo que se muestra, podría agregar MVC o API Web a un proyecto de formularios Web Forms.)

    ![Agregue los marcos de trabajo](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Si desea agregar un proyecto de prueba unitaria, haga clic en **agregar pruebas unitarias**.

    ![Agregar pruebas unitarias](creating-web-projects-in-visual-studio/_static/image5.png)
8. Si desea que un método de autenticación diferente a lo que proporciona la plantilla de forma predeterminada, haga clic en **Cambiar autenticación**.

    ![Configurar el botón de autenticación](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Configurar el cuadro de diálogo de autenticación](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Crear una aplicación web o máquina virtual en Azure

Visual Studio incluye características que facilitan trabajar con servicios de Azure para hospedar aplicaciones web. Por ejemplo, puede hacerlo siguiente desde el IDE de Visual Studio:

- Crear y administrar aplicaciones web o máquinas virtuales que hacen que la aplicación disponible a través de Internet.
- Ver los registros creados por la aplicación mientras se ejecuta en la nube.
- Trabajar en modo de depuración remota mientras se ejecuta la aplicación en la nube.
- Viiew y administrar otros servicios de bases de datos SQL.

También puede [crear una cuenta de Azure](https://www.windowsazure.com/pricing/free-trial/) que incluye servicios básicos, como web apps de forma gratuita, y si es suscriptor de MSDN puede [activar las ventajas de](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) que le proporcionan créditos mensuales hacia Azure adicional servicios. 

De forma predeterminada el **nuevo proyecto ASP.NET** cuadro de diálogo le permite crear una aplicación web o máquina virtual para un nuevo proyecto web. Si no desea crear una nueva aplicación web o máquina virtual, desactive la **Host en la nube** casilla de verificación.

![Crear recursos remotos](creating-web-projects-in-visual-studio/_static/image8.png)

El título de la casilla de verificación podría ser **Host en la nube** o **crear recursos remotos**, y en cualquier caso, el efecto es el mismo. Si deja activada la casilla, Visual Studio crea una aplicación web en Azure App Service de forma predeterminada. Puede usar el cuadro de lista desplegable para cambiar este valor a un **Máquina Virtual** si lo prefiere. Si aún no ha iniciado Azure, se le pidan las credenciales de Azure. Después de iniciar sesión, un cuadro de diálogo permite configurar los recursos que va a crear para el proyecto de Visual Studio. La siguiente ilustración muestra el cuadro de diálogo para una aplicación web; Si decide crear una máquina virtual, aparecen opciones diferentes.

![Configuración de aplicación de Azure](creating-web-projects-in-visual-studio/_static/image9.png)

Para obtener más información acerca de cómo utilizar este proceso para crear recursos de Azure, consulte [empezar a trabajar con Azure y ASP.NET](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) y [crear una máquina virtual para un sitio web con Visual Studio](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).

El resto de este artículo proporciona más información acerca de las plantillas disponibles y sus opciones. El artículo también presenta el marco de Bootstrap, el diseño y creación de temas usada en las plantillas.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Plantillas de proyecto Web de Visual Studio 2013

Visual Studio usa plantillas para crear un proyecto web. Una plantilla de proyecto puede crear archivos y carpetas en el nuevo proyecto, instalar paquetes de NuGet y proporciona código de ejemplo para una aplicación operativa rudimentarias. Las plantillas implementan los estándares web más recientes y pretenden demostrar los procedimientos recomendados acerca de cómo usar las tecnologías ASP.NET así como dar un salto iniciar acerca de cómo crear su propia aplicación.

Visual Studio 2013 proporciona las siguientes opciones para las plantillas de proyecto web para los proyectos que tienen como destino .NET 4.5 o versiones posteriores de .NET framework:

- [Plantilla vacía](#empty)
- [Plantilla de Web Forms](#wf)
- [Plantilla de MVC](#mvc)
- [Plantilla Web API](#webapi)
- [Plantilla de aplicación de página única](#spa)
- [Plantilla de servicio móvil de Azure](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Plantillas de Visual Studio 2012](#vs2012)

También puede instalar una extensión de Visual Studio que proporciona un [plantilla de Facebook](#facebook).

Para obtener información sobre cómo crear proyectos que tienen como destino .NET 4, consulte [plantillas de Visual Studio 2012](#vs2012) más adelante en este tema.

Para obtener información sobre cómo crear aplicaciones de ASP.NET para los clientes móviles, consulte [Mobile Support en ASP.NET](../../../mobile/index.md).

<a id="empty"></a>
### <a name="empty-template"></a>Plantilla vacía

La plantilla vacía proporciona los archivos para una aplicación web ASP.NET, como un archivo de proyecto y reconstrucción carpetas mínimas (*.csproj* o. *vbproj*) y un *Web.config* archivo. Puede agregar compatibilidad con formularios Web Forms, MVC o API Web mediante el uso de las casillas de verificación bajo la **agregar carpetas y referencias centrales para:** etiqueta.

Para la plantilla vacía opciones de autenticación no están disponibles. Se implementa la funcionalidad de autenticación en aplicaciones de ejemplo y la plantilla vacía no crea una aplicación de ejemplo.

<a id="wf"></a>
### <a name="web-forms-template"></a>Plantilla de Web Forms

Los formularios Web Forms framework proporciona las siguientes características que le permiten rápidamente creación sitios web que son ricas en la interfaz de usuario y datos de acceso a características:

- Un diseñador WYSIWYG en Visual Studio.
- Controles de servidor que se representan en HTML y que se puede personalizar mediante el establecimiento de propiedades y estilos.
- Una gran variedad de controles de acceso a datos y visualización de datos.
- Un modelo de eventos que expone eventos que se pueden programar como usted desea programar una aplicación cliente como WPF.
- Conservación automática del estado (datos) entre las solicitudes HTTP.

En general, la creación de una aplicación de formularios Web Forms requiere menos esfuerzo de programación de la creación de la misma aplicación con el marco de ASP.NET MVC. Sin embargo, formularios Web Forms no es solo para desarrollo rápido de aplicaciones. Hay muchas aplicaciones comerciales complejas y marcos de trabajo basados en formularios Web Forms.

Dado que una página de formularios Web Forms y los controles en la página generan automáticamente gran parte del marcado que se envía al explorador, no tiene el tipo de control más preciso sobre la HTML que ofrece ASP.NET MVC. El modelo declarativo para configurar las páginas y controles minimiza la cantidad de código que tiene que escribir pero oculta parte del comportamiento de HTTP y HTML. Por ejemplo, no siempre es posible especificar exactamente qué marcas generadas por un control.

El marco de trabajo de formularios Web Forms no se presta tan fácilmente como ASP.NET MVC a prácticas de desarrollo basado en patrones como [desarrollo controlado por pruebas](http://en.wikipedia.org/wiki/Test-driven_development), [separación de preocupaciones](http://en.wikipedia.org/wiki/Separation_of_concerns), [inversión de control](http://en.wikipedia.org/wiki/Inversion_of_control), y [inserción de dependencias](http://en.wikipedia.org/wiki/Dependency_injection). Si desea escribir código factorizado de este modo, se puede. pero no es tan automática porque está en el marco de MVC de ASP.NET. El [MVP de ASP.NET Web Forms](http://webformsmvp.com/) project muestra un enfoque que facilita la separación de intereses y capacidad de prueba manteniendo el desarrollo rápido que formularios Web Forms se ha creado para ofrecer. Microsoft SharePoint se basa en Web Forms MVP.

La plantilla de formularios Web Forms crea una aplicación de formularios Web Forms de ejemplo que usa [Bootstrap](#bootstrap) para proporcionar características de diseño y creación de temas con capacidad de respuesta. La siguiente ilustración muestra la página principal.

![Página principal de Web Forms plantilla aplicación](creating-web-projects-in-visual-studio/_static/image10.png)

Para obtener más información acerca de formularios Web Forms, consulte [formularios Web Forms ASP.NET](https://asp.net/web-forms). Para obtener información sobre lo que hace la plantilla de formularios Web Forms para usted, consulte [generar una aplicación básica de formularios Web Forms con Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>Plantilla de MVC

ASP.NET MVC se diseñó para facilitar las prácticas de desarrollo basado en patrones como [desarrollo controlado por pruebas](http://en.wikipedia.org/wiki/Test-driven_development), [separación de preocupaciones](http://en.wikipedia.org/wiki/Separation_of_concerns), [inversión de control](http://en.wikipedia.org/wiki/Inversion_of_control), y [inserción de dependencias](http://en.wikipedia.org/wiki/Dependency_injection). El marco anima a separar la capa de lógica de negocios de una aplicación web de su capa de presentación. Al dividir la aplicación en modelos (M), las vistas (V) y controladores (C), ASP.NET MVC puede facilitan administrar la complejidad de las aplicaciones más grandes.

Con ASP.NET MVC, puede trabajar más directamente con HTML y HTTP que en los formularios Web Forms. Por ejemplo, formularios Web Forms automáticamente puede conservar el estado entre las solicitudes HTTP, pero tiene codificar explícitamente en MVC. La ventaja del modelo MVC es que permite tomar el control completo sobre exactamente lo que hace la aplicación y cómo se comporta en el entorno web. La desventaja es que tendrá que escribir más código.

MVC se diseñó para ser extensible, que proporciona a los desarrolladores de power la capacidad para personalizar el marco de trabajo para sus necesidades de la aplicación. Además, el código fuente de ASP.NET MVC está disponible bajo una licencia de OSI.

La plantilla de MVC crea una aplicación de MVC 5 de ejemplo que usa [Bootstrap](#bootstrap) para proporcionar características de diseño y creación de temas con capacidad de respuesta. La siguiente ilustración muestra la página principal.

![Aplicación de ejemplo MVC](creating-web-projects-in-visual-studio/_static/image11.png)

Para obtener más información sobre MVC, consulte [ASP.NET MVC](https://asp.net/mvc). Para obtener información sobre cómo seleccionar la plantilla de MVC 4, consulte [plantillas de Visual Studio 2012](#vs2012) más adelante en este artículo.

<a id="webapi"></a>
### <a name="web-api-template"></a>Plantilla de API Web

La plantilla API Web crea un servicio web de ejemplo basado en Web API, incluidas las páginas de Ayuda de API basadas en MVC.

ASP.NET Web API es un marco que facilita la creación de servicios HTTP que lleguen a una amplia gama de clientes, incluidos los exploradores y dispositivos móviles. ASP.NET Web API es una plataforma ideal para compilar servicios RESTful en .NET Framework.

La plantilla API Web crea un servicio web de ejemplo. Las ilustraciones siguientes muestran las páginas de Ayuda de ejemplo.

![Página de Ayuda de API Web](creating-web-projects-in-visual-studio/_static/image12.png)

![Página de Ayuda de API Web de API de GET](creating-web-projects-in-visual-studio/_static/image13.png)

Para obtener más información acerca de Web API, consulte [ASP.NET Web API](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Plantilla de aplicación de página única

La plantilla de aplicación de página única (SPA) crea una aplicación de ejemplo que usa JavaScript, HTML 5, y [KnockoutJS](http://knockoutjs.com/) en el cliente y ASP.NET Web API en el servidor.

Es la única opción de autenticación para la plantilla SPA [cuentas de usuario individuales](#indauth).

La siguiente ilustración muestra el estado inicial de la aplicación de ejemplo que crea la plantilla SPA.

![Aplicación de ejemplo SPA](creating-web-projects-in-visual-studio/_static/image14.png)

Para obtener información sobre cómo crear una aplicación mediante la plantilla SPA, consulte [Web API: servicios de autenticación externos](../../../web-api/overview/security/external-authentication-services.md).

Para obtener más información acerca de aplicaciones de página única de ASP.NET y plantillas SPA adicionales que usan marcos de JavaScript que no sea KnockoutJS, consulte los siguientes recursos:

- [Aplicación de página de ASP.NET solo](../../../single-page-application/index.md).
- [Descripción de características de seguridad de la plantilla SPA de RC de VS2013](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Las aplicaciones de página única: Compilar aplicaciones Web modernas con capacidad de respuesta con ASP.NET](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Plantilla de Facebook

Puede instalar un [extensión de Visual Studio que proporciona una plantilla de Facebook](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409). Esta plantilla crea una aplicación de ejemplo está diseñada para ejecutarse en el sitio web de Facebook. Se basa en ASP.NET MVC y API Web se utiliza para la funcionalidad de actualización en tiempo real.

No hay opciones de autenticación están disponibles para la plantilla de Facebook, porque las aplicaciones de Facebook se ejecutan dentro del sitio de Facebook y se basan en la autenticación de Facebook.

Para obtener más información acerca de las aplicaciones de Facebook de ASP.NET, vea [actualización de la API de Facebook MVC](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Plantillas de Visual Studio 2012

El cuadro de diálogo de creación de proyecto de Visual Studio 2013 web no proporciona acceso a algunas plantillas que estaban disponibles en Visual Studio 2012. Si desea usar una de estas plantillas, puede hacer clic en el nodo de Visual Studio 2012 en el panel izquierdo del cuadro de diálogo nuevo proyecto de Visual Studio.

![Plantillas de Visual Studio 2012](creating-web-projects-in-visual-studio/_static/image15.png)

El **Visual Studio 2012** nodo le permite seleccionar las siguientes plantillas de web que no tiene acceso a en la lista predeterminada de plantillas para Visual Studio 2013:

- Aplicación Web ASP.NET MVC 4
- Aplicación Web de entidades de datos dinámicos de ASP.NET
- Control de servidor ASP.NET AJAX
- Extensor de Control de servidor ASP.NET AJAX
- Control de servidor ASP.NET

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Arrancar en las plantillas de proyecto web de Visual Studio 2013

Usan las plantillas de proyecto de Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), un marco de trabajo de diseño y creación de temas creado por Twitter. Bootstrap usa CSS3 para proporcionar un diseño dinámico, lo que significa que los diseños pueden adaptarse dinámicamente a los tamaños de ventana de explorador diferente. Por ejemplo, en una ventana de explorador anchas la página principal que se crean mediante la plantilla de formularios Web Forms es similar de la ilustración siguiente:

![Página principal de Web Forms plantilla aplicación](creating-web-projects-in-visual-studio/_static/image16.png)

Hacer más estrecha la ventana y mover las columnas organizadas horizontalmente en disposición vertical:

![Organización de bootstrap columna vertical](creating-web-projects-in-visual-studio/_static/image17.png)

Reducir un poco más de la ventana y el menú superior horizontal se convierte en un icono que puede hacer clic para expandir en un menú de la orientación vertical:

![Icono de menú de arranque](creating-web-projects-in-visual-studio/_static/image18.png)

![Bootstrap menú vertical](creating-web-projects-in-visual-studio/_static/image19.png)

También puede usar características de creación de temas de Bootstrap para llevar a cabo fácilmente un cambio en apariencia y comportamiento de la aplicación. Por ejemplo, puede hacer lo siguiente para cambiar el tema.

1. En el explorador, vaya a [ http://Bootswatch.com ](http://Bootswatch.com), elija un tema y, a continuación, haga clic en **descargar**. (Este modo se descarga *bootstrap.min.css* de forma predeterminada; si desea examinar el código CSS, obtener *bootstrap.css* en lugar de la versión minificada.)
2. Copie el contenido del archivo descargado de CSS.
3. En Visual Studio, cree un nuevo **hoja de estilos** archivo denominado *bootstrap-theme.css* en el *contenido* carpeta y pegue el CSS descargado de código en él.
4. Abra *aplicación\_Start/Bundle.config* y cambiar *bootstrap.css* a *bootstrap-theme.css*.

Vuelva a ejecutar el proyecto y la aplicación tiene un nuevo aspecto. La siguiente ilustración muestra el efecto del tema Amelia:

![Tema de bootstrap Amelia](creating-web-projects-in-visual-studio/_static/image20.png)

Existen muchos temas Bootstrap, las versiones gratis y premium. Bootstrap también ofrece una amplia variedad de componentes de interfaz de usuario, como [listas desplegables](http://twitter.github.io/bootstrap/components.html#dropdowns), [botón grupos](http://twitter.github.io/bootstrap/components.html#buttonGroups), y [iconos](http://twitter.github.io/bootstrap/base-css.html#images). Para obtener más información sobre Bootstrap, consulte [el sitio de Bootstrap](http://twitter.github.io/bootstrap/).

Si usa el Diseñador de formularios Web Forms en Visual Studio, tenga en cuenta que el diseñador no es compatible con CSS3, por lo que no muestra con precisión todos los efectos de los temas de Bootstrap o los cambios de diseño con capacidad de respuesta. Sin embargo, las páginas de formularios Web Forms se mostrarán correctamente cuando se ven con un explorador.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Agregar compatibilidad para plataformas adicionales

Cuando selecciona una plantilla, se selecciona automáticamente la casilla de verificación para los marcos de trabajo utilizado la plantilla. Por ejemplo, si selecciona el **formularios Web Forms** plantilla, el **formularios Web Forms** casilla está activada y no puede borrar.

![Seleccione una plantilla](creating-web-projects-in-visual-studio/_static/image21.png)

![Agregue los marcos de trabajo](creating-web-projects-in-visual-studio/_static/image22.png)

Puede seleccionar la casilla de verificación para un marco de trabajo que no se incluye en la plantilla con el fin de agregar compatibilidad para ese marco de trabajo cuando se crea el proyecto. Por ejemplo, para habilitar el uso de formularios Web Forms *.aspx* páginas cuando haya seleccionado la plantilla de MVC, seleccione el **formularios Web Forms** casilla de verificación. O para habilitar MVC cuando se usa la plantilla de formularios Web Forms, haga clic en el **MVC** casilla de verificación. Agregar un marco habilita la compatibilidad de tiempo de diseño, así como en tiempo de ejecución. Por ejemplo, si agrega compatibilidad con MVC a un proyecto de formularios Web Forms, podrá aplicar la técnica scaffolding controladores y vistas.

Si combina los formularios Web Forms y MVC en un proyecto y habilite [Friendly URLs](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) en formularios Web Forms, podría haber inesperado enrutamiento problemas donde una dirección URL tiene varios destinos posibles. Las rutas que se definen en primer lugar tendrá prioridad. Por ejemplo, si tiene un `Home` controlador y un *Home.aspx* página, el `http://contoso.com/home` URL irá a *Home.aspx* si llama a la `EnableFriendlyUrls` método antes de llamar a la `MapRoute`método *RouteConfig.cs*, o la misma dirección URL irá a la vista predeterminada para su `Home` controlador si se llama a `MapRoute` antes `EnableFriendlyUrls`.

Agregar un marco de trabajo no agrega ninguna funcionalidad de la aplicación de ejemplo. Por ejemplo, si agrega formularios Web Forms admiten cuando haya seleccionado la plantilla de MVC, no *Default.aspx* se crea el archivo de página principal. Se agregan las carpetas, archivos y referencias necesarias para admitir el marco de trabajo. Por ese motivo, la adición de marcos de trabajo no cambia las opciones de autenticación, que se implementan mediante código en aplicaciones de ejemplo creados mediante las plantillas. Por ejemplo, si selecciona la plantilla vacía y agregar formularios Web Forms o MVC admite, el **configurar autenticación** todavía se deshabilitará el botón.

Las secciones siguientes describen brevemente el efecto de cada casilla de verificación.

### <a name="add-web-forms-support"></a>Agregar compatibilidad de Web Forms

Crea vacío *aplicación\_datos* y *modelos* carpetas y un *Global.asax* archivo. Ya se crean todas las plantillas que no sea la plantilla vacía, por lo que activa la casilla de verificación de formularios Web Forms no supone ninguna diferencia de otras plantillas.

La plantilla de formularios Web Forms habilita URL preparadas de forma predeterminada, pero cuando se agrega compatibilidad con formularios Web Forms a otras plantillas seleccionando la casilla de verificación de formularios Web Forms que no se habilitan automáticamente las direcciones URL preparadas.

### <a name="add-mvc-support"></a>Agregar compatibilidad con MVC

Instala paquetes WebPages NuGet, Razor y MVC, se crea vacío *aplicación\_datos*, *controladores*, *modelos*, y *vistas*carpetas, crea *aplicación\_iniciar* carpeta con *RouteConfig.cs* de archivos y crea *Global.asax* archivo.

### <a name="add-web-api-support"></a>Agregar compatibilidad con la API Web

Instala los paquetes Newtonsoft.Json NuGet y WebApi, crea vacío *aplicación\_datos*, *controladores*, y *modelos* carpetas, crea  *Aplicación\_iniciar* carpeta con *WebApiConfig.cs* de archivos y crea *Global.asax* archivo.

<a id="auth"></a>
## <a name="authentication-methods"></a>Métodos de autenticación

Visual Studio 2013 ofrece varias opciones de autenticación para las plantillas de formularios Web Forms, MVC y Web API:

- [Sin autenticación](#noauth)
- [Cuentas de usuario individuales](#indauth) (identidad de ASP.NET, conocido anteriormente como la pertenencia a ASP.NET)
- [Las cuentas organizativas](#orgauth) (Windows Server Active Directory o Azure Active Directory)
- [Autenticación de Windows](#winauth) (Intranet)

![Configurar el cuadro de diálogo de autenticación](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Sin autenticación

Si selecciona **sin autenticación**, la aplicación de ejemplo no contendrá ninguna página web para iniciar sesión, no que indica quién ha iniciado sesión la interfaz de usuario, no de clase de entidad para una base de datos de pertenencia y ninguna cadena de conexión para una base de datos de pertenencia.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Cuentas de usuario individuales

Si selecciona **cuentas de usuario individuales**, la aplicación de ejemplo que se configurarán para usar ASP.NET Identity (conocido anteriormente como la pertenencia a ASP.NET) para la autenticación de usuario. ASP.NET Identity permite que un usuario registrar una cuenta, mediante la creación de un nombre de usuario y una contraseña en el sitio o iniciando sesión con proveedores de redes sociales como Facebook, Google, Microsoft Account o Twitter. El almacén de datos predeterminada para los perfiles de usuario en ASP.NET Identity es una base de datos de SQL Server LocalDB, que puede implementar en Azure SQL Database o SQL Server para el sitio de producción.

En Visual Studio 2013, estas características son las mismas que en Visual Studio 2012, pero se ha vuelto a escribir el código subyacente para el sistema de pertenencia ASP.NET. Ventajas de la nueva base de código incluyen lo siguiente:

- El nuevo sistema de pertenencia se basa en [OWIN](http://owin.org/) en lugar del módulo de autenticación de formularios de ASP.NET. Esto significa que puede usar el mismo mecanismo de autenticación si está utilizando formularios Web Forms o MVC en IIS o propio que aloje SignalR o API Web.
- La nueva base de datos de pertenencia se administra mediante Entity Framework Code First y todas las tablas están representadas por clases de entidad que se pueden modificar. Esto significa que se pueden personalizar fácilmente el esquema de base de datos y web relacionados con el perfil de la interfaz de usuario según sus propias necesidades, y puede implementar fácilmente sus actualizaciones con migraciones de Code First.

El nuevo sistema de pertenencia se implementa automáticamente en las nuevas plantillas y se puede implementar manualmente en cualquier proyecto que tenga como destino .NET 4.5 o posterior.

ASP.NET Identity es una buena opción si va a crear un sitio web de Internet que es principalmente para clientes externos. Si su organización utiliza Active Directory o Office 365 y desea crear un proyecto que permite solo inicio de sesión para los empleados y socios comerciales, el **cuentas organizativas** opción podría ser una opción mejor.

Para obtener más información acerca de la opción cuentas de usuario individuales, consulte los siguientes recursos:

- [www.ASP.NET/Identity](../../../identity/index.md). Documentación sobre ASP.NET Identity en el sitio web ASP.NET.
- [Crear una aplicación ASP.NET MVC 5 con Facebook y Google OAuth2 y OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). También se muestra cómo personalizar los datos de perfil de usuario.
- [Web API, servicios de autenticación externos](../../../web-api/overview/security/external-authentication-services.md)
- [Agregar inicios de sesión externos a la aplicación ASP.NET en Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Cuentas organizativas

Si selecciona **cuentas organizativas**, la aplicación de ejemplo se configurarán para utilizar Windows Identity Foundation (WIF) para la autenticación basada en cuentas de usuario de Azure Active Directory (Azure AD, que incluye Office 365) o Windows Server Active Directory. Para obtener más información, consulte [las opciones de autenticación de cuenta de organización](#orgauthoptions) más adelante en este tema.

<a id="winauth"></a>
### <a name="windows-authentication"></a>Autenticación de Windows

Si selecciona **Windows autenticación**, se configurará la aplicación de ejemplo para usar el módulo de IIS de la autenticación de Windows para la autenticación. La aplicación mostrará el identificador de usuario y dominio de Active Directory o cuenta del equipo local que ha iniciado sesión en Windows, pero no incluyen el registro de usuario o inicie sesión en la interfaz de usuario. Esta opción está diseñada para sitios web de Intranet.

Como alternativa, puede crear un sitio de Intranet que usa la autenticación de AD eligiendo el [locales opción bajo cuentas organizativas](#orgauthonprem). La opción de On-Premises usa Windows Identity Foundation (WIF) en lugar del módulo de autenticación de Windows. Se necesitan algunos pasos adicionales para configurar la opción On-Premises, pero WIF habilita características que no están disponibles en el módulo de autenticación de Windows. Por ejemplo, con WIF puede configurar el acceso a la aplicación en Active Directory y consultar datos de directorio.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Opciones de autenticación de cuenta de organización

El **configurar autenticación** cuadro de diálogo ofrece varias opciones para la autenticación de cuentas de Windows Server Active Directory (AD) o Azure Active Directory (Azure AD, que incluye Office 365):

- [Nube: organización única](#orgauthsingle) (Azure AD o AD mediante la integración de directorios con Azure AD)
- [En la nube: organización Multi](#orgauthmulti) (Azure AD o AD mediante la integración de directorios con Azure AD)
- [En el entorno local](#orgauthonprem) (AD)

Si desea probar una de las opciones de Azure AD, pero aún no tiene una cuenta, [haga clic aquí para registrarse para obtener una cuenta de Azure AD](https://go.microsoft.com/fwlink/?LinkId=309942).

> [!NOTE]
> Si elige una de las opciones de Azure AD, el proyecto requiere una base de datos y tendrá que iniciar sesión en una cuenta de administrador global para el inquilino de Azure AD. Escriba el nombre y la contraseña de una cuenta profesional (por ejemplo, admin@contoso.onmicrosoft.com) que tenga permisos administrativos para el inquilino de Azure AD.
> 
> **No escriba las credenciales de una cuenta de Microsoft (por ejemplo, contoso@hotmail.com) en el cuadro de diálogo de inicio de sesión.**


<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>En la nube - autenticación de la organización única

![Autenticación de la organización única](creating-web-projects-in-visual-studio/_static/image24.png)

Elija esta opción si desea habilitar la autenticación de cuentas de usuario que se definen en un Azure AD [inquilino](https://technet.microsoft.com/library/jj573650.aspx). Por ejemplo, el sitio es contoso.com y estará disponible a los empleados de la empresa Contoso que se encuentran en el inquilino de contoso.onmicrosoft.com. No podrá configurar Azure AD para permitir que a los usuarios de otros inquilinos para tener acceso a la aplicación.

#### <a name="domain"></a>Dominio

Escriba el dominio de Azure AD que desea configurar la aplicación, por ejemplo: `contoso.onmicrosoft.com`. Si tiene un [dominio personalizado](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/), tales como `contoso.com` en lugar de `contoso.onmicrosoft.com`, puede indicarlo aquí.

#### <a name="access-level"></a>Nivel de acceso

Si la aplicación necesita para consultar o actualizar información de directorio mediante la API Graph, elija **inicio de sesión único, leer datos de directorio** o **inicio de sesión único, leer y escribir datos de directorio**. En caso contrario, elija **Single Sign-On**. Para obtener más información, consulte [niveles de acceso de la aplicación](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) y [mediante Graph API para consultar Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx).

#### <a name="application-id-uri"></a>URI de Id. de aplicación

De forma predeterminada, la plantilla crea una URI de Id. de aplicación por usted anexando el nombre del proyecto al dominio de Azure AD. Por ejemplo, si es el nombre del proyecto `Example` y el dominio es `contoso.onmicrosoft.com`, la aplicación se convierte en el URI de Id. de `https://contoso.onmicrosoft.com/Example`. Si desea especificar manualmente el identificador URI de la aplicación, expanda la **más opciones** sección y escriba el URI de Id. de aplicación en el cuadro de texto. La aplicación de identificador URI debe comenzar por `https://`.

De forma predeterminada, si una aplicación que ya se ha aprovisionado en Azure AD tiene la misma URI de Id. de aplicación como uno que está usando Visual Studio para el proyecto, el proyecto se conectará a la aplicación existente en lugar de aprovisionar una nueva. Si desea que una nueva aplicación que se aprovisione en ese caso, desactive la **sobrescribir la entrada de la aplicación si ya existe uno con el mismo identificador** casilla de verificación.

Si el **sobrescritura** casilla de verificación está desactivada y Visual Studio busca una aplicación existente con la misma URI de Id. de aplicación, crea un nuevo URI anexando un número para el URI se va a usar. Por ejemplo, suponga que es el nombre del proyecto `Example`, se deja en blanco el cuadro de texto, borrar el **sobrescritura** casilla de verificación y el inquilino de Azure AD ya tiene una aplicación con el identificador URI `https://contoso.onmicrosoft.com/Example`. En ese caso, se aprovisionará una nueva aplicación con una aplicación, como el identificador URI `https://contoso.onmicrosoft.com/Example_20130619330903`.

#### <a name="provisioning-the-application-in-azure-ad"></a>Aprovisionamiento de la aplicación en Azure AD

Con el fin de aprovisionar la aplicación en Azure AD o conectar el proyecto a una aplicación existente, Visual Studio necesita las credenciales de administrador global para el dominio. Al hacer clic en **Aceptar** en el **configurar autenticación** cuadro de diálogo, se le solicitará el nombre de usuario y la contraseña de administrador global para el dominio especificado. Más adelante, al hacer clic en **crear proyecto** en el **nuevo proyecto ASP.NET** cuadro de diálogo, Visual Studio aprovisiona la aplicación en Azure AD. Tenga en cuenta que como parte de este proceso de Visual Studio incrusta los valores de secreto de cliente en el archivo Web.config que expira un año después de la creación.

Para obtener información sobre cómo crear aplicaciones que usan **nube: organización única** autenticación, consulte los siguientes recursos:

- [Autenticación de Azure](../2012/windows-azure-authentication.md)
- [Agregar inicio de sesión en su aplicación Web con Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Desarrollo de aplicaciones ASP.NET con Azure Active Directory](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Proteger ASP.NET Web API con Azure AD y los componentes de Microsoft OWIN](https://msdn.microsoft.com/magazine/dn463788.aspx)

Los tutoriales aún no se han actualizado para Visual Studio 2013; algunos de los tutoriales de qué ayudará a hacer manualmente es automática en Visual Studio 2013.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>En la nube - autenticación múltiple de la organización

![Autenticación de organización múltiple](creating-web-projects-in-visual-studio/_static/image25.png)

Elija esta opción si desea habilitar la autenticación de cuentas de usuario que se definen en Azure AD varios [inquilinos](https://technet.microsoft.com/library/jj573650.aspx). Por ejemplo, el sitio es contoso.com y estará disponible para los empleados de la empresa Contoso que se encuentran en el inquilino contoso.onmicrosoft.com y empleados de la empresa Fabrikam que están en el inquilino fabrikam.onmicrosoft.com.

La configuración que especifique y el paso de aprovisionamiento de aplicación son similares a [autenticación única organización](#orgauthsingle).

Para obtener información sobre cómo crear aplicaciones que usan **en la nube: organización Multi** autenticación, consulte los siguientes recursos:

- [Fácil integración de aplicaciones Web con Azure Active Directory, ASP.NET &amp; Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) en el blog del equipo de Active Directory.
- [Desarrollo de aplicaciones Web de varios inquilinos con Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx) tutorial. El tutorial aún no se ha actualizado para Visual Studio 2013; algunos de lo que el tutorial le lleva a hacer manualmente es automática en Visual Studio 2013.
- [Es necesario iniciar sesión con su propia aplicación de ASP.NET varias organizaciones antes de poder firmar](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/). Blog de Vittorio Bertocci que explica cómo resolver un personas problema común encontrar al crear un proyecto que usa la autenticación de organización múltiples.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>Autenticación de organización en el entorno local

![Autenticación de organización en el entorno local](creating-web-projects-in-visual-studio/_static/image26.png)

Elija esta opción si desea habilitar la autenticación de cuentas de usuario que se definen en Windows Server Active Directory (AD), y no desea usar Azure AD. Puede usar esta opción para crear un sitio de Intranet o un sitio de Internet. Para un sitio de Internet, utilice servicios de federación de Active Directory (ADFS) para proporcionar acceso a AD. Para obtener más información, consulte [usar la opción de autenticación organizativa de On-Premises (ADFS) con ASP.NET en Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

Para un sitio de Intranet, como alternativa puede elegir [Windows autenticación](#winauth) en lugar de esta opción. Para la opción de autenticación de Windows no tiene que proporcionar una dirección URL de metadatos. Sin embargo, la autenticación de Windows no le otorga la capacidad para controlar el acceso de aplicación en Active Directory o para consultar los datos de directorio.

#### <a name="on-premises-authority"></a>Autoridad local

Escriba una dirección URL que apunta al documento de metadatos. El documento de metadatos contiene las coordenadas de la entidad. La aplicación usará esas coordenadas para dirigir el flujo de inicio de sesión web.

#### <a name="application-id-uri"></a>URI de Id. de aplicación

Proporcione un URI único que AD puede usar para identificar esta aplicación, o déjela en blanco para permitir que Visual Studio cree uno.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Pasos siguientes

Este documento proporciona Ayuda básica para crear un nuevo proyecto web ASP.NET en Visual Studio 2013. Para obtener más información sobre el uso de Visual Studio para el desarrollo web, consulte [ https://www.asp.net/visual-studio/ ](../../index.md).
