---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Con el Inspector de página para Visual Studio 2012 en ASP.NET Web Forms | Documentos de Microsoft
author: rick-anderson
description: Inspector de página para Visual Studio 2012 es una herramienta de desarrollo web con un explorador integrado. Seleccione cualquier elemento en el explorador integrado y Inspector de página...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: ca8a3c194577766e56d0604323fef567d539316c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a><span data-ttu-id="f6071-104">Con el Inspector de página para Visual Studio 2012 en formularios Web Forms ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f6071-104">Using Page Inspector for Visual Studio 2012 in ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="f6071-105">por Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="f6071-105">by Tim Ammann</span></span>

> <span data-ttu-id="f6071-106">Inspector de página para Visual Studio 2012 es una herramienta de desarrollo web con un explorador integrado.</span><span class="sxs-lookup"><span data-stu-id="f6071-106">Page Inspector for Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="f6071-107">Seleccione cualquier elemento en el explorador integrado y Inspector de página al instante resalta el elemento de la fuente y CSS.</span><span class="sxs-lookup"><span data-stu-id="f6071-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="f6071-108">Puede examinar cualquier página en la aplicación, rápidamente buscar los orígenes del marcado representado y utilizar las herramientas de explorador en el entorno de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f6071-108">You can browse any page in your application, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> <span data-ttu-id="f6071-109">Este tutorial shwos cómo habilitar el modo de inspección y, a continuación, busque y editar rápidamente las reglas de CSS y texto en el proyecto web.</span><span class="sxs-lookup"><span data-stu-id="f6071-109">This tutorial shwos how to enable Inspection Mode and then quickly locate and edit CSS rules and text within your web project.</span></span> <span data-ttu-id="f6071-110">El tutorial utiliza un proyecto de aplicación de formularios Web, pero también puede utilizar el Inspector de página para los proyectos de sitio Web y [MVC](https://go.microsoft.com/?linkid=9802002) aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="f6071-110">The tutorial uses a Web Forms Application Project, but you can also use Page Inspector for Web Site projects and [MVC](https://go.microsoft.com/?linkid=9802002) applications.</span></span>
> 
> <span data-ttu-id="f6071-111">El tutorial consta de las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="f6071-111">The tutorial has the following sections:</span></span>
> 
> [<span data-ttu-id="f6071-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="f6071-112">Prerequisites</span></span>](#_1_prerequisites)
> 
> [<span data-ttu-id="f6071-113">Crear una aplicación Web</span><span class="sxs-lookup"><span data-stu-id="f6071-113">Create a Web Application</span></span>](#_2_creating_a)
> 
> [<span data-ttu-id="f6071-114">Usar Inspector de página para ver la aplicación</span><span class="sxs-lookup"><span data-stu-id="f6071-114">Use Page Inspector to View the Application</span></span>](#_3_using_page)
> 
> [<span data-ttu-id="f6071-115">Habilitar el modo de inspección</span><span class="sxs-lookup"><span data-stu-id="f6071-115">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> 
> [<span data-ttu-id="f6071-116">Usar Inspector de página para realizar cambios en el marcado</span><span class="sxs-lookup"><span data-stu-id="f6071-116">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> 
> [<span data-ttu-id="f6071-117">Modo de inspección y la ventana de HTML</span><span class="sxs-lookup"><span data-stu-id="f6071-117">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> 
> [<span data-ttu-id="f6071-118">Vista previa de cambios en la ventana Estilos de CSS</span><span class="sxs-lookup"><span data-stu-id="f6071-118">Preview CSS Changes in the Styles Window</span></span>](#_7_previewing_css)
> 
> [<span data-ttu-id="f6071-119">Sincronización automática de CSS</span><span class="sxs-lookup"><span data-stu-id="f6071-119">CSS Auto Sync</span></span>](#css_auto_sync)
> 
> [<span data-ttu-id="f6071-120">Mediante el selector de Color CSS</span><span class="sxs-lookup"><span data-stu-id="f6071-120">Using the CSS Color Picker</span></span>](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="f6071-121">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="f6071-121">Prerequisites</span></span>

- <span data-ttu-id="f6071-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) o [Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="f6071-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="f6071-123">Para obtener la versión más reciente de Inspector de página, use [instalador de plataforma Web](https://go.microsoft.com/fwlink/?LinkId=255386) para instalar el SDK de Azure para .NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="f6071-123">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="f6071-124">Inspector de página está integrado en Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="f6071-124">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="f6071-125">La versión más reciente es 1.3.</span><span class="sxs-lookup"><span data-stu-id="f6071-125">The latest version is 1.3.</span></span> <span data-ttu-id="f6071-126">Para comprobar qué versión tiene, ejecute Visual Studio y seleccione **acerca de Microsoft Visual Studio** desde el **ayuda** menú.</span><span class="sxs-lookup"><span data-stu-id="f6071-126">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="f6071-127">Crear una aplicación Web</span><span class="sxs-lookup"><span data-stu-id="f6071-127">Create a Web Application</span></span>

<span data-ttu-id="f6071-128">En primer lugar, creará una aplicación web que se va a usar Inspector de página con.</span><span class="sxs-lookup"><span data-stu-id="f6071-128">First, you will create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="f6071-129">En Visual Studio, elija **archivo** &gt; **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="f6071-129">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="f6071-130">En el lado izquierdo, expanda **Visual C#**, seleccione **Web**y, a continuación, seleccione **aplicación de ASP.NET Web Forms**.</span><span class="sxs-lookup"><span data-stu-id="f6071-130">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET Web Forms Application**.</span></span>

![Nueva aplicación de formularios Web](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

<span data-ttu-id="f6071-132">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="f6071-132">Click **OK**.</span></span>

<span data-ttu-id="f6071-133">Se abre la aplicación en **origen** vista.</span><span class="sxs-lookup"><span data-stu-id="f6071-133">The application opens in **Source** view.</span></span>

![Nueva aplicación de formularios Web en la vista de origen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

<span data-ttu-id="f6071-135">Ahora que tiene una aplicación para que funcione con, puede utilizar el Inspector de página para examinar y modificarlo.</span><span class="sxs-lookup"><span data-stu-id="f6071-135">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a><span data-ttu-id="f6071-136">Usar Inspector de página para ver la aplicación</span><span class="sxs-lookup"><span data-stu-id="f6071-136">Use Page Inspector to View the Application</span></span>

<span data-ttu-id="f6071-137">A continuación, verá la aplicación con el Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="f6071-137">Next, you will view the application with Page Inspector.</span></span> <span data-ttu-id="f6071-138">En **el Explorador de soluciones**, haga clic en el proyecto y, a continuación, elija **ver en Inspector de página**.</span><span class="sxs-lookup"><span data-stu-id="f6071-138">In **Solution Explorer**, right click the project, and then choose **View in Page Inspector**.</span></span>

![Ver en Inspector de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

<span data-ttu-id="f6071-140">De forma predeterminada, al Inspector de página se inicia por primera vez, se está acoplado como una ventana estrecha en el lado izquierdo del entorno de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f6071-140">By default, when Page Inspector launches for the first time, it is docked as a narrow window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="f6071-141">Dejarla acoplada en el lado izquierdo y establézcalo en un valor de ancho que resulte cómodo para usted, o cuando acopla en una de las áreas de la herramienta en la parte superior, inferior o derecho:</span><span class="sxs-lookup"><span data-stu-id="f6071-141">Leave it docked on the left side and set it to a width that is comfortable for you, or dock it in one of the tool areas on the top, bottom, or right:</span></span>

![Posiciones de acoplamiento de Inspector de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

<span data-ttu-id="f6071-143">Si desacoplar la ventana del Inspector de página, se puede colocar fuera de Visual Studio, o incluso en un segundo monitor si dispone de uno.</span><span class="sxs-lookup"><span data-stu-id="f6071-143">If you undock the Page Inspector window, you can place it outside Visual Studio, or even on a second monitor if you have one.</span></span> <span data-ttu-id="f6071-144">Sin embargo, en orden a ALT + TAB entre Visual Studio y el Inspector de página cuando la ventana de Inspector de página está desacoplada, se vaya a **herramientas** &gt; **opciones** &gt;  **Entorno** &gt; **pestañas y ventanas**y en **ficha también**, desactive la casilla de verificación denominada **las ventanas de herramientas flotante siempre está en la parte superior de la ventana principal**:</span><span class="sxs-lookup"><span data-stu-id="f6071-144">However, in order to ALT+TAB between Page Inspector and Visual Studio when the Page Inspector window is undocked, go to **Tools** &gt; **Options** &gt; **Environment** &gt; **Tabs and Windows**, and under **Tab Well**, clear the check box called **Floating tool windows always stay on top of the main window**:</span></span>

![Desactive la casilla de windows de herramienta flotante a ALT + TAB entre Visual Studio y la ventana de Inspector de página desacoplada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

<span data-ttu-id="f6071-146">El panel superior de la ventana de Inspector de página muestra la página actual en una ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="f6071-146">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="f6071-147">El panel inferior muestra la página en el marcado HTML de la izquierda, y algunas fichas a la derecha que le permiten inspeccionar los distintos aspectos de la página.</span><span class="sxs-lookup"><span data-stu-id="f6071-147">The bottom pane shows the page in HTML markup on the left, and some tabs on the right that let you inspect different aspects of the page.</span></span> <span data-ttu-id="f6071-148">El panel inferior es similar a la [herramientas de desarrollo F12](https://msdn.microsoft.com/ie/aa740478) en Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="f6071-148">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span> <span data-ttu-id="f6071-149">(Sin embargo, a diferencia de las herramientas de desarrollo, puede utilizar el Inspector de página en Visual Studio.)</span><span class="sxs-lookup"><span data-stu-id="f6071-149">(However, unlike the developer tools, you can use Page Inspector right within Visual Studio.)</span></span>

![Inspector de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

<span data-ttu-id="f6071-151">En este tutorial, va a usar el panel Explorador de Inspector de página y el **HTML** y **estilos** pestañas para ayudarle rápidamente navegar y realizar cambios en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f6071-151">In this tutorial, you will use the Page Inspector browser pane, and the **HTML** and **Styles** tabs to help you rapidly navigate and make changes to the application.</span></span>

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a><span data-ttu-id="f6071-152">Habilitar el modo de inspección</span><span class="sxs-lookup"><span data-stu-id="f6071-152">Enable Inspection Mode</span></span>

<span data-ttu-id="f6071-153">A continuación, verá cómo funciona el modo de inspección del Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="f6071-153">Next, you will see how Page Inspector's Inspection Mode works.</span></span> <span data-ttu-id="f6071-154">En la ventana del Inspector de página, haga clic en el **inspeccionar** botón.</span><span class="sxs-lookup"><span data-stu-id="f6071-154">In the Page Inspector window, click the **Inspect** button.</span></span>

![Inspeccionar el elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

<span data-ttu-id="f6071-156">Para ver el modo de inspección en acción, mueva el mouse sobre las distintas partes de la página en la ventana del explorador de Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="f6071-156">To see inspection mode in action, move your mouse over different parts of the page within the Page Inspector browser window.</span></span> <span data-ttu-id="f6071-157">Tal y como lo hace, el puntero del mouse cambia a un signo más grande y se resalta el elemento debajo:</span><span class="sxs-lookup"><span data-stu-id="f6071-157">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Mantiene el mouse sobre el contenedor de div.content](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

<span data-ttu-id="f6071-159">Cuando mueve el puntero del mouse, tenga en cuenta que</span><span class="sxs-lookup"><span data-stu-id="f6071-159">As you move the mouse pointer, note that</span></span>

- <span data-ttu-id="f6071-160">El contenido de **origen** ver cambia para mostrar el marcado correspondiente al elemento seleccionado en la página.</span><span class="sxs-lookup"><span data-stu-id="f6071-160">The content in **Source** view changes to show the markup corresponding to the selected element on the page.</span></span> <span data-ttu-id="f6071-161">El marcado correspondiente se resalta.</span><span class="sxs-lookup"><span data-stu-id="f6071-161">The relevant markup is highlighted.</span></span> <span data-ttu-id="f6071-162">Si el origen está en otro archivo, ese archivo se abre en la vista de origen con el resaltado de marcado pertinentes.</span><span class="sxs-lookup"><span data-stu-id="f6071-162">If the source is in another file, that file is opened in Source view with the relevant markup highlighted.</span></span>

- <span data-ttu-id="f6071-163">El marcado que se muestra en el **HTML** ficha en Inspector de página también se cambia para que correspondan al elemento seleccionado en la página.</span><span class="sxs-lookup"><span data-stu-id="f6071-163">The markup displayed in the **HTML** tab in Page Inspector also changes to correspond to the selected element on the page.</span></span> <span data-ttu-id="f6071-164">En el **HTML** ficha, se muestra el marcado correspondiente.</span><span class="sxs-lookup"><span data-stu-id="f6071-164">In the **HTML** tab, the relevant markup is outlined.</span></span>

- <span data-ttu-id="f6071-165">El **estilos** pestaña muestra las reglas de CSS relevantes para la selección actual.</span><span class="sxs-lookup"><span data-stu-id="f6071-165">The **Styles** tab shows the CSS rules relevant to the current selection.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="f6071-166">Usar Inspector de página para realizar cambios en el marcado</span><span class="sxs-lookup"><span data-stu-id="f6071-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="f6071-167">Ahora verá cómo se puede usar el Inspector de página para buscar y realizar cambios en el marcado o el texto cuya ubicación podría no detectarse inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="f6071-167">Now you will see how you can use Page Inspector to find and make changes to markup or text whose location might not be immediately obvious.</span></span>

<span data-ttu-id="f6071-168">Poner Inspector de página en modo de inspección y, a continuación, desplácese hasta la parte inferior de la página principal.</span><span class="sxs-lookup"><span data-stu-id="f6071-168">Put Page Inspector in Inspection Mode and then scroll to the bottom of the home page.</span></span>

<span data-ttu-id="f6071-169">En cuanto escriba el área de pie de página, Inspector de página abre los *Site.Master* archivo de diseño en **origen** vista en una pestaña temporal a la derecha de las otras pestañas y resalta la sección del maestro de página que ha seleccionado.</span><span class="sxs-lookup"><span data-stu-id="f6071-169">As soon as you enter the footer area, Page Inspector opens the *Site.Master* layout file in **Source** view in a temporary tab to the right of the other tabs and highlights the section of the master page that you have selected.</span></span> <span data-ttu-id="f6071-170">Esto muestra cómo puede buscar y mostrar el contenido en una página que realmente podría proceder de un archivo diferente a la que se abrió originalmente Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="f6071-170">This shows you how Page Inspector can find and display content on a page that might actually come from a different file than the one you originally opened.</span></span>

![Aspectos destacados de pie de página en modo de inspección](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

<span data-ttu-id="f6071-172">En la ventana del explorador de Inspector de página, mueva el puntero del mouse sobre la línea con los derechos de autor <a id="a"> </a>tenga en cuenta.</span><span class="sxs-lookup"><span data-stu-id="f6071-172">In the Page Inspector browser window, move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span>

<span data-ttu-id="f6071-173">En el *Site.Master* página, la línea correspondiente se resalta.</span><span class="sxs-lookup"><span data-stu-id="f6071-173">In the *Site.Master* page, the corresponding line is highlighted.</span></span>

![Línea de propiedad intelectual de pie de página resaltada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

<span data-ttu-id="f6071-175">Agregar texto al final de la línea en la *Site.Master* archivo.</span><span class="sxs-lookup"><span data-stu-id="f6071-175">Add some text to the end of the line in the *Site.Master* file.</span></span>

<span data-ttu-id="f6071-176">&lt;p&gt;&amp;copien; &lt;%: DateTime.Now.Year %&gt; -mi rocas de aplicación de ASP.NET!&lt; /p&gt;</span><span class="sxs-lookup"><span data-stu-id="f6071-176">&lt;p&gt;&amp;copy; &lt;%: DateTime.Now.Year %&gt; - My ASP.NET Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="f6071-177">Ahora, presione Ctrl + Alt + ENTRAR o haga clic en la barra de actualización para ver los resultados en la ventana del explorador de Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="f6071-177">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![Mi rocas de aplicación de ASP.NET.](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

<span data-ttu-id="f6071-179">Es posible que haya pensado que era el pie de página en el *Default.aspx* página, pero resultó en la página de diseño maestra y Inspector de página se encuentra para usted.</span><span class="sxs-lookup"><span data-stu-id="f6071-179">You might have thought that the footer was on the *Default.aspx* page, but it turned out to be in the master layout page, and Page Inspector found it for you.</span></span>

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="f6071-180">Modo de inspección y la ventana de HTML</span><span class="sxs-lookup"><span data-stu-id="f6071-180">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="f6071-181">A continuación, tendrá un vistazo rápido a la ventana HTML y cómo se asigna elementos automáticamente.</span><span class="sxs-lookup"><span data-stu-id="f6071-181">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="f6071-182">Poner Inspector de página en modo de inspección.</span><span class="sxs-lookup"><span data-stu-id="f6071-182">Put Page Inspector in Inspection Mode.</span></span>

![Inspeccionar el elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

<span data-ttu-id="f6071-184">Haga clic en la parte superior de la página que dice "su logotipo aquí".</span><span class="sxs-lookup"><span data-stu-id="f6071-184">Click the top part of the page that says "your logo here".</span></span> <span data-ttu-id="f6071-185">Se examina un elemento determinado con más detalle, por lo que ya no se cambia la visualización en la ventana del explorador cuando mueve el puntero del mouse.</span><span class="sxs-lookup"><span data-stu-id="f6071-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="f6071-186">Ahora, mueva el puntero del mouse hasta el **HTML** ventana.</span><span class="sxs-lookup"><span data-stu-id="f6071-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="f6071-187">Cuando mueve el puntero del mouse, Inspector de página se describen el elemento en el **HTML** ventana y resalta el elemento correspondiente en la ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="f6071-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![HTML Window](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

<span data-ttu-id="f6071-189">Como antes, Inspector de página abre los *Site.Master* archivo automáticamente en una pestaña temporal. Haga clic en la pestaña Site.Master y el marcado correspondiente se resalta en el &lt;encabezado&gt; sección:</span><span class="sxs-lookup"><span data-stu-id="f6071-189">As before, Page Inspector opens the *Site.Master* file for you in a temporary tab. Click the Site.Master tab, and the corresponding markup is highlighted in the &lt;header&gt; section:</span></span>

![Marcado resaltado](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="f6071-191">Vista previa de cambios en la ventana Estilos de CSS</span><span class="sxs-lookup"><span data-stu-id="f6071-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="f6071-192">A continuación, verá cómo se puede utilizar el Inspector de página **estilos** ventana obtener una vista previa de los cambios de CSS.</span><span class="sxs-lookup"><span data-stu-id="f6071-192">Next, you will see how you can use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="f6071-193">Haga clic en el **inspeccionar** botón para poner Inspector de página en modo de inspección.</span><span class="sxs-lookup"><span data-stu-id="f6071-193">Click the **Inspect** button to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="f6071-194">En la ventana del explorador de Inspector de página, mueva el puntero del mouse sobre la sección "Home Page" hasta que el **div.content contenedor** etiqueta aparece.</span><span class="sxs-lookup"><span data-stu-id="f6071-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Al mantener el mouse sobre los elementos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

<span data-ttu-id="f6071-196">Haga clic en la sección div.content contenedor una vez y, a continuación, mueva el puntero del mouse hasta el **estilos** ventana.</span><span class="sxs-lookup"><span data-stu-id="f6071-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="f6071-197">En el selector de clase de contenedor .content .featured, desactive y Active la casilla de verificación para la propiedad de color de fondo.</span><span class="sxs-lookup"><span data-stu-id="f6071-197">Under the .featured .content-wrapper class selector, clear and select the checkbox for the background-color property.</span></span>

![Color de fondo claro](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

<span data-ttu-id="f6071-199">Observe cómo el cambio muestra una vista previa al instante en la ventana del explorador de Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="f6071-199">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="f6071-200">Vuelva a seleccionar la casilla de verificación, a continuación, haga doble clic en el valor de propiedad y cámbiela a `red`.</span><span class="sxs-lookup"><span data-stu-id="f6071-200">Select the checkbox again, then double-click the property value and change it to `red`.</span></span> <span data-ttu-id="f6071-201">El cambio se muestra inmediatamente:</span><span class="sxs-lookup"><span data-stu-id="f6071-201">The change shows immediately:</span></span>

![Color de fondo rojo](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

<span data-ttu-id="f6071-203">El **estilos** hace ventana fáciles de probar y obtener una vista previa de CSS cambia antes de confirmar los cambios en el estilo de hojas de sí mismo.</span><span class="sxs-lookup"><span data-stu-id="f6071-203">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="f6071-204">Sincronización automática de CSS</span><span class="sxs-lookup"><span data-stu-id="f6071-204">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="f6071-205">Esta característica requiere la versión 1.3 de Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="f6071-205">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="f6071-206">La característica de sincronización automática de CSS permite editar directamente un archivo CSS y ver los cambios inmediatamente en el Explorador de Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="f6071-206">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="f6071-207">Haga clic en **inspeccionar** para poner Inspector de página en modo de inspección.</span><span class="sxs-lookup"><span data-stu-id="f6071-207">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="f6071-208">En el Explorador de Inspector de página, mueva el puntero del mouse sobre la sección "Home Page" hasta que el **div.content contenedor** etiqueta aparece.</span><span class="sxs-lookup"><span data-stu-id="f6071-208">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="f6071-209">Haga clic una vez para seleccionar este elemento.</span><span class="sxs-lookup"><span data-stu-id="f6071-209">Click once to select this element.</span></span>

<span data-ttu-id="f6071-210">El **Syles** ventana muestra todas las reglas CSS para este elemento.</span><span class="sxs-lookup"><span data-stu-id="f6071-210">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="f6071-211">Desplácese hacia abajo hasta encontrar el contenedor de .content .featured selector de clase.</span><span class="sxs-lookup"><span data-stu-id="f6071-211">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="f6071-212">Haga clic en ".featured .content-contenedor".</span><span class="sxs-lookup"><span data-stu-id="f6071-212">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="f6071-213">Inspector de página se abre el archivo CSS que define este estilo (Site.css) y resalta el estilo CSS correspondiente.</span><span class="sxs-lookup"><span data-stu-id="f6071-213">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![Archivo CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

<span data-ttu-id="f6071-215">Ahora, cambie el valor de `background-color` a "rojo".</span><span class="sxs-lookup"><span data-stu-id="f6071-215">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="f6071-216">El cambio aparece inmediatamente en el Explorador de Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="f6071-216">The change appears immediately in the Page Inspector browser.</span></span>

![Explorador de Inspector de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a><span data-ttu-id="f6071-218">Mediante el selector de Color CSS</span><span class="sxs-lookup"><span data-stu-id="f6071-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="f6071-219">A continuación, aprenderá cómo utilizar el Inspector de página para buscar rápidamente y cambiar el código CSS para el texto resaltado en la aplicación predeterminada.</span><span class="sxs-lookup"><span data-stu-id="f6071-219">Next, you'll learn how to use Page Inspector to quickly find and change the CSS for highlighted text in the default application.</span></span> <span data-ttu-id="f6071-220">En este ejemplo, ha decidido que no desee el resaltado azul y desea cambiar a otro color.</span><span class="sxs-lookup"><span data-stu-id="f6071-220">In this example, you've decided that you don't like the blue highlighting and want to change it to another color.</span></span>

<span data-ttu-id="f6071-221">Haga clic en el **inspeccionar** botón.</span><span class="sxs-lookup"><span data-stu-id="f6071-221">Click the **Inspect** button.</span></span>

![Inspeccionar el elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

<span data-ttu-id="f6071-223">En la ventana del explorador de Inspector de página, mueva el puntero del mouse sobre el resaltado "vídeos, tutoriales y ejemplos" aparece de forma que el código CSS "marcar" etiqueta de texto.</span><span class="sxs-lookup"><span data-stu-id="f6071-223">In the Page Inspector browser window, move the mouse pointer over the highlighted "videos, tutorials, and samples" text so that the CSS "mark" label appears.</span></span>

![Al mantener el mouse sobre el elemento de marca](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

<span data-ttu-id="f6071-225">Haga clic en el texto para seleccionarlo.</span><span class="sxs-lookup"><span data-stu-id="f6071-225">Click the text to select it.</span></span> <span data-ttu-id="f6071-226">El selector de marca CSS correspondiente aparece en la parte inferior de la **estilos** ventana.</span><span class="sxs-lookup"><span data-stu-id="f6071-226">The corresponding CSS mark selector appears at the bottom of the **Styles** window.</span></span>

![selector de marca en la ventana estilos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

<span data-ttu-id="f6071-228">Haga clic en el selector de marca.</span><span class="sxs-lookup"><span data-stu-id="f6071-228">Click the mark selector.</span></span> <span data-ttu-id="f6071-229">Se abrirá la *Site.css* archivo para la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="f6071-229">This opens the *Site.css* file for the web application.</span></span> <span data-ttu-id="f6071-230">Haga clic en la pestaña Site.css y se resalta el correspondiente código CSS para el selector:</span><span class="sxs-lookup"><span data-stu-id="f6071-230">Click the Site.css tab, and the corresponding CSS for the selector is highlighted:</span></span>

![selector de marca en la hoja de estilos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

<span data-ttu-id="f6071-232">Seleccione y quite la línea con la propiedad de color de fondo.</span><span class="sxs-lookup"><span data-stu-id="f6071-232">Select and remove the line with the background-color property.</span></span>

<span data-ttu-id="f6071-233">Ahora utilizará el selector de colores de Visual Studio 2012 CSS nuevo para elegir un color nuevo para la **marcar** propiedad de color de fondo.</span><span class="sxs-lookup"><span data-stu-id="f6071-233">You will now use the new Visual Studio 2012 CSS color picker to choose a new color for the **mark** background-color property.</span></span>

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a><span data-ttu-id="f6071-234">Mediante el selector de Color de CSS de Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="f6071-234">Using the Visual Studio 2012 CSS Color Picker</span></span>

<span data-ttu-id="f6071-235">El editor de CSS en Visual Studio 2012 tiene un selector de color que hace más fácil elegir e insertar otros colores.</span><span class="sxs-lookup"><span data-stu-id="f6071-235">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="f6071-236">Tiene una barra de colores simple y un selector "pop desplegable" que ofrece un control más preciso.</span><span class="sxs-lookup"><span data-stu-id="f6071-236">It has a simple color bar and a "pop-down" picker that offers finer control.</span></span>

<span data-ttu-id="f6071-237">El selector de colores incluye una paleta de colores estándar, es compatible con nombres de color estándar, códigos hash, los colores RGB, RGBA, HSL y HSLA y mantiene una lista de los colores que se haya utilizado más recientemente en el documento.</span><span class="sxs-lookup"><span data-stu-id="f6071-237">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="f6071-238">En la línea donde fue la propiedad de color de fondo, escriba "bc" y pulse la flecha hacia abajo una vez.</span><span class="sxs-lookup"><span data-stu-id="f6071-238">On the line where the background-color property was, type "bc" and press the down arrow once.</span></span>

<span data-ttu-id="f6071-239">Cuando se escribe el primer carácter de cada palabra en una propiedad separados por guión como "background-color", IntelliSense filtra la lista para mostrar solo las propiedades que coinciden con:</span><span class="sxs-lookup"><span data-stu-id="f6071-239">When you type the first character of each word in a hyphen-separated property like "background-color", IntelliSense filters the list for you to show only the properties that match:</span></span>

![Valores de filtrado de IntelliSense](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

<span data-ttu-id="f6071-241">A continuación, escriba un signo de dos puntos.</span><span class="sxs-lookup"><span data-stu-id="f6071-241">Now type a colon.</span></span> <span data-ttu-id="f6071-242">Al hacerlo, se inserta el nombre de propiedad de color de fondo completo.</span><span class="sxs-lookup"><span data-stu-id="f6071-242">When you do, the full background-color property name is inserted.</span></span> <span data-ttu-id="f6071-243">Tipo de **#** o **rgb (**, y aparece la barra de selector de color:</span><span class="sxs-lookup"><span data-stu-id="f6071-243">Type **#** or **rgb(**, and the color picker bar appears:</span></span>

![La barra de selector de color CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

<span data-ttu-id="f6071-245">Para ver cómo funciona la barra de selector de color, haga clic en los colores con el puntero del mouse, o presione la tecla flecha abajo y, a continuación, use las teclas de flecha derecha e izquierda para atravesar los colores.</span><span class="sxs-lookup"><span data-stu-id="f6071-245">To see how the color picker bar works, click its colors with the mouse pointer, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="f6071-246">Cuando visita un color, el valor correspondiente de la propiedad de color de fondo es una vista previa:</span><span class="sxs-lookup"><span data-stu-id="f6071-246">When you visit a color, the corresponding value for the background-color property is previewed:</span></span>

![valor de propiedad de color de fondo que muestra una vista previa](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

<span data-ttu-id="f6071-248">En este punto, puede presionar ENTRAR para seleccionar el valor y, a continuación, un punto y coma (;) para completar la entrada CSS.</span><span class="sxs-lookup"><span data-stu-id="f6071-248">At this point, you could press Enter to select the value and then a semicolon (;) to complete the CSS entry.</span></span> <span data-ttu-id="f6071-249">Por ahora, vaya a la sección siguiente para que pueda ver cómo funciona la profundidad de pop de selector de color.</span><span class="sxs-lookup"><span data-stu-id="f6071-249">For now, go on to the next section so that you can see how the color picker pop-down works.</span></span>

#### <a name="using-the-color-picker-pop-down"></a><span data-ttu-id="f6071-250">Usar el selector de Color Pop-abajo</span><span class="sxs-lookup"><span data-stu-id="f6071-250">Using the Color Picker Pop-Down</span></span>

<span data-ttu-id="f6071-251">Cuando la barra de colores no tiene exactamente el color que está buscando, puede usar el selector de colores emergente abajo.</span><span class="sxs-lookup"><span data-stu-id="f6071-251">When the color bar doesn't have the exact color that you're looking for, you can use the color picker pop-down.</span></span>

<span data-ttu-id="f6071-252">Para abrirlo, haga clic en las comillas angulares dobles en el extremo derecho de la barra de colores o presione la tecla flecha abajo una o dos veces en el teclado.</span><span class="sxs-lookup"><span data-stu-id="f6071-252">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Selector de Color CSS Pop-abajo](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

<span data-ttu-id="f6071-254">Haga clic en un color en la barra vertical a la derecha.</span><span class="sxs-lookup"><span data-stu-id="f6071-254">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="f6071-255">Esto muestra un degradado para ese color en la ventana principal.</span><span class="sxs-lookup"><span data-stu-id="f6071-255">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="f6071-256">Elija un color directamente desde la barra vertical, presione ENTRAR o haga clic en cualquier momento en la ventana principal para elegir con mayor precisión.</span><span class="sxs-lookup"><span data-stu-id="f6071-256">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="f6071-257">Si hay un color en la pantalla del equipo que desea utilizar (que no tiene que estar dentro de la interfaz de usuario de Visual Studio), puede capturar su valor mediante la herramienta de cuentagotas en la esquina inferior derecha.</span><span class="sxs-lookup"><span data-stu-id="f6071-257">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="f6071-258">También puede cambiar la opacidad de un color moviendo el control deslizante en la parte inferior del selector de color.</span><span class="sxs-lookup"><span data-stu-id="f6071-258">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="f6071-259">Si lo hace, cambios de color valores con los valores RGBA porque el formato RGBA puede representar la opacidad.</span><span class="sxs-lookup"><span data-stu-id="f6071-259">Doing so changes color values to RGBA values because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="f6071-260">Después de elegir un color, presione ENTRAR y, a continuación, escriba un punto y coma para completar la entrada de color de fondo en el *Site.css* archivo.</span><span class="sxs-lookup"><span data-stu-id="f6071-260">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="f6071-261">La barra de actualización de Inspector de página</span><span class="sxs-lookup"><span data-stu-id="f6071-261">The Page Inspector Update Bar</span></span>

<span data-ttu-id="f6071-262">Inspector de página detecta inmediatamente el cambio a la *Site.css* archivo (o a cualquier archivo en la aplicación) y muestra una alerta en una barra de actualización.</span><span class="sxs-lookup"><span data-stu-id="f6071-262">Page Inspector immediately detects the change to the *Site.css* file (or to any file in the application) and displays an alert in an update bar.</span></span>

![Barra de actualización](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

<span data-ttu-id="f6071-264">Para guardar todos los archivos y actualizar el Explorador de Inspector de página, presione Ctrl + Alt + ENTRAR o haga clic en la barra de actualización.</span><span class="sxs-lookup"><span data-stu-id="f6071-264">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="f6071-265">El cambio en el color de resaltado aparece en el explorador:</span><span class="sxs-lookup"><span data-stu-id="f6071-265">The change in the highlight color appears in the browser:</span></span>

![Cambiar el color de resaltado](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a><span data-ttu-id="f6071-267">Tenga en cuenta que cómodamente actualice el Explorador de Inspector de página directamente desde dentro del entorno de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f6071-267">Notice that you conveniently refreshed the Page Inspector browser right from within the Visual Studio environment.</span></span> <span data-ttu-id="f6071-268">Con el Inspector de página en lugar de un explorador externo le mantendrá en el editor cuando se desarrollan las aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="f6071-268">Using Page Inspector instead of an external browser lets you stay in the editor when you develop your web applications.</span></span>

## <a name="see-also"></a><span data-ttu-id="f6071-269">Vea también</span><span class="sxs-lookup"><span data-stu-id="f6071-269">See Also</span></span>

<span data-ttu-id="f6071-270">[Introducción a Inspector de página](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (vídeo de Channel 9)</span><span class="sxs-lookup"><span data-stu-id="f6071-270">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)</span></span>

<span data-ttu-id="f6071-271">[Mensajes de Error del Inspector de página](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="f6071-271">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
