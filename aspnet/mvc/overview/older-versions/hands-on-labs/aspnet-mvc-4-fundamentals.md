---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: Aspectos básicos de ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Este laboratorio práctico se basa en Store música MVC (Model View Controller), una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MV...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 3a282d02ba929eb86571e92f190550614962524d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386580"
---
# <a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="eb05d-103">Aspectos básicos de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="eb05d-103">ASP.NET MVC 4 Fundamentals</span></span>

<span data-ttu-id="eb05d-104">por [campamentos Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="eb05d-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="eb05d-105">Descargue el Kit de aprendizaje de campamentos de Web</span><span class="sxs-lookup"><span data-stu-id="eb05d-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="eb05d-106">Este laboratorio práctico se basa en Store música MVC (Model View Controller), una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-106">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="eb05d-107">En todo el laboratorio, aprenderá la simplicidad, aún más potencia de usar estas tecnologías conjuntamente.</span><span class="sxs-lookup"><span data-stu-id="eb05d-107">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="eb05d-108">Se iniciará con una aplicación sencilla y compilará hasta que haya una aplicación de Web de ASP.NET MVC 4 totalmente funcional.</span><span class="sxs-lookup"><span data-stu-id="eb05d-108">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>

<span data-ttu-id="eb05d-109">Esta práctica funciona con ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="eb05d-109">This Lab works with ASP.NET MVC 4.</span></span>

<span data-ttu-id="eb05d-110">Si desea explorar la versión de ASP.NET MVC 3 de la aplicación del tutorial, puede encontrarlo en [MVC-música-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="eb05d-110">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="eb05d-111">Esta práctica se da por supuesto que el desarrollador tiene experiencia en las tecnologías de desarrollo Web, como HTML y JavaScript.</span><span class="sxs-lookup"><span data-stu-id="eb05d-111">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="eb05d-112">Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de entrenamiento campamentos de Web, que está disponible en [versiones de Microsoft de Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="eb05d-112">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="eb05d-113">Está disponible en el proyecto específico para este laboratorio [aspectos básicos de ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span><span class="sxs-lookup"><span data-stu-id="eb05d-113">The project specific to this lab is available at [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span></span>

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="eb05d-114">La aplicación Music Store</span><span class="sxs-lookup"><span data-stu-id="eb05d-114">The Music Store application</span></span>

<span data-ttu-id="eb05d-115">La aplicación web de Music Store que se generarán a lo largo de este laboratorio consta de tres partes principales: la compra, la desprotección y la administración.</span><span class="sxs-lookup"><span data-stu-id="eb05d-115">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="eb05d-116">Los visitantes podrán examinar álbumes por género, agregar álbumes a su carro, revise su selección y, por último, continúe con la retirada para iniciar sesión y completar el pedido.</span><span class="sxs-lookup"><span data-stu-id="eb05d-116">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="eb05d-117">Además, los administradores del almacén podrá administrar los álbumes disponibles, así como sus propiedades principales.</span><span class="sxs-lookup"><span data-stu-id="eb05d-117">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="eb05d-118">![Las pantallas de Music Store](aspnet-mvc-4-fundamentals/_static/image1.png "pantallas de Music Store")</span><span class="sxs-lookup"><span data-stu-id="eb05d-118">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="eb05d-119">*Pantallas de Music Store*</span><span class="sxs-lookup"><span data-stu-id="eb05d-119">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="eb05d-120">Fundamentos de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="eb05d-120">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="eb05d-121">Aplicación Music Store se compilará con **Model View Controller (MVC)**, un patrón de arquitectura que separa una aplicación en tres componentes principales:</span><span class="sxs-lookup"><span data-stu-id="eb05d-121">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="eb05d-122">**Modelos**: objetos de modelo son las partes de la aplicación que implementan la lógica del dominio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-122">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="eb05d-123">A menudo, los objetos del modelo también recuperar y almacenan el estado del modelo en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="eb05d-123">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="eb05d-124">**Vistas:** las vistas son los componentes que muestran la interfaz de usuario (UI) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb05d-124">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="eb05d-125">Normalmente, esta interfaz de usuario se crea a partir de los datos del modelo.</span><span class="sxs-lookup"><span data-stu-id="eb05d-125">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="eb05d-126">Un ejemplo sería la vista de edición de álbumes que muestra los cuadros de texto y una lista desplegable según el estado actual de un objeto del álbum.</span><span class="sxs-lookup"><span data-stu-id="eb05d-126">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="eb05d-127">**Controladores:** controladores son los componentes que controlan la interacción del usuario, manipulan el modelo y por último seleccionan una vista para representar la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="eb05d-127">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="eb05d-128">En una aplicación de MVC, la vista solo muestra información; el controlador controla y responde a la interacción y los datos que introducen los usuarios.</span><span class="sxs-lookup"><span data-stu-id="eb05d-128">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="eb05d-129">El patrón de MVC le ayuda a crear aplicaciones que separan los diferentes aspectos de la aplicación (lógica de entrada, lógica de negocios y lógica de la interfaz de usuario), al tiempo que proporciona un acoplamiento vago entre estos elementos.</span><span class="sxs-lookup"><span data-stu-id="eb05d-129">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="eb05d-130">Esta separación le ayuda a administrar la complejidad al compilar una aplicación, ya que permite al usuario centrarse en uno de los aspectos de la implementación a la vez.</span><span class="sxs-lookup"><span data-stu-id="eb05d-130">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="eb05d-131">Además, el patrón MVC facilita probar las aplicaciones, también fomenta el uso de desarrollo controlado por pruebas (TDD) para crear aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="eb05d-131">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="eb05d-132">El **ASP.NET MVC** framework proporciona una alternativa al modelo de formularios Web Forms ASP.NET para crear aplicaciones Web basadas en ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="eb05d-132">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="eb05d-133">El **ASP.NET MVC** framework es un marco de presentación de poca que (al igual que con aplicaciones basadas en Web forms) se integra con las características de ASP.NET existentes, como páginas maestras y basada en pertenencia autenticación para obtener toda la potencia de la plataforma de .NET core.</span><span class="sxs-lookup"><span data-stu-id="eb05d-133">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="eb05d-134">Esto es útil si ya está familiarizado con ASP.NET Web Forms porque todas las bibliotecas que ya usan también están disponibles en ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="eb05d-134">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="eb05d-135">Además, el acoplamiento flexible entre los tres componentes principales de una aplicación MVC también favorece el desarrollo paralelo.</span><span class="sxs-lookup"><span data-stu-id="eb05d-135">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="eb05d-136">Por ejemplo, un desarrollador puede trabajar en la vista, un segundo desarrollador puede trabajar en la lógica del controlador y un desarrollador terceros puede centrarse en la lógica de negocios en el modelo.</span><span class="sxs-lookup"><span data-stu-id="eb05d-136">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="eb05d-137">Objetivos</span><span class="sxs-lookup"><span data-stu-id="eb05d-137">Objectives</span></span>

<span data-ttu-id="eb05d-138">En este laboratorio práctico, aprenderá cómo:</span><span class="sxs-lookup"><span data-stu-id="eb05d-138">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="eb05d-139">Crear una aplicación MVC de ASP.NET desde cero basándose en el tutorial de aplicación de Music Store</span><span class="sxs-lookup"><span data-stu-id="eb05d-139">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="eb05d-140">Agregar controladores para controlar las direcciones URL a la página principal del sitio y para explorar su funcionalidad principal</span><span class="sxs-lookup"><span data-stu-id="eb05d-140">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="eb05d-141">Agregar una vista para personalizar el contenido que se muestra junto con su estilo</span><span class="sxs-lookup"><span data-stu-id="eb05d-141">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="eb05d-142">Agregar clases de modelo que contiene y administra la lógica de datos y el dominio</span><span class="sxs-lookup"><span data-stu-id="eb05d-142">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="eb05d-143">Usar el patrón de modelo de vista para pasar información de las acciones de controlador a las plantillas de vista</span><span class="sxs-lookup"><span data-stu-id="eb05d-143">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="eb05d-144">Explore la nueva plantilla de ASP.NET MVC 4 para las aplicaciones de internet</span><span class="sxs-lookup"><span data-stu-id="eb05d-144">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="eb05d-145">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="eb05d-145">Prerequisites</span></span>

<span data-ttu-id="eb05d-146">Debe tener los elementos siguientes para completar esta práctica:</span><span class="sxs-lookup"><span data-stu-id="eb05d-146">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="eb05d-147">[Visual Studio 2012 Express para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (leer [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo)</span><span class="sxs-lookup"><span data-stu-id="eb05d-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="eb05d-148">Programa de instalación</span><span class="sxs-lookup"><span data-stu-id="eb05d-148">Setup</span></span>

<span data-ttu-id="eb05d-149">**Instalación de fragmentos de código**</span><span class="sxs-lookup"><span data-stu-id="eb05d-149">**Installing Code Snippets**</span></span>

<span data-ttu-id="eb05d-150">Para mayor comodidad, gran parte del código que se va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-150">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="eb05d-151">Para instalar los fragmentos de código que se ejecute **.\Source\Setup\CodeSnippets.vsi** archivo.</span><span class="sxs-lookup"><span data-stu-id="eb05d-151">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="eb05d-152">Si no está familiarizado con los fragmentos de código de Visual Studio y desea aprender a usarlas, puede consultar el apéndice de este documento &quot; [Apéndice C: Using Code Snippets](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="eb05d-152">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="eb05d-153">Ejercicios</span><span class="sxs-lookup"><span data-stu-id="eb05d-153">Exercises</span></span>

<span data-ttu-id="eb05d-154">Esta práctica se compone de los ejercicios siguientes:</span><span class="sxs-lookup"><span data-stu-id="eb05d-154">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="eb05d-155">Ejercicio 1: Crear el proyecto de aplicación Web de Music Store tal ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="eb05d-155">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="eb05d-156">Ejercicio 2: Creación de un controlador</span><span class="sxs-lookup"><span data-stu-id="eb05d-156">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="eb05d-157">Ejercicio 3: Pasar parámetros a un controlador</span><span class="sxs-lookup"><span data-stu-id="eb05d-157">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="eb05d-158">Ejercicio 4: Crear una vista</span><span class="sxs-lookup"><span data-stu-id="eb05d-158">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="eb05d-159">Ejercicio 5: Crear un modelo de vista</span><span class="sxs-lookup"><span data-stu-id="eb05d-159">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="eb05d-160">Ejercicio 6: Uso de parámetros en la vista</span><span class="sxs-lookup"><span data-stu-id="eb05d-160">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="eb05d-161">Ejercicio 7: Un paseo por la nueva plantilla de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="eb05d-161">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="eb05d-162">Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debe obtener después de completar los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="eb05d-162">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="eb05d-163">Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="eb05d-163">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="eb05d-164">Tiempo estimado para completar esta práctica: **60 minutos**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-164">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="eb05d-165">Ejercicio 1: Crear el proyecto de aplicación Web de Music Store tal ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="eb05d-165">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="eb05d-166">En este ejercicio, obtendrá información sobre cómo crear una aplicación ASP.NET MVC en Visual Studio 2012 Express para Web, así como su organización de la carpeta principal.</span><span class="sxs-lookup"><span data-stu-id="eb05d-166">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="eb05d-167">Además, obtendrá información sobre cómo agregar un nuevo controlador y que se muestre una cadena sencilla en la página principal de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb05d-167">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="eb05d-168">Tarea 1: crear el proyecto de aplicación Web de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="eb05d-168">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="eb05d-169">En esta tarea, creará un proyecto de aplicación de ASP.NET MVC vacío mediante la plantilla de MVC de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-169">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="eb05d-170">Iniciar **VS Express para Web**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-170">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="eb05d-171">En el menú **Archivo**, haga clic en **Nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-171">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="eb05d-172">En el **nuevo proyecto** cuadro de diálogo, seleccione el **aplicación Web de ASP.NET MVC 4** tipo, ubicado en proyecto **Visual C#,** **Web** plantilla lista.</span><span class="sxs-lookup"><span data-stu-id="eb05d-172">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="eb05d-173">Cambiar el **nombre** a *MvcMusicStore*.</span><span class="sxs-lookup"><span data-stu-id="eb05d-173">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="eb05d-174">Establecer la ubicación de la solución dentro de una nueva **comenzar** carpeta en la carpeta de origen de este ejercicio, por ejemplo **\Source\Ex01-CreatingMusicStoreProject\Begin [YOUR-HOL-PATH]**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-174">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="eb05d-175">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-175">Click **OK**.</span></span>

    <span data-ttu-id="eb05d-176">![Crear el cuadro de diálogo nuevo proyecto](aspnet-mvc-4-fundamentals/_static/image2.png "crear el cuadro de diálogo nuevo proyecto")</span><span class="sxs-lookup"><span data-stu-id="eb05d-176">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="eb05d-177">*Crear el cuadro de diálogo nuevo proyecto*</span><span class="sxs-lookup"><span data-stu-id="eb05d-177">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="eb05d-178">En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione el **básica** plantilla y asegúrese de que el **motor de vista** seleccionado es **Razor**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-178">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="eb05d-179">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-179">Click **OK**.</span></span>

    <span data-ttu-id="eb05d-180">![Nuevo cuadro de diálogo de proyecto de ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "nuevo cuadro de diálogo de proyecto de ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="eb05d-180">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="eb05d-181">*Nuevo cuadro de diálogo de proyecto de ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="eb05d-181">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="eb05d-182">Tarea 2: explorar la estructura de solución</span><span class="sxs-lookup"><span data-stu-id="eb05d-182">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="eb05d-183">El marco de ASP.NET MVC incluye una plantilla de proyecto de Visual Studio que le permite crear aplicaciones Web compatibles con el patrón de MVC.</span><span class="sxs-lookup"><span data-stu-id="eb05d-183">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="eb05d-184">Esta plantilla crea una nueva aplicación Web de ASP.NET MVC con las carpetas necesarias, las plantillas de elemento y las entradas del archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="eb05d-184">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="eb05d-185">En esta tarea, examinará la estructura de solución para comprender los elementos que están implicados y sus relaciones.</span><span class="sxs-lookup"><span data-stu-id="eb05d-185">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="eb05d-186">Las siguientes carpetas se incluyen en toda la aplicación de ASP.NET MVC, porque el marco de ASP.NET MVC predeterminada usa un &quot;Convención sobre configuración&quot; enfoque y hace algunas suposiciones predeterminadas basándose en la nomenclatura de carpeta convenciones.</span><span class="sxs-lookup"><span data-stu-id="eb05d-186">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="eb05d-187">Una vez creado el proyecto, revise la estructura de carpetas que se ha creado en el Explorador de soluciones en el lado derecho:</span><span class="sxs-lookup"><span data-stu-id="eb05d-187">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="eb05d-188">![Estructura de carpetas de MVC de ASP.NET en el Explorador de soluciones](aspnet-mvc-4-fundamentals/_static/image4.png "estructura de carpetas de MVC de ASP.NET en el Explorador de soluciones")</span><span class="sxs-lookup"><span data-stu-id="eb05d-188">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="eb05d-189">*Estructura de carpetas de MVC de ASP.NET en el Explorador de soluciones*</span><span class="sxs-lookup"><span data-stu-id="eb05d-189">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

   1. <span data-ttu-id="eb05d-190">**Los controladores**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-190">**Controllers**.</span></span> <span data-ttu-id="eb05d-191">Esta carpeta contiene las clases del controlador.</span><span class="sxs-lookup"><span data-stu-id="eb05d-191">This folder will contain the controller classes.</span></span> <span data-ttu-id="eb05d-192">En una aplicación basada en MVC, los controladores son responsables de controlar la interacción del usuario final, manipular el modelo y en última instancia, elija una vista para representar la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="eb05d-192">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

       > [!NOTE]
       > <span data-ttu-id="eb05d-193">El marco MVC requiere que los nombres de todos los controladores terminen con &quot;controlador&quot;-por ejemplo, HomeController, LoginController o ProductController.</span><span class="sxs-lookup"><span data-stu-id="eb05d-193">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
   2. <span data-ttu-id="eb05d-194">**Modelos**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-194">**Models**.</span></span> <span data-ttu-id="eb05d-195">Esta carpeta se proporciona para las clases que representan el modelo de aplicación para la aplicación Web de MVC.</span><span class="sxs-lookup"><span data-stu-id="eb05d-195">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="eb05d-196">Normalmente, esto incluye código que define los objetos y la lógica para interactuar con el almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="eb05d-196">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="eb05d-197">Normalmente, los objetos del modelo real será en bibliotecas de clases independientes.</span><span class="sxs-lookup"><span data-stu-id="eb05d-197">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="eb05d-198">Sin embargo, cuando se crea una nueva aplicación, se podría incluir clases y, a continuación, muévalos a bibliotecas de clases independientes en un momento posterior del ciclo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="eb05d-198">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
   3. <span data-ttu-id="eb05d-199">**Vistas**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-199">**Views**.</span></span> <span data-ttu-id="eb05d-200">Esta carpeta es la ubicación recomendada para las vistas, los componentes responsables de mostrar la interfaz de usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb05d-200">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="eb05d-201">Las vistas usan archivos .aspx, .ascx, .cshtml y. master, además de otros archivos relacionados con la representación de vistas.</span><span class="sxs-lookup"><span data-stu-id="eb05d-201">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="eb05d-202">Carpeta Views contiene una carpeta para cada controlador; la carpeta se denomina con el prefijo del nombre del controlador.</span><span class="sxs-lookup"><span data-stu-id="eb05d-202">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="eb05d-203">Por ejemplo, si tiene un controlador denominado **HomeController**, la carpeta Views contiene una carpeta denominada Home.</span><span class="sxs-lookup"><span data-stu-id="eb05d-203">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="eb05d-204">De forma predeterminada, cuando el marco de MVC de ASP.NET carga una vista, busca un archivo .aspx con el nombre de la vista solicitada en la carpeta Views\controllerName (**vistas [Nombredelcontrolador] [Action] .aspx**) o (**vistas [Nombredelcontrolador] [Action] .cshtml**) para las vistas de Razor.</span><span class="sxs-lookup"><span data-stu-id="eb05d-204">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

      > [!NOTE]
      > <span data-ttu-id="eb05d-205">Además de las carpetas enumeradas anteriormente, una aplicación Web MVC usa el **Global.asax** tiene como valor predeterminado de archivo para establecer el enrutamiento global de direcciones URL, y utiliza el **Web.config** archivo para configurar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb05d-205">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="eb05d-206">Tarea 3: agregar un HomeController</span><span class="sxs-lookup"><span data-stu-id="eb05d-206">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="eb05d-207">En las aplicaciones de ASP.NET que no utilizan el marco de MVC, interacción del usuario se organiza alrededor de las páginas y en torno a generar y controlar eventos desde esas páginas.</span><span class="sxs-lookup"><span data-stu-id="eb05d-207">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="eb05d-208">En cambio, interacción del usuario con aplicaciones ASP.NET MVC se organiza en torno a los controladores y sus métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="eb05d-208">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="eb05d-209">Por otro lado, el marco de MVC de ASP.NET asigna las direcciones URL a las clases que se conocen como controladores.</span><span class="sxs-lookup"><span data-stu-id="eb05d-209">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="eb05d-210">Los controladores de procesar las solicitudes entrantes, controlar la entrada del usuario y las interacciones, ejecutar la lógica de aplicación adecuada y determinar la respuesta enviada al cliente (Mostrar HTML, descargar un archivo, redirigir a una dirección URL diferente, etcetera.).</span><span class="sxs-lookup"><span data-stu-id="eb05d-210">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="eb05d-211">En el caso de mostrar HTML, una clase de controlador llama normalmente a un componente de vista independiente para generar el marcado HTML para la solicitud.</span><span class="sxs-lookup"><span data-stu-id="eb05d-211">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="eb05d-212">En una aplicación de MVC, la vista solo muestra información; el controlador controla y responde a la interacción y los datos que introducen los usuarios.</span><span class="sxs-lookup"><span data-stu-id="eb05d-212">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="eb05d-213">En esta tarea, agregará una clase de controlador que controlará las direcciones URL a la página principal del sitio de Music Store.</span><span class="sxs-lookup"><span data-stu-id="eb05d-213">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="eb05d-214">Haga clic en **controladores** carpeta en el Explorador de soluciones, seleccione **agregar** y, a continuación, **controlador** comando:</span><span class="sxs-lookup"><span data-stu-id="eb05d-214">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="eb05d-215">![Agregar un comando controlador](aspnet-mvc-4-fundamentals/_static/image5.png "agregar un comando de controlador")</span><span class="sxs-lookup"><span data-stu-id="eb05d-215">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="eb05d-216">*Agregar controlador (comando)*</span><span class="sxs-lookup"><span data-stu-id="eb05d-216">*Add Controller Command*</span></span>
2. <span data-ttu-id="eb05d-217">El **Agregar controlador** aparece el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="eb05d-217">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="eb05d-218">Asigne al controlador el *HomeController* y presione **agregar**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-218">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="eb05d-219">![Agregar cuadro de diálogo controlador](aspnet-mvc-4-fundamentals/_static/image6.png "Agregar cuadro de diálogo de controlador")</span><span class="sxs-lookup"><span data-stu-id="eb05d-219">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="eb05d-220">*Agregar cuadro de diálogo de controlador*</span><span class="sxs-lookup"><span data-stu-id="eb05d-220">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="eb05d-221">El archivo **HomeController.cs** se crea en el **controladores** carpeta.</span><span class="sxs-lookup"><span data-stu-id="eb05d-221">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="eb05d-222">Para poder tener la **HomeController** devuelven una cadena en su acción del índice, reemplace el **índice** método con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="eb05d-222">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="eb05d-223">(Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex1 HomeController índice*)</span><span class="sxs-lookup"><span data-stu-id="eb05d-223">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="eb05d-224">Tarea 4: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="eb05d-224">Task 4 - Running the Application</span></span>

<span data-ttu-id="eb05d-225">En esta tarea, se pruebe la aplicación en un explorador web.</span><span class="sxs-lookup"><span data-stu-id="eb05d-225">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="eb05d-226">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb05d-226">Press **F5** to run the Application.</span></span> <span data-ttu-id="eb05d-227">El proyecto se compila y se inicia de servidor Web de IIS local.</span><span class="sxs-lookup"><span data-stu-id="eb05d-227">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="eb05d-228">El equipo local, servidor Web de IIS se abrirá automáticamente un explorador web que apunte a la dirección URL del servidor Web.</span><span class="sxs-lookup"><span data-stu-id="eb05d-228">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="eb05d-229">![Aplicación que se ejecuta en un explorador web](aspnet-mvc-4-fundamentals/_static/image7.png "aplicación que se ejecuta en un explorador web")</span><span class="sxs-lookup"><span data-stu-id="eb05d-229">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="eb05d-230">*Aplicación que se ejecuta en un explorador web*</span><span class="sxs-lookup"><span data-stu-id="eb05d-230">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="eb05d-231">El servidor Web de IIS local se ejecutará el sitio Web en un número de puerto libre aleatorio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-231">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="eb05d-232">En la ilustración anterior, se está ejecutando en el sitio `http://localhost:50103/`, por lo que está utilizando el puerto 50103.</span><span class="sxs-lookup"><span data-stu-id="eb05d-232">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="eb05d-233">El número de puerto puede variar.</span><span class="sxs-lookup"><span data-stu-id="eb05d-233">Your port number may vary.</span></span>
2. <span data-ttu-id="eb05d-234">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="eb05d-234">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="eb05d-235">Ejercicio 2: Creación de un controlador</span><span class="sxs-lookup"><span data-stu-id="eb05d-235">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="eb05d-236">En este ejercicio, obtendrá información sobre cómo actualizar el controlador para implementar funciones sencillas de la aplicación Music Store.</span><span class="sxs-lookup"><span data-stu-id="eb05d-236">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="eb05d-237">Ese controlador definirá métodos de acción para cada una de las solicitudes específicas siguientes:</span><span class="sxs-lookup"><span data-stu-id="eb05d-237">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="eb05d-238">Una página de listado de los géneros de música en el Store música</span><span class="sxs-lookup"><span data-stu-id="eb05d-238">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="eb05d-239">Una página de exploración que enumera todos los álbumes de música de un género determinado</span><span class="sxs-lookup"><span data-stu-id="eb05d-239">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="eb05d-240">Una página de detalles que muestra información sobre un álbum de música específicos</span><span class="sxs-lookup"><span data-stu-id="eb05d-240">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="eb05d-241">Para el ámbito de este ejercicio, esas acciones simplemente devolverá una cadena en este momento.</span><span class="sxs-lookup"><span data-stu-id="eb05d-241">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="eb05d-242">Tarea 1: agregar una nueva clase StoreController</span><span class="sxs-lookup"><span data-stu-id="eb05d-242">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="eb05d-243">En esta tarea, agregará un nuevo controlador.</span><span class="sxs-lookup"><span data-stu-id="eb05d-243">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="eb05d-244">Si no está abierto, inicie **VS Express 2012 para Web**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-244">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="eb05d-245">En el **archivo** menú, elija **Abrir proyecto**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-245">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="eb05d-246">En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex02 CreatingAController\Begin**, seleccione **Begin.sln** y haga clic en **abierto**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-246">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="eb05d-247">Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="eb05d-247">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="eb05d-248">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="eb05d-248">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="eb05d-249">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-249">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="eb05d-250">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="eb05d-250">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="eb05d-251">Por último, compile la solución haciendo **compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-251">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="eb05d-252">Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="eb05d-252">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="eb05d-253">Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="eb05d-253">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="eb05d-254">Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-254">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="eb05d-255">Agregue el nuevo controlador.</span><span class="sxs-lookup"><span data-stu-id="eb05d-255">Add the new controller.</span></span> <span data-ttu-id="eb05d-256">Para ello, haga clic en el **controladores** carpeta en el Explorador de soluciones, seleccione **agregar** y, a continuación, el **controlador** comando.</span><span class="sxs-lookup"><span data-stu-id="eb05d-256">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="eb05d-257">Cambiar el **nombre del controlador** a *StoreController*y haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-257">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="eb05d-258">![Agregar cuadro de diálogo controlador](aspnet-mvc-4-fundamentals/_static/image8.png "Agregar cuadro de diálogo de controlador")</span><span class="sxs-lookup"><span data-stu-id="eb05d-258">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="eb05d-259">*Agregar cuadro de diálogo de controlador*</span><span class="sxs-lookup"><span data-stu-id="eb05d-259">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="eb05d-260">Tarea 2: modificar las acciones de StoreController</span><span class="sxs-lookup"><span data-stu-id="eb05d-260">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="eb05d-261">En esta tarea, modificará los métodos del controlador que se denominan **acciones**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-261">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="eb05d-262">Las acciones son responsables de controlar las solicitudes de dirección URL y determinar el contenido que se debe enviar en el explorador o el usuario que invocó la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="eb05d-262">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="eb05d-263">El **StoreController** clase ya tiene un **índice** método.</span><span class="sxs-lookup"><span data-stu-id="eb05d-263">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="eb05d-264">Se usará más adelante en este laboratorio para implementar la página que enumera todos los géneros de la tienda de música.</span><span class="sxs-lookup"><span data-stu-id="eb05d-264">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="eb05d-265">Por ahora, solo tiene que reemplazar el **índice** método con el código siguiente que devuelve una cadena &quot;Hola desde Store.Index()&quot;:</span><span class="sxs-lookup"><span data-stu-id="eb05d-265">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="eb05d-266">(Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex2 StoreController índice*)</span><span class="sxs-lookup"><span data-stu-id="eb05d-266">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="eb05d-267">Agregar **examinar** y **detalles** métodos.</span><span class="sxs-lookup"><span data-stu-id="eb05d-267">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="eb05d-268">Para ello, agregue el código siguiente a la **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="eb05d-268">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="eb05d-269">(Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="eb05d-269">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="eb05d-270">Tarea 3: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="eb05d-270">Task 3 - Running the Application</span></span>

<span data-ttu-id="eb05d-271">En esta tarea, se pruebe la aplicación en un explorador web.</span><span class="sxs-lookup"><span data-stu-id="eb05d-271">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="eb05d-272">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb05d-272">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="eb05d-273">Se inicia el proyecto en el **inicio** página.</span><span class="sxs-lookup"><span data-stu-id="eb05d-273">The project starts in the **Home** page.</span></span> <span data-ttu-id="eb05d-274">Cambie la dirección URL para comprobar la implementación de la acción.</span><span class="sxs-lookup"><span data-stu-id="eb05d-274">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="eb05d-275">**/ Store**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-275">**/Store**.</span></span> <span data-ttu-id="eb05d-276">Verá  **&quot;Hola desde Store.Index()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-276">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="eb05d-277">**/ Store/examinar**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-277">**/Store/Browse**.</span></span> <span data-ttu-id="eb05d-278">Verá  **&quot;Hola desde Store.Browse()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-278">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="eb05d-279">**/ Store/detalles**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-279">**/Store/Details**.</span></span> <span data-ttu-id="eb05d-280">Verá  **&quot;Hola desde Store.Details()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-280">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="eb05d-281">![Exploración StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "StoreBrowse de exploración")</span><span class="sxs-lookup"><span data-stu-id="eb05d-281">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="eb05d-282">*Exploración /Store/Browse*</span><span class="sxs-lookup"><span data-stu-id="eb05d-282">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="eb05d-283">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="eb05d-283">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="eb05d-284">Ejercicio 3: Pasar parámetros a un controlador</span><span class="sxs-lookup"><span data-stu-id="eb05d-284">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="eb05d-285">Hasta ahora, ha estado devolviendo las cadenas constantes desde los controladores.</span><span class="sxs-lookup"><span data-stu-id="eb05d-285">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="eb05d-286">En este ejercicio obtendrá información sobre cómo pasar parámetros a un controlador utilizando la dirección URL y la cadena de consulta y, a continuación, cómo hacer que el método acciones responder con texto en el explorador.</span><span class="sxs-lookup"><span data-stu-id="eb05d-286">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="eb05d-287">Tarea 1: Agregar parámetro de género StoreController</span><span class="sxs-lookup"><span data-stu-id="eb05d-287">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="eb05d-288">En esta tarea, utilizará el **querystring** enviar parámetros a la **examinar** método de acción en el **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-288">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="eb05d-289">Si no está abierto, inicie **VS Express para Web**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-289">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="eb05d-290">En el **archivo** menú, elija **Abrir proyecto**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-290">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="eb05d-291">En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex03 PassingParametersToAController\Begin**, seleccione **Begin.sln** y haga clic en **abierto**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-291">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="eb05d-292">Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="eb05d-292">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="eb05d-293">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="eb05d-293">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="eb05d-294">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-294">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="eb05d-295">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="eb05d-295">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="eb05d-296">Por último, compile la solución haciendo **compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-296">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="eb05d-297">Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="eb05d-297">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="eb05d-298">Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="eb05d-298">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="eb05d-299">Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-299">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="eb05d-300">Abra **StoreController** clase.</span><span class="sxs-lookup"><span data-stu-id="eb05d-300">Open **StoreController** class.</span></span> <span data-ttu-id="eb05d-301">Para ello, en el **el Explorador de soluciones**, expanda el **controladores** carpeta y haga doble clic en **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-301">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="eb05d-302">Cambiar el **examinar** método, agregar un parámetro de cadena a la solicitud de un género concreto.</span><span class="sxs-lookup"><span data-stu-id="eb05d-302">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="eb05d-303">ASP.NET MVC se automáticamente pasar cualquier cadena de consulta o parámetros con nombre de envío de formulario **género** a este método de acción cuando se invoca.</span><span class="sxs-lookup"><span data-stu-id="eb05d-303">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="eb05d-304">Para ello, reemplace el **examinar** método con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="eb05d-304">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="eb05d-305">(Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="eb05d-305">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="eb05d-306">¿Usa el **HttpUtility.HtmlEncode** método de utilidad para impide que los usuarios de inyección de Javascript en la vista con un vínculo como   **/Store/examinar? Género =&lt;script&gt;Window.Location = "[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-306">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
> 
> <span data-ttu-id="eb05d-307">Para obtener más información, visite [este artículo de msdn](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span><span class="sxs-lookup"><span data-stu-id="eb05d-307">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="eb05d-308">Tarea 2: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="eb05d-308">Task 2 - Running the Application</span></span>

<span data-ttu-id="eb05d-309">En esta tarea, va a probar la aplicación en un explorador web y usar el **género** parámetro.</span><span class="sxs-lookup"><span data-stu-id="eb05d-309">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="eb05d-310">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb05d-310">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="eb05d-311">Se inicia el proyecto en el **inicio** página.</span><span class="sxs-lookup"><span data-stu-id="eb05d-311">The project starts in the **Home** page.</span></span> <span data-ttu-id="eb05d-312">¿Cambiar la dirección URL de   */Store/examinar? Género = Disco* para comprobar que la acción recibe el parámetro de género.</span><span class="sxs-lookup"><span data-stu-id="eb05d-312">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="eb05d-313">![Exploración StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "exploración StoreBrowseGenre = Disco")</span><span class="sxs-lookup"><span data-stu-id="eb05d-313">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="eb05d-314">*¿Exploración /Store/Browse? Género = Disco*</span><span class="sxs-lookup"><span data-stu-id="eb05d-314">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="eb05d-315">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="eb05d-315">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="eb05d-316">Tarea 3: agregar un parámetro de identificador incrustado en la dirección URL</span><span class="sxs-lookup"><span data-stu-id="eb05d-316">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="eb05d-317">En esta tarea, utilizará el **URL** para pasar un **Id. de** parámetro para el **detalles** método de acción de la **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-317">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="eb05d-318">Predeterminado de ASP.NET MVC convención de enrutamiento es tratar el segmento de una dirección URL después del nombre del método de acción como un parámetro denominado **Id**. Si el método de acción tiene el parámetro denominado Id, a continuación, ASP.NET MVC automáticamente pasará el segmento de dirección URL para usted como un parámetro.</span><span class="sxs-lookup"><span data-stu-id="eb05d-318">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="eb05d-319">En la dirección URL **Store/detalles/5**, **Id** se interpretará como **5**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-319">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="eb05d-320">Cambio la **detalles** método de la **StoreController**, agregar un **int** parámetro llamado **id**. Para ello, reemplace el **detalles** método con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="eb05d-320">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="eb05d-321">(Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="eb05d-321">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="eb05d-322">Tarea 4: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="eb05d-322">Task 4 - Running the Application</span></span>

<span data-ttu-id="eb05d-323">En esta tarea, va a probar la aplicación en un explorador web y usar el **Id** parámetro.</span><span class="sxs-lookup"><span data-stu-id="eb05d-323">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="eb05d-324">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb05d-324">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="eb05d-325">Se inicia el proyecto en el **inicio** página.</span><span class="sxs-lookup"><span data-stu-id="eb05d-325">The project starts in the **Home** page.</span></span> <span data-ttu-id="eb05d-326">Cambiar la dirección URL de */Store/Details/5* para comprobar que la acción recibe el parámetro id.</span><span class="sxs-lookup"><span data-stu-id="eb05d-326">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="eb05d-327">![Exploración StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "StoreDetails5 de exploración")</span><span class="sxs-lookup"><span data-stu-id="eb05d-327">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="eb05d-328">*Exploración /Store/Details/5*</span><span class="sxs-lookup"><span data-stu-id="eb05d-328">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="eb05d-329">Ejercicio 4: Crear una vista</span><span class="sxs-lookup"><span data-stu-id="eb05d-329">Exercise 4: Creating a View</span></span>

<span data-ttu-id="eb05d-330">Hasta ahora ha sido devolver cadenas acciones de controlador.</span><span class="sxs-lookup"><span data-stu-id="eb05d-330">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="eb05d-331">Aunque es una forma útil de comprender cómo funcionan los controladores, no es, cómo se compilan las aplicaciones Web reales.</span><span class="sxs-lookup"><span data-stu-id="eb05d-331">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="eb05d-332">Las vistas son componentes que proporcionan un mejor enfoque para generar HTML al explorador con el uso de archivos de plantilla.</span><span class="sxs-lookup"><span data-stu-id="eb05d-332">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="eb05d-333">En este ejercicio obtendrá información sobre cómo agregar una página principal de diseño para configurar una plantilla para el contenido HTML común, una hoja de estilos para mejorar la apariencia del sitio y, por último, una plantilla de vista para habilitar HomeController devolver HTML.</span><span class="sxs-lookup"><span data-stu-id="eb05d-333">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a><span data-ttu-id="eb05d-334">Tarea 1: modificación del archivo \_layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="eb05d-334">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="eb05d-335">El archivo **~/Views/Shared/\_layout.cshtml** permite configurar una plantilla para HTML común usar a través de todo el sitio Web.</span><span class="sxs-lookup"><span data-stu-id="eb05d-335">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="eb05d-336">En esta tarea agregará una página principal de diseño con un encabezado común con vínculos al área de página principal y Store.</span><span class="sxs-lookup"><span data-stu-id="eb05d-336">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="eb05d-337">Si no está abierto, inicie **VS Express para Web**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-337">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="eb05d-338">En el **archivo** menú, elija **Abrir proyecto**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-338">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="eb05d-339">En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex04 CreatingAView\Begin**, seleccione **Begin.sln** y haga clic en **abierto**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-339">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="eb05d-340">Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="eb05d-340">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="eb05d-341">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="eb05d-341">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="eb05d-342">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-342">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="eb05d-343">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="eb05d-343">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="eb05d-344">Por último, compile la solución haciendo **compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-344">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="eb05d-345">Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="eb05d-345">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="eb05d-346">Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="eb05d-346">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="eb05d-347">Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-347">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="eb05d-348">El archivo  <strong>\_layout.cshtml</strong> contiene el diseño del contenedor HTML para todas las páginas del sitio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-348">The file <strong>\_layout.cshtml</strong> contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="eb05d-349">Incluye el <strong>&lt;html&gt;</strong> (elemento) para la respuesta HTML, así como el <strong>&lt;head&gt;</strong> y <strong>&lt;cuerpo&gt;</strong> elementos.</span><span class="sxs-lookup"><span data-stu-id="eb05d-349">It includes the <strong>&lt;html&gt;</strong> element for the HTML response, as well as the <strong>&lt;head&gt;</strong> and <strong>&lt;body&gt;</strong> elements.</span></span> <span data-ttu-id="eb05d-350"><strong>@RenderBody()</strong> dentro del HTML body identificar regiones esa vista plantillas podrá rellenar con contenido dinámico.</span><span class="sxs-lookup"><span data-stu-id="eb05d-350"><strong>@RenderBody()</strong> within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
   <span data-ttu-id="eb05d-351">(C#)</span><span class="sxs-lookup"><span data-stu-id="eb05d-351">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="eb05d-352">Agregar un encabezado común con vínculos al área de página principal y Store de todas las páginas del sitio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-352">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="eb05d-353">Para ello, agregue el código siguiente &lt;cuerpo&gt; instrucción.</span><span class="sxs-lookup"><span data-stu-id="eb05d-353">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
   <span data-ttu-id="eb05d-354">(C#)</span><span class="sxs-lookup"><span data-stu-id="eb05d-354">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="eb05d-355">Incluir un elemento div para representar la sección de cuerpo de cada página.</span><span class="sxs-lookup"><span data-stu-id="eb05d-355">Include a div to render the body section of each page.</span></span> <span data-ttu-id="eb05d-356">Reemplace  <strong>@RenderBody()</strong> con el siguiente código higlighted: (C#)</span><span class="sxs-lookup"><span data-stu-id="eb05d-356">Replace <strong>@RenderBody()</strong> with the following higlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="eb05d-357">¿Sabía que?</span><span class="sxs-lookup"><span data-stu-id="eb05d-357">Did you know?</span></span> <span data-ttu-id="eb05d-358">Visual Studio 2012 tiene fragmentos de código que facilitan agregar el código usado frecuentemente en HTML, archivos de código y mucho más!</span><span class="sxs-lookup"><span data-stu-id="eb05d-358">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="eb05d-359">Probarlo escribiendo **&lt;div&gt;** y presionando **ficha** dos veces para insertar una completa **div** etiqueta.</span><span class="sxs-lookup"><span data-stu-id="eb05d-359">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="eb05d-360">Tarea 2: Agregar hoja de estilos CSS</span><span class="sxs-lookup"><span data-stu-id="eb05d-360">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="eb05d-361">La plantilla proyecto vacío incluye un archivo CSS muy simplificado que solo incluye los estilos usados para mostrar formularios básicos y los mensajes de validación.</span><span class="sxs-lookup"><span data-stu-id="eb05d-361">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="eb05d-362">Usará adicionales CSS e imágenes (potencialmente proporcionadas un diseñador) con el fin de mejorar la apariencia del sitio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-362">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="eb05d-363">En esta tarea, agregará una hoja de estilos CSS para definir los estilos del sitio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-363">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="eb05d-364">El archivo CSS y las imágenes se incluyen en el **Source\Assets\Content** carpeta de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-364">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="eb05d-365">Con el fin de agregarlos a la aplicación, arrastre su contenido desde un **Windows Explorer** ventana en la **el Explorador de soluciones** en Visual Studio Express para Web, como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="eb05d-365">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="eb05d-366">![Arrastrar el contenido de estilo](aspnet-mvc-4-fundamentals/_static/image12.png "arrastrar el contenido de estilo")</span><span class="sxs-lookup"><span data-stu-id="eb05d-366">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="eb05d-367">*Arrastrar el contenido de estilo*</span><span class="sxs-lookup"><span data-stu-id="eb05d-367">*Dragging style contents*</span></span>
2. <span data-ttu-id="eb05d-368">Un cuadro de diálogo de advertencia aparecerá, le pide confirmación reemplazar **Site.css** archivo y algunas imágenes existentes.</span><span class="sxs-lookup"><span data-stu-id="eb05d-368">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="eb05d-369">Comprobar **aplicar a todos los elementos** y haga clic en **Sí**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-369">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="eb05d-370">Tarea 3: adición de una plantilla de vista</span><span class="sxs-lookup"><span data-stu-id="eb05d-370">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="eb05d-371">En esta tarea, agregará una plantilla de vista para generar la respuesta HTML que va a usar la página principal de diseño y CSS que se agregan en este ejercicio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-371">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="eb05d-372">Para usar una plantilla de vista al examinar la página principal del sitio, primero debe indicar que en lugar de devolver una cadena, el **HomeController índice** método devolverá un **vista**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-372">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="eb05d-373">Abra **HomeController** clase y cambie su **índice** método devuelva un **ActionResult**, y que devuelva **View()**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-373">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="eb05d-374">(Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex4 HomeController índice*)</span><span class="sxs-lookup"><span data-stu-id="eb05d-374">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="eb05d-375">Ahora, deberá agregar una plantilla de vista adecuada.</span><span class="sxs-lookup"><span data-stu-id="eb05d-375">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="eb05d-376">Para ello, **haga** dentro de la **índice** método de acción y seleccione **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-376">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="eb05d-377">Esto le llevará a la **agregar vista** cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="eb05d-377">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="eb05d-378">![Agregar una vista desde dentro del método de índice](aspnet-mvc-4-fundamentals/_static/image13.png "agregar una vista desde dentro del método de índice")</span><span class="sxs-lookup"><span data-stu-id="eb05d-378">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="eb05d-379">*Agregar una vista desde dentro del método de índice*</span><span class="sxs-lookup"><span data-stu-id="eb05d-379">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="eb05d-380">El **agregar vista** aparecerá el cuadro de diálogo para generar un archivo de plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="eb05d-380">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="eb05d-381">De forma predeterminada, este cuadro de diálogo rellena previamente el nombre de la plantilla de vista para que coincida con el método de acción que va a usar.</span><span class="sxs-lookup"><span data-stu-id="eb05d-381">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="eb05d-382">Ya ha usado el **agregar vista** menú contextual dentro el **índice** método de acción en HomeController, la **agregar vista** cuadro de diálogo tiene el índice como el nombre de la vista predeterminada.</span><span class="sxs-lookup"><span data-stu-id="eb05d-382">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="eb05d-383">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-383">Click **Add**.</span></span>

    <span data-ttu-id="eb05d-384">![Agregar cuadro de diálogo Vista](aspnet-mvc-4-fundamentals/_static/image14.png "diálogo Agregar vista")</span><span class="sxs-lookup"><span data-stu-id="eb05d-384">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="eb05d-385">*Agregar cuadro de diálogo de vista*</span><span class="sxs-lookup"><span data-stu-id="eb05d-385">*Add View Dialog*</span></span>
4. <span data-ttu-id="eb05d-386">Visual Studio genera un **Index.cshtml** ver plantilla dentro de la **Views\Home** carpeta y, a continuación, lo abre.</span><span class="sxs-lookup"><span data-stu-id="eb05d-386">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="eb05d-387">![Inicio de la vista de índice creada](aspnet-mvc-4-fundamentals/_static/image15.png "vista de índice de inicio creada")</span><span class="sxs-lookup"><span data-stu-id="eb05d-387">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="eb05d-388">*Vista de índice principal creada*</span><span class="sxs-lookup"><span data-stu-id="eb05d-388">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="eb05d-389">nombre y la ubicación de la **Index.cshtml** archivo es relevante y sigue las convenciones de nomenclatura predeterminada ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="eb05d-389">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="eb05d-390">La carpeta \Views\**inicio** coincide con el nombre del controlador (**inicio** controlador).</span><span class="sxs-lookup"><span data-stu-id="eb05d-390">The folder \Views\**Home** matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="eb05d-391">El nombre de la plantilla de vista (**índice**), coincide con el método de acción de controlador que se va a mostrar la vista.</span><span class="sxs-lookup"><span data-stu-id="eb05d-391">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="eb05d-392">De este modo, ASP.NET MVC evita tener que especificar explícitamente el nombre o la ubicación de una plantilla de vista cuando se usa esta convención de nomenclatura para devolver una vista.</span><span class="sxs-lookup"><span data-stu-id="eb05d-392">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="eb05d-393">La plantilla de vista generada se basa en el  **\_layout.cshtml** plantilla definida anteriormente.</span><span class="sxs-lookup"><span data-stu-id="eb05d-393">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="eb05d-394">Actualizar la propiedad ViewBag.Title a **inicio**y cambiar el contenido principal en **se trata de la página principal**, tal y como se muestra en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="eb05d-394">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="eb05d-395">Seleccione **MvcMusicStore** proyecto en el Explorador de soluciones y presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb05d-395">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="eb05d-396">Tarea 4: verificación</span><span class="sxs-lookup"><span data-stu-id="eb05d-396">Task 4: Verification</span></span>

<span data-ttu-id="eb05d-397">Para comprobar que ha realizado correctamente todos los pasos en el ejercicio anterior, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="eb05d-397">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="eb05d-398">Con la aplicación se abre en un explorador, debe tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="eb05d-398">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="eb05d-399">Método de acción de índice de HomeController encuentra y muestra el **\Views\Home\Index.cshtml** ver plantilla, aunque el código llamado **devolver View()**, porque la plantilla de vista seguido el convención de nomenclatura estándar.</span><span class="sxs-lookup"><span data-stu-id="eb05d-399">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="eb05d-400">La página principal muestra el mensaje de bienvenida definido dentro de la **\Views\Home\Index.cshtml** plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="eb05d-400">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="eb05d-401">Está usando la página principal de la  **\_layout.cshtml** plantilla, por lo que se encuentra el mensaje de bienvenida del diseño del sitio estándar HTML.</span><span class="sxs-lookup"><span data-stu-id="eb05d-401">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="eb05d-402">![Mediante el LayoutPage definido y el estilo de vista de índice de inicio](aspnet-mvc-4-fundamentals/_static/image16.png "vista de índice de inicio con el estilo y LayoutPage definido")</span><span class="sxs-lookup"><span data-stu-id="eb05d-402">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="eb05d-403">*Vista de índice principal utilizando el estilo y LayoutPage definido*</span><span class="sxs-lookup"><span data-stu-id="eb05d-403">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="eb05d-404">Ejercicio 5: Crear un modelo de vista</span><span class="sxs-lookup"><span data-stu-id="eb05d-404">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="eb05d-405">Hasta ahora, realiza sus vistas Mostrar HTML codificados, pero, con el fin de crear aplicaciones web dinámicas, la plantilla de vista debería recibir información desde el controlador.</span><span class="sxs-lookup"><span data-stu-id="eb05d-405">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="eb05d-406">Una técnica común que se usará para ese propósito es el **ViewModel** patrón, que permite que un controlador empaquetar toda la información necesaria para generar la respuesta HTML adecuada.</span><span class="sxs-lookup"><span data-stu-id="eb05d-406">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="eb05d-407">En este ejercicio, obtendrá información sobre cómo crear una clase ViewModel y agregar las propiedades necesarias: el número de géneros en el almacén y una lista de los géneros.</span><span class="sxs-lookup"><span data-stu-id="eb05d-407">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="eb05d-408">También se actualizará el StoreController para usar el modelo de vista creada y, por último, creará una nueva plantilla de vista que muestra las propiedades mencionadas en la página.</span><span class="sxs-lookup"><span data-stu-id="eb05d-408">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="eb05d-409">Tarea 1: creación de una clase ViewModel</span><span class="sxs-lookup"><span data-stu-id="eb05d-409">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="eb05d-410">En esta tarea, creará una clase ViewModel que implementará el escenario de anuncio de género de Store.</span><span class="sxs-lookup"><span data-stu-id="eb05d-410">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="eb05d-411">Si no está abierto, inicie **VS Express para Web**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-411">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="eb05d-412">En el **archivo** menú, elija **Abrir proyecto**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-412">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="eb05d-413">En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex05 CreatingAViewModel\Begin**, seleccione **Begin.sln** y haga clic en **abierto**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-413">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="eb05d-414">Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="eb05d-414">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="eb05d-415">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="eb05d-415">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="eb05d-416">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-416">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="eb05d-417">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="eb05d-417">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="eb05d-418">Por último, compile la solución haciendo **compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-418">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="eb05d-419">Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="eb05d-419">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="eb05d-420">Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="eb05d-420">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="eb05d-421">Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-421">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="eb05d-422">Crear un **ViewModels** carpeta que contendrá el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="eb05d-422">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="eb05d-423">Para ello, haga clic en el nivel superior **MvcMusicStore** proyecto, seleccione **agregar** y, a continuación, **nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-423">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="eb05d-424">![Agregar una nueva carpeta](aspnet-mvc-4-fundamentals/_static/image17.png "agregando una nueva carpeta")</span><span class="sxs-lookup"><span data-stu-id="eb05d-424">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="eb05d-425">*Agregar una nueva carpeta*</span><span class="sxs-lookup"><span data-stu-id="eb05d-425">*Adding a new folder*</span></span>
4. <span data-ttu-id="eb05d-426">Nombre de la carpeta *ViewModels*.</span><span class="sxs-lookup"><span data-stu-id="eb05d-426">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="eb05d-427">![Carpeta ViewModels en el Explorador de soluciones](aspnet-mvc-4-fundamentals/_static/image18.png "carpeta ViewModels en el Explorador de soluciones")</span><span class="sxs-lookup"><span data-stu-id="eb05d-427">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="eb05d-428">*Carpeta ViewModels en el Explorador de soluciones*</span><span class="sxs-lookup"><span data-stu-id="eb05d-428">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="eb05d-429">Crear un **ViewModel** clase.</span><span class="sxs-lookup"><span data-stu-id="eb05d-429">Create a **ViewModel** class.</span></span> <span data-ttu-id="eb05d-430">Para ello, haga doble clic en el **ViewModels** carpeta creado recientemente, seleccione **agregar** y, a continuación, **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-430">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="eb05d-431">En **código**, elija el **clase** de elemento y asigne el nombre *StoreIndexViewModel.cs*, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-431">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="eb05d-432">![Agregar una nueva clase](aspnet-mvc-4-fundamentals/_static/image19.png "agregando una nueva clase")</span><span class="sxs-lookup"><span data-stu-id="eb05d-432">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="eb05d-433">*Agregar una nueva clase*</span><span class="sxs-lookup"><span data-stu-id="eb05d-433">*Adding a new Class*</span></span>

    <span data-ttu-id="eb05d-434">![Crear clase StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel crear clase")</span><span class="sxs-lookup"><span data-stu-id="eb05d-434">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="eb05d-435">*Crear clase StoreIndexViewModel*</span><span class="sxs-lookup"><span data-stu-id="eb05d-435">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="eb05d-436">Tarea 2: agregar propiedades a la clase ViewModel</span><span class="sxs-lookup"><span data-stu-id="eb05d-436">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="eb05d-437">Hay dos parámetros que se pasan desde el StoreController a la plantilla de vista para generar la respuesta esperada de HTML: el número de géneros en el almacén y una lista de los géneros.</span><span class="sxs-lookup"><span data-stu-id="eb05d-437">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="eb05d-438">En esta tarea, agregará esas 2 propiedades para el **StoreIndexViewModel** clase: **NumberOfGenres** (entero) y **géneros** (una lista de cadenas).</span><span class="sxs-lookup"><span data-stu-id="eb05d-438">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="eb05d-439">Agregar **NumberOfGenres** y **géneros** propiedades para el **StoreIndexViewModel** clase.</span><span class="sxs-lookup"><span data-stu-id="eb05d-439">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="eb05d-440">Para ello, agregue las siguientes líneas de 2 a la definición de clase:</span><span class="sxs-lookup"><span data-stu-id="eb05d-440">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="eb05d-441">(Código de fragmento de código - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel propiedades*)</span><span class="sxs-lookup"><span data-stu-id="eb05d-441">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> <span data-ttu-id="eb05d-442">El **{get; configurar;}**  notación hace uso de C# característica propiedades implementadas automáticamente.</span><span class="sxs-lookup"><span data-stu-id="eb05d-442">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="eb05d-443">Proporciona las ventajas de una propiedad sin necesidad de nosotros declarar un campo de respaldo.</span><span class="sxs-lookup"><span data-stu-id="eb05d-443">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="eb05d-444">Tarea 3: actualización StoreController para usar el StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="eb05d-444">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="eb05d-445">El **StoreIndexViewModel** clase encapsula la información necesaria para pasar de **StoreController**del **índice** método a una plantilla de vista con el fin de generar una respuesta .</span><span class="sxs-lookup"><span data-stu-id="eb05d-445">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="eb05d-446">En esta tarea, actualizará la **StoreController** para usar el **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-446">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="eb05d-447">Abra **StoreController** clase.</span><span class="sxs-lookup"><span data-stu-id="eb05d-447">Open **StoreController** class.</span></span>

    <span data-ttu-id="eb05d-448">![Abriendo la clase StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "clase StoreController de apertura")</span><span class="sxs-lookup"><span data-stu-id="eb05d-448">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="eb05d-449">*Clase StoreController de apertura*</span><span class="sxs-lookup"><span data-stu-id="eb05d-449">*Opening StoreController class*</span></span>
2. <span data-ttu-id="eb05d-450">Para poder usar el **StoreIndexViewModel** clase desde el **StoreController**, agregue el siguiente espacio de nombres en la parte superior de la **StoreController** código:</span><span class="sxs-lookup"><span data-stu-id="eb05d-450">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="eb05d-451">(Código de fragmento de código - *ASP.NET MVC 4 Fundamentals - StoreIndexViewModel Ex5 mediante ViewModels*)</span><span class="sxs-lookup"><span data-stu-id="eb05d-451">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="eb05d-452">Cambiar el **StoreController**del **índice** método de acción para que se crea y rellena un **StoreIndexViewModel** de objetos y, a continuación, pasa a una plantilla de vista generar una respuesta HTML con él.</span><span class="sxs-lookup"><span data-stu-id="eb05d-452">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="eb05d-453">En el laboratorio &quot;acceso a datos y modelos de MVC de ASP.NET&quot; escribirá código que recupera la lista de géneros de almacén de una base de datos.</span><span class="sxs-lookup"><span data-stu-id="eb05d-453">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="eb05d-454">En el código siguiente, creará un **lista** de géneros datos ficticios que rellenarán el **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-454">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="eb05d-455">Después de crear y configurar el **StoreIndexViewModel** objeto, se pasará como un argumento para el **vista** método.</span><span class="sxs-lookup"><span data-stu-id="eb05d-455">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="eb05d-456">Esto indica que la plantilla de vista usará ese objeto para generar una respuesta HTML con él.</span><span class="sxs-lookup"><span data-stu-id="eb05d-456">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="eb05d-457">Reemplace el **índice** método con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="eb05d-457">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="eb05d-458">(Código de fragmento de código - *ASP.NET MVC 4 Fundamentals - método Ex5 StoreController Index*)</span><span class="sxs-lookup"><span data-stu-id="eb05d-458">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> <span data-ttu-id="eb05d-459">Si no está familiarizado con C#, se puede suponer que el uso **var** significa que el **viewModel** variable está enlazado en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="eb05d-459">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="eb05d-460">Que no es correcta, el compilador de C# usa según lo que asigne a la variable de la inferencia de tipos para determinar que **viewModel** es de tipo **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-460">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="eb05d-461">Además, mediante la compilación local **viewModel** variable como un **StoreIndexViewModel** escriba get comprobación en tiempo de compilación y la compatibilidad con el editor de código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-461">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="eb05d-462">Tarea 4: creación de una plantilla de vista que usa StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="eb05d-462">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="eb05d-463">En esta tarea, creará una plantilla de vista que va a usar un objeto StoreIndexViewModel pasado desde el controlador para mostrar una lista de géneros.</span><span class="sxs-lookup"><span data-stu-id="eb05d-463">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="eb05d-464">Antes de crear la nueva plantilla de vista, vamos a compilar el proyecto para que el **Agregar cuadro de diálogo de vista** conoce la **StoreIndexViewModel** clase.</span><span class="sxs-lookup"><span data-stu-id="eb05d-464">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="eb05d-465">Compile el proyecto seleccionando el **compilar** elemento de menú y, a continuación, **MvcMusicStore compilar**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-465">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="eb05d-466">![Compilar el proyecto](aspnet-mvc-4-fundamentals/_static/image22.png "compilar el proyecto")</span><span class="sxs-lookup"><span data-stu-id="eb05d-466">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="eb05d-467">*Compilar el proyecto*</span><span class="sxs-lookup"><span data-stu-id="eb05d-467">*Building the project*</span></span>
2. <span data-ttu-id="eb05d-468">Crear una nueva plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="eb05d-468">Create a new View template.</span></span> <span data-ttu-id="eb05d-469">Para ello, haga clic en el **índice** método y seleccione **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-469">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="eb05d-470">![Agregar una vista](aspnet-mvc-4-fundamentals/_static/image23.png "agregar una vista")</span><span class="sxs-lookup"><span data-stu-id="eb05d-470">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="eb05d-471">*Agregar una vista*</span><span class="sxs-lookup"><span data-stu-id="eb05d-471">*Adding a View*</span></span>
3. <span data-ttu-id="eb05d-472">Dado que el **Agregar cuadro de diálogo de vista** se invocó desde el **StoreController**, agregará la plantilla de vista de forma predeterminada en un **\Views\Store\Index.cshtml** archivo.</span><span class="sxs-lookup"><span data-stu-id="eb05d-472">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="eb05d-473">Compruebe el **crear una fuertemente tipados-vista** casilla de verificación y, a continuación, seleccione **StoreIndexViewModel** como el **clase modelo**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-473">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="eb05d-474">Además, asegúrese de que el motor de vista seleccionado es **Razor**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-474">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="eb05d-475">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-475">Click **Add**.</span></span>

    <span data-ttu-id="eb05d-476">![Agregar cuadro de diálogo Vista](aspnet-mvc-4-fundamentals/_static/image24.png "diálogo Agregar vista")</span><span class="sxs-lookup"><span data-stu-id="eb05d-476">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="eb05d-477">*Agregar cuadro de diálogo de vista*</span><span class="sxs-lookup"><span data-stu-id="eb05d-477">*Add View Dialog*</span></span>

    <span data-ttu-id="eb05d-478">El **\Views\Store\Index.cshtml** se crea y abre el archivo de plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="eb05d-478">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="eb05d-479">Según la información proporcionada a la **agregar vista** cuadro de diálogo en el último paso, la vista de plantilla se espera que sea un **StoreIndexViewModel** instancia como los datos que se va a usar para generar una respuesta HTML.</span><span class="sxs-lookup"><span data-stu-id="eb05d-479">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="eb05d-480">Observará que la plantilla hereda un `ViewPage<musicstore.viewmodels.storeindexviewmodel>` en C#.</span><span class="sxs-lookup"><span data-stu-id="eb05d-480">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="eb05d-481">Tarea 5: actualización de la plantilla de vista</span><span class="sxs-lookup"><span data-stu-id="eb05d-481">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="eb05d-482">En esta tarea, actualizará la plantilla de vista que se creó en la última tarea para recuperar el número de géneros y sus nombres dentro de la página.</span><span class="sxs-lookup"><span data-stu-id="eb05d-482">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="eb05d-483">Usará la sintaxis @ (suele denominarse &quot;fragmentos de código&quot;) para ejecutar código dentro de la plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="eb05d-483">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>

1. <span data-ttu-id="eb05d-484">En el **Index.cshtml** archivo, dentro el **Store** carpeta, reemplace el código por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="eb05d-484">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. <span data-ttu-id="eb05d-485">Bucle a través de la lista de género en **StoreIndexViewModel** y crear un elemento HTML **&lt;ul&gt;** lista mediante un **foreach** bucle.</span><span class="sxs-lookup"><span data-stu-id="eb05d-485">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
   <span data-ttu-id="eb05d-486">(C#)</span><span class="sxs-lookup"><span data-stu-id="eb05d-486">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="eb05d-487">Presione **F5** para ejecutar la aplicación y examinar **/Store**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-487">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="eb05d-488">Verá la lista de géneros pasa el **StoreIndexViewModel** objeto desde el **StoreController** a la plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="eb05d-488">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="eb05d-489">![Vista que muestra una lista de géneros](aspnet-mvc-4-fundamentals/_static/image26.png "vista que muestra una lista de géneros")</span><span class="sxs-lookup"><span data-stu-id="eb05d-489">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="eb05d-490">*Vista que muestra una lista de géneros*</span><span class="sxs-lookup"><span data-stu-id="eb05d-490">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="eb05d-491">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="eb05d-491">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="eb05d-492">Ejercicio 6: Uso de parámetros en la vista</span><span class="sxs-lookup"><span data-stu-id="eb05d-492">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="eb05d-493">En el ejercicio 3 ha aprendido cómo pasar parámetros al controlador.</span><span class="sxs-lookup"><span data-stu-id="eb05d-493">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="eb05d-494">En este ejercicio, obtendrá información sobre cómo usar estos parámetros en la plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="eb05d-494">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="eb05d-495">Para ello, le presentaremos primero a las clases de modelo que le ayudará a administrar la lógica de datos y de dominio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-495">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="eb05d-496">Además, obtendrá información sobre cómo crear vínculos a páginas dentro de la aplicación de ASP.NET MVC sin preocuparse de cosas como las rutas de acceso de dirección URL de codificación.</span><span class="sxs-lookup"><span data-stu-id="eb05d-496">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="eb05d-497">Tarea 1: agregar clases de modelo</span><span class="sxs-lookup"><span data-stu-id="eb05d-497">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="eb05d-498">A diferencia de ViewModel, que se crea para pasar información del controlador a la vista, se crean clases de modelo que contiene y administra la lógica de datos y el dominio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-498">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="eb05d-499">En esta tarea agregará dos clases de modelo para representar estos conceptos: **género** y **álbum**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-499">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="eb05d-500">Si no está abierto, inicie **VS Express para Web**</span><span class="sxs-lookup"><span data-stu-id="eb05d-500">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="eb05d-501">En el **archivo** menú, elija **Abrir proyecto**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-501">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="eb05d-502">En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex06 UsingParametersInView\Begin**, seleccione **Begin.sln** y haga clic en **abierto**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-502">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="eb05d-503">Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="eb05d-503">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="eb05d-504">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="eb05d-504">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="eb05d-505">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-505">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="eb05d-506">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="eb05d-506">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="eb05d-507">Por último, compile la solución haciendo **compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-507">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="eb05d-508">Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="eb05d-508">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="eb05d-509">Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="eb05d-509">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="eb05d-510">Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-510">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="eb05d-511">Agregar un **género** clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="eb05d-511">Add a **Genre** Model class.</span></span> <span data-ttu-id="eb05d-512">Para ello, haga clic en el **modelos** carpeta en el **el Explorador de soluciones**, seleccione **agregar** y, a continuación, el **nuevo elemento** opción.</span><span class="sxs-lookup"><span data-stu-id="eb05d-512">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="eb05d-513">En **código**, elija el **clase** de elemento y asigne el nombre *Genre.cs*, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-513">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="eb05d-514">![Agregar una clase](aspnet-mvc-4-fundamentals/_static/image27.png "agregar una clase")</span><span class="sxs-lookup"><span data-stu-id="eb05d-514">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="eb05d-515">*Agregar un nuevo elemento*</span><span class="sxs-lookup"><span data-stu-id="eb05d-515">*Adding a new item*</span></span>

    <span data-ttu-id="eb05d-516">![Agregar clase de modelo de género](aspnet-mvc-4-fundamentals/_static/image28.png "Agregar clase de modelo de género")</span><span class="sxs-lookup"><span data-stu-id="eb05d-516">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="eb05d-517">*Agregar clase de modelo de género*</span><span class="sxs-lookup"><span data-stu-id="eb05d-517">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="eb05d-518">Agregar un **nombre** propiedad a la clase de género.</span><span class="sxs-lookup"><span data-stu-id="eb05d-518">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="eb05d-519">Para ello, agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="eb05d-519">To do this, add the following code:</span></span>

    <span data-ttu-id="eb05d-520">(Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex6 género*)</span><span class="sxs-lookup"><span data-stu-id="eb05d-520">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="eb05d-521">El siguiente procedimiento como antes, agregar un **álbum** clase.</span><span class="sxs-lookup"><span data-stu-id="eb05d-521">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="eb05d-522">Para ello, haga clic en el **modelos** carpeta en el **el Explorador de soluciones**, seleccione **agregar** y, a continuación, el **nuevo elemento** opción.</span><span class="sxs-lookup"><span data-stu-id="eb05d-522">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="eb05d-523">En **código**, elija el **clase** de elemento y asigne el nombre *Album.cs*, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-523">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="eb05d-524">Agregue dos propiedades a la clase de álbum: **género** y **título**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-524">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="eb05d-525">Para ello, agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="eb05d-525">To do this, add the following code:</span></span>

    <span data-ttu-id="eb05d-526">(Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - álbum Ex6*)</span><span class="sxs-lookup"><span data-stu-id="eb05d-526">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="eb05d-527">Tarea 2: agregar un StoreBrowseViewModel</span><span class="sxs-lookup"><span data-stu-id="eb05d-527">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="eb05d-528">Un **StoreBrowseViewModel** se usará en esta tarea para mostrar los álbumes que coinciden con un género seleccionado.</span><span class="sxs-lookup"><span data-stu-id="eb05d-528">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="eb05d-529">En esta tarea, creará esta clase y, a continuación, agregue dos propiedades para controlar la **género** y su **álbum**de la lista.</span><span class="sxs-lookup"><span data-stu-id="eb05d-529">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="eb05d-530">Agregar un **StoreBrowseViewModel** clase.</span><span class="sxs-lookup"><span data-stu-id="eb05d-530">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="eb05d-531">Para ello, haga clic en el **ViewModels** carpeta en el **el Explorador de soluciones**, seleccione **agregar** y, a continuación, el **nuevo elemento** opción.</span><span class="sxs-lookup"><span data-stu-id="eb05d-531">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="eb05d-532">En **código**, elija el **clase** de elemento y asigne el nombre *StoreBrowseViewModel.cs*, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-532">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="eb05d-533">Agregue una referencia a los modelos en **StoreBrowseViewModel** clase.</span><span class="sxs-lookup"><span data-stu-id="eb05d-533">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="eb05d-534">Para ello, agregue el siguiente uso de espacio de nombres:</span><span class="sxs-lookup"><span data-stu-id="eb05d-534">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="eb05d-535">(Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="eb05d-535">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="eb05d-536">Agregue dos propiedades a **StoreBrowseViewModel** clase: **género** y **álbumes**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-536">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="eb05d-537">Para ello, agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="eb05d-537">To do this, add the following code:</span></span>

    <span data-ttu-id="eb05d-538">(Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="eb05d-538">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> <span data-ttu-id="eb05d-539">¿Qué es **lista&lt;álbum&gt;**  ?: está usando esta definición la **lista&lt;T&gt;**  tipo, donde **T** restringe el tipo de los elementos de este **lista** pertenecen, en este caso **álbum** (o cualquiera de sus descendientes).</span><span class="sxs-lookup"><span data-stu-id="eb05d-539">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
> 
> <span data-ttu-id="eb05d-540">Esta capacidad para diseñar clases y métodos que aplazan la especificación de uno o varios tipos hasta que la clase o método se declara y crea una instancia de código de cliente es una característica del lenguaje C# llamado **genéricos**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-540">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
> 
> <span data-ttu-id="eb05d-541">**Lista&lt;T&gt;**  es el equivalente genérico de la **ArrayList** escriba y está disponible en el **System.Collections.Generic** espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="eb05d-541">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="eb05d-542">Una de las ventajas de usar **genéricos** es que ya que se especifica el tipo, es necesario que se encargue de operaciones como convertir los elementos en la comprobación de tipo **álbum** como lo haría con un **ArrayList**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-542">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="eb05d-543">Tarea 3: usar el nuevo modelo de vista en el StoreController</span><span class="sxs-lookup"><span data-stu-id="eb05d-543">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="eb05d-544">En esta tarea, modificará el **StoreController**del **examinar** y **detalles** métodos de acción para que use el nuevo **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="eb05d-544">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="eb05d-545">Agregue una referencia a la carpeta modelos en **StoreController** clase.</span><span class="sxs-lookup"><span data-stu-id="eb05d-545">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="eb05d-546">Para ello, expanda el **controladores** carpeta en el **el Explorador de soluciones** y abra el **StoreController** clase.</span><span class="sxs-lookup"><span data-stu-id="eb05d-546">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="eb05d-547">A continuación, agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="eb05d-547">Then add the following code:</span></span>

    <span data-ttu-id="eb05d-548">(Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="eb05d-548">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="eb05d-549">Reemplace el **examinar** método de acción que se usará el **StoreViewBrowseController** clase.</span><span class="sxs-lookup"><span data-stu-id="eb05d-549">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="eb05d-550">Creará un género y dos nuevos objetos de álbumes con datos ficticios (en la práctica siguiente consumirá datos reales de una base de datos).</span><span class="sxs-lookup"><span data-stu-id="eb05d-550">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="eb05d-551">Para ello, reemplace el **examinar** método con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="eb05d-551">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="eb05d-552">(Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="eb05d-552">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="eb05d-553">Reemplace el **detalles** método de acción que se usará el **StoreViewBrowseController** clase.</span><span class="sxs-lookup"><span data-stu-id="eb05d-553">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="eb05d-554">Se creará una nueva **álbum** objeto que debe devolverse a la **vista**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-554">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="eb05d-555">Para ello, reemplace el **detalles** método con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="eb05d-555">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="eb05d-556">(Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="eb05d-556">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="eb05d-557">Tarea 4: agregar una plantilla de vista de exploración</span><span class="sxs-lookup"><span data-stu-id="eb05d-557">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="eb05d-558">En esta tarea, agregará un **examinar** vista para mostrar los álbumes que se encuentra un género concreto.</span><span class="sxs-lookup"><span data-stu-id="eb05d-558">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="eb05d-559">Antes de crear la nueva plantilla de vista, debe compilar el proyecto para que el **agregar vista** diálogo conoce la **ViewModel** clase se debe utilizar.</span><span class="sxs-lookup"><span data-stu-id="eb05d-559">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="eb05d-560">Compile el proyecto seleccionando el **compilar** elemento de menú y, a continuación, **MvcMusicStore compilar**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-560">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="eb05d-561">Agregar un **examinar** vista.</span><span class="sxs-lookup"><span data-stu-id="eb05d-561">Add a **Browse** View.</span></span> <span data-ttu-id="eb05d-562">Para ello, haga clic en el **examinar** método de acción de la **StoreController** y haga clic en **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-562">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="eb05d-563">En el **agregar vista** diálogo cuadro, compruebe que el nombre de vista es **examinar**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-563">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="eb05d-564">Compruebe el **crear una vista fuertemente tipada** casilla y seleccione **StoreBrowseViewModel** desde el **clase modelo** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="eb05d-564">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="eb05d-565">Deje los demás campos con sus valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="eb05d-565">Leave the other fields with their default value.</span></span> <span data-ttu-id="eb05d-566">A continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-566">Then click **Add**.</span></span>

    <span data-ttu-id="eb05d-567">![Agregar una vista Examinar](aspnet-mvc-4-fundamentals/_static/image29.png "agregar una vista de exploración")</span><span class="sxs-lookup"><span data-stu-id="eb05d-567">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="eb05d-568">*Agregar una vista de exploración*</span><span class="sxs-lookup"><span data-stu-id="eb05d-568">*Adding a Browse View*</span></span>
4. <span data-ttu-id="eb05d-569">Modificar el **Browse.cshtml** para mostrar información del género, obtener acceso a la **StoreBrowseViewModel** objeto que se pasa a la plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="eb05d-569">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="eb05d-570">Para ello, reemplace el contenido por lo siguiente: (C#)</span><span class="sxs-lookup"><span data-stu-id="eb05d-570">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="eb05d-571">Tarea 5: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="eb05d-571">Task 5 - Running the Application</span></span>

<span data-ttu-id="eb05d-572">En esta tarea, probará que la **examinar** método recupera álbumes desde el **examinar** acción del método.</span><span class="sxs-lookup"><span data-stu-id="eb05d-572">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="eb05d-573">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb05d-573">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="eb05d-574">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="eb05d-574">The project starts in the Home page.</span></span> <span data-ttu-id="eb05d-575">¿Cambiar la dirección URL de   **/Store/examinar? Género = Disco** para comprobar que la acción devuelve dos álbumes.</span><span class="sxs-lookup"><span data-stu-id="eb05d-575">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="eb05d-576">![Exploración de álbumes Disco Store](aspnet-mvc-4-fundamentals/_static/image30.png "Store Disco álbumes de exploración")</span><span class="sxs-lookup"><span data-stu-id="eb05d-576">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="eb05d-577">*Exploración álbumes de Disco de Store*</span><span class="sxs-lookup"><span data-stu-id="eb05d-577">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="eb05d-578">Tarea 6: mostrar información sobre un álbum específico</span><span class="sxs-lookup"><span data-stu-id="eb05d-578">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="eb05d-579">En esta tarea, implementará el **Store/detalles** vista para mostrar información sobre un álbum específico.</span><span class="sxs-lookup"><span data-stu-id="eb05d-579">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="eb05d-580">En este laboratorio práctico, todo lo que se va a mostrar sobre el álbum ya está incluido en el **vista** plantilla.</span><span class="sxs-lookup"><span data-stu-id="eb05d-580">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="eb05d-581">Es así, en lugar de crear un **StoreDetailsViewModel** (clase), se usará el actual **StoreBrowseViewModel** plantilla pasándole el álbum.</span><span class="sxs-lookup"><span data-stu-id="eb05d-581">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="eb05d-582">Cierre el explorador si es necesario, para volver a la ventana de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-582">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="eb05d-583">Agregue un nuevo **detalles** ver para el **StoreController**del **detalles** método de acción.</span><span class="sxs-lookup"><span data-stu-id="eb05d-583">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="eb05d-584">Para ello, haga clic en el **detalles** método en el **StoreController** clase y haga clic en **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-584">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="eb05d-585">En el **agregar vista** cuadro de diálogo, compruebe que la **nombre de la vista** es **detalles**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-585">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="eb05d-586">Compruebe el **crear una vista fuertemente tipada** casilla y seleccione **álbum** desde el **clase modelo** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="eb05d-586">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="eb05d-587">Deje los demás campos con sus valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="eb05d-587">Leave the other fields with their default value.</span></span> <span data-ttu-id="eb05d-588">A continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-588">Then click **Add**.</span></span> <span data-ttu-id="eb05d-589">Esto creará y abrirá una **\Views\Store\Details.cshtml** archivo.</span><span class="sxs-lookup"><span data-stu-id="eb05d-589">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="eb05d-590">![Agregar una vista de detalles](aspnet-mvc-4-fundamentals/_static/image31.png "agregar una vista de detalles")</span><span class="sxs-lookup"><span data-stu-id="eb05d-590">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="eb05d-591">*Agregar una vista de detalles*</span><span class="sxs-lookup"><span data-stu-id="eb05d-591">*Adding a Details View*</span></span>
3. <span data-ttu-id="eb05d-592">Modificar el **Details.cshtml** archivo para mostrar información del álbum, obtener acceso a la **álbum** objeto que se pasa a la plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="eb05d-592">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="eb05d-593">Para ello, reemplace el contenido por lo siguiente: (C#)</span><span class="sxs-lookup"><span data-stu-id="eb05d-593">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="eb05d-594">Tarea 7: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="eb05d-594">Task 7 - Running the Application</span></span>

<span data-ttu-id="eb05d-595">En esta tarea, probará que la **detalles** View recupera información del álbum desde la **detalla la acción** método.</span><span class="sxs-lookup"><span data-stu-id="eb05d-595">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="eb05d-596">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb05d-596">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="eb05d-597">Se inicia el proyecto en el **inicio** página.</span><span class="sxs-lookup"><span data-stu-id="eb05d-597">The project starts in the **Home** page.</span></span> <span data-ttu-id="eb05d-598">Cambiar la dirección URL de **/Store/Details/5** para comprobar la información del álbum.</span><span class="sxs-lookup"><span data-stu-id="eb05d-598">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="eb05d-599">![Exploración de detalle de álbumes](aspnet-mvc-4-fundamentals/_static/image32.png "detalle álbumes de exploración")</span><span class="sxs-lookup"><span data-stu-id="eb05d-599">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="eb05d-600">*Examinar los detalles del álbum*</span><span class="sxs-lookup"><span data-stu-id="eb05d-600">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="eb05d-601">Tarea 8: agregar vínculos entre páginas</span><span class="sxs-lookup"><span data-stu-id="eb05d-601">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="eb05d-602">En esta tarea, agregará un vínculo en la vista de Store para tener un vínculo en todos los nombres de género correspondientes **/Store/examinar** dirección URL.</span><span class="sxs-lookup"><span data-stu-id="eb05d-602">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="eb05d-603">Este modo, al hacer clic en uno de estos, por ejemplo **Disco**, se le remitirá a **/Store/examinar? genre = Disco** dirección URL.</span><span class="sxs-lookup"><span data-stu-id="eb05d-603">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="eb05d-604">Cierre el explorador si es necesario, para volver a la ventana de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-604">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="eb05d-605">Actualización de la **índice** página para agregar un vínculo a la **examinar** página.</span><span class="sxs-lookup"><span data-stu-id="eb05d-605">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="eb05d-606">Para ello, en el **el Explorador de soluciones** expandir la **vistas** carpeta, el **Store** carpeta y haga doble clic en el **Index.cshtml** página.</span><span class="sxs-lookup"><span data-stu-id="eb05d-606">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="eb05d-607">Agregar un vínculo a la vista de exploración que indica el género seleccionado.</span><span class="sxs-lookup"><span data-stu-id="eb05d-607">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="eb05d-608">Para ello, reemplace el código resaltado siguiente dentro de la **&lt;li&gt;** etiquetas: (C#)</span><span class="sxs-lookup"><span data-stu-id="eb05d-608">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > <span data-ttu-id="eb05d-609">otro enfoque sería se vincula directamente a la página, con un código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="eb05d-609">another approach would be linking directly to the page, with a code like the following:</span></span>
   > 
   > <span data-ttu-id="eb05d-610">&lt;un href =&quot;/Store/examinar? genre =@genreName&quot;&gt;@genreName&lt;/a&gt;</span><span class="sxs-lookup"><span data-stu-id="eb05d-610">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
   > 
   > <span data-ttu-id="eb05d-611">Aunque este enfoque funciona, depende de una cadena estática.</span><span class="sxs-lookup"><span data-stu-id="eb05d-611">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="eb05d-612">Si posteriormente se cambia el nombre del controlador, tendrá que cambiar manualmente esta instrucción.</span><span class="sxs-lookup"><span data-stu-id="eb05d-612">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="eb05d-613">Una alternativa mejor es usar un **aplicación auxiliar HTML** método.</span><span class="sxs-lookup"><span data-stu-id="eb05d-613">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="eb05d-614">ASP.NET MVC incluye un método de aplicación auxiliar HTML que está disponible para estas tareas.</span><span class="sxs-lookup"><span data-stu-id="eb05d-614">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="eb05d-615">El **Html.ActionLink()** método auxiliar facilita la compilación HTML **&lt;un&gt;** vínculos, asegurándose de que las rutas de acceso de dirección URL están correctamente codificado como URL.</span><span class="sxs-lookup"><span data-stu-id="eb05d-615">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
   > 
   > <span data-ttu-id="eb05d-616">Htlm.ActionLink tiene varias sobrecargas.</span><span class="sxs-lookup"><span data-stu-id="eb05d-616">Htlm.ActionLink has several overloads.</span></span> <span data-ttu-id="eb05d-617">En este ejercicio usará uno que toma tres parámetros:</span><span class="sxs-lookup"><span data-stu-id="eb05d-617">In this exercise you will use one that takes three parameters:</span></span>
   > 
   > 1. <span data-ttu-id="eb05d-618">Texto del vínculo, que mostrará el nombre de género</span><span class="sxs-lookup"><span data-stu-id="eb05d-618">Link text, which will display the Genre name</span></span>
   > 2. <span data-ttu-id="eb05d-619">Nombre de acción de controlador (**examinar**)</span><span class="sxs-lookup"><span data-stu-id="eb05d-619">Controller action name (**Browse**)</span></span>
   > 3. <span data-ttu-id="eb05d-620">Los valores de parámetro, especificando el nombre de ruta (**género**) y el valor (**nombre de género**)</span><span class="sxs-lookup"><span data-stu-id="eb05d-620">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="eb05d-621">Tarea 9: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="eb05d-621">Task 9 - Running the Application</span></span>

<span data-ttu-id="eb05d-622">En esta tarea, realizará las pruebas que se muestra cada género con un vínculo a la correspondiente **/Store/examinar** dirección URL.</span><span class="sxs-lookup"><span data-stu-id="eb05d-622">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="eb05d-623">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb05d-623">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="eb05d-624">El proyecto se inicia en la página principal.</span><span class="sxs-lookup"><span data-stu-id="eb05d-624">The project starts in the Home page.</span></span> <span data-ttu-id="eb05d-625">Cambiar la dirección URL de **/Store** para comprobar que cada género se vincula a la correspondiente **/Store/examinar** dirección URL.</span><span class="sxs-lookup"><span data-stu-id="eb05d-625">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="eb05d-626">![Exploración de géneros con vínculos a la página Examinar](aspnet-mvc-4-fundamentals/_static/image33.png "géneros de exploración con vínculos a la página Examinar")</span><span class="sxs-lookup"><span data-stu-id="eb05d-626">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="eb05d-627">*Géneros de exploración con vínculos a la página Examinar*</span><span class="sxs-lookup"><span data-stu-id="eb05d-627">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="eb05d-628">Tarea 10: usar la colección de ViewModel dinámicos para pasar valores</span><span class="sxs-lookup"><span data-stu-id="eb05d-628">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="eb05d-629">En esta tarea, obtendrá información sobre un método sencillo y eficaz para pasar valores entre el controlador y la vista sin realizar ningún cambio en el modelo.</span><span class="sxs-lookup"><span data-stu-id="eb05d-629">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="eb05d-630">ASP.NET MVC 4 proporciona la colección &quot;ViewModel&quot;, que se pueden asignar a cualquier valor dinámico y acceder a ellos dentro de los controladores y vistas también.</span><span class="sxs-lookup"><span data-stu-id="eb05d-630">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="eb05d-631">Ahora usará la colección dinámica ViewBag para pasar una lista de &quot; **estrellas géneros** &quot; desde el controlador a la vista.</span><span class="sxs-lookup"><span data-stu-id="eb05d-631">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="eb05d-632">Tendrá acceso la vista de índice Store a **ViewModel** y mostrar la información.</span><span class="sxs-lookup"><span data-stu-id="eb05d-632">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="eb05d-633">Cierre el explorador si es necesario, para volver a la ventana de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-633">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="eb05d-634">Abra **StoreController.cs** y modificar **índice** método para crear una lista de estrellas géneros en ViewModel de la colección:</span><span class="sxs-lookup"><span data-stu-id="eb05d-634">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="eb05d-635">También puede usar la sintaxis **ViewBag [&quot;Starred&quot;]** para tener acceso a las propiedades.</span><span class="sxs-lookup"><span data-stu-id="eb05d-635">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="eb05d-636">El icono de estrella **&quot;starred.png&quot;** se incluye en el **Source\Assets\Images** carpeta de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-636">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="eb05d-637">Para poder agregarlo a la aplicación, arrastre su contenido desde un **Windows Explorer** ventana en la **el Explorador de soluciones** en Visual Web Developer Express, como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="eb05d-637">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="eb05d-638">![Agregar imagen de estrella a la solución](aspnet-mvc-4-fundamentals/_static/image34.png "Agregar imagen de estrella a la solución")</span><span class="sxs-lookup"><span data-stu-id="eb05d-638">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="eb05d-639">*Agregar imagen de estrella a la solución*</span><span class="sxs-lookup"><span data-stu-id="eb05d-639">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="eb05d-640">Abra la vista **Store/Index.cshtml** y modificar el contenido.</span><span class="sxs-lookup"><span data-stu-id="eb05d-640">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="eb05d-641">Leerá el &quot;estrellas&quot; propiedad en el **ViewBag** colección y pregunte si es el nombre del género actual en la lista.</span><span class="sxs-lookup"><span data-stu-id="eb05d-641">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="eb05d-642">En ese caso, se mostrará un icono de estrella directamente en el vínculo de género.</span><span class="sxs-lookup"><span data-stu-id="eb05d-642">In that case you will show a star icon right to the genre link.</span></span>
   <span data-ttu-id="eb05d-643">(C#)</span><span class="sxs-lookup"><span data-stu-id="eb05d-643">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="eb05d-644">Tarea 11: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="eb05d-644">Task 11 - Running the Application</span></span>

<span data-ttu-id="eb05d-645">En esta tarea, realizará las pruebas que los géneros con estrellas mostrar un icono de estrella.</span><span class="sxs-lookup"><span data-stu-id="eb05d-645">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="eb05d-646">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb05d-646">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="eb05d-647">Se inicia el proyecto en el **inicio** página.</span><span class="sxs-lookup"><span data-stu-id="eb05d-647">The project starts in the **Home** page.</span></span> <span data-ttu-id="eb05d-648">Cambiar la dirección URL de **/Store** para comprobar que cada género destacado tiene la etiqueta respete a sí:</span><span class="sxs-lookup"><span data-stu-id="eb05d-648">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="eb05d-649">![Exploración de géneros con elementos con estrellas](aspnet-mvc-4-fundamentals/_static/image35.png "géneros de exploración con elementos con estrellas")</span><span class="sxs-lookup"><span data-stu-id="eb05d-649">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="eb05d-650">*Exploración géneros con elementos con estrellas*</span><span class="sxs-lookup"><span data-stu-id="eb05d-650">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="eb05d-651">Ejercicio 7: Un paseo por la nueva plantilla de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="eb05d-651">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="eb05d-652">En este ejercicio, explorará las mejoras en las plantillas de proyecto de ASP.NET MVC 4, echar un vistazo las características más relevantes de la nueva plantilla.</span><span class="sxs-lookup"><span data-stu-id="eb05d-652">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="eb05d-653">Tarea 1: Exploración de la plantilla de aplicación de Internet de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="eb05d-653">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="eb05d-654">Si no está abierto, inicie **VS Express para Web**</span><span class="sxs-lookup"><span data-stu-id="eb05d-654">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="eb05d-655">Seleccione el **archivo | Nuevo | Proyecto** comando de menú.</span><span class="sxs-lookup"><span data-stu-id="eb05d-655">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="eb05d-656">En el **nuevo proyecto** cuadro de diálogo, seleccione el **Visual C# | Web** plantilla en el panel izquierdo del árbol y elija el **aplicación Web de ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-656">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="eb05d-657">**Nombre** el proyecto *Music Store tal* y actualizar la **nombre de la solución** a *comenzar*, a continuación, seleccione una ubicación (o deje el valor predeterminado) y haga clic en **Aceptar** .</span><span class="sxs-lookup"><span data-stu-id="eb05d-657">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="eb05d-658">![Crear un nuevo proyecto de ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "crear un nuevo proyecto de ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="eb05d-658">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="eb05d-659">*Crear un nuevo proyecto de ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="eb05d-659">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="eb05d-660">En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione el **aplicación de Internet** plantilla de proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-660">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="eb05d-661">Tenga en cuenta que puede seleccionar como el motor de vistas Razor o ASPX.</span><span class="sxs-lookup"><span data-stu-id="eb05d-661">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="eb05d-662">![Crear una nueva aplicación de Internet de ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image37.png "creando una nueva aplicación de Internet de ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="eb05d-662">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="eb05d-663">*Crear una nueva aplicación de Internet de ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="eb05d-663">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="eb05d-664">Se ha introducido la sintaxis de Razor en ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="eb05d-664">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="eb05d-665">Su objetivo es minimizar el número de caracteres y pulsaciones de teclas necesarias en un archivo, lo que permite un rápido y fluido de flujo de trabajo de codificación.</span><span class="sxs-lookup"><span data-stu-id="eb05d-665">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="eb05d-666">Razor aprovecha existente C# / VB (u otros) habilidades lingüísticas y ofrece una sintaxis de marcado de plantilla que permite un flujo de trabajo de construcción de HTML impresionante.</span><span class="sxs-lookup"><span data-stu-id="eb05d-666">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="eb05d-667">Presione **F5** para ejecutar la solución y ver la plantilla renovada.</span><span class="sxs-lookup"><span data-stu-id="eb05d-667">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="eb05d-668">Puede comprobar las características siguientes:</span><span class="sxs-lookup"><span data-stu-id="eb05d-668">You can check out the following features:</span></span>

    1. <span data-ttu-id="eb05d-669">**Plantillas de estilo moderno**</span><span class="sxs-lookup"><span data-stu-id="eb05d-669">**Modern-style templates**</span></span>

        <span data-ttu-id="eb05d-670">Las plantillas que se han renovado, proporcionando más estilos de aspecto moderno.</span><span class="sxs-lookup"><span data-stu-id="eb05d-670">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="eb05d-671">![Las plantillas de ASP.NET MVC 4 se ha rediseñado](aspnet-mvc-4-fundamentals/_static/image38.png "vuelto a diseñar plantillas de ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="eb05d-671">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="eb05d-672">*Plantillas de ASP.NET MVC 4 se ha rediseñado*</span><span class="sxs-lookup"><span data-stu-id="eb05d-672">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="eb05d-673">**Procesamiento adaptable**</span><span class="sxs-lookup"><span data-stu-id="eb05d-673">**Adaptive Rendering**</span></span>

        <span data-ttu-id="eb05d-674">Consulte Cambiar el tamaño de la ventana del explorador y observe cómo el diseño de página se adapta dinámicamente para el nuevo tamaño de ventana.</span><span class="sxs-lookup"><span data-stu-id="eb05d-674">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="eb05d-675">Estas plantillas usan la técnica de procesamiento adaptable se presenten correctamente en las plataformas móviles y de escritorio sin ninguna personalización.</span><span class="sxs-lookup"><span data-stu-id="eb05d-675">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="eb05d-676">![Plantilla de proyecto de ASP.NET MVC 4 en tamaños de explorador diferente](aspnet-mvc-4-fundamentals/_static/image39.png "plantilla de proyecto de ASP.NET MVC 4 en tamaños de explorador diferente")</span><span class="sxs-lookup"><span data-stu-id="eb05d-676">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="eb05d-677">*Plantilla de proyecto de ASP.NET MVC 4 en tamaños de explorador diferente*</span><span class="sxs-lookup"><span data-stu-id="eb05d-677">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="eb05d-678">Cierre el explorador para detener al depurador y vuelva a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-678">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="eb05d-679">Ahora pueden explorar la solución y consulte algunas de las nuevas características introducidas por ASP.NET MVC 4 en la plantilla de proyecto.</span><span class="sxs-lookup"><span data-stu-id="eb05d-679">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="eb05d-680">![ASP.NET MVC4-internet-de-plantilla de proyecto aplicación](aspnet-mvc-4-fundamentals/_static/image40.png "la plantilla de proyecto de aplicación de Internet de ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="eb05d-680">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="eb05d-681">*La plantilla de proyecto de aplicación de Internet de ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="eb05d-681">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   1. <span data-ttu-id="eb05d-682">**Marcado de HTML5**</span><span class="sxs-lookup"><span data-stu-id="eb05d-682">**HTML5 markup**</span></span>

       <span data-ttu-id="eb05d-683">Examinar vistas de la plantilla para averiguar el marcado nuevo tema, por ejemplo open **About.cshtml** ver dentro de **inicio** carpeta.</span><span class="sxs-lookup"><span data-stu-id="eb05d-683">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

       <span data-ttu-id="eb05d-684">![Nueva plantilla, usando un marcado de Razor y HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "nueva plantilla, usando un marcado de Razor y HTML5")</span><span class="sxs-lookup"><span data-stu-id="eb05d-684">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

       <span data-ttu-id="eb05d-685">*Nueva plantilla, usando un marcado de Razor y HTML5*</span><span class="sxs-lookup"><span data-stu-id="eb05d-685">*New template, using Razor and HTML5 markup*</span></span>
   2. <span data-ttu-id="eb05d-686">**Bibliotecas de JavaScript incluidas**</span><span class="sxs-lookup"><span data-stu-id="eb05d-686">**JavaScript libraries included**</span></span>

      1. <span data-ttu-id="eb05d-687">**jQuery**: jQuery simplifica atravesar de documento HTML, control de eventos, animar y las interacciones de Ajax.</span><span class="sxs-lookup"><span data-stu-id="eb05d-687">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
      2. <span data-ttu-id="eb05d-688">**jQuery UI**: esta biblioteca proporciona abstracciones de interacción de bajo nivel y animación, advanced efectos y temas widgets, creados a partir de la biblioteca JavaScript jQuery.</span><span class="sxs-lookup"><span data-stu-id="eb05d-688">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

         > [!NOTE]
         > <span data-ttu-id="eb05d-689">Puede obtener información acerca de jQuery y jQuery UI en [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="eb05d-689">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
      3. <span data-ttu-id="eb05d-690">**KnockoutJS**: plantilla predeterminada de las ASP.NET MVC 4 ahora incluye **KnockoutJS**, un marco MVVM JavaScript que le permite crear aplicaciones web enriquecidas y alta capacidad de respuesta con JavaScript y HTML.</span><span class="sxs-lookup"><span data-stu-id="eb05d-690">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="eb05d-691">Al igual que en ASP.NET MVC 3, jQuery y bibliotecas de interfaz de usuario de jQuery también se incluyen en ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="eb05d-691">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

          > [!NOTE]
          > <span data-ttu-id="eb05d-692">Puede obtener más información acerca de la biblioteca KnockOutJS en este vínculo: [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="eb05d-692">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
      4. <span data-ttu-id="eb05d-693">**Modernizr**: esta biblioteca se ejecuta automáticamente, lo que su sitio compatible con los exploradores más antiguos cuando se usa tecnologías de HTML5 y CSS3.</span><span class="sxs-lookup"><span data-stu-id="eb05d-693">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

          > [!NOTE]
          > <span data-ttu-id="eb05d-694">Puede obtener más información acerca de la biblioteca Modernizr en este vínculo: [ http://www.modernizr.com/ ](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="eb05d-694">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
   3. <span data-ttu-id="eb05d-695">**SimpleMembership, incluido en la solución**</span><span class="sxs-lookup"><span data-stu-id="eb05d-695">**SimpleMembership included in the solution**</span></span>

       <span data-ttu-id="eb05d-696">SimpleMembership se ha diseñado como un reemplazo para el sistema anterior de proveedor de roles de ASP.NET y la pertenencia.</span><span class="sxs-lookup"><span data-stu-id="eb05d-696">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="eb05d-697">Tiene muchas características nuevas que facilitan al desarrollador de páginas web seguras en una manera más flexible.</span><span class="sxs-lookup"><span data-stu-id="eb05d-697">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

       <span data-ttu-id="eb05d-698">La plantilla de Internet ya ha configurado algunas cosas para integrar SimpleMembership, por ejemplo, AccountController está preparado para usar OAuthWebSecurity (para el registro de la cuenta de OAuth, inicio de sesión, administración, etc.) y la seguridad de la Web.</span><span class="sxs-lookup"><span data-stu-id="eb05d-698">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

       <span data-ttu-id="eb05d-699">![SimpleMembership incluidos en la solución](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership incluidos en la solución")</span><span class="sxs-lookup"><span data-stu-id="eb05d-699">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

       <span data-ttu-id="eb05d-700">*SimpleMembership incluidos en la solución*</span><span class="sxs-lookup"><span data-stu-id="eb05d-700">*SimpleMembership Included in the solution*</span></span>

       > [!NOTE]
       > <span data-ttu-id="eb05d-701">Obtenga más información sobre [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) en MSDN.</span><span class="sxs-lookup"><span data-stu-id="eb05d-701">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="eb05d-702">Además, puede implementar esta aplicación a los siguientes sitios Web Windows Azure [Apéndice B: publicar una aplicación ASP.NET MVC 4 mediante Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="eb05d-702">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="eb05d-703">Resumen</span><span class="sxs-lookup"><span data-stu-id="eb05d-703">Summary</span></span>

<span data-ttu-id="eb05d-704">Al completar este laboratorio práctico ha aprendido los aspectos básicos de ASP.NET MVC:</span><span class="sxs-lookup"><span data-stu-id="eb05d-704">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="eb05d-705">Los elementos principales de una aplicación MVC y cómo interactúan</span><span class="sxs-lookup"><span data-stu-id="eb05d-705">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="eb05d-706">Cómo crear una aplicación ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="eb05d-706">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="eb05d-707">Cómo agregar y configurar los controladores para controlar los parámetros se pasa a través de la dirección URL y la cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="eb05d-707">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="eb05d-708">Cómo agregar una página principal de diseño para configurar una plantilla para el contenido HTML común, una hoja de estilos para mejorar la apariencia y funcionamiento y una plantilla de vista para mostrar el contenido HTML</span><span class="sxs-lookup"><span data-stu-id="eb05d-708">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="eb05d-709">Cómo usar el patrón de ViewModel para pasar propiedades a la plantilla de vista para mostrar información dinámica</span><span class="sxs-lookup"><span data-stu-id="eb05d-709">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="eb05d-710">Cómo usar los parámetros pasados a los controladores en la plantilla de vista</span><span class="sxs-lookup"><span data-stu-id="eb05d-710">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="eb05d-711">Cómo agregar vínculos a páginas dentro de la aplicación de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="eb05d-711">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="eb05d-712">Cómo agregar y utilizar propiedades dinámicas en una vista</span><span class="sxs-lookup"><span data-stu-id="eb05d-712">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="eb05d-713">Las mejoras en las plantillas de proyecto de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="eb05d-713">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="eb05d-714">Apéndice A: instalación de Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="eb05d-714">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="eb05d-715">Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión utilizando el **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-715">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="eb05d-716">Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* mediante *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="eb05d-716">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="eb05d-717">Vaya a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="eb05d-717">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="eb05d-718">Como alternativa, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con SDK de Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="eb05d-718">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="eb05d-719">Haga clic en **instalar ahora**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-719">Click on **Install Now**.</span></span> <span data-ttu-id="eb05d-720">Si no tienes **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="eb05d-720">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="eb05d-721">Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.</span><span class="sxs-lookup"><span data-stu-id="eb05d-721">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="eb05d-722">![Instale Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "instale Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="eb05d-722">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="eb05d-723">*Instale Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="eb05d-723">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="eb05d-724">Leer todos los productos las licencias y los términos y haga clic en **acepto** para continuar.</span><span class="sxs-lookup"><span data-stu-id="eb05d-724">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Acepte los términos de licencia](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="eb05d-726">*Acepte los términos de licencia*</span><span class="sxs-lookup"><span data-stu-id="eb05d-726">*Accepting the license terms*</span></span>
5. <span data-ttu-id="eb05d-727">Espere hasta que finalice el proceso de descarga e instalación.</span><span class="sxs-lookup"><span data-stu-id="eb05d-727">Wait until the downloading and installation process completes.</span></span>

    ![Progreso de la instalación](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="eb05d-729">*Progreso de la instalación*</span><span class="sxs-lookup"><span data-stu-id="eb05d-729">*Installation progress*</span></span>
6. <span data-ttu-id="eb05d-730">Cuando se complete la instalación, haga clic en **finalizar**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-730">When the installation completes, click **Finish**.</span></span>

    ![Instalación completada](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="eb05d-732">*Instalación completada*</span><span class="sxs-lookup"><span data-stu-id="eb05d-732">*Installation completed*</span></span>
7. <span data-ttu-id="eb05d-733">Haga clic en **Exit** para cerrar el instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="eb05d-733">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="eb05d-734">Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **VS Express**&quot;, a continuación, haga clic en el **VS Express para Web** icono.</span><span class="sxs-lookup"><span data-stu-id="eb05d-734">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para el icono de Web](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="eb05d-736">*VS Express para el icono de Web*</span><span class="sxs-lookup"><span data-stu-id="eb05d-736">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="eb05d-737">Apéndice B: publicar una aplicación ASP.NET MVC 4 mediante Web Deploy</span><span class="sxs-lookup"><span data-stu-id="eb05d-737">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="eb05d-738">En este apéndice se mostrará cómo crear un nuevo sitio web desde el Portal de administración de Windows Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, que aprovecha la característica de publicación Web Deploy proporcionada por Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="eb05d-738">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="eb05d-739">Tarea 1: crear un nuevo sitio Web de Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="eb05d-739">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="eb05d-740">Vaya a la [Portal de administración de Windows Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas con su suscripción.</span><span class="sxs-lookup"><span data-stu-id="eb05d-740">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="eb05d-741">Con Windows Azure puede hospedar 10 sitios Web ASP.NET de forma gratuita y, a continuación, escalar a medida que crece el tráfico.</span><span class="sxs-lookup"><span data-stu-id="eb05d-741">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="eb05d-742">Puede registrarse [aquí](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="eb05d-742">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="eb05d-743">![Inicie sesión en el portal de Windows Azure](aspnet-mvc-4-fundamentals/_static/image48.png "inicie sesión en el portal de Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="eb05d-743">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="eb05d-744">*Inicie sesión en el Portal de administración de Azure de Windows*</span><span class="sxs-lookup"><span data-stu-id="eb05d-744">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="eb05d-745">Haga clic en **New** en la barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="eb05d-745">Click **New** on the command bar.</span></span>

    <span data-ttu-id="eb05d-746">![Crear un nuevo sitio Web](aspnet-mvc-4-fundamentals/_static/image49.png "crear un nuevo sitio Web")</span><span class="sxs-lookup"><span data-stu-id="eb05d-746">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="eb05d-747">*Crear un nuevo sitio Web*</span><span class="sxs-lookup"><span data-stu-id="eb05d-747">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="eb05d-748">Haga clic en **proceso** | **sitio Web**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-748">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="eb05d-749">A continuación, seleccione **creación rápida** opción.</span><span class="sxs-lookup"><span data-stu-id="eb05d-749">Then select **Quick Create** option.</span></span> <span data-ttu-id="eb05d-750">Proporcione una dirección URL disponible para el nuevo sitio web y haga clic en **crear sitio Web**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-750">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="eb05d-751">Un sitio Web de Windows Azure es el host para una aplicación web que se ejecutan en la nube que puede controlar y administrar.</span><span class="sxs-lookup"><span data-stu-id="eb05d-751">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="eb05d-752">La opción Creación rápida permite implementar una aplicación web completada en el sitio Web Windows Azure desde fuera del portal.</span><span class="sxs-lookup"><span data-stu-id="eb05d-752">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="eb05d-753">No incluye los pasos para configurar una base de datos.</span><span class="sxs-lookup"><span data-stu-id="eb05d-753">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="eb05d-754">![Crear un nuevo sitio Web mediante Creación rápida](aspnet-mvc-4-fundamentals/_static/image50.png "crear un nuevo sitio Web mediante Creación rápida")</span><span class="sxs-lookup"><span data-stu-id="eb05d-754">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="eb05d-755">*Crear un nuevo sitio Web mediante Creación rápida*</span><span class="sxs-lookup"><span data-stu-id="eb05d-755">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="eb05d-756">Espere hasta que el nuevo **sitio Web** se crea.</span><span class="sxs-lookup"><span data-stu-id="eb05d-756">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="eb05d-757">Una vez creado el sitio Web, haga clic en el vínculo situado bajo el **URL** columna.</span><span class="sxs-lookup"><span data-stu-id="eb05d-757">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="eb05d-758">Compruebe que funciona el nuevo sitio Web.</span><span class="sxs-lookup"><span data-stu-id="eb05d-758">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="eb05d-759">![Exploración al sitio web nuevo](aspnet-mvc-4-fundamentals/_static/image51.png "exploración al sitio web nuevo")</span><span class="sxs-lookup"><span data-stu-id="eb05d-759">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="eb05d-760">*Exploración al sitio web nuevo*</span><span class="sxs-lookup"><span data-stu-id="eb05d-760">*Browsing to the new web site*</span></span>

    <span data-ttu-id="eb05d-761">![Sitio Web que se ejecuta](aspnet-mvc-4-fundamentals/_static/image52.png "sitio Web que se ejecuta")</span><span class="sxs-lookup"><span data-stu-id="eb05d-761">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="eb05d-762">*Sitio Web que se ejecuta*</span><span class="sxs-lookup"><span data-stu-id="eb05d-762">*Web site running*</span></span>
6. <span data-ttu-id="eb05d-763">Vuelva al portal y haga clic en el nombre del sitio web en el **nombre** columna para mostrar las páginas de administración.</span><span class="sxs-lookup"><span data-stu-id="eb05d-763">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="eb05d-764">![Abrir las páginas de administración del sitio web](aspnet-mvc-4-fundamentals/_static/image53.png "abrir las páginas de administración del sitio web")</span><span class="sxs-lookup"><span data-stu-id="eb05d-764">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="eb05d-765">*Abrir las páginas de administración del sitio Web*</span><span class="sxs-lookup"><span data-stu-id="eb05d-765">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="eb05d-766">En el **panel** página, en el **primea** sección, haga clic en el **descargar perfil de publicación** vínculo.</span><span class="sxs-lookup"><span data-stu-id="eb05d-766">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="eb05d-767">El *perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un sitio Web de Windows Azure para cada método de publicación habilitado.</span><span class="sxs-lookup"><span data-stu-id="eb05d-767">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="eb05d-768">El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse y autenticarse en cada uno de los puntos de conexión para el que está habilitado un método de publicación.</span><span class="sxs-lookup"><span data-stu-id="eb05d-768">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="eb05d-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para publicación de aplicaciones web para sitios Web de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="eb05d-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="eb05d-770">![Descargando el sitio web de perfil de publicación](aspnet-mvc-4-fundamentals/_static/image54.png "descargando el sitio web de perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="eb05d-770">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="eb05d-771">*Descargando el sitio Web de perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="eb05d-771">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="eb05d-772">Descargue el archivo de perfil de publicación en una ubicación conocida.</span><span class="sxs-lookup"><span data-stu-id="eb05d-772">Download the publish profile file to a known location.</span></span> <span data-ttu-id="eb05d-773">Aún más en este ejercicio verá cómo usar este archivo para publicar una aplicación web a un sitios Web de Windows Azure desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eb05d-773">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="eb05d-774">![Guardar el archivo de perfil de publicación](aspnet-mvc-4-fundamentals/_static/image55.png "guardar el perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="eb05d-774">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="eb05d-775">*Guardar el archivo de perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="eb05d-775">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="eb05d-776">Tarea 2: configurar el servidor de base de datos</span><span class="sxs-lookup"><span data-stu-id="eb05d-776">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="eb05d-777">Si la aplicación hace uso de SQL Server deberá crear un servidor de base de datos SQL de bases de datos.</span><span class="sxs-lookup"><span data-stu-id="eb05d-777">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="eb05d-778">Si desea implementar una aplicación simple que no utiliza SQL Server, podría omitir esta tarea.</span><span class="sxs-lookup"><span data-stu-id="eb05d-778">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="eb05d-779">Necesitará un servidor de base de datos SQL para almacenar la base de datos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb05d-779">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="eb05d-780">Puede ver los servidores de base de datos SQL de la suscripción en el portal de administración de Windows Azure en **bases de datos Sql** | **servidores** | **del servidor Panel**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-780">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="eb05d-781">Si no tiene un servidor creado, puede crear una mediante el **agregar** botón en la barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="eb05d-781">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="eb05d-782">Tome nota de la **nombre del servidor y dirección URL, nombre de inicio de sesión de administrador y contraseña**, ya que lo usará en las siguientes tareas.</span><span class="sxs-lookup"><span data-stu-id="eb05d-782">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="eb05d-783">No cree la base de datos, como se crearán en una etapa posterior.</span><span class="sxs-lookup"><span data-stu-id="eb05d-783">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="eb05d-784">![Panel de base de datos SQL Server](aspnet-mvc-4-fundamentals/_static/image56.png "panel de base de datos SQL Server")</span><span class="sxs-lookup"><span data-stu-id="eb05d-784">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="eb05d-785">*Panel de base de datos SQL Server*</span><span class="sxs-lookup"><span data-stu-id="eb05d-785">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="eb05d-786">En la siguiente tarea probará la conexión de base de datos desde Visual Studio, por ese motivo debe incluir la dirección IP local en la lista del servidor de **direcciones IP permitidas**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-786">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="eb05d-787">Para ello, haga clic en **configurar**, seleccione la dirección IP de **dirección IP del cliente actual** y péguelo en el **dirección IP inicial** y **ladirecciónIPfinal** cuadros de texto y haga clic en el ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) botón.</span><span class="sxs-lookup"><span data-stu-id="eb05d-787">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![Agregar dirección IP del cliente](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="eb05d-789">*Agregar dirección IP del cliente*</span><span class="sxs-lookup"><span data-stu-id="eb05d-789">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="eb05d-790">Una vez el **dirección IP del cliente** se agrega a las direcciones IP permitidas de lista, haga clic en **guardar** para confirmar los cambios.</span><span class="sxs-lookup"><span data-stu-id="eb05d-790">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmar cambios](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="eb05d-792">*Confirmar cambios*</span><span class="sxs-lookup"><span data-stu-id="eb05d-792">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="eb05d-793">Tarea 3: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy</span><span class="sxs-lookup"><span data-stu-id="eb05d-793">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="eb05d-794">Vuelva a la solución de ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="eb05d-794">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="eb05d-795">En el **el Explorador de soluciones**, haga clic en el proyecto de sitio web y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-795">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="eb05d-796">![Publicar la aplicación](aspnet-mvc-4-fundamentals/_static/image60.png "publicar la aplicación")</span><span class="sxs-lookup"><span data-stu-id="eb05d-796">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="eb05d-797">*Publicar el sitio web*</span><span class="sxs-lookup"><span data-stu-id="eb05d-797">*Publishing the web site*</span></span>
2. <span data-ttu-id="eb05d-798">Importar el perfil de publicación que guardó en la primera tarea.</span><span class="sxs-lookup"><span data-stu-id="eb05d-798">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="eb05d-799">![Importar el perfil de publicación](aspnet-mvc-4-fundamentals/_static/image61.png "importar el perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="eb05d-799">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="eb05d-800">*Importar perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="eb05d-800">*Importing publish profile*</span></span>
3. <span data-ttu-id="eb05d-801">Haga clic en **validar conexión**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-801">Click **Validate Connection**.</span></span> <span data-ttu-id="eb05d-802">Una vez completada la validación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-802">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="eb05d-803">Validación está completa cuando aparece una marca de verificación verde junto al botón Validar conexión.</span><span class="sxs-lookup"><span data-stu-id="eb05d-803">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="eb05d-804">![Validación de la conexión](aspnet-mvc-4-fundamentals/_static/image62.png "validación de la conexión")</span><span class="sxs-lookup"><span data-stu-id="eb05d-804">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="eb05d-805">*Validación de la conexión*</span><span class="sxs-lookup"><span data-stu-id="eb05d-805">*Validating connection*</span></span>
4. <span data-ttu-id="eb05d-806">En el **configuración** página, en el **bases de datos** sección, haga clic en el botón situado junto al cuadro de texto de la conexión de base de datos (es decir, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="eb05d-806">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="eb05d-807">![Configuración de implementación Web](aspnet-mvc-4-fundamentals/_static/image63.png "configuración de implementación Web")</span><span class="sxs-lookup"><span data-stu-id="eb05d-807">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="eb05d-808">*Configuración de implementación Web*</span><span class="sxs-lookup"><span data-stu-id="eb05d-808">*Web deploy configuration*</span></span>
5. <span data-ttu-id="eb05d-809">Configure la conexión de base de datos como sigue:</span><span class="sxs-lookup"><span data-stu-id="eb05d-809">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="eb05d-810">En el **nombre del servidor** escriba la dirección URL de base de datos de SQL server mediante el *tcp:* prefijo.</span><span class="sxs-lookup"><span data-stu-id="eb05d-810">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="eb05d-811">En **nombre de usuario** escriba el nombre de inicio de sesión del Administrador de servidor.</span><span class="sxs-lookup"><span data-stu-id="eb05d-811">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="eb05d-812">En **contraseña** escriba la contraseña de inicio de sesión del Administrador de servidor.</span><span class="sxs-lookup"><span data-stu-id="eb05d-812">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="eb05d-813">Escriba un nuevo nombre de base de datos, por ejemplo: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="eb05d-813">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="eb05d-814">![Configuración de la cadena de conexión de destino](aspnet-mvc-4-fundamentals/_static/image64.png "configuración de la cadena de conexión de destino")</span><span class="sxs-lookup"><span data-stu-id="eb05d-814">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="eb05d-815">*Configuración de la cadena de conexión de destino*</span><span class="sxs-lookup"><span data-stu-id="eb05d-815">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="eb05d-816">A continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-816">Then click **OK**.</span></span> <span data-ttu-id="eb05d-817">Cuando se le pedirá que cree la base de datos, haga clic en **Sí**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-817">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="eb05d-818">![Creación de la base de datos](aspnet-mvc-4-fundamentals/_static/image65.png "creación de la cadena de la base de datos")</span><span class="sxs-lookup"><span data-stu-id="eb05d-818">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="eb05d-819">*Creación de la base de datos*</span><span class="sxs-lookup"><span data-stu-id="eb05d-819">*Creating the database*</span></span>
7. <span data-ttu-id="eb05d-820">La cadena de conexión que se usará para conectarse a la base de datos de SQL en Windows Azure se muestra en el cuadro de texto de conexión predeterminado.</span><span class="sxs-lookup"><span data-stu-id="eb05d-820">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="eb05d-821">Después, haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-821">Then click **Next**.</span></span>

    <span data-ttu-id="eb05d-822">![Cadena de conexión que apunte a la base de datos SQL](aspnet-mvc-4-fundamentals/_static/image66.png "cadena de conexión que apunte a la base de datos SQL")</span><span class="sxs-lookup"><span data-stu-id="eb05d-822">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="eb05d-823">*Cadena de conexión que apunte a la base de datos SQL*</span><span class="sxs-lookup"><span data-stu-id="eb05d-823">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="eb05d-824">En el **Preview** página, haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-824">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="eb05d-825">![Publicar la aplicación web](aspnet-mvc-4-fundamentals/_static/image67.png "publicar la aplicación web")</span><span class="sxs-lookup"><span data-stu-id="eb05d-825">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="eb05d-826">*Publicar la aplicación web*</span><span class="sxs-lookup"><span data-stu-id="eb05d-826">*Publishing the web application*</span></span>
9. <span data-ttu-id="eb05d-827">Una vez que finalice el proceso de publicación, el explorador predeterminado abrirá el sitio web publicado.</span><span class="sxs-lookup"><span data-stu-id="eb05d-827">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="eb05d-828">![Publica la aplicación en Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "publica la aplicación en Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="eb05d-828">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="eb05d-829">*Publicada aplicación en Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="eb05d-829">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="eb05d-830">Apéndice C: uso de fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="eb05d-830">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="eb05d-831">Con fragmentos de código, tiene todo el código que necesita a su alcance.</span><span class="sxs-lookup"><span data-stu-id="eb05d-831">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="eb05d-832">El documento de laboratorio le dirá exactamente cuándo se pueden utilizar, como se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="eb05d-832">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="eb05d-833">![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-fundamentals/_static/image69.png "fragmentos de código en Visual Studio para insertar código en el proyecto")</span><span class="sxs-lookup"><span data-stu-id="eb05d-833">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="eb05d-834">*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*</span><span class="sxs-lookup"><span data-stu-id="eb05d-834">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="eb05d-835">***Para agregar un fragmento de código mediante el teclado (solo C#)***</span><span class="sxs-lookup"><span data-stu-id="eb05d-835">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="eb05d-836">Coloque el cursor donde desea insertar el código.</span><span class="sxs-lookup"><span data-stu-id="eb05d-836">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="eb05d-837">Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).</span><span class="sxs-lookup"><span data-stu-id="eb05d-837">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="eb05d-838">Observe cómo IntelliSense muestra los nombres de fragmentos de código coincidentes.</span><span class="sxs-lookup"><span data-stu-id="eb05d-838">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="eb05d-839">Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).</span><span class="sxs-lookup"><span data-stu-id="eb05d-839">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="eb05d-840">Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.</span><span class="sxs-lookup"><span data-stu-id="eb05d-840">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="eb05d-841">![Comience a escribir el nombre del fragmento](aspnet-mvc-4-fundamentals/_static/image70.png "comience a escribir el nombre del fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="eb05d-841">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="eb05d-842">*Comience a escribir el nombre del fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="eb05d-842">*Start typing the snippet name*</span></span>

<span data-ttu-id="eb05d-843">![Presione la tecla Tab para seleccionar el fragmento de código resaltada](aspnet-mvc-4-fundamentals/_static/image71.png "presione Tab para seleccionar el fragmento de código resaltada")</span><span class="sxs-lookup"><span data-stu-id="eb05d-843">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="eb05d-844">*Presione la tecla Tab para seleccionar el fragmento de código resaltada*</span><span class="sxs-lookup"><span data-stu-id="eb05d-844">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="eb05d-845">![Vuelva a presionar Tab y el fragmento de código se expandirán](aspnet-mvc-4-fundamentals/_static/image72.png "vuelva a presionar Tab y el fragmento de código se expandirán")</span><span class="sxs-lookup"><span data-stu-id="eb05d-845">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="eb05d-846">*Vuelva a presionar Tab y el fragmento de código se expandirán*</span><span class="sxs-lookup"><span data-stu-id="eb05d-846">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="eb05d-847">***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="eb05d-847">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="eb05d-848">Haga doble clic en el desea insertar el fragmento de código.</span><span class="sxs-lookup"><span data-stu-id="eb05d-848">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="eb05d-849">Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.</span><span class="sxs-lookup"><span data-stu-id="eb05d-849">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="eb05d-850">Seleccione el fragmento de código relevante en la lista, haciendo clic en él.</span><span class="sxs-lookup"><span data-stu-id="eb05d-850">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="eb05d-851">![Con el botón secundario donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](aspnet-mvc-4-fundamentals/_static/image73.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="eb05d-851">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="eb05d-852">*Haga doble clic donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="eb05d-852">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="eb05d-853">![Elegir el fragmento de código relevante en la lista, hacer clic en ella](aspnet-mvc-4-fundamentals/_static/image74.png "elegir el fragmento de código relevante en la lista, hacer clic en ella")</span><span class="sxs-lookup"><span data-stu-id="eb05d-853">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="eb05d-854">*Elegir el fragmento de código relevante en la lista, hacer clic en ella*</span><span class="sxs-lookup"><span data-stu-id="eb05d-854">*Pick the relevant snippet from the list, by clicking on it*</span></span>
