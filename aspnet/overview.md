---
uid: overview
title: Información general sobre ASP.NET | Microsoft Docs
author: rick-anderson
description: Introducción a ASP.NET, un marco gratuito para crear sitios Web, aplicaciones web y API web.
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.author: aspnetcontent
ms.date: 03/12/2010
ms.technology: aspnet
msc.legacyurl: ''
msc.type: content
ms.openlocfilehash: d477db5f38fff6549f0a7f62963a48ee89b74101
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809490"
---
# <a name="aspnet-overview"></a>Información general sobre ASP.NET

ASP.NET es un marco web gratuito para crear excelentes sitios Web y aplicaciones web mediante HTML, CSS y JavaScript. También puede crear las API Web y utilice tecnologías en tiempo real como Sockets Web.

[ASP.NET Core](https://docs.microsoft.com/aspnet/core/) es una alternativa a ASP.NET.  Consulte la [instrucciones sobre cómo elegir entre ASP.NET y ASP.NET Core](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework).

## <a name="get-started"></a>Primeros pasos

[Visual Studio Community 2017](https://www.visualstudio.com/downloads/), un IDE gratuito para ASP.NET en Windows.

## <a name="websites-and-web-applications"></a>Sitios y aplicaciones web

 Ofrece tres marcos de trabajo para crear aplicaciones web ASP.NET: formularios Web Forms, ASP.NET MVC y ASP.NET Web Pages. Los tres marcos son estables y maduro, y puede crear aplicaciones web excelente con cualquiera de ellos. Con independencia de qué framework que elija, obtendrá todas las ventajas y características de ASP.NET en todas partes.

Cada marco de trabajo tiene como destino un estilo de desarrollo diferentes. Aquel que elija depende de una combinación de los recursos de programación (conocimientos, habilidades y experiencia de desarrollo), el tipo de aplicación que se va a crear y se siente cómodo con el enfoque de desarrollo.

A continuación es una visión general de cada uno de los marcos de trabajo y algunas ideas sobre cómo elegir entre ellas. ¿Si prefiere un vídeo de introducción, consulte [hacer que los sitios Web con ASP.NET](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET) y [What ' s herramientas Web?](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | Si tiene experiencia en | Estilo de desarrollo | Experiencia | 
|-----------|----------------------|-----------------------------------------------------|----------------|
| formularios Web Forms | Win Forms, WPF, .NET | Agiliza el desarrollo con una completa biblioteca de controles que encapsulan código HTML | RAD de nivel intermedio, avanzado |
| MVC       | Ruby on Rails, .NET  | Control total sobre marcado HTML, código y marcado separado y fácil de escribir pruebas. La mejor elección para aplicaciones móviles y de página única (SPA). | Nivel intermedio, avanzado |
| Páginas web  | Classic ASP, PHP     | Marcado HTML y el código conjuntamente en el mismo archivo | Nuevo, el nivel intermedio |

### <a name="web-forms"></a>formularios Web Forms

Con ASP.NET Web Forms, puede crear sitios Web dinámicos con un modelo conocido de arrastrar y colocar, controlado por eventos. Una superficie de diseño y cientos de controles y componentes le permiten crear rápidamente sofisticados y eficaces sitios controlados por la interfaz de usuario con acceso a datos. 

[Más información sobre los formularios Web Forms](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC ofrece una manera eficaz, basado en patrones para crear sitios Web dinámicos que permite una separación clara de intereses y aporta control total sobre el marcado para el desarrollo ameno y rápido. ASP.NET MVC incluye muchas características que permiten el desarrollo rápido y sencillo de TDD para crear aplicaciones sofisticadas que usan los estándares web más recientes. 

[Más información sobre MVC](mvc/index.md)

### <a name="aspnet-web-pages"></a>ASP.NET Web Pages

ASP.NET Web Pages y la sintaxis Razor proporcionan una manera rápida, cercana y ligera de combinar código de servidor con HTML para crear contenido web dinámico. Conectarse a bases de datos, agregar vídeo, vincular a sitios de redes sociales e incluyen muchas más características que le ayudarán a crean sitios atractivos que cumplen con los estándares web más recientes.

[Más información acerca de las páginas Web](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Notas acerca de los formularios Web Forms, MVC y Web Pages

Los tres marcos ASP.NET se basan en .NET Framework y compartan la funcionalidad de .NET y de ASP.NET core. Por ejemplo, los tres marcos ofrecen un modelo de seguridad de inicio de sesión basado en torno a la pertenencia y las tres comparten las mismas facilidades para administrar las solicitudes y sesiones de control que forman parte de la funcionalidad de ASP.NET.

Además, los tres marcos de trabajo no son completamente independientes y elegir una no excluye con otro. Puesto que los marcos pueden coexistir en la misma aplicación web, no es raro ver componentes individuales de las aplicaciones escritas con marcos diferentes. Por ejemplo, podrían desarrollar orientados al cliente de partes de una aplicación MVC para optimizar el marcado, mientras que las partes administrativas y el acceso a datos se desarrollan en formularios Web Forms para aprovechar las ventajas de los controles de datos y acceso a datos sencillo.

## <a name="web-apis"></a>API web

ASP.NET Web API es un marco que facilita la creación de servicios HTTP que lleguen a una amplia gama de clientes, incluidos los exploradores y dispositivos móviles. ASP.NET Web API es una plataforma ideal para compilar aplicaciones de RESTful en .NET Framework.

[Más información sobre la API Web](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>Tecnologías en tiempo real

ASP.NET SignalR es una nueva biblioteca para desarrolladores de ASP.NET que facilita la funcionalidad de desarrollo web en tiempo real. SignalR permite la comunicación bidireccional entre servidor y cliente. Los servidores pueden insertar contenidos en los clientes conectados al instante cuando se encuentre disponible. SignalR admite Web Sockets y vuelve a otras técnicas compatibles para los exploradores más antiguos. SignalR incluye las API de administración de conexiones (por ejemplo, conectar y desconectar eventos), agrupación de conexiones y autorización.

[Obtener más información acerca de SignalR](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>Sitios y aplicaciones móviles 

ASP.NET puede potenciar las aplicaciones móviles nativas con un back-end de API Web, así como los sitios web móviles con marcos de trabajo de diseño dinámico como Twitter Bootstrap. Si va a compilar una aplicación móvil nativa, es fácil crear una API de Web basada en JSON para el identificador de acceso a los datos, autenticación y notificaciones de inserción para la aplicación. Si está creando un sitio móvil con capacidad de respuesta, puede usar cualquier marco CSS o el sistema de cuadrícula abierto que prefiera, o seleccione un sistema eficaz de móvil como jQuery Mobile o Sencha y excelentes aplicaciones móviles con PhoneGap.

[Más información sobre el desarrollo de aplicaciones y sitios móvil](mobile/index.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>Aplicaciones de página única 

Aplicación de una página ASP.NET única (SPA) le ayuda a crear aplicaciones que incluyen significativo interacciones del lado cliente con HTML 5, 3 de CSS y JavaScript. Visual Studio incluye una plantilla para crear aplicaciones de página única mediante knockout.js y ASP.NET Web API. Además de la plantilla SPA integrada, creados por la Comunidad de plantillas SPA también están disponibles para su descarga.

[Más información sobre el desarrollo de aplicaciones de página única](single-page-application/index.md)

## <a name="webhooks"></a>WebHooks

WebHooks es un patrón HTTP ligero que proporciona un modelo de pub/sub simple para el cableado juntos los servicios de SaaS y API Web. Cuando se produce un evento en un servicio, se envía una notificación en forma de una solicitud HTTP POST a los suscriptores registrados. La solicitud POST contiene información sobre el evento que hace posible para el receptor para que actúe en consecuencia.

Los WebHooks se exponen mediante un gran número de servicios, como Dropbox, GitHub, Instagram, MailChimp, PayPal, Slack, Trello y muchos más. Por ejemplo, un WebHook puede indicar que ha cambiado un archivo en Dropbox, o se ha confirmado un cambio de código en GitHub, se ha iniciado un pago de PayPal o se ha creado una tarjeta en Trello.

[Más información sobre los WebHooks](webhooks/index.md)





<!--
Create Deployment TOC based on https://www.asp.net/aspnet/overview/deployment
Copy deployment content map to MVC, WebForms, Web Pages, Web API sections.
Copy Web Deployment in Enterprise from WebForms to MVC
Move under ASP.NET Best practices
    What not to do in ASP.NET, and what to do instead https://review.docs.microsoft.cus/aspnet/aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
    Async and await https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/async-and-await
    Building Real World Cloud Apps with Azure https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
    Hands on Lab: Maintainable Azure Websites: Managing Change and Scale https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale

-->
