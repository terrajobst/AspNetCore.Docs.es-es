---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: "Introducción a ASP.NET MVC | Documentos de Microsoft"
author: shanselman
description: "Se trata de un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Crear una aplicación web simple que lee y escribe desde una base de datos."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 08c30f4aab77bff64ed3ab874d13cc5dc863fc99
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
<a name="intro-to-aspnet-mvc"></a><span data-ttu-id="39389-104">Introducción a ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="39389-104">Intro to ASP.NET MVC</span></span>
====================
<span data-ttu-id="39389-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="39389-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="39389-106">Una versión actualizada, si está disponible en este tutorial [aquí](../../getting-started/introduction/getting-started.md) con [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span><span class="sxs-lookup"><span data-stu-id="39389-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span></span> <span data-ttu-id="39389-107">El nuevo tutorial usa ASP.NET MVC 5, que proporciona muchas mejoras con respecto a este tutorial.</span><span class="sxs-lookup"><span data-stu-id="39389-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
> 
> 
> <span data-ttu-id="39389-108">Se trata de un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="39389-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="39389-109">Se creará una aplicación web simple que lee y escribe desde una base de datos.</span><span class="sxs-lookup"><span data-stu-id="39389-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="39389-110">Visite la [centro de aprendizaje de ASP.NET MVC](../../../index.md) para buscar otros ASP.NET MVC, tutoriales y ejemplos.</span><span class="sxs-lookup"><span data-stu-id="39389-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="39389-111">Vamos a hacer nuestra primera aplicación Web ASP.NET MVC usando [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="39389-111">Let's make our first ASP.NET MVC Web Application using [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="39389-112">Nos aseguraremos de hacer una pequeña aplicación de lista de películas que creará Permítanos y lista de películas.</span><span class="sxs-lookup"><span data-stu-id="39389-112">We'll make a little Movie list application that will let us create and list movies.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="39389-113">Lo que vamos a compilar</span><span class="sxs-lookup"><span data-stu-id="39389-113">What You'll Build</span></span>

<span data-ttu-id="39389-114">Presentamos dos capturas de pantalla de la aplicación que compilará.</span><span class="sxs-lookup"><span data-stu-id="39389-114">Here are two screenshots of the application you'll build.</span></span> <span data-ttu-id="39389-115">Tendrá una tabla sencilla de películas con varias columnas.</span><span class="sxs-lookup"><span data-stu-id="39389-115">You will have a simple table of movies with various columns.</span></span>

<span data-ttu-id="39389-116">[![Lista de películas - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="39389-116">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span></span>

<span data-ttu-id="39389-117">Y tendrá una forma de crear por lo que podemos agregar a la lista de películas.</span><span class="sxs-lookup"><span data-stu-id="39389-117">And you'll have a Create Form so we can add movies to the list.</span></span>

<span data-ttu-id="39389-118">[![Crear una película - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="39389-118">[![Create a Movie - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="39389-119">Obtendrá información de conocimientos</span><span class="sxs-lookup"><span data-stu-id="39389-119">Skills You'll Learn</span></span>

<span data-ttu-id="39389-120">Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="39389-120">This tutorial will teach you the basics of building an ASP.NET MVC Web Application using Visual Studio.</span></span> <span data-ttu-id="39389-121">Aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="39389-121">You'll learn:</span></span>

- <span data-ttu-id="39389-122">Cómo crear un nuevo proyecto de MVC de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="39389-122">How to create a new ASP.NET MVC Project</span></span>
- <span data-ttu-id="39389-123">Cómo crear una nueva base de datos con SQL Server</span><span class="sxs-lookup"><span data-stu-id="39389-123">How to create a new Database with SQL Server</span></span>
- <span data-ttu-id="39389-124">Cómo crear controladores de MVC de ASP.NET y vistas</span><span class="sxs-lookup"><span data-stu-id="39389-124">How to create ASP.NET MVC Controllers and Views</span></span>
- <span data-ttu-id="39389-125">Cómo recuperar y mostrar datos</span><span class="sxs-lookup"><span data-stu-id="39389-125">How to retrieve and display data</span></span>
- <span data-ttu-id="39389-126">Cómo modificar datos y habilitar la validación de datos</span><span class="sxs-lookup"><span data-stu-id="39389-126">How to edit data and enable data validation</span></span>
- <span data-ttu-id="39389-127">Cómo actualizar el esquema de base de datos</span><span class="sxs-lookup"><span data-stu-id="39389-127">How to update the database schema</span></span>

## <a name="get-started"></a><span data-ttu-id="39389-128">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="39389-128">Get Started</span></span>

<span data-ttu-id="39389-129">Comience ejecutando Visual Web Developer 2010 Express (llamaré "VWD" de ahora en adelante) y seleccione Nuevo proyecto de la pantalla de inicio.</span><span class="sxs-lookup"><span data-stu-id="39389-129">Start by running Visual Web Developer 2010 Express (I'll call it "VWD" from now on) and select New Project from the Start Screen.</span></span>

<span data-ttu-id="39389-130">Visual Web Developer es un entorno de desarrollador integrado o IDE.</span><span class="sxs-lookup"><span data-stu-id="39389-130">Visual Web Developer is an IDE, or Integrated Developer Environment.</span></span> <span data-ttu-id="39389-131">Al igual que usa Microsoft Word para escribir documentos, deberá usar un IDE para crear aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="39389-131">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="39389-132">Hay una barra de herramientas en la parte superior que muestra distintas opciones disponibles para, así como el menú también podría haber utilizado para seleccionar archivo | Nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="39389-132">There's a toolbar along the top showing various options available to you, as well as the menu you could also have used to Select File | New project.</span></span>

<span data-ttu-id="39389-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="39389-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span></span>

## <a name="creating-your-first-application"></a><span data-ttu-id="39389-134">Crear su primera aplicación</span><span class="sxs-lookup"><span data-stu-id="39389-134">Creating Your First Application</span></span>

<span data-ttu-id="39389-135">Puede crear aplicaciones mediante Visual Basic o Visual C#.</span><span class="sxs-lookup"><span data-stu-id="39389-135">You can create applications using Visual Basic or Visual C#.</span></span> <span data-ttu-id="39389-136">Por ahora, seleccione Visual C# a la izquierda, a continuación, seleccione "Aplicación Web de ASP.NET MVC 2."</span><span class="sxs-lookup"><span data-stu-id="39389-136">For now, Select Visual C# on the left, then pick "ASP.NET MVC 2 Web Application."</span></span> <span data-ttu-id="39389-137">Denomine el proyecto "Películas" y haga clic en Aceptar.</span><span class="sxs-lookup"><span data-stu-id="39389-137">Name your project "Movies" and click OK.</span></span>

<span data-ttu-id="39389-138">[![Nuevo proyecto](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="39389-138">[![New Project](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span></span>

<span data-ttu-id="39389-139">En el lado derecho es el Explorador de soluciones que muestra todos los archivos y carpetas en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="39389-139">On the right-hand side is the Solution Explorer showing all the files and folders in your application.</span></span> <span data-ttu-id="39389-140">La ventana grande en la parte central es donde se edite el código y pasan la mayor parte del tiempo.</span><span class="sxs-lookup"><span data-stu-id="39389-140">The big window in the middle is where you edit your code and spend most of your time.</span></span> <span data-ttu-id="39389-141">Visual Studio usa una plantilla predeterminada para el proyecto de MVC de ASP.NET que acaba de crear, por lo que tendrá una aplicación que funciona en este momento sin hacer nada.</span><span class="sxs-lookup"><span data-stu-id="39389-141">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="39389-142">Se trata de un simple "Hola a todos</span><span class="sxs-lookup"><span data-stu-id="39389-142">This is a simple "Hello World!</span></span> <span data-ttu-id="39389-143">proyecto y es un buen punto de partida para nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="39389-143">project, and it is a good place to start for our application.</span></span>

<span data-ttu-id="39389-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="39389-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span></span>

<span data-ttu-id="39389-145">Seleccione el botón "reproducir" en la barra de herramientas.</span><span class="sxs-lookup"><span data-stu-id="39389-145">Select the "play" button to the toolbar.</span></span>

![Iniciar depuración](getting-started-with-mvc-part1/_static/image11.png)

<span data-ttu-id="39389-147">Es una flecha verde hacia la derecha que se compila el programa e iniciar la aplicación en un explorador web.</span><span class="sxs-lookup"><span data-stu-id="39389-147">It's a green arrow pointing to the right that will compile your program and start your application in a web browser.</span></span>

<span data-ttu-id="39389-148">*Nota: Puede en su lugar, presione F5 en el teclado, o seleccione depuración -&gt;iniciar la depuración desde el menú "Debug".*</span><span class="sxs-lookup"><span data-stu-id="39389-148">*NOTE: You can instead press F5 on your keyboard, or select Debug-&gt;Start Debugging from the "Debug" menu.*</span></span>

<span data-ttu-id="39389-149">Esto hará que Visual Web Developer iniciar un servidor web de desarrollo y ejecutar la aplicación web (no hay ninguna configuración ni pasos manuales necesarios para habilitar esta opción).</span><span class="sxs-lookup"><span data-stu-id="39389-149">This will cause Visual Web Developer to start a development web-server and run our web application (there are no configuration or manual steps required to enable this).</span></span> <span data-ttu-id="39389-150">A continuación, se inicie un explorador y configurarlo para examinar la página principal de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="39389-150">It will then launch a browser and configure it to browse the application's home-page.</span></span> <span data-ttu-id="39389-151">Tenga en cuenta que a continuación que la barra de direcciones del explorador dice "localhost" y no algo parecido a ejemplo.com. Eso es porque localhost siempre apunta a su propio equipo local - que en este caso, se ejecuta la aplicación que se acaba de compilar.</span><span class="sxs-lookup"><span data-stu-id="39389-151">Notice below that the address bar of the browser says "localhost", and not something like example.com. That's because localhost always points to your own local computer - which in this case is running the application we just built.</span></span>

<span data-ttu-id="39389-152">[![Página principal](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="39389-152">[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span></span>

<span data-ttu-id="39389-153">De fábrica esta plantilla predeterminada proporciona dos páginas para visitar y una página de inicio de sesión básica.</span><span class="sxs-lookup"><span data-stu-id="39389-153">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="39389-154">Vamos a cambiar el funcionamiento de esta aplicación y aprender un poco sobre ASP.NET MVC en el proceso.</span><span class="sxs-lookup"><span data-stu-id="39389-154">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="39389-155">Cierre el explorador y permite cambiar el código.</span><span class="sxs-lookup"><span data-stu-id="39389-155">Close your browser and lets change some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="39389-156">Siguiente</span><span class="sxs-lookup"><span data-stu-id="39389-156">Next</span></span>](getting-started-with-mvc-part2.md)
