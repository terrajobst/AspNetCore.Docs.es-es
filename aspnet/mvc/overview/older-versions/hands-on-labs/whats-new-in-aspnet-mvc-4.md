---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: "' S New en ASP.NET MVC 4 | Documentos de Microsoft"
author: rick-anderson
description: "ASP.NET MVC 4 es un marco para crear aplicaciones web escalable, basada en estándares con hacia patrones de diseño y la eficacia de ASP.NET y..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 3de952224e23eed29f90ed0e8c662e4ee3f531ce
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-mvc-4"></a><span data-ttu-id="15966-103">' S New en ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="15966-103">What's New in ASP.NET MVC 4</span></span>
====================
<span data-ttu-id="15966-104">por [Web colonias equipo](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="15966-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="15966-105">Descargar el Kit de aprendizaje de colonias de Web</span><span class="sxs-lookup"><span data-stu-id="15966-105">Download Web Camps Training Kit</span></span>](http://www.microsoft.com/en-us/download/29843)

> <span data-ttu-id="15966-106">ASP.NET MVC 4 es un marco para crear aplicaciones web escalable, basada en estándares con hacia patrones de diseño y la eficacia de ASP.NET y .NET framework.</span><span class="sxs-lookup"><span data-stu-id="15966-106">ASP.NET MVC 4 is a framework for building scalable, standards-based web applications using well-established design patterns and the power of the ASP.NET and the .NET framework.</span></span> <span data-ttu-id="15966-107">Esta nueva, cuarta versión de framework se centra en facilitar el desarrollo de aplicaciones web móvil.</span><span class="sxs-lookup"><span data-stu-id="15966-107">This new, fourth version of the framework focuses on making mobile web application development easier.</span></span>
> 
> <span data-ttu-id="15966-108">Para comenzar con, cuando se crea un nuevo proyecto de ASP.NET MVC 4 ahora hay una plantilla de proyecto de aplicación para dispositivos móviles que puede usar para compilar una aplicación independiente específicamente para dispositivos móviles.</span><span class="sxs-lookup"><span data-stu-id="15966-108">To begin with, when you create a new ASP.NET MVC 4 project there is now a mobile application project template you can use to build a standalone app specifically for mobile devices.</span></span> <span data-ttu-id="15966-109">Además, ASP.NET MVC 4 se integra con jQuery Mobile a través de un paquete de NuGet jQuery.Mobile.MVC.</span><span class="sxs-lookup"><span data-stu-id="15966-109">Additionally, ASP.NET MVC 4 integrates with jQuery Mobile through a jQuery.Mobile.MVC NuGet package.</span></span> <span data-ttu-id="15966-110">jQuery Mobile es un marco basado en HTML5 para desarrollar aplicaciones web que son compatibles con todas las plataformas de dispositivos móviles populares, incluidos Windows Phone, iPhone, Android y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="15966-110">jQuery Mobile is an HTML5-based framework for developing web apps that are compatible with all popular mobile device platforms, including Windows Phone, iPhone, Android and so on.</span></span> <span data-ttu-id="15966-111">Sin embargo, si necesita especialización, ASP.NET MVC 4 también permite actuar distintas vistas para diferentes dispositivos y proporcionar las optimizaciones específicas del dispositivo.</span><span class="sxs-lookup"><span data-stu-id="15966-111">However, if you need specialization, ASP.NET MVC 4 also enables you to serve different views for different devices and provide device-specific optimizations.</span></span>
> 
> <span data-ttu-id="15966-112">En este laboratorio práctico, se iniciará con ASP.NET MVC 4 &quot;aplicación de Internet&quot; plantilla de proyecto para crear una aplicación de la Galería fotográfica.</span><span class="sxs-lookup"><span data-stu-id="15966-112">In this hands-on lab, you will start with the ASP.NET MVC 4 &quot;Internet Application&quot; project template to create a Photo Gallery application.</span></span> <span data-ttu-id="15966-113">Progresivamente mejorará la aplicación con jQuery Mobile y nuevas características de ASP.NET MVC 4 para que sea compatible con dispositivos móviles diferentes y exploradores web de escritorio.</span><span class="sxs-lookup"><span data-stu-id="15966-113">You will progressively enhance the app using jQuery Mobile and ASP.NET MVC 4's new features to make it compatible with different mobile devices and desktop web browsers.</span></span> <span data-ttu-id="15966-114">También obtendrá información sobre las recetas de código nuevo para la generación de código y lo ASP.NET MVC 4 que resulta más fácil de escribir métodos de acción asincrónicos admitiendo la tarea&lt;ActionResult&gt; tipos de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="15966-114">You will also learn about new code recipes for code generation and how ASP.NET MVC 4 makes it easier for you to write asynchronous action methods by supporting Task&lt;ActionResult&gt; return types.</span></span>
> 
> <span data-ttu-id="15966-115">Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web colonias, disponible en [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span><span class="sxs-lookup"><span data-stu-id="15966-115">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="15966-116">Objetivos</span><span class="sxs-lookup"><span data-stu-id="15966-116">Objectives</span></span>

<span data-ttu-id="15966-117">En este laboratorio práctico, aprenderá cómo:</span><span class="sxs-lookup"><span data-stu-id="15966-117">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="15966-118">Aprovechar las ventajas de las mejoras en las plantillas de proyecto incluidas ASP.NET MVC la nueva plantilla de proyecto de aplicación para dispositivos móviles</span><span class="sxs-lookup"><span data-stu-id="15966-118">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="15966-119">Utilice el atributo de la ventanilla de HTML5 y las consultas de medios CSS para mejorar la presentación en dispositivos móviles</span><span class="sxs-lookup"><span data-stu-id="15966-119">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="15966-120">Usar jQuery Mobile para mejorar el progresiva y para la creación de la interfaz de usuario web táctiles optimizadas</span><span class="sxs-lookup"><span data-stu-id="15966-120">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="15966-121">Crear vistas específicas de mobile</span><span class="sxs-lookup"><span data-stu-id="15966-121">Create mobile-specific views</span></span>
- <span data-ttu-id="15966-122">Utilice el componente de modificador de vista para alternar entre las vistas de escritorio y móviles en la aplicación</span><span class="sxs-lookup"><span data-stu-id="15966-122">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="15966-123">Crear controladores asincrónicos utilizando el soporte de tarea</span><span class="sxs-lookup"><span data-stu-id="15966-123">Create asynchronous controllers using task support</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="15966-124">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="15966-124">Prerequisites</span></span>

<span data-ttu-id="15966-125">Debe tener los elementos siguientes para completar esta práctica:</span><span class="sxs-lookup"><span data-stu-id="15966-125">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="15966-126">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (leer [Apéndice B](#AppendixB) para obtener instrucciones sobre cómo instalarlo).</span><span class="sxs-lookup"><span data-stu-id="15966-126">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>
- <span data-ttu-id="15966-127">[ASP.NET MVC 4](../../../mvc4.md) (incluido en la instalación de Microsoft Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="15966-127">[ASP.NET MVC 4](../../../mvc4.md) (included in the Microsoft Visual Studio 2012 installation)</span></span>
- <span data-ttu-id="15966-128">Emulador de Windows Phone (incluido en el [7.1.1 de Windows Phone SDK](https://www.microsoft.com/en-us/download/details.aspx?id=29233))</span><span class="sxs-lookup"><span data-stu-id="15966-128">Windows Phone Emulator (included in the [Windows Phone 7.1.1 SDK](https://www.microsoft.com/en-us/download/details.aspx?id=29233))</span></span>
- <span data-ttu-id="15966-129">Opcional: [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) con **Electric Plum iPhone simulador** extensión (solo para el ejercicio 3 usan para navegar por la aplicación web con un emulador de iPhone)</span><span class="sxs-lookup"><span data-stu-id="15966-129">Optional - [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) with **Electric Plum iPhone Simulator** extension (only for Exercise 3 used to browse the web application with an iPhone simulator)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="15966-130">Programa de instalación</span><span class="sxs-lookup"><span data-stu-id="15966-130">Setup</span></span>

<span data-ttu-id="15966-131">En este documento de laboratorio, le indicará que insertar bloques de código.</span><span class="sxs-lookup"><span data-stu-id="15966-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="15966-132">Para su comodidad, la mayor parte de ese código se proporciona como fragmentos de código de Visual Studio, puede utilizar desde dentro de Visual Studio para evitar tener que agregarla manualmente.</span><span class="sxs-lookup"><span data-stu-id="15966-132">For your convenience, most of that code is provided as Visual Studio Code Snippets, which you can use from within Visual Studio to avoid having to add it manually.</span></span>

<span data-ttu-id="15966-133">Para instalar los fragmentos de código:</span><span class="sxs-lookup"><span data-stu-id="15966-133">To install the code snippets:</span></span>

1. <span data-ttu-id="15966-134">Abra una ventana del explorador de Windows y vaya a la práctica **Source\Setup** carpeta.</span><span class="sxs-lookup"><span data-stu-id="15966-134">Open a Windows Explorer window and browse to the lab's **Source\Setup** folder.</span></span>
2. <span data-ttu-id="15966-135">Haga doble clic en el **Setup.cmd** archivo en esta carpeta para instalar los fragmentos de código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="15966-135">Double-click the **Setup.cmd** file in this folder to install the Visual Studio code snippets.</span></span>

<span data-ttu-id="15966-136">Si no está familiarizado con los fragmentos de código de Visual Studio y desea obtener información sobre cómo utilizarlas, puede consultar el apéndice de este documento &quot; [Apéndice A: Using Code Snippets](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="15966-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="15966-137">Ejercicios</span><span class="sxs-lookup"><span data-stu-id="15966-137">Exercises</span></span>

<span data-ttu-id="15966-138">Este laboratorio de prácticas incluye los ejercicios siguientes:</span><span class="sxs-lookup"><span data-stu-id="15966-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="15966-139">Nuevas plantillas de proyecto de MVC de ASP.NET 4</span><span class="sxs-lookup"><span data-stu-id="15966-139">New ASP.NET MVC 4 Project Templates</span></span>](#Exercise1)
2. [<span data-ttu-id="15966-140">Crear la aplicación Web de la Galería fotográfica</span><span class="sxs-lookup"><span data-stu-id="15966-140">Creating the Photo Gallery Web Application</span></span>](#Exercise2)
3. [<span data-ttu-id="15966-141">Agregar compatibilidad para dispositivos móviles</span><span class="sxs-lookup"><span data-stu-id="15966-141">Adding Support for Mobile Devices</span></span>](#Exercise3)
4. [<span data-ttu-id="15966-142">Usar controladores asincrónicos</span><span class="sxs-lookup"><span data-stu-id="15966-142">Using Asynchronous Controllers</span></span>](#Exercise4)

> [!NOTE]
> <span data-ttu-id="15966-143">Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debería obtener después de completar los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="15966-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="15966-144">Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="15966-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="15966-145">Tiempo estimado para completar esta práctica: **60 minutos**.</span><span class="sxs-lookup"><span data-stu-id="15966-145">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a><span data-ttu-id="15966-146">Ejercicio 1: Nuevas plantillas de proyecto de MVC de ASP.NET 4</span><span class="sxs-lookup"><span data-stu-id="15966-146">Exercise 1: New ASP.NET MVC 4 Project Templates</span></span>

<span data-ttu-id="15966-147">En este ejercicio, explorará las mejoras en las plantillas de proyecto de ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="15966-147">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 Project templates.</span></span> <span data-ttu-id="15966-148">Además de la plantilla de aplicación de Internet, ya está presente en MVC 3, esta versión ahora incluye una plantilla independiente para las aplicaciones móviles.</span><span class="sxs-lookup"><span data-stu-id="15966-148">In addition to the Internet Application template, already present in MVC 3, this version now includes a separate template for Mobile applications.</span></span> <span data-ttu-id="15966-149">En primer lugar, veremos algunas características relevantes de cada una de las plantillas.</span><span class="sxs-lookup"><span data-stu-id="15966-149">First, you will look at some relevant features of each of the templates.</span></span> <span data-ttu-id="15966-150">A continuación, va a trabajar en la página correctamente en las diferentes plataformas de representación utilizando el enfoque correcto.</span><span class="sxs-lookup"><span data-stu-id="15966-150">Then, you will work on rendering your page properly on the different platforms by using the right approach.</span></span>

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a><span data-ttu-id="15966-151">Tarea 1: explorar la plantilla de aplicación de Internet</span><span class="sxs-lookup"><span data-stu-id="15966-151">Task 1 - Exploring the Internet Application Template</span></span>

1. <span data-ttu-id="15966-152">Abra **Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="15966-152">Open **Visual Studio**.</span></span>
2. <span data-ttu-id="15966-153">Seleccione el **archivo | Nuevos | Proyecto** comando de menú.</span><span class="sxs-lookup"><span data-stu-id="15966-153">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="15966-154">En el **nuevo proyecto** cuadro de diálogo, seleccione la **Visual C# | Web** plantilla en el panel izquierdo del árbol y elija **aplicación Web de ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="15966-154">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="15966-155">Denomine el proyecto **PhotoGallery**, seleccione una ubicación (o deje el valor predeterminado) y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="15966-155">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="15966-156">Más adelante, personalizará la solución de la Galería fotográfica ASP.NET MVC 4 que ahora va a crear.</span><span class="sxs-lookup"><span data-stu-id="15966-156">You will later customize the PhotoGallery ASP.NET MVC 4 solution you are now creating.</span></span>

    <span data-ttu-id="15966-157">![Crear un nuevo proyecto](whats-new-in-aspnet-mvc-4/_static/image1.png "crear un nuevo proyecto")</span><span class="sxs-lookup"><span data-stu-id="15966-157">![Creating a new project](whats-new-in-aspnet-mvc-4/_static/image1.png "Creating a new project")</span></span>

    <span data-ttu-id="15966-158">*Crear un nuevo proyecto*</span><span class="sxs-lookup"><span data-stu-id="15966-158">*Creating a new project*</span></span>
3. <span data-ttu-id="15966-159">En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione la **aplicación de Internet** plantilla de proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="15966-159">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="15966-160">Asegúrese de que ha seleccionado Razor como el motor de vista.</span><span class="sxs-lookup"><span data-stu-id="15966-160">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="15966-161">![Crear una nueva aplicación de Internet de ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image2.png "crear una nueva aplicación de Internet de ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="15966-161">![Creating a new ASP.NET MVC 4 Internet Application](whats-new-in-aspnet-mvc-4/_static/image2.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="15966-162">*Crear una nueva aplicación de Internet de ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="15966-162">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="15966-163">Se ha introducido la sintaxis Razor en ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="15966-163">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="15966-164">Su objetivo es reducir el número de caracteres y pulsaciones de teclas necesarias en un archivo, lo que permite una rápida y fluido de flujo de trabajo de codificación.</span><span class="sxs-lookup"><span data-stu-id="15966-164">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="15966-165">Aprovecha Razor existente C# / VB (u otros) dominio del idioma y ofrece una sintaxis de marcado de plantilla que permite a un flujo de trabajo de construcción de HTML Maravilla.</span><span class="sxs-lookup"><span data-stu-id="15966-165">Razor leverages existing C# / VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="15966-166">Presione **F5** para ejecutar la solución y ver las plantillas renovadas.</span><span class="sxs-lookup"><span data-stu-id="15966-166">Press **F5** to run the solution and see the renewed templates.</span></span> <span data-ttu-id="15966-167">Puede consultar las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="15966-167">You can check out the following features:</span></span>

    <span data-ttu-id="15966-168">**Plantillas de estilo moderno**</span><span class="sxs-lookup"><span data-stu-id="15966-168">**Modern-style templates**</span></span>

    <span data-ttu-id="15966-169">Las plantillas que se han renovado, proporcionando más estilos de aspecto moderno.</span><span class="sxs-lookup"><span data-stu-id="15966-169">The templates have been renewed, providing more modern-looking styles.</span></span>

    <span data-ttu-id="15966-170">![Plantillas de MVC de ASP.NET 4 estilo](whats-new-in-aspnet-mvc-4/_static/image3.png "estilo plantillas de MVC 4")</span><span class="sxs-lookup"><span data-stu-id="15966-170">![ASP.NET MVC 4 restyled templates](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 restyled templates")</span></span>

    <span data-ttu-id="15966-171">*Plantillas de estilo de ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="15966-171">*ASP.NET MVC 4 restyled templates*</span></span>

    <span data-ttu-id="15966-172">![Nuevo contacto, página](whats-new-in-aspnet-mvc-4/_static/image4.png "página nuevo contacto")</span><span class="sxs-lookup"><span data-stu-id="15966-172">![New Contact page](whats-new-in-aspnet-mvc-4/_static/image4.png "New Contact page")</span></span>

    <span data-ttu-id="15966-173">*Nuevo contacto, página*</span><span class="sxs-lookup"><span data-stu-id="15966-173">*New Contact page*</span></span>

    <span data-ttu-id="15966-174">**Representación adaptable**</span><span class="sxs-lookup"><span data-stu-id="15966-174">**Adaptive Rendering**</span></span>

    <span data-ttu-id="15966-175">Consulte Cambiar el tamaño de la ventana del explorador y observe cómo el diseño de página se adapta dinámicamente al tamaño de la ventana nueva.</span><span class="sxs-lookup"><span data-stu-id="15966-175">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="15966-176">Estas plantillas usan la técnica de representación adaptable para representar correctamente en plataformas de escritorio y móviles sin ninguna personalización.</span><span class="sxs-lookup"><span data-stu-id="15966-176">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

    <span data-ttu-id="15966-177">![Plantilla de proyecto de ASP.NET MVC 4 de tamaños diferentes del explorador](whats-new-in-aspnet-mvc-4/_static/image5.png "plantilla de proyecto de ASP.NET MVC 4 de tamaños diferentes del explorador")</span><span class="sxs-lookup"><span data-stu-id="15966-177">![ASP.NET MVC 4 project template in different browser sizes](whats-new-in-aspnet-mvc-4/_static/image5.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

    <span data-ttu-id="15966-178">*Plantilla de proyecto de ASP.NET MVC 4 de tamaños diferentes del explorador*</span><span class="sxs-lookup"><span data-stu-id="15966-178">*ASP.NET MVC 4 project template in different browser sizes*</span></span>

    <span data-ttu-id="15966-179">**Interfaz de usuario más enriquecida con JavaScript**</span><span class="sxs-lookup"><span data-stu-id="15966-179">**Richer UI with JavaScript**</span></span>

    <span data-ttu-id="15966-180">Otra mejora para plantillas de proyecto predeterminadas es el uso de JavaScript para proporcionar un JavaScript más interactivo.</span><span class="sxs-lookup"><span data-stu-id="15966-180">Another enhancement to default project templates is the use of JavaScript to provide a more interactive JavaScript.</span></span> <span data-ttu-id="15966-181">Los vínculos de inicio de sesión y de registro usados en la plantilla son elementos que describen cómo usar la validaciones de jQuery para validar los campos de entrada del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="15966-181">The Login and Register links used in the template exemplify how to use the jQuery Validations to validate the input fields from client-side.</span></span>

    ![Validación de jQuery](whats-new-in-aspnet-mvc-4/_static/image6.png)

    <span data-ttu-id="15966-183">*Validación de jQuery*</span><span class="sxs-lookup"><span data-stu-id="15966-183">*jQuery Validation*</span></span>

    > [!NOTE]
    > <span data-ttu-id="15966-184">Aviso de que registro de los dos en secciones, en la primera sección se puede iniciar sesión con una cuenta registrado desde el sitio y en la segunda sección que puede altenativelly sesión con otro servicio de autenticación como google (deshabilitada de forma predeterminada).</span><span class="sxs-lookup"><span data-stu-id="15966-184">Notice the two log in sections, in the first section you can log in using a registerd account from the site and in the second section you can altenativelly log in using another authentication service like google (disabled by default).</span></span>
5. <span data-ttu-id="15966-185">Cierre el explorador para detener al depurador y vuelva a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="15966-185">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="15966-186">Abra el archivo **AuthConfig.cs** situado bajo el **aplicación\_iniciar** carpeta.</span><span class="sxs-lookup"><span data-stu-id="15966-186">Open the file **AuthConfig.cs** located under the **App\_Start** folder.</span></span>
7. <span data-ttu-id="15966-187">Quite el comentario de la última línea para registrar el cliente de Google para *OAuth* autenticación.</span><span class="sxs-lookup"><span data-stu-id="15966-187">Remove the comment from the last line to register Google client for *OAuth* authentication.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > <span data-ttu-id="15966-188">Tenga en cuenta que puede habilitar fácilmente la autenticación mediante cualquier servicio OpenID o OAuth como Facebook, Twitter, Microsoft, etcetera.</span><span class="sxs-lookup"><span data-stu-id="15966-188">Notice you can easily enable authentication using any OpenID or OAuth service like Facebook, Twitter, Microsoft, etc.</span></span>
8. <span data-ttu-id="15966-189">Presione **F5** para ejecutar la solución y navegar hasta la página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="15966-189">Press **F5** to run the solution and navigate to the login page.</span></span>
9. <span data-ttu-id="15966-190">Seleccione **Google** servicio para iniciar sesión.</span><span class="sxs-lookup"><span data-stu-id="15966-190">Select **Google** service to log in.</span></span>

    ![Seleccionar el registro de servicio](whats-new-in-aspnet-mvc-4/_static/image7.png)

    <span data-ttu-id="15966-192">*Seleccionar el registro de servicio*</span><span class="sxs-lookup"><span data-stu-id="15966-192">*Selecting the log in service*</span></span>
10. <span data-ttu-id="15966-193">Inicie sesión con su cuenta de Google.</span><span class="sxs-lookup"><span data-stu-id="15966-193">Log in using your Google account.</span></span>
11. <span data-ttu-id="15966-194">Permitir que el sitio (localhost) recuperar información de cuenta de Google.</span><span class="sxs-lookup"><span data-stu-id="15966-194">Allow the site (localhost) to retrieve information from Google account.</span></span>
12. <span data-ttu-id="15966-195">Por último, tendrá que registrar en el sitio para asociar la cuenta de Google.</span><span class="sxs-lookup"><span data-stu-id="15966-195">Finally, you will have to register in the site to associate the Google account.</span></span>

    ![Asociar su cuenta de Google](whats-new-in-aspnet-mvc-4/_static/image8.png)

    <span data-ttu-id="15966-197">*Asociar su cuenta de Google*</span><span class="sxs-lookup"><span data-stu-id="15966-197">*Associating your Google account*</span></span>
13. <span data-ttu-id="15966-198">Cierre el explorador para detener al depurador y vuelva a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="15966-198">Close the browser to stop the debugger and return to Visual Studio.</span></span>
14. <span data-ttu-id="15966-199">Ahora, explore la solución para desproteger algunas otras nuevas características introducidas en ASP.NET MVC 4 en la plantilla de proyecto.</span><span class="sxs-lookup"><span data-stu-id="15966-199">Now explore the solution to check out some other new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="15966-200">![La plantilla de proyecto de aplicación de ASP.NET MVC 4 Internet](whats-new-in-aspnet-mvc-4/_static/image9.png "la plantilla de proyecto de aplicación de Internet de ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="15966-200">![The ASP.NET MVC 4 Internet Application Project Template](whats-new-in-aspnet-mvc-4/_static/image9.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="15966-201">*La plantilla de proyecto de aplicación de Internet de ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="15966-201">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

    - <span data-ttu-id="15966-202">**HTML 5 marcado**</span><span class="sxs-lookup"><span data-stu-id="15966-202">**HTML 5 Markup**</span></span>

        <span data-ttu-id="15966-203">Examinar vistas de plantilla para averiguar la nueva marca de tema.</span><span class="sxs-lookup"><span data-stu-id="15966-203">Browse template views to find out the new theme markup.</span></span>

        <span data-ttu-id="15966-204">![Nueva plantilla, uso de marcado de Razor y HTML5 About.cshtml. ] (whats-new-in-aspnet-mvc-4/_static/image10.png "Nueva plantilla, uso de marcado de Razor y HTML5 About.cshtml.")</span><span class="sxs-lookup"><span data-stu-id="15966-204">![New template, using Razor and HTML5 markup About.cshtml.](whats-new-in-aspnet-mvc-4/_static/image10.png "New template, using Razor and HTML5 markup About.cshtml.")</span></span>

        <span data-ttu-id="15966-205">*Nueva plantilla, uso de marcado de Razor y HTML5 (About.cshtml).*</span><span class="sxs-lookup"><span data-stu-id="15966-205">*New template, using Razor and HTML5 markup (About.cshtml).*</span></span>
    - <span data-ttu-id="15966-206">**Bibliotecas actualizadas de JavaScript**</span><span class="sxs-lookup"><span data-stu-id="15966-206">**Updated JavaScript libraries**</span></span>

        <span data-ttu-id="15966-207">La plantilla predeterminada de ASP.NET MVC 4 incluye ahora KnockoutJS, un marco de JavaScript MVVM que le permite crear enriquecidos y aplicaciones de alta capacidad de respuesta web con JavaScript y HTML.</span><span class="sxs-lookup"><span data-stu-id="15966-207">The ASP.NET MVC 4 default template now includes KnockoutJS, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="15966-208">Al igual que en MVC3, jQuery y jQuery bibliotecas de interfaz de usuario también se incluyen en ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="15966-208">Like in MVC3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

        > [!NOTE]
        > <span data-ttu-id="15966-209">Puede obtener más información acerca de la biblioteca de KnockOutJS en este vínculo: [ [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="15966-209">You can get more information about KnockOutJS library in this link: [[http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/).</span></span> <span data-ttu-id="15966-210">Además, puede obtener información acerca de jQuery y jQuery UI en [ [http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="15966-210">Additionally, you can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a><span data-ttu-id="15966-211">Tarea 2: explorar la plantilla de aplicación para dispositivos móviles</span><span class="sxs-lookup"><span data-stu-id="15966-211">Task 2 - Exploring the Mobile Application Template</span></span>

<span data-ttu-id="15966-212">ASP.NET MVC 4 facilita el desarrollo de sitios Web para dispositivos móviles y exploradores de Tablet PC.</span><span class="sxs-lookup"><span data-stu-id="15966-212">ASP.NET MVC 4 facilitates the development of websites for mobile and tablet browsers.</span></span> <span data-ttu-id="15966-213">Esta plantilla tiene la misma estructura de aplicación que la plantilla de aplicación de Internet (Observe que el código del controlador es prácticamente idéntico), pero su estilo se modificó para que se representen correctamente en dispositivos móviles basados en la entrada táctil.</span><span class="sxs-lookup"><span data-stu-id="15966-213">This template has the same application structure as the Internet Application template (notice that the controller code is practically identical), but its style was modified to render properly in touch-based mobile devices.</span></span>

1. <span data-ttu-id="15966-214">Seleccione el **archivo | Nuevos | Proyecto** comando de menú.</span><span class="sxs-lookup"><span data-stu-id="15966-214">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="15966-215">En el **nuevo proyecto** cuadro de diálogo, seleccione la **Visual C# | Web** plantilla en el panel izquierdo del árbol y elija la **aplicación Web de ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="15966-215">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="15966-216">Denomine el proyecto **PhotoGallery.Mobile**, seleccione una ubicación (o deje el valor predeterminado), seleccione &quot;agregar a solución&quot; y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="15966-216">Name the project **PhotoGallery.Mobile**, select a location (or leave the default), select &quot;Add to solution&quot; and click **OK**.</span></span>
2. <span data-ttu-id="15966-217">En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione la **aplicación Mobile** plantilla de proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="15966-217">In the **New ASP.NET MVC 4 Project** dialog, select the **Mobile Application** project template and click **OK**.</span></span> <span data-ttu-id="15966-218">Asegúrese de que ha seleccionado Razor como el motor de vista.</span><span class="sxs-lookup"><span data-stu-id="15966-218">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="15966-219">![Crear una nueva aplicación móvil de ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image11.png "crear una nueva aplicación móvil de ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="15966-219">![Creating a new ASP.NET MVC 4 Mobile Application](whats-new-in-aspnet-mvc-4/_static/image11.png "Creating a new ASP.NET MVC 4 Mobile Application")</span></span>

    <span data-ttu-id="15966-220">*Crear una nueva aplicación móvil de ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="15966-220">*Creating a new ASP.NET MVC 4 Mobile Application*</span></span>
3. <span data-ttu-id="15966-221">Ahora es posible explorar la solución y extraer del repositorio algunas de las nuevas características introducidas por la plantilla de solución de ASP.NET MVC 4 para dispositivos móviles:</span><span class="sxs-lookup"><span data-stu-id="15966-221">Now you are able to explore the solution and check out some of the new features introduced by the ASP.NET MVC 4 solution template for mobile:</span></span>

    - <span data-ttu-id="15966-222">**jQuery Mobile biblioteca**</span><span class="sxs-lookup"><span data-stu-id="15966-222">**jQuery Mobile Library**</span></span>

        <span data-ttu-id="15966-223">La plantilla de proyecto de aplicación Mobile incluye la biblioteca de jQuery móvil, que es una biblioteca de código abierto para la compatibilidad de explorador móvil.</span><span class="sxs-lookup"><span data-stu-id="15966-223">The Mobile Application project template includes the jQuery Mobile library, which is an open source library for mobile browser compatibility.</span></span> <span data-ttu-id="15966-224">jQuery Mobile aplica mejora progresiva a los exploradores móviles que admiten CSS y JavaScript.</span><span class="sxs-lookup"><span data-stu-id="15966-224">jQuery Mobile applies progressive enhancement to mobile browsers that support CSS and JavaScript.</span></span> <span data-ttu-id="15966-225">Mejora progresiva permite todos los exploradores mostrar el contenido básico de una página web, mientras que sólo permite que los exploradores más eficaces mostrar el contenido enriquecido.</span><span class="sxs-lookup"><span data-stu-id="15966-225">Progressive enhancement enables all browsers to display the basic content of a web page, while it only enables the most powerful browsers to display the rich content.</span></span> <span data-ttu-id="15966-226">Los archivos JavaScript y CSS, incluidos en el estilo de dispositivos móvil, jQuery ayudan a los exploradores móviles para adaptarlos al contenido en la pantalla sin realizar ningún cambio en el marcado de la página.</span><span class="sxs-lookup"><span data-stu-id="15966-226">The JavaScript and CSS files, included in the jQuery Mobile style, help mobile browsers to fit the content in the screen without making any change in the page markup.</span></span>

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        <span data-ttu-id="15966-228">*biblioteca de jQuery móvil incluido en la plantilla*</span><span class="sxs-lookup"><span data-stu-id="15966-228">*jQuery mobile library included in the template*</span></span>
    - <span data-ttu-id="15966-229">**Marcado en función de HTML5**</span><span class="sxs-lookup"><span data-stu-id="15966-229">**HTML5 based markup**</span></span>

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        <span data-ttu-id="15966-231">*Plantilla de aplicaciones móviles con HTML5 marcado, (Login.cshtml y index.cshtml)*</span><span class="sxs-lookup"><span data-stu-id="15966-231">*Mobile application template using HTML5 markup, (Login.cshtml and index.cshtml)*</span></span>
4. <span data-ttu-id="15966-232">Presione **F5** para ejecutar la solución.</span><span class="sxs-lookup"><span data-stu-id="15966-232">Press **F5** to run the solution.</span></span>
5. <span data-ttu-id="15966-233">Abra la **Windows Phone 7 emulador**.</span><span class="sxs-lookup"><span data-stu-id="15966-233">Open the **Windows Phone 7 Emulator**.</span></span>
6. <span data-ttu-id="15966-234">En la pantalla de inicio de teléfono, abra Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="15966-234">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="15966-235">Visite la dirección URL que se inicia la aplicación de escritorio y vaya a esa dirección URL desde el teléfono (p. ej. `http://localhost:[PortNumber]/`).</span><span class="sxs-lookup"><span data-stu-id="15966-235">Check out the URL where the desktop application started and browse to that URL from the phone (e.g. `http://localhost:[PortNumber]/`).</span></span>
7. <span data-ttu-id="15966-236">Ahora puede especificar la página de inicio de sesión o desproteger el acerca de la página.</span><span class="sxs-lookup"><span data-stu-id="15966-236">Now you are able to enter the login page or check out the about page.</span></span> <span data-ttu-id="15966-237">Tenga en cuenta que el estilo del sitio Web se basa en la nueva aplicación de Metro para dispositivos móviles.</span><span class="sxs-lookup"><span data-stu-id="15966-237">Notice that the style of the website is based on the new Metro app for mobile.</span></span> <span data-ttu-id="15966-238">La plantilla de proyecto de ASP.NET MVC 4 se muestra correctamente en los dispositivos móviles, asegurándose de que todos los elementos de la página son visibles y están habilitados.</span><span class="sxs-lookup"><span data-stu-id="15966-238">The ASP.NET MVC 4 project template is correctly displayed on mobile devices, making sure all the elements of the page are visible and enabled.</span></span> <span data-ttu-id="15966-239">Tenga en cuenta que los vínculos en el encabezado son lo suficientemente grandes como se hizo clic o puntea.</span><span class="sxs-lookup"><span data-stu-id="15966-239">Notice that the links on the header are big enough to be clicked or tapped.</span></span>

    <span data-ttu-id="15966-240">![Páginas de la plantilla en un dispositivo móvil del proyecto](whats-new-in-aspnet-mvc-4/_static/image14.png "proyecto páginas de plantilla en un dispositivo móvil")</span><span class="sxs-lookup"><span data-stu-id="15966-240">![Project template pages in a mobile device](whats-new-in-aspnet-mvc-4/_static/image14.png "Project template pages in a mobile device")</span></span>

    <span data-ttu-id="15966-241">*Páginas de la plantilla de proyecto en un dispositivo móvil*</span><span class="sxs-lookup"><span data-stu-id="15966-241">*Project template pages in a mobile device*</span></span>
8. <span data-ttu-id="15966-242">La nueva plantilla también usa el **ventanilla meta etiqueta**.</span><span class="sxs-lookup"><span data-stu-id="15966-242">The new template also uses the **Viewport meta tag**.</span></span> <span data-ttu-id="15966-243">Exploradores móviles más definen un ancho de una ventana del explorador virtual o &quot;ventanilla&quot;, que es mayor que el ancho real del dispositivo móvil.</span><span class="sxs-lookup"><span data-stu-id="15966-243">Most mobile browsers define a width for a virtual browser window or &quot;viewport&quot;, which is larger than the actual width of the mobile device.</span></span> <span data-ttu-id="15966-244">Esto permite que los exploradores móviles mostrar toda la página web dentro de la pantalla virtual.</span><span class="sxs-lookup"><span data-stu-id="15966-244">This enables mobile browsers to display the entire web page inside the virtual display.</span></span> <span data-ttu-id="15966-245">El **ventanilla meta etiqueta** permite a los desarrolladores de web establecer el ancho, el alto y la escala del área del explorador en dispositivos móviles **.**</span><span class="sxs-lookup"><span data-stu-id="15966-245">The **Viewport meta tag** allows web developers to set the width, height and scale of the browser area on mobile devices **.**</span></span> <span data-ttu-id="15966-246">La plantilla de ASP.NET MVC 4 para las aplicaciones móviles establece la ventanilla para el ancho del dispositivo (&quot;ancho = dispositivo ancho&quot;) en la plantilla de diseño (*Views\Shared\_Layout.cshtml*), de modo que todos los el páginas tendrá su ventanilla establecida para el ancho de pantalla del dispositivo.</span><span class="sxs-lookup"><span data-stu-id="15966-246">The ASP.NET MVC 4 template for Mobile Applications sets the viewport to the device width (&quot;width=device-width&quot;) in the layout template (*Views\Shared\_Layout.cshtml*), so that all the pages will have their viewport set to the device screen width.</span></span> <span data-ttu-id="15966-247">Tenga en cuenta que la etiqueta meta de ventanilla no cambiará la vista de explorador predeterminado.</span><span class="sxs-lookup"><span data-stu-id="15966-247">Notice that the Viewport meta tag will not change the default browser view.</span></span>
9. <span data-ttu-id="15966-248">Abra  **\_Layout.cshtml**, que se encuentra en la **vistas | Compartido** carpeta, y marque como comentario la etiqueta meta de la ventanilla.</span><span class="sxs-lookup"><span data-stu-id="15966-248">Open **\_Layout.cshtml**, located in the **Views | Shared** folder, and comment the Viewport meta tag.</span></span> <span data-ttu-id="15966-249">Ejecute la aplicación, si no ya abierto y desproteger las diferencias.</span><span class="sxs-lookup"><span data-stu-id="15966-249">Run the application, if not already opened, and check out the differences.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

    <span data-ttu-id="15966-250">![El sitio después de la creación de comentarios de la ventanilla meta etiqueta](whats-new-in-aspnet-mvc-4/_static/image15.png "el sitio después de la creación de comentarios de la ventanilla meta etiqueta")</span><span class="sxs-lookup"><span data-stu-id="15966-250">![The site after commenting the viewport meta tag](whats-new-in-aspnet-mvc-4/_static/image15.png "The site after commenting the viewport meta tag")</span></span>

    <span data-ttu-id="15966-251">*El sitio después de la creación de comentarios de la ventanilla meta etiqueta*</span><span class="sxs-lookup"><span data-stu-id="15966-251">*The site after commenting the viewport meta tag*</span></span>
10. <span data-ttu-id="15966-252">En Visual Studio, presione **MAYÚS** + **F5** para detener la depuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="15966-252">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
11. <span data-ttu-id="15966-253">Quite el comentario de la etiqueta de metadatos de la ventanilla.</span><span class="sxs-lookup"><span data-stu-id="15966-253">Uncomment the viewport meta tag.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a><span data-ttu-id="15966-254">Tarea 3: usar la representación adaptable</span><span class="sxs-lookup"><span data-stu-id="15966-254">Task 3 - Using Adaptive Rendering</span></span>

<span data-ttu-id="15966-255">En esta tarea, obtendrá información sobre otro método para presentar una página Web correctamente en los dispositivos móviles y exploradores Web al mismo tiempo sin ninguna personalización.</span><span class="sxs-lookup"><span data-stu-id="15966-255">In this task, you will learn another method to render a Web page correctly on mobile devices and Web browsers at the same time without any customization.</span></span> <span data-ttu-id="15966-256">Ya ha usado ventanilla meta etiqueta con un propósito similar.</span><span class="sxs-lookup"><span data-stu-id="15966-256">You have already used Viewport meta tag with a similar purpose.</span></span> <span data-ttu-id="15966-257">Ahora que adapta a otro método eficaz: *representación adaptable*.</span><span class="sxs-lookup"><span data-stu-id="15966-257">Now you will meet another powerful method: *adaptive rendering*.</span></span>

<span data-ttu-id="15966-258">Representación adaptable es una técnica que usa **consultas de medios de CSS3** para personalizar el estilo aplicado a una página.</span><span class="sxs-lookup"><span data-stu-id="15966-258">Adaptive rendering is a technique that uses **CSS3 media queries** to customize the style applied to a page.</span></span> <span data-ttu-id="15966-259">Consultas de medios definen las condiciones dentro de una hoja de estilos, agrupar los estilos CSS en una determinada condición.</span><span class="sxs-lookup"><span data-stu-id="15966-259">Media queries define conditions inside a style sheet, grouping CSS styles under a certain condition.</span></span> <span data-ttu-id="15966-260">Solo cuando la condición es true, el estilo se aplica a los objetos declarados.</span><span class="sxs-lookup"><span data-stu-id="15966-260">Only when the condition is true, the style is applied to the declared objects.</span></span>

<span data-ttu-id="15966-261">La flexibilidad proporcionada por la técnica de representación adaptable permite cualquier personalización para mostrar el sitio en diferentes dispositivos.</span><span class="sxs-lookup"><span data-stu-id="15966-261">The flexibility provided by the adaptive rendering technique enables any customization for displaying the site on different devices.</span></span> <span data-ttu-id="15966-262">Puede definir tantos estilos como desee en una sola hoja de estilos sin tener que escribir código con la lógica para elegir el estilo.</span><span class="sxs-lookup"><span data-stu-id="15966-262">You can define as many styles as you want on a single style sheet without writing logic code to choose the style.</span></span> <span data-ttu-id="15966-263">Por lo tanto, es una manera muy ordenada de adaptar los estilos de la página, ya que reduce la cantidad de código duplicado y la lógica para fines de representación.</span><span class="sxs-lookup"><span data-stu-id="15966-263">Therefore, it is a very neat way of adapting page styles, as it reduces the amount of duplicated code and logic for rendering purposes.</span></span> <span data-ttu-id="15966-264">Por otro lado, el consumo de ancho de banda aumentaría, como el tamaño de los archivos CSS podría aumentar ligeramente.</span><span class="sxs-lookup"><span data-stu-id="15966-264">On the other hand, bandwidth consumption would increase, as the size of your CSS files could grow marginally.</span></span>

<span data-ttu-id="15966-265">Mediante la técnica de representación adaptable, su sitio será **muestre correctamente, independientemente del explorador.**</span><span class="sxs-lookup"><span data-stu-id="15966-265">By using the adaptive rendering technique, your site will be **displayed properly, regardless of the browser.**</span></span> <span data-ttu-id="15966-266">Sin embargo, debe considerar si se carga el ancho de banda adicional es un problema.</span><span class="sxs-lookup"><span data-stu-id="15966-266">However, you should consider if the bandwidth extra load is a concern.</span></span>

> [!NOTE]
> <span data-ttu-id="15966-267">El formato básico de una consulta de medios es: @media \[ámbito: todos | mano | imprimir | proyección | pantalla\] ([: valor de propiedad] y... [propiedad: valor])</span><span class="sxs-lookup"><span data-stu-id="15966-267">The basic format of a media query is: @media \[Scope: all | handheld | print | projection | screen\] ([property:value] and ... [property:value])</span></span>


<span data-ttu-id="15966-268">Ejemplos de consultas de medios: &gt;  **@media todos y (ancho máximo: 1000px) y (min ancho: 700px) {}:** para todas las resoluciones entre 700px y 1000px.</span><span class="sxs-lookup"><span data-stu-id="15966-268">Examples of media queries: &gt;**@media all and (max-width: 1000px) and (min-width: 700px) {}:** For all the resolutions between 700px and 1000px.</span></span>

> <span data-ttu-id="15966-269">**@mediapantalla y (min ancho: 400px) y (ancho máximo: 700px) {...}:** solo para las pantallas.</span><span class="sxs-lookup"><span data-stu-id="15966-269">**@media screen and (min-width: 400px) and (max-width: 700px) { ... }:** Only for screens.</span></span> <span data-ttu-id="15966-270">La resolución debe estar entre 400 y 700px.</span><span class="sxs-lookup"><span data-stu-id="15966-270">The resolution must be between 400 and 700px.</span></span>
> 
> <span data-ttu-id="15966-271">**@mediaportátiles y (min ancho: 20em), pantalla y (min ancho: 20em) {...}:** para dispositivos de mano (mobile y dispositivos) y las pantallas.</span><span class="sxs-lookup"><span data-stu-id="15966-271">**@media handheld and (min-width: 20em), screen and (min-width: 20em) { ... }:** For handhelds(mobile and devices) and screens.</span></span> <span data-ttu-id="15966-272">El ancho mínimo debe ser mayor que 20em.</span><span class="sxs-lookup"><span data-stu-id="15966-272">The minimum width must be greater than 20em.</span></span>
> 
> <span data-ttu-id="15966-273">Puede encontrar más información sobre esto en la [sitio W3C](http://www.w3.org/TR/css3-mediaqueries/).</span><span class="sxs-lookup"><span data-stu-id="15966-273">You can find more information about this on the [W3C site](http://www.w3.org/TR/css3-mediaqueries/).</span></span>


<span data-ttu-id="15966-274">Ahora explorará el funcionamiento de la representación adaptable, mejorar la legibilidad de ASP.NET MVC 4 predeterminados plantilla de sitio Web.</span><span class="sxs-lookup"><span data-stu-id="15966-274">You will now explore how the adaptive rendering works, improving the readability of the ASP.NET MVC 4 default website template.</span></span>

1. <span data-ttu-id="15966-275">Abra la **PhotoGallery.sln** solución se ha creado en la tarea 1 y se selecciona el **PhotoGallery** proyecto.</span><span class="sxs-lookup"><span data-stu-id="15966-275">Open the **PhotoGallery.sln** solution you have created at Task 1 and select the **PhotoGallery** project.</span></span> <span data-ttu-id="15966-276">Presione **F5** para ejecutar la solución.</span><span class="sxs-lookup"><span data-stu-id="15966-276">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="15966-277">Cambiar el tamaño de ancho del explorador, establecer las ventanas a la mitad o menos de un cuarto de su tamaño original.</span><span class="sxs-lookup"><span data-stu-id="15966-277">Resize the browser's width, setting the windows to half or to less than a quarter of its original size.</span></span> <span data-ttu-id="15966-278">Tenga en cuenta lo que ocurre con los elementos en el encabezado: algunos elementos no aparecerán en el área visible del encabezado.</span><span class="sxs-lookup"><span data-stu-id="15966-278">Notice what happens with the items in the header: Some elements will not appear in the visible area of the header.</span></span>
3. <span data-ttu-id="15966-279">Abra **Site.css** archivo desde el Explorador de soluciones de Visual Studio, ubicado en **contenido** carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="15966-279">Open **Site.css** file from the Visual Studio Solution explorer, located in **Content** project folder.</span></span> <span data-ttu-id="15966-280">Presione **CTRL + F** para abrir la búsqueda integrada en Visual Studio y escribir  **@media**  para buscar la **consulta de medios CSS**.</span><span class="sxs-lookup"><span data-stu-id="15966-280">Press **CTRL + F** to open Visual Studio integrated search, and write **@media** to locate the **CSS media query**.</span></span>

    <span data-ttu-id="15966-281">La condición de consulta de medios definida en esta plantilla funciona de esta manera: cuando el tamaño de la ventana del explorador está por debajo **850 px**, las reglas CSS que se aplican son los definidos dentro de este bloque de medios.</span><span class="sxs-lookup"><span data-stu-id="15966-281">The media query condition defined in this template works in this way: When the browser's window size is below **850 px**, the CSS rules applied are the ones defined inside this media block.</span></span>

    <span data-ttu-id="15966-282">![Buscando la consulta de medios](whats-new-in-aspnet-mvc-4/_static/image16.png "buscando la consulta de medios")</span><span class="sxs-lookup"><span data-stu-id="15966-282">![Locating the media query](whats-new-in-aspnet-mvc-4/_static/image16.png "Locating the media query")</span></span>

    <span data-ttu-id="15966-283">*Buscando la consulta de medios*</span><span class="sxs-lookup"><span data-stu-id="15966-283">*Locating the media query*</span></span>
4. <span data-ttu-id="15966-284">Reemplace el valor de atributo de ancho máximo establecido en 850 px con **10px**, con el fin de deshabilitar la representación adaptable y presione **CTRL + S** para guardar los cambios.</span><span class="sxs-lookup"><span data-stu-id="15966-284">Replace the max-width attribute value set in 850 px with **10px**, in order to disable the adaptive rendering, and press **CTRL + S** to save the changes.</span></span> <span data-ttu-id="15966-285">Volver al explorador y presione **CTRL + F5** para actualizar la página con los cambios realizados.</span><span class="sxs-lookup"><span data-stu-id="15966-285">Return to the browser and press **CTRL + F5** to refresh the page with the changes you have made.</span></span> <span data-ttu-id="15966-286">Tenga en cuenta las diferencias en ambas páginas al ajustar el ancho de la ventana.</span><span class="sxs-lookup"><span data-stu-id="15966-286">Notice the differences in both pages when adjusting the width of the window.</span></span>

    <span data-ttu-id="15966-287">![En la izquierda, la página está aplicando el @media se omite style, en la parte derecha, el estilo de](whats-new-in-aspnet-mvc-4/_static/image17.png "en la izquierda, está aplicando la página el @media se omite style, en la parte derecha, el estilo de")</span><span class="sxs-lookup"><span data-stu-id="15966-287">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image17.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="15966-288">*En la izquierda, la página está aplicando la @media se omite style, en la parte derecha, el estilo*</span><span class="sxs-lookup"><span data-stu-id="15966-288">*In the left, the page is applying the @media style, in the right, the style is omitted*</span></span>

    <span data-ttu-id="15966-289">Ahora, vamos a desproteger lo que sucede en los dispositivos móviles:</span><span class="sxs-lookup"><span data-stu-id="15966-289">Now, let's check out what happens on mobile devices:</span></span>

    <span data-ttu-id="15966-290">![En la izquierda, la página está aplicando el @media se omite style, en la parte derecha, el estilo de](whats-new-in-aspnet-mvc-4/_static/image18.png "en la izquierda, está aplicando la página el @media se omite style, en la parte derecha, el estilo de")</span><span class="sxs-lookup"><span data-stu-id="15966-290">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image18.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="15966-291">*En la izquierda, la página está aplicando la @media se omite style, en la parte derecha, el estilo*</span><span class="sxs-lookup"><span data-stu-id="15966-291">*In the left, the page is applying the @media style, in the right, the style is omitted*</span></span>

    <span data-ttu-id="15966-292">Aunque, observará que los cambios cuando se representa la página en un explorador Web no son muy importantes, cuando se usa un dispositivo móvil resulta más evidentes en las diferencias.</span><span class="sxs-lookup"><span data-stu-id="15966-292">Although you will notice that the changes when the page is rendered in a Web browser are not very significant, when using a mobile device the differences become more obvious.</span></span> <span data-ttu-id="15966-293">En el lado izquierdo de la imagen, podemos ver que el estilo personalizado mejora la legibilidad.</span><span class="sxs-lookup"><span data-stu-id="15966-293">On the left side of the image, we can see that the custom style improved the readability.</span></span>

    <span data-ttu-id="15966-294">Representación adaptable puede usarse en muchos escenarios más, lo que sea más fácil aplicar estilos a un sitio Web y solucionar los problemas comunes de estilo con un enfoque ordenado condicional.</span><span class="sxs-lookup"><span data-stu-id="15966-294">Adaptive rendering can be used in many more scenarios, making it easier to apply conditional styling to a Web site and solving common style issues with a neat approach.</span></span>

    <span data-ttu-id="15966-295">La etiqueta meta de la ventanilla y las consultas de medios CSS no son específicas de ASP.NET MVC 4, por lo que puede aprovechar las ventajas de estas características en cualquier aplicación web.</span><span class="sxs-lookup"><span data-stu-id="15966-295">The Viewport meta tag and CSS media queries are not specific to ASP.NET MVC 4, so you can take advantage of these features in any web application.</span></span>
5. <span data-ttu-id="15966-296">En Visual Studio, presione **MAYÚS** + **F5** para detener la depuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="15966-296">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a><span data-ttu-id="15966-297">Ejercicio 2: Crear la aplicación Web de la Galería fotográfica</span><span class="sxs-lookup"><span data-stu-id="15966-297">Exercise 2: Creating the Photo Gallery Web Application</span></span>

<span data-ttu-id="15966-298">En este ejercicio, trabajará en una aplicación de la Galería fotográfica para mostrar fotografías.</span><span class="sxs-lookup"><span data-stu-id="15966-298">In this exercise, you will work on a Photo Gallery application to display photos.</span></span> <span data-ttu-id="15966-299">Se iniciará con la plantilla de proyecto de ASP.NET MVC 4 y, a continuación, agregará una función para recuperar las fotos de un servicio y mostrarlos en la página principal.</span><span class="sxs-lookup"><span data-stu-id="15966-299">You will start with the ASP.NET MVC 4 project template, and then you will add a feature to retrieve photos from a service and display them in the home page.</span></span>

<span data-ttu-id="15966-300">En el siguiente ejercicio, se actualizará esta solución para mejorar la forma en que se muestra en los dispositivos móviles.</span><span class="sxs-lookup"><span data-stu-id="15966-300">In the following exercise, you will update this solution to enhance the way it is displayed on mobile devices.</span></span>

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a><span data-ttu-id="15966-301">Tarea 1: crear un servicio de fotografía ficticio</span><span class="sxs-lookup"><span data-stu-id="15966-301">Task 1 - Creating a Mock Photo Service</span></span>

<span data-ttu-id="15966-302">En esta tarea, creará un simulacro del servicio para recuperar el contenido que se mostrará en la Galería fotográfica.</span><span class="sxs-lookup"><span data-stu-id="15966-302">In this task, you will create a mock of the photo service to retrieve the content that will be displayed in the gallery.</span></span> <span data-ttu-id="15966-303">Para ello, agregará un controlador nuevo que va a devolver simplemente un archivo JSON con los datos de cada fotografía.</span><span class="sxs-lookup"><span data-stu-id="15966-303">To do this, you will add a new controller that will simply return a JSON file with the data of each photo.</span></span>

1. <span data-ttu-id="15966-304">Abra **Visual Studio** si aún no está abierto.</span><span class="sxs-lookup"><span data-stu-id="15966-304">Open **Visual Studio** if not already opened.</span></span>
2. <span data-ttu-id="15966-305">Seleccione el **archivo | Nuevos | Proyecto** comando de menú.</span><span class="sxs-lookup"><span data-stu-id="15966-305">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="15966-306">En el **nuevo proyecto** cuadro de diálogo, seleccione la **Visual C# | Web** plantilla en el panel izquierdo del árbol y elija **aplicación Web de ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="15966-306">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="15966-307">Denomine el proyecto **PhotoGallery**, seleccione una ubicación (o deje el valor predeterminado) y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="15966-307">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span> <span data-ttu-id="15966-308">Como alternativa, puede seguir trabajando desde la versión existente de ASP.NET MVC 4 **aplicación de Internet** solución de **ejercicio 1** y omitir el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="15966-308">Alternatively, you can continue working from your existing ASP.NET MVC 4 **Internet Application** solution from **Exercise 1** and skip the next step.</span></span>
3. <span data-ttu-id="15966-309">En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione la **aplicación de Internet** plantilla de proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="15966-309">In the **New ASP.NET MVC 4 Project** dialog box, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="15966-310">Asegúrese de que tiene seleccionado como el motor de vistas de Razor.</span><span class="sxs-lookup"><span data-stu-id="15966-310">Make sure you have Razor selected as the View Engine.</span></span>
4. <span data-ttu-id="15966-311">En el **el Explorador de soluciones**, haga clic en el **aplicación\_datos** carpeta del proyecto y seleccione **agregar | Elemento existente**.</span><span class="sxs-lookup"><span data-stu-id="15966-311">In the **Solution Explorer**, right-click the **App\_Data** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="15966-312">Vaya a la **Source\Assets\App\_datos** carpeta de este laboratorio y agregue el **Photos.json** archivo.</span><span class="sxs-lookup"><span data-stu-id="15966-312">Browse to the **Source\Assets\App\_Data** folder of this lab and add the **Photos.json** file.</span></span>
5. <span data-ttu-id="15966-313">Crear un nuevo controlador con el nombre **PhotoController**.</span><span class="sxs-lookup"><span data-stu-id="15966-313">Create a new controller with the name **PhotoController**.</span></span> <span data-ttu-id="15966-314">Para ello, haga doble clic en el **controladores** carpeta, vaya a **agregar** y seleccione **controlador.**</span><span class="sxs-lookup"><span data-stu-id="15966-314">To do this, right-click on the **Controllers** folder, go to **Add** and select **Controller.**</span></span> <span data-ttu-id="15966-315">Escriba el nombre de controlador, deje el **controlador MVC vacío** plantilla y haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="15966-315">Complete the controller name, leave the **Empty MVC controller** template and click **Add**.</span></span>

    <span data-ttu-id="15966-316">![Agregar el PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "agregando el PhotoController")</span><span class="sxs-lookup"><span data-stu-id="15966-316">![Adding the PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "Adding the PhotoController")</span></span>

    <span data-ttu-id="15966-317">*Agregar el PhotoController*</span><span class="sxs-lookup"><span data-stu-id="15966-317">*Adding the PhotoController*</span></span>
6. <span data-ttu-id="15966-318">Reemplace el **índice** método con el siguiente **galería** acción y devuelve el contenido del archivo JSON que haya agregado recientemente al proyecto.</span><span class="sxs-lookup"><span data-stu-id="15966-318">Replace the **Index** method with the following **Gallery** action, and return the content from the JSON file you have recently added to the project.</span></span>

    <span data-ttu-id="15966-319">(Código de fragmento de código: *acción de la Galería de ASP.NET MVC 4 laboratorio - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="15966-319">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Gallery Action*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. <span data-ttu-id="15966-320">Presione **F5** para ejecutar la solución y, a continuación, vaya a la dirección URL siguiente para probar el servicio de fotografías simuladas: `http://localhost:[port]/photo/gallery` (el valor de [puerto] depende de puerto actual donde se inicia la aplicación).</span><span class="sxs-lookup"><span data-stu-id="15966-320">Press **F5** to run the solution, and then browse to the following URL in order to test the mocked photo service: `http://localhost:[port]/photo/gallery` (the [port] value depends on the current port where the application was launched).</span></span> <span data-ttu-id="15966-321">La solicitud a esta dirección URL debe recuperar el contenido de la **Photos.json** archivo.</span><span class="sxs-lookup"><span data-stu-id="15966-321">The request to this URL should retrieve the content of the **Photos.json** file.</span></span>

    <span data-ttu-id="15966-322">![Probar el servicio de fotos simuladas](whats-new-in-aspnet-mvc-4/_static/image20.png "probar el servicio de fotos simuladas")</span><span class="sxs-lookup"><span data-stu-id="15966-322">![Testing the mocked photo service](whats-new-in-aspnet-mvc-4/_static/image20.png "Testing the mocked photo service")</span></span>

    <span data-ttu-id="15966-323">*Probar el servicio de fotos simuladas*</span><span class="sxs-lookup"><span data-stu-id="15966-323">*Testing the mocked photo service*</span></span>

<span data-ttu-id="15966-324">En una implementación real podría utilizar [ASP.NET Web API](../../../../web-api/index.md) para implementar el servicio de la Galería fotográfica.</span><span class="sxs-lookup"><span data-stu-id="15966-324">In a real implementation you could use [ASP.NET Web API](../../../../web-api/index.md) to implement the Photo Gallery service.</span></span> <span data-ttu-id="15966-325">ASP.NET Web API es un marco que facilita la creación de servicios HTTP que llegan a una amplia gama de clientes, incluidos los exploradores y dispositivos móviles.</span><span class="sxs-lookup"><span data-stu-id="15966-325">ASP.NET Web API is a framework that makes it easy to build HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="15966-326">ASP.NET Web API es una plataforma ideal para compilar aplicaciones de RESTful en .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="15966-326">ASP.NET Web API is an ideal platform for building RESTful applications on the .NET Framework.</span></span>

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a><span data-ttu-id="15966-327">Tarea 2: mostrar la Galería fotográfica</span><span class="sxs-lookup"><span data-stu-id="15966-327">Task 2 - Displaying the Photo Gallery</span></span>

<span data-ttu-id="15966-328">En esta tarea, actualizará la página de inicio para mostrar la Galería fotográfica mediante el servicio ficticios que creó en la primera tarea de este ejercicio.</span><span class="sxs-lookup"><span data-stu-id="15966-328">In this task, you will update the Home page to show the photo gallery by using the mocked service you created in the first task of this exercise.</span></span> <span data-ttu-id="15966-329">Agregará los archivos de modelo y actualizar las vistas de la galería.</span><span class="sxs-lookup"><span data-stu-id="15966-329">You will add model files and update the gallery views.</span></span>

1. <span data-ttu-id="15966-330">En Visual Studio, presione **MAYÚS** + **F5** para detener la depuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="15966-330">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="15966-331">Crear el **fotográfica** clase en el **modelos** carpeta.</span><span class="sxs-lookup"><span data-stu-id="15966-331">Create the **Photo** class in the **Models** folder.</span></span> <span data-ttu-id="15966-332">Para ello, haga doble clic en el **modelos** carpeta, seleccione **agregar** y haga clic en **clase**.</span><span class="sxs-lookup"><span data-stu-id="15966-332">To do this, right-click on the **Models** folder, select **Add** and click **Class**.</span></span> <span data-ttu-id="15966-333">A continuación, establezca el nombre **Photo.cs** y haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="15966-333">Then, set the name to **Photo.cs** and click **Add**.</span></span>
3. <span data-ttu-id="15966-334">Agregue los siguientes miembros a la **fotográfica** clase.</span><span class="sxs-lookup"><span data-stu-id="15966-334">Add the following members to the **Photo** class.</span></span>

    <span data-ttu-id="15966-335">(Código de fragmento de código: *modelo foto de ASP.NET MVC 4 laboratorio - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="15966-335">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo model*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. <span data-ttu-id="15966-336">Abra la **HomeController.cs** de archivos desde el **controladores** carpeta.</span><span class="sxs-lookup"><span data-stu-id="15966-336">Open the **HomeController.cs** file from the **Controllers** folder.</span></span>
5. <span data-ttu-id="15966-337">Agregue las siguientes instrucciones de uso.</span><span class="sxs-lookup"><span data-stu-id="15966-337">Add the following using statements.</span></span>

    <span data-ttu-id="15966-338">(Código de fragmento de código: *HomeController usos de ASP.NET MVC 4 laboratorio - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="15966-338">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - HomeController Usings*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. <span data-ttu-id="15966-339">Actualización del **índice** acción que se utilizará **HttpClient** para recuperar los datos de la galería y, a continuación, usar el **JavaScriptSerializer** para deserializar en el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="15966-339">Update the **Index** action to use **HttpClient** to retrieve the gallery data, and then use the **JavaScriptSerializer** to deserialize it to the view model.</span></span>

    <span data-ttu-id="15966-340">(Código de fragmento de código: *acción del índice de ASP.NET MVC 4 laboratorio - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="15966-340">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Index Action*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. <span data-ttu-id="15966-341">Abra la **Index.cshtml** archivo situado bajo el **Views\Home** carpeta y reemplace todo el contenido con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="15966-341">Open the **Index.cshtml** file located under the **Views\Home** folder and replace all the content with the following code.</span></span>

    <span data-ttu-id="15966-342">Este código recorre en bucle todas las fotos que se recupera del servicio y las muestra en una lista sin ordenar.</span><span class="sxs-lookup"><span data-stu-id="15966-342">This code loops through all the photos retrieved from the service and displays them into an unordered list.</span></span>

    <span data-ttu-id="15966-343">(Código de fragmento de código: *lista de fotos de ASP.NET MVC 4 laboratorio - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="15966-343">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo List*)</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. <span data-ttu-id="15966-344">En el **el Explorador de soluciones**, haga clic en el **contenido** carpeta del proyecto y seleccione **agregar | Elemento existente**.</span><span class="sxs-lookup"><span data-stu-id="15966-344">In the **Solution Explorer**, right-click the **Content** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="15966-345">Vaya a la **Source\Assets\Content** carpeta de este laboratorio y agregue el **Site.css** archivo.</span><span class="sxs-lookup"><span data-stu-id="15966-345">Browse to the **Source\Assets\Content** folder of this lab and add the **Site.css** file.</span></span> <span data-ttu-id="15966-346">Tendrá que confirmar su reemplazo.</span><span class="sxs-lookup"><span data-stu-id="15966-346">You will have to confirm its replacement.</span></span> <span data-ttu-id="15966-347">Si tiene la **Site.css** abierto el archivo, tendrá que confirmar para volver a cargar el archivo también.</span><span class="sxs-lookup"><span data-stu-id="15966-347">If you have the **Site.css** file open, you will have to confirm to reload the file also.</span></span>
9. <span data-ttu-id="15966-348">Abra el Explorador de archivos y copiar todo el **fotos** carpeta se encuentra en la **Source\Assets** carpeta de este laboratorio a la carpeta raíz del proyecto en el Explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="15966-348">Open File Explorer and copy the entire **Photos** folder located under the **Source\Assets** folder of this lab to the root folder of your project in Solution Explorer.</span></span>
10. <span data-ttu-id="15966-349">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="15966-349">Run the application.</span></span> <span data-ttu-id="15966-350">Ahora debería ver la página principal muestra las fotos en la galería.</span><span class="sxs-lookup"><span data-stu-id="15966-350">You should now see the home page displaying the photos in the gallery.</span></span>

    <span data-ttu-id="15966-351">![La Galería fotográfica](whats-new-in-aspnet-mvc-4/_static/image21.png "la Galería fotográfica")</span><span class="sxs-lookup"><span data-stu-id="15966-351">![Photo Gallery](whats-new-in-aspnet-mvc-4/_static/image21.png "Photo Gallery")</span></span>

    <span data-ttu-id="15966-352">*La Galería fotográfica*</span><span class="sxs-lookup"><span data-stu-id="15966-352">*Photo Gallery*</span></span>
11. <span data-ttu-id="15966-353">En Visual Studio, presione **MAYÚS** + **F5** para detener la depuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="15966-353">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a><span data-ttu-id="15966-354">Ejercicio 3: Agregar compatibilidad con dispositivos móviles</span><span class="sxs-lookup"><span data-stu-id="15966-354">Exercise 3: Adding support for mobile devices</span></span>

<span data-ttu-id="15966-355">Una de las actualizaciones de claves en ASP.NET MVC 4 es la compatibilidad para el desarrollo móvil.</span><span class="sxs-lookup"><span data-stu-id="15966-355">One of the key updates in ASP.NET MVC 4 is the support for mobile development.</span></span> <span data-ttu-id="15966-356">En este ejercicio, explorará las nuevas características de ASP.NET MVC 4 para aplicaciones móviles mediante la extensión de la solución de la Galería fotográfica que ha creado en el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="15966-356">In this exercise, you will explore ASP.NET MVC 4 new features for mobile applications by extending the PhotoGallery solution you have created in the previous exercise.</span></span>

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a><span data-ttu-id="15966-357">Tarea 1: instalar jQuery Mobile en una aplicación de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="15966-357">Task 1 - Installing jQuery Mobile in an ASP.NET MVC 4 Application</span></span>

1. <span data-ttu-id="15966-358">Abra la **comenzar** solución ubicado en **origen/Ex3-MobileSupport/Begin/** carpeta.</span><span class="sxs-lookup"><span data-stu-id="15966-358">Open the **Begin** solution located at **Source/Ex3-MobileSupport/Begin/** folder.</span></span> <span data-ttu-id="15966-359">En caso contrario, puede seguir usando el **final** solución obtenido siguiendo el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="15966-359">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="15966-360">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="15966-360">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="15966-361">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="15966-361">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="15966-362">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="15966-362">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="15966-363">Por último, compile la solución haciendo clic en **generar** | **generar solución**.</span><span class="sxs-lookup"><span data-stu-id="15966-363">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="15966-364">Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="15966-364">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="15966-365">Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="15966-365">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="15966-366">Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="15966-366">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="15966-367">Abra la **Package Manager Console** haciendo clic en el **herramientas** &gt; **Administrador de paquetes de biblioteca** &gt; **el Administrador de paquetes Consola** opción de menú.</span><span class="sxs-lookup"><span data-stu-id="15966-367">Open the **Package Manager Console** by clicking the **Tools** &gt; **Library Package Manager** &gt; **Package Manager Console** menu option.</span></span>

    <span data-ttu-id="15966-368">![Abra la consola de administrador de paquetes de NuGet](whats-new-in-aspnet-mvc-4/_static/image22.png "abrir la consola de administrador de paquetes de NuGet")</span><span class="sxs-lookup"><span data-stu-id="15966-368">![Opening the NuGet Package Manager Console](whats-new-in-aspnet-mvc-4/_static/image22.png "Opening the NuGet Package Manager Console")</span></span>

    <span data-ttu-id="15966-369">*Abra la consola de administrador de paquetes de NuGet*</span><span class="sxs-lookup"><span data-stu-id="15966-369">*Opening the NuGet Package Manager Console*</span></span>
3. <span data-ttu-id="15966-370">En la consola de administrador de paquetes, ejecute el siguiente comando para instalar el **jQuery.Mobile.MVC** paquete.</span><span class="sxs-lookup"><span data-stu-id="15966-370">In the Package Manager Console run the following command to install the **jQuery.Mobile.MVC** package.</span></span>

    <span data-ttu-id="15966-371">jQuery Mobile es una biblioteca de código abierto para la creación de la interfaz de usuario web táctil optimizada.</span><span class="sxs-lookup"><span data-stu-id="15966-371">jQuery Mobile is an open source library for building touch-optimized web UI.</span></span> <span data-ttu-id="15966-372">El paquete de NuGet jQuery.Mobile.MVC incluye aplicaciones auxiliares para usar jQuery Mobile con una aplicación de ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="15966-372">The jQuery.Mobile.MVC NuGet package includes helpers to use jQuery Mobile with an ASP.NET MVC 4 application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="15966-373">Al ejecutar el comando siguiente, se descarga la biblioteca jQuery.Mobile.MVC de Nuget.</span><span class="sxs-lookup"><span data-stu-id="15966-373">By running the following command, you will be downloading the jQuery.Mobile.MVC library from Nuget.</span></span>

    <span data-ttu-id="15966-374">PM</span><span class="sxs-lookup"><span data-stu-id="15966-374">PM</span></span>

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    <span data-ttu-id="15966-375">Este comando instala jQuery Mobile y algunos archivos de aplicación auxiliar, incluido lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="15966-375">This command installs jQuery Mobile and some helper files, including the following:</span></span>

    - <span data-ttu-id="15966-376">**Vistas/compartida/\_Layout.Mobile.cshtml**: es un diseño basado en Mobile de jQuery optimizado para una pantalla más pequeña.</span><span class="sxs-lookup"><span data-stu-id="15966-376">**Views/Shared/\_Layout.Mobile.cshtml**: is a jQuery Mobile-based layout optimized for a smaller screen.</span></span> <span data-ttu-id="15966-377">Cuando el sitio Web recibe una solicitud de un explorador móvil, reemplazará el diseño original (\_Layout.cshtml) con éste.</span><span class="sxs-lookup"><span data-stu-id="15966-377">When the website receives a request from a mobile browser, it will replace the original layout (\_Layout.cshtml) with this one.</span></span>
    - <span data-ttu-id="15966-378">Un componente de modificador de vista: consta de los **vistas/compartida/\_ViewSwitcher.cshtml** vista parcial y la **ViewSwitcherController.cs** controlador.</span><span class="sxs-lookup"><span data-stu-id="15966-378">A view-switcher component: consists of the **Views/Shared/\_ViewSwitcher.cshtml** partial view and the **ViewSwitcherController.cs** controller.</span></span> <span data-ttu-id="15966-379">Este componente mostrará un vínculo en exploradores móviles para permitir a los usuarios cambiar a la versión de escritorio de la página.</span><span class="sxs-lookup"><span data-stu-id="15966-379">This component will show a link on mobile browsers to enable users to switch to the desktop version of the page.</span></span>  
        <span data-ttu-id="15966-380">![Proyecto de la Galería fotográfica con compatibilidad con dispositivos móvil](whats-new-in-aspnet-mvc-4/_static/image23.png "proyecto de la Galería fotográfica con compatibilidad con dispositivos móvil")</span><span class="sxs-lookup"><span data-stu-id="15966-380">![Photo Gallery project with mobile support](whats-new-in-aspnet-mvc-4/_static/image23.png "Photo Gallery project with mobile support")</span></span>

        <span data-ttu-id="15966-381">*Proyecto de la Galería fotográfica con compatibilidad con dispositivos móvil*</span><span class="sxs-lookup"><span data-stu-id="15966-381">*Photo Gallery project with mobile support*</span></span>
4. <span data-ttu-id="15966-382">Registrar las agrupaciones móviles.</span><span class="sxs-lookup"><span data-stu-id="15966-382">Register the Mobile bundles.</span></span> <span data-ttu-id="15966-383">Para ello, abra el **Global.asax.cs** de archivos y agregue la siguiente línea.</span><span class="sxs-lookup"><span data-stu-id="15966-383">To do this, open the **Global.asax.cs** file and add the following line.</span></span>

    <span data-ttu-id="15966-384">(Código de fragmento de código: *agrupaciones móviles de ASP.NET MVC 4 laboratorio - Ex03 - Register*)</span><span class="sxs-lookup"><span data-stu-id="15966-384">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - Register Mobile Bundles*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. <span data-ttu-id="15966-385">Ejecute la aplicación mediante un explorador web de escritorio.</span><span class="sxs-lookup"><span data-stu-id="15966-385">Run the application using a desktop web browser.</span></span>
6. <span data-ttu-id="15966-386">Abra la **emulador de Windows Phone 7,** ubicado en **menú Inicio | Todos los programas | Windows Phone SDK 7.1 | Emulador de Windows Phone.**</span><span class="sxs-lookup"><span data-stu-id="15966-386">Open the **Windows Phone 7 Emulator,** located in **Start Menu | All Programs | Windows Phone SDK 7.1 | Windows Phone Emulator.**</span></span>
7. <span data-ttu-id="15966-387">En la pantalla de inicio de teléfono, abra Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="15966-387">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="15966-388">Visite la dirección URL donde se inicia la aplicación y vaya a esa dirección URL con el Explorador de teléfono (p. ej. `http://localhost:[PortNumber]/`).</span><span class="sxs-lookup"><span data-stu-id="15966-388">Check out the URL where the application started and browse to that URL with the phone browser (e.g. `http://localhost:[PortNumber]/`).</span></span>

    <span data-ttu-id="15966-389">Observará que la aplicación tendrá un aspecto distinta en el emulador de Windows Phone, como el jQuery.Mobile.MVC ha creado nuevos activos del proyecto que muestren vistas optimizadas para dispositivos móviles.</span><span class="sxs-lookup"><span data-stu-id="15966-389">You will notice that your application will look different in the Windows Phone emulator, as the jQuery.Mobile.MVC has created new assets in your project that show views optimized for mobile devices.</span></span>

    <span data-ttu-id="15966-390">Observe el mensaje en la parte superior del teléfono, que muestra el vínculo que se activa en la vista de escritorio.</span><span class="sxs-lookup"><span data-stu-id="15966-390">Notice the message at the top of the phone, showing the link that switches to the Desktop view.</span></span> <span data-ttu-id="15966-391">Además, el  **\_Layout.Mobile.cshtml** diseño que se creó el paquete ha instalado incluyen un diseño diferente en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="15966-391">Additionally, the **\_Layout.Mobile.cshtml** layout that was created by the package you have installed is including a different layout in the application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="15966-392">Hasta ahora, no hay ningún vínculo para volver a la vista móvil.</span><span class="sxs-lookup"><span data-stu-id="15966-392">So far, there is no link to get back to mobile view.</span></span> <span data-ttu-id="15966-393">Se incluirá en versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="15966-393">It will be included in later versions.</span></span>

    <span data-ttu-id="15966-394">![Vista móvil de la página principal de la Galería fotográfica](whats-new-in-aspnet-mvc-4/_static/image24.png "vista móvil de la página principal de la Galería fotográfica")</span><span class="sxs-lookup"><span data-stu-id="15966-394">![Mobile view of the Photo Gallery Home page](whats-new-in-aspnet-mvc-4/_static/image24.png "Mobile view of the Photo Gallery Home page")</span></span>

    <span data-ttu-id="15966-395">*Vista móvil de la página principal de la Galería fotográfica*</span><span class="sxs-lookup"><span data-stu-id="15966-395">*Mobile view of the Photo Gallery Home page*</span></span>
8. <span data-ttu-id="15966-396">En Visual Studio, presione **MAYÚS** + **F5** para detener la depuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="15966-396">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a><span data-ttu-id="15966-397">Tarea 2: creación de vistas móviles</span><span class="sxs-lookup"><span data-stu-id="15966-397">Task 2 - Creating Mobile Views</span></span>

<span data-ttu-id="15966-398">En esta tarea, creará una versión móvil de la vista de índice con contenido adaptado para appareance mejor en dispositivos móviles.</span><span class="sxs-lookup"><span data-stu-id="15966-398">In this task, you will create a mobile version of the index view with content adapted for better appareance in mobile devices.</span></span>

1. <span data-ttu-id="15966-399">Copia la **Views\Home\Index.cshtml** ver y péguelo para crear una copia, cambie el nombre del nuevo archivo a **Index.Mobile.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="15966-399">Copy the **Views\Home\Index.cshtml** view and paste it to create a copy, rename the new file to **Index.Mobile.cshtml**.</span></span>
2. <span data-ttu-id="15966-400">Abrir la nueva creada **Index.Mobile.cshtml** ver y reemplazar a las &lt;ul&gt; etiqueta con este código.</span><span class="sxs-lookup"><span data-stu-id="15966-400">Open the new created **Index.Mobile.cshtml** view and replace the existing &lt;ul&gt; tag with this code.</span></span> <span data-ttu-id="15966-401">Al hacerlo, actualizará el &lt;ul&gt; etiqueta con las anotaciones de datos móviles de jQuery que se va a utilizar los temas de jQuery mobile.</span><span class="sxs-lookup"><span data-stu-id="15966-401">By doing this, you will be updating the &lt;ul&gt; tag with jQuery Mobile data annotations to use the mobile themes from jQuery.</span></span>


    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > <span data-ttu-id="15966-402">Tenga en cuenta que:</span><span class="sxs-lookup"><span data-stu-id="15966-402">Notice that:</span></span>
    > 
    > - <span data-ttu-id="15966-403">El **datos rol** atributo establecido en **listview** se representarán la lista con los estilos de listview.</span><span class="sxs-lookup"><span data-stu-id="15966-403">The **data-role** attribute set to **listview** will render the list using the listview styles.</span></span>
    > 
    > - <span data-ttu-id="15966-404">El **datos bajorrelieve** atributo establecido en true mostrará la lista con borde redondeado y margen.</span><span class="sxs-lookup"><span data-stu-id="15966-404">The **data-inset** attribute set to true will show the list with rounded border and margin.</span></span>
    > 
    > - <span data-ttu-id="15966-405">El **filtro de datos** atributo establecido en **true** generará un cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="15966-405">The **data-filter** attribute set to **true** will generate a search box.</span></span>
    > 
    > <span data-ttu-id="15966-406">Puede aprender más acerca de las convenciones de móviles jQuery en la documentación del proyecto: [ [http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span><span class="sxs-lookup"><span data-stu-id="15966-406">You can learn more about jQuery Mobile conventions in the project documentation: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span></span>
3. <span data-ttu-id="15966-407">Presione **CTRL + S** para guardar los cambios.</span><span class="sxs-lookup"><span data-stu-id="15966-407">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="15966-408">Cambie a la **emulador de Windows Phone** y actualizar el sitio.</span><span class="sxs-lookup"><span data-stu-id="15966-408">Switch to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="15966-409">Tenga en cuenta la nueva apariencia y funcionamiento de la lista de la galería, así como el nuevo cuadro de búsqueda que se encuentra en la parte superior.</span><span class="sxs-lookup"><span data-stu-id="15966-409">Notice the new look and feel of the gallery list, as well as the new search box located on the top.</span></span> <span data-ttu-id="15966-410">A continuación, escriba una palabra en el cuadro de búsqueda (por ejemplo, **Tulips**) para iniciar una búsqueda en la Galería fotográfica.</span><span class="sxs-lookup"><span data-stu-id="15966-410">Then, type a word in the search box (for instance, **Tulips**) to start a search in the photo gallery.</span></span>

    <span data-ttu-id="15966-411">![Galería con estilo de listview con filtrado](whats-new-in-aspnet-mvc-4/_static/image25.png "utilizando el estilo de listview con el filtrado de la Galería")</span><span class="sxs-lookup"><span data-stu-id="15966-411">![Gallery using listview style with filtering](whats-new-in-aspnet-mvc-4/_static/image25.png "Gallery using listview style with filtering")</span></span>

    <span data-ttu-id="15966-412">*Utilizando el estilo de listview con el filtrado de la Galería*</span><span class="sxs-lookup"><span data-stu-id="15966-412">*Gallery using listview style with filtering*</span></span>

    <span data-ttu-id="15966-413">En resumen, ha utilizado la receta Mobilizer de vista para crear una copia de la vista de índice con la &quot;móvil&quot; sufijo.</span><span class="sxs-lookup"><span data-stu-id="15966-413">To summarize, you have used the View Mobilizer recipe to create a copy of the Index view with the &quot;mobile&quot; suffix.</span></span> <span data-ttu-id="15966-414">Este sufijo indica a ASP.NET MVC 4 que cada solicitud que se genera a partir de un dispositivo móvil utilizará esta copia del índice.</span><span class="sxs-lookup"><span data-stu-id="15966-414">This suffix indicates to ASP.NET MVC 4 that every request generated from a mobile device will use this copy of the index.</span></span> <span data-ttu-id="15966-415">Además, se actualizó la versión móvil de la vista de índice para usar jQuery Mobile para mejorar la apariencia y funcionamiento de sitio en los dispositivos móviles.</span><span class="sxs-lookup"><span data-stu-id="15966-415">Additionally, you have updated the mobile version of the Index view to use jQuery Mobile for enhancing the site look and feel in mobile devices.</span></span>
5. <span data-ttu-id="15966-416">Vuelva a Visual Studio y abra **Site.Mobile.css** situado bajo el **contenido** carpeta.</span><span class="sxs-lookup"><span data-stu-id="15966-416">Go back to Visual Studio and open **Site.Mobile.css** located under the **Content** folder.</span></span>
6. <span data-ttu-id="15966-417">Corrija la posición del título fotográfica para que aparezca en el lado derecho de la imagen.</span><span class="sxs-lookup"><span data-stu-id="15966-417">Fix the positioning of the photo title to make it show at the right side of the image.</span></span> <span data-ttu-id="15966-418">Para ello, agregue el código siguiente a la **Site.Mobile.css** archivo.</span><span class="sxs-lookup"><span data-stu-id="15966-418">To do this, add the following code to the **Site.Mobile.css** file.</span></span>

    <span data-ttu-id="15966-419">CSS</span><span class="sxs-lookup"><span data-stu-id="15966-419">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. <span data-ttu-id="15966-420">Presione **CTRL + S** para guardar los cambios.</span><span class="sxs-lookup"><span data-stu-id="15966-420">Press **CTRL + S** to save the changes.</span></span>
8. <span data-ttu-id="15966-421">Vuelva a la **emulador de Windows Phone** y actualizar el sitio.</span><span class="sxs-lookup"><span data-stu-id="15966-421">Switch back to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="15966-422">Tenga en cuenta que el título de la fotografía está colocado correctamente ahora.</span><span class="sxs-lookup"><span data-stu-id="15966-422">Notice the photo title is properly positioned now.</span></span>

    <span data-ttu-id="15966-423">![Título que se coloca en el lado derecho de la imagen](whats-new-in-aspnet-mvc-4/_static/image26.png "título situado en el lado derecho de la imagen")</span><span class="sxs-lookup"><span data-stu-id="15966-423">![Title positioned on the right side of the image](whats-new-in-aspnet-mvc-4/_static/image26.png "Title positioned on the right side of the image")</span></span>

    <span data-ttu-id="15966-424">*Título que se coloca en el lado derecho de la imagen*</span><span class="sxs-lookup"><span data-stu-id="15966-424">*Title positioned on the right side of the image*</span></span>

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a><span data-ttu-id="15966-425">Tarea 3: temas de jQuery Mobile</span><span class="sxs-lookup"><span data-stu-id="15966-425">Task 3 - jQuery Mobile Themes</span></span>

<span data-ttu-id="15966-426">Cada diseño y el widget de jQuery Mobile está diseñado alrededor de un nuevo marco CSS orientada a objetos que permite aplicar un tema de completar el diseño visual unificado a sitios y aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="15966-426">Every layout and widget in jQuery Mobile is designed around a new object-oriented CSS framework that makes it possible to apply a complete unified visual design theme to sites and applications.</span></span>

<span data-ttu-id="15966-427">predeterminado de jQuery Mobile tema incluye 5 muestras que se asignan letras (a, b, c y d, e) para una referencia rápida.</span><span class="sxs-lookup"><span data-stu-id="15966-427">jQuery Mobile's default Theme includes 5 swatches that are given letters (a, b, c, d, e) for quick reference.</span></span>

<span data-ttu-id="15966-428">En esta tarea, actualizará el diseño móvil para que utilice un tema diferente que el valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="15966-428">In this task, you will update the mobile layout to use a different theme than the default.</span></span>

1. <span data-ttu-id="15966-429">Cambie a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="15966-429">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="15966-430">Abra la  **\_Layout.Mobile.cshtml** archivo se encuentra en **Views\Shared**.</span><span class="sxs-lookup"><span data-stu-id="15966-430">Open the **\_Layout.Mobile.cshtml** file located in **Views\Shared**.</span></span>
3. <span data-ttu-id="15966-431">Busque el elemento div con el rol de datos establecido en &quot;página&quot; y actualizar la **datos tema** a &quot; **e**&quot;.</span><span class="sxs-lookup"><span data-stu-id="15966-431">Find the div element with the data-role set to &quot;page&quot; and update the **data-theme** to &quot;**e**&quot;.</span></span>


    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. <span data-ttu-id="15966-432">Presione **CTRL + S** para guardar los cambios.</span><span class="sxs-lookup"><span data-stu-id="15966-432">Press **CTRL + S** to save the changes.</span></span>
5. <span data-ttu-id="15966-433">Actualizar el sitio en el **emulador de Windows Phone** y observe la nueva combinación de colores.</span><span class="sxs-lookup"><span data-stu-id="15966-433">Refresh the site in the **Windows Phone Emulator** and notice the new colors scheme.</span></span>

    <span data-ttu-id="15966-434">![Diseño móvil con una combinación de colores diferentes](whats-new-in-aspnet-mvc-4/_static/image27.png "diseño móvil con una combinación de colores diferentes")</span><span class="sxs-lookup"><span data-stu-id="15966-434">![Mobile layout with a different color scheme](whats-new-in-aspnet-mvc-4/_static/image27.png "Mobile layout with a different color scheme")</span></span>

    <span data-ttu-id="15966-435">*Diseño móvil con una combinación de colores diferentes*</span><span class="sxs-lookup"><span data-stu-id="15966-435">*Mobile layout with a different color scheme*</span></span>

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a><span data-ttu-id="15966-436">Tarea 4: mediante el componente de modificador de vista y el explorador reemplazar características</span><span class="sxs-lookup"><span data-stu-id="15966-436">Task 4 - Using the View-Switcher Component and the Browser Overriding Features</span></span>

<span data-ttu-id="15966-437">Es una convención para las páginas de web móvil optimizada agregar un vínculo cuyo texto está algo como vista de escritorio o en modo de sitio completo que permite a los usuarios cambiar a una versión de escritorio de la página.</span><span class="sxs-lookup"><span data-stu-id="15966-437">A convention for mobile-optimized web pages is to add a link whose text is something like Desktop view or Full site mode that lets users switch to a desktop version of the page.</span></span> <span data-ttu-id="15966-438">El paquete jQuery.Mobile.MVC incluye un ejemplo de **modificador de vista** componente para este propósito que se utiliza en el  **\_Layout.Mobile.cshtml** vista.</span><span class="sxs-lookup"><span data-stu-id="15966-438">The jQuery.Mobile.MVC package includes a sample **view-switcher** component for this purpose used in the **\_Layout.Mobile.cshtml** view.</span></span>

<span data-ttu-id="15966-439">![Vínculo para cambiar a vista de escritorio](whats-new-in-aspnet-mvc-4/_static/image28.png "vínculo para cambiar a vista de escritorio")</span><span class="sxs-lookup"><span data-stu-id="15966-439">![Link to switch to Desktop View](whats-new-in-aspnet-mvc-4/_static/image28.png "Link to switch to Desktop View")</span></span>

<span data-ttu-id="15966-440">*Use el vínculo para cambiar a la vista de escritorio*</span><span class="sxs-lookup"><span data-stu-id="15966-440">*Link to switch to Desktop View*</span></span>

<span data-ttu-id="15966-441">El modificador de vista utiliza una nueva característica denominada **explorador reemplazar**.</span><span class="sxs-lookup"><span data-stu-id="15966-441">The view switcher uses a new feature called **Browser Overriding**.</span></span> <span data-ttu-id="15966-442">Esta característica permite que la aplicación trate las solicitudes como si procediesen de un explorador distinto (agente de usuario) a la que realmente proceden del.</span><span class="sxs-lookup"><span data-stu-id="15966-442">This feature lets your application treat requests as if they were coming from a different browser (user agent) than the one they are actually coming from.</span></span>

<span data-ttu-id="15966-443">En esta tarea, explorará la implementación del ejemplo de un modificador de vista agregada por jQuery.Mobile.MVC y el nuevo explorador reemplazar las características de ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="15966-443">In this task, you will explore the sample implementation of a view-switcher added by jQuery.Mobile.MVC and the new browser overriding features in ASP.NET MVC 4.</span></span>

1. <span data-ttu-id="15966-444">Cambie a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="15966-444">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="15966-445">Abra la  **\_Layout.Mobile.cshtml** vista se encuentra en la **Views\Shared** carpeta y observe el componente de modificador de vista que se hace referencia como una vista parcial.</span><span class="sxs-lookup"><span data-stu-id="15966-445">Open the **\_Layout.Mobile.cshtml** view located under the **Views\Shared** folder and notice the view-switcher component being referenced as a partial view.</span></span>

    <span data-ttu-id="15966-446">![Diseño móvil mediante el componente de modificador de vista](whats-new-in-aspnet-mvc-4/_static/image29.png "diseño móvil mediante el componente de modificador de vista")</span><span class="sxs-lookup"><span data-stu-id="15966-446">![Mobile layout using View-Switcher component](whats-new-in-aspnet-mvc-4/_static/image29.png "Mobile layout using View-Switcher component")</span></span>

    <span data-ttu-id="15966-447">*Diseño móvil mediante el componente de modificador de vista*</span><span class="sxs-lookup"><span data-stu-id="15966-447">*Mobile layout using View-Switcher component*</span></span>
3. <span data-ttu-id="15966-448">Abra la  **\_ViewSwitcher.cshtml** vista parcial.</span><span class="sxs-lookup"><span data-stu-id="15966-448">Open the **\_ViewSwitcher.cshtml** partial view.</span></span>

    <span data-ttu-id="15966-449">La vista parcial utiliza el nuevo método **ViewContext.HttpContext.GetOverriddenBrowser()** para determinar el origen de la solicitud web y mostrar el vínculo correspondiente para pasar a las vistas de escritorio o móvil.</span><span class="sxs-lookup"><span data-stu-id="15966-449">The partial view uses the new method **ViewContext.HttpContext.GetOverriddenBrowser()** to determine the origin of the web request and show the corresponding link to switch either to the Desktop or Mobile views.</span></span>

    <span data-ttu-id="15966-450">El **GetOverridenBrowser** método devuelve un **HttpBrowserCapabilitiesBase** instancia que se corresponde con el agente de usuario establecido actualmente para la solicitud (real o reemplazada).</span><span class="sxs-lookup"><span data-stu-id="15966-450">The **GetOverridenBrowser** method returns an **HttpBrowserCapabilitiesBase** instance that corresponds to the user agent currently set for the request (actual or overridden).</span></span> <span data-ttu-id="15966-451">Puede usar este valor para obtener propiedades como **IsMobileDevice**.</span><span class="sxs-lookup"><span data-stu-id="15966-451">You can use this value to get properties such as **IsMobileDevice**.</span></span>

    <span data-ttu-id="15966-452">![Vista parcial ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher de vista parcial")</span><span class="sxs-lookup"><span data-stu-id="15966-452">![ViewSwitcher partial view](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher partial view")</span></span>

    <span data-ttu-id="15966-453">*Vista parcial ViewSwitcher*</span><span class="sxs-lookup"><span data-stu-id="15966-453">*ViewSwitcher partial view*</span></span>
4. <span data-ttu-id="15966-454">Abra la **ViewSwitcherController.cs** clase ubicado en el **controladores** carpeta.</span><span class="sxs-lookup"><span data-stu-id="15966-454">Open the **ViewSwitcherController.cs** class located in the **Controllers** folder.</span></span> <span data-ttu-id="15966-455">Desprotección esa acción SwitchView es llamado por el vínculo en el componente ViewSwitcher y observe los nuevos métodos de HttpContext.</span><span class="sxs-lookup"><span data-stu-id="15966-455">Check out that SwitchView action is called by the link in the ViewSwitcher component, and notice the new HttpContext methods.</span></span>

    - <span data-ttu-id="15966-456">El **HttpContext.ClearOverridenBrowser()** método quita cualquier agente de usuario invalidado para la solicitud actual.</span><span class="sxs-lookup"><span data-stu-id="15966-456">The **HttpContext.ClearOverridenBrowser()** method removes any overridden user agent for the current request.</span></span>
    - <span data-ttu-id="15966-457">El **HttpContext.SetOverridenBrowser()** método invalida el valor de agente de usuario real de la solicitud mediante el agente de usuario especificado.</span><span class="sxs-lookup"><span data-stu-id="15966-457">The **HttpContext.SetOverridenBrowser()** method overrides the request's actual user agent value using the specified user agent.</span></span>  
        <span data-ttu-id="15966-458">![Controlador de ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher controlador")</span><span class="sxs-lookup"><span data-stu-id="15966-458">![ViewSwitcher Controller](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher Controller")</span></span>  
<span data-ttu-id="15966-459">*Controlador ViewSwitcher*</span><span class="sxs-lookup"><span data-stu-id="15966-459">*ViewSwitcher Controller*</span></span>

        <span data-ttu-id="15966-460">Reemplazando explorador es una característica fundamental de ASP.NET MVC 4, que también está disponible incluso si no instala el paquete jQuery.Mobile.MVC.</span><span class="sxs-lookup"><span data-stu-id="15966-460">Browser Overriding is a core feature of ASP.NET MVC 4, which is also available even if you do not install the jQuery.Mobile.MVC package.</span></span> <span data-ttu-id="15966-461">Sin embargo, esta característica puede afectar a solo vista, diseño y vista parcial y no afecta a ninguna de las características que dependen del objeto Request.Browser.</span><span class="sxs-lookup"><span data-stu-id="15966-461">However, this feature affects only view, layout, and partial-view, and it does not affect any of the features that depend on the Request.Browser object.</span></span>

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a><span data-ttu-id="15966-462">Tarea 5: agregar al modificador de vista en la vista de escritorio</span><span class="sxs-lookup"><span data-stu-id="15966-462">Task 5 - Adding the View-Switcher in the Desktop View</span></span>

<span data-ttu-id="15966-463">En esta tarea, actualizará el diseño del escritorio para que incluya al modificador de vista.</span><span class="sxs-lookup"><span data-stu-id="15966-463">In this task, you will update the desktop layout to include the view-switcher.</span></span> <span data-ttu-id="15966-464">Esto permitirá a los usuarios móviles volver a la vista móvil al examinar la vista de escritorio.</span><span class="sxs-lookup"><span data-stu-id="15966-464">This will allow mobile users to go back to the mobile view when browsing the desktop view.</span></span>

1. <span data-ttu-id="15966-465">Actualizar el sitio en el **emulador de Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="15966-465">Refresh the site in the **Windows Phone Emulator**.</span></span>
2. <span data-ttu-id="15966-466">Haga clic en el **vista de escritorio** vínculo en la parte superior de la galería.</span><span class="sxs-lookup"><span data-stu-id="15966-466">Click on the **Desktop view** link at the top of the gallery.</span></span> <span data-ttu-id="15966-467">Tenga en cuenta que no hay ningún modificador de vista en la vista de escritorio para permitir que volver a la vista móvil.</span><span class="sxs-lookup"><span data-stu-id="15966-467">Notice there is no view-switcher in the desktop view to allow you return to the mobile view.</span></span>
3. <span data-ttu-id="15966-468">Vuelva a Visual Studio y abra el  **\_Layout.cshtml** vista.</span><span class="sxs-lookup"><span data-stu-id="15966-468">Go back to Visual Studio and open the **\_Layout.cshtml** view.</span></span>
4. <span data-ttu-id="15966-469">Busque la sección de inicio de sesión e inserte una llamada para representar la  **\_ViewSwitcher** vista parcial siguiente la  **\_LogOnPartial** vista parcial.</span><span class="sxs-lookup"><span data-stu-id="15966-469">Find the login section and insert a call to render the **\_ViewSwitcher** partial view below the **\_LogOnPartial** partial view.</span></span> <span data-ttu-id="15966-470">A continuación, presione **CTRL + S** para guardar los cambios.</span><span class="sxs-lookup"><span data-stu-id="15966-470">Then, press **CTRL + S** to save the changes.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. <span data-ttu-id="15966-471">Presione **CTRL + S** para guardar los cambios.</span><span class="sxs-lookup"><span data-stu-id="15966-471">Press **CTRL + S** to save the changes.</span></span>
6. <span data-ttu-id="15966-472">Actualice la página en el emulador de Windows Phone y haga doble clic en la pantalla para acercar.</span><span class="sxs-lookup"><span data-stu-id="15966-472">Refresh the page in the Windows Phone Emulator and double-click the screen to zoom in.</span></span> <span data-ttu-id="15966-473">Observe que ahora se muestra en la página principal de la **vista móvil** vínculo que se activa desde dispositivos móviles en vista de escritorio.</span><span class="sxs-lookup"><span data-stu-id="15966-473">Notice that the home page now shows the **Mobile view** link that switches from mobile to desktop view.</span></span>

    <span data-ttu-id="15966-474">![Ver modificador que se representan en la vista de escritorio](whats-new-in-aspnet-mvc-4/_static/image32.png "cambiador de vistas que se representan en la vista de escritorio")</span><span class="sxs-lookup"><span data-stu-id="15966-474">![View Switcher rendered in desktop view](whats-new-in-aspnet-mvc-4/_static/image32.png "View Switcher rendered in desktop view")</span></span>

    <span data-ttu-id="15966-475">*Modificador de vista que se representan en la vista de escritorio*</span><span class="sxs-lookup"><span data-stu-id="15966-475">*View Switcher rendered in desktop view*</span></span>
7. <span data-ttu-id="15966-476">Cambie a la vista móvil nuevo y vaya a **sobre** página (¡http://localhost [puerto] / principal/acerca de).</span><span class="sxs-lookup"><span data-stu-id="15966-476">Switch to the Mobile view again and browse to **About** page (http://localhost[port]/Home/About).</span></span> <span data-ttu-id="15966-477">Tenga en cuenta que, incluso si no ha creado una vista About.Mobile.cshtml, acerca de se muestra la página con el diseño móvil (\_Layout.Mobile.cshtml).</span><span class="sxs-lookup"><span data-stu-id="15966-477">Notice that, even if you haven't created an About.Mobile.cshtml view, the About page is displayed using the mobile layout (\_Layout.Mobile.cshtml).</span></span>

    <span data-ttu-id="15966-478">![Acerca de la página](whats-new-in-aspnet-mvc-4/_static/image33.png "acerca de la página")</span><span class="sxs-lookup"><span data-stu-id="15966-478">![About page](whats-new-in-aspnet-mvc-4/_static/image33.png "About page")</span></span>

    <span data-ttu-id="15966-479">*Acerca de la página*</span><span class="sxs-lookup"><span data-stu-id="15966-479">*About page*</span></span>
8. <span data-ttu-id="15966-480">Por último, abra el sitio en un explorador Web de escritorio.</span><span class="sxs-lookup"><span data-stu-id="15966-480">Finally, open the site in a desktop Web browser.</span></span> <span data-ttu-id="15966-481">Tenga en cuenta que ninguna de las actualizaciones anteriores ha afectado a la vista de escritorio.</span><span class="sxs-lookup"><span data-stu-id="15966-481">Notice that none of the previous updates has affected the desktop view.</span></span>

    <span data-ttu-id="15966-482">![Vista de escritorio de la Galería fotográfica](whats-new-in-aspnet-mvc-4/_static/image34.png "vista de escritorio de la Galería fotográfica")</span><span class="sxs-lookup"><span data-stu-id="15966-482">![PhotoGallery desktop view](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery desktop view")</span></span>

    <span data-ttu-id="15966-483">*Vista de escritorio de la Galería fotográfica*</span><span class="sxs-lookup"><span data-stu-id="15966-483">*PhotoGallery desktop view*</span></span>

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a><span data-ttu-id="15966-484">Tarea 6: crear nuevos modos de presentación</span><span class="sxs-lookup"><span data-stu-id="15966-484">Task 6 - Creating New Display Modes</span></span>

<span data-ttu-id="15966-485">La nueva característica de modos de presentación permite que una aplicación seleccionar vistas dependiendo del explorador que se va a generar la solicitud.</span><span class="sxs-lookup"><span data-stu-id="15966-485">The new display modes feature lets an application select views depending on the browser that is generating the request.</span></span> <span data-ttu-id="15966-486">Por ejemplo, si un explorador de escritorio, solicita la página de inicio, la aplicación devolverá la **Views\Home\Index.cshtml** plantilla.</span><span class="sxs-lookup"><span data-stu-id="15966-486">For example, if a desktop browser requests the Home page, the application will return the **Views\Home\Index.cshtml** template.</span></span> <span data-ttu-id="15966-487">A continuación, si un explorador móvil solicita la página de inicio, la aplicación devolverá la **Views\Home\Index.mobile.cshtml** plantilla.</span><span class="sxs-lookup"><span data-stu-id="15966-487">Then, if a mobile browser requests the Home page, the application will return the **Views\Home\Index.mobile.cshtml** template.</span></span>

<span data-ttu-id="15966-488">En esta tarea, creará un diseño personalizado para los dispositivos iPhone y tendrá que simular las solicitudes de dispositivos iPhone.</span><span class="sxs-lookup"><span data-stu-id="15966-488">In this task, you will create a customized layout for iPhone devices, and you will have to simulate requests from iPhone devices.</span></span> <span data-ttu-id="15966-489">Para ello, puede usar cualquier un iPhone emulador o simulador (como [Electric simulador Mobile](http://www.electricplum.com/)) o un explorador con los complementos que modifican el agente de usuario.</span><span class="sxs-lookup"><span data-stu-id="15966-489">To do this, you can use either an iPhone emulator/simulator (like [Electric Mobile Simulator](http://www.electricplum.com/)) or a browser with add-ons that modify the user agent.</span></span> <span data-ttu-id="15966-490">Para que obtener instrucciones sobre cómo establecer la cadena de agente de usuario en un explorador Safari emular un iPhone, consulte [cómo permiten Safari Finjan es IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) en el blog de David Alison.</span><span class="sxs-lookup"><span data-stu-id="15966-490">For instructions on how to set the user agent string in an Safari browser to emulate an iPhone, see [How to let Safari pretend it's IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) in David Alison's blog.</span></span>

<span data-ttu-id="15966-491">**Tenga en cuenta que esta tarea es opcional y puede continuar a lo largo del laboratorio sin ejecutarla.**</span><span class="sxs-lookup"><span data-stu-id="15966-491">**Notice that this task is optional and you can continue throughout the lab without executing it.**</span></span>

1. <span data-ttu-id="15966-492">En Visual Studio, presione **MAYÚS** + **F5** para detener la depuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="15966-492">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="15966-493">Abra **Global.asax.cs** y agregue la siguiente instrucción using.</span><span class="sxs-lookup"><span data-stu-id="15966-493">Open **Global.asax.cs** and add the following using statement.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. <span data-ttu-id="15966-494">Agregue el código que aparece resaltado en la aplicación\_Start (método).</span><span class="sxs-lookup"><span data-stu-id="15966-494">Add the following highlighted code into the Application\_Start method.</span></span>

    <span data-ttu-id="15966-495">(Código de fragmento de código: *iPhone DisplayMode de ASP.NET MVC 4 laboratorio - Ex03 -*)</span><span class="sxs-lookup"><span data-stu-id="15966-495">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - iPhone DisplayMode*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

    <span data-ttu-id="15966-496">Se ha registrado un nuevo **DefaultDisplayMode denominado &quot;iPhone&quot;**, en el método estático **DisplayModeProvider.Instance.Modes** lista estática, que se comparará con cada solicitud entrante.</span><span class="sxs-lookup"><span data-stu-id="15966-496">You have registered a new **DefaultDisplayMode named &quot;iPhone&quot;**, within the static **DisplayModeProvider.Instance.Modes** static list, that will be matched against each incoming request.</span></span> <span data-ttu-id="15966-497">Si la solicitud entrante contiene la cadena &quot;iPhone&quot;, ASP.NET MVC se detectarán las vistas cuyo nombre contenga la &quot;iPhone&quot; sufijo.</span><span class="sxs-lookup"><span data-stu-id="15966-497">If the incoming request contains the string &quot;iPhone&quot;, ASP.NET MVC will find the views whose name contain the &quot;iPhone&quot; suffix.</span></span> <span data-ttu-id="15966-498">El parámetro 0 indica cómo específico es el nuevo modo; Por ejemplo, esta vista es más específica que la ficha general &quot;.mobile&quot; regla que coincide con las solicitudes de dispositivos móviles.</span><span class="sxs-lookup"><span data-stu-id="15966-498">The 0 parameter indicates how specific is the new mode; for instance, this view is more specific than the general &quot;.mobile&quot; rule that matches requests from mobile devices.</span></span>

    <span data-ttu-id="15966-499">Después de ejecuta este código, cuando un explorador de iPhone genera una solicitud, la aplicación utilizará la **Views\Shared\\_Layout.iPhone.cshtml** diseño que se va a crear en los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="15966-499">After this code runs, when an iPhone browser generates a request, your application will use the **Views\Shared\\_Layout.iPhone.cshtml** layout you will create in the next steps.</span></span>

    > [!NOTE]
    > <span data-ttu-id="15966-500">Esta manera de probar la solicitud para iPhone se ha simplificado para fines de demostración y podría no funcionar según lo previsto para cada cadena de agente de usuario de iPhone (para la prueba de ejemplo distingue mayúsculas de minúsculas).</span><span class="sxs-lookup"><span data-stu-id="15966-500">This way of testing the request for iPhone has been simplified for demo purposes and might not work as expected for every iPhone user agent string (for example test is case sensitive).</span></span>
4. <span data-ttu-id="15966-501">Crear una copia de la  **\_Layout.Mobile.cshtml** un archivo en el **Views\Shared** carpeta y cambiar el nombre de la copia a &quot;  **\_Layout.iPhone.csthml** &quot;.</span><span class="sxs-lookup"><span data-stu-id="15966-501">Create a copy of the **\_Layout.Mobile.cshtml** file in the **Views\Shared** folder and rename the copy to &quot;**\_Layout.iPhone.csthml**&quot;.</span></span>
5. <span data-ttu-id="15966-502">Abra  **\_Layout.iPhone.csthml** que creó en el paso anterior.</span><span class="sxs-lookup"><span data-stu-id="15966-502">Open **\_Layout.iPhone.csthml** you created in the previous step.</span></span>
6. <span data-ttu-id="15966-503">Busque el elemento div con el atributo de rol de datos establecido en **página** y cambie el **datos tema** atribuir a &quot; **una**&quot;.</span><span class="sxs-lookup"><span data-stu-id="15966-503">Find the div element with the data-role attribute set to **page** and change the **data-theme** attribute to &quot;**a**&quot;.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

    <span data-ttu-id="15966-504">Ahora tiene 3 diseños en la aplicación de ASP.NET MVC 4:</span><span class="sxs-lookup"><span data-stu-id="15966-504">Now you have 3 layouts in your ASP.NET MVC 4 application:</span></span>

    1. <span data-ttu-id="15966-505">**\_Layout.cshtml**: diseño predeterminado utilizado para los exploradores de escritorio.</span><span class="sxs-lookup"><span data-stu-id="15966-505">**\_Layout.cshtml**: default layout used for desktop browsers.</span></span>
    2. <span data-ttu-id="15966-506">**\_Layout.Mobile.cshtml**: diseño predeterminado utilizado para dispositivos móviles.</span><span class="sxs-lookup"><span data-stu-id="15966-506">**\_Layout.mobile.cshtml**: default layout used for mobile devices.</span></span>
    3. <span data-ttu-id="15966-507">**\_Layout.iPhone.cshtml**: diseño específico de dispositivos de iPhone, mediante una combinación de colores diferentes para diferenciar de \_Layout.mobile.cshtml.</span><span class="sxs-lookup"><span data-stu-id="15966-507">**\_Layout.iPhone.cshtml**: specific layout for iPhone devices, using a different color scheme to differentiate from \_Layout.mobile.cshtml.</span></span>
7. <span data-ttu-id="15966-508">Presione **F5** para ejecutar la aplicación y explora el sitio en el **emulador de Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="15966-508">Press **F5** to run the application and browse the site in the **Windows Phone Emulator**.</span></span>
8. <span data-ttu-id="15966-509">Abrir un **iPhone simulador** (consulte [Apéndice C](#AppendixC) para obtener instrucciones sobre cómo instalar y configurar un emulador de iPhone) y busque el sitio demasiado.</span><span class="sxs-lookup"><span data-stu-id="15966-509">Open an **iPhone simulator** (see [Appendix C](#AppendixC) for instructions on how to install and configure an iPhone simulator), and browse to the site too.</span></span> <span data-ttu-id="15966-510">Tenga en cuenta que cada teléfono está usando la plantilla específica.</span><span class="sxs-lookup"><span data-stu-id="15966-510">Notice that each phone is using the specific template.</span></span>

    ![Using-Different-Views-for-Each-Mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    <span data-ttu-id="15966-512">*Usar vistas diferentes para cada dispositivo móvil*</span><span class="sxs-lookup"><span data-stu-id="15966-512">*Using different views for each mobile device*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a><span data-ttu-id="15966-513">Ejercicio 4: Usar controladores asincrónicos</span><span class="sxs-lookup"><span data-stu-id="15966-513">Exercise 4: Using Asynchronous Controllers</span></span>

<span data-ttu-id="15966-514">Microsoft .NET Framework 4.5 presenta nuevas características del lenguaje en C# y Visual Basic para proporcionar una nueva base de asincronía en programación de. NET.</span><span class="sxs-lookup"><span data-stu-id="15966-514">Microsoft .NET Framework 4.5 introduces new language features in C# and Visual Basic to provide a new foundation for asynchrony in .NET programming.</span></span> <span data-ttu-id="15966-515">Esta nueva base hace que la programación asincrónica similar a - y unos tan sencillo como - programación sincrónica.</span><span class="sxs-lookup"><span data-stu-id="15966-515">This new foundation makes asynchronous programming similar to - and about as straightforward as - synchronous programming.</span></span> <span data-ttu-id="15966-516">Ahora es posible escribir métodos de acción asincrónicos en ASP.NET MVC 4 mediante la **AsyncController** clase.</span><span class="sxs-lookup"><span data-stu-id="15966-516">You are now able to write asynchronous action methods in ASP.NET MVC 4 by using the **AsyncController** class.</span></span> <span data-ttu-id="15966-517">Puede utilizar métodos de acción asincrónicos de ejecución prolongada, no con la CPU solicitudes.</span><span class="sxs-lookup"><span data-stu-id="15966-517">You can use asynchronous action methods for long-running, non-CPU bound requests.</span></span> <span data-ttu-id="15966-518">Esto evita que el servidor Web realizar trabajo mientras se procesa la solicitud de bloqueo.</span><span class="sxs-lookup"><span data-stu-id="15966-518">This avoids blocking the Web server from performing work while the request is being processed.</span></span> <span data-ttu-id="15966-519">La clase AsyncController normalmente se usa para llamadas de servicio Web de ejecución prolongada.</span><span class="sxs-lookup"><span data-stu-id="15966-519">The AsyncController class is typically used for long-running Web service calls.</span></span>

<span data-ttu-id="15966-520">Este ejercicio explica los conceptos básicos de la operación asincrónica en ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="15966-520">This exercise explains the basics of asynchronous operation in ASP.NET MVC 4.</span></span> <span data-ttu-id="15966-521">Si desea un análisis más profundo, puede consultar el siguiente artículo: [ [https://msdn.microsoft.com/en-us/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/en-us/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/en-us/library/ee728598%28v=vs.100%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="15966-521">If you want a deeper dive, you can check out the following article: [[https://msdn.microsoft.com/en-us/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/en-us/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/en-us/library/ee728598%28v=vs.100%29.aspx)</span></span>

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a><span data-ttu-id="15966-522">Tarea 1: implementar un controlador asincrónico</span><span class="sxs-lookup"><span data-stu-id="15966-522">Task 1 - Implementing an Asynchronous Controller</span></span>

1. <span data-ttu-id="15966-523">Abra la **comenzar** solución ubicado en **origen/Ex4-Async/Begin/** carpeta.</span><span class="sxs-lookup"><span data-stu-id="15966-523">Open the **Begin** solution located at **Source/Ex4-Async/Begin/** folder.</span></span> <span data-ttu-id="15966-524">En caso contrario, puede seguir usando el **final** solución obtenido siguiendo el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="15966-524">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="15966-525">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="15966-525">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="15966-526">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="15966-526">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="15966-527">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="15966-527">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="15966-528">Por último, compile la solución haciendo clic en **generar** | **generar solución**.</span><span class="sxs-lookup"><span data-stu-id="15966-528">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="15966-529">Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="15966-529">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="15966-530">Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="15966-530">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="15966-531">Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="15966-531">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="15966-532">Abra la **HomeController.cs** clase desde el **controladores** carpeta.</span><span class="sxs-lookup"><span data-stu-id="15966-532">Open the **HomeController.cs** class from the **Controllers** folder.</span></span>
3. <span data-ttu-id="15966-533">Agregue la siguiente instrucción using.</span><span class="sxs-lookup"><span data-stu-id="15966-533">Add the following using statement.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. <span data-ttu-id="15966-534">Actualización de la **HomeController** clase herede de **AsyncController**.</span><span class="sxs-lookup"><span data-stu-id="15966-534">Update the **HomeController** class to inherit from **AsyncController**.</span></span> <span data-ttu-id="15966-535">Controladores que se derivan de AsyncController permiten a ASP.NET procesar solicitudes asincrónicas y pueden métodos de acción sincrónicos de servicio.</span><span class="sxs-lookup"><span data-stu-id="15966-535">Controllers that derive from AsyncController enable ASP.NET to process asynchronous requests, and they can still service synchronous action methods.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. <span data-ttu-id="15966-536">Agregar el **async** palabra clave para la **índice** método y hacer que el tipo de valor devuelto **tarea&lt;ActionResult&gt;**.</span><span class="sxs-lookup"><span data-stu-id="15966-536">Add the **async** keyword to the **Index** method and make it return the type **Task&lt;ActionResult&gt;**.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > <span data-ttu-id="15966-537">El **async** palabra clave es una de las nuevas palabras clave proporciona .NET Framework 4.5; indica al compilador que este método contiene código asincrónico.</span><span class="sxs-lookup"><span data-stu-id="15966-537">The **async** keyword is one of the new keywords the .NET Framework 4.5 provides; it tells the compiler that this method contains asynchronous code.</span></span> <span data-ttu-id="15966-538">A **tarea** objeto representa una operación asincrónica que puede completar en algún momento en el futuro.</span><span class="sxs-lookup"><span data-stu-id="15966-538">A **Task** object represents an asynchronous operation that may complete at some point in the future.</span></span>
6. <span data-ttu-id="15966-539">Reemplace el **cliente. GetAsync()** llamada con la versión de async completa mediante palabra clave await, tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="15966-539">Replace the **client.GetAsync()** call with the full async version using await keyword as shown below.</span></span>

    <span data-ttu-id="15966-540">(Código de fragmento de código: *GetAsync de ASP.NET MVC 4 laboratorio - Ex04 -*)</span><span class="sxs-lookup"><span data-stu-id="15966-540">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - GetAsync*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="15966-541">En la versión anterior, usaba la **resultado** propiedad desde el **tarea** objeto para bloquear el subproceso hasta que el resultado se devuelve (versión de sincronización).</span><span class="sxs-lookup"><span data-stu-id="15966-541">In the previous version, you were using the **Result** property from the **Task** object to block the thread until the result is returned (sync version).</span></span>
    > 
    > <span data-ttu-id="15966-542">Agregar el **await** palabra clave indica al compilador que esperar para la tarea devuelta desde la llamada al método de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="15966-542">Adding the **await** keyword tells the compiler to asynchronously wait for the task returned from the method call.</span></span> <span data-ttu-id="15966-543">Esto significa que el resto del código se ejecutará como una devolución de llamada únicamente cuando haya finalizado el método esperado.</span><span class="sxs-lookup"><span data-stu-id="15966-543">This means that the rest of the code will be executed as a callback only after the awaited method completes.</span></span> <span data-ttu-id="15966-544">Otra cosa que debe tener en cuenta es que no es necesario cambiar el bloque try-catch para solucionar este problema: todavía se detectarán las excepciones que se producen en segundo plano o en primer plano sin ningún trabajo adicional con un controlador proporcionado por el marco de trabajo.</span><span class="sxs-lookup"><span data-stu-id="15966-544">Another thing to notice is that you do not need to change your try-catch block in order to make this work: the exceptions that happen in background or in foreground will still be caught without any extra work using a handler provided by the framework.</span></span>
7. <span data-ttu-id="15966-545">Cambiar el código para continuar con la implementación asincrónica reemplazando las líneas con el nuevo código, tal y como se muestra a continuación</span><span class="sxs-lookup"><span data-stu-id="15966-545">Change the code to continue with the asynchronous implementation by replacing the lines with the new code as shown below</span></span>

    <span data-ttu-id="15966-546">(Código de fragmento de código: *ReadAsStringAsync de ASP.NET MVC 4 laboratorio - Ex04 -*)</span><span class="sxs-lookup"><span data-stu-id="15966-546">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. <span data-ttu-id="15966-547">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="15966-547">Run the application.</span></span> <span data-ttu-id="15966-548">Observará que no hay cambios importantes, pero el código, no se bloqueará un subproceso del grupo de subprocesos que hace un mejor uso de los recursos del servidor y se mejora el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="15966-548">You will notice no major changes, but your code will not block a thread from the thread pool making a better usage of the server resources and improving performance.</span></span>

    > [!NOTE]
    > <span data-ttu-id="15966-549">Puede aprender más sobre las nuevas características de programación asincrónicas en el laboratorio &quot; **programación asincrónica en .NET 4.5 con C# y Visual Basic** &quot; incluidos en el Kit de aprendizaje Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="15966-549">You can learn more about the new asynchronous programming features in the lab &quot;**Asynchronous Programming in .NET 4.5 with C# and Visual Basic**&quot; included in the Visual Studio Training Kit.</span></span>

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a><span data-ttu-id="15966-550">Tarea 2: los tiempos de espera de control con Tokens de cancelación</span><span class="sxs-lookup"><span data-stu-id="15966-550">Task 2 - Handling Time-Outs with Cancellation Tokens</span></span>

<span data-ttu-id="15966-551">Métodos de acción asincrónicos que devuelven instancias de la tarea también admiten los tiempos de espera.</span><span class="sxs-lookup"><span data-stu-id="15966-551">Asynchronous action methods that return Task instances can also support time-outs.</span></span> <span data-ttu-id="15966-552">En esta tarea, actualizará el código del método de índice para controlar una situación de tiempo de espera con un token de cancelación.</span><span class="sxs-lookup"><span data-stu-id="15966-552">In this task, you will update the Index method code to handle a time-out scenario using a cancellation token.</span></span>

1. <span data-ttu-id="15966-553">Vuelva a Visual Studio y presione **MAYÚS + F5** para detener la depuración.</span><span class="sxs-lookup"><span data-stu-id="15966-553">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>
2. <span data-ttu-id="15966-554">Agregue la siguiente instrucción using en la **HomeController.cs** archivo.</span><span class="sxs-lookup"><span data-stu-id="15966-554">Add the following using statement to the **HomeController.cs** file.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. <span data-ttu-id="15966-555">Actualizar la acción del índice para recibir un **CancellationToken** argumento.</span><span class="sxs-lookup"><span data-stu-id="15966-555">Update the Index action to receive a **CancellationToken** argument.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. <span data-ttu-id="15966-556">Actualización de la **GetAsync** llamada para pasar el token de cancelación.</span><span class="sxs-lookup"><span data-stu-id="15966-556">Update the **GetAsync** call to pass the cancellation token.</span></span>

    <span data-ttu-id="15966-557">(Código de fragmento de código: *ASP.NET MVC 4 laboratorio - Ex04 - SendAsync con CancellationToken*)</span><span class="sxs-lookup"><span data-stu-id="15966-557">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - SendAsync with CancellationToken*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. <span data-ttu-id="15966-558">Decorar la *índice* método con un **AsyncTimeout** atributo establecido en 500 milisegundos y un **HandleError** configurado para controlar el atributo  **TaskCanceledException** redirigiendo a un **TimedOut** vista.</span><span class="sxs-lookup"><span data-stu-id="15966-558">Decorate the *Index* method with an **AsyncTimeout** attribute set to 500 milliseconds and a **HandleError** attribute configured to handle **TaskCanceledException** by redirecting to a **TimedOut** view.</span></span>

    <span data-ttu-id="15966-559">(Código de fragmento de código: *atributos de ASP.NET MVC 4 laboratorio - Ex04 -*)</span><span class="sxs-lookup"><span data-stu-id="15966-559">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - Attributes*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. <span data-ttu-id="15966-560">Abra el **PhotoController** clase y actualizar el **galería** método retrasar los milisegundos de ejecución 1000 (1 segundo) para simular una tarea de ejecución prolongada.</span><span class="sxs-lookup"><span data-stu-id="15966-560">Open the **PhotoController** class and update the **Gallery** method to delay the execution 1000 miliseconds (1 second) to simulate a long running task.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. <span data-ttu-id="15966-561">Abra la **Web.config** de archivos y habilitar errores personalizados agregando el siguiente elemento.</span><span class="sxs-lookup"><span data-stu-id="15966-561">Open the **Web.config** file and enable custom errors by adding the following element.</span></span>


    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. <span data-ttu-id="15966-562">Crear una nueva vista en **Views\Shared** denominado **TimedOut** y usar el diseño predeterminado.</span><span class="sxs-lookup"><span data-stu-id="15966-562">Create a new view in **Views\Shared** named **TimedOut** and use the default layout.</span></span> <span data-ttu-id="15966-563">En el Explorador de soluciones, haga clic en el **Views\Shared** carpeta y seleccione **agregar | Vista**.</span><span class="sxs-lookup"><span data-stu-id="15966-563">In the Solution Explorer, right-click the **Views\Shared** folder and select **Add | View**.</span></span>

    <span data-ttu-id="15966-564">![Usar vistas diferentes para cada dispositivo móvil](whats-new-in-aspnet-mvc-4/_static/image36.png "con vistas diferentes para cada dispositivo móvil")</span><span class="sxs-lookup"><span data-stu-id="15966-564">![Using different views for each mobile device](whats-new-in-aspnet-mvc-4/_static/image36.png "Using different views for each mobile device")</span></span>

    <span data-ttu-id="15966-565">*Usar vistas diferentes para cada dispositivo móvil*</span><span class="sxs-lookup"><span data-stu-id="15966-565">*Using different views for each mobile device*</span></span>
9. <span data-ttu-id="15966-566">Actualización de la **TimedOut** ver el contenido tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="15966-566">Update the **TimedOut** view content as shown below.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. <span data-ttu-id="15966-567">Ejecute la aplicación y vaya a la dirección URL raíz.</span><span class="sxs-lookup"><span data-stu-id="15966-567">Run the application and navigate to the root URL.</span></span> <span data-ttu-id="15966-568">Tal y como se ha agregado un **Thread.Sleep** de 1000 milisegundos, obtendrá un error de tiempo de espera, generado por el **AsyncTimeout** de atributo y captura el **HandleError** atributo.</span><span class="sxs-lookup"><span data-stu-id="15966-568">As you have added a **Thread.Sleep** of 1000 milliseconds, you will get a time-out error, generated by the **AsyncTimeout** attribute and catch by the **HandleError** attribute.</span></span>

    <span data-ttu-id="15966-569">![Excepción de tiempo de espera controlan](whats-new-in-aspnet-mvc-4/_static/image37.png "excepción de tiempo de espera controlan")</span><span class="sxs-lookup"><span data-stu-id="15966-569">![Time-out exception handled](whats-new-in-aspnet-mvc-4/_static/image37.png "Time-out exception handled")</span></span>

    <span data-ttu-id="15966-570">*Excepción de tiempo de espera controlan*</span><span class="sxs-lookup"><span data-stu-id="15966-570">*Time-out exception handled*</span></span>

> [!NOTE]
> <span data-ttu-id="15966-571">Además, puede implementar esta aplicación a los siguientes sitios Web Windows Azure [apéndice D: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy](#AppendixD).</span><span class="sxs-lookup"><span data-stu-id="15966-571">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixD).</span></span>


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="15966-572">Resumen</span><span class="sxs-lookup"><span data-stu-id="15966-572">Summary</span></span>

<span data-ttu-id="15966-573">En este laboratorio de prácticas, se han observado que algunas de las nuevas características residente en ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="15966-573">In this hands-on-lab, you've observed some of the new features resident in ASP.NET MVC 4.</span></span> <span data-ttu-id="15966-574">Se han tratado los conceptos siguientes:</span><span class="sxs-lookup"><span data-stu-id="15966-574">The following concepts have been discussed:</span></span>

- <span data-ttu-id="15966-575">Aprovechar las ventajas de las mejoras en las plantillas de proyecto incluidas ASP.NET MVC la nueva plantilla de proyecto de aplicación para dispositivos móviles</span><span class="sxs-lookup"><span data-stu-id="15966-575">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="15966-576">Utilice el atributo de la ventanilla de HTML5 y las consultas de medios CSS para mejorar la presentación en dispositivos móviles</span><span class="sxs-lookup"><span data-stu-id="15966-576">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="15966-577">Usar jQuery Mobile para mejorar el progresiva y para la creación de la interfaz de usuario web táctiles optimizadas</span><span class="sxs-lookup"><span data-stu-id="15966-577">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="15966-578">Crear vistas específicas de mobile</span><span class="sxs-lookup"><span data-stu-id="15966-578">Create mobile-specific views</span></span>
- <span data-ttu-id="15966-579">Utilice el componente de modificador de vista para alternar entre las vistas de escritorio y móviles en la aplicación</span><span class="sxs-lookup"><span data-stu-id="15966-579">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="15966-580">Crear controladores asincrónicos utilizando el soporte de tarea</span><span class="sxs-lookup"><span data-stu-id="15966-580">Create asynchronous controllers using task support</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="15966-581">Apéndice A: usar fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="15966-581">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="15966-582">Con fragmentos de código, tiene todo el código que necesita a su alcance.</span><span class="sxs-lookup"><span data-stu-id="15966-582">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="15966-583">El documento de laboratorio le indicará exactamente cuándo utilizarlas, tal como se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="15966-583">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="15966-584">![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](whats-new-in-aspnet-mvc-4/_static/image38.png "fragmentos de código en Visual Studio para insertar código en el proyecto")</span><span class="sxs-lookup"><span data-stu-id="15966-584">![Using Visual Studio code snippets to insert code into your project](whats-new-in-aspnet-mvc-4/_static/image38.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="15966-585">*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*</span><span class="sxs-lookup"><span data-stu-id="15966-585">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="15966-586">***Para agregar un fragmento de código mediante el teclado (solo C#)***</span><span class="sxs-lookup"><span data-stu-id="15966-586">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="15966-587">Coloque el cursor donde desea insertar el código.</span><span class="sxs-lookup"><span data-stu-id="15966-587">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="15966-588">Comience a escribir el nombre del fragmento (sin espacios ni guiones).</span><span class="sxs-lookup"><span data-stu-id="15966-588">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="15966-589">Observe como coincidencia de nombres de fragmentos de código de muestra de IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="15966-589">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="15966-590">Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).</span><span class="sxs-lookup"><span data-stu-id="15966-590">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="15966-591">Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.</span><span class="sxs-lookup"><span data-stu-id="15966-591">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="15966-592">![Comience a escribir el nombre del fragmento](whats-new-in-aspnet-mvc-4/_static/image39.png "comience a escribir el nombre del fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="15966-592">![Start typing the snippet name](whats-new-in-aspnet-mvc-4/_static/image39.png "Start typing the snippet name")</span></span>

<span data-ttu-id="15966-593">*Comience a escribir el nombre del fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="15966-593">*Start typing the snippet name*</span></span>

<span data-ttu-id="15966-594">![Presione la tecla Tab para seleccionar el fragmento de código resaltada](whats-new-in-aspnet-mvc-4/_static/image40.png "presione Tab para seleccionar el fragmento de código resaltada")</span><span class="sxs-lookup"><span data-stu-id="15966-594">![Press Tab to select the highlighted snippet](whats-new-in-aspnet-mvc-4/_static/image40.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="15966-595">*Presione la tecla Tab para seleccionar el fragmento de código resaltada*</span><span class="sxs-lookup"><span data-stu-id="15966-595">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="15966-596">![Vuelva a presionar Tab y el fragmento de código se expandirán](whats-new-in-aspnet-mvc-4/_static/image41.png "vuelva a presionar Tab y el fragmento de código se expandirán")</span><span class="sxs-lookup"><span data-stu-id="15966-596">![Press Tab again and the snippet will expand](whats-new-in-aspnet-mvc-4/_static/image41.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="15966-597">*Vuelva a presionar Tab y el fragmento de código se expandirán*</span><span class="sxs-lookup"><span data-stu-id="15966-597">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="15966-598">***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)***</span><span class="sxs-lookup"><span data-stu-id="15966-598">***To add a code snippet using the mouse (C#, Visual Basic and XML)***</span></span>

1. <span data-ttu-id="15966-599">Haga clic en donde desea insertar el fragmento de código.</span><span class="sxs-lookup"><span data-stu-id="15966-599">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="15966-600">Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.</span><span class="sxs-lookup"><span data-stu-id="15966-600">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="15966-601">Seleccione el fragmento de código relevante de la lista haciendo clic en él.</span><span class="sxs-lookup"><span data-stu-id="15966-601">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="15966-602">![Menú contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](whats-new-in-aspnet-mvc-4/_static/image42.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="15966-602">![Right-click where you want to insert the code snippet and select Insert Snippet](whats-new-in-aspnet-mvc-4/_static/image42.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="15966-603">*Haga clic en donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="15966-603">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="15966-604">![Seleccione el fragmento de código relevante de la lista haciendo clic en él](whats-new-in-aspnet-mvc-4/_static/image43.png "elegir el fragmento de código relevante de la lista haciendo clic en él")</span><span class="sxs-lookup"><span data-stu-id="15966-604">![Pick the relevant snippet from the list, by clicking on it](whats-new-in-aspnet-mvc-4/_static/image43.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="15966-605">*Seleccione el fragmento de código relevante de la lista haciendo clic en él*</span><span class="sxs-lookup"><span data-stu-id="15966-605">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="15966-606">Apéndice B: instalación de Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="15966-606">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="15966-607">Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión usando la  **[instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="15966-607">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="15966-608">Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* con *instalador de plataforma Web de Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="15966-608">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="15966-609">Vaya a [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="15966-609">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="15966-610">O bien, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; *Visual Studio Express 2012 for Web con SDK de Windows Azure*&quot;.</span><span class="sxs-lookup"><span data-stu-id="15966-610">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="15966-611">Haga clic en **instalar ahora**.</span><span class="sxs-lookup"><span data-stu-id="15966-611">Click on **Install Now**.</span></span> <span data-ttu-id="15966-612">Si no tiene **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo primero.</span><span class="sxs-lookup"><span data-stu-id="15966-612">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="15966-613">Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.</span><span class="sxs-lookup"><span data-stu-id="15966-613">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="15966-614">![Instale Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "instale Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="15966-614">![Install Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="15966-615">*Instale Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="15966-615">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="15966-616">Lea los términos y licencias de todos los productos y haga clic en **acepto** para continuar.</span><span class="sxs-lookup"><span data-stu-id="15966-616">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Acepte los términos de licencia](whats-new-in-aspnet-mvc-4/_static/image45.png)

    <span data-ttu-id="15966-618">*Acepte los términos de licencia*</span><span class="sxs-lookup"><span data-stu-id="15966-618">*Accepting the license terms*</span></span>
5. <span data-ttu-id="15966-619">Espere hasta que finalice el proceso de descarga e instalación.</span><span class="sxs-lookup"><span data-stu-id="15966-619">Wait until the downloading and installation process completes.</span></span>

    ![Progreso de la instalación](whats-new-in-aspnet-mvc-4/_static/image46.png)

    <span data-ttu-id="15966-621">*Progreso de la instalación*</span><span class="sxs-lookup"><span data-stu-id="15966-621">*Installation progress*</span></span>
6. <span data-ttu-id="15966-622">Cuando se complete la instalación, haga clic en **finalizar**.</span><span class="sxs-lookup"><span data-stu-id="15966-622">When the installation completes, click **Finish**.</span></span>

    ![Se completó la instalación](whats-new-in-aspnet-mvc-4/_static/image47.png)

    <span data-ttu-id="15966-624">*Se completó la instalación*</span><span class="sxs-lookup"><span data-stu-id="15966-624">*Installation completed*</span></span>
7. <span data-ttu-id="15966-625">Haga clic en **Exit** para cerrar el instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="15966-625">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="15966-626">Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **Express frente a**&quot;, a continuación, haga clic en el **VS Express para Web** colocar en mosaico.</span><span class="sxs-lookup"><span data-stu-id="15966-626">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Express de VS para icono Web](whats-new-in-aspnet-mvc-4/_static/image48.png)

    <span data-ttu-id="15966-628">*Express de VS para icono Web*</span><span class="sxs-lookup"><span data-stu-id="15966-628">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a><span data-ttu-id="15966-629">Apéndice C: instalar WebMatrix 2 y iPhone simulador</span><span class="sxs-lookup"><span data-stu-id="15966-629">Appendix C: Installing WebMatrix 2 and iPhone Simulator</span></span>

<span data-ttu-id="15966-630">Para ejecutar el sitio en un dispositivo simulado iPhone puede utilizar la extensión de WebMatrix &quot;Electric simulador Mobile para iPhone&quot;.</span><span class="sxs-lookup"><span data-stu-id="15966-630">To run your site in a simulated iPhone device you can use the WebMatrix extension &quot;Electric Mobile Simulator for the iPhone&quot;.</span></span> <span data-ttu-id="15966-631">Además, puede configurar la misma extensión para ejecutar el simulador de Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="15966-631">Also, you can configure the same extension to run the simulator from Visual Studio 2012.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a><span data-ttu-id="15966-632">Tarea 1: instalar WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="15966-632">Task 1 - Installing WebMatrix 2</span></span>

1. <span data-ttu-id="15966-633">Vaya a [ [https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="15966-633">Go to [[https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="15966-634">O bien, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; *WebMatrix 2*&quot;.</span><span class="sxs-lookup"><span data-stu-id="15966-634">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*WebMatrix 2*&quot;.</span></span>
2. <span data-ttu-id="15966-635">Haga clic en **instalar ahora**.</span><span class="sxs-lookup"><span data-stu-id="15966-635">Click on **Install Now**.</span></span> <span data-ttu-id="15966-636">Si no tiene **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo primero.</span><span class="sxs-lookup"><span data-stu-id="15966-636">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="15966-637">Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.</span><span class="sxs-lookup"><span data-stu-id="15966-637">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="15966-638">![Instalar WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "instalar WebMatrix 2")</span><span class="sxs-lookup"><span data-stu-id="15966-638">![Install WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Install WebMatrix 2")</span></span>

    <span data-ttu-id="15966-639">*Instalar WebMatrix 2*</span><span class="sxs-lookup"><span data-stu-id="15966-639">*Install WebMatrix 2*</span></span>
4. <span data-ttu-id="15966-640">Lea los términos y licencias de todos los productos y haga clic en **acepto** para continuar.</span><span class="sxs-lookup"><span data-stu-id="15966-640">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    <span data-ttu-id="15966-641">![Acepte los términos de licencia](whats-new-in-aspnet-mvc-4/_static/image50.png "aceptando los términos de licencia")</span><span class="sxs-lookup"><span data-stu-id="15966-641">![Accepting the license terms](whats-new-in-aspnet-mvc-4/_static/image50.png "Accepting the license terms")</span></span>

    <span data-ttu-id="15966-642">*Acepte los términos de licencia*</span><span class="sxs-lookup"><span data-stu-id="15966-642">*Accepting the license terms*</span></span>
5. <span data-ttu-id="15966-643">Espere hasta que finalice el proceso de descarga e instalación.</span><span class="sxs-lookup"><span data-stu-id="15966-643">Wait until the downloading and installation process completes.</span></span>

    <span data-ttu-id="15966-644">![Progreso de la instalación](whats-new-in-aspnet-mvc-4/_static/image51.png "progreso de la instalación")</span><span class="sxs-lookup"><span data-stu-id="15966-644">![Installation progress](whats-new-in-aspnet-mvc-4/_static/image51.png "Installation progress")</span></span>

    <span data-ttu-id="15966-645">*Progreso de la instalación*</span><span class="sxs-lookup"><span data-stu-id="15966-645">*Installation progress*</span></span>
6. <span data-ttu-id="15966-646">Cuando se complete la instalación, haga clic en **finalizar**.</span><span class="sxs-lookup"><span data-stu-id="15966-646">When the installation completes, click **Finish**.</span></span>

    <span data-ttu-id="15966-647">![Se completó la instalación](whats-new-in-aspnet-mvc-4/_static/image52.png "se completó la instalación")</span><span class="sxs-lookup"><span data-stu-id="15966-647">![Installation completed](whats-new-in-aspnet-mvc-4/_static/image52.png "Installation completed")</span></span>

    <span data-ttu-id="15966-648">*Se completó la instalación*</span><span class="sxs-lookup"><span data-stu-id="15966-648">*Installation completed*</span></span>
7. <span data-ttu-id="15966-649">Haga clic en **Exit** para cerrar el instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="15966-649">Click **Exit** to close Web Platform Installer.</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a><span data-ttu-id="15966-650">Tarea 2: instalar la extensión de simulador de iPhone</span><span class="sxs-lookup"><span data-stu-id="15966-650">Task 2 - Installing the iPhone Simulator Extension</span></span>

1. <span data-ttu-id="15966-651">Ejecutar **WebMatrix** y abrir cualquier sitio Web existente o cree uno nuevo.</span><span class="sxs-lookup"><span data-stu-id="15966-651">Run **WebMatrix** and open any existing Web site or create a new one.</span></span>
2. <span data-ttu-id="15966-652">Haga clic en el **ejecutar** botón desde la **inicio** la cinta de opciones y seleccione **agregar nueva**.</span><span class="sxs-lookup"><span data-stu-id="15966-652">Click the **Run** button from the **Home** ribbon and select **Add new**.</span></span>

    <span data-ttu-id="15966-653">![Agregar nueva extensión de WebMatrix](whats-new-in-aspnet-mvc-4/_static/image53.png "agregar nueva extensión de WebMatrix")</span><span class="sxs-lookup"><span data-stu-id="15966-653">![Adding new WebMatrix extension](whats-new-in-aspnet-mvc-4/_static/image53.png "Adding new WebMatrix extension")</span></span>

    <span data-ttu-id="15966-654">*Agregar nueva extensión de WebMatrix*</span><span class="sxs-lookup"><span data-stu-id="15966-654">*Adding new WebMatrix extension*</span></span>
3. <span data-ttu-id="15966-655">Seleccione **iPhone simulador** y haga clic en **instalar**.</span><span class="sxs-lookup"><span data-stu-id="15966-655">Select **iPhone Simulator** and click **Install**.</span></span>

    <span data-ttu-id="15966-656">![Examen de las extensiones de WebMatrix](whats-new-in-aspnet-mvc-4/_static/image54.png "extensiones de examen de WebMatrix")</span><span class="sxs-lookup"><span data-stu-id="15966-656">![Browsing WebMatrix extensions](whats-new-in-aspnet-mvc-4/_static/image54.png "Browsing WebMatrix extensions")</span></span>

    <span data-ttu-id="15966-657">*Exploración de las extensiones de WebMatrix*</span><span class="sxs-lookup"><span data-stu-id="15966-657">*Browsing WebMatrix extensions*</span></span>
4. <span data-ttu-id="15966-658">En los detalles del paquete, haga clic en **instalar** para continuar con la instalación de la extensión.</span><span class="sxs-lookup"><span data-stu-id="15966-658">In the package details, click **Install** to continue with the extension installation.</span></span>

    <span data-ttu-id="15966-659">![iPhone extensión simulador](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone extensión simulador")</span><span class="sxs-lookup"><span data-stu-id="15966-659">![iPhone Simulator extension](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone Simulator extension")</span></span>

    <span data-ttu-id="15966-660">*extensión de simulador de iPhone*</span><span class="sxs-lookup"><span data-stu-id="15966-660">*iPhone Simulator extension*</span></span>
5. <span data-ttu-id="15966-661">Lea y acepte la términos de licencia de extensión.</span><span class="sxs-lookup"><span data-stu-id="15966-661">Read and accept the extension EULA.</span></span>

    <span data-ttu-id="15966-662">![Extensión de WebMatrix CLUF](whats-new-in-aspnet-mvc-4/_static/image56.png "extensión de WebMatrix términos de licencia")</span><span class="sxs-lookup"><span data-stu-id="15966-662">![WebMatrix extension EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix extension EULA")</span></span>

    <span data-ttu-id="15966-663">*Extensión de WebMatrix términos de licencia*</span><span class="sxs-lookup"><span data-stu-id="15966-663">*WebMatrix extension EULA*</span></span>
6. <span data-ttu-id="15966-664">Ahora, puede ejecutar el sitio Web desde WebMatrix mediante la opción de simulador de iPhone.</span><span class="sxs-lookup"><span data-stu-id="15966-664">Now, you can run your Web site from WebMatrix using the iPhone Simulator option.</span></span>

    <span data-ttu-id="15966-665">![Ejecutar con iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "ejecutar con iPhone")</span><span class="sxs-lookup"><span data-stu-id="15966-665">![Run using iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "Run using iPhone")</span></span>

    <span data-ttu-id="15966-666">*Ejecutar con iPhone*</span><span class="sxs-lookup"><span data-stu-id="15966-666">*Run using iPhone*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a><span data-ttu-id="15966-667">Tarea 3: configuración de Visual Studio 2012 para ejecutar el simulador de iPhone</span><span class="sxs-lookup"><span data-stu-id="15966-667">Task 3 - Configuring Visual Studio 2012 to run iPhone Simulator</span></span>

1. <span data-ttu-id="15966-668">Abra **Visual Studio 2012** y abrir cualquier sitio Web o crear un nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="15966-668">Open **Visual Studio 2012** and open any Web site or create a new project.</span></span>
2. <span data-ttu-id="15966-669">Haga clic en la flecha hacia abajo en el botón de ejecución y seleccione **examinar con**.</span><span class="sxs-lookup"><span data-stu-id="15966-669">Click the down arrow from the Run button and select **Browse with**.</span></span>

    <span data-ttu-id="15966-670">![Examinar con](whats-new-in-aspnet-mvc-4/_static/image58.png "examinar con")</span><span class="sxs-lookup"><span data-stu-id="15966-670">![Browse with](whats-new-in-aspnet-mvc-4/_static/image58.png "Browse with")</span></span>

    <span data-ttu-id="15966-671">*Examinar con*</span><span class="sxs-lookup"><span data-stu-id="15966-671">*Browse with*</span></span>
3. <span data-ttu-id="15966-672">En el &quot;explorar con&quot; cuadro de diálogo, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="15966-672">In the &quot;Browse With&quot; dialog, click **Add**.</span></span>
4. <span data-ttu-id="15966-673">En el &quot;Agregar programa&quot; cuadro de diálogo, utilice los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="15966-673">In the &quot;Add Program&quot; dialog, use the following values:</span></span>

    - <span data-ttu-id="15966-674">**Programa**: C:\Users\*{CurrentUser}*\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(actualización de la ruta de acceso según corresponda)*</span><span class="sxs-lookup"><span data-stu-id="15966-674">**Program**: C:\Users\*{CurrentUser}*\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(update the path accordingly)*</span></span>
    - <span data-ttu-id="15966-675">**Argumentos**: &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="15966-675">**Arguments**: &quot;1&quot;</span></span>
    - <span data-ttu-id="15966-676">**Nombre descriptivo**: iPhone simulador</span><span class="sxs-lookup"><span data-stu-id="15966-676">**Friendly name**: iPhone Simulator</span></span>

    <span data-ttu-id="15966-677">![Agregar programa](whats-new-in-aspnet-mvc-4/_static/image59.png "Agregar programa")</span><span class="sxs-lookup"><span data-stu-id="15966-677">![Add program](whats-new-in-aspnet-mvc-4/_static/image59.png "Add program")</span></span>

    <span data-ttu-id="15966-678">*Agregar programa para examinar con*</span><span class="sxs-lookup"><span data-stu-id="15966-678">*Add program to browse with*</span></span>
5. <span data-ttu-id="15966-679">Haga clic en **Aceptar** y cierre los cuadros de diálogo.</span><span class="sxs-lookup"><span data-stu-id="15966-679">Click **OK** and close the dialogs.</span></span>
6. <span data-ttu-id="15966-680">Ahora es posible ejecutar las aplicaciones Web en el simulador iPhone desde Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="15966-680">Now you are able to run your Web applications in the iPhone simulator from Visual Studio 2012.</span></span>

    <span data-ttu-id="15966-681">![Examinar con iPhone simulador](whats-new-in-aspnet-mvc-4/_static/image60.png "examinar con iPhone simulador")</span><span class="sxs-lookup"><span data-stu-id="15966-681">![Browse with iPhone Simulator](whats-new-in-aspnet-mvc-4/_static/image60.png "Browse with iPhone Simulator")</span></span>

    <span data-ttu-id="15966-682">*Examinar con iPhone simulador*</span><span class="sxs-lookup"><span data-stu-id="15966-682">*Browse with iPhone Simulator*</span></span>

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="15966-683">Apéndice D: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy</span><span class="sxs-lookup"><span data-stu-id="15966-683">Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="15966-684">Este apéndice le mostrará cómo crear un nuevo sitio web del Portal de administración de Windows Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, aprovechando la característica de publicación Web Deploy proporcionada por Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="15966-684">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="15966-685">Tarea 1: crear un nuevo sitio Web desde las ventanas de Portal de Azure</span><span class="sxs-lookup"><span data-stu-id="15966-685">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="15966-686">Vaya a la [Portal de administración de Windows Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas con su suscripción.</span><span class="sxs-lookup"><span data-stu-id="15966-686">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="15966-687">Con Windows Azure puede hospedar 10 sitios Web de ASP.NET de forma gratuita y, a continuación, ajustar la escala a medida que crece el tráfico.</span><span class="sxs-lookup"><span data-stu-id="15966-687">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="15966-688">Puede registrarse [aquí](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="15966-688">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="15966-689">![Inicie sesión en el portal de Windows Azure](whats-new-in-aspnet-mvc-4/_static/image61.png "inicie sesión en el portal de Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="15966-689">![Log on to Windows Azure portal](whats-new-in-aspnet-mvc-4/_static/image61.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="15966-690">*Inicie sesión en el Portal de administración de Azure*</span><span class="sxs-lookup"><span data-stu-id="15966-690">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="15966-691">Haga clic en **New** en la barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="15966-691">Click **New** on the command bar.</span></span>

    <span data-ttu-id="15966-692">![Crear un nuevo sitio Web](whats-new-in-aspnet-mvc-4/_static/image62.png "crear un nuevo sitio Web")</span><span class="sxs-lookup"><span data-stu-id="15966-692">![Creating a new Web Site](whats-new-in-aspnet-mvc-4/_static/image62.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="15966-693">*Crear un nuevo sitio Web*</span><span class="sxs-lookup"><span data-stu-id="15966-693">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="15966-694">Haga clic en **proceso** | **sitio Web**.</span><span class="sxs-lookup"><span data-stu-id="15966-694">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="15966-695">A continuación, seleccione **creación rápida** opción.</span><span class="sxs-lookup"><span data-stu-id="15966-695">Then select **Quick Create** option.</span></span> <span data-ttu-id="15966-696">Proporcione una dirección de URL disponible para el nuevo sitio web y haga clic en **crear sitio Web**.</span><span class="sxs-lookup"><span data-stu-id="15966-696">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="15966-697">Un sitio Web de Windows Azure es el host para una aplicación web que se ejecutan en la nube que puede controlar y administrar.</span><span class="sxs-lookup"><span data-stu-id="15966-697">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="15966-698">La opción Creación rápida permite implementar una aplicación web completada al Windows Azure sitio Web desde fuera del portal.</span><span class="sxs-lookup"><span data-stu-id="15966-698">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="15966-699">No se incluyen pasos para configurar una base de datos.</span><span class="sxs-lookup"><span data-stu-id="15966-699">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="15966-700">![Crear un nuevo sitio Web mediante Creación rápida](whats-new-in-aspnet-mvc-4/_static/image63.png "crear un nuevo sitio Web mediante Creación rápida")</span><span class="sxs-lookup"><span data-stu-id="15966-700">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-mvc-4/_static/image63.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="15966-701">*Crear un nuevo sitio Web mediante Creación rápida*</span><span class="sxs-lookup"><span data-stu-id="15966-701">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="15966-702">Espere hasta que el nuevo **sitio Web** se crea.</span><span class="sxs-lookup"><span data-stu-id="15966-702">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="15966-703">Una vez creado el sitio Web, haga clic en el vínculo situado bajo el **URL** columna.</span><span class="sxs-lookup"><span data-stu-id="15966-703">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="15966-704">Compruebe que funciona el nuevo sitio Web.</span><span class="sxs-lookup"><span data-stu-id="15966-704">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="15966-705">![En el nuevo sitio web](whats-new-in-aspnet-mvc-4/_static/image64.png "exploración al sitio web nuevo")</span><span class="sxs-lookup"><span data-stu-id="15966-705">![Browsing to the new web site](whats-new-in-aspnet-mvc-4/_static/image64.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="15966-706">*Examinar el sitio web nuevo*</span><span class="sxs-lookup"><span data-stu-id="15966-706">*Browsing to the new web site*</span></span>

    <span data-ttu-id="15966-707">![Sitio Web que ejecute](whats-new-in-aspnet-mvc-4/_static/image65.png "sitio Web que se ejecuta")</span><span class="sxs-lookup"><span data-stu-id="15966-707">![Web site running](whats-new-in-aspnet-mvc-4/_static/image65.png "Web site running")</span></span>

    <span data-ttu-id="15966-708">*Sitio Web que se ejecuta*</span><span class="sxs-lookup"><span data-stu-id="15966-708">*Web site running*</span></span>
6. <span data-ttu-id="15966-709">Vuelva al portal y haga clic en el nombre del sitio web en el **nombre** columna para mostrar las páginas de administración.</span><span class="sxs-lookup"><span data-stu-id="15966-709">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="15966-710">![Abrir las páginas de administración del sitio web](whats-new-in-aspnet-mvc-4/_static/image66.png "abrir las páginas de administración del sitio web")</span><span class="sxs-lookup"><span data-stu-id="15966-710">![Opening the web site management pages](whats-new-in-aspnet-mvc-4/_static/image66.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="15966-711">*Abrir las páginas de administración del sitio Web*</span><span class="sxs-lookup"><span data-stu-id="15966-711">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="15966-712">En el **panel** página, en la **vista rápida** sección, haga clic en el **descargar perfil de publicación** vínculo.</span><span class="sxs-lookup"><span data-stu-id="15966-712">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="15966-713">El *perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un sitio Web de Windows Azure para cada método de publicación habilitado.</span><span class="sxs-lookup"><span data-stu-id="15966-713">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="15966-714">El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse y autenticarse en cada uno de los puntos de conexión para el que está habilitado un método de publicación.</span><span class="sxs-lookup"><span data-stu-id="15966-714">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="15966-715">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para publicación de aplicaciones web a los sitios Web de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="15966-715">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="15966-716">![Descargar el sitio web de perfil de publicación](whats-new-in-aspnet-mvc-4/_static/image67.png "descargar el sitio web de perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="15966-716">![Downloading the web site publish profile](whats-new-in-aspnet-mvc-4/_static/image67.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="15966-717">*Descargar el sitio Web de perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="15966-717">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="15966-718">Descargue el archivo de perfil de publicación en una ubicación conocida.</span><span class="sxs-lookup"><span data-stu-id="15966-718">Download the publish profile file to a known location.</span></span> <span data-ttu-id="15966-719">Aún más en este ejercicio verá cómo utilizar este archivo para publicar una aplicación web a un sitios Web de Windows Azure desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="15966-719">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="15966-720">![Guardar el archivo de perfil de publicación](whats-new-in-aspnet-mvc-4/_static/image68.png "guardar el perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="15966-720">![Saving the publish profile file](whats-new-in-aspnet-mvc-4/_static/image68.png "Saving the publish profile")</span></span>

    <span data-ttu-id="15966-721">*Guardar el archivo de perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="15966-721">*Saving the publish profile file*</span></span>

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="15966-722">Tarea 2: configurar el servidor de base de datos</span><span class="sxs-lookup"><span data-stu-id="15966-722">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="15966-723">Si la aplicación realiza el uso de SQL Server debe crear un servidor de base de datos SQL de bases de datos.</span><span class="sxs-lookup"><span data-stu-id="15966-723">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="15966-724">Si desea implementar una aplicación simple que no utiliza SQL Server, podría omitir esta tarea.</span><span class="sxs-lookup"><span data-stu-id="15966-724">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="15966-725">Necesitará un servidor de base de datos SQL para almacenar la base de datos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="15966-725">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="15966-726">Puede ver los servidores de base de datos SQL de la suscripción en el portal de administración de Windows Azure en **bases de datos Sql** | **servidores** | **del servidor Panel**.</span><span class="sxs-lookup"><span data-stu-id="15966-726">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="15966-727">Si no tiene un servidor que creó, puede crear uno mediante la **agregar** botón en la barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="15966-727">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="15966-728">Tome nota de la **nombre del servidor y la dirección URL, nombre de inicio de sesión de administrador y contraseña**, tal y como se va a utilizar en las siguientes tareas.</span><span class="sxs-lookup"><span data-stu-id="15966-728">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="15966-729">No cree la base de datos, tal y como se creará en una fase posterior.</span><span class="sxs-lookup"><span data-stu-id="15966-729">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="15966-730">![Panel de servidor de base de datos SQL](whats-new-in-aspnet-mvc-4/_static/image69.png "panel de base de datos SQL Server")</span><span class="sxs-lookup"><span data-stu-id="15966-730">![SQL Database Server Dashboard](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="15966-731">*Panel de base de datos SQL Server*</span><span class="sxs-lookup"><span data-stu-id="15966-731">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="15966-732">En la siguiente tarea probará la conexión de base de datos de Visual Studio, por ese motivo debe incluir la dirección IP local en la lista del servidor de **direcciones IP permitidas**.</span><span class="sxs-lookup"><span data-stu-id="15966-732">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="15966-733">Para ello, haga clic en **configurar**, seleccione la dirección IP de **dirección IP del cliente actual** y péguela en el **dirección IP inicial** y **ladirecciónIPfinal** cuadros de texto y haga clic en el ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) botón.</span><span class="sxs-lookup"><span data-stu-id="15966-733">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) button.</span></span>

    ![Agregar dirección IP del cliente](whats-new-in-aspnet-mvc-4/_static/image71.png)

    <span data-ttu-id="15966-735">*Agregar dirección IP del cliente*</span><span class="sxs-lookup"><span data-stu-id="15966-735">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="15966-736">Una vez el **dirección IP del cliente** se agrega a las direcciones IP permitidas lista, haga clic en **guardar** para confirmar los cambios.</span><span class="sxs-lookup"><span data-stu-id="15966-736">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmar cambios](whats-new-in-aspnet-mvc-4/_static/image72.png)

    <span data-ttu-id="15966-738">*Confirmar cambios*</span><span class="sxs-lookup"><span data-stu-id="15966-738">*Confirm Changes*</span></span>

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="15966-739">Tarea 3: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy</span><span class="sxs-lookup"><span data-stu-id="15966-739">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="15966-740">Vuelva a la solución de ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="15966-740">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="15966-741">En el **el Explorador de soluciones**, haga clic en el proyecto de sitio web y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="15966-741">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="15966-742">![Publicar la aplicación](whats-new-in-aspnet-mvc-4/_static/image73.png "publicar la aplicación")</span><span class="sxs-lookup"><span data-stu-id="15966-742">![Publishing the Application](whats-new-in-aspnet-mvc-4/_static/image73.png "Publishing the Application")</span></span>

    <span data-ttu-id="15966-743">*Publicar el sitio web*</span><span class="sxs-lookup"><span data-stu-id="15966-743">*Publishing the web site*</span></span>
2. <span data-ttu-id="15966-744">Importar el perfil de publicación que se ha guardado en la primera tarea.</span><span class="sxs-lookup"><span data-stu-id="15966-744">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="15966-745">![Importar el perfil de publicación](whats-new-in-aspnet-mvc-4/_static/image74.png "importar el perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="15966-745">![Importing the publish profile](whats-new-in-aspnet-mvc-4/_static/image74.png "Importing the publish profile")</span></span>

    <span data-ttu-id="15966-746">*Importar perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="15966-746">*Importing publish profile*</span></span>
3. <span data-ttu-id="15966-747">Haga clic en **validar conexión**.</span><span class="sxs-lookup"><span data-stu-id="15966-747">Click **Validate Connection**.</span></span> <span data-ttu-id="15966-748">Una vez completada la validación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="15966-748">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="15966-749">Validación está completa cuando aparece una marca de verificación verde junto al botón Validar conexión.</span><span class="sxs-lookup"><span data-stu-id="15966-749">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="15966-750">![Validación de la conexión](whats-new-in-aspnet-mvc-4/_static/image75.png "validación de la conexión")</span><span class="sxs-lookup"><span data-stu-id="15966-750">![Validating connection](whats-new-in-aspnet-mvc-4/_static/image75.png "Validating connection")</span></span>

    <span data-ttu-id="15966-751">*Validación de la conexión*</span><span class="sxs-lookup"><span data-stu-id="15966-751">*Validating connection*</span></span>
4. <span data-ttu-id="15966-752">En el **configuración** página, en la **bases de datos** sección, haga clic en el botón situado junto al cuadro de texto de la conexión de la base de datos (es decir, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="15966-752">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="15966-753">![Configuración de Web deploy](whats-new-in-aspnet-mvc-4/_static/image76.png "configuración de Web deploy")</span><span class="sxs-lookup"><span data-stu-id="15966-753">![Web deploy configuration](whats-new-in-aspnet-mvc-4/_static/image76.png "Web deploy configuration")</span></span>

    <span data-ttu-id="15966-754">*Configuración de Web deploy*</span><span class="sxs-lookup"><span data-stu-id="15966-754">*Web deploy configuration*</span></span>
5. <span data-ttu-id="15966-755">Configure la conexión de base de datos de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="15966-755">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="15966-756">En el **nombre del servidor** escriba la dirección URL de base de datos de SQL server mediante la *tcp:* prefijo.</span><span class="sxs-lookup"><span data-stu-id="15966-756">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="15966-757">En **nombre de usuario** escriba el nombre de inicio de sesión del Administrador de servidor.</span><span class="sxs-lookup"><span data-stu-id="15966-757">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="15966-758">En **contraseña** escriba la contraseña de inicio de sesión de administrador de servidor.</span><span class="sxs-lookup"><span data-stu-id="15966-758">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="15966-759">Escriba un nuevo nombre de base de datos, por ejemplo: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="15966-759">Type a new database name, for example: *MVC4SampleDB*.</span></span>

    <span data-ttu-id="15966-760">![Configurar la cadena de conexión de destino](whats-new-in-aspnet-mvc-4/_static/image77.png "configurar la cadena de conexión de destino")</span><span class="sxs-lookup"><span data-stu-id="15966-760">![Configuring destination connection string](whats-new-in-aspnet-mvc-4/_static/image77.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="15966-761">*Configurar la cadena de conexión de destino*</span><span class="sxs-lookup"><span data-stu-id="15966-761">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="15966-762">A continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="15966-762">Then click **OK**.</span></span> <span data-ttu-id="15966-763">Cuando se le solicite para crear la base de datos, haga clic en **Sí**.</span><span class="sxs-lookup"><span data-stu-id="15966-763">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="15966-764">![Crear la base de datos](whats-new-in-aspnet-mvc-4/_static/image78.png "crear la cadena de la base de datos")</span><span class="sxs-lookup"><span data-stu-id="15966-764">![Creating the database](whats-new-in-aspnet-mvc-4/_static/image78.png "Creating the database string")</span></span>

    <span data-ttu-id="15966-765">*Crear la base de datos*</span><span class="sxs-lookup"><span data-stu-id="15966-765">*Creating the database*</span></span>
7. <span data-ttu-id="15966-766">La cadena de conexión que se va a usar para conectarse a la base de datos de SQL en Windows Azure se muestra en el cuadro de texto de conexión predeterminado.</span><span class="sxs-lookup"><span data-stu-id="15966-766">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="15966-767">Después, haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="15966-767">Then click **Next**.</span></span>

    <span data-ttu-id="15966-768">![Cadena de conexión que apunte a la base de datos SQL](whats-new-in-aspnet-mvc-4/_static/image79.png "cadena de conexión que apunte a la base de datos SQL")</span><span class="sxs-lookup"><span data-stu-id="15966-768">![Connection string pointing to SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="15966-769">*Cadena de conexión que apunte a la base de datos SQL*</span><span class="sxs-lookup"><span data-stu-id="15966-769">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="15966-770">En el **vista previa** página, haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="15966-770">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="15966-771">![Publicar la aplicación web](whats-new-in-aspnet-mvc-4/_static/image80.png "publicar la aplicación web")</span><span class="sxs-lookup"><span data-stu-id="15966-771">![Publishing the web application](whats-new-in-aspnet-mvc-4/_static/image80.png "Publishing the web application")</span></span>

    <span data-ttu-id="15966-772">*Publicar la aplicación web*</span><span class="sxs-lookup"><span data-stu-id="15966-772">*Publishing the web application*</span></span>
9. <span data-ttu-id="15966-773">Una vez que finalice el proceso de publicación, el explorador predeterminado abrirá el sitio web publicado.</span><span class="sxs-lookup"><span data-stu-id="15966-773">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="15966-774">![Publica la aplicación en Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "aplicación se publicó en Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="15966-774">![Application published to Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="15966-775">*Aplicación que se publican en Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="15966-775">*Application published to Windows Azure*</span></span>
