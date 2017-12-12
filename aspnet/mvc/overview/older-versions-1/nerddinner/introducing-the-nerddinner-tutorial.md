---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: "Introducción al Tutorial NerdDinner | Documentos de Microsoft"
author: shanselman
description: "Es la mejor manera de aprender un nuevo marco compilar algo con él. Este tutorial le guía a través de cómo crear una aplicación pequeña, pero completa, con ASP.NE..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 57eedb224e26867c78cc399b89f91b95f722074d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="introducing-the-nerddinner-tutorial"></a>Introducción al Tutorial NerdDinner
====================
por [Scott Hanselman](https://github.com/shanselman)

[Descarga de PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Es la mejor manera de aprender un nuevo marco compilar algo con él. Este tutorial le guía por el proceso de generar una pequeña, pero completa, la aplicación mediante ASP.NET MVC 1 y presenta algunos de los conceptos básicos detrás de él.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [obtener iniciado con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [tienda de música de MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-tutorial"></a>Tutorial de NerdDinner

Es la mejor manera de aprender un nuevo marco compilar algo con él. Este tutorial le guía por el proceso de generar una pequeña, pero completa, la aplicación mediante ASP.NET MVC y presenta algunos de los conceptos básicos detrás de él.

La aplicación que vamos a crear se denomina "NerdDinner". NerdDinner proporciona una manera sencilla para los usuarios a buscar y organizar cenas en línea:

![](introducing-the-nerddinner-tutorial/_static/image1.png)

NerdDinner permite a los usuarios registrados crear, editar y eliminar cenas. Aplica un conjunto coherente de reglas de validación y empresariales a través de la aplicación:

![](introducing-the-nerddinner-tutorial/_static/image2.png)

Los visitantes puede utilizar una asignación basada en AJAX para buscar próximas cenas retenidos cerca de ellos:

![](introducing-the-nerddinner-tutorial/_static/image3.png)

Al hacer clic en una cena lleva a una página de detalles donde puede obtener más información:

![](introducing-the-nerddinner-tutorial/_static/image4.png)

Si están interesados en asistir a la cena pueden iniciar sesión o registrarse en el sitio:

![](introducing-the-nerddinner-tutorial/_static/image5.png)

Puede, a continuación, haga clic en un vínculo a RSVP basadas en AJAX para participar en el evento:

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>Implementar NerdDinner

Vamos a empezar a nuestra aplicación NerdDinner utilizando el archivo -&gt;comando nuevo proyecto en Visual Studio para crear un nuevo proyecto de ASP.NET MVC. A continuación, incrementalmente agregaremos funcionalidad y las características. A lo largo de la forma se tratarán:

1. [Cómo crear un nuevo proyecto de MVC de ASP.NET](# "crear un nuevo proyecto de MVC de ASP.NET")
2. [Cómo crear una base de datos](# "crear una base de datos")
3. [Cómo crear un modelo con las validaciones de regla de negocios](# "generar un modelo con las validaciones de regla de negocios")
4. [Cómo utilizar controladores y vistas para implementar una interfaz de usuario de lista/detalles](# "usar controladores y vistas para implementar una interfaz de usuario de la lista y detalles")
5. [Cómo proporcionar CRUD (crear, leer, actualizar y eliminar) compatibilidad con entrada de formularios de datos](# "admite de entrada de formulario de datos de proporciona CRUD (creación, lectura, actualización, eliminación)")
6. [Cómo utilizar ViewData e implementar clases de ViewModel](# "usar ViewData e implementar clases de modelo de vista")
7. [Cómo volver a usar la interfaz de usuario mediante páginas maestras y parciales](# "volver a usar interfaz de usuario usando las páginas principales y parciales")
8. [Cómo implementar la paginación de datos eficaz](# "implementar eficaz paginación de datos")
9. [Cómo proteger las aplicaciones mediante la autenticación y autorización](# "segura aplicaciones mediante la autenticación y autorización")
10. [Cómo usar AJAX para entregar las actualizaciones dinámicas](# "usar AJAX para entregar las actualizaciones dinámicas")
11. [Cómo usar AJAX para implementar escenarios de asignación](# "usar AJAX para implementar escenarios de asignación")
12. [Cómo habilitar las pruebas unitarias automatizadas](# "habilitar pruebas unitarias automatizadas")

Puede crear su propia copia del NerdDinner desde el principio siguiendo cada paso se tutorial en este capítulo. Como alternativa, puede descargar una versión completa del código fuente aquí: [NerdDinner en GitHub](https://github.com/AspNetMVPSamples/NerdDinner). Además, opcionalmente, también puede [descargar una versión gratuita de PDF de este tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) si desea leer el tutorial sin conexión.

Puede usar Visual Studio 2008 o el libre Visual Web Developer 2008 Express para compilar la aplicación. Puede usar SQL Server o la libre SQL Server Express para la base de datos.

Puede instalar ASP.NET MVC, Visual Web Developer 2008 Express y SQL Server Express (todos los gratis) con V2 de la [instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>Ahora vamos a empezar a...

Ahora que hemos analizado ¿qué es NerdDinner, vamos a nuestro manguitos se acumulan y escribir código.

Comenzaremos mediante el uso de archivo -&gt;nuevo proyecto en Visual Studio para crear la aplicación NerdDinner.

>[!div class="step-by-step"]
[Siguiente](create-a-new-aspnet-mvc-project.md)
