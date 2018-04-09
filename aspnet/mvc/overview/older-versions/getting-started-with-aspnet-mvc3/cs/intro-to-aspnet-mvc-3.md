---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Introducción a ASP.NET MVC 3 (C#) | Documentos de Microsoft
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 9b965df6175051a084de35627160161c116be42d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="intro-to-aspnet-mvc-3-c"></a><span data-ttu-id="d53c9-103">Introducción a ASP.NET MVC 3 (C#)</span><span class="sxs-lookup"><span data-stu-id="d53c9-103">Intro to ASP.NET MVC 3 (C#)</span></span>
====================
<span data-ttu-id="d53c9-104">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="d53c9-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="d53c9-105">Hay disponible una versión actualizada de este tutorial [aquí](../../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="d53c9-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="d53c9-106">Es más seguro y mucho más fácil de seguir y se muestra más características.</span><span class="sxs-lookup"><span data-stu-id="d53c9-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="d53c9-107">Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d53c9-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="d53c9-108">Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación.</span><span class="sxs-lookup"><span data-stu-id="d53c9-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="d53c9-109">Puede instalar todas ellas haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="d53c9-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="d53c9-110">Como alternativa, puede instalar por separado los requisitos previos mediante los siguientes vínculos:</span><span class="sxs-lookup"><span data-stu-id="d53c9-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="d53c9-111">Requisitos previos visuales Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="d53c9-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="d53c9-112">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="d53c9-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="d53c9-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(se admiten en tiempo de ejecución + herramientas)</span><span class="sxs-lookup"><span data-stu-id="d53c9-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="d53c9-114">Si usa Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="d53c9-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="d53c9-115">Un proyecto de Visual Web Developer con el código fuente de C# está disponible como acompañamiento de este tema.</span><span class="sxs-lookup"><span data-stu-id="d53c9-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="d53c9-116">[Descargue la versión de C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="d53c9-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="d53c9-117">Si prefiere Visual Basic, cambie a la [versión de Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="d53c9-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="d53c9-118">Lo que vamos a compilar</span><span class="sxs-lookup"><span data-stu-id="d53c9-118">What You'll Build</span></span>

<span data-ttu-id="d53c9-119">Se implementa una aplicación sencilla de mostrar una lista de películas que admite la creación, edición y lista de películas de una base de datos.</span><span class="sxs-lookup"><span data-stu-id="d53c9-119">You'll implement a simple movie-listing application that supports creating, editing, and listing movies from a database.</span></span> <span data-ttu-id="d53c9-120">A continuación se muestran dos capturas de pantalla de la aplicación que compilará.</span><span class="sxs-lookup"><span data-stu-id="d53c9-120">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="d53c9-121">Incluye una página que muestra una lista de películas de una base de datos:</span><span class="sxs-lookup"><span data-stu-id="d53c9-121">It includes a page that displays a list of movies from a database:</span></span>

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

<span data-ttu-id="d53c9-123">La aplicación también permite agregar, editar y eliminar películas, así como ver detalles sobre los individuales.</span><span class="sxs-lookup"><span data-stu-id="d53c9-123">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="d53c9-124">Todos los escenarios de entrada de datos incluyen la validación para asegurarse de que los datos almacenados en la base de datos son correctos.</span><span class="sxs-lookup"><span data-stu-id="d53c9-124">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="d53c9-125">Obtendrá información de conocimientos</span><span class="sxs-lookup"><span data-stu-id="d53c9-125">Skills You'll Learn</span></span>

<span data-ttu-id="d53c9-126">Esto es lo aprenderá:</span><span class="sxs-lookup"><span data-stu-id="d53c9-126">Here's what you'll learn:</span></span>

- <span data-ttu-id="d53c9-127">Cómo crear un nuevo proyecto de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d53c9-127">How to create a new ASP.NET MVC project.</span></span>
- <span data-ttu-id="d53c9-128">Cómo crear ASP.NET MVC controladores y vistas.</span><span class="sxs-lookup"><span data-stu-id="d53c9-128">How to create ASP.NET MVC controllers and views.</span></span>
- <span data-ttu-id="d53c9-129">Cómo crear una nueva base de datos mediante el paradigma de Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="d53c9-129">How to create a new database using the Entity Framework Code First paradigm.</span></span>
- <span data-ttu-id="d53c9-130">Cómo recuperar y mostrar los datos.</span><span class="sxs-lookup"><span data-stu-id="d53c9-130">How to retrieve and display data.</span></span>
- <span data-ttu-id="d53c9-131">Cómo modificar datos y habilitar la validación de datos.</span><span class="sxs-lookup"><span data-stu-id="d53c9-131">How to edit data and enable data validation.</span></span>

## <a name="getting-started"></a><span data-ttu-id="d53c9-132">Introducción</span><span class="sxs-lookup"><span data-stu-id="d53c9-132">Getting Started</span></span>

<span data-ttu-id="d53c9-133">Comience ejecutando Visual Web Developer 2010 Express ("Visual Web Developer" para abreviar) y seleccione **nuevo proyecto** desde el **iniciar** página.</span><span class="sxs-lookup"><span data-stu-id="d53c9-133">Start by running Visual Web Developer 2010 Express ("Visual Web Developer" for short) and select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="d53c9-134">Visual Web Developer es un entorno de desarrollo integrado o IDE.</span><span class="sxs-lookup"><span data-stu-id="d53c9-134">Visual Web Developer is an IDE, or integrated development environment.</span></span> <span data-ttu-id="d53c9-135">Al igual que usa Microsoft Word para escribir documentos, deberá usar un IDE para crear aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="d53c9-135">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="d53c9-136">En Visual Web Developer, hay una barra de herramientas en la parte superior que muestra distintas opciones disponibles para usted.</span><span class="sxs-lookup"><span data-stu-id="d53c9-136">In Visual Web Developer there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="d53c9-137">También hay un menú que proporciona otra manera de realizar las tareas en el IDE.</span><span class="sxs-lookup"><span data-stu-id="d53c9-137">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="d53c9-138">(Por ejemplo, en lugar de seleccionar **nuevo proyecto** desde el **iniciar** página, puede usar el menú y seleccione **archivo** &gt; **denuevoproyecto**.)</span><span class="sxs-lookup"><span data-stu-id="d53c9-138">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="d53c9-139">Crear su primera aplicación</span><span class="sxs-lookup"><span data-stu-id="d53c9-139">Creating Your First Application</span></span>

<span data-ttu-id="d53c9-140">Puede crear aplicaciones mediante Visual Basic o Visual C# como lenguaje de programación.</span><span class="sxs-lookup"><span data-stu-id="d53c9-140">You can create applications using either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="d53c9-141">Seleccione Visual C# a la izquierda y, a continuación, seleccione **aplicación Web de ASP.NET MVC 3**.</span><span class="sxs-lookup"><span data-stu-id="d53c9-141">Select Visual C# on the left and then select **ASP.NET MVC 3 Web Application**.</span></span> <span data-ttu-id="d53c9-142">Denomine el proyecto "MvcMovie" y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="d53c9-142">Name your project "MvcMovie" and then click **OK**.</span></span> <span data-ttu-id="d53c9-143">(Si lo prefiere, Visual Basic, cambie a la [versión de Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de este tutorial.)</span><span class="sxs-lookup"><span data-stu-id="d53c9-143">(If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.)</span></span>

![](intro-to-aspnet-mvc-3/_static/image5.png)

<span data-ttu-id="d53c9-144">En el **nuevo proyecto de ASP.NET MVC 3** cuadro de diálogo, seleccione **aplicación de Internet**.</span><span class="sxs-lookup"><span data-stu-id="d53c9-144">In the **New ASP.NET MVC 3 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="d53c9-145">Comprobar **uso HTML5 marcado** y deje **Razor** como el motor de vista predeterminado.</span><span class="sxs-lookup"><span data-stu-id="d53c9-145">Check **Use HTML5 markup** and leave **Razor** as the default view engine.</span></span>

![](intro-to-aspnet-mvc-3/_static/image6.png)

<span data-ttu-id="d53c9-146">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="d53c9-146">Click **OK**.</span></span> <span data-ttu-id="d53c9-147">Visual Web Developer utilizará una plantilla predeterminada para el proyecto de MVC de ASP.NET que acaba de crear, por lo que tendrá una aplicación que funciona en este momento sin hacer nada.</span><span class="sxs-lookup"><span data-stu-id="d53c9-147">Visual Web Developer used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="d53c9-148">Se trata de un simple "Hola a todos"</span><span class="sxs-lookup"><span data-stu-id="d53c9-148">This is a simple "Hello World!"</span></span> <span data-ttu-id="d53c9-149">proyecto y, de un buen lugar para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d53c9-149">project, and it's a good place to start your application.</span></span>

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

<span data-ttu-id="d53c9-150">En el menú **Depurar**, seleccione **Iniciar depuración**.</span><span class="sxs-lookup"><span data-stu-id="d53c9-150">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-3/_static/image9.png)

<span data-ttu-id="d53c9-151">Observe que el método abreviado de teclado para iniciar la depuración es F5.</span><span class="sxs-lookup"><span data-stu-id="d53c9-151">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="d53c9-152">F5 hace que Visual Web Developer iniciar un servidor de desarrollo de web y ejecutar la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="d53c9-152">F5 causes Visual Web Developer to start a development web server and run your web application.</span></span> <span data-ttu-id="d53c9-153">A continuación, Visual Web Developer inicia un explorador y se abre la página principal de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d53c9-153">Visual Web Developer then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="d53c9-154">Observe que la barra de direcciones del explorador indica `localhost` y no algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="d53c9-154">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="d53c9-155">Esto es así porque `localhost` siempre apunta a su propio equipo local, que en este caso, se ejecuta la aplicación que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="d53c9-155">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="d53c9-156">Cuando Visual Web Developer ejecuta un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="d53c9-156">When Visual Web Developer runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="d53c9-157">En la imagen siguiente, el número de puerto aleatorio es 43246.</span><span class="sxs-lookup"><span data-stu-id="d53c9-157">In the image below, the random port number is 43246.</span></span> <span data-ttu-id="d53c9-158">Al ejecutar la aplicación, probablemente verá un número de puerto diferente.</span><span class="sxs-lookup"><span data-stu-id="d53c9-158">When you run the application, you'll probably see a different port number.</span></span>

![](intro-to-aspnet-mvc-3/_static/image10.png)

<span data-ttu-id="d53c9-159">Desde el cuadro de esta plantilla predeterminada proporciona dos páginas para visitar y una página de inicio de sesión básica.</span><span class="sxs-lookup"><span data-stu-id="d53c9-159">Right out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="d53c9-160">El paso siguiente es cambiar el funcionamiento de esta aplicación y aprender un poco sobre ASP.NET MVC en el proceso.</span><span class="sxs-lookup"><span data-stu-id="d53c9-160">The next step is to change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="d53c9-161">Cierre el explorador y vamos a cambiar parte del código.</span><span class="sxs-lookup"><span data-stu-id="d53c9-161">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d53c9-162">Siguiente</span><span class="sxs-lookup"><span data-stu-id="d53c9-162">Next</span></span>](adding-a-controller.md)
