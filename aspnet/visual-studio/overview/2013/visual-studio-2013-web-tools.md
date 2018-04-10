---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Laboratorio de prácticas: Visual Studio 2013 Web Tools | Documentos de Microsoft'
author: rick-anderson
description: Visual Studio es un entorno de desarrollo excelente. Ventanas basadas en NET y proyectos web. Incluye un editor de texto eficaz que puede utilizarse fácilmente para...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: ef8ab82f9043ef9da3a3e6a146a97f083149534d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2018
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a><span data-ttu-id="43df2-104">Laboratorio de prácticas: Visual Studio 2013 Web Tools</span><span class="sxs-lookup"><span data-stu-id="43df2-104">Hands On Lab: Visual Studio 2013 Web Tools</span></span>
====================
<span data-ttu-id="43df2-105">Por [Web colonias equipo](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="43df2-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="43df2-106">Descargar el Kit de aprendizaje de colonias de Web</span><span class="sxs-lookup"><span data-stu-id="43df2-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="43df2-107">Visual Studio es un entorno de desarrollo excelente. Ventanas basadas en NET y proyectos web.</span><span class="sxs-lookup"><span data-stu-id="43df2-107">Visual Studio is an excellent development environment for .NET-based Windows and web projects.</span></span> <span data-ttu-id="43df2-108">Incluye un editor de texto eficaz que puede utilizar fácilmente para editar archivos independientes sin un proyecto.</span><span class="sxs-lookup"><span data-stu-id="43df2-108">It includes a powerful text editor that can easily be used to edit standalone files without a project.</span></span>
> 
> <span data-ttu-id="43df2-109">Visual Studio mantiene un árbol de análisis completos cuando se edita cada archivo.</span><span class="sxs-lookup"><span data-stu-id="43df2-109">Visual Studio maintains a full-featured parse tree as you edit each file.</span></span> <span data-ttu-id="43df2-110">Esto permite a Visual Studio proporcionar características de Autocompletar sin parangón y acciones basadas en el documento al realizar la experiencia de desarrollo más agradable y mucho más rápida.</span><span class="sxs-lookup"><span data-stu-id="43df2-110">This allows Visual Studio to provide unparalleled auto-completion and document-based actions while making the development experience much faster and more pleasant.</span></span> <span data-ttu-id="43df2-111">Estas características son especialmente eficaces en documentos HTML y CSS.</span><span class="sxs-lookup"><span data-stu-id="43df2-111">These features are especially powerful in HTML and CSS documents.</span></span>
> 
> <span data-ttu-id="43df2-112">Toda esta potencia también está disponible para las extensiones, de forma que sea fácil ampliar los editores con características nuevas y eficaces para satisfacer sus necesidades.</span><span class="sxs-lookup"><span data-stu-id="43df2-112">All of this power is also available for extensions, making it simple to extend the editors with powerful new features to suit your needs.</span></span> <span data-ttu-id="43df2-113">Web Essentials es una colección de mejoras (principalmente) basadas en la web de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="43df2-113">Web Essentials is a collection of (mostly) web-related enhancements to Visual Studio.</span></span> <span data-ttu-id="43df2-114">Incluye una gran cantidad de nuevos finalizaciones de IntelliSense (especialmente para CSS), nuevas características de vínculo de explorador, automático de archivos JSHint para JavaScript, nuevas advertencias para HTML, CSS y muchas otras características que son esenciales para el desarrollo web moderna.</span><span class="sxs-lookup"><span data-stu-id="43df2-114">It includes lots of new IntelliSense completions (especially for CSS), new Browser Link features, automatic JSHint for JavaScript files, new warnings for HTML and CSS, and many other features that are essential to modern web development.</span></span>
> 
> <span data-ttu-id="43df2-115">Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web colonias, disponible en [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="43df2-115">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="43df2-116">Información general</span><span class="sxs-lookup"><span data-stu-id="43df2-116">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="43df2-117">Objetivos</span><span class="sxs-lookup"><span data-stu-id="43df2-117">Objectives</span></span>

<span data-ttu-id="43df2-118">En este laboratorio práctico, aprenderá cómo:</span><span class="sxs-lookup"><span data-stu-id="43df2-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="43df2-119">Usar las nuevas características del editor HTML Web Essentials incluidas como fragmentos de código de HTML5 enriquecidos y codificación Zen</span><span class="sxs-lookup"><span data-stu-id="43df2-119">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="43df2-120">Usar las nuevas características del editor de CSS Web Essentials incluidas como el selector de colores y la información sobre herramientas de explorador matriz</span><span class="sxs-lookup"><span data-stu-id="43df2-120">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="43df2-121">Usar las nuevas características de editor de JavaScript incluidas en la Web Essentials, como extraer a archivo e IntelliSense de todos los elementos HTML</span><span class="sxs-lookup"><span data-stu-id="43df2-121">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="43df2-122">Intercambiar datos entre el explorador y Visual Studio mediante el vínculo de explorador</span><span class="sxs-lookup"><span data-stu-id="43df2-122">Exchange data between your browser and Visual Studio using Browser Link</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="43df2-123">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="43df2-123">Prerequisites</span></span>

<span data-ttu-id="43df2-124">Para completar este laboratorio de prácticas, es necesario lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="43df2-124">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="43df2-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) o superior</span><span class="sxs-lookup"><span data-stu-id="43df2-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="43df2-126">Web Essentials 2013</span><span class="sxs-lookup"><span data-stu-id="43df2-126">Web Essentials 2013</span></span>](http://vswebessentials.com/)
- [<span data-ttu-id="43df2-127">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="43df2-127">Google Chrome</span></span>](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="43df2-128">Programa de instalación</span><span class="sxs-lookup"><span data-stu-id="43df2-128">Setup</span></span>

<span data-ttu-id="43df2-129">Para ejecutar los ejercicios en este laboratorio práctico, debe configurar primero el entorno.</span><span class="sxs-lookup"><span data-stu-id="43df2-129">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="43df2-130">Abra una ventana del explorador de Windows y vaya a la práctica **origen** carpeta.</span><span class="sxs-lookup"><span data-stu-id="43df2-130">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="43df2-131">Haga clic en **Setup.cmd** y seleccione **ejecutar como administrador** para iniciar el proceso de instalación que configure el entorno e instalará los fragmentos de código de Visual Studio para este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="43df2-131">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="43df2-132">Si se muestra el cuadro de diálogo de Control de cuentas de usuario, confirme la acción para continuar.</span><span class="sxs-lookup"><span data-stu-id="43df2-132">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="43df2-133">Asegúrese de que ha activado todas las dependencias para este laboratorio antes de ejecutar el programa de instalación.</span><span class="sxs-lookup"><span data-stu-id="43df2-133">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="43df2-134">Uso de los fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="43df2-134">Using the Code Snippets</span></span>

<span data-ttu-id="43df2-135">En este documento de laboratorio, le indicará que insertar bloques de código.</span><span class="sxs-lookup"><span data-stu-id="43df2-135">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="43df2-136">Para su comodidad, la mayor parte de este código se proporciona como fragmentos de código de Visual Studio, puede tener acceso desde dentro de Visual Studio 2013 para evitar tener que agregarla manualmente.</span><span class="sxs-lookup"><span data-stu-id="43df2-136">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="43df2-137">Cada ejercicio viene acompañado por una solución de inicia ubicada en el **comenzar** carpeta del ejercicio que le permite seguir cada ejercicio independientemente de las otras.</span><span class="sxs-lookup"><span data-stu-id="43df2-137">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="43df2-138">Ten en cuenta que los fragmentos de código que se agregan durante un ejercicio se encuentran desde estas a partir de soluciones y no pueden funcionar hasta que haya completado el ejercicio.</span><span class="sxs-lookup"><span data-stu-id="43df2-138">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="43df2-139">En el código fuente de un ejercicio, también encontrará un **final** carpeta que contiene una solución de Visual Studio con el código que se obtiene al completar los pasos en el ejercicio correspondiente.</span><span class="sxs-lookup"><span data-stu-id="43df2-139">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="43df2-140">Puede usar estas soluciones como guía si necesita ayuda adicional cuando se trabaja a través de este laboratorio práctico.</span><span class="sxs-lookup"><span data-stu-id="43df2-140">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="43df2-141">Ejercicios</span><span class="sxs-lookup"><span data-stu-id="43df2-141">Exercises</span></span>

<span data-ttu-id="43df2-142">Este laboratorio de prácticas incluye los ejercicios siguientes:</span><span class="sxs-lookup"><span data-stu-id="43df2-142">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="43df2-143">Trabajar con elementos esenciales de Web y de vínculo de explorador</span><span class="sxs-lookup"><span data-stu-id="43df2-143">Working with Browser Link and Web Essentials</span></span>](#Exercise1)
2. [<span data-ttu-id="43df2-144">Sacar partido de IntelliSense y de fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="43df2-144">Taking Advantage of Code Snippets and IntelliSense</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="43df2-145">Cuando se inicia Visual Studio por primera vez, debe seleccionar una de las colecciones de configuraciones predefinidas.</span><span class="sxs-lookup"><span data-stu-id="43df2-145">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="43df2-146">Cada colección predefinida está diseñado para que coincida con un estilo de desarrollo determinado y determina los diseños de ventana, comportamiento del editor, fragmentos de código de IntelliSense y opciones del cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="43df2-146">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="43df2-147">Los procedimientos descritos en este laboratorio describen las acciones necesarias para realizar una tarea concreta en Visual Studio cuando se usa el **configuración General de desarrollo** colección.</span><span class="sxs-lookup"><span data-stu-id="43df2-147">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="43df2-148">Si elige una colección de configuraciones diferentes para el entorno de desarrollo, puede haber diferencias en los pasos que debe tener en cuenta.</span><span class="sxs-lookup"><span data-stu-id="43df2-148">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a><span data-ttu-id="43df2-149">Ejercicio 1: Trabajar con elementos esenciales de Web y de vínculo de explorador</span><span class="sxs-lookup"><span data-stu-id="43df2-149">Exercise 1: Working with Browser Link and Web Essentials</span></span>

<span data-ttu-id="43df2-150">**Web Essentials** es una extensión de Visual Studio que agrega una variedad de características útiles para el desarrollo web moderna, se centra principalmente en hacer que la experiencia de desarrollo web mucho más rápida y más agradable.</span><span class="sxs-lookup"><span data-stu-id="43df2-150">**Web Essentials** is a Visual Studio extension that adds a variety of useful features for modern web development, mostly focused on making the web development experience much faster and more pleasant.</span></span> <span data-ttu-id="43df2-151">Puede instalar Essentials Web desde la Galería de extensiones de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="43df2-151">You can install Web Essentials from the Extension Gallery in Visual Studio.</span></span>

<span data-ttu-id="43df2-152">**Vínculo de explorador** es una característica nueva incluida en Visual Studio 2013 que proporciona un canal entre el IDE de Visual Studio y cualquier explorador abierto para intercambiar datos entre la aplicación web y Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="43df2-152">**Browser Link** is a new feature included in Visual Studio 2013 that provides a channel between the Visual Studio IDE and any open browser to exchange data between your web application and Visual Studio.</span></span> <span data-ttu-id="43df2-153">Web Essentials amplía vínculo de explorador con herramientas para manipular el modelo de objetos DOM y los estilos CSS de las páginas web directamente desde el explorador.</span><span class="sxs-lookup"><span data-stu-id="43df2-153">Web Essentials extends Browser Link with tools to manipulate the DOM object model and the CSS styles of your web pages directly from the browser.</span></span>

<span data-ttu-id="43df2-154">En este ejercicio, explorará algunas de las características admitidas por **Web Essentials** y **vínculo de explorador** para mejorar una página de prueba simple.</span><span class="sxs-lookup"><span data-stu-id="43df2-154">In this exercise, you will explore some of the features supported by **Web Essentials** and **Browser Link** to enhance a simple quiz page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a><span data-ttu-id="43df2-155">Tarea 1: ejecutar el proyecto en varios exploradores</span><span class="sxs-lookup"><span data-stu-id="43df2-155">Task 1 - Running the Project in Multiple Browsers</span></span>

<span data-ttu-id="43df2-156">En esta tarea, configurará la aplicación web para que se ejecute en varios exploradores a la vez, lo que resulta útil para las pruebas de varios exploradores.</span><span class="sxs-lookup"><span data-stu-id="43df2-156">In this task, you will configure your web application to run in multiple browsers at once, which is useful for cross-browser testing.</span></span>

1. <span data-ttu-id="43df2-157">Abra **Microsoft Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="43df2-157">Open **Microsoft Visual Studio**.</span></span>
2. <span data-ttu-id="43df2-158">En el **archivo** menú, seleccione **abrir | Proyecto o solución... ** y vaya a **Ex1 WorkingwithBrowserLinkandWebEssentials\Begin** en el **origen** carpeta del laboratorio (C:\WebCampsTK\HOL\VSWebTooling\Source).</span><span class="sxs-lookup"><span data-stu-id="43df2-158">In the **File** menu, select **Open | Project/Solution...** and browse to **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** in the **Source** folder of the lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span></span> <span data-ttu-id="43df2-159">Seleccione **Begin.sln** y haga clic en **abiertos**.</span><span class="sxs-lookup"><span data-stu-id="43df2-159">Select **Begin.sln** and click **Open**.</span></span>
3. <span data-ttu-id="43df2-160">En la barra de herramientas de Visual Studio, expanda el menú del explorador y seleccione **examinar con... **.</span><span class="sxs-lookup"><span data-stu-id="43df2-160">In the Visual Studio toolbar, expand the browser menu and select **Browse With...**.</span></span>

    <span data-ttu-id="43df2-161">![Opción del menú Examinar con](visual-studio-2013-web-tools/_static/image1.png "examinar con... en el menú del explorador")</span><span class="sxs-lookup"><span data-stu-id="43df2-161">![Browse With menu option](visual-studio-2013-web-tools/_static/image1.png "Browse with... in browser menu")</span></span>

    <span data-ttu-id="43df2-162">*Explorar con opción de menú*</span><span class="sxs-lookup"><span data-stu-id="43df2-162">*Browse With menu option*</span></span>
4. <span data-ttu-id="43df2-163">En el **explorar con** cuadro de diálogo, seleccione **Google Chrome** y **Internet Explorer** , mantenga presionada la **CTRL** clave y haga clic en ** Establecer como predeterminado**.</span><span class="sxs-lookup"><span data-stu-id="43df2-163">In the **Browse With** dialog box, select both **Google Chrome** and **Internet Explorer** by holding down the **CTRL** key and click **Set as Default**.</span></span>

    <span data-ttu-id="43df2-164">![Examinar con el cuadro de diálogo](visual-studio-2013-web-tools/_static/image2.png "examinar con el cuadro de diálogo")</span><span class="sxs-lookup"><span data-stu-id="43df2-164">![Browse with dialog box](visual-studio-2013-web-tools/_static/image2.png "Browse with dialog box")</span></span>

    <span data-ttu-id="43df2-165">*Al seleccionar varios exploradores predeterminados*</span><span class="sxs-lookup"><span data-stu-id="43df2-165">*Selecting multiple default browsers*</span></span>
5. <span data-ttu-id="43df2-166">Internet Explorer y Google Chrome deberían aparecer ahora como los exploradores de manera predeterminada.</span><span class="sxs-lookup"><span data-stu-id="43df2-166">Both Google Chrome and Internet Explorer should now appear as the default browsers.</span></span> <span data-ttu-id="43df2-167">Haga clic en **cancelar** para cerrar el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="43df2-167">Click **Cancel** to close the dialog box.</span></span>

    <span data-ttu-id="43df2-168">![Google Chrome e Internet Explorer como exploradores predeterminados](visual-studio-2013-web-tools/_static/image3.png "exploradores predeterminados de Internet Explorer y Google Chrome")</span><span class="sxs-lookup"><span data-stu-id="43df2-168">![Google Chrome and Internet Explorer as default browsers](visual-studio-2013-web-tools/_static/image3.png "Google Chrome and Internet Explorer default browsers")</span></span>

    <span data-ttu-id="43df2-169">*Google Chrome e Internet Explorer como exploradores predeterminados*</span><span class="sxs-lookup"><span data-stu-id="43df2-169">*Google Chrome and Internet Explorer as default browsers*</span></span>

    > [!NOTE]
    > <span data-ttu-id="43df2-170">Después de configurar los exploradores de manera predeterminada, el **varios exploradores** opción está seleccionada en el menú del explorador.</span><span class="sxs-lookup"><span data-stu-id="43df2-170">After configuring the default browsers, the **Multiple Browsers** option is selected in the browser menu.</span></span>
    > 
    > <span data-ttu-id="43df2-171">![Varios exploradores](visual-studio-2013-web-tools/_static/image4.png "varios exploradores")</span><span class="sxs-lookup"><span data-stu-id="43df2-171">![Multiple browsers](visual-studio-2013-web-tools/_static/image4.png "Multiple browsers")</span></span>
6. <span data-ttu-id="43df2-172">Presione **CTRL** + **F5** para ejecutar la aplicación sin depuración.</span><span class="sxs-lookup"><span data-stu-id="43df2-172">Press **CTRL** + **F5** to run the application without debugging.</span></span>
7. <span data-ttu-id="43df2-173">Cuando abren dos ventanas del explorador, coloque uno de ellos por encima de la otra con el fin de ver las actualizaciones en los exploradores al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="43df2-173">When both browser windows open, place one of them above the other in order to see the updates on both browsers simultaneously.</span></span> <span data-ttu-id="43df2-174">Los exploradores deben mostrar una página web con un rectángulo de color azul claro.</span><span class="sxs-lookup"><span data-stu-id="43df2-174">The browsers should display a web page with a light-blue rectangle.</span></span>

    <span data-ttu-id="43df2-175">![Colocación de un explorador encima de otro](visual-studio-2013-web-tools/_static/image5.png "colocar uno explorador encima de otro")</span><span class="sxs-lookup"><span data-stu-id="43df2-175">![Placing one browser above the other](visual-studio-2013-web-tools/_static/image5.png "Placing one browser above the other")</span></span>

    <span data-ttu-id="43df2-176">*Colocación de un explorador encima de otro*</span><span class="sxs-lookup"><span data-stu-id="43df2-176">*Placing one browser above the other*</span></span>
8. <span data-ttu-id="43df2-177">No cierre los exploradores.</span><span class="sxs-lookup"><span data-stu-id="43df2-177">Do not close the browsers.</span></span> <span data-ttu-id="43df2-178">Los usará en la siguiente tarea.</span><span class="sxs-lookup"><span data-stu-id="43df2-178">You will use them in the next task.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a><span data-ttu-id="43df2-179">Tarea 2: usar Zen de codificación para crear elementos HTML</span><span class="sxs-lookup"><span data-stu-id="43df2-179">Task 2 - Using Zen Coding to Create HTML Elements</span></span>

<span data-ttu-id="43df2-180">**Codificación Zen** es un editor de código de complemento de alta velocidad HTML, XML, XSL (o cualquier otro formato de código estructurado) y de edición.</span><span class="sxs-lookup"><span data-stu-id="43df2-180">**Zen Coding** is an editor plugin for high-speed HTML, XML, XSL (or any other structured code format) coding and editing.</span></span> <span data-ttu-id="43df2-181">El núcleo de este complemento es un motor de abreviatura eficaz que permite expandir expresiones - similares a los selectores CSS - en código HTML.</span><span class="sxs-lookup"><span data-stu-id="43df2-181">The core of this plugin is a powerful abbreviation engine which allows you to expand expressions -similar to CSS selectors- into HTML code.</span></span> <span data-ttu-id="43df2-182">Codificación Zen es una forma rápida de escribir la sintaxis del selector de estilo HTML mediante CSS.</span><span class="sxs-lookup"><span data-stu-id="43df2-182">Zen Coding is a fast way to write HTML using a CSS style selector syntax.</span></span>

<span data-ttu-id="43df2-183">En este ejercicio, utilizará la característica de codificación Zen proporcionada por Web Essentials para generar los botones HTML que representan las opciones de la pregunta.</span><span class="sxs-lookup"><span data-stu-id="43df2-183">In this exercise, you will use the Zen Coding feature provided by Web Essentials to generate the HTML buttons that represent the options of the question.</span></span>

1. <span data-ttu-id="43df2-184">Cambie a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="43df2-184">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="43df2-185">Abra la **Index.cshtml** archivo se encuentra en la **vistas** | **inicio** carpeta.</span><span class="sxs-lookup"><span data-stu-id="43df2-185">Open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="43df2-186">Reemplace el ** &lt;!--TODO: agregar aquí--opciones&gt; ** comentario con el código siguiente y presione **ficha**.</span><span class="sxs-lookup"><span data-stu-id="43df2-186">Replace the **&lt;!-- TODO: add options here--&gt;** comment with the following code, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. <span data-ttu-id="43df2-187">El código debe ampliarse a HTML.</span><span class="sxs-lookup"><span data-stu-id="43df2-187">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="43df2-188">![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")</span><span class="sxs-lookup"><span data-stu-id="43df2-188">![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")</span></span>

    <span data-ttu-id="43df2-189">*Expanded HTML*</span><span class="sxs-lookup"><span data-stu-id="43df2-189">*Expanded HTML*</span></span>

    > [!NOTE]
    > <span data-ttu-id="43df2-190">Para obtener más información sobre sintaxis Zen de codificación, vea la siguiente [artículo](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="43df2-190">To learn more about Zen Coding syntax, see the following [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span></span>
5. <span data-ttu-id="43df2-191">Haga clic en el **Actualizar exploradores vinculados** botón para actualizar dos exploradores.</span><span class="sxs-lookup"><span data-stu-id="43df2-191">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="43df2-192">![Actualizar exploradores vinculados](visual-studio-2013-web-tools/_static/image7.png "Actualizar exploradores vinculados")</span><span class="sxs-lookup"><span data-stu-id="43df2-192">![Refresh linked browsers](visual-studio-2013-web-tools/_static/image7.png "Refresh linked browsers")</span></span>

    <span data-ttu-id="43df2-193">*Actualizar exploradores vinculados*</span><span class="sxs-lookup"><span data-stu-id="43df2-193">*Refresh linked browsers*</span></span>

    <span data-ttu-id="43df2-194">![Internet Explorer - página se actualiza con cuatro botones](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - página se actualiza con cuatro botones")</span><span class="sxs-lookup"><span data-stu-id="43df2-194">![Internet Explorer - Page updated with four buttons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Page updated with four buttons")</span></span>

    <span data-ttu-id="43df2-195">*Internet Explorer - página se actualiza con cuatro botones*</span><span class="sxs-lookup"><span data-stu-id="43df2-195">*Internet Explorer - Page updated with four buttons*</span></span>

    <span data-ttu-id="43df2-196">![Google Chrome - página se actualiza con cuatro botones](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - página se actualiza con cuatro botones")</span><span class="sxs-lookup"><span data-stu-id="43df2-196">![Google Chrome - Page updated with four buttons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Page updated with four buttons")</span></span>

    <span data-ttu-id="43df2-197">*Google Chrome - página se actualiza con cuatro botones*</span><span class="sxs-lookup"><span data-stu-id="43df2-197">*Google Chrome - Page updated with four buttons*</span></span>
6. <span data-ttu-id="43df2-198">Cambie a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="43df2-198">Switch back to Visual Studio.</span></span>
7. <span data-ttu-id="43df2-199">Ha agregado los botones a la página, pero necesite agregar una pregunta de la plantilla.</span><span class="sxs-lookup"><span data-stu-id="43df2-199">You have added the buttons to the page, but you still need to add a template question.</span></span> <span data-ttu-id="43df2-200">Para ello, usará una nueva característica en Essentials Web denominado **Lorem Ipsum generador**.</span><span class="sxs-lookup"><span data-stu-id="43df2-200">To do so, you will use a new feature in Web Essentials called **Lorem Ipsum generator**.</span></span> <span data-ttu-id="43df2-201">Busque la **div** elemento con la **clase** atributo **frontal**.</span><span class="sxs-lookup"><span data-stu-id="43df2-201">Locate the **div** element with the **class** attribute **front**.</span></span>
8. <span data-ttu-id="43df2-202">Agregue el siguiente código como el primer elemento secundario de la **div**y presione **ficha**.</span><span class="sxs-lookup"><span data-stu-id="43df2-202">Add the following code as the first child element of the **div**, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. <span data-ttu-id="43df2-203">El código debe ampliarse a HTML.</span><span class="sxs-lookup"><span data-stu-id="43df2-203">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="43df2-204">![Genera automáticamente Lorem Ipsum](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerado")</span><span class="sxs-lookup"><span data-stu-id="43df2-204">![Lorem Ipsum autogenerated](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerated")</span></span>

    <span data-ttu-id="43df2-205">*Lorem Ipsum autogenerado*</span><span class="sxs-lookup"><span data-stu-id="43df2-205">*Lorem Ipsum autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="43df2-206">Como parte de codificación Zen, ahora pueden generar código Lorem Ipsum directamente en el editor de HTML.</span><span class="sxs-lookup"><span data-stu-id="43df2-206">As part of Zen Coding, you can now generate Lorem Ipsum code directly in the HTML editor.</span></span> <span data-ttu-id="43df2-207">Basta con que escriba **lorem** y posicionamiento **ficha** y un 30 Lorem Ipsum se insertará texto de word.</span><span class="sxs-lookup"><span data-stu-id="43df2-207">Simply type **lorem** and hit **TAB** and a 30 word Lorem Ipsum text will be inserted.</span></span> <span data-ttu-id="43df2-208">P. ej.,</span><span class="sxs-lookup"><span data-stu-id="43df2-208">E.g.</span></span> <span data-ttu-id="43df2-209">*lorem10* inserta 10 palabras Lorem Ipsum.</span><span class="sxs-lookup"><span data-stu-id="43df2-209">*lorem10* inserts 10 Lorem Ipsum words.</span></span>
10. <span data-ttu-id="43df2-210">Agregará un logotipo en la parte superior de la pregunta utilizando otra característica nueva en Essentials Web denominado **generador Lorem píxeles**.</span><span class="sxs-lookup"><span data-stu-id="43df2-210">You will add a logo at the top of the question by using another new feature in Web Essentials called **Lorem Pixel generator**.</span></span> <span data-ttu-id="43df2-211">Agregue el siguiente código como el primer elemento secundario de la **div** elemento con **contenedor** como **clase** valor y presione **ficha**.</span><span class="sxs-lookup"><span data-stu-id="43df2-211">Add the following code as the first child element of the **div** element with **container** as **class** value, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. <span data-ttu-id="43df2-212">El código debe expandir en HTML.</span><span class="sxs-lookup"><span data-stu-id="43df2-212">The code should expand to HTML.</span></span>

    <span data-ttu-id="43df2-213">![lorem Píxel autogenerado](visual-studio-2013-web-tools/_static/image11.png "autogenerado Lorem píxeles")</span><span class="sxs-lookup"><span data-stu-id="43df2-213">![Lorem Pixel autogenerated](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel autogenerated")</span></span>

    <span data-ttu-id="43df2-214">*Genera automáticamente lorem píxeles*</span><span class="sxs-lookup"><span data-stu-id="43df2-214">*Lorem Pixel autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="43df2-215">Como parte de codificación Zen, también puede generar código Lorem píxeles directamente en el editor de HTML.</span><span class="sxs-lookup"><span data-stu-id="43df2-215">As part of Zen Coding, you can also generate Lorem Pixel code directly in the HTML editor.</span></span> <span data-ttu-id="43df2-216">Basta con que escriba **foto-200 x 200-animales** y posicionamiento **ficha** y **img** etiqueta con una imagen de 200 x 200 de un animal que se van a insertar.</span><span class="sxs-lookup"><span data-stu-id="43df2-216">Simply type **pix-200x200-animals** and hit **TAB** and an **img** tag with a 200x200 image of an animal will be inserted.</span></span> <span data-ttu-id="43df2-217">Para obtener más información, consulte [Lorem píxeles](http://www.lorempixel.com).</span><span class="sxs-lookup"><span data-stu-id="43df2-217">For more information, refer to [Lorem Pixel](http://www.lorempixel.com).</span></span>
12. <span data-ttu-id="43df2-218">Haga clic en el **Actualizar exploradores vinculados** botón para actualizar dos exploradores.</span><span class="sxs-lookup"><span data-stu-id="43df2-218">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="43df2-219">![Internet Explorer - autogenerado imagen y texto](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - autogenerado imagen y texto")</span><span class="sxs-lookup"><span data-stu-id="43df2-219">![Internet Explorer - Autogenerated image and text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - Autogenerated image and text")</span></span>

    <span data-ttu-id="43df2-220">*Internet Explorer - autogenerado imagen y texto*</span><span class="sxs-lookup"><span data-stu-id="43df2-220">*Internet Explorer - Autogenerated image and text*</span></span>

    <span data-ttu-id="43df2-221">![Google Chrome - autogenerado imagen y texto](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - autogenerado imagen y texto")</span><span class="sxs-lookup"><span data-stu-id="43df2-221">![Google Chrome - Autogenerated image and text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - Autogenerated image and text")</span></span>

    <span data-ttu-id="43df2-222">*Google Chrome - autogenerado imagen y texto*</span><span class="sxs-lookup"><span data-stu-id="43df2-222">*Google Chrome - Autogenerated image and text*</span></span>

    > [!NOTE]
    > <span data-ttu-id="43df2-223">Dado que la imagen se selecciona aleatoriamente al agregar el fragmento de código, puede diferir la imagen que se muestra en los exploradores.</span><span class="sxs-lookup"><span data-stu-id="43df2-223">Because the image is selected randomly when adding the code snippet, the image shown in the browsers may differ.</span></span>
13. <span data-ttu-id="43df2-224">No cierre los exploradores.</span><span class="sxs-lookup"><span data-stu-id="43df2-224">Do not close the browsers.</span></span> <span data-ttu-id="43df2-225">Los usará en la siguiente tarea.</span><span class="sxs-lookup"><span data-stu-id="43df2-225">You will use them in the next task.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a><span data-ttu-id="43df2-226">Tarea 3: actualizar una propiedad de estilo</span><span class="sxs-lookup"><span data-stu-id="43df2-226">Task 3 - Updating a Style Property</span></span>

<span data-ttu-id="43df2-227">En esta tarea, va a usar el vínculo de explorador **inspeccionar modo** característica para detectar la ubicación exacta donde se genera el elemento DOM específico y, a continuación, actualice la propiedad de color de ese elemento mediante un selector de colores proporcionado por Web Essentials.</span><span class="sxs-lookup"><span data-stu-id="43df2-227">In this task, you will use the Browser Link's **Inspect Mode** feature to detect the exact location where the specific DOM element is generated and then update the color property of that element using a color picker provided by Web Essentials.</span></span>

1. <span data-ttu-id="43df2-228">En el Explorador de Internet Explorer, presione **CTRL** + **ALT** + **I** para habilitar el modo de inspeccionar.</span><span class="sxs-lookup"><span data-stu-id="43df2-228">In the Internet Explorer browser, press **CTRL** + **ALT** + **I** to enable Inspect Mode.</span></span>
2. <span data-ttu-id="43df2-229">Mueva el puntero sobre el borde de color azul claro y haga clic en.</span><span class="sxs-lookup"><span data-stu-id="43df2-229">Move the pointer over the light blue border and click.</span></span>

    <span data-ttu-id="43df2-230">![Mueva el puntero sobre el borde de color azul claro](visual-studio-2013-web-tools/_static/image14.png "mover el puntero sobre el borde de color azul claro")</span><span class="sxs-lookup"><span data-stu-id="43df2-230">![Moving the pointer over the light blue border](visual-studio-2013-web-tools/_static/image14.png "Moving the pointer over the light blue border")</span></span>

    <span data-ttu-id="43df2-231">*Mueva el puntero sobre el borde de color azul claro*</span><span class="sxs-lookup"><span data-stu-id="43df2-231">*Moving the pointer over the light blue border*</span></span>
3. <span data-ttu-id="43df2-232">Cambie a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="43df2-232">Switch back to Visual Studio.</span></span> <span data-ttu-id="43df2-233">Observe cómo el elemento HTML que seleccionó en el explorador también está seleccionado en el editor HTML de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="43df2-233">Notice how the HTML element that you selected in the browser is also selected in the Visual Studio HTML editor.</span></span>

    <span data-ttu-id="43df2-234">![Elemento HTML seleccionado en el editor HTML de Visual Studio](visual-studio-2013-web-tools/_static/image15.png "elemento HTML seleccionado en el editor HTML de Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="43df2-234">![HTML element selected in the Visual Studio HTML editor](visual-studio-2013-web-tools/_static/image15.png "HTML element selected in the Visual Studio HTML editor")</span></span>

    <span data-ttu-id="43df2-235">*Elemento HTML seleccionado en el editor HTML de Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="43df2-235">*HTML element selected in the Visual Studio HTML editor*</span></span>
4. <span data-ttu-id="43df2-236">A continuación, actualizaremos la **frontal** clase CSS para cambiar el estilo del elemento seleccionado.</span><span class="sxs-lookup"><span data-stu-id="43df2-236">You will now update the **front** CSS class in order to change the styling of the selected element.</span></span> <span data-ttu-id="43df2-237">Para ello, presione **CTRL** + **,** para abrir el **navegar a** cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="43df2-237">To do so, press **CTRL** + **,** to open the **Navigate To** search box.</span></span> <span data-ttu-id="43df2-238">Tipo de **site.css** y presione **ENTRAR** para abrir el archivo.</span><span class="sxs-lookup"><span data-stu-id="43df2-238">Type **site.css** and press **ENTER** to open the file.</span></span>

    <span data-ttu-id="43df2-239">![Abrir archivo Site.css](visual-studio-2013-web-tools/_static/image16.png "al abrir el archivo Site.css")</span><span class="sxs-lookup"><span data-stu-id="43df2-239">![Opening file Site.css](visual-studio-2013-web-tools/_static/image16.png "Opening file Site.css")</span></span>

    <span data-ttu-id="43df2-240">*Abrir el archivo Site.css*</span><span class="sxs-lookup"><span data-stu-id="43df2-240">*Opening file Site.css*</span></span>
5. <span data-ttu-id="43df2-241">Presione **CTRL** + **F** y tipo **.front .flip-Container** para encontrar el selector de CSS.</span><span class="sxs-lookup"><span data-stu-id="43df2-241">Press **CTRL** + **F** and type **.flip-containter .front** to find the CSS selector.</span></span>
6. <span data-ttu-id="43df2-242">Haga clic en el cuadrado azul claro en la propiedad de borde de la clase para abrir el selector de colores.</span><span class="sxs-lookup"><span data-stu-id="43df2-242">Click the light blue square in the border property of the class to open the Color Picker.</span></span>

    <span data-ttu-id="43df2-243">![Abrir el selector de colores](visual-studio-2013-web-tools/_static/image17.png "abrir el selector de colores")</span><span class="sxs-lookup"><span data-stu-id="43df2-243">![Opening the Color Picker](visual-studio-2013-web-tools/_static/image17.png "Opening the Color Picker")</span></span>

    <span data-ttu-id="43df2-244">*Abrir el selector de colores*</span><span class="sxs-lookup"><span data-stu-id="43df2-244">*Opening the Color Picker*</span></span>
7. <span data-ttu-id="43df2-245">Expanda el selector de colores, haga clic en el botón de contenido y seleccione un color nuevo.</span><span class="sxs-lookup"><span data-stu-id="43df2-245">Expand the Color Picker by clicking the chevron button and select a new color.</span></span>

    <span data-ttu-id="43df2-246">![Al expandir el selector de colores](visual-studio-2013-web-tools/_static/image18.png "expandiendo el selector de colores")</span><span class="sxs-lookup"><span data-stu-id="43df2-246">![Expanding the Color Picker](visual-studio-2013-web-tools/_static/image18.png "Expanding the Color Picker")</span></span>

    <span data-ttu-id="43df2-247">*Al expandir el selector de colores*</span><span class="sxs-lookup"><span data-stu-id="43df2-247">*Expanding the Color Picker*</span></span>
8. <span data-ttu-id="43df2-248">Presione **CTRL** + **ALT** + **ENTRAR** para actualizar los exploradores vinculados.</span><span class="sxs-lookup"><span data-stu-id="43df2-248">Press **CTRL** + **ALT** + **ENTER** to refresh linked browsers.</span></span>
9. <span data-ttu-id="43df2-249">Cambie a Internet Explorer y observe cómo ha cambiado el color del borde.</span><span class="sxs-lookup"><span data-stu-id="43df2-249">Switch to Internet Explorer and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="43df2-250">![Internet Explorer - actualizarlo el color del borde](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - actualizarlo el color del borde")</span><span class="sxs-lookup"><span data-stu-id="43df2-250">![Internet Explorer - Border color updated](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - Border color updated")</span></span>

    <span data-ttu-id="43df2-251">*Internet Explorer - actualizarlo el color del borde*</span><span class="sxs-lookup"><span data-stu-id="43df2-251">*Internet Explorer - Border color updated*</span></span>
10. <span data-ttu-id="43df2-252">Cambie a Google Chrome y observe cómo ha cambiado el color del borde.</span><span class="sxs-lookup"><span data-stu-id="43df2-252">Switch to Google Chrome and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="43df2-253">![Google Chrome - actualizarlo el color del borde](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - actualizarlo el color del borde")</span><span class="sxs-lookup"><span data-stu-id="43df2-253">![Google Chrome - Border color updated](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - Border color updated")</span></span>

    <span data-ttu-id="43df2-254">*Google Chrome - actualizarlo el color del borde*</span><span class="sxs-lookup"><span data-stu-id="43df2-254">*Google Chrome - Border color updated*</span></span>
11. <span data-ttu-id="43df2-255">Cambie a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="43df2-255">Switch back to Visual Studio.</span></span>
12. <span data-ttu-id="43df2-256">Vaya al final de la **Site.css** archivo y presione **CTRL** + **F** para buscar la **.btn** selector.</span><span class="sxs-lookup"><span data-stu-id="43df2-256">Go to the end of the **Site.css** file and press **CTRL** + **F** to locate the **.btn** selector.</span></span>
13. <span data-ttu-id="43df2-257">Tenga en cuenta que la **- webkit-border-radius** propiedad subrayada en verde.</span><span class="sxs-lookup"><span data-stu-id="43df2-257">Notice that the **-webkit-border-radius** property is underlined in green.</span></span>

    <span data-ttu-id="43df2-258">![propiedad - webkit-border-radius del botón selector de](visual-studio-2013-web-tools/_static/image21.png "- webkit-border-radius propiedad del selector de botón")</span><span class="sxs-lookup"><span data-stu-id="43df2-258">![-webkit-border-radius property of the btn selector](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius property of the btn selector")</span></span>

    <span data-ttu-id="43df2-259">*propiedad - webkit-border-radius del selector de botón*</span><span class="sxs-lookup"><span data-stu-id="43df2-259">*-webkit-border-radius property of the btn selector*</span></span>
14. <span data-ttu-id="43df2-260">Colocar el símbolo de intercalación en el **- webkit-border-radius** propiedad.</span><span class="sxs-lookup"><span data-stu-id="43df2-260">Place the caret in the **-webkit-border-radius** property.</span></span> <span data-ttu-id="43df2-261">Debe aparecer una línea azul debajo de la primera letra de la primera palabra de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="43df2-261">A blue line should appear under the first letter of the first word of the property.</span></span> <span data-ttu-id="43df2-262">Se trata de la **etiqueta inteligente**.</span><span class="sxs-lookup"><span data-stu-id="43df2-262">This is the **smart tag**.</span></span>
15. <span data-ttu-id="43df2-263">Presione **CTRL** + **.**</span><span class="sxs-lookup"><span data-stu-id="43df2-263">Press **CTRL** + **.**</span></span> <span data-ttu-id="43df2-264">Para abrir el menú de sugerencias y haga clic en **agregar falta la propiedad estándar (border-radius)**.</span><span class="sxs-lookup"><span data-stu-id="43df2-264">to open the suggestions menu and click **Add missing standard property (border-radius)**.</span></span>

    <span data-ttu-id="43df2-265">![Agregar falta sugerencia de propiedad estándar](visual-studio-2013-web-tools/_static/image22.png "agregar falta sugerencias de propiedades estándar")</span><span class="sxs-lookup"><span data-stu-id="43df2-265">![Add missing standard property suggestion](visual-studio-2013-web-tools/_static/image22.png "Add missing standard property suggestion")</span></span>

    <span data-ttu-id="43df2-266">*Agregar falta sugerencias de propiedades estándar*</span><span class="sxs-lookup"><span data-stu-id="43df2-266">*Add missing standard property suggestion*</span></span>
16. <span data-ttu-id="43df2-267">El **border-radius** propiedad se agrega automáticamente a la regla CSS.</span><span class="sxs-lookup"><span data-stu-id="43df2-267">The **border-radius** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="43df2-268">![Falta la propiedad estándar agregado](visual-studio-2013-web-tools/_static/image23.png "falta la propiedad estándar agregado")</span><span class="sxs-lookup"><span data-stu-id="43df2-268">![Missing standard property added](visual-studio-2013-web-tools/_static/image23.png "Missing standard property added")</span></span>

    <span data-ttu-id="43df2-269">*Falta la propiedad estándar agregado*</span><span class="sxs-lookup"><span data-stu-id="43df2-269">*Missing standard property added*</span></span>
17. <span data-ttu-id="43df2-270">Mueva el puntero sobre la **border-radius** propiedad para mostrar el **información sobre herramientas de explorador matriz**.</span><span class="sxs-lookup"><span data-stu-id="43df2-270">Move the pointer over the **border-radius** property to display the **Browser matrix tooltip**.</span></span> <span data-ttu-id="43df2-271">El **información sobre herramientas de explorador matriz** muestra la disponibilidad de la propiedad en cada explorador.</span><span class="sxs-lookup"><span data-stu-id="43df2-271">The **Browser matrix tooltip** shows the availability of the property in each browser.</span></span>

    <span data-ttu-id="43df2-272">![Información sobre herramientas de explorador matriz](visual-studio-2013-web-tools/_static/image24.png "información sobre herramientas de explorador matriz")</span><span class="sxs-lookup"><span data-stu-id="43df2-272">![Browser matrix tooltip](visual-studio-2013-web-tools/_static/image24.png "Browser matrix tooltip")</span></span>

    <span data-ttu-id="43df2-273">*Información sobre herramientas de explorador matriz*</span><span class="sxs-lookup"><span data-stu-id="43df2-273">*Browser matrix tooltip*</span></span>
18. <span data-ttu-id="43df2-274">Tenga en cuenta que el valor de la **border-radius** propiedad aparece subrayada todavía.</span><span class="sxs-lookup"><span data-stu-id="43df2-274">Notice that the value of the **border-radius** property is still underlined.</span></span> <span data-ttu-id="43df2-275">Mueva el puntero sobre el valor para ver el mensaje de advertencia.</span><span class="sxs-lookup"><span data-stu-id="43df2-275">Move the pointer over the value to see the warning message.</span></span>

    <span data-ttu-id="43df2-276">![Advertencia de valor de propiedad Border-radius](visual-studio-2013-web-tools/_static/image25.png "advertencia de valor de propiedad Border-radius")</span><span class="sxs-lookup"><span data-stu-id="43df2-276">![Border-radius property value warning](visual-studio-2013-web-tools/_static/image25.png "Border-radius property value warning")</span></span>

    <span data-ttu-id="43df2-277">*Advertencia de valor de propiedad de radio del borde*</span><span class="sxs-lookup"><span data-stu-id="43df2-277">*Border-radius property value warning*</span></span>
19. <span data-ttu-id="43df2-278">Quite la unidad de la **border-radius** valor de la propiedad tal como se sugiere la información sobre herramientas.</span><span class="sxs-lookup"><span data-stu-id="43df2-278">Remove the unit of the **border-radius** property value as suggested by the tooltip.</span></span>
20. <span data-ttu-id="43df2-279">Como **border-radius** es la propiedad estándar para definir cómo redondeado borde esquinas son, puede quitar el **- webkit-border-radius** propiedad y valor de la regla CSS.</span><span class="sxs-lookup"><span data-stu-id="43df2-279">As **border-radius** is the standard property for defining how rounded border corners are, you can remove the **-webkit-border-radius** property and value from the CSS rule.</span></span>
21. <span data-ttu-id="43df2-280">Colocar el símbolo de intercalación en el **ajuste** propiedad y observe que la etiqueta inteligente también aparece a continuación.</span><span class="sxs-lookup"><span data-stu-id="43df2-280">Place the caret in the **word-wrap** property and notice that the smart tag also appears below.</span></span>
22. <span data-ttu-id="43df2-281">Abra el menú y haga clic en **añadir falta información específica del proveedor**.</span><span class="sxs-lookup"><span data-stu-id="43df2-281">Open the menu and click **Add missing vendor specifics**.</span></span>

    <span data-ttu-id="43df2-282">![Agregar sugerencias de datos específicos de proveedor falta](visual-studio-2013-web-tools/_static/image26.png "agregar sugerencias de datos específicos de proveedor que faltan")</span><span class="sxs-lookup"><span data-stu-id="43df2-282">![Add missing vendor specifics suggestion](visual-studio-2013-web-tools/_static/image26.png "Add missing vendor specifics suggestion")</span></span>

    <span data-ttu-id="43df2-283">*Agregar sugerencias de datos específicos de proveedor que faltan*</span><span class="sxs-lookup"><span data-stu-id="43df2-283">*Add missing vendor specifics suggestion*</span></span>
23. <span data-ttu-id="43df2-284">El **-ms-word-wrap** propiedad se agrega automáticamente a la regla CSS.</span><span class="sxs-lookup"><span data-stu-id="43df2-284">The **-ms-word-wrap** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="43df2-285">![Propiedad específica de proveedor agregado](visual-studio-2013-web-tools/_static/image27.png "propiedad específica de proveedor agregado")</span><span class="sxs-lookup"><span data-stu-id="43df2-285">![Vendor specific property added](visual-studio-2013-web-tools/_static/image27.png "Vendor specific property added")</span></span>

    <span data-ttu-id="43df2-286">*Propiedad específica de proveedor agregado*</span><span class="sxs-lookup"><span data-stu-id="43df2-286">*Vendor specific property added*</span></span>

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a><span data-ttu-id="43df2-287">Tarea 4: actualizar el código HTML desde el explorador</span><span class="sxs-lookup"><span data-stu-id="43df2-287">Task 4 - Updating the HTML Code from the Browser</span></span>

<span data-ttu-id="43df2-288">En esta tarea, va a usar el vínculo de explorador **el modo de diseño** característica para editar el objeto DOM desde el explorador y transferir los cambios en el archivo de código fuente HTML en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="43df2-288">In this task, you will use the Browser Link's **Design Mode** feature to edit the DOM object from the browser and transfer the changes to the HTML source file in Visual Studio.</span></span>

1. <span data-ttu-id="43df2-289">En Google Chrome, presione **CTRL** + **ALT** + **d.** para habilitar el modo de diseño.</span><span class="sxs-lookup"><span data-stu-id="43df2-289">In Google Chrome, press **CTRL** + **ALT** + **D** to enable Design Mode.</span></span>
2. <span data-ttu-id="43df2-290">Mueva el puntero sobre la **Lorem Ipsum dolor sit AME** etiquetar y haga clic en.</span><span class="sxs-lookup"><span data-stu-id="43df2-290">Move the pointer over the **Lorem Ipsum dolor sit amet** label and click.</span></span>

    <span data-ttu-id="43df2-291">![Editar la pregunta](visual-studio-2013-web-tools/_static/image28.png "editar la pregunta")</span><span class="sxs-lookup"><span data-stu-id="43df2-291">![Editing the question](visual-studio-2013-web-tools/_static/image28.png "Editing the question")</span></span>

    <span data-ttu-id="43df2-292">*Edición de la pregunta*</span><span class="sxs-lookup"><span data-stu-id="43df2-292">*Editing the question*</span></span>
3. <span data-ttu-id="43df2-293">Debería aparecer un cursor.</span><span class="sxs-lookup"><span data-stu-id="43df2-293">A cursor should appear.</span></span> <span data-ttu-id="43df2-294">Reemplace el texto original con *lo que parece al igual que cuando escribe una pregunta más?*y, a continuación, presione **ESC** para salir del modo de diseño.</span><span class="sxs-lookup"><span data-stu-id="43df2-294">Replace the original text with *What does it look like when I write a longer question?*, and then press **ESC** to exit Design Mode.</span></span>

    <span data-ttu-id="43df2-295">![Pregunta editado](visual-studio-2013-web-tools/_static/image29.png "pregunta editado")</span><span class="sxs-lookup"><span data-stu-id="43df2-295">![Question edited](visual-studio-2013-web-tools/_static/image29.png "Question edited")</span></span>

    <span data-ttu-id="43df2-296">*Pregunta editado*</span><span class="sxs-lookup"><span data-stu-id="43df2-296">*Question edited*</span></span>
4. <span data-ttu-id="43df2-297">Vuelva a Visual Studio y abra switch **Index.cshtml**, si aún no está abierto.</span><span class="sxs-lookup"><span data-stu-id="43df2-297">Switch back to Visual Studio and open **Index.cshtml**, if not already opened.</span></span> <span data-ttu-id="43df2-298">Tenga en cuenta que el texto interno de la ** &lt;p&gt; ** se ha actualizado el elemento.</span><span class="sxs-lookup"><span data-stu-id="43df2-298">Notice that the inner text of the **&lt;p&gt;** element has been updated.</span></span>

    <span data-ttu-id="43df2-299">![Pregunta de actualizados en la página HTML](visual-studio-2013-web-tools/_static/image30.png "pregunta actualizados en la página HTML")</span><span class="sxs-lookup"><span data-stu-id="43df2-299">![Updated question in the HTML page](visual-studio-2013-web-tools/_static/image30.png "Updated question in the HTML page")</span></span>

    <span data-ttu-id="43df2-300">*Pregunta actualizada en la página HTML*</span><span class="sxs-lookup"><span data-stu-id="43df2-300">*Updated question in the HTML page*</span></span>

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a><span data-ttu-id="43df2-301">Advertencias relacionadas con la tarea 5: revisión SEO</span><span class="sxs-lookup"><span data-stu-id="43df2-301">Task 5 - Reviewing SEO Related Warnings</span></span>

<span data-ttu-id="43df2-302">**Optimización de motor de búsqueda** (SEO) es el proceso de realizar un rango de sitio Web superior en la lista de un motor de búsqueda de resultados.</span><span class="sxs-lookup"><span data-stu-id="43df2-302">**Search Engine Optimization** (SEO) is the process of making a website rank higher on a search engine's list of results.</span></span> <span data-ttu-id="43df2-303">Cuanto mayor sea el sitio clasifica y más coherente aparece, los visitantes más obtendrán el sitio de ese motor de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="43df2-303">The higher the site ranks and the more consistently it is listed, the more visitors the site will get from that search engine.</span></span> <span data-ttu-id="43df2-304">Web Essentials incorpora una herramienta de análisis que examina HTML, los problemas de informes encontró y proporciona ayuda para corregirlas.</span><span class="sxs-lookup"><span data-stu-id="43df2-304">Web Essentials incorporates an analytical tool that examines HTML, reports the issues found and provides assistance to fix them.</span></span>

1. <span data-ttu-id="43df2-305">Vaya a la **vista** menú y haga clic en **lista de errores** para abrir el **lista de errores** ventana.</span><span class="sxs-lookup"><span data-stu-id="43df2-305">Go to the **View** menu and click **Error List** to open the **Error List** window.</span></span>

    <span data-ttu-id="43df2-306">![Lista de errores en la vista menú](visual-studio-2013-web-tools/_static/image31.png "lista de errores en el menú Ver")</span><span class="sxs-lookup"><span data-stu-id="43df2-306">![Error List in View menu](visual-studio-2013-web-tools/_static/image31.png "Error List in View menu")</span></span>

    <span data-ttu-id="43df2-307">*Lista de errores en la vista menú*</span><span class="sxs-lookup"><span data-stu-id="43df2-307">*Error List in View menu*</span></span>
2. <span data-ttu-id="43df2-308">Observe que hay una advertencia de SEO le notifica que un ** &lt;meta&gt; ** etiqueta para la descripción de la página falta.</span><span class="sxs-lookup"><span data-stu-id="43df2-308">Notice that there is an SEO warning notifying that a **&lt;meta&gt;** tag for the page description is missing.</span></span> <span data-ttu-id="43df2-309">Haga doble clic en la entrada de advertencia de SEO para corregirlo.</span><span class="sxs-lookup"><span data-stu-id="43df2-309">Double-click the SEO warning entry to fix it.</span></span>

    <span data-ttu-id="43df2-310">![Ventana Lista de errores](visual-studio-2013-web-tools/_static/image32.png "ventana Lista de errores")</span><span class="sxs-lookup"><span data-stu-id="43df2-310">![Error List window](visual-studio-2013-web-tools/_static/image32.png "Error List window")</span></span>

    <span data-ttu-id="43df2-311">*Ventana Lista de errores*</span><span class="sxs-lookup"><span data-stu-id="43df2-311">*Error List window*</span></span>
3. <span data-ttu-id="43df2-312">En el **Web Essentials** cuadro de diálogo, haga clic en **Sí** para insertar una descripción &lt;meta&gt; etiqueta.</span><span class="sxs-lookup"><span data-stu-id="43df2-312">In the **Web Essentials** dialog box, click **Yes** to insert a description &lt;meta&gt; tag.</span></span>

    <span data-ttu-id="43df2-313">![Cuadro de diálogo de Web Essentials](visual-studio-2013-web-tools/_static/image33.png "cuadro de diálogo Web Essentials")</span><span class="sxs-lookup"><span data-stu-id="43df2-313">![Web Essentials dialog box](visual-studio-2013-web-tools/_static/image33.png "Web Essentials dialog box")</span></span>

    <span data-ttu-id="43df2-314">*Cuadro de diálogo de Web Essentials*</span><span class="sxs-lookup"><span data-stu-id="43df2-314">*Web Essentials dialog box*</span></span>
4. <span data-ttu-id="43df2-315">El editor para ** \_Layout.cshtml** abre y ** &lt;meta&gt; ** etiqueta se agrega automáticamente a la **head** sección de la Archivo HTML.</span><span class="sxs-lookup"><span data-stu-id="43df2-315">The editor for **\_Layout.cshtml** opens and the **&lt;meta&gt;** tag is automatically added to the **head** section of the HTML file.</span></span>

    <span data-ttu-id="43df2-316">![Etiqueta META que agrega automáticamente en la página de _Layout](visual-studio-2013-web-tools/_static/image34.png "Meta etiqueta agregado automáticamente en la página de _Layout")</span><span class="sxs-lookup"><span data-stu-id="43df2-316">![Meta tag automatically added in _Layout page](visual-studio-2013-web-tools/_static/image34.png "Meta tag automatically added in _Layout page")</span></span>

    <span data-ttu-id="43df2-317">*Etiqueta META que agrega automáticamente a \_página de diseño*</span><span class="sxs-lookup"><span data-stu-id="43df2-317">*Meta tag automatically added to \_Layout page*</span></span>
5. <span data-ttu-id="43df2-318">Cambie el valor de la **contenido** atribuir a *GeekQuiz* y guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="43df2-318">Change the value of the **content** attribute to *GeekQuiz* and save the file.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a><span data-ttu-id="43df2-319">Ejercicio 2: Sacar partido de fragmentos de código y de IntelliSense</span><span class="sxs-lookup"><span data-stu-id="43df2-319">Exercise 2: Taking Advantage of Code Snippets and IntelliSense</span></span>

<span data-ttu-id="43df2-320">Con Essentials de Web, el editor HTML se ha ampliado con una funcionalidad adicional.</span><span class="sxs-lookup"><span data-stu-id="43df2-320">With Web Essentials, the HTML editor has been extended with extra functionality.</span></span> <span data-ttu-id="43df2-321">En este ejercicio, verá algunas características nuevas que resultan útiles al desarrollar aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="43df2-321">In this exercise, you will see some new features that are helpful when developing web applications.</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a><span data-ttu-id="43df2-322">Tarea 1: utilizar IntelliSense en documentos HTML</span><span class="sxs-lookup"><span data-stu-id="43df2-322">Task 1 - Using IntelliSense in HTML Documents</span></span>

<span data-ttu-id="43df2-323">La primera función nueva que aparece en esta tarea se llama **IntelliSense dinámico**.</span><span class="sxs-lookup"><span data-stu-id="43df2-323">The first new feature you will see in this task is called **Dynamic IntelliSense**.</span></span> <span data-ttu-id="43df2-324">IntelliSense dinámica lee otras etiquetas y atributos para deducir los posibles identificadores que va a usar.</span><span class="sxs-lookup"><span data-stu-id="43df2-324">Dynamic IntelliSense reads other tags and attributes to infer the possible ids you will use.</span></span>

<span data-ttu-id="43df2-325">En esta tarea, creará un nuevo elemento de formulario HTML que contiene una etiqueta y un campo de entrada.</span><span class="sxs-lookup"><span data-stu-id="43df2-325">In this task, you will create a new HTML form element which contains a label and an input field.</span></span> <span data-ttu-id="43df2-326">A continuación, agregará un **para** de atributo a la etiqueta y enlazarla a la entrada, y verá sugerencias de IntelliSense en función de los identificadores de las entradas en el ámbito.</span><span class="sxs-lookup"><span data-stu-id="43df2-326">Then you will add a **for** attribute to the label to bind it to the input, and you will see IntelliSense suggestions based on the ids of the inputs in scope.</span></span>

1. <span data-ttu-id="43df2-327">Abra **Visual Studio Express 2013 para Web** y **Begin.sln** soluciones se encuentran en la **origen/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** carpeta.</span><span class="sxs-lookup"><span data-stu-id="43df2-327">Open **Visual Studio Express 2013 for Web** and the **Begin.sln** solution located in the **Source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** folder.</span></span> <span data-ttu-id="43df2-328">Como alternativa, puede continuar con la solución que obtuvo en el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="43df2-328">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="43df2-329">En **el Explorador de soluciones**, abra el **Index.cshtml** archivo se encuentra en la **vistas** | **inicio** carpeta.</span><span class="sxs-lookup"><span data-stu-id="43df2-329">In **Solution Explorer**, open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="43df2-330">Agregue el siguiente formulario dentro de la ** &lt;sección&gt; ** elemento.</span><span class="sxs-lookup"><span data-stu-id="43df2-330">Add the following form inside the **&lt;section&gt;** element.</span></span>

    <span data-ttu-id="43df2-331">(Código de fragmento de código: *VisualStudio2013WebTooling* - *Ex2* - *formulario*)</span><span class="sxs-lookup"><span data-stu-id="43df2-331">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *Form*)</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. <span data-ttu-id="43df2-332">La etiqueta de entrada debe estar precedida de una etiqueta con una descripción del campo.</span><span class="sxs-lookup"><span data-stu-id="43df2-332">The input tag should be preceded by a label with some description of the field.</span></span> <span data-ttu-id="43df2-333">Agregue la etiqueta siguiente antes de la etiqueta de entrada.</span><span class="sxs-lookup"><span data-stu-id="43df2-333">Add the following label before the input tag.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. <span data-ttu-id="43df2-334">El **para** atributo de un ** &lt;etiqueta&gt; ** especifica qué elemento form con una etiqueta que está enlazado.</span><span class="sxs-lookup"><span data-stu-id="43df2-334">The **for** attribute of a **&lt;label&gt;** specifies which form element a label is bound to.</span></span> <span data-ttu-id="43df2-335">El valor del atributo debe ser igual que el identificador del elemento relacionado.</span><span class="sxs-lookup"><span data-stu-id="43df2-335">The attribute's value should be equal to the id of the related element.</span></span> <span data-ttu-id="43df2-336">Agregar el **para** atribuir a la ** &lt;etiqueta&gt; ** elemento.</span><span class="sxs-lookup"><span data-stu-id="43df2-336">Add the **for** attribute to the **&lt;label&gt;** element.</span></span> <span data-ttu-id="43df2-337">Como se muestra en la ilustración siguiente, la &quot;nombre&quot; valor aparece en el cuadro de IntelliSense, basándose en el identificador de los elementos dentro del mismo ámbito (incluye ** &lt;formulario&gt;**).</span><span class="sxs-lookup"><span data-stu-id="43df2-337">As shown in the following figure, the &quot;name&quot; value pops up in the IntelliSense box, based on the id of the elements within the same scope (the enclosing **&lt;form&gt;**).</span></span>

    <span data-ttu-id="43df2-338">![Que muestra el Id. de IntelliSense](visual-studio-2013-web-tools/_static/image35.png "que muestra el Id. de IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="43df2-338">![Showing the id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Showing the id in IntelliSense")</span></span>

    <span data-ttu-id="43df2-339">*Que muestra el Id. de IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="43df2-339">*Showing the id in IntelliSense*</span></span>
6. <span data-ttu-id="43df2-340">Eliminar agregadas recientemente ** &lt;formulario&gt; ** elemento y su contenido.</span><span class="sxs-lookup"><span data-stu-id="43df2-340">Delete the recently added **&lt;form&gt;** element and its content.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a><span data-ttu-id="43df2-341">Tarea 2: uso de fragmentos de código de HTML</span><span class="sxs-lookup"><span data-stu-id="43df2-341">Task 2 - Using HTML Code Snippets</span></span>

<span data-ttu-id="43df2-342">HTML5 introdujo más de 25 etiquetas semánticas de nuevo.</span><span class="sxs-lookup"><span data-stu-id="43df2-342">HTML5 introduced more than 25 new semantic tags.</span></span> <span data-ttu-id="43df2-343">Visual Studio ya tenía compatibilidad con IntelliSense para estas etiquetas, pero Visual Studio 2013 hace que sea más rápida y más fácil de escribir marcado mediante la adición de nuevos fragmentos de código.</span><span class="sxs-lookup"><span data-stu-id="43df2-343">Visual Studio already had IntelliSense support for these tags, but Visual Studio 2013 makes it faster and easier to write markup by adding new code snippets.</span></span> <span data-ttu-id="43df2-344">Aunque estas etiquetas no son complejas, vienen con unos matices pequeños, como la adición de las reservas de códec correcto para el *audio* etiqueta.</span><span class="sxs-lookup"><span data-stu-id="43df2-344">Though these tags are not complicated, they come with a few small subtleties, such as adding the correct codec fallbacks for the *audio* tag.</span></span> <span data-ttu-id="43df2-345">En esta tarea, verá los fragmentos de código HTML para la etiqueta de audio.</span><span class="sxs-lookup"><span data-stu-id="43df2-345">In this task, you will see the HTML code snippets for the audio tag.</span></span>

1. <span data-ttu-id="43df2-346">En el **Index.cshtml** de archivo, escriba ** &lt;aud** dentro de la ** &lt;sección&gt; ** elemento tal como se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="43df2-346">In the **Index.cshtml** file, type **&lt;aud** inside the **&lt;section&gt;** element as shown in the following figure.</span></span>

    <span data-ttu-id="43df2-347">![Inserción de un elemento audio](visual-studio-2013-web-tools/_static/image36.png "insertar un elemento de audio")</span><span class="sxs-lookup"><span data-stu-id="43df2-347">![Inserting an audio element](visual-studio-2013-web-tools/_static/image36.png "Inserting an audio element")</span></span>

    <span data-ttu-id="43df2-348">*Insertar un elemento de audio*</span><span class="sxs-lookup"><span data-stu-id="43df2-348">*Inserting an audio element*</span></span>
2. <span data-ttu-id="43df2-349">Presione **ficha** dos veces y observe cómo se agrega el código siguiente en la página y el cursor se coloca en el **src** atributo del primer origen.</span><span class="sxs-lookup"><span data-stu-id="43df2-349">Press **TAB** twice and notice how the following code is added on the page and the cursor is placed on the **src** attribute of the first source.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > <span data-ttu-id="43df2-350">Presionando el **ficha** clave dos veces, el fragmento de código se inserta.</span><span class="sxs-lookup"><span data-stu-id="43df2-350">By pressing the **TAB** key twice, the code snippet is inserted.</span></span> <span data-ttu-id="43df2-351">El fragmento de audio muestra el uso estándar de la *audio* etiqueta, con dos archivos de origen para la compatibilidad mejorada.</span><span class="sxs-lookup"><span data-stu-id="43df2-351">The audio snippet shows the standard usage of the *audio* tag, with two source files for improved support.</span></span>
3. <span data-ttu-id="43df2-352">Elimine la segunda línea y actualizar el origen de la primera línea con el siguiente vínculo para la presentación de WebCampsTV Katana: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span><span class="sxs-lookup"><span data-stu-id="43df2-352">Delete the second line and update the source of the first line with the following link to the WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span></span> <span data-ttu-id="43df2-353">El código resultante se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="43df2-353">The resulting code is shown below.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="43df2-354">El archivo de origen *KatanaProject.mp3* se utiliza como ejemplo.</span><span class="sxs-lookup"><span data-stu-id="43df2-354">The source file *KatanaProject.mp3* is used as an example.</span></span> <span data-ttu-id="43df2-355">Puede usar otro origen, si lo prefiere.</span><span class="sxs-lookup"><span data-stu-id="43df2-355">You can use another source if you prefer.</span></span>
4. <span data-ttu-id="43df2-356">Presione **CTRL** + **S** para guardar el archivo.</span><span class="sxs-lookup"><span data-stu-id="43df2-356">Press **CTRL** + **S** to save the file.</span></span>
5. <span data-ttu-id="43df2-357">Presione **CTRL** + **F5** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="43df2-357">Press **CTRL** + **F5** to start the application.</span></span>
6. <span data-ttu-id="43df2-358">Tenga en cuenta que un Reproductor de audio se agregó a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="43df2-358">Notice that an audio player was added to the application.</span></span>

    <span data-ttu-id="43df2-359">![Reproductor de audio en Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Reproductor de Audio en Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="43df2-359">![Audio player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player in Internet Explorer")</span></span>

    <span data-ttu-id="43df2-360">*Reproductor de audio en Internet Explorer*</span><span class="sxs-lookup"><span data-stu-id="43df2-360">*Audio player in Internet Explorer*</span></span>

    <span data-ttu-id="43df2-361">![Reproductor de audio en Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Reproductor de Audio en Google Chrome")</span><span class="sxs-lookup"><span data-stu-id="43df2-361">![Audio player in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio player in Google Chrome")</span></span>

    <span data-ttu-id="43df2-362">*Reproductor de audio en Google Chrome*</span><span class="sxs-lookup"><span data-stu-id="43df2-362">*Audio player in Google Chrome*</span></span>
7. <span data-ttu-id="43df2-363">No cierre los exploradores.</span><span class="sxs-lookup"><span data-stu-id="43df2-363">Do not close the browsers.</span></span> <span data-ttu-id="43df2-364">Los usará en la siguiente tarea.</span><span class="sxs-lookup"><span data-stu-id="43df2-364">You will use them in the next task.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a><span data-ttu-id="43df2-365">Tarea 3: utilizar IntelliSense en documentos de JavaScript</span><span class="sxs-lookup"><span data-stu-id="43df2-365">Task 3 - Using IntelliSense in JavaScript Documents</span></span>

<span data-ttu-id="43df2-366">Con Web Essentials 2013, hojas de estilos y páginas HTML generan una lista de identificadores y nombres de clase.</span><span class="sxs-lookup"><span data-stu-id="43df2-366">With Web Essentials 2013, style sheets and HTML pages produce a list of IDs and class names.</span></span> <span data-ttu-id="43df2-367">En esta tarea, obtendrá información sobre cómo mejoran la compatibilidad con IntelliSense para JavaScript en Web Essentials 2013 esas listas.</span><span class="sxs-lookup"><span data-stu-id="43df2-367">In this task, you will learn how those lists improve JavaScript IntelliSense support in Web Essentials 2013.</span></span>

1. <span data-ttu-id="43df2-368">En el **Index.cshtml** de archivos, agregue el código siguiente para definir un **script** etiqueta para el código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="43df2-368">In the **Index.cshtml** file, add the following code to define a **script** tag for JavaScript code.</span></span>

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. <span data-ttu-id="43df2-369">Agregue el código siguiente dentro de la **script** etiqueta para definir la función de devolución de llamada listo.</span><span class="sxs-lookup"><span data-stu-id="43df2-369">Add the following code inside the **script** tag to define the ready callback function.</span></span>

    <span data-ttu-id="43df2-370">(Código de fragmento de código: *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span><span class="sxs-lookup"><span data-stu-id="43df2-370">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. <span data-ttu-id="43df2-371">Colocar el símbolo de intercalación en el **script** etiqueta y presione **CTRL** + **.**</span><span class="sxs-lookup"><span data-stu-id="43df2-371">Place the caret in the **script** tag and press **CTRL** + **.**</span></span> <span data-ttu-id="43df2-372">Para abrir el menú de sugerencias.</span><span class="sxs-lookup"><span data-stu-id="43df2-372">to open the suggestion menu.</span></span>
4. <span data-ttu-id="43df2-373">Haga clic en **extraer al archivo**.</span><span class="sxs-lookup"><span data-stu-id="43df2-373">Click **Extract To File**.</span></span>

    <span data-ttu-id="43df2-374">![Extraer JavaScript para sugerencias de archivo](visual-studio-2013-web-tools/_static/image39.png "JavaScript extraer a sugerencias de archivo")</span><span class="sxs-lookup"><span data-stu-id="43df2-374">![JavaScript extract to file suggestion](visual-studio-2013-web-tools/_static/image39.png "JavaScript extract to file suggestion")</span></span>

    <span data-ttu-id="43df2-375">*Extraer JavaScript para sugerencias de archivo*</span><span class="sxs-lookup"><span data-stu-id="43df2-375">*JavaScript extract to file suggestion*</span></span>
5. <span data-ttu-id="43df2-376">En el **Guardar como** ventana, seleccione la **Scripts** carpeta, asigne nombre al archivo **init.js** y haga clic en **guardar**.</span><span class="sxs-lookup"><span data-stu-id="43df2-376">In the **Save As** window, select the **Scripts** folder, name the file **init.js** and click **Save**.</span></span>

    <span data-ttu-id="43df2-377">![Guardar como ventana](visual-studio-2013-web-tools/_static/image40.png "ventana Guardar como")</span><span class="sxs-lookup"><span data-stu-id="43df2-377">![Save As window](visual-studio-2013-web-tools/_static/image40.png "Save As window")</span></span>

    <span data-ttu-id="43df2-378">*Guardar como ventana*</span><span class="sxs-lookup"><span data-stu-id="43df2-378">*Save As window*</span></span>

    > [!NOTE]
    > <span data-ttu-id="43df2-379">El **init.js** archivo se crea y se mueve el contenido de la secuencia de comandos al archivo.</span><span class="sxs-lookup"><span data-stu-id="43df2-379">The **init.js** file is created and the content of the script is moved to the file.</span></span>
    > 
    > <span data-ttu-id="43df2-380">![Archivo de init.js creado con el contenido incluido](visual-studio-2013-web-tools/_static/image41.png "archivo Init.js creado con el contenido incluido")</span><span class="sxs-lookup"><span data-stu-id="43df2-380">![Init.js file created with the content included](visual-studio-2013-web-tools/_static/image41.png "Init.js file created with the content included")</span></span>
    > 
    > <span data-ttu-id="43df2-381">*Archivo de init.js creado con el contenido incluido*</span><span class="sxs-lookup"><span data-stu-id="43df2-381">*Init.js file created with the content included*</span></span>
6. <span data-ttu-id="43df2-382">Abra la **Index.cshtml** de archivo y compruebe que la etiqueta de script se ha reemplazado por una referencia a la **init.js** archivo.</span><span class="sxs-lookup"><span data-stu-id="43df2-382">Open the **Index.cshtml** file and check that the script tag was replaced with a reference to the **init.js** file.</span></span>

    <span data-ttu-id="43df2-383">![Referencia de html init.js](visual-studio-2013-web-tools/_static/image42.png "Init.js referencia de html")</span><span class="sxs-lookup"><span data-stu-id="43df2-383">![Init.js html reference](visual-studio-2013-web-tools/_static/image42.png "Init.js html reference")</span></span>

    <span data-ttu-id="43df2-384">*Referencia de html init.js*</span><span class="sxs-lookup"><span data-stu-id="43df2-384">*Init.js html reference*</span></span>
7. <span data-ttu-id="43df2-385">Vaya a la **el Explorador de soluciones** y tenga en cuenta que la **init.js** archivo se incluye automáticamente en la solución.</span><span class="sxs-lookup"><span data-stu-id="43df2-385">Go to the **Solution Explorer** and notice that the **init.js** file was included automatically in the solution.</span></span>

    <span data-ttu-id="43df2-386">![Archivo de init.js incluido en la solución](visual-studio-2013-web-tools/_static/image43.png "archivo Init.js incluido en la solución")</span><span class="sxs-lookup"><span data-stu-id="43df2-386">![Init.js file included in solution](visual-studio-2013-web-tools/_static/image43.png "Init.js file included in solution")</span></span>

    <span data-ttu-id="43df2-387">*Archivo de init.js incluido en la solución*</span><span class="sxs-lookup"><span data-stu-id="43df2-387">*Init.js file included in solution*</span></span>
8. <span data-ttu-id="43df2-388">Vuelva a la **init.js** archivo para actualizar la **listo** función de devolución de llamada.</span><span class="sxs-lookup"><span data-stu-id="43df2-388">Switch back to the **init.js** file to update the **ready** function callback.</span></span>
9. <span data-ttu-id="43df2-389">Dentro de la definición de devolución de llamada de función que se pasa a *listo*, agregue el código siguiente para obtener todos los elementos mediante un atributo de clase específica.</span><span class="sxs-lookup"><span data-stu-id="43df2-389">Inside the function callback definition that is passed to *ready*, add the following code to get all the elements by a specific class attribute.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. <span data-ttu-id="43df2-390">Presione **CTRL** + **espacio** entre las comillas dentro de la **getElementsByClassName** llamada de función.</span><span class="sxs-lookup"><span data-stu-id="43df2-390">Press **CTRL** + **Space** between the quotes inside the **getElementsByClassName** function call.</span></span>

    <span data-ttu-id="43df2-391">![Con IntelliSense para la función getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "IntelliSense que muestra para la función getElementsByClassName")</span><span class="sxs-lookup"><span data-stu-id="43df2-391">![Showing IntelliSense for the getElementsByClassName function](visual-studio-2013-web-tools/_static/image44.png "Showing IntelliSense for the getElementsByClassName function")</span></span>

    <span data-ttu-id="43df2-392">*IntelliSense que muestra para la función getElementsByClassName*</span><span class="sxs-lookup"><span data-stu-id="43df2-392">*Showing IntelliSense for the getElementsByClassName function*</span></span>

    > [!NOTE]
    > <span data-ttu-id="43df2-393">Tenga en cuenta que IntelliSense muestra las clases definidas en las hojas de estilos del proyecto.</span><span class="sxs-lookup"><span data-stu-id="43df2-393">Notice that IntelliSense shows the classes defined in the project style sheets.</span></span>
11. <span data-ttu-id="43df2-394">Reemplace la línea que ha creado con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="43df2-394">Replace the line that you have created with the following code.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. <span data-ttu-id="43df2-395">Coloque el cursor después de **au** dentro de las comillas en el **getElementsByTagName** función del sistema y presione **CTRL** + **espacio**.</span><span class="sxs-lookup"><span data-stu-id="43df2-395">Position the cursor after **au** inside the quotes in the **getElementsByTagName** function and press **CTRL** + **Space**.</span></span>

    <span data-ttu-id="43df2-396">![Con IntelliSense para el método getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "que muestra IntelliSense para el método getElementByTagName")</span><span class="sxs-lookup"><span data-stu-id="43df2-396">![Showing IntelliSense for the getElementByTagName method](visual-studio-2013-web-tools/_static/image45.png "Showing IntelliSense for the getElementByTagName method")</span></span>

    <span data-ttu-id="43df2-397">*Que muestra IntelliSense para el método getElementsByTagName*</span><span class="sxs-lookup"><span data-stu-id="43df2-397">*Showing IntelliSense for the getElementsByTagName method*</span></span>
13. <span data-ttu-id="43df2-398">Seleccione ** &quot;audio&quot; ** en la lista y presione **ENTRAR**.</span><span class="sxs-lookup"><span data-stu-id="43df2-398">Select **&quot;audio&quot;** from the list and press **ENTER**.</span></span> <span data-ttu-id="43df2-399">El resultado se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="43df2-399">The result is shown in the following figure.</span></span>

    <span data-ttu-id="43df2-400">![Recuperar los elementos de Audio](visual-studio-2013-web-tools/_static/image46.png "recuperar los elementos de Audio")</span><span class="sxs-lookup"><span data-stu-id="43df2-400">![Retrieving Audio Elements](visual-studio-2013-web-tools/_static/image46.png "Retrieving Audio Elements")</span></span>

    <span data-ttu-id="43df2-401">*Recuperar los elementos de Audio*</span><span class="sxs-lookup"><span data-stu-id="43df2-401">*Retrieving Audio Elements*</span></span>
14. <span data-ttu-id="43df2-402">En **el Explorador de soluciones**, haga clic en el **init.js** un archivo en el **Scripts** carpeta y seleccione **archivos JavaScript Minificar** desde el **Web Essentials** menú.</span><span class="sxs-lookup"><span data-stu-id="43df2-402">In **Solution Explorer**, right-click the **init.js** file in the **Scripts** folder and select **Minify JavaScript file(s)** from the **Web Essentials** menu.</span></span>

    <span data-ttu-id="43df2-403">![Los archivos JavaScript de minificar](visual-studio-2013-web-tools/_static/image47.png "archivos JavaScript Minificar")</span><span class="sxs-lookup"><span data-stu-id="43df2-403">![Minify JavaScript file(s)](visual-studio-2013-web-tools/_static/image47.png "Minify JavaScript files")</span></span>

    <span data-ttu-id="43df2-404">*Minificar los archivos de JavaScript*</span><span class="sxs-lookup"><span data-stu-id="43df2-404">*Minify JavaScript file(s)*</span></span>
15. <span data-ttu-id="43df2-405">Cuando se le solicita que habilite la reducción automática cuando cambia el archivo de origen, haga clic en **Sí**.</span><span class="sxs-lookup"><span data-stu-id="43df2-405">When prompted to enable automatic minification when the source file changes click **Yes**.</span></span>

    <span data-ttu-id="43df2-406">![Habilitar la advertencia de reducción automática](visual-studio-2013-web-tools/_static/image48.png "habilitar la advertencia de reducción automática")</span><span class="sxs-lookup"><span data-stu-id="43df2-406">![Enabling automatic minification warning](visual-studio-2013-web-tools/_static/image48.png "Enabling automatic minification warning")</span></span>

    <span data-ttu-id="43df2-407">*Habilitar la advertencia de reducción automática*</span><span class="sxs-lookup"><span data-stu-id="43df2-407">*Enabling automatic minification warning*</span></span>

    > [!NOTE]
    > <span data-ttu-id="43df2-408">El **init.min.js** se crea y se agrega como una dependencia de la **init.js** archivo.</span><span class="sxs-lookup"><span data-stu-id="43df2-408">The **init.min.js** is created and is added as a dependency of the **init.js** file.</span></span>
    > 
    > <span data-ttu-id="43df2-409">![Archivo init.min.js creado](visual-studio-2013-web-tools/_static/image49.png "archivo Init.min.js creado")</span><span class="sxs-lookup"><span data-stu-id="43df2-409">![Init.min.js file created](visual-studio-2013-web-tools/_static/image49.png "Init.min.js file created")</span></span>
    > 
    > <span data-ttu-id="43df2-410">*Archivo init.min.js creado*</span><span class="sxs-lookup"><span data-stu-id="43df2-410">*Init.min.js file created*</span></span>
16. <span data-ttu-id="43df2-411">Abra la **init.min.js** de archivos y observe que el archivo se reduce.</span><span class="sxs-lookup"><span data-stu-id="43df2-411">Open the **init.min.js** file and notice that the file is minified.</span></span>

    <span data-ttu-id="43df2-412">![Contenido del archivo init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js contenido del archivo")</span><span class="sxs-lookup"><span data-stu-id="43df2-412">![Init.min.js file content](visual-studio-2013-web-tools/_static/image50.png "Init.min.js file content")</span></span>

    <span data-ttu-id="43df2-413">*Contenido del archivo init.min.js*</span><span class="sxs-lookup"><span data-stu-id="43df2-413">*Init.min.js file content*</span></span>
17. <span data-ttu-id="43df2-414">En el **init.js** , agregue el código siguiente la **getElementsByTagName** llamada para reproducir todos los elementos de audio de función.</span><span class="sxs-lookup"><span data-stu-id="43df2-414">In the **init.js** file, add the following code below the **getElementsByTagName** function call to play all the audio elements.</span></span>

    <span data-ttu-id="43df2-415">(Código de fragmento de código: *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span><span class="sxs-lookup"><span data-stu-id="43df2-415">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span></span>

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. <span data-ttu-id="43df2-416">Haga clic en **CTRL** + **S** para guardar el archivo.</span><span class="sxs-lookup"><span data-stu-id="43df2-416">Click **CTRL** + **S** to save the file.</span></span> <span data-ttu-id="43df2-417">Puesto que ya está abierto el archivo reducido, verá un cuadro de diálogo que dice que el archivo se ha modificado fuera del editor de origen.</span><span class="sxs-lookup"><span data-stu-id="43df2-417">Since the minified file is already opened, you will see a dialog box saying that the file was modified outside of the source editor.</span></span> <span data-ttu-id="43df2-418">Haga clic en **Sí**.</span><span class="sxs-lookup"><span data-stu-id="43df2-418">Click **Yes**.</span></span>

    <span data-ttu-id="43df2-419">![Advertencia de Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "advertencia de Microsoft Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="43df2-419">![Microsoft Visual Studio warning](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="43df2-420">*Advertencia de Microsoft Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="43df2-420">*Microsoft Visual Studio warning*</span></span>
19. <span data-ttu-id="43df2-421">Vuelva a la **init.min.js** archivo para comprobar que el archivo se ha actualizado con el nuevo código.</span><span class="sxs-lookup"><span data-stu-id="43df2-421">Switch back to the **init.min.js** file to verify that the file was updated with the new code.</span></span>

    <span data-ttu-id="43df2-422">![Actualizado el archivo init.min.js](visual-studio-2013-web-tools/_static/image52.png "archivo Init.min.js actualizado")</span><span class="sxs-lookup"><span data-stu-id="43df2-422">![Init.min.js file updated](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file updated")</span></span>

    <span data-ttu-id="43df2-423">*Archivo init.min.js actualizado*</span><span class="sxs-lookup"><span data-stu-id="43df2-423">*Init.min.js file updated*</span></span>
20. <span data-ttu-id="43df2-424">Haga clic en el **actualizar de vínculo de explorador** botón.</span><span class="sxs-lookup"><span data-stu-id="43df2-424">Click the **Browser Link Refresh** button.</span></span>
21. <span data-ttu-id="43df2-425">Una vez que se actualizan dos exploradores los reproductores de audio que vio en la tarea anterior empezará a reproducirse automáticamente.</span><span class="sxs-lookup"><span data-stu-id="43df2-425">Once both browsers are refreshed the audio players you saw in the previous task will start playing automatically.</span></span>

    <span data-ttu-id="43df2-426">![Reproductor de audio incluido en la vista](visual-studio-2013-web-tools/_static/image53.png "incluido en la vista de Reproductor de Audio")</span><span class="sxs-lookup"><span data-stu-id="43df2-426">![Audio player included in view](visual-studio-2013-web-tools/_static/image53.png "Audio player included in view")</span></span>

    <span data-ttu-id="43df2-427">*Reproductor de audio incluido en la vista*</span><span class="sxs-lookup"><span data-stu-id="43df2-427">*Audio player included in view*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="43df2-428">Resumen</span><span class="sxs-lookup"><span data-stu-id="43df2-428">Summary</span></span>

<span data-ttu-id="43df2-429">Al completar este laboratorio práctico ha aprendido cómo:</span><span class="sxs-lookup"><span data-stu-id="43df2-429">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="43df2-430">Usar las nuevas características del editor HTML Web Essentials incluidas como fragmentos de código de HTML5 enriquecidos y codificación Zen</span><span class="sxs-lookup"><span data-stu-id="43df2-430">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="43df2-431">Usar las nuevas características del editor de CSS Web Essentials incluidas como el selector de colores y la información sobre herramientas de explorador matriz</span><span class="sxs-lookup"><span data-stu-id="43df2-431">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="43df2-432">Usar las nuevas características de editor de JavaScript incluidas en la Web Essentials, como extraer a archivo e IntelliSense de todos los elementos HTML</span><span class="sxs-lookup"><span data-stu-id="43df2-432">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="43df2-433">Intercambiar datos entre el explorador y Visual Studio mediante el vínculo de explorador</span><span class="sxs-lookup"><span data-stu-id="43df2-433">Exchange data between your browser and Visual Studio using Browser Link</span></span>
