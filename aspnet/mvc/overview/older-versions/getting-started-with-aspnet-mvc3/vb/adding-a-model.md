---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Agregar un modelo (VB) | Documentos de Microsoft
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: e9a271c64347b4004d5cc5d9d91085c4e642e95d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868144"
---
<a name="adding-a-model-vb"></a><span data-ttu-id="cac7b-103">Agregar un modelo (VB)</span><span class="sxs-lookup"><span data-stu-id="cac7b-103">Adding a Model (VB)</span></span>
====================
<span data-ttu-id="cac7b-104">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="cac7b-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="cac7b-105">Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cac7b-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="cac7b-106">Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación.</span><span class="sxs-lookup"><span data-stu-id="cac7b-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="cac7b-107">Puede instalar todas ellas haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="cac7b-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="cac7b-108">Como alternativa, puede instalar por separado los requisitos previos mediante los siguientes vínculos:</span><span class="sxs-lookup"><span data-stu-id="cac7b-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="cac7b-109">Requisitos previos visuales Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="cac7b-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="cac7b-110">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="cac7b-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="cac7b-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(se admiten en tiempo de ejecución + herramientas)</span><span class="sxs-lookup"><span data-stu-id="cac7b-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="cac7b-112">Si usa Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="cac7b-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="cac7b-113">Un proyecto de Visual Web Developer con el código fuente VB.NET está disponible como acompañamiento de este tema.</span><span class="sxs-lookup"><span data-stu-id="cac7b-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="cac7b-114">[Descargue la versión VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="cac7b-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="cac7b-115">Si lo prefiere C#, cambie a la [versión de C#](../cs/adding-a-model.md) de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="cac7b-115">If you prefer C#, switch to the [C# version](../cs/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="cac7b-116">Agregar un modelo</span><span class="sxs-lookup"><span data-stu-id="cac7b-116">Adding a Model</span></span>

<span data-ttu-id="cac7b-117">En esta sección agregará algunas clases para la administración de películas en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="cac7b-117">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="cac7b-118">Estas clases será la parte "modelo" de la aplicación de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="cac7b-118">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="cac7b-119">Deberá usar una tecnología de acceso a datos de .NET Framework conocida como Entity Framework para definir y trabajar con estas clases de modelo.</span><span class="sxs-lookup"><span data-stu-id="cac7b-119">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="cac7b-120">El Entity Framework (a menudo denominado EF) es compatible con un paradigma de desarrollo llama *Code First*.</span><span class="sxs-lookup"><span data-stu-id="cac7b-120">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="cac7b-121">Código primero le permite crear objetos del modelo mediante la escritura de clases simples.</span><span class="sxs-lookup"><span data-stu-id="cac7b-121">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="cac7b-122">(También conocido como son clases POCO, de "objetos CLR antiguos sin formato".) A continuación, puede hacer que la base de datos que se crea sobre la marcha de las clases, lo que permite a un flujo de trabajo de desarrollo muy limpio y rápido.</span><span class="sxs-lookup"><span data-stu-id="cac7b-122">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="cac7b-123">Agregar clases de modelo</span><span class="sxs-lookup"><span data-stu-id="cac7b-123">Adding Model Classes</span></span>

<span data-ttu-id="cac7b-124">En **el Explorador de soluciones**, haga clic con el *modelos* carpeta, seleccione **agregar**y, a continuación, seleccione **clase**.</span><span class="sxs-lookup"><span data-stu-id="cac7b-124">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="cac7b-125">Nombre de la clase "Película".</span><span class="sxs-lookup"><span data-stu-id="cac7b-125">Name the class "Movie".</span></span>

<span data-ttu-id="cac7b-126">Agregue las siguientes propiedades de cinco a la `Movie` clase:</span><span class="sxs-lookup"><span data-stu-id="cac7b-126">Add the following five properties to the `Movie` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

<span data-ttu-id="cac7b-127">Vamos a usar la `Movie` clase para representar las películas de una base de datos.</span><span class="sxs-lookup"><span data-stu-id="cac7b-127">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="cac7b-128">Cada instancia de un `Movie` objeto corresponderá a una fila en una tabla de base de datos y cada propiedad de la `Movie` clase se asignará a una columna en la tabla.</span><span class="sxs-lookup"><span data-stu-id="cac7b-128">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="cac7b-129">En el mismo archivo, agregue las siguientes `MovieDBContext` clase:</span><span class="sxs-lookup"><span data-stu-id="cac7b-129">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

<span data-ttu-id="cac7b-130">El `MovieDBContext` clase representa el contexto de base de datos de película de Entity Framework, que controla capturar, almacenar y actualizar `Movie` instancias en una base de datos de clase.</span><span class="sxs-lookup"><span data-stu-id="cac7b-130">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="cac7b-131">El `MovieDBContext` se deriva de la `DbContext` clase proporcionada por el marco de trabajo de la entidad base.</span><span class="sxs-lookup"><span data-stu-id="cac7b-131">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="cac7b-132">Para obtener más información acerca de `DbContext` y `DbSet`, consulte [mejoras de productividad para Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="cac7b-132">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="cac7b-133">Para poder hacer referencia a `DbContext` y `DbSet`, debe agregar la siguiente `imports` instrucción en la parte superior del archivo:</span><span class="sxs-lookup"><span data-stu-id="cac7b-133">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `imports` statement at the top of the file:</span></span>

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

<span data-ttu-id="cac7b-134">La sección completa *Movie.vb* archivo se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="cac7b-134">The complete *Movie.vb* file is shown below.</span></span>

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="cac7b-135">Crear una cadena de conexión y trabajar con SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="cac7b-135">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="cac7b-136">El `MovieDBContext` clase creada controla la tarea de conectarse a la base de datos y asignación `Movie` objetos a los registros de base de datos.</span><span class="sxs-lookup"><span data-stu-id="cac7b-136">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="cac7b-137">Una pregunta que podría preguntar, sin embargo, se muestra cómo especificar qué base de datos se conecta a.</span><span class="sxs-lookup"><span data-stu-id="cac7b-137">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="cac7b-138">Llevará a cabo mediante la adición de información de conexión en el *Web.config* archivo de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="cac7b-138">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="cac7b-139">Abrir la raíz de la aplicación *Web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="cac7b-139">Open the application root *Web.config* file.</span></span> <span data-ttu-id="cac7b-140">(No el *Web.config* un archivo en el *vistas* carpeta.) La imagen siguiente se muestran ambos *Web.config* archivos; abrir el *Web.config* archivo con un círculo rojo.</span><span class="sxs-lookup"><span data-stu-id="cac7b-140">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image2.png)

## 

<span data-ttu-id="cac7b-141">Agregue la siguiente cadena de conexión para el `<connectionStrings>` elemento en el *Web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="cac7b-141">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="cac7b-142">En el ejemplo siguiente se muestra una parte de la *Web.config* archivo con la nueva cadena de conexión agregada:</span><span class="sxs-lookup"><span data-stu-id="cac7b-142">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="cac7b-143">Esta cantidad pequeña de código y XML es todo lo que necesario para escribir con el fin de representar y almacenar los datos de la película en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="cac7b-143">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="cac7b-144">A continuación, vamos a compilar un nuevo `MoviesController` clase que puede usar para mostrar los datos de la película y permitir a los usuarios crear nuevos anuncios de película.</span><span class="sxs-lookup"><span data-stu-id="cac7b-144">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cac7b-145">[Anterior](adding-a-view.md)
> [Siguiente](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="cac7b-145">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
