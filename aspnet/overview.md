---
uid: overview
title: "Información general sobre ASP.NET | Documentos de Microsoft"
author: rick-anderson
description: "Introducción a ASP.NET, un marco de trabajo gratuito para crear sitios Web, aplicaciones web y las API web."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2010
ms.topic: article
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: 
msc.type: content
ms.openlocfilehash: 0ba7814d4004b17e678eab9a2a41a6d6f34773e1
ms.sourcegitcommit: 53ee14b9c8200f44705d8997c3619fa874192d45
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2018
---
# <a name="aspnet-overview"></a>Introducción a ASP.NET

ASP.NET es un marco web gratuito para crear excelentes sitios Web y aplicaciones web con HTML, CSS y JavaScript. También puede crear las API Web y usar tecnologías en tiempo real como Sockets Web.

[ASP.NET Core](https://docs.microsoft.com/aspnet/core/) es una alternativa a ASP.NET.  Consulte la [obtener instrucciones sobre cómo elegir entre ASP.NET y ASP.NET Core](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework).

## <a name="get-started"></a>Primeros pasos

[Comunidad de Visual Studio 2017](https://www.visualstudio.com/downloads/), un IDE es gratis para ASP.NET en Windows.

## <a name="websites-and-web-applications"></a>Aplicaciones web y sitios Web

 ASP.NET ofrece tres marcos de trabajo para crear aplicaciones web: páginas Web de ASP.NET, formularios Web Forms y ASP.NET MVC. Todos los marcos de tres son estables y consolidada, y puede crear aplicaciones web excelente con cualquiera de ellos. Con independencia de qué framework que elija, obtendrá todas las ventajas y características de ASP.NET en cualquier lugar.

Cada marco de trabajo tiene como destino un estilo de desarrollo diferentes. Aquel que elija depende de una combinación de los activos de programación (conocimientos, conocimientos y experiencia de desarrollo), el tipo de aplicación que se va a crear así como el enfoque de desarrollo con que se sienta cómodo.

A continuación se muestra una visión general de cada uno de los marcos y algunas ideas sobre cómo elegir entre ellos. Si prefiere un vídeo de introducción, consulte [hacer que sitios Web con ASP.NET](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET) y [¿qué es herramientas Web?](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | Si tiene experiencia | Estilo de desarrollo | Experiencia | 
|-----------|----------------------|-----------------------------------------------------|----------------|
| formularios Web Forms | Windows Forms, WPF, .NET | Desarrollo rápido mediante una biblioteca de controles que encapsulan código HTML enriquecida | RAD de nivel intermedio, avanzado |
| MVC       | Ruby sobre raíles, .NET  | Control total sobre marcado HTML, código y marcado separado y fácil de escribir pruebas. La mejor opción para las aplicaciones móviles y de página (SPA). | Nivel intermedio, avanzado |
| Páginas web  | Clásico ASP, PHP     | Marcado HTML y el código conjuntamente en el mismo archivo | Nuevo, de nivel intermedio |

### <a name="web-forms"></a>formularios Web Forms

Con los formularios Web Forms de ASP.NET, puede crear sitios Web dinámicos con un modelo familiar de arrastrar y colocar, controlada por eventos. Una superficie de diseño y cientos de controles y componentes le permiten crear rápidamente sofisticados y eficaces sitios controlados por la interfaz de usuario con acceso a datos. 

[Obtener más información acerca de los formularios Web Forms](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC proporciona una manera eficaz, basada en modelos para crear sitios Web dinámicos que permite una separación clara de intereses y aporta control total sobre el marcado para el desarrollo ameno y rápido. ASP.NET MVC incluye muchas características que permiten el desarrollo rápido de TDD rápido para crear aplicaciones sofisticadas que usan los estándares web más recientes. 

[Obtener más información sobre MVC](mvc/index.md)

### <a name="aspnet-web-pages"></a>ASP.NET Web Pages

ASP.NET Web Pages y la sintaxis Razor proporcionan una manera rápida, cercana y ligera de combinar código de servidor con HTML para crear contenido web dinámico. Conectarse a bases de datos, agregar vídeo, vincular a sitios de redes sociales e incluir muchas más características que le ayudarán a crean sitios atractivos que cumplen con los estándares web más recientes.

[Más información acerca de las páginas Web](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Notas acerca de formularios Web Forms, MVC y páginas Web

Todos los marcos de trabajo ASP.NET tres se basan en .NET Framework y comparten una funcionalidad básica de .NET y de ASP.NET. Por ejemplo, todos los marcos de tres ofrecen un modelo de seguridad de inicio de sesión basado en la pertenencia y los tres comparten las mismas facilidades para administrar las solicitudes, las sesiones de control etc. que forman parte de la funcionalidad ASP.NET principal.

Además, los tres marcos no son totalmente independientes y elegir una no excluye con otro. Puesto que los marcos pueden coexistir en la misma aplicación web, no es raro ver componentes individuales de las aplicaciones escritas mediante diferentes marcos de trabajo. Por ejemplo, algunas partes de orientado al cliente de una aplicación podrían desarrollarse en MVC para optimizar el marcado, mientras que las partes administrativas y el acceso a datos se desarrollan en formularios Web Forms para aprovechar las ventajas de los controles de datos y el acceso de datos simple.

## <a name="web-apis"></a>API web

ASP.NET Web API es un marco que facilita la creación de servicios HTTP que llegan a una amplia gama de clientes, incluidos los exploradores y dispositivos móviles. ASP.NET Web API es una plataforma ideal para compilar aplicaciones de RESTful en .NET Framework.

[Más información sobre la API Web](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>Tecnologías en tiempo real

ASP.NET SignalR es una nueva biblioteca para desarrolladores ASP.NET que facilita la funcionalidad de desarrollo web en tiempo real. SignalR permite la comunicación bidireccional entre servidor y cliente. Servidores pueden insertar contenidos en los clientes conectados de forma instantánea cuando se encuentre disponible. SignalR admite Sockets Web y vuelve a otras técnicas compatibles para los exploradores más antiguos. SignalR incluye las API de administración de la conexión (por ejemplo, conectar y desconectar eventos), agrupación de conexiones y autorización.

[Obtener más información sobre SignalR](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>Sitios y aplicaciones móviles 

ASP.NET puede activar aplicaciones móviles nativas con un back-end de API Web, así como los sitios web móviles con marcos de trabajo de diseño dinámico como Twitter Bootstrap. Si está compilando una aplicación nativa móvil, es fácil crear una API de Web JSON en función del identificador y acceso a datos, autenticación, las notificaciones de inserción para la aplicación. Si está creando un sitio de dispositivos móvil con capacidad de respuesta, puede usar cualquier marco CSS o el sistema de cuadrícula abierta que prefiera, o seleccione un eficaz sistema móvil como jQuery Mobile o Sencha y excelentes aplicaciones móviles con PhoneGap.

[Más información sobre el desarrollo móvil de aplicación y sitio](mobile/index.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>Aplicaciones de la página 

Aplicación de una página ASP.NET única (SPA) le ayuda a crear aplicaciones que incluyen importantes interacciones del lado cliente mediante HTML 5, 3 de CSS y JavaScript. Visual Studio incluye una plantilla para la creación de aplicaciones de una página con knockout.js y ASP.NET Web API. Además de la plantilla SPA integrada, plantillas SPA creados por la Comunidad también están disponibles para su descarga.

[Obtener más información sobre el desarrollo de aplicaciones de la página](single-page-application/index.md)

## <a name="webhooks"></a>WebHooks

Webhook es un patrón HTTP ligero proporcionando un modelo de pub/sub simple para el cableado juntos los servicios API Web y SaaS. Cuando se produce un evento en un servicio, se envía una notificación en forma de una solicitud HTTP POST a los suscriptores registrados. La solicitud POST contiene información sobre el evento que hace posible para el receptor para que actúe en consecuencia.

WebHooks expuestos por un gran número de servicios que incluyen Dropbox, GitHub, Instagram, MailChimp, PayPal, demora, Trello y mucho más. Por ejemplo, un WebHook puede indicar que un archivo ha cambiado en Dropbox, o un cambio en el código se ha confirmado en GitHub, o se ha iniciado un pago de PayPal o se ha creado una tarjeta en Trello.

[Obtener más información acerca de Webhook](webhooks/index.md)





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
