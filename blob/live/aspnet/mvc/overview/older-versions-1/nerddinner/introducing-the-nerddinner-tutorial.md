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
<a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="4f496-104">Introducción al Tutorial NerdDinner</span><span class="sxs-lookup"><span data-stu-id="4f496-104">Introducing the NerdDinner Tutorial</span></span>
====================
<span data-ttu-id="4f496-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="4f496-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="4f496-106">Descarga de PDF</span><span class="sxs-lookup"><span data-stu-id="4f496-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="4f496-107">Es la mejor manera de aprender un nuevo marco compilar algo con él.</span><span class="sxs-lookup"><span data-stu-id="4f496-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="4f496-108">Este tutorial le guía por el proceso de generar una pequeña, pero completa, la aplicación mediante ASP.NET MVC 1 y presenta algunos de los conceptos básicos detrás de él.</span><span class="sxs-lookup"><span data-stu-id="4f496-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="4f496-109">Si usa ASP.NET MVC 3, se recomienda que siga el [obtener iniciado con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [tienda de música de MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.</span><span class="sxs-lookup"><span data-stu-id="4f496-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-tutorial"></a><span data-ttu-id="4f496-110">Tutorial de NerdDinner</span><span class="sxs-lookup"><span data-stu-id="4f496-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="4f496-111">Es la mejor manera de aprender un nuevo marco compilar algo con él.</span><span class="sxs-lookup"><span data-stu-id="4f496-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="4f496-112">Este tutorial le guía por el proceso de generar una pequeña, pero completa, la aplicación mediante ASP.NET MVC y presenta algunos de los conceptos básicos detrás de él.</span><span class="sxs-lookup"><span data-stu-id="4f496-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="4f496-113">La aplicación que vamos a crear se denomina "NerdDinner".</span><span class="sxs-lookup"><span data-stu-id="4f496-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="4f496-114">NerdDinner proporciona una manera sencilla para los usuarios a buscar y organizar cenas en línea:</span><span class="sxs-lookup"><span data-stu-id="4f496-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="4f496-115">NerdDinner permite a los usuarios registrados crear, editar y eliminar cenas.</span><span class="sxs-lookup"><span data-stu-id="4f496-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="4f496-116">Aplica un conjunto coherente de reglas de validación y empresariales a través de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="4f496-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="4f496-117">Los visitantes puede utilizar una asignación basada en AJAX para buscar próximas cenas retenidos cerca de ellos:</span><span class="sxs-lookup"><span data-stu-id="4f496-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="4f496-118">Al hacer clic en una cena lleva a una página de detalles donde puede obtener más información:</span><span class="sxs-lookup"><span data-stu-id="4f496-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="4f496-119">Si están interesados en asistir a la cena pueden iniciar sesión o registrarse en el sitio:</span><span class="sxs-lookup"><span data-stu-id="4f496-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="4f496-120">Puede, a continuación, haga clic en un vínculo a RSVP basadas en AJAX para participar en el evento:</span><span class="sxs-lookup"><span data-stu-id="4f496-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="4f496-121">Implementar NerdDinner</span><span class="sxs-lookup"><span data-stu-id="4f496-121">Implementing NerdDinner</span></span>

<span data-ttu-id="4f496-122">Vamos a empezar a nuestra aplicación NerdDinner utilizando el archivo -&gt;comando nuevo proyecto en Visual Studio para crear un nuevo proyecto de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4f496-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="4f496-123">A continuación, incrementalmente agregaremos funcionalidad y las características.</span><span class="sxs-lookup"><span data-stu-id="4f496-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="4f496-124">A lo largo de la forma se tratarán:</span><span class="sxs-lookup"><span data-stu-id="4f496-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="4f496-125">Cómo crear un nuevo proyecto de MVC de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4f496-125">How to create a new ASP.NET MVC Project</span></span>](# "crear un nuevo proyecto de MVC de ASP.NET")
2. [<span data-ttu-id="4f496-126">Cómo crear una base de datos</span><span class="sxs-lookup"><span data-stu-id="4f496-126">How to create a database</span></span>](# "crear una base de datos")
3. [<span data-ttu-id="4f496-127">Cómo crear un modelo con las validaciones de regla de negocios</span><span class="sxs-lookup"><span data-stu-id="4f496-127">How to build a model with business rule validations</span></span>](# "generar un modelo con las validaciones de regla de negocios")
4. [<span data-ttu-id="4f496-128">Cómo utilizar controladores y vistas para implementar una interfaz de usuario de lista/detalles</span><span class="sxs-lookup"><span data-stu-id="4f496-128">How to use controllers and views to implement a listing/details UI</span></span>](# "usar controladores y vistas para implementar una interfaz de usuario de la lista y detalles")
5. <span data-ttu-id="4f496-129">[Cómo proporcionar CRUD (crear, leer, actualizar y eliminar) compatibilidad con entrada de formularios de datos](# "admite de entrada de formulario de datos de proporciona CRUD (creación, lectura, actualización, eliminación)")</span><span class="sxs-lookup"><span data-stu-id="4f496-129">[How to provide CRUD (create, read, update, delete) data form entry support](# "Provide CRUD (Create, Read, Update, Delete) Data Form Entry Support")</span></span>
6. [<span data-ttu-id="4f496-130">Cómo utilizar ViewData e implementar clases de ViewModel</span><span class="sxs-lookup"><span data-stu-id="4f496-130">How to use ViewData and implement ViewModel classes</span></span>](# "usar ViewData e implementar clases de modelo de vista")
7. [<span data-ttu-id="4f496-131">Cómo volver a usar la interfaz de usuario mediante páginas maestras y parciales</span><span class="sxs-lookup"><span data-stu-id="4f496-131">How to re-use UI using master pages and partials</span></span>](# "volver a usar interfaz de usuario usando las páginas principales y parciales")
8. [<span data-ttu-id="4f496-132">Cómo implementar la paginación de datos eficaz</span><span class="sxs-lookup"><span data-stu-id="4f496-132">How to implement efficient data paging</span></span>](# "implementar eficaz paginación de datos")
9. [<span data-ttu-id="4f496-133">Cómo proteger las aplicaciones mediante la autenticación y autorización</span><span class="sxs-lookup"><span data-stu-id="4f496-133">How to secure applications using authentication and authorization</span></span>](# "segura aplicaciones mediante la autenticación y autorización")
10. [<span data-ttu-id="4f496-134">Cómo usar AJAX para entregar las actualizaciones dinámicas</span><span class="sxs-lookup"><span data-stu-id="4f496-134">How to use AJAX to deliver dynamic updates</span></span>](# "usar AJAX para entregar las actualizaciones dinámicas")
11. [<span data-ttu-id="4f496-135">Cómo usar AJAX para implementar escenarios de asignación</span><span class="sxs-lookup"><span data-stu-id="4f496-135">How to use AJAX to implement mapping scenarios</span></span>](# "usar AJAX para implementar escenarios de asignación")
12. [<span data-ttu-id="4f496-136">Cómo habilitar las pruebas unitarias automatizadas</span><span class="sxs-lookup"><span data-stu-id="4f496-136">How to enable automated unit testing</span></span>](# "habilitar pruebas unitarias automatizadas")

<span data-ttu-id="4f496-137">Puede crear su propia copia del NerdDinner desde el principio siguiendo cada paso se tutorial en este capítulo.</span><span class="sxs-lookup"><span data-stu-id="4f496-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="4f496-138">Como alternativa, puede descargar una versión completa del código fuente aquí: [NerdDinner en GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span><span class="sxs-lookup"><span data-stu-id="4f496-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="4f496-139">Además, opcionalmente, también puede [descargar una versión gratuita de PDF de este tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) si desea leer el tutorial sin conexión.</span><span class="sxs-lookup"><span data-stu-id="4f496-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="4f496-140">Puede usar Visual Studio 2008 o el libre Visual Web Developer 2008 Express para compilar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4f496-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="4f496-141">Puede usar SQL Server o la libre SQL Server Express para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4f496-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="4f496-142">Puede instalar ASP.NET MVC, Visual Web Developer 2008 Express y SQL Server Express (todos los gratis) con V2 de la [instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="4f496-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="4f496-143">Ahora vamos a empezar a...</span><span class="sxs-lookup"><span data-stu-id="4f496-143">Now let's get started....</span></span>

<span data-ttu-id="4f496-144">Ahora que hemos analizado ¿qué es NerdDinner, vamos a nuestro manguitos se acumulan y escribir código.</span><span class="sxs-lookup"><span data-stu-id="4f496-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="4f496-145">Comenzaremos mediante el uso de archivo -&gt;nuevo proyecto en Visual Studio para crear la aplicación NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="4f496-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="4f496-146">Siguiente</span><span class="sxs-lookup"><span data-stu-id="4f496-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)
