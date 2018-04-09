---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: Agregar un modelo (C#) | Documentos de Microsoft
author: Rick-Anderson
description: 'Nota: Una versión actualizada de este tutorial está disponible aquí que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y demostraciones...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a7f9206ddd43b4a3b4f96240ee48b9414450da22
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-model-c"></a><span data-ttu-id="20ce7-104">Agregar un modelo (C#)</span><span class="sxs-lookup"><span data-stu-id="20ce7-104">Adding a Model (C#)</span></span>
====================
<span data-ttu-id="20ce7-105">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="20ce7-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="20ce7-106">Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="20ce7-106">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="20ce7-107">Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación.</span><span class="sxs-lookup"><span data-stu-id="20ce7-107">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="20ce7-108">Puede instalar todas ellas haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="20ce7-108">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="20ce7-109">Como alternativa, puede instalar por separado los requisitos previos mediante los siguientes vínculos:</span><span class="sxs-lookup"><span data-stu-id="20ce7-109">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="20ce7-110">Requisitos previos visuales Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="20ce7-110">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="20ce7-111">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="20ce7-111">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="20ce7-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(se admiten en tiempo de ejecución + herramientas)</span><span class="sxs-lookup"><span data-stu-id="20ce7-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="20ce7-113">Si usa Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="20ce7-113">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="20ce7-114">Un proyecto de Visual Web Developer con el código fuente de C# está disponible como acompañamiento de este tema.</span><span class="sxs-lookup"><span data-stu-id="20ce7-114">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="20ce7-115">[Descargue la versión de C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="20ce7-115">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="20ce7-116">Si prefiere Visual Basic, cambie a la [versión de Visual Basic](../vb/adding-a-model.md) de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="20ce7-116">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="20ce7-117">Agregar un modelo</span><span class="sxs-lookup"><span data-stu-id="20ce7-117">Adding a Model</span></span>

<span data-ttu-id="20ce7-118">En esta sección agregará algunas clases para la administración de películas en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="20ce7-118">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="20ce7-119">Estas clases será la parte "modelo" de la aplicación de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="20ce7-119">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="20ce7-120">Deberá usar una tecnología de acceso a datos de .NET Framework conocida como Entity Framework para definir y trabajar con estas clases de modelo.</span><span class="sxs-lookup"><span data-stu-id="20ce7-120">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="20ce7-121">El Entity Framework (a menudo denominado EF) es compatible con un paradigma de desarrollo llama *Code First*.</span><span class="sxs-lookup"><span data-stu-id="20ce7-121">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="20ce7-122">Código primero le permite crear objetos del modelo mediante la escritura de clases simples.</span><span class="sxs-lookup"><span data-stu-id="20ce7-122">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="20ce7-123">(También conocido como son clases POCO, de "objetos CLR antiguos sin formato".) A continuación, puede hacer que la base de datos que se crea sobre la marcha de las clases, lo que permite a un flujo de trabajo de desarrollo muy limpio y rápido.</span><span class="sxs-lookup"><span data-stu-id="20ce7-123">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="20ce7-124">Agregar clases de modelo</span><span class="sxs-lookup"><span data-stu-id="20ce7-124">Adding Model Classes</span></span>

<span data-ttu-id="20ce7-125">En **el Explorador de soluciones**, haga clic con el *modelos* carpeta, seleccione **agregar**y, a continuación, seleccione **clase**.</span><span class="sxs-lookup"><span data-stu-id="20ce7-125">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="20ce7-126">Nombre de la *clase* "Película".</span><span class="sxs-lookup"><span data-stu-id="20ce7-126">Name the *class* "Movie".</span></span>

<span data-ttu-id="20ce7-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="20ce7-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span></span>

<span data-ttu-id="20ce7-128">Agregue las siguientes propiedades de cinco a la `Movie` clase:</span><span class="sxs-lookup"><span data-stu-id="20ce7-128">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="20ce7-129">Vamos a usar la `Movie` clase para representar las películas de una base de datos.</span><span class="sxs-lookup"><span data-stu-id="20ce7-129">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="20ce7-130">Cada instancia de un `Movie` objeto corresponderá a una fila en una tabla de base de datos y cada propiedad de la `Movie` clase se asignará a una columna en la tabla.</span><span class="sxs-lookup"><span data-stu-id="20ce7-130">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="20ce7-131">En el mismo archivo, agregue las siguientes `MovieDBContext` clase:</span><span class="sxs-lookup"><span data-stu-id="20ce7-131">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="20ce7-132">El `MovieDBContext` clase representa el contexto de base de datos de película de Entity Framework, que controla capturar, almacenar y actualizar `Movie` instancias en una base de datos de clase.</span><span class="sxs-lookup"><span data-stu-id="20ce7-132">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="20ce7-133">El `MovieDBContext` se deriva de la `DbContext` clase proporcionada por el marco de trabajo de la entidad base.</span><span class="sxs-lookup"><span data-stu-id="20ce7-133">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="20ce7-134">Para obtener más información acerca de `DbContext` y `DbSet`, consulte [mejoras de productividad para Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="20ce7-134">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="20ce7-135">Para poder hacer referencia a `DbContext` y `DbSet`, debe agregar la siguiente `using` instrucción en la parte superior del archivo:</span><span class="sxs-lookup"><span data-stu-id="20ce7-135">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="20ce7-136">La sección completa *Movie.cs* archivo se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="20ce7-136">The complete *Movie.cs* file is shown below.</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="20ce7-137">Crear una cadena de conexión y trabajar con SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="20ce7-137">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="20ce7-138">El `MovieDBContext` clase creada controla la tarea de conectarse a la base de datos y asignación `Movie` objetos a los registros de base de datos.</span><span class="sxs-lookup"><span data-stu-id="20ce7-138">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="20ce7-139">Una pregunta que podría preguntar, sin embargo, se muestra cómo especificar qué base de datos se conecta a.</span><span class="sxs-lookup"><span data-stu-id="20ce7-139">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="20ce7-140">Llevará a cabo mediante la adición de información de conexión en el *Web.config* archivo de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="20ce7-140">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="20ce7-141">Abrir la raíz de la aplicación *Web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="20ce7-141">Open the application root *Web.config* file.</span></span> <span data-ttu-id="20ce7-142">(No el *Web.config* un archivo en el *vistas* carpeta.) La imagen siguiente se muestran ambos *Web.config* archivos; abrir el *Web.config* archivo con un círculo rojo.</span><span class="sxs-lookup"><span data-stu-id="20ce7-142">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image4.png)

### 

<span data-ttu-id="20ce7-143">Agregue la siguiente cadena de conexión para el `<connectionStrings>` elemento en el *Web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="20ce7-143">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="20ce7-144">En el ejemplo siguiente se muestra una parte de la *Web.config* archivo con la nueva cadena de conexión agregada:</span><span class="sxs-lookup"><span data-stu-id="20ce7-144">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="20ce7-145">Esta cantidad pequeña de código y XML es todo lo que necesario para escribir con el fin de representar y almacenar los datos de la película en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="20ce7-145">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="20ce7-146">A continuación, vamos a compilar un nuevo `MoviesController` clase que puede usar para mostrar los datos de la película y permitir a los usuarios crear nuevos anuncios de película.</span><span class="sxs-lookup"><span data-stu-id="20ce7-146">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="20ce7-147">[Anterior](adding-a-view.md)
> [Siguiente](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="20ce7-147">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
