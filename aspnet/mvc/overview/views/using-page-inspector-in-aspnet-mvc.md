---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: Usar el Inspector de página en ASP.NET MVC | Microsoft Docs
author: rick-anderson
description: Inspector de página en Visual Studio 2012 es una herramienta de desarrollo web con un explorador integrado. Seleccione cualquier elemento en el explorador integrado y el Inspector de página i...
ms.author: aspnetcontent
ms.date: 08/15/2012
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: e3a6b79811cae15ec69ba3c5babe38b117b697a5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806091"
---
<a name="using-page-inspector-in-aspnet-mvc"></a><span data-ttu-id="e76b3-104">Usar el Inspector de página en ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="e76b3-104">Using Page Inspector in ASP.NET MVC</span></span>
====================
<span data-ttu-id="e76b3-105">por Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="e76b3-105">by Tim Ammann</span></span>

> <span data-ttu-id="e76b3-106">Inspector de página en Visual Studio 2012 es una herramienta de desarrollo web con un explorador integrado.</span><span class="sxs-lookup"><span data-stu-id="e76b3-106">Page Inspector in Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="e76b3-107">Seleccione cualquier elemento en el explorador integrado y el Inspector de página al instante resalta del elemento origen y CSS.</span><span class="sxs-lookup"><span data-stu-id="e76b3-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="e76b3-108">Puede examinar cualquier vista MVC, rápidamente buscar los orígenes de marcado representado y usar las herramientas del explorador directamente en el entorno de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e76b3-108">You can browse any MVC view, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> [<span data-ttu-id="e76b3-109">Vea el vídeo</span><span class="sxs-lookup"><span data-stu-id="e76b3-109">Watch the Video</span></span>](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> <span data-ttu-id="e76b3-110">Este tutorial muestra cómo habilitar el modo de inspección y, a continuación, buscar y editar rápidamente marcado y CSS dentro del proyecto web.</span><span class="sxs-lookup"><span data-stu-id="e76b3-110">This tutorial shows how to enable Inspection Mode, and then quickly locate and edit markup and CSS within your web project.</span></span> <span data-ttu-id="e76b3-111">Este tutorial usa un proyecto de MVC, pero también puede usar el Inspector de página para [formularios Web Forms](https://go.microsoft.com/?linkid=9802001) y otras aplicaciones de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e76b3-111">The tutorial uses an MVC Project, but you can also use Page Inspector for [Web Forms](https://go.microsoft.com/?linkid=9802001) and other ASP.NET applications.</span></span>
> 
> <span data-ttu-id="e76b3-112">El tutorial tiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="e76b3-112">The tutorial has the following sections:</span></span>
> 
> - [<span data-ttu-id="e76b3-113">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="e76b3-113">Prerequisites</span></span>](#_1_prerequisites)
> - [<span data-ttu-id="e76b3-114">Crear una aplicación Web</span><span class="sxs-lookup"><span data-stu-id="e76b3-114">Create a Web Application</span></span>](#_2_creating_a)
> - [<span data-ttu-id="e76b3-115">Usar Inspector de página para examinar a una vista</span><span class="sxs-lookup"><span data-stu-id="e76b3-115">Use Page Inspector to Browse to a View</span></span>](#_3_using_page)
> - [<span data-ttu-id="e76b3-116">Habilitar el modo de inspección</span><span class="sxs-lookup"><span data-stu-id="e76b3-116">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> - [<span data-ttu-id="e76b3-117">Usar el Inspector de página para realizar cambios en el marcado</span><span class="sxs-lookup"><span data-stu-id="e76b3-117">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> - [<span data-ttu-id="e76b3-118">Modo de inspección y la ventana de HTML</span><span class="sxs-lookup"><span data-stu-id="e76b3-118">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> - [<span data-ttu-id="e76b3-119">Vista previa de cambios en la ventana estilos CSS</span><span class="sxs-lookup"><span data-stu-id="e76b3-119">Preview CSS Changes in the Styles window</span></span>](#_7_previewing_css)
> - [<span data-ttu-id="e76b3-120">Sincronización automática de CSS</span><span class="sxs-lookup"><span data-stu-id="e76b3-120">CSS Auto Sync</span></span>](#css_auto_sync)
> - [<span data-ttu-id="e76b3-121">Con el selector de Color CSS</span><span class="sxs-lookup"><span data-stu-id="e76b3-121">Using the CSS Color Picker</span></span>](#css_color_picker)
> - [<span data-ttu-id="e76b3-122">Asignación de elementos de página dinámica a JavaScript</span><span class="sxs-lookup"><span data-stu-id="e76b3-122">Mapping Dynamic Page Elements to JavaScript</span></span>](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="e76b3-123">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="e76b3-123">Prerequisites</span></span>

- <span data-ttu-id="e76b3-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) o [Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="e76b3-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="e76b3-125">Para obtener la versión más reciente de Inspector de página, use [instalador de plataforma Web](https://go.microsoft.com/fwlink/?LinkId=255386) para instalar el SDK de Windows Azure para .NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="e76b3-125">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Windows Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="e76b3-126">Inspector de página se incluye con Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="e76b3-126">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="e76b3-127">La versión más reciente es 1.3.</span><span class="sxs-lookup"><span data-stu-id="e76b3-127">The latest version is 1.3.</span></span> <span data-ttu-id="e76b3-128">Para comprobar qué versión tiene, ejecute Visual Studio y seleccione **acerca de Microsoft Visual Studio** desde el **ayuda** menú.</span><span class="sxs-lookup"><span data-stu-id="e76b3-128">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="e76b3-129">Crear una aplicación Web</span><span class="sxs-lookup"><span data-stu-id="e76b3-129">Create a Web Application</span></span>

<span data-ttu-id="e76b3-130">En primer lugar, cree una aplicación web que va a utilizar con el Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="e76b3-130">First, create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="e76b3-131">En Visual Studio, elija **archivo** &gt; **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="e76b3-131">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="e76b3-132">En el lado izquierdo, expanda **Visual C#**, seleccione **Web**y, a continuación, seleccione **aplicación Web de ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="e76b3-132">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span>

![Nueva aplicación MVC de ASP.NET](using-page-inspector-in-aspnet-mvc/_static/image2.png)

<span data-ttu-id="e76b3-134">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="e76b3-134">Click **OK**.</span></span>

<span data-ttu-id="e76b3-135">En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione **aplicación de Internet**.</span><span class="sxs-lookup"><span data-stu-id="e76b3-135">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="e76b3-136">Deje **Razor** como el motor de vistas predeterminado.</span><span class="sxs-lookup"><span data-stu-id="e76b3-136">Leave **Razor** as the default view engine.</span></span>

![Nuevo proyecto de MVC de ASP.NET: aplicación de Internet](using-page-inspector-in-aspnet-mvc/_static/image4.png)

<span data-ttu-id="e76b3-138">Se abre la aplicación en **origen** vista.</span><span class="sxs-lookup"><span data-stu-id="e76b3-138">The application opens in **Source** view.</span></span>

![Nueva aplicación MVC de ASP.NET en la vista de origen](using-page-inspector-in-aspnet-mvc/_static/image6.png)

<span data-ttu-id="e76b3-140">Ahora que tiene una aplicación para que funcione con, puede usar el Inspector de página para examinar y modificarlo.</span><span class="sxs-lookup"><span data-stu-id="e76b3-140">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a><span data-ttu-id="e76b3-141">Usar Inspector de página para examinar a una vista</span><span class="sxs-lookup"><span data-stu-id="e76b3-141">Use Page Inspector to Browse to a View</span></span>

<span data-ttu-id="e76b3-142">En Visual Studio 2012, puede hacer clic en cualquier vista en el proyecto, seleccione **ver en Inspector de página**, y lo averiguar la ruta y se mostrará la página de Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="e76b3-142">In Visual Studio 2012, you can right-click any view in your project, select **View in Page Inspector**, and Page Inspector will figure out the route and display the page.</span></span>

<span data-ttu-id="e76b3-143">En **el Explorador de soluciones**, expanda el **vistas** carpeta y, a continuación, el **inicio** carpeta.</span><span class="sxs-lookup"><span data-stu-id="e76b3-143">In **Solution Explorer**, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="e76b3-144">Haga clic en el archivo Index.cshtml y elija **ver en Inspector de página**.</span><span class="sxs-lookup"><span data-stu-id="e76b3-144">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

![Ver Index.cshtml en Inspector de página](using-page-inspector-in-aspnet-mvc/_static/image8.png)

<span data-ttu-id="e76b3-146">De forma predeterminada, el Inspector de página está acoplado como una ventana en el lado izquierdo del entorno de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e76b3-146">By default, Page Inspector is docked as a window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="e76b3-147">Si lo prefiere, puede acoplarlo en otro lugar o desacoplar la ventana.</span><span class="sxs-lookup"><span data-stu-id="e76b3-147">If you prefer, you can dock it elsewhere, or undock the window.</span></span> <span data-ttu-id="e76b3-148">Consulte [Cómo: organizar y acoplar Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span><span class="sxs-lookup"><span data-stu-id="e76b3-148">See [How to: Arrange and Dock Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span></span>

<span data-ttu-id="e76b3-149">El panel superior de la ventana del Inspector de página muestra la página actual en una ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="e76b3-149">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="e76b3-150">El panel inferior muestra la página en el marcado HTML, junto con algunas de las pestañas que le permiten consultar distintos aspectos de la página.</span><span class="sxs-lookup"><span data-stu-id="e76b3-150">The bottom pane shows the page in HTML markup, along with some tabs that let you inspect different aspects of the page.</span></span> <span data-ttu-id="e76b3-151">El panel inferior es similar a la [herramientas de desarrollo F12](https://msdn.microsoft.com/ie/aa740478) en Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="e76b3-151">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span>

![Aplicación de ASP.NET MVC en Inspector de página](using-page-inspector-in-aspnet-mvc/_static/image10.png)

<span data-ttu-id="e76b3-153">En este tutorial, usará el **HTML** y **estilos** pestañas para navegar rápidamente y realizar cambios en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e76b3-153">In this tutorial, you will use the **HTML** and **Styles** tabs to navigate quickly and make changes to the application.</span></span>

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a><span data-ttu-id="e76b3-154">Modo EnableInspection</span><span class="sxs-lookup"><span data-stu-id="e76b3-154">EnableInspection Mode</span></span>

<span data-ttu-id="e76b3-155">Para poner Inspector de página en modo de inspección, haga clic en el **inspeccionar** botón.</span><span class="sxs-lookup"><span data-stu-id="e76b3-155">To put Page Inspector into Inspection Mode, click the **Inspect** button.</span></span> <span data-ttu-id="e76b3-156">En el modo de inspección, cuando se mantiene el puntero del mouse sobre cualquier parte de la página presentada, el marcado del código fuente correspondiente o el código se resalta.</span><span class="sxs-lookup"><span data-stu-id="e76b3-156">In Inspection Mode, when you hold the mouse pointer over any part of the rendered page, the corresponding source markup or code is highlighted.</span></span>

![Alternar el modo de inspección](using-page-inspector-in-aspnet-mvc/_static/image12.png)

<span data-ttu-id="e76b3-158">Ahora, mueva el mouse sobre las distintas partes de la página en el Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="e76b3-158">Now move your mouse over different parts of the page within Page Inspector.</span></span> <span data-ttu-id="e76b3-159">Como lo hace, el puntero del mouse cambia a un signo más grande y se resalta el elemento debajo:</span><span class="sxs-lookup"><span data-stu-id="e76b3-159">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Mantener el mouse sobre el contenedor de div.content](using-page-inspector-in-aspnet-mvc/_static/image14.png)

<span data-ttu-id="e76b3-161">Al mover el puntero del mouse, Visual Studio resalta la sintaxis de Razor correspondiente en el archivo de origen.</span><span class="sxs-lookup"><span data-stu-id="e76b3-161">As you move the mouse pointer, Visual Studio highlights the corresponding Razor syntax in the source file.</span></span> <span data-ttu-id="e76b3-162">Si el elemento HTML procede de otro archivo de código fuente, Visual Studio abre automáticamente el archivo.</span><span class="sxs-lookup"><span data-stu-id="e76b3-162">If the HTML element comes from another source file, Visual Studio automatically opens the file.</span></span>

<span data-ttu-id="e76b3-163">En el Inspector de página, el **HTML** pestaña muestra el código HTML que se generó a partir de la sintaxis de Razor.</span><span class="sxs-lookup"><span data-stu-id="e76b3-163">In Page Inspector, the **HTML** tab shows the HTML that was generated from the Razor syntax.</span></span> <span data-ttu-id="e76b3-164">A medida que mueve el puntero del mouse, se resaltan los elementos HTML.</span><span class="sxs-lookup"><span data-stu-id="e76b3-164">As you move the mouse pointer, the HTML elements are highlighted.</span></span> <span data-ttu-id="e76b3-165">El **estilos** ficha muestra las reglas CSS para el elemento.</span><span class="sxs-lookup"><span data-stu-id="e76b3-165">The **Styles** tab shows the CSS rules for the element.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="e76b3-166">Usar el Inspector de página para realizar cambios en el marcado</span><span class="sxs-lookup"><span data-stu-id="e76b3-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="e76b3-167">Inspector de página permite buscar el marcado cuya ubicación puede no ser obvio.</span><span class="sxs-lookup"><span data-stu-id="e76b3-167">Page Inspector lets you find markup whose location might not be obvious.</span></span> <span data-ttu-id="e76b3-168">A continuación, puede modificar el marcado y ver los cambios resultantes.</span><span class="sxs-lookup"><span data-stu-id="e76b3-168">Then you can modify the markup and see the resulting changes.</span></span>

<span data-ttu-id="e76b3-169">Para ver esto, haga clic en **inspeccionar** y, a continuación, desplácese hasta la parte inferior de la página en la ventana del Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="e76b3-169">To see this, click **Inspect** and then scroll to the bottom of the page in the Page Inspector window.</span></span>

<span data-ttu-id="e76b3-170">Cuando se mueve el puntero del mouse en el área de pie de página, Inspector de página abre el \_archivo Layout.cshtml y resalta la sección de la página de diseño que ha seleccionado.</span><span class="sxs-lookup"><span data-stu-id="e76b3-170">When you move the mouse pointer into the footer area, Page Inspector opens the \_Layout.cshtml file and highlights the section of the layout page that you have selected.</span></span> <span data-ttu-id="e76b3-171">Como puede ver, el pie de página son se define en el archivo de diseño y no la propia vista.</span><span class="sxs-lookup"><span data-stu-id="e76b3-171">As you can see, the footer are is defined in the layout file, and not the view itself.</span></span>

![Pie de página](using-page-inspector-in-aspnet-mvc/_static/image16.png)

<span data-ttu-id="e76b3-173">Ahora se mueva el puntero del mouse sobre la línea con los derechos de autor <a id="a"> </a>tenga en cuenta.</span><span class="sxs-lookup"><span data-stu-id="e76b3-173">Now move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span> <span data-ttu-id="e76b3-174">En el \_Layout.cshtml página, la línea correspondiente se resalta.</span><span class="sxs-lookup"><span data-stu-id="e76b3-174">In the \_Layout.cshtml page, the corresponding line is highlighted.</span></span>

![Línea de copyright del pie de página resaltada](using-page-inspector-in-aspnet-mvc/_static/image18.png)

<span data-ttu-id="e76b3-176">Agregue texto al final de la línea en el \_Layout.cshtml archivo.</span><span class="sxs-lookup"><span data-stu-id="e76b3-176">Add some text to the end of the line in the \_Layout.cshtml file.</span></span>

<span data-ttu-id="e76b3-177">&lt;p&gt;&amp;copien; @DateTime.Now.Year -Mi aplicación ASP.NET MVC es impresionante! &lt;/p&gt;</span><span class="sxs-lookup"><span data-stu-id="e76b3-177">&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET MVC Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="e76b3-178">Ahora, presione Ctrl + Alt + ENTRAR o haga clic en la barra de actualización para ver los resultados en la ventana del explorador de Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="e76b3-178">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![Mi Rocks de aplicación de ASP.NET.](using-page-inspector-in-aspnet-mvc/_static/image20.png)

<span data-ttu-id="e76b3-180">Es posible que haya pensado que el pie de página definido en Index.cshtml, pero, resultó para ser en el \_Layout.cshtml y resultó que el Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="e76b3-180">You might have thought that the footer defined in Index.cshtml, but it turned out to be in the \_Layout.cshtml, and Page Inspector found it for you.</span></span>

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="e76b3-181">Modo de inspección y la ventana de HTML</span><span class="sxs-lookup"><span data-stu-id="e76b3-181">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="e76b3-182">A continuación, tendrá una visión rápida de la ventana de HTML y cómo se asigna elementos por usted.</span><span class="sxs-lookup"><span data-stu-id="e76b3-182">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="e76b3-183">Haga clic en **inspeccionar** para poner Inspector de página en modo de inspección.</span><span class="sxs-lookup"><span data-stu-id="e76b3-183">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="e76b3-184">Haga clic en la parte superior de la página que dice "El logohere".</span><span class="sxs-lookup"><span data-stu-id="e76b3-184">Click the top part of the page that says "Your logohere".</span></span> <span data-ttu-id="e76b3-185">Si está examinando un elemento determinado con más detalle, por lo que ya no se cambia la presentación en la ventana del explorador al mover el puntero del mouse.</span><span class="sxs-lookup"><span data-stu-id="e76b3-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="e76b3-186">Ahora, mueva el puntero del mouse en el **HTML** ventana.</span><span class="sxs-lookup"><span data-stu-id="e76b3-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="e76b3-187">Al mover el puntero del mouse, Inspector de página se describe el elemento dentro de la **HTML** ventana y resalta el elemento correspondiente en la ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="e76b3-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Ventana de HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

<span data-ttu-id="e76b3-189">Como antes, Inspector de página abre el \_archivo Layout.cshtml automáticamente en una pestaña temporal. Haga clic en el \_Layout.cshtml de pestaña temporal y el marcado correspondiente se resaltará en el &lt;encabezado&gt; sección para usted:</span><span class="sxs-lookup"><span data-stu-id="e76b3-189">As before, Page Inspector opens the \_Layout.cshtml file for you in a temporary tab. Click the \_Layout.cshtml temporary tab, and the corresponding markup will be highlighted in the &lt;header&gt; section for you:</span></span>

![Marcado resaltado](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="e76b3-191">Vista previa de cambios en la ventana estilos CSS</span><span class="sxs-lookup"><span data-stu-id="e76b3-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="e76b3-192">A continuación, usará el Inspector de página **estilos** ventana para obtener una vista previa de los cambios de CSS.</span><span class="sxs-lookup"><span data-stu-id="e76b3-192">Next, you will use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="e76b3-193">Haga clic en **inspeccionar** para poner Inspector de página en modo de inspección.</span><span class="sxs-lookup"><span data-stu-id="e76b3-193">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="e76b3-194">En la ventana de explorador del Inspector de página, mueva el puntero del mouse sobre la sección "Página principal" hasta que el **div.content contenedor** etiqueta aparece.</span><span class="sxs-lookup"><span data-stu-id="e76b3-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Mantener el mouse sobre el contenedor de div.content](using-page-inspector-in-aspnet-mvc/_static/image26.png)

<span data-ttu-id="e76b3-196">Haga clic en la sección div.content contenedor una vez y, a continuación, mueva el puntero del mouse hasta el **estilos** ventana.</span><span class="sxs-lookup"><span data-stu-id="e76b3-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="e76b3-197">El **Syles** ventana muestra todas las reglas CSS para este elemento.</span><span class="sxs-lookup"><span data-stu-id="e76b3-197">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="e76b3-198">Desplácese hacia abajo hasta encontrar el contenedor de .content .featured selector de clase.</span><span class="sxs-lookup"><span data-stu-id="e76b3-198">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="e76b3-199">Ahora, desactive la casilla de la propiedad de color de fondo.</span><span class="sxs-lookup"><span data-stu-id="e76b3-199">Now clear the checkbox for the background-color property.</span></span>

![Color de fondo claro](using-page-inspector-in-aspnet-mvc/_static/image28.png)

<span data-ttu-id="e76b3-201">Observe cómo el cambio muestra una vista previa al instante en la ventana del explorador de Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="e76b3-201">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="e76b3-202">Vuelva a seleccionar la casilla de verificación, a continuación, haga doble clic en el valor de propiedad y cámbielo a rojo.</span><span class="sxs-lookup"><span data-stu-id="e76b3-202">Select the checkbox again, then double-click the property value and change it to red.</span></span> <span data-ttu-id="e76b3-203">El cambio aparece inmediatamente:</span><span class="sxs-lookup"><span data-stu-id="e76b3-203">The change shows immediately:</span></span>

![Color de fondo rojo](using-page-inspector-in-aspnet-mvc/_static/image30.png)

<span data-ttu-id="e76b3-205">El **estilos** facilita la ventana que resulte fácil probar y obtener una vista previa de CSS cambia antes de confirmar los cambios en el estilo de hojas de sí mismo.</span><span class="sxs-lookup"><span data-stu-id="e76b3-205">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="e76b3-206">Sincronización automática de CSS</span><span class="sxs-lookup"><span data-stu-id="e76b3-206">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="e76b3-207">Esta característica requiere la versión 1.3 de Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="e76b3-207">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="e76b3-208">La característica de sincronización automática de CSS le permite editar directamente un archivo CSS y ver los cambios inmediatamente en el Explorador de Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="e76b3-208">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="e76b3-209">Haga clic en **inspeccionar** para poner Inspector de página en modo de inspección.</span><span class="sxs-lookup"><span data-stu-id="e76b3-209">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="e76b3-210">En el Explorador de Inspector de página, mueva el puntero del mouse sobre la sección "Página principal" hasta que el **div.content contenedor** etiqueta aparece.</span><span class="sxs-lookup"><span data-stu-id="e76b3-210">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="e76b3-211">Haga clic una vez para seleccionar este elemento.</span><span class="sxs-lookup"><span data-stu-id="e76b3-211">Click once to select this element.</span></span>

<span data-ttu-id="e76b3-212">El **Syles** ventana muestra todas las reglas CSS para este elemento.</span><span class="sxs-lookup"><span data-stu-id="e76b3-212">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="e76b3-213">Desplácese hacia abajo hasta encontrar el contenedor de .content .featured selector de clase.</span><span class="sxs-lookup"><span data-stu-id="e76b3-213">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="e76b3-214">Haga clic en ".featured .content-contenedor".</span><span class="sxs-lookup"><span data-stu-id="e76b3-214">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="e76b3-215">Inspector de página abre el archivo CSS que define este estilo (Site.css) y resalta el estilo CSS correspondiente.</span><span class="sxs-lookup"><span data-stu-id="e76b3-215">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

<span data-ttu-id="e76b3-216">Ahora, cambie el valor de `background-color` a "rojo".</span><span class="sxs-lookup"><span data-stu-id="e76b3-216">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="e76b3-217">El cambio aparece inmediatamente en el Explorador de Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="e76b3-217">The change appears immediately in the Page Inspector browser.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a><span data-ttu-id="e76b3-218">Con el selector de Color CSS</span><span class="sxs-lookup"><span data-stu-id="e76b3-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="e76b3-219">El editor de CSS en Visual Studio 2012 tiene un selector de colores que resulta muy fácil elegir e insertar los colores.</span><span class="sxs-lookup"><span data-stu-id="e76b3-219">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="e76b3-220">El selector de colores incluye una paleta de colores estándar, es compatible con nombres de colores estándar, códigos hash, los colores RGB, RGBA, HSL y HSLA y mantiene una lista de los colores que se ha usado más recientemente en el documento.</span><span class="sxs-lookup"><span data-stu-id="e76b3-220">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="e76b3-221">En la sección anterior, ha cambiado el valor de la `background-color` propiedad.</span><span class="sxs-lookup"><span data-stu-id="e76b3-221">In the previous section, you changed the value of the `background-color` property.</span></span> <span data-ttu-id="e76b3-222">Para invocar el selector de colores, sitúe el cursor después del nombre de la propiedad y escriba **#** o **rgb (**.</span><span class="sxs-lookup"><span data-stu-id="e76b3-222">To invoke the color picker, place the insertion point after the property name and type **#** or **rgb(**.</span></span>

![La barra de selector de colores CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

<span data-ttu-id="e76b3-224">Haga clic en un color para seleccionarlo, o presione la tecla flecha abajo y, a continuación, use las teclas de flecha derecha e izquierda para atravesar los colores.</span><span class="sxs-lookup"><span data-stu-id="e76b3-224">Click on a color to select it, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="e76b3-225">Cuando visita un color, el valor hexadecimal correspondiente es una vista previa:</span><span class="sxs-lookup"><span data-stu-id="e76b3-225">When you visit a color, the corresponding hex value is previewed:</span></span>

![valor de propiedad de color de fondo muestra una vista previa](using-page-inspector-in-aspnet-mvc/_static/image38.png)

<span data-ttu-id="e76b3-227">Si la barra de colores no tiene el color exacto que desea, puede usar el pop desplegable de selector de color.</span><span class="sxs-lookup"><span data-stu-id="e76b3-227">If the color bar doesn't have the exact color you want, you can use the color picker pop-down.</span></span> <span data-ttu-id="e76b3-228">Para abrirlo, haga clic en el cheurón doble en el extremo derecho de la barra de colores o presione la flecha abajo de una o dos veces en el teclado.</span><span class="sxs-lookup"><span data-stu-id="e76b3-228">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Selector de Color CSS Pop-abajo](using-page-inspector-in-aspnet-mvc/_static/image40.png)

<span data-ttu-id="e76b3-230">Haga clic en un color en la barra vertical a la derecha.</span><span class="sxs-lookup"><span data-stu-id="e76b3-230">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="e76b3-231">Esto muestra un degradado para ese color en la ventana principal.</span><span class="sxs-lookup"><span data-stu-id="e76b3-231">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="e76b3-232">Elegir un color directamente desde la barra vertical, presione ENTRAR o haga clic en cualquier punto en la ventana principal para elegir con mayor precisión.</span><span class="sxs-lookup"><span data-stu-id="e76b3-232">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="e76b3-233">Si hay un color en la pantalla del equipo que desea utilizar (que no tiene que estar dentro de la interfaz de usuario de Visual Studio), puede capturar su valor mediante la herramienta de cuentagotas en la esquina inferior derecha.</span><span class="sxs-lookup"><span data-stu-id="e76b3-233">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="e76b3-234">También puede cambiar la opacidad de un color moviendo el control deslizante en la parte inferior del selector de colores.</span><span class="sxs-lookup"><span data-stu-id="e76b3-234">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="e76b3-235">Hacerlo cambia al color valores a los valores RGBA, porque el formato RGBA puede representar la opacidad.</span><span class="sxs-lookup"><span data-stu-id="e76b3-235">Doing so changes color values to RGBA values, because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="e76b3-236">Después de elegir un color, presione ENTRAR y, a continuación, escriba un punto y coma para completar la entrada de color de fondo en el *Site.css* archivo.</span><span class="sxs-lookup"><span data-stu-id="e76b3-236">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="e76b3-237">La barra de actualización de Inspector de página</span><span class="sxs-lookup"><span data-stu-id="e76b3-237">The Page Inspector Update Bar</span></span>

<span data-ttu-id="e76b3-238">Inspector de página detecta inmediatamente el cambio en el *Site.css* de archivo y muestra una alerta en una barra de actualización.</span><span class="sxs-lookup"><span data-stu-id="e76b3-238">Page Inspector immediately detects the change to the *Site.css* file and displays an alert in an update bar.</span></span>

![Barra de actualización](using-page-inspector-in-aspnet-mvc/_static/image42.png)

<span data-ttu-id="e76b3-240">Para guardar todos los archivos y actualice el Explorador de Inspector de página, presione Ctrl + Alt + ENTRAR o haga clic en la barra de actualización.</span><span class="sxs-lookup"><span data-stu-id="e76b3-240">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="e76b3-241">El cambio en el color de resaltado aparece en el explorador.</span><span class="sxs-lookup"><span data-stu-id="e76b3-241">The change in the highlight color appears in the browser.</span></span>

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a><span data-ttu-id="e76b3-242">Asignación de elementos de página dinámica a JavaScript</span><span class="sxs-lookup"><span data-stu-id="e76b3-242">Mapping Dynamic Page Elements to JavaScript</span></span>

<span data-ttu-id="e76b3-243">En las aplicaciones web modernas, los elementos de la página a menudo se generan dinámicamente con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e76b3-243">In modern web applications, elements in the page are often generated dynamically with JavaScript.</span></span> <span data-ttu-id="e76b3-244">Esto significa que no hay ningún marcado estático (HTML o Razor) que corresponde a estos elementos de la página.</span><span class="sxs-lookup"><span data-stu-id="e76b3-244">That means there is no static markup (either HTML or Razor) that corresponds to these page elements.</span></span>

<span data-ttu-id="e76b3-245">Con la versión 1.3, Inspector de página ahora puede asignar los elementos que se han agregado dinámicamente a la página al código JavaScript correspondiente.</span><span class="sxs-lookup"><span data-stu-id="e76b3-245">With version 1.3, Page Inspector can now map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span> <span data-ttu-id="e76b3-246">Para demostrar esta característica, usaremos el [plantilla de aplicación de página única (SPA)](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="e76b3-246">To demonstrate this feature, we'll use the [Single Page Application (SPA) template](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span></span>

> [!NOTE]
> <span data-ttu-id="e76b3-247">La plantilla SPA requiere la [ASP.NET y Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) actualizar.</span><span class="sxs-lookup"><span data-stu-id="e76b3-247">The SPA template requires the [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) update.</span></span>


<span data-ttu-id="e76b3-248">En Visual Studio, elija **archivo** &gt; **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="e76b3-248">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="e76b3-249">En el lado izquierdo, expanda **Visual C#**, seleccione **Web**y, a continuación, seleccione **aplicación Web de ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="e76b3-249">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span> <span data-ttu-id="e76b3-250">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="e76b3-250">Click **OK**.</span></span>

<span data-ttu-id="e76b3-251">En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione **aplicación de página única**.</span><span class="sxs-lookup"><span data-stu-id="e76b3-251">In the **New ASP.NET MVC 4 Project** dialog, select **Single Page Application**.</span></span>

<span data-ttu-id="e76b3-252">En el Explorador de soluciones, expanda el **vistas** carpeta y, a continuación, el **inicio** carpeta.</span><span class="sxs-lookup"><span data-stu-id="e76b3-252">In Solution Explorer, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="e76b3-253">Haga clic en el archivo Index.cshtml y elija **ver en Inspector de página**.</span><span class="sxs-lookup"><span data-stu-id="e76b3-253">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

<span data-ttu-id="e76b3-254">Lo primero que se muestran en el Explorador de Inspector de página es una página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="e76b3-254">The first thing that is displayed in the Page Inspector browser is a login page.</span></span> <span data-ttu-id="e76b3-255">Haga clic en "Registrarse" y cree un nombre de usuario y una contraseña.</span><span class="sxs-lookup"><span data-stu-id="e76b3-255">Click "Sign Up" and create a user name and password.</span></span> <span data-ttu-id="e76b3-256">Una vez registrado, la aplicación inicia la sesión y crea una lista de tareas pendientes con algunos elementos de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="e76b3-256">Once you sign up, the application logs you in and creates a to-do list with some sample items.</span></span>

<span data-ttu-id="e76b3-257">Haga clic en **inspeccionar** para poner Inspector de página en modo de inspección.</span><span class="sxs-lookup"><span data-stu-id="e76b3-257">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span> <span data-ttu-id="e76b3-258">En el explorador del Inspector de página, haga clic en uno de los elementos de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="e76b3-258">In the Page Inspector browser, click on one of the to-do items.</span></span> <span data-ttu-id="e76b3-259">Tenga en cuenta que, en lugar de que se resaltan en azul, se resalta el elemento en color naranja, con "JS" junto al nombre del elemento.</span><span class="sxs-lookup"><span data-stu-id="e76b3-259">Notice that instead of being highlighted in blue, the element is highlighted in orange, with "JS" next to the element name.</span></span> <span data-ttu-id="e76b3-260">Esto indica que el elemento se creó dinámicamente a través del script.</span><span class="sxs-lookup"><span data-stu-id="e76b3-260">This indicates that the element was created dynamically through script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

<span data-ttu-id="e76b3-261">Además, aparece un subrayado de color naranja en la **pila de llamadas** ficha. Esto indica que el **pila de llamadas** panel tiene más información sobre el elemento.</span><span class="sxs-lookup"><span data-stu-id="e76b3-261">In addition, an orange underline appears on the **Call Stack** tab. This indicates that the **Call Stack** pane has more information about the element.</span></span>

<span data-ttu-id="e76b3-262">Haga clic en el **pila de llamadas** ficha. El **pila de llamadas** panel muestra la pila de llamadas de la llamada de JavaScript que crea el elemento.</span><span class="sxs-lookup"><span data-stu-id="e76b3-262">Click on the **Call Stack** tab. The **Call Stack** pane shows the call stack for the JavaScript call that created the element.</span></span> <span data-ttu-id="e76b3-263">Las llamadas a bibliotecas externas, como jQuery se contraen, por lo que puede ver fácilmente las llamadas a la secuencia de comandos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e76b3-263">Calls to external libraries such as jQuery are collapsed, so that you can easily see the calls to your application script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

<span data-ttu-id="e76b3-264">Para ver la pila completa, incluidas las llamadas a bibliotecas externas, puede expandir los nodos con la etiqueta "Bibliotecas externas":</span><span class="sxs-lookup"><span data-stu-id="e76b3-264">To see the full stack, including calls to external libraries, you can expand the nodes labeled "External Libraries":</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

<span data-ttu-id="e76b3-265">Si hace clic en un elemento en la pila de llamadas, Visual Studio abre el archivo de código y resalta la secuencia de comandos correspondiente.</span><span class="sxs-lookup"><span data-stu-id="e76b3-265">If you click an item in the call stack, Visual Studio opens the code file and highlights the corresponding script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a><span data-ttu-id="e76b3-266">Vea también</span><span class="sxs-lookup"><span data-stu-id="e76b3-266">See Also</span></span>

<span data-ttu-id="e76b3-267">[Introducción a ASP.NET MVC 4 con Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (sitio Web de ASP.net)</span><span class="sxs-lookup"><span data-stu-id="e76b3-267">[Intro to ASP.NET MVC 4 with Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net website)</span></span>

<span data-ttu-id="e76b3-268">[Presentación de Inspector de página](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (vídeo de Channel 9)</span><span class="sxs-lookup"><span data-stu-id="e76b3-268">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)</span></span>

<span data-ttu-id="e76b3-269">[Los mensajes de Error del Inspector de página](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="e76b3-269">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
