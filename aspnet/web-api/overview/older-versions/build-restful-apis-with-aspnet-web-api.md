---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Compilar API RESTful con ASP.NET Web API | Microsoft Docs
author: rick-anderson
description: En los últimos años, se ha convertido en claro que HTTP no es solo para servir como páginas HTML. También es una plataforma eficaz para la creación de API de Web, con un puñado de o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 88ac5102a1cf14050412abc336e7a8260a9fa80d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363525"
---
<a name="build-restful-apis-with-aspnet-web-api"></a><span data-ttu-id="f5b45-104">Compilar API RESTful con ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="f5b45-104">Build RESTful APIs with ASP.NET Web API</span></span>
====================
<span data-ttu-id="f5b45-105">por [campamentos Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="f5b45-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="f5b45-106">En los últimos años, se ha convertido en claro que HTTP no es solo para servir como páginas HTML.</span><span class="sxs-lookup"><span data-stu-id="f5b45-106">In recent years, it has become clear that HTTP is not just for serving up HTML pages.</span></span> <span data-ttu-id="f5b45-107">También es una plataforma eficaz para crear las API Web, con unos cuantos verbos (GET, POST etc.) además de algunos conceptos simples como *URI* y *encabezados*.</span><span class="sxs-lookup"><span data-stu-id="f5b45-107">It is also a powerful platform for building Web APIs, using a handful of verbs (GET, POST, and so forth) plus a few simple concepts such as *URIs* and *headers*.</span></span> <span data-ttu-id="f5b45-108">ASP.NET Web API es un conjunto de componentes que simplifican la programación de HTTP.</span><span class="sxs-lookup"><span data-stu-id="f5b45-108">ASP.NET Web API is a set of components that simplify HTTP programming.</span></span> <span data-ttu-id="f5b45-109">Dado que se basa en el tiempo de ejecución de ASP.NET MVC, API Web controla automáticamente los detalles de bajo nivel de transporte de HTTP.</span><span class="sxs-lookup"><span data-stu-id="f5b45-109">Because it is built on top of the ASP.NET MVC runtime, Web API automatically handles the low-level transport details of HTTP.</span></span> <span data-ttu-id="f5b45-110">Al mismo tiempo, API Web expone naturalmente el modelo de programación HTTP.</span><span class="sxs-lookup"><span data-stu-id="f5b45-110">At the same time, Web API naturally exposes the HTTP programming model.</span></span> <span data-ttu-id="f5b45-111">De hecho, es uno de los objetivos de la API Web *no* abstraer la realidad de HTTP.</span><span class="sxs-lookup"><span data-stu-id="f5b45-111">In fact, one goal of Web API is to *not* abstract away the reality of HTTP.</span></span> <span data-ttu-id="f5b45-112">Como resultado, la API Web es flexible y fácil de extender.</span><span class="sxs-lookup"><span data-stu-id="f5b45-112">As a result, Web API is both flexible and easy to extend.</span></span> <span data-ttu-id="f5b45-113">En este laboratorio práctico, usará la API Web para crear una sencilla API de REST para una aplicación de administrador de contactos.</span><span class="sxs-lookup"><span data-stu-id="f5b45-113">In this hands-on lab, you will use Web API to build a simple REST API for a contact manager application.</span></span> <span data-ttu-id="f5b45-114">También creará un cliente para usar la API.</span><span class="sxs-lookup"><span data-stu-id="f5b45-114">You will also build a client to consume the API.</span></span> <span data-ttu-id="f5b45-115">El estilo arquitectónico REST ha demostrado para ser una forma eficaz de aprovechar HTTP - aunque ciertamente no es el enfoque sólo es válido para HTTP.</span><span class="sxs-lookup"><span data-stu-id="f5b45-115">The REST architectural style has proven to be an effective way to leverage HTTP - although it is certainly not the only valid approach to HTTP.</span></span> <span data-ttu-id="f5b45-116">El Administrador de contactos expondrá el RESTful para enumerar, agregar y quitar contactos, entre otros.</span><span class="sxs-lookup"><span data-stu-id="f5b45-116">The contact manager will expose the RESTful for listing, adding and removing contacts, among others.</span></span> <span data-ttu-id="f5b45-117">Esta práctica requiere una comprensión básica de HTTP, REST y se supone que tiene conocimientos prácticos básicos de HTML, JavaScript y jQuery.</span><span class="sxs-lookup"><span data-stu-id="f5b45-117">This lab requires a basic understanding of HTTP, REST, and assumes you have a basic working knowledge of HTML, JavaScript, and jQuery.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="f5b45-118">El sitio Web de ASP.NET tiene un área dedicada para el marco de ASP.NET Web API en [ https://asp.net/web-api ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="f5b45-118">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [https://asp.net/web-api](https://asp.net/web-api).</span></span> <span data-ttu-id="f5b45-119">Este sitio seguirá proporcionando información más reciente, ejemplos y noticias relacionadas con la API Web, así que comprobación con frecuencia si gustaría profundizar más en el arte de la creación de API Web personalizadas disponibles para prácticamente cualquier marco de desarrollo o de dispositivo.</span><span class="sxs-lookup"><span data-stu-id="f5b45-119">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>
> > 
> > <span data-ttu-id="f5b45-120">ASP.NET Web API, similar a ASP.NET MVC 4, tiene gran flexibilidad en cuanto a la separación de la capa de servicio desde los controladores que le permite usar algunos de los marcos de trabajo disponibles de inserción de dependencias bastante fácil.</span><span class="sxs-lookup"><span data-stu-id="f5b45-120">ASP.NET Web API, similar to ASP.NET MVC 4, has great flexibility in terms of separating the service layer from the controllers allowing you to use several of the available Dependency Injection frameworks fairly easy.</span></span> <span data-ttu-id="f5b45-121">Hay un buen ejemplo de MSDN que se muestra cómo usar Ninject para inserción de dependencias en un proyecto de ASP.NET Web API que puede descargarse desde [aquí](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span><span class="sxs-lookup"><span data-stu-id="f5b45-121">There is a good sample in MSDN that shows how to use Ninject for dependency injection in an ASP.NET Web API project that you can download it from [here](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span></span>
> 
> 
> <span data-ttu-id="f5b45-122">Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de entrenamiento campamentos de Web, que está disponible en [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="f5b45-122">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="f5b45-123">Objetivos</span><span class="sxs-lookup"><span data-stu-id="f5b45-123">Objectives</span></span>

<span data-ttu-id="f5b45-124">En este laboratorio práctico, aprenderá cómo:</span><span class="sxs-lookup"><span data-stu-id="f5b45-124">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="f5b45-125">Implementar una API Web RESTful</span><span class="sxs-lookup"><span data-stu-id="f5b45-125">Implement a RESTful Web API</span></span>
- <span data-ttu-id="f5b45-126">Llamar a la API desde un cliente HTML</span><span class="sxs-lookup"><span data-stu-id="f5b45-126">Call the API from an HTML client</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="f5b45-127">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="f5b45-127">Prerequisites</span></span>

<span data-ttu-id="f5b45-128">El siguiente es necesario para completar este laboratorio práctico:</span><span class="sxs-lookup"><span data-stu-id="f5b45-128">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="f5b45-129">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (leer [Apéndice B](#AppendixB) para obtener instrucciones sobre cómo instalarlo).</span><span class="sxs-lookup"><span data-stu-id="f5b45-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="f5b45-130">Programa de instalación</span><span class="sxs-lookup"><span data-stu-id="f5b45-130">Setup</span></span>

<span data-ttu-id="f5b45-131">**Instalación de fragmentos de código**</span><span class="sxs-lookup"><span data-stu-id="f5b45-131">**Installing Code Snippets**</span></span>

<span data-ttu-id="f5b45-132">Para mayor comodidad, gran parte del código que se va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f5b45-132">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="f5b45-133">Para instalar los fragmentos de código que se ejecute **.\Source\Setup\CodeSnippets.vsi** archivo.</span><span class="sxs-lookup"><span data-stu-id="f5b45-133">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="f5b45-134">Si no está familiarizado con los fragmentos de código de Visual Studio y desea aprender a usarlas, puede consultar el apéndice de este documento &quot; [Apéndice A: Using Code Snippets](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="f5b45-134">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="f5b45-135">Ejercicios</span><span class="sxs-lookup"><span data-stu-id="f5b45-135">Exercises</span></span>

<span data-ttu-id="f5b45-136">Este laboratorio práctico incluye en el siguiente ejercicio:</span><span class="sxs-lookup"><span data-stu-id="f5b45-136">This hands-on lab includes the following exercise:</span></span>

1. [<span data-ttu-id="f5b45-137">Ejercicio 1: Crear una API de Web de solo lectura</span><span class="sxs-lookup"><span data-stu-id="f5b45-137">Exercise 1: Create a Read-Only Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="f5b45-138">Ejercicio 2: Crear una API Web de lectura/escritura</span><span class="sxs-lookup"><span data-stu-id="f5b45-138">Exercise 2: Create a Read/Write Web API</span></span>](#Exercise2)
3. [<span data-ttu-id="f5b45-139">Ejercicio 3: Usar la API Web desde un cliente HTML</span><span class="sxs-lookup"><span data-stu-id="f5b45-139">Exercise 3: Consume the Web API from an HTML Client</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="f5b45-140">Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debe obtener después de completar los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="f5b45-140">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="f5b45-141">Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="f5b45-141">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="f5b45-142">Tiempo estimado para completar esta práctica: **60 minutos**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-142">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a><span data-ttu-id="f5b45-143">Ejercicio 1: Crear una API de Web de solo lectura</span><span class="sxs-lookup"><span data-stu-id="f5b45-143">Exercise 1: Create a Read-Only Web API</span></span>

<span data-ttu-id="f5b45-144">En este ejercicio, implementará los métodos GET de solo lectura para el Administrador de contactos.</span><span class="sxs-lookup"><span data-stu-id="f5b45-144">In this exercise, you will implement the read-only GET methods for the contact manager.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a><span data-ttu-id="f5b45-145">Tarea 1: crear el proyecto de API</span><span class="sxs-lookup"><span data-stu-id="f5b45-145">Task 1 - Creating the API Project</span></span>

<span data-ttu-id="f5b45-146">En esta tarea, utilizará las nuevas plantillas de proyecto web ASP.NET para crear una aplicación web de API Web.</span><span class="sxs-lookup"><span data-stu-id="f5b45-146">In this task, you will use the new ASP.NET web project templates to create a Web API web application.</span></span>

1. <span data-ttu-id="f5b45-147">Ejecute **Visual Studio 2012 Express para Web**, para ello, vaya a **iniciar** y tipo **VS Express para Web** , a continuación, presione **ENTRAR**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-147">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="f5b45-148">Desde el **archivo** menú, seleccione **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-148">From the **File** menu, select **New Project**.</span></span> <span data-ttu-id="f5b45-149">Seleccione el **Visual C# | Web** tipo desde la vista de árbol del tipo de proyecto del proyecto y, después, seleccione el **aplicación Web de ASP.NET MVC 4** tipo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="f5b45-149">Select the **Visual C# | Web** project type from the project type tree view, then select the **ASP.NET MVC 4 Web Application** project type.</span></span> <span data-ttu-id="f5b45-150">Establezca el proyecto **nombre** a *ContactManager* y el **nombre de la solución** a *comenzar*, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-150">Set the project's **Name** to *ContactManager* and the **Solution name** to *Begin*, then click **OK**.</span></span>

    <span data-ttu-id="f5b45-151">![Crear un nuevo proyecto de aplicación de ASP.NET MVC 4.0 Web](build-restful-apis-with-aspnet-web-api/_static/image1.png "crear un nuevo proyecto de aplicación de ASP.NET MVC 4.0 Web")</span><span class="sxs-lookup"><span data-stu-id="f5b45-151">![Creating a new ASP.NET MVC 4.0 Web Application Project](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creating a new ASP.NET MVC 4.0 Web Application Project")</span></span>

    <span data-ttu-id="f5b45-152">*Crear un nuevo proyecto de aplicación de ASP.NET MVC 4.0 Web*</span><span class="sxs-lookup"><span data-stu-id="f5b45-152">*Creating a new ASP.NET MVC 4.0 Web Application Project*</span></span>
3. <span data-ttu-id="f5b45-153">En el cuadro de diálogo de tipo de proyecto de ASP.NET MVC 4, seleccione el **API Web** tipo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="f5b45-153">In the ASP.NET MVC 4 project type dialog, select the **Web API** project type.</span></span> <span data-ttu-id="f5b45-154">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-154">Click **OK**.</span></span>

    <span data-ttu-id="f5b45-155">![Especifica el tipo de proyecto Web API](build-restful-apis-with-aspnet-web-api/_static/image2.png "especifica el tipo de proyecto Web API")</span><span class="sxs-lookup"><span data-stu-id="f5b45-155">![Specifying the Web API project type](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifying the Web API project type")</span></span>

    <span data-ttu-id="f5b45-156">*Especifica el tipo de proyecto Web API*</span><span class="sxs-lookup"><span data-stu-id="f5b45-156">*Specifying the Web API project type*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a><span data-ttu-id="f5b45-157">Tarea 2: creación de los controladores de API de Contact Manager</span><span class="sxs-lookup"><span data-stu-id="f5b45-157">Task 2 - Creating the Contact Manager API Controllers</span></span>

<span data-ttu-id="f5b45-158">En esta tarea, creará las clases del controlador en el que van a residir los métodos de API.</span><span class="sxs-lookup"><span data-stu-id="f5b45-158">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="f5b45-159">Eliminar el archivo denominado **ValuesController.cs** en **controladores** carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="f5b45-159">Delete the file named **ValuesController.cs** within **Controllers** folder from the project.</span></span>
2. <span data-ttu-id="f5b45-160">Haga clic en el **controladores** carpeta en el proyecto y seleccione **Add | Controlador** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="f5b45-160">Right-click the **Controllers** folder in the project and select **Add | Controller** from the context menu.</span></span>

    <span data-ttu-id="f5b45-161">![Agregar un nuevo controlador para el proyecto](build-restful-apis-with-aspnet-web-api/_static/image3.png "agregando un nuevo controlador al proyecto")</span><span class="sxs-lookup"><span data-stu-id="f5b45-161">![Adding a new controller to the project](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adding a new controller to the project")</span></span>

    <span data-ttu-id="f5b45-162">*Agregar un nuevo controlador al proyecto*</span><span class="sxs-lookup"><span data-stu-id="f5b45-162">*Adding a new controller to the project*</span></span>
3. <span data-ttu-id="f5b45-163">En el **Agregar controlador** cuadro de diálogo que aparece, seleccione **controlador de API en blanco** en el menú de la plantilla.</span><span class="sxs-lookup"><span data-stu-id="f5b45-163">In the **Add Controller** dialog that appears, select **Empty API Controller** from the Template menu.</span></span> <span data-ttu-id="f5b45-164">Nombre de la clase de controlador **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-164">Name the controller class **ContactController**.</span></span> <span data-ttu-id="f5b45-165">A continuación, haga clic en **agregar.**</span><span class="sxs-lookup"><span data-stu-id="f5b45-165">Then, click **Add.**</span></span>

    <span data-ttu-id="f5b45-166">![Mediante el cuadro de diálogo Agregar controlador para crear un nuevo controlador de API Web](build-restful-apis-with-aspnet-web-api/_static/image4.png "mediante el cuadro de diálogo Agregar controlador para crear un nuevo controlador de API Web")</span><span class="sxs-lookup"><span data-stu-id="f5b45-166">![Using the Add Controller dialog to create a new Web API controller](build-restful-apis-with-aspnet-web-api/_static/image4.png "Using the Add Controller dialog to create a new Web API controller")</span></span>

    <span data-ttu-id="f5b45-167">*Usar el cuadro de diálogo Agregar controlador para crear un nuevo controlador de API Web*</span><span class="sxs-lookup"><span data-stu-id="f5b45-167">*Using the Add Controller dialog to create a new Web API controller*</span></span>
4. <span data-ttu-id="f5b45-168">Agregue el código siguiente a la **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-168">Add the following code to the **ContactController**.</span></span>

    <span data-ttu-id="f5b45-169">(Código de fragmento de código - *Web API laboratorio - Ex01 - Get (método) API*)</span><span class="sxs-lookup"><span data-stu-id="f5b45-169">(Code Snippet - *Web API Lab - Ex01 - Get API Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. <span data-ttu-id="f5b45-170">Presione **F5** para depurar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f5b45-170">Press **F5** to debug the application.</span></span> <span data-ttu-id="f5b45-171">Debería aparecer la página principal predeterminada para un proyecto Web API.</span><span class="sxs-lookup"><span data-stu-id="f5b45-171">The default home page for a Web API project should appear.</span></span>

    <span data-ttu-id="f5b45-172">![La página principal predeterminada de una aplicación de ASP.NET Web API](build-restful-apis-with-aspnet-web-api/_static/image5.png "la página principal predeterminada de una aplicación de ASP.NET Web API")</span><span class="sxs-lookup"><span data-stu-id="f5b45-172">![The default home page of an ASP.NET Web API application](build-restful-apis-with-aspnet-web-api/_static/image5.png "The default home page of an ASP.NET Web API application")</span></span>

    <span data-ttu-id="f5b45-173">*La página principal predeterminada de una aplicación de ASP.NET Web API*</span><span class="sxs-lookup"><span data-stu-id="f5b45-173">*The default home page of an ASP.NET Web API application*</span></span>
6. <span data-ttu-id="f5b45-174">En la ventana de Internet Explorer, presione el **F12** tecla para abrir el **Developer Tools** ventana.</span><span class="sxs-lookup"><span data-stu-id="f5b45-174">In the Internet Explorer window, press the **F12** key to open the **Developer Tools** window.</span></span> <span data-ttu-id="f5b45-175">Haga clic en el **red** pestaña y, a continuación, haga clic en el **Iniciar captura** botón para empezar a capturar el tráfico de red en la ventana.</span><span class="sxs-lookup"><span data-stu-id="f5b45-175">Click the **Network** tab, and then click the **Start Capturing** button to begin capturing network traffic into the window.</span></span>

    <span data-ttu-id="f5b45-176">![Abre la pestaña red e iniciando captura de red](build-restful-apis-with-aspnet-web-api/_static/image6.png "abre la pestaña red e iniciando captura de red")</span><span class="sxs-lookup"><span data-stu-id="f5b45-176">![Opening the network tab and initiating network capture](build-restful-apis-with-aspnet-web-api/_static/image6.png "Opening the network tab and initiating network capture")</span></span>

    <span data-ttu-id="f5b45-177">*Abre la pestaña red e iniciar la captura de red*</span><span class="sxs-lookup"><span data-stu-id="f5b45-177">*Opening the network tab and initiating network capture*</span></span>
7. <span data-ttu-id="f5b45-178">Anexe la dirección URL en la barra de direcciones del explorador con **/api/contact** y presione ENTRAR.</span><span class="sxs-lookup"><span data-stu-id="f5b45-178">Append the URL in the browser's address bar with **/api/contact** and press enter.</span></span> <span data-ttu-id="f5b45-179">Los detalles de la transmisión aparecerán en la ventana de captura de red.</span><span class="sxs-lookup"><span data-stu-id="f5b45-179">The transmission details will appear in the network capture window.</span></span> <span data-ttu-id="f5b45-180">Tenga en cuenta que el tipo MIME de la respuesta es **application/json**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-180">Note that the response's MIME type is **application/json**.</span></span> <span data-ttu-id="f5b45-181">Esto demuestra cómo el formato de salida predeterminado es JSON.</span><span class="sxs-lookup"><span data-stu-id="f5b45-181">This demonstrates how the default output format is JSON.</span></span>

    <span data-ttu-id="f5b45-182">![Ver la salida de la solicitud de API Web en la vista de red](build-restful-apis-with-aspnet-web-api/_static/image7.png "viendo la salida de la solicitud de API Web en la vista de red")</span><span class="sxs-lookup"><span data-stu-id="f5b45-182">![Viewing the output of the Web API request in the Network view](build-restful-apis-with-aspnet-web-api/_static/image7.png "Viewing the output of the Web API request in the Network view")</span></span>

    <span data-ttu-id="f5b45-183">*Ver la salida de la solicitud de API Web en la vista de red*</span><span class="sxs-lookup"><span data-stu-id="f5b45-183">*Viewing the output of the Web API request in the Network view*</span></span>

    > [!NOTE]
    > <span data-ttu-id="f5b45-184">Configuración predeterminada de Internet Explorer 10 en este momento será preguntar si el usuario desea guardar o abrir la secuencia resultante de la llamada de API Web.</span><span class="sxs-lookup"><span data-stu-id="f5b45-184">Internet Explorer 10's default behavior at this point will be to ask if the user would like to save or open the stream resulting from the Web API call.</span></span> <span data-ttu-id="f5b45-185">El resultado será un archivo de texto que contiene el resultado JSON de la llamada a la dirección URL de Web API.</span><span class="sxs-lookup"><span data-stu-id="f5b45-185">The output will be a text file containing the JSON result of the Web API URL call.</span></span> <span data-ttu-id="f5b45-186">No cancele el cuadro de diálogo para poder ver el contenido de la respuesta a través de la ventana de herramientas de desarrolladores.</span><span class="sxs-lookup"><span data-stu-id="f5b45-186">Do not cancel the dialog in order to be able to watch the response's content through Developers Tool window.</span></span>
8. <span data-ttu-id="f5b45-187">Haga clic en el **vaya a la vista detallada** botón para ver más detalles sobre la respuesta de la llamada API.</span><span class="sxs-lookup"><span data-stu-id="f5b45-187">Click the **Go to detailed view** button to see more details about the response of this API call.</span></span>

    <span data-ttu-id="f5b45-188">![Cambie a la vista detallada](build-restful-apis-with-aspnet-web-api/_static/image8.png "cambiar a vista de detalles")</span><span class="sxs-lookup"><span data-stu-id="f5b45-188">![Switch to Detailed View](build-restful-apis-with-aspnet-web-api/_static/image8.png "Switch to Details View")</span></span>

    <span data-ttu-id="f5b45-189">*Cambie a la vista detallada*</span><span class="sxs-lookup"><span data-stu-id="f5b45-189">*Switch to Detailed View*</span></span>
9. <span data-ttu-id="f5b45-190">Haga clic en el **cuerpo de respuesta** pestaña para ver el texto de respuesta JSON real.</span><span class="sxs-lookup"><span data-stu-id="f5b45-190">Click the **Response body** tab to view the actual JSON response text.</span></span>

    <span data-ttu-id="f5b45-191">![Ver el JSON de salida de texto en el monitor de red](build-restful-apis-with-aspnet-web-api/_static/image9.png "ver el JSON de salida de texto en el monitor de red")</span><span class="sxs-lookup"><span data-stu-id="f5b45-191">![Viewing the JSON output text in the network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Viewing the JSON output text in the network monitor")</span></span>

    <span data-ttu-id="f5b45-192">*Ver el texto de salida JSON en el monitor de red*</span><span class="sxs-lookup"><span data-stu-id="f5b45-192">*Viewing the JSON output text in the network monitor*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a><span data-ttu-id="f5b45-193">Tarea 3: creación de los modelos de contacto y aumentar el controlador de contacto</span><span class="sxs-lookup"><span data-stu-id="f5b45-193">Task 3 - Creating the Contact Models and Augment the Contact Controller</span></span>

<span data-ttu-id="f5b45-194">En esta tarea, creará las clases del controlador en el que van a residir los métodos de API.</span><span class="sxs-lookup"><span data-stu-id="f5b45-194">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="f5b45-195">Haga clic en el **modelos** carpeta y seleccione **Add | Clase...**  en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="f5b45-195">Right-click the **Models** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="f5b45-196">![Agregar un modelo nuevo a la aplicación web](build-restful-apis-with-aspnet-web-api/_static/image10.png "adición de un modelo nuevo a la aplicación web")</span><span class="sxs-lookup"><span data-stu-id="f5b45-196">![Adding a new model to the web application](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adding a new model to the web application")</span></span>

    <span data-ttu-id="f5b45-197">*Agregar un modelo nuevo a la aplicación web*</span><span class="sxs-lookup"><span data-stu-id="f5b45-197">*Adding a new model to the web application*</span></span>
2. <span data-ttu-id="f5b45-198">En el **Agregar nuevo elemento** cuadro de diálogo, el nombre del nuevo archivo **Contact.cs** y haga clic en **agregar.**</span><span class="sxs-lookup"><span data-stu-id="f5b45-198">In the **Add New Item** dialog, name the new file **Contact.cs** and click **Add.**</span></span>

    <span data-ttu-id="f5b45-199">![Crear el nuevo archivo de clase de contacto](build-restful-apis-with-aspnet-web-api/_static/image11.png "crear el nuevo archivo de clase de contacto")</span><span class="sxs-lookup"><span data-stu-id="f5b45-199">![Creating the new Contact class file](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creating the new Contact class file")</span></span>

    <span data-ttu-id="f5b45-200">*Crear el nuevo archivo de clase de contacto*</span><span class="sxs-lookup"><span data-stu-id="f5b45-200">*Creating the new Contact class file*</span></span>
3. <span data-ttu-id="f5b45-201">Agregue el código resaltado siguiente a la **póngase en contacto con** clase.</span><span class="sxs-lookup"><span data-stu-id="f5b45-201">Add the following highlighted code to the **Contact** class.</span></span>

    <span data-ttu-id="f5b45-202">(Código de fragmento de código - *clase póngase en contacto con el laboratorio de API - Ex01 - Web*)</span><span class="sxs-lookup"><span data-stu-id="f5b45-202">(Code Snippet - *Web API Lab - Ex01 - Contact Class*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. <span data-ttu-id="f5b45-203">En el **ContactController** de clases, seleccione la palabra **cadena** en la definición de método de la **obtener** método y escriba la palabra *póngase en contacto con*.</span><span class="sxs-lookup"><span data-stu-id="f5b45-203">In the **ContactController** class, select the word **string** in method definition of the **Get** method, and type the word *Contact*.</span></span> <span data-ttu-id="f5b45-204">Una vez se ha escrito la palabra, aparecerá un indicador al principio de la palabra **póngase en contacto con**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-204">Once the word is typed in, an indicator will appear at the beginning of the word **Contact**.</span></span> <span data-ttu-id="f5b45-205">Ya sea mantenga presionada la tecla el **Ctrl** clave y presione la tecla de punto (.) o haga clic en el icono con el mouse para abrir el cuadro de diálogo de asistencia en el editor de código para rellenar automáticamente el **mediante** la directiva para los modelos espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="f5b45-205">Either hold down the **Ctrl** key and press the period (.) key or click the icon using your mouse to open up the assistance dialog in the code editor, to automatically fill in the **using** directive for the Models namespace.</span></span>

    ![Mediante la asistencia de Intellisense para las declaraciones de espacio de nombres](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    <span data-ttu-id="f5b45-207">*Mediante la asistencia de Intellisense para las declaraciones de espacio de nombres*</span><span class="sxs-lookup"><span data-stu-id="f5b45-207">*Using Intellisense assistance for namespace declarations*</span></span>
5. <span data-ttu-id="f5b45-208">Modifique el código para el **obtener** método para que devuelva una matriz de instancias de modelo de contacto.</span><span class="sxs-lookup"><span data-stu-id="f5b45-208">Modify the code for the **Get** method so that it returns an array of Contact model instances.</span></span>

    <span data-ttu-id="f5b45-209">(Código de fragmento de código - *devolver una lista de contactos de laboratorio de API Web - Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="f5b45-209">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. <span data-ttu-id="f5b45-210">Presione **F5** para depurar la aplicación web en el explorador.</span><span class="sxs-lookup"><span data-stu-id="f5b45-210">Press **F5** to debug the web application in the browser.</span></span> <span data-ttu-id="f5b45-211">Para ver los cambios realizados en la salida de respuesta de la API, realice los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="f5b45-211">To view the changes made to the response output of the API, perform the following steps.</span></span>

   1. <span data-ttu-id="f5b45-212">Una vez que el explorador se abre, presione **F12** si las herramientas de desarrollo no están abiertas todavía.</span><span class="sxs-lookup"><span data-stu-id="f5b45-212">Once the browser opens, press **F12** if the developer tools are not open yet.</span></span>
   2. <span data-ttu-id="f5b45-213">Haga clic en el **red** ficha.</span><span class="sxs-lookup"><span data-stu-id="f5b45-213">Click the **Network** tab.</span></span>
   3. <span data-ttu-id="f5b45-214">Presione el **Iniciar captura** botón.</span><span class="sxs-lookup"><span data-stu-id="f5b45-214">Press the **Start Capturing** button.</span></span>
   4. <span data-ttu-id="f5b45-215">Agregue el sufijo de dirección URL **/api/contact** a la dirección URL en la barra de direcciones y presione la **ENTRAR** clave.</span><span class="sxs-lookup"><span data-stu-id="f5b45-215">Add the URL suffix **/api/contact** to the URL in the address bar and press the **Enter** key.</span></span>
   5. <span data-ttu-id="f5b45-216">Presione el **vaya a la vista detallada** botón.</span><span class="sxs-lookup"><span data-stu-id="f5b45-216">Press the **Go to detailed view** button.</span></span>
   6. <span data-ttu-id="f5b45-217">Seleccione el **cuerpo de respuesta** ficha. Debería ver una cadena JSON que representa el formulario serializado de una matriz de instancias de contacto.</span><span class="sxs-lookup"><span data-stu-id="f5b45-217">Select the **Response body** tab. You should see a JSON string representing the serialized form of an array of Contact instances.</span></span>

      <span data-ttu-id="f5b45-218">![Salida de una llamada al método de Web API compleja serializados de JSON](build-restful-apis-with-aspnet-web-api/_static/image13.png "salida de una llamada al método de Web API compleja serializados de JSON")</span><span class="sxs-lookup"><span data-stu-id="f5b45-218">![JSON serialized output of a complex Web API method call](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialized output of a complex Web API method call")</span></span>

      <span data-ttu-id="f5b45-219">*Salida de una llamada al método de Web API compleja serializados de JSON*</span><span class="sxs-lookup"><span data-stu-id="f5b45-219">*JSON serialized output of a complex Web API method call*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a><span data-ttu-id="f5b45-220">Tarea 4: funcionalidad de extracción en una capa de servicio</span><span class="sxs-lookup"><span data-stu-id="f5b45-220">Task 4 - Extracting Functionality into a Service Layer</span></span>

<span data-ttu-id="f5b45-221">Esta tarea mostrará cómo extraer la funcionalidad en una capa de servicio para que sea fácil para los desarrolladores separar su funcionalidad de servicio de la capa de controlador, lo que permite la reutilización de los servicios que realmente realizan el trabajo.</span><span class="sxs-lookup"><span data-stu-id="f5b45-221">This task will demonstrate how to extract functionality into a Service layer to make it easy for developers to separate their service functionality from the controller layer, thereby allowing reusability of the services that actually do the work.</span></span>

1. <span data-ttu-id="f5b45-222">Cree una nueva carpeta en la raíz de la solución y asígnele el nombre **servicios**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-222">Create a new folder in the solution root and name it **Services**.</span></span> <span data-ttu-id="f5b45-223">Para ello, haga clic en **ContactManager** proyecto, seleccione **agregar** | **nueva carpeta**, asígnele el nombre *servicios*.</span><span class="sxs-lookup"><span data-stu-id="f5b45-223">To do this, right-click **ContactManager** project, select **Add** | **New Folder**, name it *Services*.</span></span>

    <span data-ttu-id="f5b45-224">![Creando carpeta de servicios](build-restful-apis-with-aspnet-web-api/_static/image14.png "carpeta crear servicios")</span><span class="sxs-lookup"><span data-stu-id="f5b45-224">![Creating Services folder](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creating Services folder")</span></span>

    <span data-ttu-id="f5b45-225">*Creando la carpeta de servicios*</span><span class="sxs-lookup"><span data-stu-id="f5b45-225">*Creating Services folder*</span></span>
2. <span data-ttu-id="f5b45-226">Haga clic en el **servicios** carpeta y seleccione **Add | Clase...**  en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="f5b45-226">Right-click the **Services** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="f5b45-227">![Agregar una nueva clase a la carpeta servicios](build-restful-apis-with-aspnet-web-api/_static/image15.png "agregando una nueva clase a la carpeta de servicios")</span><span class="sxs-lookup"><span data-stu-id="f5b45-227">![Adding a new class to the Services folder](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adding a new class to the Services folder")</span></span>

    <span data-ttu-id="f5b45-228">*Agregar una nueva clase a la carpeta de servicios*</span><span class="sxs-lookup"><span data-stu-id="f5b45-228">*Adding a new class to the Services folder*</span></span>
3. <span data-ttu-id="f5b45-229">Cuando el **Agregar nuevo elemento** aparece el cuadro de diálogo, el nombre de la nueva clase **ContactRepository** y haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-229">When the **Add New Item** dialog appears, name the new class **ContactRepository** and click **Add**.</span></span>

    <span data-ttu-id="f5b45-230">![Creación de un archivo de clase para que contenga el código de la capa de repositorio póngase en contacto con servicio](build-restful-apis-with-aspnet-web-api/_static/image16.png "creación de un archivo de clase para que contenga el código de la capa de servicio del repositorio de contacto")</span><span class="sxs-lookup"><span data-stu-id="f5b45-230">![Creating a class file to contain the code for the Contact Repository service layer](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creating a class file to contain the code for the Contact Repository service layer")</span></span>

    <span data-ttu-id="f5b45-231">*Creación de un archivo de clase para que contenga el código de la capa de servicio del repositorio de contacto*</span><span class="sxs-lookup"><span data-stu-id="f5b45-231">*Creating a class file to contain the code for the Contact Repository service layer*</span></span>
4. <span data-ttu-id="f5b45-232">Agregue una mediante la directiva a la **ContactRepository.cs** archivo para incluir el espacio de nombres de los modelos.</span><span class="sxs-lookup"><span data-stu-id="f5b45-232">Add a using directive to the **ContactRepository.cs** file to include the models namespace.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. <span data-ttu-id="f5b45-233">Agregue el código resaltado siguiente a la **ContactRepository.cs** archivo para implementar el método GetAllContacts.</span><span class="sxs-lookup"><span data-stu-id="f5b45-233">Add the following highlighted code to the **ContactRepository.cs** file to implement GetAllContacts method.</span></span>

    <span data-ttu-id="f5b45-234">(Código de fragmento de código - *Web de repositorio de contactos de laboratorio de API - Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="f5b45-234">(Code Snippet - *Web API Lab - Ex01 - Contact Repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. <span data-ttu-id="f5b45-235">Abra el **ContactController.cs** archivo si aún no está abierto.</span><span class="sxs-lookup"><span data-stu-id="f5b45-235">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="f5b45-236">Agregue lo siguiente mediante la instrucción a la sección de declaración de espacio de nombres del archivo.</span><span class="sxs-lookup"><span data-stu-id="f5b45-236">Add the following using statement to the namespace declaration section of the file.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. <span data-ttu-id="f5b45-237">Agregue el código resaltado siguiente a la **ContactController.cs** clase para agregar un campo privado para representar la instancia del repositorio, por lo que usar el resto de la clase pueden hacer los miembros de la implementación del servicio.</span><span class="sxs-lookup"><span data-stu-id="f5b45-237">Add the following highlighted code to the **ContactController.cs** class to add a private field to represent the instance of the repository, so that the rest of the class members can make use of the service implementation.</span></span>

    <span data-ttu-id="f5b45-238">(Código de fragmento de código - *API laboratorio - Ex01 - contacto controlador Web*)</span><span class="sxs-lookup"><span data-stu-id="f5b45-238">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. <span data-ttu-id="f5b45-239">Cambiar el **obtener** método por lo que ofrece utilizar el servicio del repositorio de contactos.</span><span class="sxs-lookup"><span data-stu-id="f5b45-239">Change the **Get** method so that it makes use of the contact repository service.</span></span>

    <span data-ttu-id="f5b45-240">(Código de fragmento de código - *devolver una lista de contactos a través del repositorio de laboratorio de API Web - Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="f5b45-240">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts via the repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. <span data-ttu-id="f5b45-241">Coloque un punto de interrupción en el **ContactController**del **obtener** definición de método.</span><span class="sxs-lookup"><span data-stu-id="f5b45-241">Put a breakpoint on the **ContactController**'s **Get** method definition.</span></span>

   <span data-ttu-id="f5b45-242">![Agregar puntos de interrupción en el controlador de contacto](build-restful-apis-with-aspnet-web-api/_static/image17.png "agregar puntos de interrupción en el controlador de contacto")</span><span class="sxs-lookup"><span data-stu-id="f5b45-242">![Adding breakpoints to the contact controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adding breakpoints to the contact controller")</span></span>

   <span data-ttu-id="f5b45-243">*Agregar puntos de interrupción en el controlador de contacto*</span><span class="sxs-lookup"><span data-stu-id="f5b45-243">*Adding breakpoints to the contact controller*</span></span>
11. <span data-ttu-id="f5b45-244">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f5b45-244">Press **F5** to run the application.</span></span>
12. <span data-ttu-id="f5b45-245">Cuando el explorador se abre, presione **F12** para abrir las herramientas de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="f5b45-245">When the browser opens, press **F12** to open the developer tools.</span></span>
13. <span data-ttu-id="f5b45-246">Haga clic en el **red** ficha.</span><span class="sxs-lookup"><span data-stu-id="f5b45-246">Click the **Network** tab.</span></span>
14. <span data-ttu-id="f5b45-247">Haga clic en el **Iniciar captura** botón.</span><span class="sxs-lookup"><span data-stu-id="f5b45-247">Click the **Start Capturing** button.</span></span>
15. <span data-ttu-id="f5b45-248">Anexe la dirección URL en la barra de direcciones con el sufijo **/api/contact** y presione **ENTRAR** para cargar el controlador de API.</span><span class="sxs-lookup"><span data-stu-id="f5b45-248">Append the URL in the address bar with the suffix **/api/contact** and press **Enter** to load the API controller.</span></span>
16. <span data-ttu-id="f5b45-249">Visual Studio 2012 debe interrumpir una vez **obtener** método comienza la ejecución.</span><span class="sxs-lookup"><span data-stu-id="f5b45-249">Visual Studio 2012 should break once **Get** method begins execution.</span></span>

   <span data-ttu-id="f5b45-250">![Importantes dentro del método Get](build-restful-apis-with-aspnet-web-api/_static/image18.png "importantes dentro del método Get")</span><span class="sxs-lookup"><span data-stu-id="f5b45-250">![Breaking within the Get method](build-restful-apis-with-aspnet-web-api/_static/image18.png "Breaking within the Get method")</span></span>

   <span data-ttu-id="f5b45-251">*Última hora dentro del método Get*</span><span class="sxs-lookup"><span data-stu-id="f5b45-251">*Breaking within the Get method*</span></span>
17. <span data-ttu-id="f5b45-252">Presione **F5** para continuar.</span><span class="sxs-lookup"><span data-stu-id="f5b45-252">Press **F5** to continue.</span></span>
18. <span data-ttu-id="f5b45-253">Vuelva a Internet Explorer si aún no está en el foco.</span><span class="sxs-lookup"><span data-stu-id="f5b45-253">Go back to Internet Explorer if it is not already in focus.</span></span> <span data-ttu-id="f5b45-254">Tenga en cuenta la ventana de captura de red.</span><span class="sxs-lookup"><span data-stu-id="f5b45-254">Note the network capture window.</span></span>

    <span data-ttu-id="f5b45-255">![Vista en Internet Explorer que muestra los resultados de la llamada de API Web de red](build-restful-apis-with-aspnet-web-api/_static/image19.png "vista en Internet Explorer que muestra los resultados de la llamada de API Web de red")</span><span class="sxs-lookup"><span data-stu-id="f5b45-255">![Network view in Internet Explorer showing results of the Web API call](build-restful-apis-with-aspnet-web-api/_static/image19.png "Network view in Internet Explorer showing results of the Web API call")</span></span>

    <span data-ttu-id="f5b45-256">*Vista de red en Internet Explorer que muestra los resultados de la llamada de API Web*</span><span class="sxs-lookup"><span data-stu-id="f5b45-256">*Network view in Internet Explorer showing results of the Web API call*</span></span>
19. <span data-ttu-id="f5b45-257">Haga clic en el **vaya a la vista detallada** botón.</span><span class="sxs-lookup"><span data-stu-id="f5b45-257">Click the **Go to detailed view** button.</span></span>
20. <span data-ttu-id="f5b45-258">Haga clic en el **cuerpo de respuesta** ficha. Tenga en cuenta la salida JSON de la llamada de API y cómo representa los dos contactos recuperados por la capa de servicio.</span><span class="sxs-lookup"><span data-stu-id="f5b45-258">Click the **Response body** tab. Note the JSON output of the API call, and how it represents the two contacts retrieved by the service layer.</span></span>

    <span data-ttu-id="f5b45-259">![Ver la salida JSON desde la API Web en la ventana de herramientas de desarrollador](build-restful-apis-with-aspnet-web-api/_static/image20.png "ver la salida JSON desde la API Web en la ventana de herramientas para desarrolladores")</span><span class="sxs-lookup"><span data-stu-id="f5b45-259">![Viewing the JSON output from the Web API in the developer tools window](build-restful-apis-with-aspnet-web-api/_static/image20.png "Viewing the JSON output from the Web API in the developer tools window")</span></span>

    <span data-ttu-id="f5b45-260">*Ver la salida JSON desde la API Web en la ventana de herramientas para desarrolladores*</span><span class="sxs-lookup"><span data-stu-id="f5b45-260">*Viewing the JSON output from the Web API in the developer tools window*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a><span data-ttu-id="f5b45-261">Ejercicio 2: Crear una API Web de lectura/escritura</span><span class="sxs-lookup"><span data-stu-id="f5b45-261">Exercise 2: Create a Read/Write Web API</span></span>

<span data-ttu-id="f5b45-262">En este ejercicio, implementará POST y PUT métodos para el Administrador de contactos habilitarlo con características de edición de datos.</span><span class="sxs-lookup"><span data-stu-id="f5b45-262">In this exercise, you will implement POST and PUT methods for the contact manager to enable it with data-editing features.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a><span data-ttu-id="f5b45-263">Tarea 1: abrir el proyecto de API Web</span><span class="sxs-lookup"><span data-stu-id="f5b45-263">Task 1 - Opening the Web API Project</span></span>

<span data-ttu-id="f5b45-264">En esta tarea, preparará mejorar el proyecto Web API que creó en el ejercicio 1 para que pueda aceptar la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="f5b45-264">In this task, you will prepare to enhance the Web API project created in Exercise 1 so that it can accept user input.</span></span>

1. <span data-ttu-id="f5b45-265">Ejecute **Visual Studio 2012 Express para Web**, para ello, vaya a **iniciar** y tipo **VS Express para Web** , a continuación, presione **ENTRAR**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-265">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="f5b45-266">Abra el **comenzar** solución ubicado en **origen/Ex02-ReadWriteWebAPI/inicio/** carpeta.</span><span class="sxs-lookup"><span data-stu-id="f5b45-266">Open the **Begin** solution located at **Source/Ex02-ReadWriteWebAPI/Begin/** folder.</span></span> <span data-ttu-id="f5b45-267">En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="f5b45-267">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="f5b45-268">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="f5b45-268">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="f5b45-269">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-269">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="f5b45-270">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="f5b45-270">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="f5b45-271">Por último, compile la solución haciendo **compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-271">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="f5b45-272">Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="f5b45-272">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="f5b45-273">Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="f5b45-273">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="f5b45-274">Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="f5b45-274">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="f5b45-275">Abra el **Services/ContactRepository.cs** archivo.</span><span class="sxs-lookup"><span data-stu-id="f5b45-275">Open the **Services/ContactRepository.cs** file.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a><span data-ttu-id="f5b45-276">Tarea 2: agregar características de persistencia de datos para la implementación de repositorio de contactos</span><span class="sxs-lookup"><span data-stu-id="f5b45-276">Task 2 - Adding Data-Persistence Features to the Contact Repository Implementation</span></span>

<span data-ttu-id="f5b45-277">En esta tarea, se aumentan la clase ContactRepository del proyecto de API Web creado en el ejercicio 1 para que pueda conservar y aceptar entrada del usuario y nuevas instancias de contacto.</span><span class="sxs-lookup"><span data-stu-id="f5b45-277">In this task, you will augment the ContactRepository class of the Web API project created in Exercise 1 so that it can persist and accept user input and new Contact instances.</span></span>

1. <span data-ttu-id="f5b45-278">Agregue la siguiente constante a la **ContactRepository** clase para representar el nombre de la memoria caché elemento clave nombre del servidor web más adelante en este ejercicio.</span><span class="sxs-lookup"><span data-stu-id="f5b45-278">Add the following constant to the **ContactRepository** class to represent the name of the web server cache item key name later in this exercise.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. <span data-ttu-id="f5b45-279">Agregue un constructor a la **ContactRepository** que contiene el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="f5b45-279">Add a constructor to the **ContactRepository** containing the following code.</span></span>

    <span data-ttu-id="f5b45-280">(Código de fragmento de código - *Web Constructor del repositorio de contactos de laboratorio API - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="f5b45-280">(Code Snippet - *Web API Lab - Ex02 - Contact Repository Constructor*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. <span data-ttu-id="f5b45-281">Modifique el código para el **GetAllContacts** método tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="f5b45-281">Modify the code for the **GetAllContacts** method as demonstrated below.</span></span>

    <span data-ttu-id="f5b45-282">(Código de fragmento de código - *Web API laboratorio - Ex02 - obtener todos los contactos*)</span><span class="sxs-lookup"><span data-stu-id="f5b45-282">(Code Snippet - *Web API Lab - Ex02 - Get All Contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="f5b45-283">En este ejemplo es para fines de demostración y usa memoria caché del servidor web como un medio de almacenamiento, para que los valores se estén disponibles para varios clientes simultáneamente, en lugar de usar un mecanismo de almacenamiento de sesión o una duración de almacenamiento de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="f5b45-283">This example is for demonstration purposes and will use the web server's cache as a storage medium, so that the values will be available to multiple clients simultaneously, rather than use a Session storage mechanism or a Request storage lifetime.</span></span> <span data-ttu-id="f5b45-284">Se podría usar Entity Framework, almacenamiento de XML o cualquier otra variedad en lugar de la caché del servidor web.</span><span class="sxs-lookup"><span data-stu-id="f5b45-284">One could use Entity Framework, XML storage, or any other variety in place of the web server cache.</span></span>
4. <span data-ttu-id="f5b45-285">Implementar un nuevo método denominado **SaveContact** a la **ContactRepository** clase para realizar el trabajo del proceso de guardar un contacto.</span><span class="sxs-lookup"><span data-stu-id="f5b45-285">Implement a new method named **SaveContact** to the **ContactRepository** class to do the work of saving a contact.</span></span> <span data-ttu-id="f5b45-286">El **SaveContact** método debe tomar un único **póngase en contacto con** valor de parámetro y devuelva un valor booleano que indica éxito o error.</span><span class="sxs-lookup"><span data-stu-id="f5b45-286">The **SaveContact** method should take a single **Contact** parameter and return a Boolean value indicating success or failure.</span></span>

    <span data-ttu-id="f5b45-287">(Código de fragmento de código - *Web API laboratorio - Ex02 - implementación del método SaveContact*)</span><span class="sxs-lookup"><span data-stu-id="f5b45-287">(Code Snippet - *Web API Lab - Ex02 - Implementing the SaveContact Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a><span data-ttu-id="f5b45-288">Ejercicio 3: Usar la API Web desde un cliente HTML</span><span class="sxs-lookup"><span data-stu-id="f5b45-288">Exercise 3: Consume the Web API from an HTML Client</span></span>

<span data-ttu-id="f5b45-289">En este ejercicio, creará un cliente HTML para llamar a la API Web.</span><span class="sxs-lookup"><span data-stu-id="f5b45-289">In this exercise, you will create an HTML client to call the Web API.</span></span> <span data-ttu-id="f5b45-290">Este cliente facilitará el intercambio de datos con la API Web con JavaScript y mostrará los resultados en un explorador web mediante el formato HTML.</span><span class="sxs-lookup"><span data-stu-id="f5b45-290">This client will facilitate data exchange with the Web API using JavaScript and will display the results in a web browser using HTML markup.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a><span data-ttu-id="f5b45-291">Tarea 1: modificación de la vista de índice para proporcionar una interfaz gráfica de usuario para mostrar contactos</span><span class="sxs-lookup"><span data-stu-id="f5b45-291">Task 1 - Modifying the Index View to Provide a GUI for Displaying Contacts</span></span>

<span data-ttu-id="f5b45-292">En esta tarea, modificará la vista de índice predeterminada de la aplicación web para admitir el requisito de mostrar la lista de contactos existentes en un explorador HTML.</span><span class="sxs-lookup"><span data-stu-id="f5b45-292">In this task, you will modify the default Index view of the web application to support the requirement of displaying the list of existing contacts in an HTML browser.</span></span>

1. <span data-ttu-id="f5b45-293">Abra **Visual Studio 2012 Express para Web** si aún no está abierto.</span><span class="sxs-lookup"><span data-stu-id="f5b45-293">Open **Visual Studio 2012 Express for Web** if it is not already open.</span></span>
2. <span data-ttu-id="f5b45-294">Abra el **comenzar** solución ubicado en **origen/Ex03-ConsumingWebAPI/inicio/** carpeta.</span><span class="sxs-lookup"><span data-stu-id="f5b45-294">Open the **Begin** solution located at **Source/Ex03-ConsumingWebAPI/Begin/** folder.</span></span> <span data-ttu-id="f5b45-295">En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="f5b45-295">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="f5b45-296">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="f5b45-296">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="f5b45-297">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-297">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="f5b45-298">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="f5b45-298">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="f5b45-299">Por último, compile la solución haciendo **compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-299">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="f5b45-300">Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="f5b45-300">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="f5b45-301">Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="f5b45-301">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="f5b45-302">Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="f5b45-302">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="f5b45-303">Abra el **Index.cshtml** ubicado en el archivo **vistas/inicio** carpeta.</span><span class="sxs-lookup"><span data-stu-id="f5b45-303">Open the **Index.cshtml** file located at **Views/Home** folder.</span></span>
4. <span data-ttu-id="f5b45-304">Reemplace el código HTML dentro del elemento div con el identificador **cuerpo** para que quede como el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="f5b45-304">Replace the HTML code within the div element with id **body** so that it looks like the following code.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. <span data-ttu-id="f5b45-305">Agregue el siguiente código Javascript en la parte inferior del archivo para realizar la solicitud HTTP a la API Web.</span><span class="sxs-lookup"><span data-stu-id="f5b45-305">Add the following Javascript code at the bottom of the file to perform the HTTP request to the Web API.</span></span>

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. <span data-ttu-id="f5b45-306">Abra el **ContactController.cs** archivo si aún no está abierto.</span><span class="sxs-lookup"><span data-stu-id="f5b45-306">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="f5b45-307">Coloque un punto de interrupción en el **obtener** método de la **ContactController** clase.</span><span class="sxs-lookup"><span data-stu-id="f5b45-307">Place a breakpoint on the **Get** method of the **ContactController** class.</span></span>

    <span data-ttu-id="f5b45-308">![Al colocar un punto de interrupción en el método Get del controlador API](build-restful-apis-with-aspnet-web-api/_static/image21.png "colocando un punto de interrupción en el método Get del controlador de API")</span><span class="sxs-lookup"><span data-stu-id="f5b45-308">![Placing a breakpoint on the Get method of the API controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placing a breakpoint on the Get method of the API controller")</span></span>

    <span data-ttu-id="f5b45-309">*Al colocar un punto de interrupción en el método Get del controlador de API*</span><span class="sxs-lookup"><span data-stu-id="f5b45-309">*Placing a breakpoint on the Get method of the API controller*</span></span>
8. <span data-ttu-id="f5b45-310">Presione **F5** para ejecutar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="f5b45-310">Press **F5** to run the project.</span></span> <span data-ttu-id="f5b45-311">El explorador cargará el documento HTML.</span><span class="sxs-lookup"><span data-stu-id="f5b45-311">The browser will load the HTML document.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f5b45-312">Asegúrese de que va a examinar la dirección URL raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f5b45-312">Ensure that you are browsing to the root URL of your application.</span></span>
9. <span data-ttu-id="f5b45-313">Una vez que la página se carga y ejecuta el código JavaScript, se alcanzará el punto de interrupción y se pausará la ejecución del código en el controlador.</span><span class="sxs-lookup"><span data-stu-id="f5b45-313">Once the page loads and the JavaScript executes, the breakpoint will be hit and the code execution will pause in the controller.</span></span>

    <span data-ttu-id="f5b45-314">![Depuración en las llamadas de API Web con VS Express para Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "depuración en las llamadas de API Web con VS Express para Web")</span><span class="sxs-lookup"><span data-stu-id="f5b45-314">![Debugging into the Web API calls using VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugging into the Web API calls using VS Express for Web")</span></span>

    <span data-ttu-id="f5b45-315">*Depuración en la llamada de API Web mediante Visual Studio 2012 Express para Web*</span><span class="sxs-lookup"><span data-stu-id="f5b45-315">*Debugging into the Web API call using Visual Studio 2012 Express for Web*</span></span>
10. <span data-ttu-id="f5b45-316">Quitar el punto de interrupción y presione **F5** o la barra de herramientas depuración **continuar** botón para continuar cargando la vista en el explorador.</span><span class="sxs-lookup"><span data-stu-id="f5b45-316">Remove the breakpoint and press **F5** or the debugging toolbar's **Continue** button to continue loading the view in the browser.</span></span> <span data-ttu-id="f5b45-317">Una vez que se complete la llamada de API Web debería ver los contactos devueltos por la API Web llame a mostrados como elementos de lista en el explorador.</span><span class="sxs-lookup"><span data-stu-id="f5b45-317">Once the Web API call completes you should see the contacts returned from the Web API call displayed as list items in the browser.</span></span>

    <span data-ttu-id="f5b45-318">![Resultados de la llamada de API que se muestra en el explorador como elementos de lista](build-restful-apis-with-aspnet-web-api/_static/image23.png "los resultados de la llamada de API que se muestra en el explorador como elementos de lista")</span><span class="sxs-lookup"><span data-stu-id="f5b45-318">![Results of the API call displayed in the browser as list items](build-restful-apis-with-aspnet-web-api/_static/image23.png "Results of the API call displayed in the browser as list items")</span></span>

    <span data-ttu-id="f5b45-319">*Resultados de la llamada de API que se muestra en el explorador como elementos de lista*</span><span class="sxs-lookup"><span data-stu-id="f5b45-319">*Results of the API call displayed in the browser as list items*</span></span>
11. <span data-ttu-id="f5b45-320">Detenga la depuración.</span><span class="sxs-lookup"><span data-stu-id="f5b45-320">Stop debugging.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a><span data-ttu-id="f5b45-321">Tarea 2: modificar la vista de índice para proporcionar una interfaz gráfica de usuario para la creación de contactos</span><span class="sxs-lookup"><span data-stu-id="f5b45-321">Task 2 - Modifying the Index View to Provide a GUI for Creating Contacts</span></span>

<span data-ttu-id="f5b45-322">En esta tarea, continuará modificar la vista de índice de la aplicación MVC.</span><span class="sxs-lookup"><span data-stu-id="f5b45-322">In this task, you will continue to modify the Index view of the MVC application.</span></span> <span data-ttu-id="f5b45-323">Se agregará un formulario a la página HTML que capturará la entrada del usuario y enviarlo a la API Web para crear un nuevo contacto, y se creará un nuevo método de controlador Web API para recopilar la fecha de la interfaz gráfica de usuario.</span><span class="sxs-lookup"><span data-stu-id="f5b45-323">A form will be added to the HTML page that will capture user input and send it to the Web API to create a new Contact, and a new Web API controller method will be created to collect date from the GUI.</span></span>

1. <span data-ttu-id="f5b45-324">Abra el **ContactController.cs** archivo.</span><span class="sxs-lookup"><span data-stu-id="f5b45-324">Open the **ContactController.cs** file.</span></span>
2. <span data-ttu-id="f5b45-325">Agregue un nuevo método a la clase de controlador denominada **Post** tal como se muestra en el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="f5b45-325">Add a new method to the controller class named **Post** as shown in the following code.</span></span>

    <span data-ttu-id="f5b45-326">(Código de fragmento de código - *Web método Post de laboratorio de API - Ex03 -*)</span><span class="sxs-lookup"><span data-stu-id="f5b45-326">(Code Snippet - *Web API Lab - Ex03 - Post Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. <span data-ttu-id="f5b45-327">Abra el **Index.cshtml** archivo en Visual Studio si aún no está abierto.</span><span class="sxs-lookup"><span data-stu-id="f5b45-327">Open the **Index.cshtml** file in Visual Studio if it is not already open.</span></span>
4. <span data-ttu-id="f5b45-328">Agregue el código HTML siguiente al archivo justo después de la lista sin ordenar que agregó en la tarea anterior.</span><span class="sxs-lookup"><span data-stu-id="f5b45-328">Add the HTML code below to the file just after the unordered list you added in the previous task.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. <span data-ttu-id="f5b45-329">Dentro del elemento de secuencia de comandos en la parte inferior del documento, agregue el siguiente código resaltado para controlar los eventos de clic de botón, que enviará los datos a la API Web mediante una llamada HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="f5b45-329">Within the script element at the bottom of the document, add the following highlighted code to handle button-click events, which will post the data to the Web API using an HTTP POST call.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. <span data-ttu-id="f5b45-330">En **ContactController.cs**, coloque un punto de interrupción en el **Post** método.</span><span class="sxs-lookup"><span data-stu-id="f5b45-330">In **ContactController.cs**, place a breakpoint on the **Post** method.</span></span>
7. <span data-ttu-id="f5b45-331">Presione **F5** para ejecutar la aplicación en el explorador.</span><span class="sxs-lookup"><span data-stu-id="f5b45-331">Press **F5** to run the application in the browser.</span></span>
8. <span data-ttu-id="f5b45-332">Una vez que se carga la página en el explorador, escriba un nuevo nombre de contacto y el identificador y haga clic en el **guardar** botón.</span><span class="sxs-lookup"><span data-stu-id="f5b45-332">Once the page is loaded in the browser, type in a new contact name and Id and click the **Save** button.</span></span>

    <span data-ttu-id="f5b45-333">![Carga el documento de cliente HTML en el explorador](build-restful-apis-with-aspnet-web-api/_static/image24.png "cargará el documento de cliente HTML en el explorador")</span><span class="sxs-lookup"><span data-stu-id="f5b45-333">![The client HTML document loaded in the browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "The client HTML document loaded in the browser")</span></span>

    <span data-ttu-id="f5b45-334">*Carga el documento HTML de cliente en el explorador*</span><span class="sxs-lookup"><span data-stu-id="f5b45-334">*The client HTML document loaded in the browser*</span></span>
9. <span data-ttu-id="f5b45-335">Cuando la ventana del depurador se interrumpe la **Post** método, echar un vistazo a las propiedades de la **póngase en contacto con** parámetro.</span><span class="sxs-lookup"><span data-stu-id="f5b45-335">When the debugger window breaks in the **Post** method, take a look at the properties of the **contact** parameter.</span></span> <span data-ttu-id="f5b45-336">Los valores deben coincidir con los datos especificados en el formulario.</span><span class="sxs-lookup"><span data-stu-id="f5b45-336">The values should match the data you entered in the form.</span></span>

    <span data-ttu-id="f5b45-337">![El objeto de contacto que se envían a la API Web desde el cliente](build-restful-apis-with-aspnet-web-api/_static/image25.png "objeto póngase en contacto con el que se envían a la API Web desde el cliente")</span><span class="sxs-lookup"><span data-stu-id="f5b45-337">![The Contact object being sent to the Web API from the client](build-restful-apis-with-aspnet-web-api/_static/image25.png "The Contact object being sent to the Web API from the client")</span></span>

    <span data-ttu-id="f5b45-338">*El objeto de contacto que se envían a la API Web desde el cliente*</span><span class="sxs-lookup"><span data-stu-id="f5b45-338">*The Contact object being sent to the Web API from the client*</span></span>
10. <span data-ttu-id="f5b45-339">Paso a través del método en el depurador hasta el **respuesta** variable se ha creado.</span><span class="sxs-lookup"><span data-stu-id="f5b45-339">Step through the method in the debugger until the **response** variable has been created.</span></span> <span data-ttu-id="f5b45-340">Tras la inspección en el **variables locales** ventana en el depurador, verá que se han establecido todas las propiedades.</span><span class="sxs-lookup"><span data-stu-id="f5b45-340">Upon inspection in the **Locals** window in the debugger, you'll see that all the properties have been set.</span></span>

   <span data-ttu-id="f5b45-341">![La respuesta después de la creación en el depurador](build-restful-apis-with-aspnet-web-api/_static/image26.png "la respuesta después de la creación en el depurador")</span><span class="sxs-lookup"><span data-stu-id="f5b45-341">![The response following creation in the debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "The response following creation in the debugger")</span></span>

   <span data-ttu-id="f5b45-342">*La respuesta después de la creación en el depurador*</span><span class="sxs-lookup"><span data-stu-id="f5b45-342">*The response following creation in the debugger*</span></span>
11. <span data-ttu-id="f5b45-343">Si presiona **F5** o haga clic en **continuar** en el depurador se completará la solicitud.</span><span class="sxs-lookup"><span data-stu-id="f5b45-343">If you press **F5** or click **Continue** in the debugger the request will complete.</span></span> <span data-ttu-id="f5b45-344">Cuando cambie al explorador, el nuevo contacto se agregó a la lista de contactos almacenados por la **ContactRepository** implementación.</span><span class="sxs-lookup"><span data-stu-id="f5b45-344">Once you switch back to the browser, the new contact has been added to the list of contacts stored by the **ContactRepository** implementation.</span></span>

   <span data-ttu-id="f5b45-345">![El explorador refleja la creación correcta de la nueva instancia de contacto](build-restful-apis-with-aspnet-web-api/_static/image27.png "el explorador refleja la creación correcta de la nueva instancia de contacto")</span><span class="sxs-lookup"><span data-stu-id="f5b45-345">![The browser reflects successful creation of the new contact instance](build-restful-apis-with-aspnet-web-api/_static/image27.png "The browser reflects successful creation of the new contact instance")</span></span>

   <span data-ttu-id="f5b45-346">*El explorador refleja la creación correcta de la nueva instancia de contacto*</span><span class="sxs-lookup"><span data-stu-id="f5b45-346">*The browser reflects successful creation of the new contact instance*</span></span>

> [!NOTE]
> <span data-ttu-id="f5b45-347">Además, puede implementar esta aplicación a Azure siguiente [Apéndice C: publicar una aplicación ASP.NET MVC 4 mediante Web Deploy](#AppendixC).</span><span class="sxs-lookup"><span data-stu-id="f5b45-347">Additionally, you can deploy this application to Azure following [Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixC).</span></span>


* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="f5b45-348">Resumen</span><span class="sxs-lookup"><span data-stu-id="f5b45-348">Summary</span></span>

<span data-ttu-id="f5b45-349">Este laboratorio presenta el nuevo marco de ASP.NET Web API y la implementación de API Web RESTful mediante el marco de trabajo.</span><span class="sxs-lookup"><span data-stu-id="f5b45-349">This lab has introduced you to the new ASP.NET Web API framework and to the implementation of RESTful Web APIs using the framework.</span></span> <span data-ttu-id="f5b45-350">Desde aquí, puede crear un nuevo repositorio que facilita la persistencia de datos mediante cualquier número de mecanismos y conectar ese servicio en lugar del simple proporcionada como ejemplo en este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="f5b45-350">From here, you could create a new repository that facilitates data persistence using any number of mechanisms and wire that service up rather than the simple one provided as an example in this lab.</span></span> <span data-ttu-id="f5b45-351">Web API es compatible con un número de características adicionales, como la habilitación de la comunicación de clientes que no sean HTML escritos en cualquier lenguaje que admita HTTP y JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="f5b45-351">Web API supports a number of additional features, such as enabling communication from non-HTML clients written in any language that supports HTTP and JSON or XML.</span></span> <span data-ttu-id="f5b45-352">La capacidad para hospedar una API Web fuera de una aplicación web típica también es posible, así como es la capacidad para crear sus propios formatos de serialización.</span><span class="sxs-lookup"><span data-stu-id="f5b45-352">The ability to host a Web API outside of a typical web application is also possible, as well as is the ability to create your own serialization formats.</span></span>

<span data-ttu-id="f5b45-353">El sitio Web de ASP.NET tiene un área dedicada para el marco de ASP.NET Web API en [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="f5b45-353">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="f5b45-354">Este sitio seguirá proporcionando información más reciente, ejemplos y noticias relacionadas con la API Web, así que comprobación con frecuencia si gustaría profundizar más en el arte de la creación de API Web personalizadas disponibles para prácticamente cualquier marco de desarrollo o de dispositivo.</span><span class="sxs-lookup"><span data-stu-id="f5b45-354">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="f5b45-355">Apéndice A: usar fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="f5b45-355">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="f5b45-356">Con fragmentos de código, tiene todo el código que necesita a su alcance.</span><span class="sxs-lookup"><span data-stu-id="f5b45-356">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="f5b45-357">El documento de laboratorio le dirá exactamente cuándo se pueden utilizar, como se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="f5b45-357">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="f5b45-358">![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](build-restful-apis-with-aspnet-web-api/_static/image28.png "fragmentos de código en Visual Studio para insertar código en el proyecto")</span><span class="sxs-lookup"><span data-stu-id="f5b45-358">![Using Visual Studio code snippets to insert code into your project](build-restful-apis-with-aspnet-web-api/_static/image28.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="f5b45-359">*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*</span><span class="sxs-lookup"><span data-stu-id="f5b45-359">*Using Visual Studio code snippets to insert code into your project*</span></span>

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a><span data-ttu-id="f5b45-360">Para agregar un fragmento de código mediante el teclado (solo C#)</span><span class="sxs-lookup"><span data-stu-id="f5b45-360">To add a code snippet using the keyboard (C# only)</span></span>

1. <span data-ttu-id="f5b45-361">Coloque el cursor donde desea insertar el código.</span><span class="sxs-lookup"><span data-stu-id="f5b45-361">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="f5b45-362">Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).</span><span class="sxs-lookup"><span data-stu-id="f5b45-362">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="f5b45-363">Observe cómo IntelliSense muestra los nombres de fragmentos de código coincidentes.</span><span class="sxs-lookup"><span data-stu-id="f5b45-363">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="f5b45-364">Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).</span><span class="sxs-lookup"><span data-stu-id="f5b45-364">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="f5b45-365">Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.</span><span class="sxs-lookup"><span data-stu-id="f5b45-365">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

    <span data-ttu-id="f5b45-366">![Comience a escribir el nombre del fragmento](build-restful-apis-with-aspnet-web-api/_static/image29.png "comience a escribir el nombre del fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="f5b45-366">![Start typing the snippet name](build-restful-apis-with-aspnet-web-api/_static/image29.png "Start typing the snippet name")</span></span>

    <span data-ttu-id="f5b45-367">*Comience a escribir el nombre del fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="f5b45-367">*Start typing the snippet name*</span></span>

    <span data-ttu-id="f5b45-368">![Presione la tecla Tab para seleccionar el fragmento de código resaltada](build-restful-apis-with-aspnet-web-api/_static/image30.png "presione Tab para seleccionar el fragmento de código resaltada")</span><span class="sxs-lookup"><span data-stu-id="f5b45-368">![Press Tab to select the highlighted snippet](build-restful-apis-with-aspnet-web-api/_static/image30.png "Press Tab to select the highlighted snippet")</span></span>

    <span data-ttu-id="f5b45-369">*Presione la tecla Tab para seleccionar el fragmento de código resaltada*</span><span class="sxs-lookup"><span data-stu-id="f5b45-369">*Press Tab to select the highlighted snippet*</span></span>

    <span data-ttu-id="f5b45-370">![Vuelva a presionar Tab y el fragmento de código se expandirán](build-restful-apis-with-aspnet-web-api/_static/image31.png "vuelva a presionar Tab y el fragmento de código se expandirán")</span><span class="sxs-lookup"><span data-stu-id="f5b45-370">![Press Tab again and the snippet will expand](build-restful-apis-with-aspnet-web-api/_static/image31.png "Press Tab again and the snippet will expand")</span></span>

    <span data-ttu-id="f5b45-371">*Vuelva a presionar Tab y el fragmento de código se expandirán*</span><span class="sxs-lookup"><span data-stu-id="f5b45-371">*Press Tab again and the snippet will expand*</span></span>

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a><span data-ttu-id="f5b45-372">Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)</span><span class="sxs-lookup"><span data-stu-id="f5b45-372">To add a code snippet using the mouse (C#, Visual Basic and XML)</span></span>

1. <span data-ttu-id="f5b45-373">Haga doble clic en el desea insertar el fragmento de código.</span><span class="sxs-lookup"><span data-stu-id="f5b45-373">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="f5b45-374">Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-374">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="f5b45-375">Seleccione el fragmento de código relevante en la lista, haciendo clic en él.</span><span class="sxs-lookup"><span data-stu-id="f5b45-375">Pick the relevant snippet from the list, by clicking on it.</span></span>

    <span data-ttu-id="f5b45-376">![Con el botón secundario donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](build-restful-apis-with-aspnet-web-api/_static/image32.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="f5b45-376">![Right-click where you want to insert the code snippet and select Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

    <span data-ttu-id="f5b45-377">*Haga doble clic donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="f5b45-377">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

    <span data-ttu-id="f5b45-378">![Elegir el fragmento de código relevante en la lista, hacer clic en ella](build-restful-apis-with-aspnet-web-api/_static/image33.png "elegir el fragmento de código relevante en la lista, hacer clic en ella")</span><span class="sxs-lookup"><span data-stu-id="f5b45-378">![Pick the relevant snippet from the list, by clicking on it](build-restful-apis-with-aspnet-web-api/_static/image33.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

    <span data-ttu-id="f5b45-379">*Elegir el fragmento de código relevante en la lista, hacer clic en ella*</span><span class="sxs-lookup"><span data-stu-id="f5b45-379">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="f5b45-380">Apéndice B: instalar Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="f5b45-380">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="f5b45-381">Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión utilizando el **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-381">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="f5b45-382">Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* mediante *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="f5b45-382">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="f5b45-383">Vaya a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="f5b45-383">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="f5b45-384">Como alternativa, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="f5b45-384">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="f5b45-385">Haga clic en **instalar ahora**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-385">Click on **Install Now**.</span></span> <span data-ttu-id="f5b45-386">Si no tienes **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="f5b45-386">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="f5b45-387">Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.</span><span class="sxs-lookup"><span data-stu-id="f5b45-387">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="f5b45-388">![Instale Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "instale Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="f5b45-388">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="f5b45-389">*Instale Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="f5b45-389">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="f5b45-390">Leer todos los productos las licencias y los términos y haga clic en **acepto** para continuar.</span><span class="sxs-lookup"><span data-stu-id="f5b45-390">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Acepte los términos de licencia](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    <span data-ttu-id="f5b45-392">*Acepte los términos de licencia*</span><span class="sxs-lookup"><span data-stu-id="f5b45-392">*Accepting the license terms*</span></span>
5. <span data-ttu-id="f5b45-393">Espere hasta que finalice el proceso de descarga e instalación.</span><span class="sxs-lookup"><span data-stu-id="f5b45-393">Wait until the downloading and installation process completes.</span></span>

    ![Progreso de la instalación](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    <span data-ttu-id="f5b45-395">*Progreso de la instalación*</span><span class="sxs-lookup"><span data-stu-id="f5b45-395">*Installation progress*</span></span>
6. <span data-ttu-id="f5b45-396">Cuando se complete la instalación, haga clic en **finalizar**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-396">When the installation completes, click **Finish**.</span></span>

    ![Instalación completada](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    <span data-ttu-id="f5b45-398">*Instalación completada*</span><span class="sxs-lookup"><span data-stu-id="f5b45-398">*Installation completed*</span></span>
7. <span data-ttu-id="f5b45-399">Haga clic en **Exit** para cerrar el instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="f5b45-399">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="f5b45-400">Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **VS Express**&quot;, a continuación, haga clic en el **VS Express para Web** icono.</span><span class="sxs-lookup"><span data-stu-id="f5b45-400">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para el icono de Web](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    <span data-ttu-id="f5b45-402">*VS Express para el icono de Web*</span><span class="sxs-lookup"><span data-stu-id="f5b45-402">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="f5b45-403">Apéndice C: publicar una aplicación ASP.NET MVC 4 mediante Web Deploy</span><span class="sxs-lookup"><span data-stu-id="f5b45-403">Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="f5b45-404">En este apéndice se mostrará cómo crear un nuevo sitio web desde el Portal de Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, que aprovecha la característica de publicación Web Deploy proporcionada por Azure.</span><span class="sxs-lookup"><span data-stu-id="f5b45-404">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="f5b45-405">Tarea 1: crear un nuevo sitio Web desde el Portal de Azure</span><span class="sxs-lookup"><span data-stu-id="f5b45-405">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="f5b45-406">Vaya a la [Portal de administración de Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas con su suscripción.</span><span class="sxs-lookup"><span data-stu-id="f5b45-406">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f5b45-407">Con Azure puede hospedar 10 sitios Web ASP.NET de forma gratuita y, a continuación, escalar a medida que crece el tráfico.</span><span class="sxs-lookup"><span data-stu-id="f5b45-407">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="f5b45-408">Puede registrarse [aquí](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="f5b45-408">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="f5b45-409">![Inicie sesión en el portal de Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "inicie sesión en el portal de Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="f5b45-409">![Log on to Windows Azure portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="f5b45-410">*Inicie sesión en el Portal*</span><span class="sxs-lookup"><span data-stu-id="f5b45-410">*Log on to Portal*</span></span>
2. <span data-ttu-id="f5b45-411">Haga clic en **New** en la barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="f5b45-411">Click **New** on the command bar.</span></span>

    <span data-ttu-id="f5b45-412">![Crear un nuevo sitio Web](build-restful-apis-with-aspnet-web-api/_static/image40.png "crear un nuevo sitio Web")</span><span class="sxs-lookup"><span data-stu-id="f5b45-412">![Creating a new Web Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="f5b45-413">*Crear un nuevo sitio Web*</span><span class="sxs-lookup"><span data-stu-id="f5b45-413">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="f5b45-414">Haga clic en **proceso** | **sitio Web**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-414">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="f5b45-415">A continuación, seleccione **creación rápida** opción.</span><span class="sxs-lookup"><span data-stu-id="f5b45-415">Then select **Quick Create** option.</span></span> <span data-ttu-id="f5b45-416">Proporcione una dirección URL disponible para el nuevo sitio web y haga clic en **crear sitio Web**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-416">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f5b45-417">Azure es el host para una aplicación web que se ejecutan en la nube que puede controlar y administrar.</span><span class="sxs-lookup"><span data-stu-id="f5b45-417">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="f5b45-418">La opción Creación rápida permite implementar una aplicación web completada en Azure desde fuera del portal.</span><span class="sxs-lookup"><span data-stu-id="f5b45-418">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="f5b45-419">No incluye los pasos para configurar una base de datos.</span><span class="sxs-lookup"><span data-stu-id="f5b45-419">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="f5b45-420">![Crear un nuevo sitio Web mediante Creación rápida](build-restful-apis-with-aspnet-web-api/_static/image41.png "crear un nuevo sitio Web mediante Creación rápida")</span><span class="sxs-lookup"><span data-stu-id="f5b45-420">![Creating a new Web Site using Quick Create](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="f5b45-421">*Crear un nuevo sitio Web mediante Creación rápida*</span><span class="sxs-lookup"><span data-stu-id="f5b45-421">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="f5b45-422">Espere hasta que el nuevo **sitio Web** se crea.</span><span class="sxs-lookup"><span data-stu-id="f5b45-422">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="f5b45-423">Una vez creado el sitio Web, haga clic en el vínculo situado bajo el **URL** columna.</span><span class="sxs-lookup"><span data-stu-id="f5b45-423">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="f5b45-424">Compruebe que funciona el nuevo sitio Web.</span><span class="sxs-lookup"><span data-stu-id="f5b45-424">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="f5b45-425">![Exploración al sitio web nuevo](build-restful-apis-with-aspnet-web-api/_static/image42.png "exploración al sitio web nuevo")</span><span class="sxs-lookup"><span data-stu-id="f5b45-425">![Browsing to the new web site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="f5b45-426">*Exploración al sitio web nuevo*</span><span class="sxs-lookup"><span data-stu-id="f5b45-426">*Browsing to the new web site*</span></span>

    <span data-ttu-id="f5b45-427">![Sitio Web que se ejecuta](build-restful-apis-with-aspnet-web-api/_static/image43.png "sitio Web que se ejecuta")</span><span class="sxs-lookup"><span data-stu-id="f5b45-427">![Web site running](build-restful-apis-with-aspnet-web-api/_static/image43.png "Web site running")</span></span>

    <span data-ttu-id="f5b45-428">*Sitio Web que se ejecuta*</span><span class="sxs-lookup"><span data-stu-id="f5b45-428">*Web site running*</span></span>
6. <span data-ttu-id="f5b45-429">Vuelva al portal y haga clic en el nombre del sitio web en el **nombre** columna para mostrar las páginas de administración.</span><span class="sxs-lookup"><span data-stu-id="f5b45-429">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="f5b45-430">![Abrir las páginas de administración del sitio web](build-restful-apis-with-aspnet-web-api/_static/image44.png "abrir las páginas de administración del sitio web")</span><span class="sxs-lookup"><span data-stu-id="f5b45-430">![Opening the web site management pages](build-restful-apis-with-aspnet-web-api/_static/image44.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="f5b45-431">*Abrir las páginas de administración del sitio Web*</span><span class="sxs-lookup"><span data-stu-id="f5b45-431">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="f5b45-432">En el **panel** página, en el **primea** sección, haga clic en el **descargar perfil de publicación** vínculo.</span><span class="sxs-lookup"><span data-stu-id="f5b45-432">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f5b45-433">El *perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un Azure para cada método de publicación habilitado.</span><span class="sxs-lookup"><span data-stu-id="f5b45-433">The *publish profile* contains all of the information required to publish a web application to a Azure for each enabled publication method.</span></span> <span data-ttu-id="f5b45-434">El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse y autenticarse en cada uno de los puntos de conexión para el que está habilitado un método de publicación.</span><span class="sxs-lookup"><span data-stu-id="f5b45-434">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="f5b45-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para publicación de aplicaciones web en Azure.</span><span class="sxs-lookup"><span data-stu-id="f5b45-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="f5b45-436">![Descargando el sitio web de perfil de publicación](build-restful-apis-with-aspnet-web-api/_static/image45.png "descargando el sitio web de perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="f5b45-436">![Downloading the web site publish profile](build-restful-apis-with-aspnet-web-api/_static/image45.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="f5b45-437">*Descargando el sitio Web de perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="f5b45-437">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="f5b45-438">Descargue el archivo de perfil de publicación en una ubicación conocida.</span><span class="sxs-lookup"><span data-stu-id="f5b45-438">Download the publish profile file to a known location.</span></span> <span data-ttu-id="f5b45-439">Aún más en este ejercicio verá cómo usar este archivo para publicar una aplicación web en Azure desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f5b45-439">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="f5b45-440">![Guardar el archivo de perfil de publicación](build-restful-apis-with-aspnet-web-api/_static/image46.png "guardar el perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="f5b45-440">![Saving the publish profile file](build-restful-apis-with-aspnet-web-api/_static/image46.png "Saving the publish profile")</span></span>

    <span data-ttu-id="f5b45-441">*Guardar el archivo de perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="f5b45-441">*Saving the publish profile file*</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="f5b45-442">Tarea 2: configurar el servidor de base de datos</span><span class="sxs-lookup"><span data-stu-id="f5b45-442">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="f5b45-443">Si la aplicación hace uso de SQL Server deberá crear un servidor de base de datos SQL de bases de datos.</span><span class="sxs-lookup"><span data-stu-id="f5b45-443">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="f5b45-444">Si desea implementar una aplicación simple que no utiliza SQL Server, podría omitir esta tarea.</span><span class="sxs-lookup"><span data-stu-id="f5b45-444">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="f5b45-445">Necesitará un servidor de base de datos SQL para almacenar la base de datos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="f5b45-445">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="f5b45-446">Puede ver los servidores de base de datos SQL de la suscripción en el portal de administración de Azure en **bases de datos Sql** | **servidores** | **panel del servidor**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-446">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="f5b45-447">Si no tiene un servidor creado, puede crear una mediante el **agregar** botón en la barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="f5b45-447">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="f5b45-448">Tome nota de la **nombre del servidor y dirección URL, nombre de inicio de sesión de administrador y contraseña**, ya que lo usará en las siguientes tareas.</span><span class="sxs-lookup"><span data-stu-id="f5b45-448">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="f5b45-449">No cree la base de datos, como se crearán en una etapa posterior.</span><span class="sxs-lookup"><span data-stu-id="f5b45-449">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="f5b45-450">![Panel de base de datos SQL Server](build-restful-apis-with-aspnet-web-api/_static/image47.png "panel de base de datos SQL Server")</span><span class="sxs-lookup"><span data-stu-id="f5b45-450">![SQL Database Server Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="f5b45-451">*Panel de base de datos SQL Server*</span><span class="sxs-lookup"><span data-stu-id="f5b45-451">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="f5b45-452">En la siguiente tarea probará la conexión de base de datos desde Visual Studio, por ese motivo debe incluir la dirección IP local en la lista del servidor de **direcciones IP permitidas**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-452">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="f5b45-453">Para ello, haga clic en **configurar**, seleccione la dirección IP de **dirección IP del cliente actual** y péguelo en el **dirección IP inicial** y **ladirecciónIPfinal** cuadros de texto y haga clic en el ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) botón.</span><span class="sxs-lookup"><span data-stu-id="f5b45-453">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) button.</span></span>

    ![Agregar dirección IP del cliente](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    <span data-ttu-id="f5b45-455">*Agregar dirección IP del cliente*</span><span class="sxs-lookup"><span data-stu-id="f5b45-455">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="f5b45-456">Una vez el **dirección IP del cliente** se agrega a las direcciones IP permitidas de lista, haga clic en **guardar** para confirmar los cambios.</span><span class="sxs-lookup"><span data-stu-id="f5b45-456">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmar cambios](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    <span data-ttu-id="f5b45-458">*Confirmar cambios*</span><span class="sxs-lookup"><span data-stu-id="f5b45-458">*Confirm Changes*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="f5b45-459">Tarea 3: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy</span><span class="sxs-lookup"><span data-stu-id="f5b45-459">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="f5b45-460">Vuelva a la solución de ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="f5b45-460">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="f5b45-461">En el **el Explorador de soluciones**, haga clic en el proyecto de sitio web y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-461">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="f5b45-462">![Publicar la aplicación](build-restful-apis-with-aspnet-web-api/_static/image51.png "publicar la aplicación")</span><span class="sxs-lookup"><span data-stu-id="f5b45-462">![Publishing the Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publishing the Application")</span></span>

    <span data-ttu-id="f5b45-463">*Publicar el sitio web*</span><span class="sxs-lookup"><span data-stu-id="f5b45-463">*Publishing the web site*</span></span>
2. <span data-ttu-id="f5b45-464">Importar el perfil de publicación que guardó en la primera tarea.</span><span class="sxs-lookup"><span data-stu-id="f5b45-464">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="f5b45-465">![Importar el perfil de publicación](build-restful-apis-with-aspnet-web-api/_static/image52.png "importar el perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="f5b45-465">![Importing the publish profile](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importing the publish profile")</span></span>

    <span data-ttu-id="f5b45-466">*Importar perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="f5b45-466">*Importing publish profile*</span></span>
3. <span data-ttu-id="f5b45-467">Haga clic en **validar conexión**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-467">Click **Validate Connection**.</span></span> <span data-ttu-id="f5b45-468">Una vez completada la validación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-468">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f5b45-469">Validación está completa cuando aparece una marca de verificación verde junto al botón Validar conexión.</span><span class="sxs-lookup"><span data-stu-id="f5b45-469">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="f5b45-470">![Validación de la conexión](build-restful-apis-with-aspnet-web-api/_static/image53.png "validación de la conexión")</span><span class="sxs-lookup"><span data-stu-id="f5b45-470">![Validating connection](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validating connection")</span></span>

    <span data-ttu-id="f5b45-471">*Validación de la conexión*</span><span class="sxs-lookup"><span data-stu-id="f5b45-471">*Validating connection*</span></span>
4. <span data-ttu-id="f5b45-472">En el **configuración** página, en el **bases de datos** sección, haga clic en el botón situado junto al cuadro de texto de la conexión de base de datos (es decir, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="f5b45-472">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="f5b45-473">![Configuración de implementación Web](build-restful-apis-with-aspnet-web-api/_static/image54.png "configuración de implementación Web")</span><span class="sxs-lookup"><span data-stu-id="f5b45-473">![Web deploy configuration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy configuration")</span></span>

    <span data-ttu-id="f5b45-474">*Configuración de implementación Web*</span><span class="sxs-lookup"><span data-stu-id="f5b45-474">*Web deploy configuration*</span></span>
5. <span data-ttu-id="f5b45-475">Configure la conexión de base de datos como sigue:</span><span class="sxs-lookup"><span data-stu-id="f5b45-475">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="f5b45-476">En el **nombre del servidor** escriba la dirección URL de base de datos de SQL server mediante el *tcp:* prefijo.</span><span class="sxs-lookup"><span data-stu-id="f5b45-476">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="f5b45-477">En **nombre de usuario** escriba el nombre de inicio de sesión del Administrador de servidor.</span><span class="sxs-lookup"><span data-stu-id="f5b45-477">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="f5b45-478">En **contraseña** escriba la contraseña de inicio de sesión del Administrador de servidor.</span><span class="sxs-lookup"><span data-stu-id="f5b45-478">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="f5b45-479">Escriba un nuevo nombre de base de datos, por ejemplo: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="f5b45-479">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="f5b45-480">![Configuración de la cadena de conexión de destino](build-restful-apis-with-aspnet-web-api/_static/image55.png "configuración de la cadena de conexión de destino")</span><span class="sxs-lookup"><span data-stu-id="f5b45-480">![Configuring destination connection string](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="f5b45-481">*Configuración de la cadena de conexión de destino*</span><span class="sxs-lookup"><span data-stu-id="f5b45-481">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="f5b45-482">A continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-482">Then click **OK**.</span></span> <span data-ttu-id="f5b45-483">Cuando se le pedirá que cree la base de datos, haga clic en **Sí**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-483">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="f5b45-484">![Creación de la base de datos](build-restful-apis-with-aspnet-web-api/_static/image56.png "creación de la cadena de la base de datos")</span><span class="sxs-lookup"><span data-stu-id="f5b45-484">![Creating the database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creating the database string")</span></span>

    <span data-ttu-id="f5b45-485">*Creación de la base de datos*</span><span class="sxs-lookup"><span data-stu-id="f5b45-485">*Creating the database*</span></span>
7. <span data-ttu-id="f5b45-486">La cadena de conexión que se usará para conectarse a la base de datos de SQL en Windows Azure se muestra en el cuadro de texto de conexión predeterminado.</span><span class="sxs-lookup"><span data-stu-id="f5b45-486">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="f5b45-487">Después, haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-487">Then click **Next**.</span></span>

    <span data-ttu-id="f5b45-488">![Cadena de conexión que apunte a la base de datos SQL](build-restful-apis-with-aspnet-web-api/_static/image57.png "cadena de conexión que apunte a la base de datos SQL")</span><span class="sxs-lookup"><span data-stu-id="f5b45-488">![Connection string pointing to SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="f5b45-489">*Cadena de conexión que apunte a la base de datos SQL*</span><span class="sxs-lookup"><span data-stu-id="f5b45-489">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="f5b45-490">En el **Preview** página, haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="f5b45-490">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="f5b45-491">![Publicar la aplicación web](build-restful-apis-with-aspnet-web-api/_static/image58.png "publicar la aplicación web")</span><span class="sxs-lookup"><span data-stu-id="f5b45-491">![Publishing the web application](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publishing the web application")</span></span>

    <span data-ttu-id="f5b45-492">*Publicar la aplicación web*</span><span class="sxs-lookup"><span data-stu-id="f5b45-492">*Publishing the web application*</span></span>
9. <span data-ttu-id="f5b45-493">Una vez que finalice el proceso de publicación, el explorador predeterminado abrirá el sitio web publicado.</span><span class="sxs-lookup"><span data-stu-id="f5b45-493">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="f5b45-494">![Publica la aplicación en Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "publica la aplicación en Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="f5b45-494">![Application published to Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="f5b45-495">*Aplicación publicada en Azure*</span><span class="sxs-lookup"><span data-stu-id="f5b45-495">*Application published to Azure*</span></span>
