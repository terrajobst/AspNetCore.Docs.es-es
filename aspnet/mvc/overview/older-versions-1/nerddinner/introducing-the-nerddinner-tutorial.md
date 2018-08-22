---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Introducción al Tutorial de NerdDinner | Microsoft Docs
author: shanselman
description: Es la mejor forma de aprender un nuevo marco crear algo con él. Este tutorial le guía a través de cómo crear una aplicación pequeña, pero completa, mediante la configuración de ASP.NE...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: d5efab525841b5c526aa3b656f27b1c42cc74648
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829678"
---
<a name="introducing-the-nerddinner-tutorial"></a>Introducción al Tutorial de NerdDinner
====================
por [Scott Hanselman](https://github.com/shanselman)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Es la mejor forma de aprender un nuevo marco crear algo con él. Este tutorial explica cómo crear un pequeño, pero completa, la aplicación mediante ASP.NET MVC 1 y presenta algunos de los principales conceptos detrás de él.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [Introducción a trabajar con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-tutorial"></a>Tutorial de NerdDinner

Es la mejor forma de aprender un nuevo marco crear algo con él. Este tutorial explica cómo crear un pequeño, pero completa, la aplicación mediante ASP.NET MVC y presenta algunos de los principales conceptos detrás de él.

La aplicación, vamos a crear se denomina "NerdDinner". NerdDinner proporciona una manera fácil para los usuarios buscar y organizar las cenas en línea:

![](introducing-the-nerddinner-tutorial/_static/image1.png)

NerdDinner permite a los usuarios registrados podrán crear, editar y eliminar las cenas. Aplica un conjunto coherente de validación y reglas de negocios a través de la aplicación:

![](introducing-the-nerddinner-tutorial/_static/image2.png)

Los visitantes puede utilizar una asignación basada en AJAX para buscar próximos dinners mantenidos cerca de ellos:

![](introducing-the-nerddinner-tutorial/_static/image3.png)

Al hacer clic en una cena llevará a una página de detalles donde puede aprender más sobre ella:

![](introducing-the-nerddinner-tutorial/_static/image4.png)

Si están interesados en asistir a la cena pueden iniciar sesión o registrarse en el sitio:

![](introducing-the-nerddinner-tutorial/_static/image5.png)

Puede, a continuación, haga clic en un vínculo RSVP basadas en AJAX para participar en el evento:

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>Implementación de NerdDinner

Vamos a empezar a nuestra aplicación NerdDinner utilizando el archivo -&gt;comando nuevo proyecto en Visual Studio para crear un nuevo proyecto de ASP.NET MVC. A continuación, incrementalmente agregaremos las características y funcionalidad. En el camino que trataremos:

1. [Cómo crear un nuevo proyecto de MVC de ASP.NET](# "crear un nuevo proyecto de MVC de ASP.NET")
2. [Cómo crear una base de datos](# "crear una base de datos")
3. [Cómo crear un modelo con validaciones de regla de negocio](# "crear un modelo con validaciones de regla de negocio")
4. [Uso de controladores y vistas para implementar una interfaz de usuario de lista/detalles](# "usar controladores y vistas para implementar una interfaz de usuario de la lista/detalles")
5. [Cómo proporcionar CRUD (crear, leer, actualizar y eliminar) compatibilidad con entradas de formularios de datos](# "proporcionar CRUD (creación, lectura, actualización, eliminación) de datos formulario de entrada compatible")
6. [Cómo usar ViewData e implementar las clases ViewModel](# "usar ViewData e implementar clases ViewModel")
7. [Cómo volver a usar la interfaz de usuario mediante páginas maestras y parciales](# "volver a usar la interfaz de usuario utilizando las páginas principales y parciales")
8. [Cómo implementar la paginación eficiente de los datos](# "implementar eficaz paginación de datos")
9. [Cómo proteger las aplicaciones mediante la autenticación y autorización](# "proteger aplicaciones mediante la autenticación y autorización")
10. [Cómo usar AJAX para entregar actualizaciones dinámicas](# "usar AJAX para entregar actualizaciones dinámicas")
11. [Cómo usar AJAX para implementar escenarios de asignación](# "usar AJAX para implementar escenarios de asignación")
12. [Cómo habilitar las pruebas unitarias automatizadas](# "habilitar pruebas automatizadas de unidades")

Puede crear su propia copia de NerdDinner desde el principio al completar cada paso se tutorial en este capítulo. Como alternativa, puede descargar una versión completa del código fuente aquí: [NerdDinner en GitHub](https://github.com/AspNetMVPSamples/NerdDinner). Opcionalmente, también puede también [descargar una versión gratuita de PDF de este tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) si desea leer el tutorial sin conexión.

Puede usar Visual Studio 2008 o el gratuita Visual Web Developer 2008 Express para compilar la aplicación. Puede usar SQL Server o la versión gratuita SQL Server Express para la base de datos.

Puede instalar ASP.NET MVC, Visual Web Developer 2008 Express y SQL Server Express (todo gratis) con V2 de la [instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>Ahora vamos a empezar a trabajar...

Ahora que hemos analizado NerdDinner What ' s, vamos a subirnos la mangas y escribir algo de código.

Empezaremos utilizando archivo -&gt;nuevo proyecto en Visual Studio para crear la aplicación NerdDinner.

> [!div class="step-by-step"]
> [Siguiente](create-a-new-aspnet-mvc-project.md)
