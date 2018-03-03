---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: "Conceptos básicos de ASP.NET MVC 4 | Documentos de Microsoft"
author: rick-anderson
description: "Este laboratorio práctico se basa en la tienda de música MVC (Model View Controller), una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MV..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: f93f51219403cd5aeca2dd3648444a84690c3d25
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="573f1-103">Conceptos básicos de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="573f1-103">ASP.NET MVC 4 Fundamentals</span></span>

<span data-ttu-id="573f1-104">Por [Web colonias equipo](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="573f1-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="573f1-105">Descargar el Kit de aprendizaje de colonias de Web</span><span class="sxs-lookup"><span data-stu-id="573f1-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="573f1-106">Este laboratorio práctico se basa en la tienda de música MVC (Model View Controller), una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="573f1-106">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="573f1-107">En el laboratorio, aprenderá la simplicidad, todavía power de usar estas tecnologías conjuntamente.</span><span class="sxs-lookup"><span data-stu-id="573f1-107">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="573f1-108">Se iniciará con una aplicación sencilla y compilará hasta que haya una aplicación de Web de ASP.NET MVC 4 totalmente funcional.</span><span class="sxs-lookup"><span data-stu-id="573f1-108">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>

<span data-ttu-id="573f1-109">Esta práctica funciona con ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="573f1-109">This Lab works with ASP.NET MVC 4.</span></span>

<span data-ttu-id="573f1-110">Si desea explorar la versión de ASP.NET MVC 3 de la aplicación del tutorial, puede encontrarlo en [tienda de música de MVC](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="573f1-110">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="573f1-111">Este laboratorio práctico se da por supuesto que el desarrollador tiene experiencia en tecnologías de desarrollo Web, como HTML y JavaScript.</span><span class="sxs-lookup"><span data-stu-id="573f1-111">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="573f1-112">Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web colonias, disponible en [versiones de Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="573f1-112">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="573f1-113">Está disponible en el proyecto específico para este laboratorio [Fundamentos de ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span><span class="sxs-lookup"><span data-stu-id="573f1-113">The project specific to this lab is available at [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span></span>

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="573f1-114">La aplicación de tienda de música</span><span class="sxs-lookup"><span data-stu-id="573f1-114">The Music Store application</span></span>

<span data-ttu-id="573f1-115">La aplicación web de tienda de música que se generarán a lo largo de este laboratorio consta de tres partes principales: la compra, la desprotección y administración.</span><span class="sxs-lookup"><span data-stu-id="573f1-115">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="573f1-116">Los visitantes podrán examinar álbumes por género, agregar álbumes a su carro, revise su selección y por último, ir a la caja para iniciar sesión y completar el pedido.</span><span class="sxs-lookup"><span data-stu-id="573f1-116">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="573f1-117">Además, los administradores del almacén podrá administrar los álbumes disponibles, así como sus propiedades principales.</span><span class="sxs-lookup"><span data-stu-id="573f1-117">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="573f1-118">![Pantallas de la tienda de música](aspnet-mvc-4-fundamentals/_static/image1.png "pantallas de la tienda de música")</span><span class="sxs-lookup"><span data-stu-id="573f1-118">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="573f1-119">*Pantallas de la tienda de música*</span><span class="sxs-lookup"><span data-stu-id="573f1-119">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="573f1-120">Essentials de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="573f1-120">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="573f1-121">Aplicación de la tienda de música se generarán utilizando **Model View Controller (MVC)**, un modelo de arquitectura que separa una aplicación en tres componentes principales:</span><span class="sxs-lookup"><span data-stu-id="573f1-121">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="573f1-122">**Modelos**: objetos de modelo son las partes de la aplicación que implementan la lógica del dominio.</span><span class="sxs-lookup"><span data-stu-id="573f1-122">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="573f1-123">A menudo, los objetos del modelo también recuperar y almacenan el estado del modelo en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="573f1-123">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="573f1-124">**Vistas:** vistas son los componentes que muestra la interfaz de usuario (UI) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="573f1-124">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="573f1-125">Normalmente, esta interfaz de usuario se crea a partir de los datos del modelo.</span><span class="sxs-lookup"><span data-stu-id="573f1-125">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="573f1-126">Un ejemplo sería la vista de edición de álbumes que muestra cuadros de texto y una lista desplegable en función del estado actual de un objeto de álbum.</span><span class="sxs-lookup"><span data-stu-id="573f1-126">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="573f1-127">**Controladores:** controladores son los componentes que controlen la interacción del usuario, manipulan el modelo y por último seleccionan una vista para representar la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="573f1-127">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="573f1-128">En una aplicación de MVC, la vista solo muestra información; el controlador controla y responde a la interacción y los datos que introducen los usuarios.</span><span class="sxs-lookup"><span data-stu-id="573f1-128">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="573f1-129">El modelo de MVC le ayuda a crear aplicaciones que separan los diferentes aspectos de la aplicación (lógica de entrada, lógica comercial y lógica de la interfaz de usuario), y proporciona un vago acoplamiento entre estos elementos.</span><span class="sxs-lookup"><span data-stu-id="573f1-129">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="573f1-130">Esta separación le ayuda a administrar la complejidad al compilar una aplicación, ya que permite centrarse en un aspecto de la implementación a la vez.</span><span class="sxs-lookup"><span data-stu-id="573f1-130">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="573f1-131">Además, el modelo de MVC facilita probar aplicaciones, también fomenta el uso de desarrollo controlado por pruebas (TDD) para crear aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="573f1-131">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="573f1-132">El **ASP.NET MVC** framework proporciona una alternativa al modelo de formularios Web Forms de ASP.NET para crear aplicaciones Web basadas en ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="573f1-132">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="573f1-133">El **ASP.NET MVC** framework es un marco de presentación ligera, alta comprobable que (al igual que con las aplicaciones basadas en formularios Web) se integra con las características de ASP.NET existentes, como páginas maestras y basada en pertenencia autenticación de modo que lleguen todas las posibilidades de .NET framework core.</span><span class="sxs-lookup"><span data-stu-id="573f1-133">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="573f1-134">Esto es útil si ya está familiarizado con ASP.NET Web Forms porque todas las bibliotecas que ya usa también están disponibles en ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="573f1-134">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="573f1-135">Además, el acoplamiento flexible entre los tres componentes principales de una aplicación MVC también favorece el desarrollo paralelo.</span><span class="sxs-lookup"><span data-stu-id="573f1-135">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="573f1-136">Por ejemplo, un desarrollador de software puede trabajar en la vista, un segundo desarrollador puede trabajar en la lógica del controlador y una tercera puede Centrar en la lógica de negocios en el modelo.</span><span class="sxs-lookup"><span data-stu-id="573f1-136">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="573f1-137">Objetivos</span><span class="sxs-lookup"><span data-stu-id="573f1-137">Objectives</span></span>

<span data-ttu-id="573f1-138">En este laboratorio práctico, aprenderá cómo:</span><span class="sxs-lookup"><span data-stu-id="573f1-138">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="573f1-139">Crear una aplicación de ASP.NET MVC desde el principio, basado en el tutorial de la aplicación de la tienda de música</span><span class="sxs-lookup"><span data-stu-id="573f1-139">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="573f1-140">Agregar controladores para controlar las direcciones URL a la página principal del sitio y para examinar su funcionalidad principal</span><span class="sxs-lookup"><span data-stu-id="573f1-140">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="573f1-141">Agregar una vista para personalizar el contenido que se muestra junto con su estilo</span><span class="sxs-lookup"><span data-stu-id="573f1-141">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="573f1-142">Agregar clases de modelo para contener y administrar la lógica de datos y el dominio</span><span class="sxs-lookup"><span data-stu-id="573f1-142">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="573f1-143">Usar el modelo de vista (MVC) para pasar información de las acciones de controlador a las plantillas de vista</span><span class="sxs-lookup"><span data-stu-id="573f1-143">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="573f1-144">Explorar la nueva plantilla de ASP.NET MVC 4 para las aplicaciones de internet</span><span class="sxs-lookup"><span data-stu-id="573f1-144">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="573f1-145">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="573f1-145">Prerequisites</span></span>

<span data-ttu-id="573f1-146">Debe tener los elementos siguientes para completar esta práctica:</span><span class="sxs-lookup"><span data-stu-id="573f1-146">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="573f1-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (leer [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo)</span><span class="sxs-lookup"><span data-stu-id="573f1-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="573f1-148">Programa de instalación</span><span class="sxs-lookup"><span data-stu-id="573f1-148">Setup</span></span>

<span data-ttu-id="573f1-149">**Instalación de fragmentos de código**</span><span class="sxs-lookup"><span data-stu-id="573f1-149">**Installing Code Snippets**</span></span>

<span data-ttu-id="573f1-150">Para mayor comodidad, gran parte del código que se va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="573f1-150">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="573f1-151">Para instalar los fragmentos de código que se ejecute **.\Source\Setup\CodeSnippets.vsi** archivo.</span><span class="sxs-lookup"><span data-stu-id="573f1-151">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="573f1-152">Si no está familiarizado con los fragmentos de código de Visual Studio y desea obtener información sobre cómo utilizarlas, puede consultar el apéndice de este documento &quot; [Apéndice C: Using Code Snippets](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="573f1-152">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="573f1-153">Ejercicios</span><span class="sxs-lookup"><span data-stu-id="573f1-153">Exercises</span></span>

<span data-ttu-id="573f1-154">Este laboratorio práctico se compone de los ejercicios siguientes:</span><span class="sxs-lookup"><span data-stu-id="573f1-154">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="573f1-155">Ejercicio 1: Crear el proyecto de aplicación Web de MusicStore MVC de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="573f1-155">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="573f1-156">Ejercicio 2: Creación de un controlador</span><span class="sxs-lookup"><span data-stu-id="573f1-156">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="573f1-157">Ejercicio 3: Pasar parámetros a un controlador</span><span class="sxs-lookup"><span data-stu-id="573f1-157">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="573f1-158">Ejercicio 4: Crear una vista</span><span class="sxs-lookup"><span data-stu-id="573f1-158">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="573f1-159">Ejercicio 5: Crear un modelo de vista</span><span class="sxs-lookup"><span data-stu-id="573f1-159">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="573f1-160">Ejercicio 6: Usar parámetros en la vista</span><span class="sxs-lookup"><span data-stu-id="573f1-160">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="573f1-161">Ejercicio 7: Aspectos básicos de nueva plantilla de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="573f1-161">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="573f1-162">Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debería obtener después de completar los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="573f1-162">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="573f1-163">Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="573f1-163">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="573f1-164">Tiempo estimado para completar esta práctica: **60 minutos**.</span><span class="sxs-lookup"><span data-stu-id="573f1-164">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="573f1-165">Ejercicio 1: Crear el proyecto de aplicación Web de MusicStore MVC de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="573f1-165">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="573f1-166">En este ejercicio, obtendrá información sobre cómo crear una aplicación ASP.NET MVC en Visual Studio 2012 Express para Web, así como su organización de la carpeta principal.</span><span class="sxs-lookup"><span data-stu-id="573f1-166">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="573f1-167">Además, obtendrá información sobre cómo agregar un nuevo controlador y que se muestre una cadena simple en la página principal de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="573f1-167">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="573f1-168">Tarea 1: crear el proyecto de aplicación Web de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="573f1-168">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="573f1-169">En esta tarea, creará un proyecto vacío de aplicación de ASP.NET MVC mediante la plantilla de MVC Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="573f1-169">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="573f1-170">Iniciar **VS Express para Web**.</span><span class="sxs-lookup"><span data-stu-id="573f1-170">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="573f1-171">En el menú **Archivo**, haga clic en **Nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="573f1-171">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="573f1-172">En el **nuevo proyecto** cuadro de diálogo, seleccione la **aplicación Web de ASP.NET MVC 4** proyecto tipo, ubicado en **Visual C#,** **Web** plantilla lista.</span><span class="sxs-lookup"><span data-stu-id="573f1-172">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="573f1-173">Cambiar el **nombre** a *MvcMusicStore*.</span><span class="sxs-lookup"><span data-stu-id="573f1-173">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="573f1-174">Establecer la ubicación de la solución dentro de una nueva **comenzar** carpeta en la carpeta de origen de este ejercicio, por ejemplo **\Source\Ex01-CreatingMusicStoreProject\Begin [YOUR-HOL-PATH]**.</span><span class="sxs-lookup"><span data-stu-id="573f1-174">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="573f1-175">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="573f1-175">Click **OK**.</span></span>

    <span data-ttu-id="573f1-176">![Crear cuadro de diálogo nuevo proyecto](aspnet-mvc-4-fundamentals/_static/image2.png "Crear cuadro de diálogo nuevo proyecto")</span><span class="sxs-lookup"><span data-stu-id="573f1-176">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="573f1-177">*Crear cuadro de diálogo nuevo proyecto*</span><span class="sxs-lookup"><span data-stu-id="573f1-177">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="573f1-178">En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione la **básica** plantilla y asegúrese de que el **motor de vista** seleccionado es **Razor**.</span><span class="sxs-lookup"><span data-stu-id="573f1-178">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="573f1-179">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="573f1-179">Click **OK**.</span></span>

    <span data-ttu-id="573f1-180">![Nuevo cuadro de diálogo de proyecto de MVC de ASP.NET 4](aspnet-mvc-4-fundamentals/_static/image3.png "nuevo cuadro de diálogo de proyecto de MVC de ASP.NET 4")</span><span class="sxs-lookup"><span data-stu-id="573f1-180">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="573f1-181">*Nuevo cuadro de diálogo de proyecto de MVC de ASP.NET 4*</span><span class="sxs-lookup"><span data-stu-id="573f1-181">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="573f1-182">Tarea 2: explorar la estructura de la solución</span><span class="sxs-lookup"><span data-stu-id="573f1-182">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="573f1-183">El marco de MVC de ASP.NET incluye una plantilla de proyecto de Visual Studio que le ayuda a crear aplicaciones Web que admiten el modelo de MVC.</span><span class="sxs-lookup"><span data-stu-id="573f1-183">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="573f1-184">Esta plantilla crea una nueva aplicación Web de MVC de ASP.NET con las carpetas necesarias, plantillas de elementos y entradas del archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="573f1-184">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="573f1-185">En esta tarea, examinará la estructura de la solución para comprender los elementos que están implicados y sus relaciones.</span><span class="sxs-lookup"><span data-stu-id="573f1-185">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="573f1-186">Las siguientes carpetas se incluyen en la aplicación de MVC de ASP.NET porque usa el marco de MVC de ASP.NET de forma predeterminada un &quot;Convención sobre configuración&quot; enfoque y hace algunas suposiciones predeterminado se basa en nombres de carpeta convenciones.</span><span class="sxs-lookup"><span data-stu-id="573f1-186">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="573f1-187">Una vez creado el proyecto, revise la estructura de carpetas que se ha creado en el Explorador de soluciones en el lado derecho:</span><span class="sxs-lookup"><span data-stu-id="573f1-187">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="573f1-188">![Estructura de carpetas de MVC de ASP.NET en el Explorador de soluciones](aspnet-mvc-4-fundamentals/_static/image4.png "estructura de carpetas de MVC de ASP.NET en el Explorador de soluciones")</span><span class="sxs-lookup"><span data-stu-id="573f1-188">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="573f1-189">*Estructura de carpetas de MVC de ASP.NET en el Explorador de soluciones*</span><span class="sxs-lookup"><span data-stu-id="573f1-189">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

    1. <span data-ttu-id="573f1-190">**Controladores de**.</span><span class="sxs-lookup"><span data-stu-id="573f1-190">**Controllers**.</span></span> <span data-ttu-id="573f1-191">Esta carpeta contiene las clases de controlador.</span><span class="sxs-lookup"><span data-stu-id="573f1-191">This folder will contain the controller classes.</span></span> <span data-ttu-id="573f1-192">En una aplicación de MVC en función, controladores están responsables de controlar la interacción del usuario final, manipular el modelo y, en última instancia elige una vista para representar la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="573f1-192">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

        > [!NOTE]
        > <span data-ttu-id="573f1-193">El marco de MVC requiere que los nombres de todos los controladores terminen con &quot;controlador&quot;-por ejemplo, HomeController, LoginController o ProductController.</span><span class="sxs-lookup"><span data-stu-id="573f1-193">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
    2. <span data-ttu-id="573f1-194">**Modelos**.</span><span class="sxs-lookup"><span data-stu-id="573f1-194">**Models**.</span></span> <span data-ttu-id="573f1-195">Esta carpeta está disponible para las clases que representan el modelo de aplicación para la aplicación Web de MVC.</span><span class="sxs-lookup"><span data-stu-id="573f1-195">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="573f1-196">Normalmente, esto incluye código que define los objetos y la lógica para interactuar con el almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="573f1-196">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="573f1-197">Normalmente, los objetos del modelo real será en bibliotecas de clases independientes.</span><span class="sxs-lookup"><span data-stu-id="573f1-197">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="573f1-198">Sin embargo, cuando se crea una nueva aplicación, se podría incluir clases y, a continuación, muévalos a bibliotecas de clases independientes en un momento posterior del ciclo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="573f1-198">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
    3. <span data-ttu-id="573f1-199">**Vistas**.</span><span class="sxs-lookup"><span data-stu-id="573f1-199">**Views**.</span></span> <span data-ttu-id="573f1-200">Esta carpeta es la ubicación recomendada para las vistas, los componentes responsables de mostrar la interfaz de usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="573f1-200">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="573f1-201">Vistas usan archivos .aspx, .ascx, .cshtml y. master, además de otros archivos relacionados con la representación de vistas.</span><span class="sxs-lookup"><span data-stu-id="573f1-201">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="573f1-202">Carpeta Views contiene una carpeta para cada controlador; la carpeta se denomina con el prefijo del nombre del controlador.</span><span class="sxs-lookup"><span data-stu-id="573f1-202">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="573f1-203">Por ejemplo, si tiene un controlador denominado **HomeController**, la carpeta Views contiene una carpeta denominada Home.</span><span class="sxs-lookup"><span data-stu-id="573f1-203">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="573f1-204">De forma predeterminada, cuando el marco de MVC de ASP.NET carga una vista, busca un archivo .aspx con el nombre de la vista solicitada en la carpeta Views\controllerName (**vistas [ControllerName] [acción] .aspx**) o (**vistas [ControllerName] [Acción] .cshtml**) para las vistas de Razor.</span><span class="sxs-lookup"><span data-stu-id="573f1-204">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

    > [!NOTE]
    > <span data-ttu-id="573f1-205">Además de las carpetas indicadas anteriormente, una aplicación Web de MVC usa el **Global.asax** valores predeterminados del archivo para establecer el enrutamiento global de direcciones URL, y utiliza el **Web.config** archivo para configurar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="573f1-205">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="573f1-206">Tarea 3: agregar un HomeController</span><span class="sxs-lookup"><span data-stu-id="573f1-206">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="573f1-207">En las aplicaciones ASP.NET que no utilizan el marco de MVC, interacción del usuario se organiza alrededor de las páginas y provocar y controlar eventos desde esas páginas.</span><span class="sxs-lookup"><span data-stu-id="573f1-207">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="573f1-208">En cambio, la interacción del usuario con aplicaciones ASP.NET MVC está organizada según controladores y sus métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="573f1-208">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="573f1-209">Por otro lado, el marco de MVC de ASP.NET asigna las direcciones URL a las clases que se denominan controladores.</span><span class="sxs-lookup"><span data-stu-id="573f1-209">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="573f1-210">Controladores de procesar las solicitudes entrantes, controlar proporcionados por el usuario y las interacciones, ejecutar lógica de aplicación adecuada y determinar la respuesta para enviar al cliente (Mostrar HTML, descargar un archivo, redirija a una dirección URL diferente, etcetera).</span><span class="sxs-lookup"><span data-stu-id="573f1-210">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="573f1-211">En el caso de mostrar HTML, una clase de controlador llama normalmente a un componente de vista independiente para generar el marcado HTML para la solicitud.</span><span class="sxs-lookup"><span data-stu-id="573f1-211">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="573f1-212">En una aplicación de MVC, la vista solo muestra información; el controlador controla y responde a la interacción y los datos que introducen los usuarios.</span><span class="sxs-lookup"><span data-stu-id="573f1-212">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="573f1-213">En esta tarea, agregará una clase de controlador que controlará las direcciones URL a la página principal del sitio de la tienda de música.</span><span class="sxs-lookup"><span data-stu-id="573f1-213">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="573f1-214">Haga clic en **controladores** carpeta en el Explorador de soluciones, seleccione **agregar** y, a continuación, **controlador** comando:</span><span class="sxs-lookup"><span data-stu-id="573f1-214">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="573f1-215">![Agregar un comando controlador](aspnet-mvc-4-fundamentals/_static/image5.png "agregar un comando de controlador")</span><span class="sxs-lookup"><span data-stu-id="573f1-215">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="573f1-216">*Agregar controlador (comando)*</span><span class="sxs-lookup"><span data-stu-id="573f1-216">*Add Controller Command*</span></span>
2. <span data-ttu-id="573f1-217">El **Agregar controlador** aparece el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="573f1-217">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="573f1-218">Nombre del controlador *HomeController* y presione **agregar**.</span><span class="sxs-lookup"><span data-stu-id="573f1-218">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="573f1-219">![Agregar cuadro de diálogo controlador](aspnet-mvc-4-fundamentals/_static/image6.png "Agregar cuadro de diálogo de controlador")</span><span class="sxs-lookup"><span data-stu-id="573f1-219">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="573f1-220">*Agregar cuadro de diálogo de controlador*</span><span class="sxs-lookup"><span data-stu-id="573f1-220">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="573f1-221">El archivo **HomeController.cs** se crea en el **controladores** carpeta.</span><span class="sxs-lookup"><span data-stu-id="573f1-221">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="573f1-222">Para tener la **HomeController** devuelven una cadena en su acción del índice, reemplace la **índice** método por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="573f1-222">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="573f1-223">(Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex1 HomeController índice*)</span><span class="sxs-lookup"><span data-stu-id="573f1-223">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="573f1-224">Tarea 4: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="573f1-224">Task 4 - Running the Application</span></span>

<span data-ttu-id="573f1-225">En esta tarea, se pruebe la aplicación en un explorador web.</span><span class="sxs-lookup"><span data-stu-id="573f1-225">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="573f1-226">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="573f1-226">Press **F5** to run the Application.</span></span> <span data-ttu-id="573f1-227">El proyecto se compila y servidor Web de IIS local se inicia.</span><span class="sxs-lookup"><span data-stu-id="573f1-227">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="573f1-228">El equipo local, servidor Web de IIS se abrirá automáticamente un explorador web que apunte a la dirección URL del servidor Web.</span><span class="sxs-lookup"><span data-stu-id="573f1-228">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="573f1-229">![Aplicación que se ejecuta en un explorador web](aspnet-mvc-4-fundamentals/_static/image7.png "aplicación que se ejecuta en un explorador web")</span><span class="sxs-lookup"><span data-stu-id="573f1-229">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="573f1-230">*Aplicación que se ejecuta en un explorador web*</span><span class="sxs-lookup"><span data-stu-id="573f1-230">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="573f1-231">El servidor Web de IIS local se ejecutará el sitio Web en un número de puerto libre aleatorio.</span><span class="sxs-lookup"><span data-stu-id="573f1-231">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="573f1-232">En la ilustración anterior, el sitio se ejecuta en `http://localhost:50103/`, por lo que está utilizando el puerto 50103.</span><span class="sxs-lookup"><span data-stu-id="573f1-232">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="573f1-233">El número de puerto puede variar.</span><span class="sxs-lookup"><span data-stu-id="573f1-233">Your port number may vary.</span></span>
2. <span data-ttu-id="573f1-234">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="573f1-234">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="573f1-235">Ejercicio 2: Creación de un controlador</span><span class="sxs-lookup"><span data-stu-id="573f1-235">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="573f1-236">En este ejercicio, obtendrá información sobre cómo actualizar el controlador para implementar funcionalidad simple de la aplicación de tienda de música.</span><span class="sxs-lookup"><span data-stu-id="573f1-236">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="573f1-237">Ese controlador definirá métodos de acción para administrar cada una de las solicitudes específicas siguientes:</span><span class="sxs-lookup"><span data-stu-id="573f1-237">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="573f1-238">Una página de lista de los géneros de música en la tienda de música</span><span class="sxs-lookup"><span data-stu-id="573f1-238">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="573f1-239">Examinar página que enumera todos los álbumes de música de un género determinado</span><span class="sxs-lookup"><span data-stu-id="573f1-239">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="573f1-240">Una página de detalles que muestra información sobre un álbum de música específico</span><span class="sxs-lookup"><span data-stu-id="573f1-240">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="573f1-241">Para el ámbito de este ejercicio, esas acciones simplemente devolverá una cadena de hasta ahora.</span><span class="sxs-lookup"><span data-stu-id="573f1-241">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="573f1-242">Tarea 1: agregar una nueva clase de StoreController</span><span class="sxs-lookup"><span data-stu-id="573f1-242">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="573f1-243">En esta tarea, agregará un nuevo controlador.</span><span class="sxs-lookup"><span data-stu-id="573f1-243">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="573f1-244">Si no está abierto, inicie **VS Express para Web 2012**.</span><span class="sxs-lookup"><span data-stu-id="573f1-244">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="573f1-245">En el **archivo** menú, elija **Abrir proyecto**.</span><span class="sxs-lookup"><span data-stu-id="573f1-245">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="573f1-246">En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex02 CreatingAController\Begin**, seleccione **Begin.sln** y haga clic en **abiertos**.</span><span class="sxs-lookup"><span data-stu-id="573f1-246">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="573f1-247">Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="573f1-247">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="573f1-248">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="573f1-248">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="573f1-249">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="573f1-249">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="573f1-250">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="573f1-250">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="573f1-251">Por último, compile la solución haciendo clic en **generar** | **generar solución**.</span><span class="sxs-lookup"><span data-stu-id="573f1-251">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="573f1-252">Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="573f1-252">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="573f1-253">Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="573f1-253">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="573f1-254">Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="573f1-254">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="573f1-255">Agregue el nuevo controlador.</span><span class="sxs-lookup"><span data-stu-id="573f1-255">Add the new controller.</span></span> <span data-ttu-id="573f1-256">Para ello, haga clic en el **controladores** carpeta en el Explorador de soluciones, seleccione **agregar** y, a continuación, el **controlador** comando.</span><span class="sxs-lookup"><span data-stu-id="573f1-256">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="573f1-257">Cambiar el **nombre de controlador** a *StoreController*y haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="573f1-257">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="573f1-258">![Agregar cuadro de diálogo controlador](aspnet-mvc-4-fundamentals/_static/image8.png "Agregar cuadro de diálogo de controlador")</span><span class="sxs-lookup"><span data-stu-id="573f1-258">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="573f1-259">*Agregar cuadro de diálogo de controlador*</span><span class="sxs-lookup"><span data-stu-id="573f1-259">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="573f1-260">Tarea 2: modificar las acciones del StoreController</span><span class="sxs-lookup"><span data-stu-id="573f1-260">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="573f1-261">En esta tarea, modificará los métodos de controlador que se denominan **acciones**.</span><span class="sxs-lookup"><span data-stu-id="573f1-261">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="573f1-262">Las acciones son responsables de controlar las solicitudes de dirección URL y determinar el contenido que debe enviarse en el explorador o el usuario que invocó la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="573f1-262">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="573f1-263">El **StoreController** clase ya tiene un **índice** método.</span><span class="sxs-lookup"><span data-stu-id="573f1-263">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="573f1-264">Se usará más adelante en esta práctica para implementar la página que enumera todos los juegos de la tienda de música.</span><span class="sxs-lookup"><span data-stu-id="573f1-264">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="573f1-265">Por ahora, solo tiene que reemplazar el **índice** método por el código siguiente que devuelve una cadena &quot;Hola de Store.Index()&quot;:</span><span class="sxs-lookup"><span data-stu-id="573f1-265">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="573f1-266">(Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex2 StoreController índice*)</span><span class="sxs-lookup"><span data-stu-id="573f1-266">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="573f1-267">Agregar **examinar** y **detalles** métodos.</span><span class="sxs-lookup"><span data-stu-id="573f1-267">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="573f1-268">Para ello, agregue el código siguiente a la **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="573f1-268">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="573f1-269">(Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="573f1-269">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="573f1-270">Tarea 3: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="573f1-270">Task 3 - Running the Application</span></span>

<span data-ttu-id="573f1-271">En esta tarea, se pruebe la aplicación en un explorador web.</span><span class="sxs-lookup"><span data-stu-id="573f1-271">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="573f1-272">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="573f1-272">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="573f1-273">El proyecto se inicia en el **inicio** página.</span><span class="sxs-lookup"><span data-stu-id="573f1-273">The project starts in the **Home** page.</span></span> <span data-ttu-id="573f1-274">Cambiar la dirección URL para comprobar la implementación de cada acción.</span><span class="sxs-lookup"><span data-stu-id="573f1-274">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="573f1-275">**/ Almacenar**.</span><span class="sxs-lookup"><span data-stu-id="573f1-275">**/Store**.</span></span> <span data-ttu-id="573f1-276">Verá  **&quot;Hola de Store.Index()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="573f1-276">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="573f1-277">**/Store/Browse**.</span><span class="sxs-lookup"><span data-stu-id="573f1-277">**/Store/Browse**.</span></span> <span data-ttu-id="573f1-278">Verá  **&quot;Hola de Store.Browse()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="573f1-278">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="573f1-279">**/ / Detalles del almacén**.</span><span class="sxs-lookup"><span data-stu-id="573f1-279">**/Store/Details**.</span></span> <span data-ttu-id="573f1-280">Verá  **&quot;Hola de Store.Details()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="573f1-280">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="573f1-281">![Exploración StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "StoreBrowse de exploración")</span><span class="sxs-lookup"><span data-stu-id="573f1-281">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="573f1-282">*Exploración /Store/Browse*</span><span class="sxs-lookup"><span data-stu-id="573f1-282">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="573f1-283">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="573f1-283">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="573f1-284">Ejercicio 3: Pasar parámetros a un controlador</span><span class="sxs-lookup"><span data-stu-id="573f1-284">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="573f1-285">Hasta ahora, ha sido devolver cadenas constantes desde los controladores de.</span><span class="sxs-lookup"><span data-stu-id="573f1-285">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="573f1-286">En este ejercicio obtendrá información sobre cómo pasar parámetros a un controlador de usar la dirección URL y la cadena de consulta y, a continuación, realizar las acciones de método responder con texto en el explorador.</span><span class="sxs-lookup"><span data-stu-id="573f1-286">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="573f1-287">Tarea 1: Agregar parámetro género StoreController</span><span class="sxs-lookup"><span data-stu-id="573f1-287">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="573f1-288">En esta tarea, va a usar el **querystring** enviar parámetros a la **examinar** método de acción en el **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="573f1-288">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="573f1-289">Si no está abierto, inicie **VS Express para Web**.</span><span class="sxs-lookup"><span data-stu-id="573f1-289">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="573f1-290">En el **archivo** menú, elija **Abrir proyecto**.</span><span class="sxs-lookup"><span data-stu-id="573f1-290">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="573f1-291">En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex03 PassingParametersToAController\Begin**, seleccione **Begin.sln** y haga clic en **abiertos**.</span><span class="sxs-lookup"><span data-stu-id="573f1-291">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="573f1-292">Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="573f1-292">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="573f1-293">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="573f1-293">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="573f1-294">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="573f1-294">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="573f1-295">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="573f1-295">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="573f1-296">Por último, compile la solución haciendo clic en **generar** | **generar solución**.</span><span class="sxs-lookup"><span data-stu-id="573f1-296">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="573f1-297">Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="573f1-297">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="573f1-298">Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="573f1-298">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="573f1-299">Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="573f1-299">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="573f1-300">Abra **StoreController** clase.</span><span class="sxs-lookup"><span data-stu-id="573f1-300">Open **StoreController** class.</span></span> <span data-ttu-id="573f1-301">Para ello, en el **el Explorador de soluciones**, expanda la **controladores** carpeta y haga doble clic en **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="573f1-301">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="573f1-302">Cambiar el **examinar** método, agregar un parámetro de cadena para solicitar un género específico.</span><span class="sxs-lookup"><span data-stu-id="573f1-302">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="573f1-303">ASP.NET MVC automáticamente pasará cualquier cadena de consulta o parámetros con nombre de envío de formulario **género** a este método de acción cuando se invoca.</span><span class="sxs-lookup"><span data-stu-id="573f1-303">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="573f1-304">Para ello, reemplace el **examinar** método por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="573f1-304">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="573f1-305">(Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="573f1-305">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="573f1-306">¿Usa el **HttpUtility.HtmlEncode** método de utilidad que impide que los usuarios insertar Javascript en la vista con un vínculo como   **/almacén/examinar? Género =&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span><span class="sxs-lookup"><span data-stu-id="573f1-306">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
    > 
    > <span data-ttu-id="573f1-307">Para obtener más información, visite [este artículo de msdn](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span><span class="sxs-lookup"><span data-stu-id="573f1-307">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="573f1-308">Tarea 2: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="573f1-308">Task 2 - Running the Application</span></span>

<span data-ttu-id="573f1-309">En esta tarea, va a probar la aplicación en un explorador web y usar el **género** parámetro.</span><span class="sxs-lookup"><span data-stu-id="573f1-309">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="573f1-310">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="573f1-310">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="573f1-311">El proyecto se inicia en el **inicio** página.</span><span class="sxs-lookup"><span data-stu-id="573f1-311">The project starts in the **Home** page.</span></span> <span data-ttu-id="573f1-312">¿Cambiar la dirección URL de   */almacenar/examinar? Género = Disco* para comprobar que la acción recibe el parámetro de género.</span><span class="sxs-lookup"><span data-stu-id="573f1-312">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="573f1-313">![Exploración StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "exploración StoreBrowseGenre = Disco")</span><span class="sxs-lookup"><span data-stu-id="573f1-313">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="573f1-314">*¿Exploración /Store/Browse? Género = Disco*</span><span class="sxs-lookup"><span data-stu-id="573f1-314">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="573f1-315">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="573f1-315">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="573f1-316">Tarea 3: agregar un parámetro de identificador que se incrustan en la dirección URL</span><span class="sxs-lookup"><span data-stu-id="573f1-316">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="573f1-317">En esta tarea, va a usar el **URL** para pasar un **Id. de** parámetro a la **detalles** método de acción de la **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="573f1-317">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="573f1-318">Predeterminado de ASP.NET MVC convención de enrutamiento consiste en tratar el segmento de una dirección URL después del nombre de método de acción como un parámetro denominado **identificador**. Si el método de acción tiene el parámetro denominado Id, a continuación, ASP.NET MVC automáticamente pasará el segmento de dirección URL para usted como un parámetro.</span><span class="sxs-lookup"><span data-stu-id="573f1-318">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="573f1-319">En la dirección URL **almacén/detalles/5**, **identificador** se interpretará como **5**.</span><span class="sxs-lookup"><span data-stu-id="573f1-319">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="573f1-320">Cambiar el **detalles** método de la **StoreController**, agregando una **int** parámetro denominado **Id. de**. Para ello, reemplace el **detalles** método por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="573f1-320">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="573f1-321">(Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="573f1-321">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="573f1-322">Tarea 4: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="573f1-322">Task 4 - Running the Application</span></span>

<span data-ttu-id="573f1-323">En esta tarea, va a probar la aplicación en un explorador web y usar el **identificador** parámetro.</span><span class="sxs-lookup"><span data-stu-id="573f1-323">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="573f1-324">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="573f1-324">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="573f1-325">El proyecto se inicia en el **inicio** página.</span><span class="sxs-lookup"><span data-stu-id="573f1-325">The project starts in the **Home** page.</span></span> <span data-ttu-id="573f1-326">Cambiar la dirección URL de */Store/Details/5* para comprobar que la acción recibe el parámetro id.</span><span class="sxs-lookup"><span data-stu-id="573f1-326">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="573f1-327">![Exploración StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "StoreDetails5 de exploración")</span><span class="sxs-lookup"><span data-stu-id="573f1-327">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="573f1-328">*Exploración /Store/Details/5*</span><span class="sxs-lookup"><span data-stu-id="573f1-328">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="573f1-329">Ejercicio 4: Crear una vista</span><span class="sxs-lookup"><span data-stu-id="573f1-329">Exercise 4: Creating a View</span></span>

<span data-ttu-id="573f1-330">Hasta ahora ha sido devolver cadenas de acciones del controlador.</span><span class="sxs-lookup"><span data-stu-id="573f1-330">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="573f1-331">Aunque es una forma útil de conocer cómo funcionan los controladores, no es cómo se generan las aplicaciones Web reales.</span><span class="sxs-lookup"><span data-stu-id="573f1-331">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="573f1-332">Las vistas son componentes que proporcionan un enfoque más adecuado para generar HTML al explorador con el uso de archivos de plantilla.</span><span class="sxs-lookup"><span data-stu-id="573f1-332">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="573f1-333">En este ejercicio obtendrá información sobre cómo agregar una página maestra de diseño para configurar una plantilla para el contenido HTML común, una hoja de estilos para mejorar la apariencia y funcionamiento del sitio y, por último, una plantilla de vista para habilitar HomeController devolver HTML.</span><span class="sxs-lookup"><span data-stu-id="573f1-333">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a><span data-ttu-id="573f1-334">Tarea 1: modificar el archivo \_layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="573f1-334">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="573f1-335">El archivo **~/Views/Shared/\_layout.cshtml** le permite configurar una plantilla para HTML comunes utilizar en todo el sitio Web.</span><span class="sxs-lookup"><span data-stu-id="573f1-335">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="573f1-336">En esta tarea agregará una página maestra de diseño con un encabezado común con vínculos al área de página principal y el almacén.</span><span class="sxs-lookup"><span data-stu-id="573f1-336">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="573f1-337">Si no está abierto, inicie **VS Express para Web**.</span><span class="sxs-lookup"><span data-stu-id="573f1-337">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="573f1-338">En el **archivo** menú, elija **Abrir proyecto**.</span><span class="sxs-lookup"><span data-stu-id="573f1-338">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="573f1-339">En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex04 CreatingAView\Begin**, seleccione **Begin.sln** y haga clic en **abiertos**.</span><span class="sxs-lookup"><span data-stu-id="573f1-339">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="573f1-340">Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="573f1-340">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="573f1-341">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="573f1-341">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="573f1-342">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="573f1-342">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="573f1-343">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="573f1-343">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="573f1-344">Por último, compile la solución haciendo clic en **generar** | **generar solución**.</span><span class="sxs-lookup"><span data-stu-id="573f1-344">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="573f1-345">Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="573f1-345">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="573f1-346">Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="573f1-346">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="573f1-347">Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="573f1-347">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="573f1-348">El archivo  **\_layout.cshtml** contiene el diseño de contenedor HTML para todas las páginas en el sitio.</span><span class="sxs-lookup"><span data-stu-id="573f1-348">The file **\_layout.cshtml** contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="573f1-349">Incluye el  **&lt;html&gt;**  (elemento) para la respuesta HTML, así como el  **&lt;head&gt;**  y  **&lt;cuerpo&gt;**  elementos.</span><span class="sxs-lookup"><span data-stu-id="573f1-349">It includes the **&lt;html&gt;** element for the HTML response, as well as the **&lt;head&gt;** and **&lt;body&gt;** elements.</span></span> <span data-ttu-id="573f1-350">**@RenderBody()** dentro del código HTML cuerpo identificar regiones esa vista plantillas podrá rellenar con contenido dinámico.</span><span class="sxs-lookup"><span data-stu-id="573f1-350">**@RenderBody()** within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
<span data-ttu-id="573f1-351">(C#)</span><span class="sxs-lookup"><span data-stu-id="573f1-351">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="573f1-352">Agregue un encabezado común con vínculos al área de página principal y el almacén en todas las páginas en el sitio.</span><span class="sxs-lookup"><span data-stu-id="573f1-352">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="573f1-353">Para ello, agregue el código siguiente &lt;cuerpo&gt; instrucción.</span><span class="sxs-lookup"><span data-stu-id="573f1-353">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
<span data-ttu-id="573f1-354">(C#)</span><span class="sxs-lookup"><span data-stu-id="573f1-354">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="573f1-355">Incluir un elemento div para representar la sección de cuerpo de cada página.</span><span class="sxs-lookup"><span data-stu-id="573f1-355">Include a div to render the body section of each page.</span></span> <span data-ttu-id="573f1-356">Reemplace  **@RenderBody()** con el siguiente código higlighted: (C#)</span><span class="sxs-lookup"><span data-stu-id="573f1-356">Replace **@RenderBody()** with the following higlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="573f1-357">¿Sabía que?</span><span class="sxs-lookup"><span data-stu-id="573f1-357">Did you know?</span></span> <span data-ttu-id="573f1-358">Visual Studio 2012 tiene fragmentos de código que resulta fácil agregar código de uso general en HTML, archivos de código y mucho más!</span><span class="sxs-lookup"><span data-stu-id="573f1-358">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="573f1-359">Pruébelo alejar escribiendo  **&lt;div&gt;**  y presionando **ficha** dos veces para insertar una completa **div** etiqueta.</span><span class="sxs-lookup"><span data-stu-id="573f1-359">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="573f1-360">Tarea 2: agregar hojas de estilo CSS</span><span class="sxs-lookup"><span data-stu-id="573f1-360">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="573f1-361">La plantilla proyecto vacío de servidor incluye un archivo CSS muy simplificado que solo incluye estilos utilizados para mostrar mensajes de validación y formas básicas.</span><span class="sxs-lookup"><span data-stu-id="573f1-361">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="573f1-362">Usará adicionales CSS e imágenes (potencialmente proporcionadas por un diseñador) con el fin de mejorar la apariencia y funcionamiento del sitio.</span><span class="sxs-lookup"><span data-stu-id="573f1-362">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="573f1-363">En esta tarea, agregará una hoja de estilos CSS para definir los estilos del sitio.</span><span class="sxs-lookup"><span data-stu-id="573f1-363">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="573f1-364">El archivo CSS y las imágenes se incluyen en el **Source\Assets\Content** carpeta de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="573f1-364">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="573f1-365">Con el fin de agregarlos a la aplicación, arrastre su contenido desde un **el Explorador de Windows** ventana en el **el Explorador de soluciones** en Visual Studio Express para Web, tal y como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="573f1-365">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="573f1-366">![Arrastre el contenido de estilo](aspnet-mvc-4-fundamentals/_static/image12.png "arrastrando contenido de estilo")</span><span class="sxs-lookup"><span data-stu-id="573f1-366">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="573f1-367">*Arrastre el contenido de estilo*</span><span class="sxs-lookup"><span data-stu-id="573f1-367">*Dragging style contents*</span></span>
2. <span data-ttu-id="573f1-368">Un cuadro de diálogo de advertencia aparecerá, le pide confirmación reemplazar **Site.css** archivo y algunas imágenes existentes.</span><span class="sxs-lookup"><span data-stu-id="573f1-368">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="573f1-369">Comprobar **aplicar a todos los elementos** y haga clic en **Sí**.</span><span class="sxs-lookup"><span data-stu-id="573f1-369">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="573f1-370">Tarea 3: agregar una plantilla de vista</span><span class="sxs-lookup"><span data-stu-id="573f1-370">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="573f1-371">En esta tarea, agregará una plantilla de vista para generar la respuesta HTML que se va a usar la página maestra de diseño y CSS que se agregan en este ejercicio.</span><span class="sxs-lookup"><span data-stu-id="573f1-371">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="573f1-372">Para usar una plantilla de vista al examinar la página principal del sitio, primero deberá indicar que en lugar de devolver una cadena, la **HomeController índice** método devolverá un **vista**.</span><span class="sxs-lookup"><span data-stu-id="573f1-372">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="573f1-373">Abra **HomeController** clase y cambiar su **índice** método para devolver un **ActionResult**, y que devuelva **View()**.</span><span class="sxs-lookup"><span data-stu-id="573f1-373">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="573f1-374">(Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex4 HomeController índice*)</span><span class="sxs-lookup"><span data-stu-id="573f1-374">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="573f1-375">Ahora, debe agregar una plantilla de vista adecuada.</span><span class="sxs-lookup"><span data-stu-id="573f1-375">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="573f1-376">Para ello, **haga** dentro de la **índice** método de acción y seleccione **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="573f1-376">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="573f1-377">Esto le llevará a la **agregar vista** cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="573f1-377">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="573f1-378">![Agregar una vista desde dentro del método de índice](aspnet-mvc-4-fundamentals/_static/image13.png "agregar una vista desde dentro del método de índice")</span><span class="sxs-lookup"><span data-stu-id="573f1-378">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="573f1-379">*Agregar una vista desde dentro del método de índice*</span><span class="sxs-lookup"><span data-stu-id="573f1-379">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="573f1-380">El **agregar vista** aparecerá el cuadro de diálogo para generar un archivo de plantilla de la vista.</span><span class="sxs-lookup"><span data-stu-id="573f1-380">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="573f1-381">De forma predeterminada, este cuadro de diálogo rellena previamente el nombre de la plantilla de vista para que coincida con el método de acción que va a usar.</span><span class="sxs-lookup"><span data-stu-id="573f1-381">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="573f1-382">Debido a que utilizó el **agregar vista** menú contextual en el **índice** método de acción en HomeController, el **agregar vista** cuadro de diálogo tiene un índice con el nombre de la vista predeterminada.</span><span class="sxs-lookup"><span data-stu-id="573f1-382">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="573f1-383">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="573f1-383">Click **Add**.</span></span>

    <span data-ttu-id="573f1-384">![Agregar cuadro de diálogo Vista](aspnet-mvc-4-fundamentals/_static/image14.png "diálogo Agregar vista")</span><span class="sxs-lookup"><span data-stu-id="573f1-384">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="573f1-385">*Agregar cuadro de diálogo de vista*</span><span class="sxs-lookup"><span data-stu-id="573f1-385">*Add View Dialog*</span></span>
4. <span data-ttu-id="573f1-386">Visual Studio genera un **Index.cshtml** plantilla vista dentro de la **Views\Home** carpeta y, a continuación, lo abre.</span><span class="sxs-lookup"><span data-stu-id="573f1-386">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="573f1-387">![Crea la vista de índice de inicio](aspnet-mvc-4-fundamentals/_static/image15.png "vista de índice de inicio creada")</span><span class="sxs-lookup"><span data-stu-id="573f1-387">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="573f1-388">*Vista de índice principal creada*</span><span class="sxs-lookup"><span data-stu-id="573f1-388">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="573f1-389">nombre y ubicación de la **Index.cshtml** archivo es pertinente y sigue las convenciones de nomenclatura de MVC de ASP.NET de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="573f1-389">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="573f1-390">La carpeta \Views\**inicio** coincide con el nombre del controlador (**inicio** controlador).</span><span class="sxs-lookup"><span data-stu-id="573f1-390">The folder \Views\**Home** matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="573f1-391">El nombre de la plantilla de vista (**índice**), coincide con el método de acción de controlador que se va a mostrar la vista.</span><span class="sxs-lookup"><span data-stu-id="573f1-391">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="573f1-392">De esta manera, ASP.NET MVC evita tener que especificar explícitamente el nombre o la ubicación de una plantilla de vista cuando se usa esta convención de nomenclatura para obtener una vista.</span><span class="sxs-lookup"><span data-stu-id="573f1-392">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="573f1-393">La plantilla de vista generada se basa en el  **\_layout.cshtml** anteriormente definida por la plantilla.</span><span class="sxs-lookup"><span data-stu-id="573f1-393">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="573f1-394">Actualizar la propiedad ViewBag.Title a **inicio**y cambiar el contenido principal a **se trata de la página principal**, tal y como se muestra en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="573f1-394">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="573f1-395">Seleccione **MvcMusicStore** proyecto en el Explorador de soluciones y presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="573f1-395">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="573f1-396">Tarea 4: comprobación</span><span class="sxs-lookup"><span data-stu-id="573f1-396">Task 4: Verification</span></span>

<span data-ttu-id="573f1-397">Para comprobar que ha realizado correctamente todos los pasos en el ejercicio anterior, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="573f1-397">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="573f1-398">Con la aplicación abierta en un explorador, debe tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="573f1-398">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="573f1-399">Método de acción de índice del HomeController encuentra y muestra el **\Views\Home\Index.cshtml** ver plantilla, aunque el código llame **devolver View()**, porque la plantilla de vista seguido el convención de nomenclatura estándar.</span><span class="sxs-lookup"><span data-stu-id="573f1-399">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="573f1-400">La página principal muestra el mensaje de bienvenida definido dentro de la **\Views\Home\Index.cshtml** plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="573f1-400">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="573f1-401">Está usando la página principal de la  **\_layout.cshtml** plantilla, de modo que el mensaje de bienvenida se encuentra en el diseño de sitio estándar HTML.</span><span class="sxs-lookup"><span data-stu-id="573f1-401">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="573f1-402">![Vista de índice mediante el LayoutPage definido y el estilo de inicio](aspnet-mvc-4-fundamentals/_static/image16.png "inicio la vista del índice mediante el LayoutPage y el estilo definido")</span><span class="sxs-lookup"><span data-stu-id="573f1-402">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="573f1-403">*Vista de índice principal mediante el LayoutPage y el estilo definido*</span><span class="sxs-lookup"><span data-stu-id="573f1-403">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="573f1-404">Ejercicio 5: Crear un modelo de vista</span><span class="sxs-lookup"><span data-stu-id="573f1-404">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="573f1-405">Hasta ahora, realizado las vistas Mostrar codificado de forma rígida HTML, pero, para crear aplicaciones web dinámicas, la plantilla de vista debe recibir información desde el controlador.</span><span class="sxs-lookup"><span data-stu-id="573f1-405">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="573f1-406">Una técnica común que se usará para ese fin es el **ViewModel** patrón, que permite que un controlador en un paquete toda la información necesaria para generar la respuesta HTML adecuada.</span><span class="sxs-lookup"><span data-stu-id="573f1-406">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="573f1-407">En este ejercicio, obtendrá información sobre cómo crear una clase ViewModel y agregar las propiedades necesarias: el número de géneros en el almacén y una lista de esas géneros.</span><span class="sxs-lookup"><span data-stu-id="573f1-407">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="573f1-408">También se actualizará el StoreController para usar el modelo de vista creada y, por último, creará una nueva plantilla de vista que se mostrará las propiedades mencionadas en la página.</span><span class="sxs-lookup"><span data-stu-id="573f1-408">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="573f1-409">Tarea 1: crear una clase ViewModel</span><span class="sxs-lookup"><span data-stu-id="573f1-409">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="573f1-410">En esta tarea, creará una clase ViewModel que va a implementar el escenario de lista de género de almacén.</span><span class="sxs-lookup"><span data-stu-id="573f1-410">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="573f1-411">Si no está abierto, inicie **VS Express para Web**.</span><span class="sxs-lookup"><span data-stu-id="573f1-411">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="573f1-412">En el **archivo** menú, elija **Abrir proyecto**.</span><span class="sxs-lookup"><span data-stu-id="573f1-412">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="573f1-413">En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex05 CreatingAViewModel\Begin**, seleccione **Begin.sln** y haga clic en **abiertos**.</span><span class="sxs-lookup"><span data-stu-id="573f1-413">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="573f1-414">Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="573f1-414">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="573f1-415">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="573f1-415">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="573f1-416">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="573f1-416">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="573f1-417">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="573f1-417">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="573f1-418">Por último, compile la solución haciendo clic en **generar** | **generar solución**.</span><span class="sxs-lookup"><span data-stu-id="573f1-418">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="573f1-419">Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="573f1-419">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="573f1-420">Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="573f1-420">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="573f1-421">Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="573f1-421">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="573f1-422">Crear un **ViewModels** carpeta que contendrá el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="573f1-422">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="573f1-423">Para ello, haga clic en el nivel superior **MvcMusicStore** proyecto, seleccione **agregar** y, a continuación, **nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="573f1-423">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="573f1-424">![Agregar una nueva carpeta](aspnet-mvc-4-fundamentals/_static/image17.png "agregar una nueva carpeta")</span><span class="sxs-lookup"><span data-stu-id="573f1-424">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="573f1-425">*Agregar una nueva carpeta*</span><span class="sxs-lookup"><span data-stu-id="573f1-425">*Adding a new folder*</span></span>
4. <span data-ttu-id="573f1-426">Nombre de la carpeta *ViewModels*.</span><span class="sxs-lookup"><span data-stu-id="573f1-426">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="573f1-427">![Carpeta ViewModels en el Explorador de soluciones](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels carpeta en el Explorador de soluciones")</span><span class="sxs-lookup"><span data-stu-id="573f1-427">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="573f1-428">*Carpeta ViewModels en el Explorador de soluciones*</span><span class="sxs-lookup"><span data-stu-id="573f1-428">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="573f1-429">Crear un **ViewModel** clase.</span><span class="sxs-lookup"><span data-stu-id="573f1-429">Create a **ViewModel** class.</span></span> <span data-ttu-id="573f1-430">Para ello, haga doble clic en el **ViewModels** carpeta recientemente creado, seleccione **agregar** y, a continuación, **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="573f1-430">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="573f1-431">En **código**, elija la **clase** de elemento y un nombre al archivo *StoreIndexViewModel.cs*, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="573f1-431">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="573f1-432">![Agregar una nueva clase](aspnet-mvc-4-fundamentals/_static/image19.png "agregar una nueva clase")</span><span class="sxs-lookup"><span data-stu-id="573f1-432">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="573f1-433">*Agregar una nueva clase*</span><span class="sxs-lookup"><span data-stu-id="573f1-433">*Adding a new Class*</span></span>

    <span data-ttu-id="573f1-434">![Crear clase de StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel de creación de clase")</span><span class="sxs-lookup"><span data-stu-id="573f1-434">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="573f1-435">*Crear clase de StoreIndexViewModel*</span><span class="sxs-lookup"><span data-stu-id="573f1-435">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="573f1-436">Tarea 2: agregar propiedades a la clase ViewModel</span><span class="sxs-lookup"><span data-stu-id="573f1-436">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="573f1-437">Hay dos parámetros que se pasan desde el StoreController a la plantilla de vista con el fin de generar la respuesta esperada de HTML: el número de géneros en el almacén y una lista de esas géneros.</span><span class="sxs-lookup"><span data-stu-id="573f1-437">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="573f1-438">En esta tarea, agregará esas 2 propiedades la **StoreIndexViewModel** clase: **NumberOfGenres** (un entero) y **géneros** (una lista de cadenas).</span><span class="sxs-lookup"><span data-stu-id="573f1-438">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="573f1-439">Agregar **NumberOfGenres** y **géneros** propiedades para la **StoreIndexViewModel** clase.</span><span class="sxs-lookup"><span data-stu-id="573f1-439">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="573f1-440">Para ello, agregue las siguientes líneas de 2 a la definición de clase:</span><span class="sxs-lookup"><span data-stu-id="573f1-440">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="573f1-441">(Código de fragmento de código: *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel propiedades*)</span><span class="sxs-lookup"><span data-stu-id="573f1-441">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="573f1-442">El **{get; estableció;}**  notación hace uso de C# la característica de propiedades autoimplementadas.</span><span class="sxs-lookup"><span data-stu-id="573f1-442">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="573f1-443">Proporciona las ventajas de una propiedad sin necesidad de nosotros declarar un campo de respaldo.</span><span class="sxs-lookup"><span data-stu-id="573f1-443">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="573f1-444">Tarea 3: actualización StoreController utilizar el StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="573f1-444">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="573f1-445">El **StoreIndexViewModel** clase encapsula la información necesaria para pasar de **StoreController**del **índice** método a una plantilla de vista con el fin de generar una respuesta .</span><span class="sxs-lookup"><span data-stu-id="573f1-445">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="573f1-446">En esta tarea, actualizará la **StoreController** para usar el **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="573f1-446">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="573f1-447">Abra **StoreController** clase.</span><span class="sxs-lookup"><span data-stu-id="573f1-447">Open **StoreController** class.</span></span>

    <span data-ttu-id="573f1-448">![Abriendo la clase StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "StoreController de apertura (clase)")</span><span class="sxs-lookup"><span data-stu-id="573f1-448">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="573f1-449">*Clase de StoreController de apertura*</span><span class="sxs-lookup"><span data-stu-id="573f1-449">*Opening StoreController class*</span></span>
2. <span data-ttu-id="573f1-450">Para poder usar el **StoreIndexViewModel** clase desde el **StoreController**, agregue el siguiente espacio de nombres en la parte superior de la **StoreController** código:</span><span class="sxs-lookup"><span data-stu-id="573f1-450">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="573f1-451">(Código de fragmento de código: *ASP.NET MVC 4 Fundamentals - StoreIndexViewModel Ex5 mediante ViewModels*)</span><span class="sxs-lookup"><span data-stu-id="573f1-451">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="573f1-452">Cambiar el **StoreController**del **índice** método de acción para que crea y rellena una **StoreIndexViewModel** objeto y, a continuación, pasa a una plantilla de vista para generar una respuesta HTML con él.</span><span class="sxs-lookup"><span data-stu-id="573f1-452">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="573f1-453">En el laboratorio &quot;acceso a datos y modelos de ASP.NET MVC&quot; escribirá el código que recupera la lista de géneros de almacén de una base de datos.</span><span class="sxs-lookup"><span data-stu-id="573f1-453">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="573f1-454">En el código siguiente, creará un **lista** de géneros datos ficticios que rellenarán el **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="573f1-454">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="573f1-455">Después de crear y configurar la **StoreIndexViewModel** de objeto, se pasarán como argumento a la **vista** método.</span><span class="sxs-lookup"><span data-stu-id="573f1-455">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="573f1-456">Esto indica que la plantilla de vista usará ese objeto para generar una respuesta HTML con él.</span><span class="sxs-lookup"><span data-stu-id="573f1-456">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="573f1-457">Reemplace el **índice** método por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="573f1-457">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="573f1-458">(Código de fragmento de código: *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index (método)*)</span><span class="sxs-lookup"><span data-stu-id="573f1-458">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="573f1-459">Si no está familiarizado con C#, puede suponer que el uso **var** significa que la **viewModel** variable está enlazada a un tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="573f1-459">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="573f1-460">No es correcto: el compilador de C# utiliza la inferencia de tipos en función de lo que se asigna a la variable para determinar que **viewModel** es de tipo **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="573f1-460">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="573f1-461">Además, si compila local **viewModel** variable como un **StoreIndexViewModel** escriba get comprobación en tiempo de compilación y soporte técnico del editor de código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="573f1-461">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="573f1-462">Tarea 4: crear una plantilla de vista que usa StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="573f1-462">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="573f1-463">En esta tarea, creará una plantilla de vista que usará un objeto StoreIndexViewModel pasado desde el controlador para mostrar una lista de géneros.</span><span class="sxs-lookup"><span data-stu-id="573f1-463">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="573f1-464">Antes de crear la nueva plantilla de vista, vamos a compilar el proyecto para que la **Agregar cuadro de diálogo de vista** conoce la **StoreIndexViewModel** clase.</span><span class="sxs-lookup"><span data-stu-id="573f1-464">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="573f1-465">Compile el proyecto seleccionando la **generar** elemento de menú y, a continuación, **MvcMusicStore generar**.</span><span class="sxs-lookup"><span data-stu-id="573f1-465">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="573f1-466">![Compilar el proyecto](aspnet-mvc-4-fundamentals/_static/image22.png "compilar el proyecto")</span><span class="sxs-lookup"><span data-stu-id="573f1-466">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="573f1-467">*Compilar el proyecto*</span><span class="sxs-lookup"><span data-stu-id="573f1-467">*Building the project*</span></span>
2. <span data-ttu-id="573f1-468">Crear una nueva plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="573f1-468">Create a new View template.</span></span> <span data-ttu-id="573f1-469">Para ello, pulse el botón derecho dentro de la **índice** método y seleccione **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="573f1-469">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="573f1-470">![Agregar una vista](aspnet-mvc-4-fundamentals/_static/image23.png "agregar una vista")</span><span class="sxs-lookup"><span data-stu-id="573f1-470">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="573f1-471">*Agregar una vista*</span><span class="sxs-lookup"><span data-stu-id="573f1-471">*Adding a View*</span></span>
3. <span data-ttu-id="573f1-472">Dado que la **Agregar cuadro de diálogo de vista** se invoca desde la **StoreController**, agregará la plantilla de vista de forma predeterminada en un **\Views\Store\Index.cshtml** archivo.</span><span class="sxs-lookup"><span data-stu-id="573f1-472">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="573f1-473">Compruebe el **crear una fuertemente tipados-vista** casilla de verificación y, a continuación, seleccione **StoreIndexViewModel** como el **clase modelo**.</span><span class="sxs-lookup"><span data-stu-id="573f1-473">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="573f1-474">Además, asegúrese de que el motor de vista seleccionado es **Razor**.</span><span class="sxs-lookup"><span data-stu-id="573f1-474">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="573f1-475">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="573f1-475">Click **Add**.</span></span>

    <span data-ttu-id="573f1-476">![Agregar cuadro de diálogo Vista](aspnet-mvc-4-fundamentals/_static/image24.png "diálogo Agregar vista")</span><span class="sxs-lookup"><span data-stu-id="573f1-476">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="573f1-477">*Agregar cuadro de diálogo de vista*</span><span class="sxs-lookup"><span data-stu-id="573f1-477">*Add View Dialog*</span></span>

    <span data-ttu-id="573f1-478">El **\Views\Store\Index.cshtml** ver el archivo de plantilla se crea y se abre.</span><span class="sxs-lookup"><span data-stu-id="573f1-478">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="573f1-479">En función de la información proporcionada para el **agregar vista** cuadro de diálogo en el último paso, la vista plantilla esperará un **StoreIndexViewModel** instancia como los datos que se va a usar para generar una respuesta HTML.</span><span class="sxs-lookup"><span data-stu-id="573f1-479">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="573f1-480">Observará que la plantilla hereda un `ViewPage<musicstore.viewmodels.storeindexviewmodel>` en C#.</span><span class="sxs-lookup"><span data-stu-id="573f1-480">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="573f1-481">Tarea 5: actualizar la plantilla de vista</span><span class="sxs-lookup"><span data-stu-id="573f1-481">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="573f1-482">En esta tarea, actualizará la plantilla de vista creada en la última tarea para recuperar el número de géneros y sus nombres en la página.</span><span class="sxs-lookup"><span data-stu-id="573f1-482">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="573f1-483">Va a usar sintaxis @ (suele denominarse &quot;fragmentos de código&quot;) para ejecutar el código dentro de la plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="573f1-483">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>


1. <span data-ttu-id="573f1-484">En el **Index.cshtml** de archivos, en la **almacén** carpeta, reemplace el código con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="573f1-484">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="573f1-485">En cuanto termine de escribir el período después de la palabra **modelo**, Intellisense de Visual Studio mostrará una lista de posibles propiedades y métodos que puede elegir.</span><span class="sxs-lookup"><span data-stu-id="573f1-485">As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.</span></span>
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > <span data-ttu-id="573f1-486">*Obtener propiedades del modelo y los métodos con IntelliSense de Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="573f1-486">*Getting Model properties and methods with Visual Studio's IntelliSense*</span></span>
    > 
    > <span data-ttu-id="573f1-487">El **modelo** referencias de propiedad el **StoreIndexViewModel** objeto de que el controlador pasa a la plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="573f1-487">The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template.</span></span> <span data-ttu-id="573f1-488">Esto significa que puede tener acceso a todos los datos pasados desde el controlador a la plantilla de vista a través de la **modelo** propiedad y dele formato en una respuesta adecuada de HTML dentro de la plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="573f1-488">This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.</span></span>
    > 
    > <span data-ttu-id="573f1-489">Basta con seleccionar el **NumberOfGenres** lista de propiedades de las características de Intellisense en lugar de escribir en y, a continuación, se le Autocompletar, presionando la **tecla tab**.</span><span class="sxs-lookup"><span data-stu-id="573f1-489">You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.</span></span>
2. <span data-ttu-id="573f1-490">Recorrer la lista género en **StoreIndexViewModel** y crear un elemento HTML  **&lt;ul&gt;**  lista mediante un **foreach** bucle.</span><span class="sxs-lookup"><span data-stu-id="573f1-490">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
<span data-ttu-id="573f1-491">(C#)</span><span class="sxs-lookup"><span data-stu-id="573f1-491">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="573f1-492">Presione **F5** para ejecutar la aplicación y examinar **/almacén**.</span><span class="sxs-lookup"><span data-stu-id="573f1-492">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="573f1-493">Verá la lista de géneros pasa el **StoreIndexViewModel** objeto desde el **StoreController** a la plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="573f1-493">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="573f1-494">![Vista que muestra una lista de géneros](aspnet-mvc-4-fundamentals/_static/image26.png "vista muestra una lista de géneros")</span><span class="sxs-lookup"><span data-stu-id="573f1-494">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="573f1-495">*Vista que muestra una lista de géneros*</span><span class="sxs-lookup"><span data-stu-id="573f1-495">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="573f1-496">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="573f1-496">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="573f1-497">Ejercicio 6: Usar parámetros en la vista</span><span class="sxs-lookup"><span data-stu-id="573f1-497">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="573f1-498">En el ejercicio 3 aprendido cómo pasar parámetros al controlador.</span><span class="sxs-lookup"><span data-stu-id="573f1-498">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="573f1-499">En este ejercicio, aprenderá a usar esos parámetros en la plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="573f1-499">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="573f1-500">A tal efecto, se introducirán en primer lugar para las clases de modelo que le ayudará a administrar su lógica de datos y el dominio.</span><span class="sxs-lookup"><span data-stu-id="573f1-500">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="573f1-501">Además, obtendrá información sobre cómo crear vínculos a páginas dentro de la aplicación de MVC de ASP.NET sin preocuparse de cosas como las rutas de acceso de dirección URL de codificación.</span><span class="sxs-lookup"><span data-stu-id="573f1-501">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="573f1-502">Tarea 1: agregar clases de modelo</span><span class="sxs-lookup"><span data-stu-id="573f1-502">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="573f1-503">A diferencia de ViewModels, que se crean exclusivamente para devolver información del controlador a la vista, se generan clases de modelo para contener y administrar la lógica de datos y el dominio.</span><span class="sxs-lookup"><span data-stu-id="573f1-503">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="573f1-504">En esta tarea agregará dos clases de modelo para representar estos conceptos: **género** y **álbum**.</span><span class="sxs-lookup"><span data-stu-id="573f1-504">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="573f1-505">Si no está abierto, inicie **VS Express para Web**</span><span class="sxs-lookup"><span data-stu-id="573f1-505">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="573f1-506">En el **archivo** menú, elija **Abrir proyecto**.</span><span class="sxs-lookup"><span data-stu-id="573f1-506">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="573f1-507">En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex06 UsingParametersInView\Begin**, seleccione **Begin.sln** y haga clic en **abiertos**.</span><span class="sxs-lookup"><span data-stu-id="573f1-507">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="573f1-508">Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="573f1-508">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="573f1-509">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="573f1-509">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="573f1-510">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="573f1-510">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="573f1-511">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="573f1-511">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="573f1-512">Por último, compile la solución haciendo clic en **generar** | **generar solución**.</span><span class="sxs-lookup"><span data-stu-id="573f1-512">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="573f1-513">Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="573f1-513">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="573f1-514">Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="573f1-514">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="573f1-515">Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="573f1-515">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="573f1-516">Agregar un **género** clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="573f1-516">Add a **Genre** Model class.</span></span> <span data-ttu-id="573f1-517">Para ello, haga clic en el **modelos** carpeta en el **el Explorador de soluciones**, seleccione **agregar** y, a continuación, el **nuevo elemento** opción.</span><span class="sxs-lookup"><span data-stu-id="573f1-517">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="573f1-518">En **código**, elija la **clase** de elemento y un nombre al archivo *Genre.cs*, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="573f1-518">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="573f1-519">![Agregar una clase](aspnet-mvc-4-fundamentals/_static/image27.png "agregar una clase")</span><span class="sxs-lookup"><span data-stu-id="573f1-519">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="573f1-520">*Agregar un nuevo elemento*</span><span class="sxs-lookup"><span data-stu-id="573f1-520">*Adding a new item*</span></span>

    <span data-ttu-id="573f1-521">![Agregar clase de modelo de género](aspnet-mvc-4-fundamentals/_static/image28.png "Agregar clase de modelo de género")</span><span class="sxs-lookup"><span data-stu-id="573f1-521">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="573f1-522">*Agregar clase de modelo de género*</span><span class="sxs-lookup"><span data-stu-id="573f1-522">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="573f1-523">Agregar un **nombre** propiedad a la clase de género.</span><span class="sxs-lookup"><span data-stu-id="573f1-523">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="573f1-524">Para ello, agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="573f1-524">To do this, add the following code:</span></span>

    <span data-ttu-id="573f1-525">(Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex6 género*)</span><span class="sxs-lookup"><span data-stu-id="573f1-525">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="573f1-526">Siguiendo el mismo procedimiento que antes, agregue un **álbum** clase.</span><span class="sxs-lookup"><span data-stu-id="573f1-526">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="573f1-527">Para ello, haga clic en el **modelos** carpeta en el **el Explorador de soluciones**, seleccione **agregar** y, a continuación, el **nuevo elemento** opción.</span><span class="sxs-lookup"><span data-stu-id="573f1-527">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="573f1-528">En **código**, elija la **clase** de elemento y un nombre al archivo *Album.cs*, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="573f1-528">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="573f1-529">Agregue dos propiedades a la clase de álbum: **género** y **título**.</span><span class="sxs-lookup"><span data-stu-id="573f1-529">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="573f1-530">Para ello, agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="573f1-530">To do this, add the following code:</span></span>

    <span data-ttu-id="573f1-531">(Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - álbum Ex6*)</span><span class="sxs-lookup"><span data-stu-id="573f1-531">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="573f1-532">Tarea 2: agregar un StoreBrowseViewModel</span><span class="sxs-lookup"><span data-stu-id="573f1-532">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="573f1-533">A **StoreBrowseViewModel** se utilizarán en esta tarea para mostrar los álbumes que coinciden con un género seleccionado.</span><span class="sxs-lookup"><span data-stu-id="573f1-533">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="573f1-534">En esta tarea, creará esta clase y, a continuación, agregue dos propiedades para controlar la **género** y su **álbum**de la lista.</span><span class="sxs-lookup"><span data-stu-id="573f1-534">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="573f1-535">Agregar un **StoreBrowseViewModel** clase.</span><span class="sxs-lookup"><span data-stu-id="573f1-535">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="573f1-536">Para ello, haga clic en el **ViewModels** carpeta en el **el Explorador de soluciones**, seleccione **agregar** y, a continuación, el **nuevo elemento** opción.</span><span class="sxs-lookup"><span data-stu-id="573f1-536">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="573f1-537">En **código**, elija la **clase** de elemento y un nombre al archivo *StoreBrowseViewModel.cs*, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="573f1-537">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="573f1-538">Agregue una referencia a los modelos en **StoreBrowseViewModel** clase.</span><span class="sxs-lookup"><span data-stu-id="573f1-538">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="573f1-539">Para ello, agregue lo siguiente con el espacio de nombres:</span><span class="sxs-lookup"><span data-stu-id="573f1-539">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="573f1-540">(Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="573f1-540">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="573f1-541">Agregar dos propiedades que **StoreBrowseViewModel** clase: **género** y **álbumes**.</span><span class="sxs-lookup"><span data-stu-id="573f1-541">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="573f1-542">Para ello, agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="573f1-542">To do this, add the following code:</span></span>

    <span data-ttu-id="573f1-543">(Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="573f1-543">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="573f1-544">¿Qué es **lista&lt;álbum&gt;**  ?: está usando esta definición de la **lista&lt;T&gt;**  tipo, donde **T** restringe el tipo de los elementos de esta **lista** pertenecen, en este caso **álbum** (o cualquiera de sus descendientes).</span><span class="sxs-lookup"><span data-stu-id="573f1-544">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
    > 
    > <span data-ttu-id="573f1-545">Esta capacidad para diseñar clases y métodos que aplazan la especificación de uno o más tipos hasta que la clase o el método se declara y crea una instancia de un código de cliente es una característica del lenguaje C# llamado **genéricos**.</span><span class="sxs-lookup"><span data-stu-id="573f1-545">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
    > 
    > <span data-ttu-id="573f1-546">**Lista&lt;T&gt;**  es el equivalente genérico de la **ArrayList** escriba y está disponible en la **System.Collections.Generic** espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="573f1-546">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="573f1-547">Una de las ventajas de usar **genéricos** es que porque no se especifica el tipo, no es necesario ocuparse de operaciones como la conversión de los elementos en la comprobación de tipo **álbum** como lo haría con un **ArrayList**.</span><span class="sxs-lookup"><span data-stu-id="573f1-547">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="573f1-548">Tarea 3: usar el nuevo modelo de vista en el StoreController</span><span class="sxs-lookup"><span data-stu-id="573f1-548">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="573f1-549">En esta tarea, modificará el **StoreController**del **examinar** y **detalles** métodos de acción para utilizar el nuevo **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="573f1-549">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="573f1-550">Agregue una referencia a la carpeta de modelos en **StoreController** clase.</span><span class="sxs-lookup"><span data-stu-id="573f1-550">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="573f1-551">Para ello, expanda el **controladores** carpeta en el **el Explorador de soluciones** y abra el **StoreController** clase.</span><span class="sxs-lookup"><span data-stu-id="573f1-551">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="573f1-552">A continuación, agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="573f1-552">Then add the following code:</span></span>

    <span data-ttu-id="573f1-553">(Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="573f1-553">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="573f1-554">Reemplace el **examinar** método de acción para usar el **StoreViewBrowseController** clase.</span><span class="sxs-lookup"><span data-stu-id="573f1-554">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="573f1-555">Creará un género y dos nuevos objetos de álbumes con datos ficticios (en el laboratorio de prácticas siguiente consumirá datos reales de una base de datos).</span><span class="sxs-lookup"><span data-stu-id="573f1-555">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="573f1-556">Para ello, reemplace el **examinar** método por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="573f1-556">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="573f1-557">(Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="573f1-557">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="573f1-558">Reemplace el **detalles** método de acción para usar el **StoreViewBrowseController** clase.</span><span class="sxs-lookup"><span data-stu-id="573f1-558">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="573f1-559">Se creará una nueva **álbum** objeto que se devolverá a la **vista**.</span><span class="sxs-lookup"><span data-stu-id="573f1-559">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="573f1-560">Para ello, reemplace el **detalles** método por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="573f1-560">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="573f1-561">(Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="573f1-561">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="573f1-562">Tarea 4: agregar una plantilla de vista de exploración</span><span class="sxs-lookup"><span data-stu-id="573f1-562">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="573f1-563">En esta tarea, agregará un **examinar** vista para mostrar los álbumes que encuentra para un género específico.</span><span class="sxs-lookup"><span data-stu-id="573f1-563">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="573f1-564">Antes de crear la nueva plantilla de vista, debe compilar el proyecto para que la **agregar vista** diálogo conoce la **ViewModel** clase se debe utilizar.</span><span class="sxs-lookup"><span data-stu-id="573f1-564">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="573f1-565">Compile el proyecto seleccionando la **generar** elemento de menú y, a continuación, **MvcMusicStore generar**.</span><span class="sxs-lookup"><span data-stu-id="573f1-565">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="573f1-566">Agregar un **examinar** vista.</span><span class="sxs-lookup"><span data-stu-id="573f1-566">Add a **Browse** View.</span></span> <span data-ttu-id="573f1-567">Para ello, haga clic en el **examinar** método de acción de la **StoreController** y haga clic en **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="573f1-567">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="573f1-568">En el **agregar vista** diálogo cuadro, compruebe que el nombre de vista es **examinar**.</span><span class="sxs-lookup"><span data-stu-id="573f1-568">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="573f1-569">Compruebe el **crear una vista fuertemente tipada** casilla de verificación y seleccione **StoreBrowseViewModel** desde el **clase modelo** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="573f1-569">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="573f1-570">Deje los demás campos con sus valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="573f1-570">Leave the other fields with their default value.</span></span> <span data-ttu-id="573f1-571">A continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="573f1-571">Then click **Add**.</span></span>

    <span data-ttu-id="573f1-572">![Agregar una vista Examinar](aspnet-mvc-4-fundamentals/_static/image29.png "agregar una vista de exploración")</span><span class="sxs-lookup"><span data-stu-id="573f1-572">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="573f1-573">*Agregar una vista de exploración*</span><span class="sxs-lookup"><span data-stu-id="573f1-573">*Adding a Browse View*</span></span>
4. <span data-ttu-id="573f1-574">Modificar el **Browse.cshtml** para mostrar información del género, obtener acceso a la **StoreBrowseViewModel** objeto que se pasa a la plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="573f1-574">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="573f1-575">Para ello, reemplace el contenido por lo siguiente: (C#)</span><span class="sxs-lookup"><span data-stu-id="573f1-575">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="573f1-576">Tarea 5: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="573f1-576">Task 5 - Running the Application</span></span>

<span data-ttu-id="573f1-577">En esta tarea, realizará las pruebas que el **examinar** método recupera álbumes de la **examinar** acción del método.</span><span class="sxs-lookup"><span data-stu-id="573f1-577">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="573f1-578">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="573f1-578">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="573f1-579">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="573f1-579">The project starts in the Home page.</span></span> <span data-ttu-id="573f1-580">¿Cambiar la dirección URL de   **/almacenar/examinar? Género = Disco** para comprobar que la acción devuelve dos álbumes.</span><span class="sxs-lookup"><span data-stu-id="573f1-580">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="573f1-581">![Examen de Disco álbumes de almacén](aspnet-mvc-4-fundamentals/_static/image30.png "exploración álbumes de Disco de almacenamiento")</span><span class="sxs-lookup"><span data-stu-id="573f1-581">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="573f1-582">*Exploración álbumes de Disco de almacenamiento*</span><span class="sxs-lookup"><span data-stu-id="573f1-582">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="573f1-583">Tarea 6: mostrar información sobre un álbum determinado</span><span class="sxs-lookup"><span data-stu-id="573f1-583">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="573f1-584">En esta tarea, implementará el **/detalles del almacén** vista para mostrar información sobre un álbum determinado.</span><span class="sxs-lookup"><span data-stu-id="573f1-584">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="573f1-585">En este laboratorio práctico, todo lo que se va a mostrar sobre el álbum ya esté incluido en el **vista** plantilla.</span><span class="sxs-lookup"><span data-stu-id="573f1-585">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="573f1-586">Así, en lugar de crear un **StoreDetailsViewModel** (clase), se usará el actual **StoreBrowseViewModel** plantilla pasándole el álbum.</span><span class="sxs-lookup"><span data-stu-id="573f1-586">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="573f1-587">Cierre el explorador si es necesario, para volver a la ventana de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="573f1-587">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="573f1-588">Agregue un nuevo **detalles** ver para el **StoreController**del **detalles** método de acción.</span><span class="sxs-lookup"><span data-stu-id="573f1-588">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="573f1-589">Para ello, haga clic en el **detalles** método en el **StoreController** clase y haga clic en **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="573f1-589">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="573f1-590">En el **agregar vista** cuadro de diálogo, compruebe que la **nombre de la vista** es **detalles**.</span><span class="sxs-lookup"><span data-stu-id="573f1-590">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="573f1-591">Compruebe el **crear una vista fuertemente tipada** casilla de verificación y seleccione **álbum** desde el **clase modelo** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="573f1-591">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="573f1-592">Deje los demás campos con sus valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="573f1-592">Leave the other fields with their default value.</span></span> <span data-ttu-id="573f1-593">A continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="573f1-593">Then click **Add**.</span></span> <span data-ttu-id="573f1-594">Esto creará y abrirá una **\Views\Store\Details.cshtml** archivo.</span><span class="sxs-lookup"><span data-stu-id="573f1-594">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="573f1-595">![Agregar una vista de detalles](aspnet-mvc-4-fundamentals/_static/image31.png "agregar una vista de detalles")</span><span class="sxs-lookup"><span data-stu-id="573f1-595">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="573f1-596">*Agregar una vista de detalles*</span><span class="sxs-lookup"><span data-stu-id="573f1-596">*Adding a Details View*</span></span>
3. <span data-ttu-id="573f1-597">Modificar el **Details.cshtml** archivo para mostrar información del álbum, obtener acceso a la **álbum** objeto que se pasa a la plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="573f1-597">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="573f1-598">Para ello, reemplace el contenido por lo siguiente: (C#)</span><span class="sxs-lookup"><span data-stu-id="573f1-598">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="573f1-599">Tarea 7: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="573f1-599">Task 7 - Running the Application</span></span>

<span data-ttu-id="573f1-600">En esta tarea, realizará las pruebas que el **detalles** vista recupera información del álbum desde la **detalles acción** método.</span><span class="sxs-lookup"><span data-stu-id="573f1-600">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="573f1-601">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="573f1-601">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="573f1-602">El proyecto se inicia en el **inicio** página.</span><span class="sxs-lookup"><span data-stu-id="573f1-602">The project starts in the **Home** page.</span></span> <span data-ttu-id="573f1-603">Cambiar la dirección URL de **/Store/Details/5** para comprobar la información del álbum.</span><span class="sxs-lookup"><span data-stu-id="573f1-603">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="573f1-604">![Examinar los detalles de álbumes](aspnet-mvc-4-fundamentals/_static/image32.png "exploración álbumes detalle")</span><span class="sxs-lookup"><span data-stu-id="573f1-604">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="573f1-605">*Examinar los detalles del álbum*</span><span class="sxs-lookup"><span data-stu-id="573f1-605">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="573f1-606">Tarea 8: agregar vínculos entre páginas</span><span class="sxs-lookup"><span data-stu-id="573f1-606">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="573f1-607">En esta tarea, agregará un vínculo en la vista de almacén para tener un vínculo en cada nombre de género correspondientes **/almacén/examinar** dirección URL.</span><span class="sxs-lookup"><span data-stu-id="573f1-607">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="573f1-608">Así, al hacer clic en un género, por ejemplo **Disco**, se le remitirá a **/almacén/examinar? género = Disco** dirección URL.</span><span class="sxs-lookup"><span data-stu-id="573f1-608">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="573f1-609">Cierre el explorador si es necesario, para volver a la ventana de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="573f1-609">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="573f1-610">Actualización de la **índice** página para agregar un vínculo a la **examinar** página.</span><span class="sxs-lookup"><span data-stu-id="573f1-610">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="573f1-611">Para ello, en el **el Explorador de soluciones** expandir la **vistas** carpeta, la **almacén** carpeta y haga doble clic en el **Index.cshtml** página.</span><span class="sxs-lookup"><span data-stu-id="573f1-611">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="573f1-612">Agregar un vínculo a la vista de exploración que indica el género seleccionado.</span><span class="sxs-lookup"><span data-stu-id="573f1-612">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="573f1-613">Para ello, reemplace el código que aparece resaltado en el  **&lt;li&gt;**  etiquetas: (C#)</span><span class="sxs-lookup"><span data-stu-id="573f1-613">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="573f1-614">otro enfoque podría ser vincular directamente a la página, con un código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="573f1-614">another approach would be linking directly to the page, with a code like the following:</span></span>
    > 
    > <span data-ttu-id="573f1-615">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span><span class="sxs-lookup"><span data-stu-id="573f1-615">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
    > 
    > <span data-ttu-id="573f1-616">Aunque este enfoque funciona, depende de una cadena codificada.</span><span class="sxs-lookup"><span data-stu-id="573f1-616">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="573f1-617">Si posteriormente se cambia el nombre del controlador, tendrá que cambiar manualmente esta instrucción.</span><span class="sxs-lookup"><span data-stu-id="573f1-617">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="573f1-618">Una alternativa mejor es usar un **aplicación auxiliar HTML** método.</span><span class="sxs-lookup"><span data-stu-id="573f1-618">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="573f1-619">ASP.NET MVC incluye un método de aplicación auxiliar HTML que está disponible para estas tareas.</span><span class="sxs-lookup"><span data-stu-id="573f1-619">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="573f1-620">El **Html.ActionLink()** método auxiliar resulta muy sencillo crear HTML  **&lt;una&gt;**  vínculos, asegurándose de que las rutas de acceso de dirección URL están correctamente la dirección URL codificada.</span><span class="sxs-lookup"><span data-stu-id="573f1-620">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
    > 
    > <span data-ttu-id="573f1-621">Htlm.ActionLink tiene varias sobrecargas.</span><span class="sxs-lookup"><span data-stu-id="573f1-621">Htlm.ActionLink has several overloads.</span></span> <span data-ttu-id="573f1-622">En este ejercicio usará una que toma tres parámetros:</span><span class="sxs-lookup"><span data-stu-id="573f1-622">In this exercise you will use one that takes three parameters:</span></span>
    > 
    > 1. <span data-ttu-id="573f1-623">Texto del vínculo, que mostrará el nombre de género</span><span class="sxs-lookup"><span data-stu-id="573f1-623">Link text, which will display the Genre name</span></span>
    > 2. <span data-ttu-id="573f1-624">Nombre de acción de controlador (**examinar**)</span><span class="sxs-lookup"><span data-stu-id="573f1-624">Controller action name (**Browse**)</span></span>
    > 3. <span data-ttu-id="573f1-625">Valores de parámetro, especificando el nombre de ruta (**género**) y el valor (**nombre de género**)</span><span class="sxs-lookup"><span data-stu-id="573f1-625">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="573f1-626">Tarea 9 - ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="573f1-626">Task 9 - Running the Application</span></span>

<span data-ttu-id="573f1-627">En esta tarea, probará que se muestra cada género con un vínculo a la correspondiente **/almacén/examinar** dirección URL.</span><span class="sxs-lookup"><span data-stu-id="573f1-627">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="573f1-628">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="573f1-628">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="573f1-629">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="573f1-629">The project starts in the Home page.</span></span> <span data-ttu-id="573f1-630">Cambiar la dirección URL de **/almacenar** para comprobar que cada género vincula a la correspondiente **/almacén/examinar** dirección URL.</span><span class="sxs-lookup"><span data-stu-id="573f1-630">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="573f1-631">![Exploración géneros con vínculos a la página Examinar](aspnet-mvc-4-fundamentals/_static/image33.png "géneros de exploración con vínculos a la página Examinar")</span><span class="sxs-lookup"><span data-stu-id="573f1-631">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="573f1-632">*Géneros de exploración con vínculos a la página Examinar*</span><span class="sxs-lookup"><span data-stu-id="573f1-632">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="573f1-633">Tarea 10 - utilizando la colección de ViewModel dinámica para pasar valores</span><span class="sxs-lookup"><span data-stu-id="573f1-633">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="573f1-634">En esta tarea, obtendrá información sobre un método simple y efectivo para pasar valores entre el controlador y la vista sin realizar ningún cambio en el modelo.</span><span class="sxs-lookup"><span data-stu-id="573f1-634">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="573f1-635">ASP.NET MVC 4 proporciona la colección &quot;ViewModel&quot;, que se puede asignar a cualquier valor dinámico y acceder a ellos dentro de los controladores y vistas también.</span><span class="sxs-lookup"><span data-stu-id="573f1-635">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="573f1-636">Ahora usará la colección dinámica ViewBag para pasar una lista de &quot; **destacan géneros** &quot; desde el controlador a la vista.</span><span class="sxs-lookup"><span data-stu-id="573f1-636">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="573f1-637">Tendrá acceso la vista de índice de almacén para **ViewModel** y mostrar la información.</span><span class="sxs-lookup"><span data-stu-id="573f1-637">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="573f1-638">Cierre el explorador si es necesario, para volver a la ventana de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="573f1-638">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="573f1-639">Abra **StoreController.cs** y modificar **índice** método para crear una lista de destacan géneros en ViewModel colección:</span><span class="sxs-lookup"><span data-stu-id="573f1-639">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="573f1-640">También puede usar la sintaxis **ViewBag [&quot;Starred&quot;]** para tener acceso a las propiedades.</span><span class="sxs-lookup"><span data-stu-id="573f1-640">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="573f1-641">El icono de estrella  **&quot;starred.png&quot;**  se incluye en el **Source\Assets\Images** carpeta de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="573f1-641">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="573f1-642">Para agregarlo a la aplicación, arrastre su contenido desde un **el Explorador de Windows** ventana en el **el Explorador de soluciones** en Visual Web Developer Express, tal y como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="573f1-642">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="573f1-643">![Imagen de estrella de agregar a la solución](aspnet-mvc-4-fundamentals/_static/image34.png "imagen de estrella de agregar a la solución")</span><span class="sxs-lookup"><span data-stu-id="573f1-643">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="573f1-644">*Agregar imagen de estrella a la solución*</span><span class="sxs-lookup"><span data-stu-id="573f1-644">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="573f1-645">Abra la vista **Store/Index.cshtml** y modificar el contenido.</span><span class="sxs-lookup"><span data-stu-id="573f1-645">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="573f1-646">Va a leer el &quot;destacan&quot; propiedad en el **ViewBag** colección y pregunte si el nombre de género actual está en la lista.</span><span class="sxs-lookup"><span data-stu-id="573f1-646">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="573f1-647">En ese caso, se mostrará un icono de estrella de derecha al vínculo de género.</span><span class="sxs-lookup"><span data-stu-id="573f1-647">In that case you will show a star icon right to the genre link.</span></span>
<span data-ttu-id="573f1-648">(C#)</span><span class="sxs-lookup"><span data-stu-id="573f1-648">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="573f1-649">Tarea 11: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="573f1-649">Task 11 - Running the Application</span></span>

<span data-ttu-id="573f1-650">En esta tarea, probará que el marcado géneros mostrar un icono de estrella.</span><span class="sxs-lookup"><span data-stu-id="573f1-650">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="573f1-651">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="573f1-651">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="573f1-652">El proyecto se inicia en el **inicio** página.</span><span class="sxs-lookup"><span data-stu-id="573f1-652">The project starts in the **Home** page.</span></span> <span data-ttu-id="573f1-653">Cambiar la dirección URL de **/almacén** para comprobar que cada género destacado tiene la etiqueta respetar:</span><span class="sxs-lookup"><span data-stu-id="573f1-653">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="573f1-654">![Exploración géneros con elementos de marcado](aspnet-mvc-4-fundamentals/_static/image35.png "exploración géneros con elementos de marcado a continuación:")</span><span class="sxs-lookup"><span data-stu-id="573f1-654">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="573f1-655">*Exploración géneros con elementos de marcado a continuación:*</span><span class="sxs-lookup"><span data-stu-id="573f1-655">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="573f1-656">Ejercicio 7: Aspectos básicos de nueva plantilla de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="573f1-656">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="573f1-657">En este ejercicio, explorará las mejoras en las plantillas de proyecto de ASP.NET MVC 4, echar un vistazo las características más relevantes de la nueva plantilla.</span><span class="sxs-lookup"><span data-stu-id="573f1-657">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="573f1-658">Tarea 1: Explorar la plantilla de aplicación de Internet de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="573f1-658">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="573f1-659">Si no está abierto, inicie **VS Express para Web**</span><span class="sxs-lookup"><span data-stu-id="573f1-659">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="573f1-660">Seleccione el **archivo | Nuevos | Proyecto** comando de menú.</span><span class="sxs-lookup"><span data-stu-id="573f1-660">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="573f1-661">En el **nuevo proyecto** cuadro de diálogo, seleccione la **Visual C# | Web** plantilla en el panel izquierdo del árbol y elija la **aplicación Web de ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="573f1-661">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="573f1-662">**Nombre** el proyecto *MusicStore* y actualizar la **nombre de la solución** a *comenzar*, a continuación, seleccione una ubicación (o deje el valor predeterminado) y haga clic en **Aceptar** .</span><span class="sxs-lookup"><span data-stu-id="573f1-662">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="573f1-663">![Crear un nuevo proyecto de ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "crear un nuevo proyecto de ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="573f1-663">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="573f1-664">*Crear un nuevo proyecto de ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="573f1-664">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="573f1-665">En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione la **aplicación de Internet** plantilla de proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="573f1-665">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="573f1-666">Tenga en cuenta que puede seleccionar como el motor de vista Razor o ASPX.</span><span class="sxs-lookup"><span data-stu-id="573f1-666">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="573f1-667">![Crear una nueva aplicación de Internet de ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image37.png "crear una nueva aplicación de Internet de ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="573f1-667">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="573f1-668">*Crear una nueva aplicación de Internet de ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="573f1-668">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="573f1-669">Se ha introducido la sintaxis Razor en ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="573f1-669">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="573f1-670">Su objetivo es reducir el número de caracteres y pulsaciones de teclas necesarias en un archivo, lo que permite una rápida y fluido de flujo de trabajo de codificación.</span><span class="sxs-lookup"><span data-stu-id="573f1-670">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="573f1-671">Razor aprovecha existente C# /VB (u otro) dominio del idioma y ofrece una sintaxis de marcado de plantilla que permite a un flujo de trabajo de construcción de HTML Maravilla.</span><span class="sxs-lookup"><span data-stu-id="573f1-671">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="573f1-672">Presione **F5** para ejecutar la solución y ver la plantilla renovada.</span><span class="sxs-lookup"><span data-stu-id="573f1-672">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="573f1-673">Puede consultar las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="573f1-673">You can check out the following features:</span></span>

    1. <span data-ttu-id="573f1-674">**Plantillas de estilo moderno**</span><span class="sxs-lookup"><span data-stu-id="573f1-674">**Modern-style templates**</span></span>

        <span data-ttu-id="573f1-675">Las plantillas que se han renovado, proporcionando más estilos de aspecto moderno.</span><span class="sxs-lookup"><span data-stu-id="573f1-675">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="573f1-676">![Plantillas de MVC de ASP.NET 4 estilo](aspnet-mvc-4-fundamentals/_static/image38.png "estilo plantillas ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="573f1-676">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="573f1-677">*Plantillas de estilo de ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="573f1-677">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="573f1-678">**Representación adaptable**</span><span class="sxs-lookup"><span data-stu-id="573f1-678">**Adaptive Rendering**</span></span>

        <span data-ttu-id="573f1-679">Consulte Cambiar el tamaño de la ventana del explorador y observe cómo el diseño de página se adapta dinámicamente al tamaño de la ventana nueva.</span><span class="sxs-lookup"><span data-stu-id="573f1-679">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="573f1-680">Estas plantillas usan la técnica de representación adaptable para representar correctamente en plataformas de escritorio y móviles sin ninguna personalización.</span><span class="sxs-lookup"><span data-stu-id="573f1-680">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="573f1-681">![Plantilla de proyecto de ASP.NET MVC 4 de tamaños diferentes del explorador](aspnet-mvc-4-fundamentals/_static/image39.png "plantilla de proyecto de ASP.NET MVC 4 de tamaños diferentes del explorador")</span><span class="sxs-lookup"><span data-stu-id="573f1-681">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="573f1-682">*Plantilla de proyecto de ASP.NET MVC 4 de tamaños diferentes del explorador*</span><span class="sxs-lookup"><span data-stu-id="573f1-682">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="573f1-683">Cierre el explorador para detener al depurador y vuelva a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="573f1-683">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="573f1-684">Ahora es posible explorar la solución y extraer del repositorio algunas de las nuevas características presentadas en ASP.NET MVC 4 en la plantilla de proyecto.</span><span class="sxs-lookup"><span data-stu-id="573f1-684">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="573f1-685">![ASP.NET MVC4-internet---plantilla de proyecto aplicación](aspnet-mvc-4-fundamentals/_static/image40.png "la plantilla de proyecto de aplicación de Internet de ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="573f1-685">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="573f1-686">*La plantilla de proyecto de aplicación de Internet de ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="573f1-686">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

    1. <span data-ttu-id="573f1-687">**Marcado de HTML5**</span><span class="sxs-lookup"><span data-stu-id="573f1-687">**HTML5 markup**</span></span>

        <span data-ttu-id="573f1-688">Examinar vistas de plantilla para averiguar el marcado de tema nuevo, por ejemplo abierto **About.cshtml** ver dentro de **inicio** carpeta.</span><span class="sxs-lookup"><span data-stu-id="573f1-688">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

        <span data-ttu-id="573f1-689">![Nueva plantilla, usando un marcado Razor y HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "nueva plantilla, usando un marcado Razor y HTML5")</span><span class="sxs-lookup"><span data-stu-id="573f1-689">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

        <span data-ttu-id="573f1-690">*Nueva plantilla, usando un marcado Razor y HTML5*</span><span class="sxs-lookup"><span data-stu-id="573f1-690">*New template, using Razor and HTML5 markup*</span></span>
    2. <span data-ttu-id="573f1-691">**Bibliotecas de JavaScript incluidas**</span><span class="sxs-lookup"><span data-stu-id="573f1-691">**JavaScript libraries included**</span></span>

        1. <span data-ttu-id="573f1-692">**jQuery**: jQuery simplifica atraviesa de documento HTML, control de eventos, animación y las interacciones de Ajax.</span><span class="sxs-lookup"><span data-stu-id="573f1-692">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
        2. <span data-ttu-id="573f1-693">**jQuery UI**: esta biblioteca proporciona abstracciones de interacción de bajo nivel y animación, avanzada efectos y widgets aptas para temas, creados a partir de la biblioteca de JavaScript de jQuery.</span><span class="sxs-lookup"><span data-stu-id="573f1-693">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

            > [!NOTE]
            > <span data-ttu-id="573f1-694">Puede obtener información acerca de jQuery y jQuery UI en [ [http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="573f1-694">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
        3. <span data-ttu-id="573f1-695">**KnockoutJS**: plantilla predeterminada de las 4 de ASP.NET MVC incluye ahora **KnockoutJS**, un marco de JavaScript MVVM que le permite crear aplicaciones completas y de alta capacidad de respuesta web con JavaScript y HTML.</span><span class="sxs-lookup"><span data-stu-id="573f1-695">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="573f1-696">Al igual que en ASP.NET MVC 3, jQuery y jQuery bibliotecas de interfaz de usuario también se incluyen en ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="573f1-696">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

            > [!NOTE]
            > <span data-ttu-id="573f1-697">Puede obtener más información acerca de la biblioteca de KnockOutJS en este vínculo: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="573f1-697">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
        4. <span data-ttu-id="573f1-698">**Modernizr**: esta biblioteca se ejecuta automáticamente, hacer que su sitio compatible con los exploradores más antiguos cuando se empleen tecnologías HTML5 y CSS3.</span><span class="sxs-lookup"><span data-stu-id="573f1-698">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

            > [!NOTE]
            > <span data-ttu-id="573f1-699">Puede obtener más información acerca de la biblioteca de Modernizr pertenecientes a este vínculo: [http://www.modernizr.com/](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="573f1-699">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
    3. <span data-ttu-id="573f1-700">**SimpleMembership incluido en la solución**</span><span class="sxs-lookup"><span data-stu-id="573f1-700">**SimpleMembership included in the solution**</span></span>

        <span data-ttu-id="573f1-701">SimpleMembership se ha diseñado como un reemplazo para el sistema de proveedor de funciones ASP.NET y la pertenencia a anterior.</span><span class="sxs-lookup"><span data-stu-id="573f1-701">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="573f1-702">Tiene muchas características nuevas que facilitan el desarrollador a páginas web seguras de manera más flexible.</span><span class="sxs-lookup"><span data-stu-id="573f1-702">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

        <span data-ttu-id="573f1-703">La plantilla de Internet ya ha configurado algunas cosas para integrar SimpleMembership, por ejemplo, el AccountController está preparado para usar OAuthWebSecurity (para el registro de la cuenta de OAuth, inicio de sesión, administración, etc.) y la seguridad de la Web.</span><span class="sxs-lookup"><span data-stu-id="573f1-703">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

        <span data-ttu-id="573f1-704">![SimpleMembership incluidos en la solución](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership incluidos en la solución")</span><span class="sxs-lookup"><span data-stu-id="573f1-704">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

        <span data-ttu-id="573f1-705">*SimpleMembership incluidos en la solución*</span><span class="sxs-lookup"><span data-stu-id="573f1-705">*SimpleMembership Included in the solution*</span></span>

        > [!NOTE]
        > <span data-ttu-id="573f1-706">Obtener más información acerca de [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) en MSDN.</span><span class="sxs-lookup"><span data-stu-id="573f1-706">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="573f1-707">Además, puede implementar esta aplicación a los siguientes sitios Web Windows Azure [Apéndice B: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="573f1-707">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="573f1-708">Resumen</span><span class="sxs-lookup"><span data-stu-id="573f1-708">Summary</span></span>

<span data-ttu-id="573f1-709">Al completar este laboratorio práctico ha aprendido los fundamentos de MVC de ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="573f1-709">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="573f1-710">Los elementos básicos de una aplicación MVC y cómo interactúan</span><span class="sxs-lookup"><span data-stu-id="573f1-710">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="573f1-711">Cómo crear una aplicación de MVC de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="573f1-711">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="573f1-712">Cómo agregar y configurar controladores para controlar los parámetros se pasa a través de la dirección URL y la cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="573f1-712">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="573f1-713">Cómo agregar una página maestra de diseño para configurar una plantilla para el contenido HTML común, una hoja de estilos para mejorar la apariencia y funcionamiento y una plantilla de vista para mostrar el contenido HTML</span><span class="sxs-lookup"><span data-stu-id="573f1-713">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="573f1-714">Cómo usar el patrón ViewModel para pasar propiedades a la plantilla de vista para mostrar información dinámica</span><span class="sxs-lookup"><span data-stu-id="573f1-714">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="573f1-715">Cómo usar los parámetros pasados a los controladores en la plantilla de vista</span><span class="sxs-lookup"><span data-stu-id="573f1-715">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="573f1-716">Cómo agregar vínculos a páginas dentro de la aplicación de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="573f1-716">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="573f1-717">Cómo agregar y utilizar propiedades dinámicas en una vista</span><span class="sxs-lookup"><span data-stu-id="573f1-717">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="573f1-718">Las mejoras en las plantillas de proyecto de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="573f1-718">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="573f1-719">Apéndice A: instalación de Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="573f1-719">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="573f1-720">Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión usando la  **[instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="573f1-720">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="573f1-721">Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* con *instalador de plataforma Web de Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="573f1-721">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="573f1-722">Vaya a [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="573f1-722">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="573f1-723">O bien, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; *Visual Studio Express 2012 for Web con SDK de Windows Azure*&quot;.</span><span class="sxs-lookup"><span data-stu-id="573f1-723">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="573f1-724">Haga clic en **instalar ahora**.</span><span class="sxs-lookup"><span data-stu-id="573f1-724">Click on **Install Now**.</span></span> <span data-ttu-id="573f1-725">Si no tiene **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo primero.</span><span class="sxs-lookup"><span data-stu-id="573f1-725">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="573f1-726">Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.</span><span class="sxs-lookup"><span data-stu-id="573f1-726">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="573f1-727">![Instale Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "instale Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="573f1-727">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="573f1-728">*Instale Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="573f1-728">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="573f1-729">Lea los términos y licencias de todos los productos y haga clic en **acepto** para continuar.</span><span class="sxs-lookup"><span data-stu-id="573f1-729">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Acepte los términos de licencia](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="573f1-731">*Acepte los términos de licencia*</span><span class="sxs-lookup"><span data-stu-id="573f1-731">*Accepting the license terms*</span></span>
5. <span data-ttu-id="573f1-732">Espere hasta que finalice el proceso de descarga e instalación.</span><span class="sxs-lookup"><span data-stu-id="573f1-732">Wait until the downloading and installation process completes.</span></span>

    ![Progreso de la instalación](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="573f1-734">*Progreso de la instalación*</span><span class="sxs-lookup"><span data-stu-id="573f1-734">*Installation progress*</span></span>
6. <span data-ttu-id="573f1-735">Cuando se complete la instalación, haga clic en **finalizar**.</span><span class="sxs-lookup"><span data-stu-id="573f1-735">When the installation completes, click **Finish**.</span></span>

    ![Se completó la instalación](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="573f1-737">*Se completó la instalación*</span><span class="sxs-lookup"><span data-stu-id="573f1-737">*Installation completed*</span></span>
7. <span data-ttu-id="573f1-738">Haga clic en **Exit** para cerrar el instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="573f1-738">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="573f1-739">Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **Express frente a**&quot;, a continuación, haga clic en el **VS Express para Web** colocar en mosaico.</span><span class="sxs-lookup"><span data-stu-id="573f1-739">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Express de VS para icono Web](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="573f1-741">*Express de VS para icono Web*</span><span class="sxs-lookup"><span data-stu-id="573f1-741">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="573f1-742">Apéndice B: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy</span><span class="sxs-lookup"><span data-stu-id="573f1-742">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="573f1-743">Este apéndice le mostrará cómo crear un nuevo sitio web del Portal de administración de Windows Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, aprovechando la característica de publicación Web Deploy proporcionada por Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="573f1-743">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="573f1-744">Tarea 1: crear un nuevo sitio Web desde las ventanas de Portal de Azure</span><span class="sxs-lookup"><span data-stu-id="573f1-744">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="573f1-745">Vaya a la [Portal de administración de Windows Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas con su suscripción.</span><span class="sxs-lookup"><span data-stu-id="573f1-745">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="573f1-746">Con Windows Azure puede hospedar 10 sitios Web de ASP.NET de forma gratuita y, a continuación, ajustar la escala a medida que crece el tráfico.</span><span class="sxs-lookup"><span data-stu-id="573f1-746">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="573f1-747">Puede registrarse [aquí](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="573f1-747">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="573f1-748">![Inicie sesión en el portal de Windows Azure](aspnet-mvc-4-fundamentals/_static/image48.png "inicie sesión en el portal de Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="573f1-748">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="573f1-749">*Inicie sesión en el Portal de administración de Azure*</span><span class="sxs-lookup"><span data-stu-id="573f1-749">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="573f1-750">Haga clic en **New** en la barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="573f1-750">Click **New** on the command bar.</span></span>

    <span data-ttu-id="573f1-751">![Crear un nuevo sitio Web](aspnet-mvc-4-fundamentals/_static/image49.png "crear un nuevo sitio Web")</span><span class="sxs-lookup"><span data-stu-id="573f1-751">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="573f1-752">*Crear un nuevo sitio Web*</span><span class="sxs-lookup"><span data-stu-id="573f1-752">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="573f1-753">Haga clic en **proceso** | **sitio Web**.</span><span class="sxs-lookup"><span data-stu-id="573f1-753">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="573f1-754">A continuación, seleccione **creación rápida** opción.</span><span class="sxs-lookup"><span data-stu-id="573f1-754">Then select **Quick Create** option.</span></span> <span data-ttu-id="573f1-755">Proporcione una dirección de URL disponible para el nuevo sitio web y haga clic en **crear sitio Web**.</span><span class="sxs-lookup"><span data-stu-id="573f1-755">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="573f1-756">Un sitio Web de Windows Azure es el host para una aplicación web que se ejecutan en la nube que puede controlar y administrar.</span><span class="sxs-lookup"><span data-stu-id="573f1-756">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="573f1-757">La opción Creación rápida permite implementar una aplicación web completada al Windows Azure sitio Web desde fuera del portal.</span><span class="sxs-lookup"><span data-stu-id="573f1-757">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="573f1-758">No se incluyen pasos para configurar una base de datos.</span><span class="sxs-lookup"><span data-stu-id="573f1-758">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="573f1-759">![Crear un nuevo sitio Web mediante Creación rápida](aspnet-mvc-4-fundamentals/_static/image50.png "crear un nuevo sitio Web mediante Creación rápida")</span><span class="sxs-lookup"><span data-stu-id="573f1-759">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="573f1-760">*Crear un nuevo sitio Web mediante Creación rápida*</span><span class="sxs-lookup"><span data-stu-id="573f1-760">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="573f1-761">Espere hasta que el nuevo **sitio Web** se crea.</span><span class="sxs-lookup"><span data-stu-id="573f1-761">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="573f1-762">Una vez creado el sitio Web, haga clic en el vínculo situado bajo el **URL** columna.</span><span class="sxs-lookup"><span data-stu-id="573f1-762">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="573f1-763">Compruebe que funciona el nuevo sitio Web.</span><span class="sxs-lookup"><span data-stu-id="573f1-763">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="573f1-764">![En el nuevo sitio web](aspnet-mvc-4-fundamentals/_static/image51.png "exploración al sitio web nuevo")</span><span class="sxs-lookup"><span data-stu-id="573f1-764">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="573f1-765">*Examinar el sitio web nuevo*</span><span class="sxs-lookup"><span data-stu-id="573f1-765">*Browsing to the new web site*</span></span>

    <span data-ttu-id="573f1-766">![Sitio Web que ejecute](aspnet-mvc-4-fundamentals/_static/image52.png "sitio Web que se ejecuta")</span><span class="sxs-lookup"><span data-stu-id="573f1-766">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="573f1-767">*Sitio Web que se ejecuta*</span><span class="sxs-lookup"><span data-stu-id="573f1-767">*Web site running*</span></span>
6. <span data-ttu-id="573f1-768">Vuelva al portal y haga clic en el nombre del sitio web en el **nombre** columna para mostrar las páginas de administración.</span><span class="sxs-lookup"><span data-stu-id="573f1-768">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="573f1-769">![Abrir las páginas de administración del sitio web](aspnet-mvc-4-fundamentals/_static/image53.png "abrir las páginas de administración del sitio web")</span><span class="sxs-lookup"><span data-stu-id="573f1-769">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="573f1-770">*Abrir las páginas de administración del sitio Web*</span><span class="sxs-lookup"><span data-stu-id="573f1-770">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="573f1-771">En el **panel** página, en la **vista rápida** sección, haga clic en el **descargar perfil de publicación** vínculo.</span><span class="sxs-lookup"><span data-stu-id="573f1-771">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="573f1-772">El *perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un sitio Web de Windows Azure para cada método de publicación habilitado.</span><span class="sxs-lookup"><span data-stu-id="573f1-772">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="573f1-773">El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse y autenticarse en cada uno de los puntos de conexión para el que está habilitado un método de publicación.</span><span class="sxs-lookup"><span data-stu-id="573f1-773">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="573f1-774">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para publicación de aplicaciones web a los sitios Web de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="573f1-774">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="573f1-775">![Descargar el sitio web de perfil de publicación](aspnet-mvc-4-fundamentals/_static/image54.png "descargar el sitio web de perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="573f1-775">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="573f1-776">*Descargar el sitio Web de perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="573f1-776">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="573f1-777">Descargue el archivo de perfil de publicación en una ubicación conocida.</span><span class="sxs-lookup"><span data-stu-id="573f1-777">Download the publish profile file to a known location.</span></span> <span data-ttu-id="573f1-778">Aún más en este ejercicio verá cómo utilizar este archivo para publicar una aplicación web a un sitios Web de Windows Azure desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="573f1-778">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="573f1-779">![Guardar el archivo de perfil de publicación](aspnet-mvc-4-fundamentals/_static/image55.png "guardar el perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="573f1-779">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="573f1-780">*Guardar el archivo de perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="573f1-780">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="573f1-781">Tarea 2: configurar el servidor de base de datos</span><span class="sxs-lookup"><span data-stu-id="573f1-781">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="573f1-782">Si la aplicación realiza el uso de SQL Server debe crear un servidor de base de datos SQL de bases de datos.</span><span class="sxs-lookup"><span data-stu-id="573f1-782">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="573f1-783">Si desea implementar una aplicación simple que no utiliza SQL Server, podría omitir esta tarea.</span><span class="sxs-lookup"><span data-stu-id="573f1-783">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="573f1-784">Necesitará un servidor de base de datos SQL para almacenar la base de datos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="573f1-784">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="573f1-785">Puede ver los servidores de base de datos SQL de la suscripción en el portal de administración de Windows Azure en **bases de datos Sql** | **servidores** | **del servidor Panel**.</span><span class="sxs-lookup"><span data-stu-id="573f1-785">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="573f1-786">Si no tiene un servidor que creó, puede crear uno mediante la **agregar** botón en la barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="573f1-786">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="573f1-787">Tome nota de la **nombre del servidor y la dirección URL, nombre de inicio de sesión de administrador y contraseña**, tal y como se va a utilizar en las siguientes tareas.</span><span class="sxs-lookup"><span data-stu-id="573f1-787">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="573f1-788">No cree la base de datos, tal y como se creará en una fase posterior.</span><span class="sxs-lookup"><span data-stu-id="573f1-788">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="573f1-789">![Panel de servidor de base de datos SQL](aspnet-mvc-4-fundamentals/_static/image56.png "panel de base de datos SQL Server")</span><span class="sxs-lookup"><span data-stu-id="573f1-789">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="573f1-790">*Panel de base de datos SQL Server*</span><span class="sxs-lookup"><span data-stu-id="573f1-790">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="573f1-791">En la siguiente tarea probará la conexión de base de datos de Visual Studio, por ese motivo debe incluir la dirección IP local en la lista del servidor de **direcciones IP permitidas**.</span><span class="sxs-lookup"><span data-stu-id="573f1-791">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="573f1-792">Para ello, haga clic en **configurar**, seleccione la dirección IP de **dirección IP del cliente actual** y péguela en el **dirección IP inicial** y **ladirecciónIPfinal** cuadros de texto y haga clic en el ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) botón.</span><span class="sxs-lookup"><span data-stu-id="573f1-792">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![Agregar dirección IP del cliente](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="573f1-794">*Agregar dirección IP del cliente*</span><span class="sxs-lookup"><span data-stu-id="573f1-794">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="573f1-795">Una vez el **dirección IP del cliente** se agrega a las direcciones IP permitidas lista, haga clic en **guardar** para confirmar los cambios.</span><span class="sxs-lookup"><span data-stu-id="573f1-795">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmar cambios](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="573f1-797">*Confirmar cambios*</span><span class="sxs-lookup"><span data-stu-id="573f1-797">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="573f1-798">Tarea 3: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy</span><span class="sxs-lookup"><span data-stu-id="573f1-798">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="573f1-799">Vuelva a la solución de ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="573f1-799">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="573f1-800">En el **el Explorador de soluciones**, haga clic en el proyecto de sitio web y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="573f1-800">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="573f1-801">![Publicar la aplicación](aspnet-mvc-4-fundamentals/_static/image60.png "publicar la aplicación")</span><span class="sxs-lookup"><span data-stu-id="573f1-801">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="573f1-802">*Publicar el sitio web*</span><span class="sxs-lookup"><span data-stu-id="573f1-802">*Publishing the web site*</span></span>
2. <span data-ttu-id="573f1-803">Importar el perfil de publicación que se ha guardado en la primera tarea.</span><span class="sxs-lookup"><span data-stu-id="573f1-803">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="573f1-804">![Importar el perfil de publicación](aspnet-mvc-4-fundamentals/_static/image61.png "importar el perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="573f1-804">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="573f1-805">*Importar perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="573f1-805">*Importing publish profile*</span></span>
3. <span data-ttu-id="573f1-806">Haga clic en **validar conexión**.</span><span class="sxs-lookup"><span data-stu-id="573f1-806">Click **Validate Connection**.</span></span> <span data-ttu-id="573f1-807">Una vez completada la validación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="573f1-807">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="573f1-808">Validación está completa cuando aparece una marca de verificación verde junto al botón Validar conexión.</span><span class="sxs-lookup"><span data-stu-id="573f1-808">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="573f1-809">![Validación de la conexión](aspnet-mvc-4-fundamentals/_static/image62.png "validación de la conexión")</span><span class="sxs-lookup"><span data-stu-id="573f1-809">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="573f1-810">*Validación de la conexión*</span><span class="sxs-lookup"><span data-stu-id="573f1-810">*Validating connection*</span></span>
4. <span data-ttu-id="573f1-811">En el **configuración** página, en la **bases de datos** sección, haga clic en el botón situado junto al cuadro de texto de la conexión de la base de datos (es decir, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="573f1-811">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="573f1-812">![Configuración de Web deploy](aspnet-mvc-4-fundamentals/_static/image63.png "configuración de Web deploy")</span><span class="sxs-lookup"><span data-stu-id="573f1-812">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="573f1-813">*Configuración de Web deploy*</span><span class="sxs-lookup"><span data-stu-id="573f1-813">*Web deploy configuration*</span></span>
5. <span data-ttu-id="573f1-814">Configure la conexión de base de datos de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="573f1-814">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="573f1-815">En el **nombre del servidor** escriba la dirección URL de base de datos de SQL server mediante la *tcp:* prefijo.</span><span class="sxs-lookup"><span data-stu-id="573f1-815">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="573f1-816">En **nombre de usuario** escriba el nombre de inicio de sesión del Administrador de servidor.</span><span class="sxs-lookup"><span data-stu-id="573f1-816">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="573f1-817">En **contraseña** escriba la contraseña de inicio de sesión de administrador de servidor.</span><span class="sxs-lookup"><span data-stu-id="573f1-817">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="573f1-818">Escriba un nuevo nombre de base de datos, por ejemplo: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="573f1-818">Type a new database name, for example: *MVC4SampleDB*.</span></span>

    <span data-ttu-id="573f1-819">![Configurar la cadena de conexión de destino](aspnet-mvc-4-fundamentals/_static/image64.png "configurar la cadena de conexión de destino")</span><span class="sxs-lookup"><span data-stu-id="573f1-819">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="573f1-820">*Configurar la cadena de conexión de destino*</span><span class="sxs-lookup"><span data-stu-id="573f1-820">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="573f1-821">A continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="573f1-821">Then click **OK**.</span></span> <span data-ttu-id="573f1-822">Cuando se le solicite para crear la base de datos, haga clic en **Sí**.</span><span class="sxs-lookup"><span data-stu-id="573f1-822">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="573f1-823">![Crear la base de datos](aspnet-mvc-4-fundamentals/_static/image65.png "crear la cadena de la base de datos")</span><span class="sxs-lookup"><span data-stu-id="573f1-823">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="573f1-824">*Crear la base de datos*</span><span class="sxs-lookup"><span data-stu-id="573f1-824">*Creating the database*</span></span>
7. <span data-ttu-id="573f1-825">La cadena de conexión que se va a usar para conectarse a la base de datos de SQL en Windows Azure se muestra en el cuadro de texto de conexión predeterminado.</span><span class="sxs-lookup"><span data-stu-id="573f1-825">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="573f1-826">Después, haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="573f1-826">Then click **Next**.</span></span>

    <span data-ttu-id="573f1-827">![Cadena de conexión que apunte a la base de datos SQL](aspnet-mvc-4-fundamentals/_static/image66.png "cadena de conexión que apunte a la base de datos SQL")</span><span class="sxs-lookup"><span data-stu-id="573f1-827">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="573f1-828">*Cadena de conexión que apunte a la base de datos SQL*</span><span class="sxs-lookup"><span data-stu-id="573f1-828">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="573f1-829">En el **vista previa** página, haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="573f1-829">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="573f1-830">![Publicar la aplicación web](aspnet-mvc-4-fundamentals/_static/image67.png "publicar la aplicación web")</span><span class="sxs-lookup"><span data-stu-id="573f1-830">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="573f1-831">*Publicar la aplicación web*</span><span class="sxs-lookup"><span data-stu-id="573f1-831">*Publishing the web application*</span></span>
9. <span data-ttu-id="573f1-832">Una vez que finalice el proceso de publicación, el explorador predeterminado abrirá el sitio web publicado.</span><span class="sxs-lookup"><span data-stu-id="573f1-832">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="573f1-833">![Publica la aplicación en Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "aplicación se publicó en Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="573f1-833">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="573f1-834">*Aplicación que se publican en Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="573f1-834">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="573f1-835">Apéndice C: usar fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="573f1-835">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="573f1-836">Con fragmentos de código, tiene todo el código que necesita a su alcance.</span><span class="sxs-lookup"><span data-stu-id="573f1-836">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="573f1-837">El documento de laboratorio le indicará exactamente cuándo utilizarlas, tal como se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="573f1-837">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="573f1-838">![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-fundamentals/_static/image69.png "fragmentos de código en Visual Studio para insertar código en el proyecto")</span><span class="sxs-lookup"><span data-stu-id="573f1-838">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="573f1-839">*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*</span><span class="sxs-lookup"><span data-stu-id="573f1-839">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="573f1-840">***Para agregar un fragmento de código mediante el teclado (solo C#)***</span><span class="sxs-lookup"><span data-stu-id="573f1-840">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="573f1-841">Coloque el cursor donde desea insertar el código.</span><span class="sxs-lookup"><span data-stu-id="573f1-841">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="573f1-842">Comience a escribir el nombre del fragmento (sin espacios ni guiones).</span><span class="sxs-lookup"><span data-stu-id="573f1-842">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="573f1-843">Observe como coincidencia de nombres de fragmentos de código de muestra de IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="573f1-843">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="573f1-844">Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).</span><span class="sxs-lookup"><span data-stu-id="573f1-844">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="573f1-845">Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.</span><span class="sxs-lookup"><span data-stu-id="573f1-845">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="573f1-846">![Comience a escribir el nombre del fragmento](aspnet-mvc-4-fundamentals/_static/image70.png "comience a escribir el nombre del fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="573f1-846">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="573f1-847">*Comience a escribir el nombre del fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="573f1-847">*Start typing the snippet name*</span></span>

<span data-ttu-id="573f1-848">![Presione la tecla Tab para seleccionar el fragmento de código resaltada](aspnet-mvc-4-fundamentals/_static/image71.png "presione Tab para seleccionar el fragmento de código resaltada")</span><span class="sxs-lookup"><span data-stu-id="573f1-848">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="573f1-849">*Presione la tecla Tab para seleccionar el fragmento de código resaltada*</span><span class="sxs-lookup"><span data-stu-id="573f1-849">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="573f1-850">![Vuelva a presionar Tab y el fragmento de código se expandirán](aspnet-mvc-4-fundamentals/_static/image72.png "vuelva a presionar Tab y el fragmento de código se expandirán")</span><span class="sxs-lookup"><span data-stu-id="573f1-850">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="573f1-851">*Vuelva a presionar Tab y el fragmento de código se expandirán*</span><span class="sxs-lookup"><span data-stu-id="573f1-851">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="573f1-852">***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="573f1-852">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="573f1-853">Haga clic en donde desea insertar el fragmento de código.</span><span class="sxs-lookup"><span data-stu-id="573f1-853">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="573f1-854">Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.</span><span class="sxs-lookup"><span data-stu-id="573f1-854">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="573f1-855">Seleccione el fragmento de código relevante de la lista haciendo clic en él.</span><span class="sxs-lookup"><span data-stu-id="573f1-855">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="573f1-856">![Menú contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](aspnet-mvc-4-fundamentals/_static/image73.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="573f1-856">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="573f1-857">*Haga clic en donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="573f1-857">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="573f1-858">![Seleccione el fragmento de código relevante de la lista haciendo clic en él](aspnet-mvc-4-fundamentals/_static/image74.png "elegir el fragmento de código relevante de la lista haciendo clic en él")</span><span class="sxs-lookup"><span data-stu-id="573f1-858">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="573f1-859">*Seleccione el fragmento de código relevante de la lista haciendo clic en él*</span><span class="sxs-lookup"><span data-stu-id="573f1-859">*Pick the relevant snippet from the list, by clicking on it*</span></span>
