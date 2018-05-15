---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Laboratorio de prácticas: Uno ASP.NET: integrar formularios ASP.NET Web Forms, MVC y API Web | Documentos de Microsoft'
author: rick-anderson
description: ASP.NET es un marco para la creación de sitios Web, aplicaciones y servicios mediante tecnologías especializadas como MVC, Web API y otros. Con la expansión h ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 55109723e566a9f7c66c1a59414377b05dbec760
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a><span data-ttu-id="5b7f6-104">Laboratorio de prácticas: Uno ASP.NET: integrar formularios ASP.NET Web Forms, MVC y API Web</span><span class="sxs-lookup"><span data-stu-id="5b7f6-104">Hands On Lab: One ASP.NET: Integrating ASP.NET Web Forms, MVC and Web API</span></span>
====================
<span data-ttu-id="5b7f6-105">por [Web colonias equipo](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="5b7f6-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="5b7f6-106">Descargar el Kit de aprendizaje de colonias de Web</span><span class="sxs-lookup"><span data-stu-id="5b7f6-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="5b7f6-107">ASP.NET es un marco para la creación de sitios Web, aplicaciones y servicios mediante tecnologías especializadas como MVC, Web API y otros.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-107">ASP.NET is a framework for building Web sites, apps and services using specialized technologies such as MVC, Web API and others.</span></span> <span data-ttu-id="5b7f6-108">Con la expansión de ASP.NET ha visto desde su creación y la expresa necesita tener estas tecnologías integradas, ha habido recientes esfuerzos de trabajo hacia **One ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-108">With the expansion ASP.NET has seen since its creation and the expressed need to have these technologies integrated, there have been recent efforts in working towards **One ASP.NET**.</span></span>
> 
> <span data-ttu-id="5b7f6-109">Visual Studio 2013 presenta un nuevo sistema de proyectos unificada que permite compilar una aplicación y utilizar todas las tecnologías ASP.NET en un proyecto.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-109">Visual Studio 2013 introduces a new unified project system which lets you build an application and use all the ASP.NET technologies in one project.</span></span> <span data-ttu-id="5b7f6-110">Esta característica elimina la necesidad de elegir una tecnología al principio de un proyecto y stick con él y en su lugar, recomienda el uso de varios marcos de trabajo ASP.NET dentro de un proyecto.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-110">This feature eliminates the need to pick one technology at the start of a project and stick with it, and instead encourages the use of multiple ASP.NET frameworks within one project.</span></span>
> 
> <span data-ttu-id="5b7f6-111">Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web colonias, disponible en [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="5b7f6-111">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="5b7f6-112">Información general</span><span class="sxs-lookup"><span data-stu-id="5b7f6-112">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="5b7f6-113">Objetivos</span><span class="sxs-lookup"><span data-stu-id="5b7f6-113">Objectives</span></span>

<span data-ttu-id="5b7f6-114">En este laboratorio práctico, aprenderá cómo:</span><span class="sxs-lookup"><span data-stu-id="5b7f6-114">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="5b7f6-115">Crear un sitio Web basado en la **One ASP.NET** tipo de proyecto</span><span class="sxs-lookup"><span data-stu-id="5b7f6-115">Create a Web site based on the **One ASP.NET** project type</span></span>
- <span data-ttu-id="5b7f6-116">Usar diferentes **ASP.NET** marcos como **MVC** y **API Web** en el mismo proyecto</span><span class="sxs-lookup"><span data-stu-id="5b7f6-116">Use different **ASP.NET** frameworks like **MVC** and **Web API** in the same project</span></span>
- <span data-ttu-id="5b7f6-117">Identificar los componentes principales de un **ASP.NET** aplicación</span><span class="sxs-lookup"><span data-stu-id="5b7f6-117">Identify the main components of an **ASP.NET** application</span></span>
- <span data-ttu-id="5b7f6-118">Aprovechar las ventajas de la **ASP.NET Scaffolding** framework para crear automáticamente los controladores y vistas para llevar a cabo operaciones CRUD en función de las clases de modelo</span><span class="sxs-lookup"><span data-stu-id="5b7f6-118">Take advantage of the **ASP.NET Scaffolding** framework to automatically create Controllers and Views to perform CRUD operations based on your model classes</span></span>
- <span data-ttu-id="5b7f6-119">Exponer el mismo conjunto de información de máquina y legible para las personas formatos usando la herramienta adecuada para cada trabajo</span><span class="sxs-lookup"><span data-stu-id="5b7f6-119">Expose the same set of information in machine- and human-readable formats using the right tool for each job</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="5b7f6-120">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="5b7f6-120">Prerequisites</span></span>

<span data-ttu-id="5b7f6-121">Para completar este laboratorio de prácticas, es necesario lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5b7f6-121">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="5b7f6-122">[Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/) o superior</span><span class="sxs-lookup"><span data-stu-id="5b7f6-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="5b7f6-123">Visual Studio 2013 Update 1</span><span class="sxs-lookup"><span data-stu-id="5b7f6-123">Visual Studio 2013 Update 1</span></span>](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="5b7f6-124">Programa de instalación</span><span class="sxs-lookup"><span data-stu-id="5b7f6-124">Setup</span></span>

<span data-ttu-id="5b7f6-125">Para ejecutar los ejercicios en este laboratorio práctico, debe configurar primero el entorno.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-125">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="5b7f6-126">Abra el Explorador de Windows y vaya a la práctica **origen** carpeta.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-126">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="5b7f6-127">Haga doble clic en **Setup.cmd** y seleccione **ejecutar como administrador** para iniciar el proceso de instalación que configure el entorno e instalará los fragmentos de código de Visual Studio para este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-127">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="5b7f6-128">Si se muestra el cuadro de diálogo de Control de cuentas de usuario, confirme la acción para continuar.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-128">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="5b7f6-129">Asegúrese de que ha activado todas las dependencias para este laboratorio antes de ejecutar el programa de instalación.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-129">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="5b7f6-130">Uso de los fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="5b7f6-130">Using the Code Snippets</span></span>

<span data-ttu-id="5b7f6-131">En este documento de laboratorio, le indicará que insertar bloques de código.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="5b7f6-132">Para su comodidad, la mayor parte de este código se proporciona como fragmentos de código de Visual Studio, puede tener acceso desde dentro de Visual Studio 2013 para evitar tener que agregarla manualmente.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-132">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="5b7f6-133">Cada ejercicio viene acompañado por una solución de inicia ubicada en el **comenzar** carpeta del ejercicio que le permite seguir cada ejercicio independientemente de las otras.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-133">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="5b7f6-134">Ten en cuenta que los fragmentos de código que se agregan durante un ejercicio se encuentran desde estas a partir de soluciones y no pueden funcionar hasta que haya completado el ejercicio.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-134">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="5b7f6-135">En el código fuente de un ejercicio, también encontrará un **final** carpeta que contiene una solución de Visual Studio con el código que se obtiene al completar los pasos en el ejercicio correspondiente.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-135">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="5b7f6-136">Puede usar estas soluciones como guía si necesita ayuda adicional cuando se trabaja a través de este laboratorio práctico.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-136">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="5b7f6-137">Ejercicios</span><span class="sxs-lookup"><span data-stu-id="5b7f6-137">Exercises</span></span>

<span data-ttu-id="5b7f6-138">Este laboratorio de prácticas incluye los ejercicios siguientes:</span><span class="sxs-lookup"><span data-stu-id="5b7f6-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="5b7f6-139">Crear un nuevo proyecto de formularios Web</span><span class="sxs-lookup"><span data-stu-id="5b7f6-139">Creating a New Web Forms Project</span></span>](#Exercise1)
2. [<span data-ttu-id="5b7f6-140">Crear un controlador MVC con Scaffolding</span><span class="sxs-lookup"><span data-stu-id="5b7f6-140">Creating an MVC Controller Using Scaffolding</span></span>](#Exercise2)
3. [<span data-ttu-id="5b7f6-141">Creación de un controlador de API Web mediante Scaffolding</span><span class="sxs-lookup"><span data-stu-id="5b7f6-141">Creating a Web API Controller Using Scaffolding</span></span>](#Exercise3)

<span data-ttu-id="5b7f6-142">Tiempo estimado para completar esta práctica: **60 minutos**</span><span class="sxs-lookup"><span data-stu-id="5b7f6-142">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="5b7f6-143">Cuando se inicia Visual Studio por primera vez, debe seleccionar una de las colecciones de configuraciones predefinidas.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-143">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="5b7f6-144">Cada colección predefinida está diseñado para que coincida con un estilo de desarrollo determinado y determina los diseños de ventana, comportamiento del editor, fragmentos de código de IntelliSense y opciones del cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-144">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="5b7f6-145">Los procedimientos descritos en este laboratorio describen las acciones necesarias para realizar una tarea concreta en Visual Studio cuando se usa el **configuración General de desarrollo** colección.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-145">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="5b7f6-146">Si elige una colección de configuraciones diferentes para el entorno de desarrollo, puede haber diferencias en los pasos que debe tener en cuenta.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-146">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a><span data-ttu-id="5b7f6-147">Ejercicio 1: Crear un nuevo proyecto de formularios Web</span><span class="sxs-lookup"><span data-stu-id="5b7f6-147">Exercise 1: Creating a New Web Forms Project</span></span>

<span data-ttu-id="5b7f6-148">En este ejercicio se creará un nuevo sitio de formularios Web Forms en Visual Studio 2013 usando la **One ASP.NET** unificado experiencia en el proyecto, lo que le permitirá integrar fácilmente los componentes de formularios Web Forms, MVC y API Web en la misma aplicación.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-148">In this exercise you will create a new Web Forms site in Visual Studio 2013 using the **One ASP.NET** unified project experience, which will allow you to easily integrate Web Forms, MVC and Web API components in the same application.</span></span> <span data-ttu-id="5b7f6-149">Podrá explorar la solución generada e identificar sus partes, y por último, verá el sitio Web en acción.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-149">You will then explore the generated solution and identify its parts, and finally you will see the Web site in action.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a><span data-ttu-id="5b7f6-150">Tarea 1: crear un sitio nuevo con una experiencia de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5b7f6-150">Task 1 – Creating a New Site Using the One ASP.NET Experience</span></span>

<span data-ttu-id="5b7f6-151">En esta tarea se iniciará crea un nuevo sitio Web en Visual Studio basado en la **One ASP.NET** tipo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-151">In this task you will start creating a new Web site in Visual Studio based on the **One ASP.NET** project type.</span></span> <span data-ttu-id="5b7f6-152">**Uno ASP.NET** unifica todas las tecnologías ASP.NET y ofrece la opción de mezclar y hacerlos coincidir según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-152">**One ASP.NET** unifies all ASP.NET technologies and gives you the option to mix and match them as desired.</span></span> <span data-ttu-id="5b7f6-153">A continuación, reconocerá los diferentes componentes de formularios Web Forms, MVC y API Web que se encuentran en paralelo dentro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-153">You will then recognize the different components of Web Forms, MVC and Web API that live side by side within your application.</span></span>

1. <span data-ttu-id="5b7f6-154">Abra **Visual Studio Express 2013 para Web** y seleccione **archivo | Nuevo proyecto...**  para iniciar una nueva solución.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-154">Open **Visual Studio Express 2013 for Web** and select **File | New Project...** to start a new solution.</span></span>

    ![Crear un proyecto nuevo](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    <span data-ttu-id="5b7f6-156">*Crear un nuevo proyecto*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-156">*Creating a New Project*</span></span>
2. <span data-ttu-id="5b7f6-157">En el **nuevo proyecto** cuadro de diálogo, seleccione **aplicación Web ASP.NET** en el **Visual C# | Web** pestaña y asegúrese de que **.NET Framework 4.5** está seleccionada.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-157">In the **New Project** dialog box, select **ASP.NET Web Application** under the **Visual C# | Web** tab, and make sure **.NET Framework 4.5** is selected.</span></span> <span data-ttu-id="5b7f6-158">Denomine el proyecto *MyHybridSite*, elija un **ubicación** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-158">Name the project *MyHybridSite*, choose a **Location** and click **OK**.</span></span>

    ![Nuevo proyecto de aplicación Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    <span data-ttu-id="5b7f6-160">*Crear un nuevo proyecto de aplicación Web ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-160">*Creating a new ASP.NET Web Application project*</span></span>
3. <span data-ttu-id="5b7f6-161">En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione la **formularios Web Forms** plantilla y seleccione el **MVC** y **API Web** opciones.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-161">In the **New ASP.NET Project** dialog box, select the **Web Forms** template and select the **MVC** and **Web API** options.</span></span> <span data-ttu-id="5b7f6-162">Además, asegúrese de que el **autenticación** opción está establecida en **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-162">Also, make sure that the **Authentication** option is set to **Individual User Accounts**.</span></span> <span data-ttu-id="5b7f6-163">Haga clic en **Aceptar** para continuar.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-163">Click **OK** to continue.</span></span>

    ![Crear un nuevo proyecto con la plantilla de formularios Web Forms, incluidos los componentes de API Web y MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    <span data-ttu-id="5b7f6-165">*Crear un nuevo proyecto con la plantilla de formularios Web Forms, incluidos los componentes de API Web y MVC*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-165">*Creating a new project with the Web Forms template, including Web API and MVC components*</span></span>
4. <span data-ttu-id="5b7f6-166">Ahora puede explorar la estructura de la solución generada.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-166">You can now explore the structure of the generated solution.</span></span>

    ![Explorar la solución generada](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    <span data-ttu-id="5b7f6-168">*Explorar la solución generada*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-168">*Exploring the generated solution*</span></span>

    1. <span data-ttu-id="5b7f6-169">**Cuenta:** esta carpeta contiene las páginas de formularios Web Forms para registrar, inicie sesión en y administrar cuentas de usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-169">**Account:** This folder contains the Web Form pages to register, log in to and manage the application's user accounts.</span></span> <span data-ttu-id="5b7f6-170">Esta carpeta se agrega cuando el **cuentas de usuario individuales** se selecciona la opción de autenticación durante la configuración de la plantilla de proyecto de formularios Web Forms.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-170">This folder is added when the **Individual User Accounts** authentication option is selected during the configuration of the Web Forms project template.</span></span>
    2. <span data-ttu-id="5b7f6-171">**Modelos:** esta carpeta contiene las clases que representan los datos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-171">**Models:** This folder will contain the classes that represent your application data.</span></span>
    3. <span data-ttu-id="5b7f6-172">**Controladores de** y **vistas**: estas carpetas son necesarias para la **ASP.NET MVC** y **ASP.NET Web API** componentes.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-172">**Controllers** and **Views**: These folders are required for the **ASP.NET MVC** and **ASP.NET Web API** components.</span></span> <span data-ttu-id="5b7f6-173">Explorará las tecnologías MVC y API Web en los ejercicios siguientes.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-173">You will explore the MVC and Web API technologies in the next exercises.</span></span>
    4. <span data-ttu-id="5b7f6-174">El **Default.aspx**, **Contact.aspx** y **About.aspx** archivos son páginas de formularios Web Forms predefinidas que puede usar como punto de partida para crear las páginas específicas de su aplicación.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-174">The **Default.aspx**, **Contact.aspx** and **About.aspx** files are pre-defined Web Form pages that you can use as starting points to build the pages specific to your application.</span></span> <span data-ttu-id="5b7f6-175">La lógica de programación de dichos archivos reside en un archivo independiente que se conoce como el &quot;código subyacente&quot; archivo, que tiene un &quot;. aspx.vb&quot; o &quot;. aspx.cs&quot; extensión (en función de la lenguaje utilizado).</span><span class="sxs-lookup"><span data-stu-id="5b7f6-175">The programming logic of those files resides in a separate file referred to as the &quot;code-behind&quot; file, which has an &quot;.aspx.vb&quot; or &quot;.aspx.cs&quot; extension (depending on the language used).</span></span> <span data-ttu-id="5b7f6-176">La lógica de código subyacente se ejecuta en el servidor y genera dinámicamente la salida HTML de la página.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-176">The code-behind logic runs on the server and dynamically produces the HTML output for your page.</span></span>
    5. <span data-ttu-id="5b7f6-177">El **Site.Master** y **Site.Mobile.Master** páginas definen la apariencia y el comportamiento estándar de todas las páginas en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-177">The **Site.Master** and **Site.Mobile.Master** pages define the look and feel and the standard behavior of all the pages in the application.</span></span>
5. <span data-ttu-id="5b7f6-178">Haga doble clic en el **Default.aspx** archivo para explorar el contenido de la página.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-178">Double-click the **Default.aspx** file to explore the content of the page.</span></span>

    ![Explorar la página Default.aspx](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    <span data-ttu-id="5b7f6-180">*Explorar la página Default.aspx*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-180">*Exploring the Default.aspx page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5b7f6-181">El **página** la directiva en la parte superior del archivo define los atributos de la página de formularios Web Forms.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-181">The **Page** directive at the top of the file defines the attributes of the Web Forms page.</span></span> <span data-ttu-id="5b7f6-182">Por ejemplo, el **MasterPageFile** atributo especifica la ruta de acceso al maestro de página: en este caso, el *Site.Master* página y la **Inherits** atributo define el clase de código subyacente de la página para heredar.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-182">For example, the **MasterPageFile** attribute specifies the path to the master page -in this case, the *Site.Master* page- and the **Inherits** attribute defines the code-behind class for the page to inherit.</span></span> <span data-ttu-id="5b7f6-183">Esta clase se encuentra en el archivo determinado por la **código subyacente** atributo.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-183">This class is located in the file determined by the **CodeBehind** attribute.</span></span>
    > 
    > <span data-ttu-id="5b7f6-184">El **asp: Content** control mantiene el contenido real de la página (texto, marcado y controles) y se asigna a un **asp: ContentPlaceHolder** control en la página maestra.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-184">The **asp:Content** control holds the actual content of the page (text, markup and controls) and is mapped to a **asp:ContentPlaceHolder** control on the master page.</span></span> <span data-ttu-id="5b7f6-185">En este caso, el contenido de la página se representará dentro de la *MainContent* control definido en el *Site.Master* página.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-185">In this case, the page content will be rendered inside the *MainContent* control defined in the *Site.Master* page.</span></span>
6. <span data-ttu-id="5b7f6-186">Expanda el **aplicación\_iniciar** carpeta y observe el **WebApiConfig.cs** archivo.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-186">Expand the **App\_Start** folder and notice the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="5b7f6-187">Visual Studio incluye ese archivo en la solución generada porque incluye API Web al configurar el proyecto con la plantilla de One ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-187">Visual Studio included that file in the generated solution because you included Web API when configuring your project with the One ASP.NET template.</span></span>
7. <span data-ttu-id="5b7f6-188">Abra la **WebApiConfig.cs** archivo.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-188">Open the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="5b7f6-189">En el *WebApiConfig* clase encontrará la configuración asociada a la API Web, que se asigna HTTP enruta a **controladores de API Web**.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-189">In the *WebApiConfig* class you will find the configuration associated with Web API, which maps HTTP routes to **Web API controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. <span data-ttu-id="5b7f6-190">Abra la **RouteConfig.cs** archivo.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-190">Open the **RouteConfig.cs** file.</span></span> <span data-ttu-id="5b7f6-191">Dentro de la *RegisterRoutes* encontrará la configuración asociada a MVC, que se asigna la ruta HTTP para el método **controladores MVC**.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-191">Inside the *RegisterRoutes* method you will find the configuration associated with MVC, which maps HTTP routes to **MVC controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="5b7f6-192">Tarea 2: ejecutar la solución</span><span class="sxs-lookup"><span data-stu-id="5b7f6-192">Task 2 – Running the Solution</span></span>

<span data-ttu-id="5b7f6-193">En esta tarea se ejecute la solución generada, explorar la aplicación y algunas de sus características, como la reescritura de direcciones URL y la autenticación integrada.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-193">In this task you will run the generated solution, explore the app and some of its features, like URL rewriting and built-in authentication.</span></span>

1. <span data-ttu-id="5b7f6-194">Para ejecutar la solución, presione **F5** o haga clic en el **iniciar** situado en la barra de herramientas.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-194">To run the solution, press **F5** or click the **Start** button located on the toolbar.</span></span> <span data-ttu-id="5b7f6-195">Debe abrir la página de inicio de la aplicación en el explorador.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-195">The application home page should open in the browser.</span></span>

    ![Ejecución de la solución](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. <span data-ttu-id="5b7f6-197">Compruebe que se está llamando a las páginas de formularios Web Forms.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-197">Verify that the Web Forms pages are being invoked.</span></span> <span data-ttu-id="5b7f6-198">Para ello, anexe **/contact.aspx** a la dirección URL en la barra de direcciones y presione **ENTRAR**.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-198">To do this, append **/contact.aspx** to the URL in the address bar and press **Enter**.</span></span>

    ![Direcciones URL descriptivas](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    <span data-ttu-id="5b7f6-200">*Direcciones URL descriptivas*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-200">*Friendly URLs*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5b7f6-201">Como puede ver, la dirección URL cambia a **o póngase en contacto con**.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-201">As you can see, the URL changes to **/contact**.</span></span> <span data-ttu-id="5b7f6-202">A partir de **ASP.NET 4**, capacidades de enrutamiento de dirección URL se agregaron a formularios Web Forms, por lo que puede escribir las direcciones URL como *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* en lugar de  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-202">Starting from **ASP.NET 4**, URL routing capabilities were added to Web Forms, so you can write URLs like *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* instead of *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span></span> <span data-ttu-id="5b7f6-203">Para obtener más información, consulte [enrutamiento de direcciones URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span><span class="sxs-lookup"><span data-stu-id="5b7f6-203">For more information refer to [URL Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span></span>
3. <span data-ttu-id="5b7f6-204">Ahora explorará el flujo de autenticación que se integra en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-204">You will now explore the authentication flow integrated into the application.</span></span> <span data-ttu-id="5b7f6-205">Para ello, haga clic en **registrar** en la esquina superior derecha de la página.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-205">To do this, click **Register** in the upper-right corner of the page.</span></span>

    ![Registrar un nuevo usuario](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    <span data-ttu-id="5b7f6-207">*Registrar un nuevo usuario*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-207">*Registering a new user*</span></span>
4. <span data-ttu-id="5b7f6-208">En el **registrar** página, escriba un **nombre de usuario** y **contraseña**y, a continuación, haga clic en **registrar**.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-208">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    ![Página registrar](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    <span data-ttu-id="5b7f6-210">*Página registrar*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-210">*Register page*</span></span>
5. <span data-ttu-id="5b7f6-211">La aplicación registra la nueva cuenta, y el usuario está autenticado.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-211">The application registers the new account, and the user is authenticated.</span></span>

    ![Usuario autenticado](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    <span data-ttu-id="5b7f6-213">*Usuario autenticado*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-213">*User authenticated*</span></span>
6. <span data-ttu-id="5b7f6-214">Vuelva a Visual Studio y presione **MAYÚS + F5** para detener la depuración.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-214">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a><span data-ttu-id="5b7f6-215">Ejercicio 2: Crear un controlador MVC con Scaffolding</span><span class="sxs-lookup"><span data-stu-id="5b7f6-215">Exercise 2: Creating an MVC Controller Using Scaffolding</span></span>

<span data-ttu-id="5b7f6-216">En este ejercicio tendrá aprovechar el marco de trabajo de ASP.NET Scaffolding proporcionada por Visual Studio para crear un controlador de ASP.NET MVC 5 con acciones y vistas de Razor para realizar operaciones CRUD, sin escribir una sola línea de código.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-216">In this exercise you will take advantage of the ASP.NET Scaffolding framework provided by Visual Studio to create an ASP.NET MVC 5 controller with actions and Razor views to perform CRUD operations, without writing a single line of code.</span></span> <span data-ttu-id="5b7f6-217">El proceso de scaffolding Utilice Entity Framework Code First para generar el contexto de datos y el esquema de base de datos en la base de datos SQL.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-217">The scaffolding process will use Entity Framework Code First to generate the data context and the database schema in the SQL database.</span></span>

<span data-ttu-id="5b7f6-218">**Acerca de Entity Framework Code First**</span><span class="sxs-lookup"><span data-stu-id="5b7f6-218">**About Entity Framework Code First**</span></span>

<span data-ttu-id="5b7f6-219">Entity Framework (EF) es un asignador relacional de objetos (ORM) que le permite crear aplicaciones de acceso a datos programando con un modelo de aplicación conceptual en lugar de programar directamente con un esquema de almacenamiento relacional.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-219">Entity Framework (EF) is an object-relational mapper (ORM) that enables you to create data access applications by programming with a conceptual application model instead of programming directly using a relational storage schema.</span></span>

<span data-ttu-id="5b7f6-220">El flujo de trabajo de modelado Entity Framework Code First le permite utilizar sus propias clases de dominio para representar el modelo que dependa de EF al realizar la consulta, las funciones de seguimiento de cambios y actualizaciones.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-220">The Entity Framework Code First modeling workflow allows you to use your own domain classes to represent the model that EF relies on when performing querying, change-tracking and updating functions.</span></span> <span data-ttu-id="5b7f6-221">Mediante el flujo de trabajo de desarrollo Code First, es necesario iniciar la aplicación mediante la creación de una base de datos o especificar un esquema.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-221">Using the Code First development workflow, you do not need to begin your application by creating a database or specifying a schema.</span></span> <span data-ttu-id="5b7f6-222">En su lugar, puede escribir las clases de .NET estándar que definen los objetos de modelo de dominio más adecuados para su aplicación y Entity Framework creará la base de datos por usted.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-222">Instead, you can write standard .NET classes that define the most appropriate domain model objects for your application, and Entity Framework will create the database for you.</span></span>

> [!NOTE]
> <span data-ttu-id="5b7f6-223">Puede aprender más acerca de Entity Framework [aquí](../../../entity-framework.md).</span><span class="sxs-lookup"><span data-stu-id="5b7f6-223">You can learn more about Entity Framework [here](../../../entity-framework.md).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a><span data-ttu-id="5b7f6-224">Tarea 1: crear un nuevo modelo</span><span class="sxs-lookup"><span data-stu-id="5b7f6-224">Task 1 – Creating a New Model</span></span>

<span data-ttu-id="5b7f6-225">Ahora va a definir un **persona** (clase), que será el modelo utilizado por el proceso de scaffolding para crear el controlador MVC y las vistas.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-225">You will now define a **Person** class, which will be the model used by the scaffolding process to create the MVC controller and the views.</span></span> <span data-ttu-id="5b7f6-226">Primero debe crear un **persona** clase de modelo y las operaciones CRUD en el controlador se crearán automáticamente con características de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-226">You will start by creating a **Person** model class, and the CRUD operations in the controller will be automatically created using scaffolding features.</span></span>

1. <span data-ttu-id="5b7f6-227">Abra **Visual Studio Express 2013 para Web** y **MyHybridSite.sln** soluciones se encuentran en la **origen/Ex2-MvcScaffolding/Begin** carpeta.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-227">Open **Visual Studio Express 2013 for Web** and the **MyHybridSite.sln** solution located in the **Source/Ex2-MvcScaffolding/Begin** folder.</span></span> <span data-ttu-id="5b7f6-228">Como alternativa, puede continuar con la solución que obtuvo en el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-228">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="5b7f6-229">En **el Explorador de soluciones**, haga clic en el **modelos** carpeta de la **MyHybridSite** de proyecto y seleccione **agregar | Clase...** .</span><span class="sxs-lookup"><span data-stu-id="5b7f6-229">In **Solution Explorer**, right-click the **Models** folder of the **MyHybridSite** project and select **Add | Class...**.</span></span>

    ![Agregar la clase de modelo de persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    <span data-ttu-id="5b7f6-231">*Agregar la clase de modelo de persona*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-231">*Adding the Person model class*</span></span>
3. <span data-ttu-id="5b7f6-232">En el **Agregar nuevo elemento** cuadro de diálogo, el nombre del archivo *Person.cs* y haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-232">In the **Add New Item** dialog box, name the file *Person.cs* and click **Add**.</span></span>

    ![Crear la clase de modelo de persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    <span data-ttu-id="5b7f6-234">*Crear la clase de modelo de persona*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-234">*Creating the Person model class*</span></span>
4. <span data-ttu-id="5b7f6-235">Reemplace el contenido de la **Person.cs** archivo con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-235">Replace the content of the **Person.cs** file with the following code.</span></span> <span data-ttu-id="5b7f6-236">Presione **CTRL + S** para guardar los cambios.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-236">Press **CTRL + S** to save the changes.</span></span>

    <span data-ttu-id="5b7f6-237">(Código de fragmento de código: *PersonClass BringingTogetherOneAspNet - Ex2 -*)</span><span class="sxs-lookup"><span data-stu-id="5b7f6-237">(Code Snippet - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. <span data-ttu-id="5b7f6-238">En **el Explorador de soluciones**, haga clic en el **MyHybridSite** de proyecto y seleccione **generar**, o bien presione **CTRL + MAYÚS + B** para compilar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-238">In **Solution Explorer**, right-click the **MyHybridSite** project and select **Build**, or press **CTRL + SHIFT + B** to build the project.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a><span data-ttu-id="5b7f6-239">Tarea 2: crear un controlador MVC</span><span class="sxs-lookup"><span data-stu-id="5b7f6-239">Task 2 – Creating an MVC Controller</span></span>

<span data-ttu-id="5b7f6-240">Ahora que el **persona** se crea un modelo, se utilizará el scaffolding de ASP.NET MVC con Entity Framework para crear las acciones del controlador CRUD y vistas para **persona**.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-240">Now that the **Person** model is created, you will use ASP.NET MVC scaffolding with Entity Framework to create the CRUD controller actions and views for **Person**.</span></span>

1. <span data-ttu-id="5b7f6-241">En **el Explorador de soluciones**, haga clic en el **controladores** carpeta de la **MyHybridSite** de proyecto y seleccione **agregar | Nuevo elemento con scaffolding...** .</span><span class="sxs-lookup"><span data-stu-id="5b7f6-241">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Crear un nuevo controlador con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    <span data-ttu-id="5b7f6-243">*Crear un nuevo controlador de scaffolding*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-243">*Creating a new Scaffolded Controller*</span></span>
2. <span data-ttu-id="5b7f6-244">En el **agregar scaffolding** cuadro de diálogo, seleccione **controlador de MVC 5 con vistas, mediante Entity Framework** y, a continuación, haga clic en **agregar.**</span><span class="sxs-lookup"><span data-stu-id="5b7f6-244">In the **Add Scaffold** dialog box, select **MVC 5 Controller with views, using Entity Framework** and then click **Add.**</span></span>

    ![Seleccionar controlador de MVC 5 con vistas y Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    <span data-ttu-id="5b7f6-246">*Seleccionar controlador de MVC 5 con vistas y Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-246">*Selecting MVC 5 Controller with views and Entity Framework*</span></span>
3. <span data-ttu-id="5b7f6-247">Establecer *MvcPersonController* como el **nombre de controlador**, seleccione la **utilizar las acciones de controlador asincrónico** opción y seleccione **persona (MyHybridSite.Models)**  como el **clase modelo**.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-247">Set *MvcPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** as the **Model class**.</span></span>

    ![Agregar un controlador de MVC con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    <span data-ttu-id="5b7f6-249">*Agregar un controlador de MVC con scaffolding*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-249">*Adding an MVC controller with scaffolding*</span></span>
4. <span data-ttu-id="5b7f6-250">En **clase de contexto de datos**, haga clic en **nuevo contexto de datos...** .</span><span class="sxs-lookup"><span data-stu-id="5b7f6-250">Under **Data context class**, click **New data context...**.</span></span>

    ![Crear un nuevo contexto de datos](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    <span data-ttu-id="5b7f6-252">*Crear un nuevo contexto de datos*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-252">*Creating a new data context*</span></span>
5. <span data-ttu-id="5b7f6-253">En el **nuevo contexto de datos** nombre el nuevo contexto de datos de cuadro de diálogo *PersonContext* y haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-253">In the **New Data Context** dialog box, name the new data context *PersonContext* and click **Add**.</span></span>

    ![Crear la nueva PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    <span data-ttu-id="5b7f6-255">*Crear el nuevo tipo de PersonContext*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-255">*Creating the new PersonContext type*</span></span>
6. <span data-ttu-id="5b7f6-256">Haga clic en **agregar** para crear el nuevo controlador de **persona** con la técnica scaffolding.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-256">Click **Add** to create the new controller for **Person** with scaffolding.</span></span> <span data-ttu-id="5b7f6-257">Visual Studio, a continuación, generará las acciones de controlador, el contexto de datos de la persona y las vistas de Razor.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-257">Visual Studio will then generate the controller actions, the Person data context and the Razor views.</span></span>

    ![Después de crear el controlador MVC con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    <span data-ttu-id="5b7f6-259">*Después de crear el controlador MVC con scaffolding*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-259">*After creating the MVC controller with scaffolding*</span></span>
7. <span data-ttu-id="5b7f6-260">Abra la **MvcPersonController.cs** un archivo en el **controladores** carpeta.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-260">Open the **MvcPersonController.cs** file in the **Controllers** folder.</span></span> <span data-ttu-id="5b7f6-261">Tenga en cuenta que los métodos de acción CRUD se han generado automáticamente.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-261">Notice that the CRUD action methods have been generated automatically.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="5b7f6-262">Si selecciona el **utilizar las acciones de controlador asincrónico** opciones de casilla de verificación de la técnica scaffolding en los pasos anteriores, Visual Studio genera los métodos de acción asincrónicos para todas las acciones que implican el acceso en el contexto de datos de la persona.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-262">By selecting the **Use async controller actions** check box from the scaffolding options in the previous steps, Visual Studio generates asynchronous action methods for all actions that involve access to the Person data context.</span></span> <span data-ttu-id="5b7f6-263">Se recomienda utilizar métodos de acción asincrónicos de larga duración, no con la CPU solicitudes para evitar el bloqueo en el servidor Web realizar trabajo mientras se procesa la solicitud.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-263">It is recommended that you use asynchronous action methods for long-running, non-CPU bound requests to avoid blocking the Web server from performing work while the request is being processed.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="5b7f6-264">Tarea 3: ejecutar la solución</span><span class="sxs-lookup"><span data-stu-id="5b7f6-264">Task 3 – Running the Solution</span></span>

<span data-ttu-id="5b7f6-265">En esta tarea, se ejecutará la solución para comprobar que las vistas de **persona** funcionan del modo esperado.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-265">In this task, you will run the solution again to verify that the views for **Person** are working as expected.</span></span> <span data-ttu-id="5b7f6-266">Se agregará una nueva persona para comprobar que se guardó correctamente para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-266">You will add a new person to verify that it is successfully saved to the database.</span></span>

1. <span data-ttu-id="5b7f6-267">Presione **F5** para ejecutar la solución.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-267">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="5b7f6-268">Vaya a **/MvcPerson**.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-268">Navigate to **/MvcPerson**.</span></span> <span data-ttu-id="5b7f6-269">Debería aparecer la vista con scaffolding que muestra la lista de usuarios.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-269">The scaffolded view that shows the list of people should appear.</span></span>
3. <span data-ttu-id="5b7f6-270">Haga clic en **crear nuevo** para agregar una nueva persona.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-270">Click **Create New** to add a new person.</span></span>

    ![Navegar a las vistas MVC con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    <span data-ttu-id="5b7f6-272">*Navegar a las vistas MVC con scaffolding*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-272">*Navigating to the scaffolded MVC views*</span></span>
4. <span data-ttu-id="5b7f6-273">En el **crear** ver, proporcione un **nombre** y un **Age** la persona y haga clic en **crear**.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-273">In the **Create** view, provide a **Name** and an **Age** for the person, and click **Create**.</span></span>

    ![Agregar una nueva persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    <span data-ttu-id="5b7f6-275">*Agregar una nueva persona*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-275">*Adding a new person*</span></span>
5. <span data-ttu-id="5b7f6-276">La persona nueva se agrega a la lista.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-276">The new person is added to the list.</span></span> <span data-ttu-id="5b7f6-277">En la lista de elementos, haga clic en **detalles** para mostrar la vista de detalles de la persona.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-277">In the element list, click **Details** to display the person's details view.</span></span> <span data-ttu-id="5b7f6-278">A continuación, en la **detalles** ver, haga clic en **volver a la lista** para volver a la vista de lista.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-278">Then, in the **Details** view, click **Back to List** to go back to the list view.</span></span>

    ![Vista de detalles de la persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    <span data-ttu-id="5b7f6-280">*Vista de detalles de la persona*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-280">*Person's details view*</span></span>
6. <span data-ttu-id="5b7f6-281">Haga clic en el **eliminar** vínculo Eliminar de la persona.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-281">Click the **Delete** link to delete the person.</span></span> <span data-ttu-id="5b7f6-282">En el **eliminar** ver, haga clic en **eliminar** para confirmar la operación.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-282">In the **Delete** view, click **Delete** to confirm the operation.</span></span>

    ![Eliminación de una persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    <span data-ttu-id="5b7f6-284">*Eliminación de una persona*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-284">*Deleting a person*</span></span>
7. <span data-ttu-id="5b7f6-285">Vuelva a Visual Studio y presione **MAYÚS + F5** para detener la depuración.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-285">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a><span data-ttu-id="5b7f6-286">Ejercicio 3: Crear un controlador de API Web mediante Scaffolding</span><span class="sxs-lookup"><span data-stu-id="5b7f6-286">Exercise 3: Creating a Web API Controller Using Scaffolding</span></span>

<span data-ttu-id="5b7f6-287">El marco Web API forma parte de la pila de ASP.NET y diseñado para facilitar la implementación de los servicios HTTP, por lo general, enviar y recibir datos con formato JSON o XML a través de la API de REST.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-287">The Web API framework is part of the ASP.NET Stack and designed to make implementing HTTP services easier, generally sending and receiving JSON- or XML-formatted data through a RESTful API.</span></span>

<span data-ttu-id="5b7f6-288">En este ejercicio, se usará Scaffolding de ASP.NET nuevo para generar un controlador de Web API.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-288">In this exercise, you will use ASP.NET Scaffolding again to generate a Web API controller.</span></span> <span data-ttu-id="5b7f6-289">Utilizará las mismas **persona** y **PersonContext** clases desde el ejercicio anterior para proporcionar los mismos datos de la persona en formato JSON.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-289">You will use the same **Person** and **PersonContext** classes from the previous exercise to provide the same person data in JSON format.</span></span> <span data-ttu-id="5b7f6-290">Verá cómo puede exponer los mismos recursos de maneras diferentes dentro de la misma aplicación de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-290">You will see how you can expose the same resources in different ways within the same ASP.NET application.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a><span data-ttu-id="5b7f6-291">Tarea 1: creación de un controlador de API Web</span><span class="sxs-lookup"><span data-stu-id="5b7f6-291">Task 1 – Creating a Web API Controller</span></span>

<span data-ttu-id="5b7f6-292">En esta tarea se creará una nueva **controlador de API Web** que va a exponer los datos de la persona en un formato de equipo puede consumir como JSON.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-292">In this task you will create a new **Web API Controller** that will expose the person data in a machine-consumable format like JSON.</span></span>

1. <span data-ttu-id="5b7f6-293">Si aún no está abierto, abra **Visual Studio Express 2013 para Web** y abra el **MyHybridSite.sln** soluciones se encuentran en la **origen/Ex3-WebAPI/Begin** carpeta.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-293">If not already opened, open **Visual Studio Express 2013 for Web** and open the **MyHybridSite.sln** solution located in the **Source/Ex3-WebAPI/Begin** folder.</span></span> <span data-ttu-id="5b7f6-294">Como alternativa, puede continuar con la solución que obtuvo en el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-294">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5b7f6-295">Si empieza con la solución de inicio de ejercicio 3, presione **CTRL + MAYÚS + B** para compilar la solución.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-295">If you start with the Begin solution from Exercise 3, press **CTRL + SHIFT + B** to build the solution.</span></span>
2. <span data-ttu-id="5b7f6-296">En **el Explorador de soluciones**, haga clic en el **controladores** carpeta de la **MyHybridSite** de proyecto y seleccione **agregar | Nuevo elemento con scaffolding...** .</span><span class="sxs-lookup"><span data-stu-id="5b7f6-296">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Crear un nuevo controlador con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    <span data-ttu-id="5b7f6-298">*Crear un nuevo controlador con scaffolding*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-298">*Creating a new scaffolded Controller*</span></span>
3. <span data-ttu-id="5b7f6-299">En el **agregar scaffolding** cuadro de diálogo, seleccione **API Web** en el panel izquierdo, a continuación, **Web API 2 controlador con acciones que usan Entity Framework** en el panel central y, a continuación, haga clic en  **Agregar.**</span><span class="sxs-lookup"><span data-stu-id="5b7f6-299">In the **Add Scaffold** dialog box, select **Web API** in the left pane, then **Web API 2 Controller with actions, using Entity Framework** in the middle pane and then click **Add.**</span></span>

    <span data-ttu-id="5b7f6-300">![Seleccionar controlador de Web API 2 con acciones y Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "seleccionar controlador de Web API 2 con acciones y Entity Framework")</span><span class="sxs-lookup"><span data-stu-id="5b7f6-300">![Selecting Web API 2 Controller with actions and Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selecting Web API 2 Controller with actions and Entity Framework")</span></span>

    <span data-ttu-id="5b7f6-301">*Seleccionar controlador de Web API 2 con acciones y Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-301">*Selecting Web API 2 Controller with actions and Entity Framework*</span></span>
4. <span data-ttu-id="5b7f6-302">Establecer *ApiPersonController* como el **nombre de controlador**, seleccione la **utilizar las acciones de controlador asincrónico** opción y seleccione **persona (MyHybridSite.Models)**  y **PersonContext (MyHybridSite.Models)** como el **modelo** y **contexto de datos** clases respectivamente.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-302">Set *ApiPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** and **PersonContext (MyHybridSite.Models)** as the **Model** and **Data context** classes respectively.</span></span> <span data-ttu-id="5b7f6-303">A continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-303">Then click **Add**.</span></span>

    <span data-ttu-id="5b7f6-304">![Cómo agregar un controlador de API Web con la técnica scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "agregando un controlador de Web API con la técnica scaffolding")</span><span class="sxs-lookup"><span data-stu-id="5b7f6-304">![Adding a Web API Controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Adding a Web API controller with scaffolding")</span></span>

    <span data-ttu-id="5b7f6-305">*Agregar un controlador de Web API con la técnica scaffolding*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-305">*Adding a Web API controller with scaffolding*</span></span>
5. <span data-ttu-id="5b7f6-306">Visual Studio, a continuación, generará el **ApiPersonController** clase con las cuatro acciones CRUD para trabajar con los datos.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-306">Visual Studio will then generate the **ApiPersonController** class with the four CRUD actions to work with your data.</span></span>

    <span data-ttu-id="5b7f6-307">![Después de crear el controlador de Web API con la técnica scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "después de crear el controlador de Web API con la técnica scaffolding")</span><span class="sxs-lookup"><span data-stu-id="5b7f6-307">![After creating the Web API controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "After creating the Web API controller with scaffolding")</span></span>

    <span data-ttu-id="5b7f6-308">*Después de crear el controlador de Web API con la técnica scaffolding*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-308">*After creating the Web API controller with scaffolding*</span></span>
6. <span data-ttu-id="5b7f6-309">Abra la **ApiPersonController.cs** archivo e inspeccione el *GetPeople* método de acción.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-309">Open the **ApiPersonController.cs** file and inspect the *GetPeople* action method.</span></span> <span data-ttu-id="5b7f6-310">Este método consulta el campo de base de datos de **PersonContext** tipo con el fin de obtener datos de las personas.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-310">This method queries the db field of **PersonContext** type in order to get the people data.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. <span data-ttu-id="5b7f6-311">Ahora observe el comentario encima de la definición de método.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-311">Now notice the comment above the method definition.</span></span> <span data-ttu-id="5b7f6-312">Proporciona el URI que expone esta acción que se va a utilizar en la siguiente tarea.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-312">It provides the URI that exposes this action which you will use in the next task.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="5b7f6-313">De forma predeterminada, la API Web está configurada para detectar las consultas para el */API* ruta de acceso para evitar conflictos con los controladores MVC.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-313">By default, Web API is configured to catch the queries to the */api* path to avoid collisions with MVC controllers.</span></span> <span data-ttu-id="5b7f6-314">Si necesita cambiar esta configuración, consulte [enrutamiento de ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="5b7f6-314">If you need to change this setting, refer to [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="5b7f6-315">Tarea 2: ejecutar la solución</span><span class="sxs-lookup"><span data-stu-id="5b7f6-315">Task 2 – Running the Solution</span></span>

<span data-ttu-id="5b7f6-316">En esta tarea utilizará el Explorador de Internet **herramientas de desarrollo F12** para inspeccionar la respuesta completa desde el controlador de API Web.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-316">In this task you will use the Internet Explorer **F12 developer tools** to inspect the full response from the Web API controller.</span></span> <span data-ttu-id="5b7f6-317">Verá cómo puede capturar el tráfico de red para obtener más información sobre los datos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-317">You will see how you can capture network traffic to get more insight into your application data.</span></span>

> [!NOTE]
> <span data-ttu-id="5b7f6-318">Asegúrese de que **Internet Explorer** está seleccionado en el **iniciar** situado en la barra de herramientas de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-318">Make sure that **Internet Explorer** is selected in the **Start** button located on the Visual Studio toolbar.</span></span>
> 
> ![Opción de Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> <span data-ttu-id="5b7f6-320">El **herramientas de desarrollo F12** tiene un amplio conjunto de funcionalidad que no se trata en este laboratorio prácticas.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-320">The **F12 developer tools** have a wide set of functionality that is not covered in this hands-on-lab.</span></span> <span data-ttu-id="5b7f6-321">Si desea obtener más información, consulte [mediante las herramientas de desarrollo F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span><span class="sxs-lookup"><span data-stu-id="5b7f6-321">If you want to learn more about it, refer to [Using the F12 developer tools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span></span>


1. <span data-ttu-id="5b7f6-322">Presione **F5** para ejecutar la solución.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-322">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5b7f6-323">Para seguir esta tarea correctamente, la aplicación necesita que los datos.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-323">In order to follow this task correctly, your application needs to have data.</span></span> <span data-ttu-id="5b7f6-324">Si la base de datos está vacía, puede volver a la tarea 3 en el ejercicio 2 y siga los pasos sobre cómo crear a una nueva persona mediante las vistas MVC.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-324">If your database is empty, you can go back to Task 3 in Exercise 2 and follow the steps on how to create a new person using the MVC views.</span></span>
2. <span data-ttu-id="5b7f6-325">En el explorador, presione **F12** para abrir el **Developer Tools** panel.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-325">In the browser, press **F12** to open the **Developer Tools** panel.</span></span> <span data-ttu-id="5b7f6-326">Presione **CTRL** + **4** o haga clic en el **red** icono y, a continuación, haga clic en la flecha verde botón para empezar a capturar el tráfico de red.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-326">Press **CTRL** + **4** or click the **Network** icon, and then click the green arrow button to begin capturing network traffic.</span></span>

    <span data-ttu-id="5b7f6-327">![Iniciar la captura de red de la API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "captura de red iniciando Web API")</span><span class="sxs-lookup"><span data-stu-id="5b7f6-327">![Initiating Web API network capture](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Initiating Web API network capture")</span></span>

    <span data-ttu-id="5b7f6-328">*Iniciar la captura de red de la API de Web*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-328">*Initiating Web API network capture*</span></span>
3. <span data-ttu-id="5b7f6-329">Anexar **api/ApiPerson** a la dirección URL en la barra de direcciones del explorador.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-329">Append **api/ApiPerson** to the URL in the browser's address bar.</span></span> <span data-ttu-id="5b7f6-330">Ahora inspeccionará los detalles de la respuesta de la **ApiPersonController**.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-330">You will now inspect the details of the response from the **ApiPersonController**.</span></span>

    <span data-ttu-id="5b7f6-331">![Recuperar datos de la persona a través de Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "recuperar datos de la persona a través de Web API")</span><span class="sxs-lookup"><span data-stu-id="5b7f6-331">![Retrieving person data through Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Retrieving person data through Web API")</span></span>

    <span data-ttu-id="5b7f6-332">*Recuperar datos de la persona a través de Web API*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-332">*Retrieving person data through Web API*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5b7f6-333">Una vez finalizada la descarga, se le pedirá que realice una acción con el archivo descargado.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-333">Once the download finishes, you will be prompted to make an action with the downloaded file.</span></span> <span data-ttu-id="5b7f6-334">Deje el cuadro de diálogo abierto para poder ver el contenido de la respuesta a través de la ventana de herramientas de los desarrolladores.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-334">Leave the dialog box open in order to be able to watch the response content through the Developers Tool window.</span></span>
4. <span data-ttu-id="5b7f6-335">Ahora inspeccionará el cuerpo de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-335">Now you will inspect the body of the response.</span></span> <span data-ttu-id="5b7f6-336">Para ello, haga clic en el **detalles** ficha y, a continuación, haga clic en **cuerpo de respuesta**.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-336">To do this, click the **Details** tab and then click **Response body**.</span></span> <span data-ttu-id="5b7f6-337">Puede comprobar que los datos descargados están una lista de objetos con las propiedades **identificador**, **nombre** y **Age** que corresponden a la **persona** clase.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-337">You can check that the downloaded data is a list of objects with the properties **Id**, **Name** and **Age** that correspond to the **Person** class.</span></span>

    <span data-ttu-id="5b7f6-338">![Ver el cuerpo de respuesta de la API de Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "ver cuerpo de respuesta de la API de Web")</span><span class="sxs-lookup"><span data-stu-id="5b7f6-338">![Viewing Web API Response Body](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Viewing Web API Response Body")</span></span>

    <span data-ttu-id="5b7f6-339">*Cuerpo de respuesta mediante una API Web de visualización*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-339">*Viewing Web API Response Body*</span></span>

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a><span data-ttu-id="5b7f6-340">Tarea 3: adición de páginas de Ayuda de API de Web</span><span class="sxs-lookup"><span data-stu-id="5b7f6-340">Task 3 – Adding Web API Help Pages</span></span>

<span data-ttu-id="5b7f6-341">Cuando se crea una API Web, resulta útil crear una página de ayuda para que otros desarrolladores sepan cómo llamar a la API.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-341">When you create a Web API, it is useful to create a help page so that other developers will know how to call your API.</span></span> <span data-ttu-id="5b7f6-342">Puede crear y actualizar manualmente las páginas de documentación, pero es mejor para evitar tener que realizar trabajo de mantenimiento genere automáticamente.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-342">You could create and update the documentation pages manually, but it is better to auto-generate them to avoid having to do maintenance work.</span></span> <span data-ttu-id="5b7f6-343">En esta tarea usará un paquete de Nuget para generar automáticamente las páginas de Ayuda de API Web a la solución.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-343">In this task you will use a Nuget package to automatically generate Web API help pages to the solution.</span></span>

1. <span data-ttu-id="5b7f6-344">Desde el **herramientas** menú en Visual Studio, seleccione **Administrador de paquetes de biblioteca**y, a continuación, haga clic en **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-344">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="5b7f6-345">En el **Package Manager Console** ventana, ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="5b7f6-345">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > <span data-ttu-id="5b7f6-346">El **Microsoft.AspNet.WebApi.HelpPage** paquete instala los ensamblados necesarios y agrega las vistas de MVC para las páginas de ayuda en la **áreas/HelpPage** carpeta.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-346">The **Microsoft.AspNet.WebApi.HelpPage** package installs the necessary assemblies and adds MVC Views for the help pages under the **Areas/HelpPage** folder.</span></span>

    <span data-ttu-id="5b7f6-347">![Área de HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage área")</span><span class="sxs-lookup"><span data-stu-id="5b7f6-347">![HelpPage Area](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage Area")</span></span>

    <span data-ttu-id="5b7f6-348">*Área de HelpPage*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-348">*HelpPage Area*</span></span>
3. <span data-ttu-id="5b7f6-349">De forma predeterminada, la Ayuda de las páginas tienen cadenas de marcador de posición para la documentación.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-349">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="5b7f6-350">Puede utilizar comentarios de documentación XML para crear la documentación.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-350">You can use XML documentation comments to create the documentation.</span></span> <span data-ttu-id="5b7f6-351">Para habilitar esta característica, abra la **HelpPageConfig.cs** archivo se encuentra en la **áreas/HelpPage/aplicación\_iniciar** carpeta y elimine la línea siguiente:</span><span class="sxs-lookup"><span data-stu-id="5b7f6-351">To enable this feature, open the **HelpPageConfig.cs** file located in the **Areas/HelpPage/App\_Start** folder and uncomment the following line:</span></span>

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. <span data-ttu-id="5b7f6-352">En **el Explorador de soluciones**, haga clic en el proyecto **MyHybridSite**, seleccione **propiedades** y haga clic en el **generar** ficha.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-352">In **Solution Explorer**, right-click the project **MyHybridSite**, select **Properties** and click the **Build** tab.</span></span>

    <span data-ttu-id="5b7f6-353">![Pestaña compilar](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "crear sección")</span><span class="sxs-lookup"><span data-stu-id="5b7f6-353">![Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Build section")</span></span>

    <span data-ttu-id="5b7f6-354">*Generar (ficha)*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-354">*Build tab*</span></span>
5. <span data-ttu-id="5b7f6-355">En **salida**, seleccione **archivo de documentación XML**.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-355">Under **Output**, select **XML documentation file**.</span></span> <span data-ttu-id="5b7f6-356">En el cuadro de edición, escriba **aplicación\_Data/XmlDocument.xml**.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-356">In the edit box, type **App\_Data/XmlDocument.xml**.</span></span>

    <span data-ttu-id="5b7f6-357">![Salida de la sección en la ficha generar](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "sección en la pestaña de la compilación de salida")</span><span class="sxs-lookup"><span data-stu-id="5b7f6-357">![Output section in Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Output section in build tab")</span></span>

    <span data-ttu-id="5b7f6-358">*Sección de salida en la ficha generar*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-358">*Output section in Build tab*</span></span>
6. <span data-ttu-id="5b7f6-359">Presione **CTRL** + **S** para guardar los cambios.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-359">Press **CTRL** + **S** to save the changes.</span></span>
7. <span data-ttu-id="5b7f6-360">Abra la **ApiPersonController.cs** de archivos desde el **controladores** carpeta.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-360">Open the **ApiPersonController.cs** file from the **Controllers** folder.</span></span>
8. <span data-ttu-id="5b7f6-361">Escriba una nueva línea entre la *GetPeople* firma del método y la */ / obtener api/ApiPerson* comentar y, a continuación, escriba tres barras diagonales.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-361">Enter a new line between the *GetPeople* method signature and the *// GET api/ApiPerson* comment, and then type three forward slashes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5b7f6-362">Visual Studio inserta automáticamente los elementos XML que definen la documentación del método.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-362">Visual Studio automatically inserts the XML elements which define the method documentation.</span></span>
9. <span data-ttu-id="5b7f6-363">Agregar un texto de resumen y el valor devuelto para la *GetPeople* método.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-363">Add a summary text and the return value for the *GetPeople* method.</span></span> <span data-ttu-id="5b7f6-364">Debe ser similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-364">It should look like the following.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. <span data-ttu-id="5b7f6-365">Presione **F5** para ejecutar la solución.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-365">Press **F5** to run the solution.</span></span>
11. <span data-ttu-id="5b7f6-366">Anexar **/help** a la dirección URL en la barra de direcciones para ir a la página de ayuda.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-366">Append **/help** to the URL in the address bar to browse to the help page.</span></span>

    <span data-ttu-id="5b7f6-367">![Página de Ayuda de ASP.NET Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "página de Ayuda de ASP.NET Web API")</span><span class="sxs-lookup"><span data-stu-id="5b7f6-367">![ASP.NET Web API Help Page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API Help Page")</span></span>

    <span data-ttu-id="5b7f6-368">*Página de Ayuda de ASP.NET Web API*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-368">*ASP.NET Web API Help Page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="5b7f6-369">El contenido de la página principal es una tabla de API, agrupadas por el controlador.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-369">The main content of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="5b7f6-370">Las entradas de tabla se generan de forma dinámica, mediante la **IApiExplorer** interfaz.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-370">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="5b7f6-371">Si agrega o actualiza un controlador de API, la tabla se actualizarán automáticamente la próxima vez que compile la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-371">If you add or update an API controller, the table will be automatically updated the next time you build the application.</span></span>
    > 
    > <span data-ttu-id="5b7f6-372">El **API** columna muestra el método HTTP y el URI relativo.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-372">The **API** column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="5b7f6-373">El **descripción** columna contiene información que se ha extraído de la documentación del método.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-373">The **Description** column contains information that has been extracted from the method's documentation.</span></span>
12. <span data-ttu-id="5b7f6-374">Tenga en cuenta que la descripción que ha agregado la definición de método se muestra en la columna Descripción.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-374">Note that the description you added above the method definition is displayed in the description column.</span></span>

    <span data-ttu-id="5b7f6-375">![Descripción del método de API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "descripción del método de API")</span><span class="sxs-lookup"><span data-stu-id="5b7f6-375">![API method description](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API method description")</span></span>

    <span data-ttu-id="5b7f6-376">*Descripción del método de API*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-376">*API method description*</span></span>
13. <span data-ttu-id="5b7f6-377">Haga clic en uno de los métodos de API para navegar a una página con información más detallada, incluidos los cuerpos de respuesta de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="5b7f6-377">Click one of the API methods to navigate to a page with more detailed information, including sample response bodies.</span></span>

    <span data-ttu-id="5b7f6-378">![Página de información de detalle](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "página de información detallada")</span><span class="sxs-lookup"><span data-stu-id="5b7f6-378">![Detail Information page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detail Information page")</span></span>

    <span data-ttu-id="5b7f6-379">*Página de información detallada*</span><span class="sxs-lookup"><span data-stu-id="5b7f6-379">*Detailed information page*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="5b7f6-380">Resumen</span><span class="sxs-lookup"><span data-stu-id="5b7f6-380">Summary</span></span>

<span data-ttu-id="5b7f6-381">Al completar este laboratorio práctico ha aprendido cómo:</span><span class="sxs-lookup"><span data-stu-id="5b7f6-381">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="5b7f6-382">Cree una nueva aplicación Web con una experiencia de ASP.NET en Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5b7f6-382">Create a new Web application using the One ASP.NET Experience in Visual Studio 2013</span></span>
- <span data-ttu-id="5b7f6-383">Integrar diversas tecnologías ASP.NET en un solo proyecto</span><span class="sxs-lookup"><span data-stu-id="5b7f6-383">Integrate multiple ASP.NET technologies into one single project</span></span>
- <span data-ttu-id="5b7f6-384">Generar controladores MVC y vistas de las clases de modelo mediante Scaffolding de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5b7f6-384">Generate MVC controllers and views from your model classes using ASP.NET Scaffolding</span></span>
- <span data-ttu-id="5b7f6-385">Generar controladores de API Web que usan características como la programación asincrónica y acceso a los datos a través de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="5b7f6-385">Generate Web API controllers, which use features such as Async Programming and data access through Entity Framework</span></span>
- <span data-ttu-id="5b7f6-386">Generar automáticamente las páginas de Ayuda de Web API para los controladores</span><span class="sxs-lookup"><span data-stu-id="5b7f6-386">Automatically generate Web API Help Pages for your controllers</span></span>
