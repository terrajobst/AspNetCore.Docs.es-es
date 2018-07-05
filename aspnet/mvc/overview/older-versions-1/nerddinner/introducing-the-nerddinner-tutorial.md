---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Introducción al Tutorial de NerdDinner | Microsoft Docs
author: shanselman
description: Es la mejor forma de aprender un nuevo marco crear algo con él. Este tutorial le guía a través de cómo crear una aplicación pequeña, pero completa, mediante la configuración de ASP.NE...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 735a775752beda3fc24852422b1ff9d60a7c46b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371633"
---
<a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="056cd-104">Introducción al Tutorial de NerdDinner</span><span class="sxs-lookup"><span data-stu-id="056cd-104">Introducing the NerdDinner Tutorial</span></span>
====================
<span data-ttu-id="056cd-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="056cd-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="056cd-106">Descargar PDF</span><span class="sxs-lookup"><span data-stu-id="056cd-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="056cd-107">Es la mejor forma de aprender un nuevo marco crear algo con él.</span><span class="sxs-lookup"><span data-stu-id="056cd-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="056cd-108">Este tutorial explica cómo crear un pequeño, pero completa, la aplicación mediante ASP.NET MVC 1 y presenta algunos de los principales conceptos detrás de él.</span><span class="sxs-lookup"><span data-stu-id="056cd-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="056cd-109">Si usa ASP.NET MVC 3, se recomienda que siga el [Introducción a trabajar con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.</span><span class="sxs-lookup"><span data-stu-id="056cd-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-tutorial"></a><span data-ttu-id="056cd-110">Tutorial de NerdDinner</span><span class="sxs-lookup"><span data-stu-id="056cd-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="056cd-111">Es la mejor forma de aprender un nuevo marco crear algo con él.</span><span class="sxs-lookup"><span data-stu-id="056cd-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="056cd-112">Este tutorial explica cómo crear un pequeño, pero completa, la aplicación mediante ASP.NET MVC y presenta algunos de los principales conceptos detrás de él.</span><span class="sxs-lookup"><span data-stu-id="056cd-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="056cd-113">La aplicación, vamos a crear se denomina "NerdDinner".</span><span class="sxs-lookup"><span data-stu-id="056cd-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="056cd-114">NerdDinner proporciona una manera fácil para los usuarios buscar y organizar las cenas en línea:</span><span class="sxs-lookup"><span data-stu-id="056cd-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="056cd-115">NerdDinner permite a los usuarios registrados podrán crear, editar y eliminar las cenas.</span><span class="sxs-lookup"><span data-stu-id="056cd-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="056cd-116">Aplica un conjunto coherente de validación y reglas de negocios a través de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="056cd-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="056cd-117">Los visitantes puede utilizar una asignación basada en AJAX para buscar próximos dinners mantenidos cerca de ellos:</span><span class="sxs-lookup"><span data-stu-id="056cd-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="056cd-118">Al hacer clic en una cena llevará a una página de detalles donde puede aprender más sobre ella:</span><span class="sxs-lookup"><span data-stu-id="056cd-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="056cd-119">Si están interesados en asistir a la cena pueden iniciar sesión o registrarse en el sitio:</span><span class="sxs-lookup"><span data-stu-id="056cd-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="056cd-120">Puede, a continuación, haga clic en un vínculo RSVP basadas en AJAX para participar en el evento:</span><span class="sxs-lookup"><span data-stu-id="056cd-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="056cd-121">Implementación de NerdDinner</span><span class="sxs-lookup"><span data-stu-id="056cd-121">Implementing NerdDinner</span></span>

<span data-ttu-id="056cd-122">Vamos a empezar a nuestra aplicación NerdDinner utilizando el archivo -&gt;comando nuevo proyecto en Visual Studio para crear un nuevo proyecto de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="056cd-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="056cd-123">A continuación, incrementalmente agregaremos las características y funcionalidad.</span><span class="sxs-lookup"><span data-stu-id="056cd-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="056cd-124">En el camino que trataremos:</span><span class="sxs-lookup"><span data-stu-id="056cd-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="056cd-125">Cómo crear un nuevo proyecto de MVC de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="056cd-125">How to create a new ASP.NET MVC Project</span></span>](# "crear un nuevo proyecto de MVC de ASP.NET")
2. [<span data-ttu-id="056cd-126">Cómo crear una base de datos</span><span class="sxs-lookup"><span data-stu-id="056cd-126">How to create a database</span></span>](# "crear una base de datos")
3. [<span data-ttu-id="056cd-127">Cómo crear un modelo con validaciones de regla de negocio</span><span class="sxs-lookup"><span data-stu-id="056cd-127">How to build a model with business rule validations</span></span>](# "crear un modelo con validaciones de regla de negocio")
4. [<span data-ttu-id="056cd-128">Uso de controladores y vistas para implementar una interfaz de usuario de lista/detalles</span><span class="sxs-lookup"><span data-stu-id="056cd-128">How to use controllers and views to implement a listing/details UI</span></span>](# "usar controladores y vistas para implementar una interfaz de usuario de la lista/detalles")
5. <span data-ttu-id="056cd-129">[Cómo proporcionar CRUD (crear, leer, actualizar y eliminar) compatibilidad con entradas de formularios de datos](# "proporcionar CRUD (creación, lectura, actualización, eliminación) de datos formulario de entrada compatible")</span><span class="sxs-lookup"><span data-stu-id="056cd-129">[How to provide CRUD (create, read, update, delete) data form entry support](# "Provide CRUD (Create, Read, Update, Delete) Data Form Entry Support")</span></span>
6. [<span data-ttu-id="056cd-130">Cómo usar ViewData e implementar las clases ViewModel</span><span class="sxs-lookup"><span data-stu-id="056cd-130">How to use ViewData and implement ViewModel classes</span></span>](# "usar ViewData e implementar clases ViewModel")
7. [<span data-ttu-id="056cd-131">Cómo volver a usar la interfaz de usuario mediante páginas maestras y parciales</span><span class="sxs-lookup"><span data-stu-id="056cd-131">How to re-use UI using master pages and partials</span></span>](# "volver a usar la interfaz de usuario utilizando las páginas principales y parciales")
8. [<span data-ttu-id="056cd-132">Cómo implementar la paginación eficiente de los datos</span><span class="sxs-lookup"><span data-stu-id="056cd-132">How to implement efficient data paging</span></span>](# "implementar eficaz paginación de datos")
9. [<span data-ttu-id="056cd-133">Cómo proteger las aplicaciones mediante la autenticación y autorización</span><span class="sxs-lookup"><span data-stu-id="056cd-133">How to secure applications using authentication and authorization</span></span>](# "proteger aplicaciones mediante la autenticación y autorización")
10. [<span data-ttu-id="056cd-134">Cómo usar AJAX para entregar actualizaciones dinámicas</span><span class="sxs-lookup"><span data-stu-id="056cd-134">How to use AJAX to deliver dynamic updates</span></span>](# "usar AJAX para entregar actualizaciones dinámicas")
11. [<span data-ttu-id="056cd-135">Cómo usar AJAX para implementar escenarios de asignación</span><span class="sxs-lookup"><span data-stu-id="056cd-135">How to use AJAX to implement mapping scenarios</span></span>](# "usar AJAX para implementar escenarios de asignación")
12. [<span data-ttu-id="056cd-136">Cómo habilitar las pruebas unitarias automatizadas</span><span class="sxs-lookup"><span data-stu-id="056cd-136">How to enable automated unit testing</span></span>](# "habilitar pruebas automatizadas de unidades")

<span data-ttu-id="056cd-137">Puede crear su propia copia de NerdDinner desde el principio al completar cada paso se tutorial en este capítulo.</span><span class="sxs-lookup"><span data-stu-id="056cd-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="056cd-138">Como alternativa, puede descargar una versión completa del código fuente aquí: [NerdDinner en GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span><span class="sxs-lookup"><span data-stu-id="056cd-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="056cd-139">Opcionalmente, también puede también [descargar una versión gratuita de PDF de este tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) si desea leer el tutorial sin conexión.</span><span class="sxs-lookup"><span data-stu-id="056cd-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="056cd-140">Puede usar Visual Studio 2008 o el gratuita Visual Web Developer 2008 Express para compilar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="056cd-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="056cd-141">Puede usar SQL Server o la versión gratuita SQL Server Express para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="056cd-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="056cd-142">Puede instalar ASP.NET MVC, Visual Web Developer 2008 Express y SQL Server Express (todo gratis) con V2 de la [instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="056cd-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="056cd-143">Ahora vamos a empezar a trabajar...</span><span class="sxs-lookup"><span data-stu-id="056cd-143">Now let's get started....</span></span>

<span data-ttu-id="056cd-144">Ahora que hemos analizado NerdDinner What ' s, vamos a subirnos la mangas y escribir algo de código.</span><span class="sxs-lookup"><span data-stu-id="056cd-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="056cd-145">Empezaremos utilizando archivo -&gt;nuevo proyecto en Visual Studio para crear la aplicación NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="056cd-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="056cd-146">Siguiente</span><span class="sxs-lookup"><span data-stu-id="056cd-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)
