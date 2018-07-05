---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programación de ASP.NET Web Pages (Razor) con Visual Studio | Microsoft Docs
author: tfitzmac
description: En este apéndice se explica cómo puede usar Visual Studio 2010 o Visual Web Developer 2010 Express programar ASP.NET Web Pages con la sintaxis Razor.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2014
ms.topic: article
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: b7f9a6c2d55d31dc918d2b2e542e26639a54b39a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380368"
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="3b15c-103">Programación de ASP.NET Web Pages (Razor) con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3b15c-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>
====================
<span data-ttu-id="3b15c-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3b15c-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3b15c-105">En este artículo se explica cómo puede usar Visual Studio o Visual Web Developer Express para sitios Web de ASP.NET Web Pages (Razor) del programa.</span><span class="sxs-lookup"><span data-stu-id="3b15c-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
> 
> <span data-ttu-id="3b15c-106">¿Qué aprenderá</span><span class="sxs-lookup"><span data-stu-id="3b15c-106">What you'll learn</span></span>
> 
> - <span data-ttu-id="3b15c-107">Lo que necesita instalar (si hay alguna) para que funcione con ASP.NET Web Pages en su versión de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3b15c-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="3b15c-108">Cómo agregar compatibilidad para ASP.NET Web Pages para Visual Web Developer 2010 Express.</span><span class="sxs-lookup"><span data-stu-id="3b15c-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="3b15c-109">Cómo utilizar las características de Visual Studio para trabajar con páginas de Razor de ASP.NET, incluidos IntelliSense y el depurador.</span><span class="sxs-lookup"><span data-stu-id="3b15c-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3b15c-110">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="3b15c-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="3b15c-111">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="3b15c-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="3b15c-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3b15c-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="3b15c-113">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="3b15c-113">WebMatrix 3</span></span>
>   
> 
> <span data-ttu-id="3b15c-114">Este tutorial también funciona con ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010 y WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="3b15c-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>


<span data-ttu-id="3b15c-115">Puede programar ASP.NET Web pages con sintaxis Razor mediante WebMatrix o muchos otros editores de código.</span><span class="sxs-lookup"><span data-stu-id="3b15c-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="3b15c-116">También puede utilizar Microsoft Visual Studio, que es un entorno completo de desarrollo integrado (IDE) que proporciona un eficaz conjunto de herramientas para crear muchos tipos de aplicaciones (no solo los sitios Web).</span><span class="sxs-lookup"><span data-stu-id="3b15c-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="3b15c-117">Para trabajar con páginas de Razor de ASP.NET, puede usar una de las ediciones completas de Visual Studio o la versión gratuita [Visual Studio Express para Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.</span><span class="sxs-lookup"><span data-stu-id="3b15c-117">To work with ASP.NET Razor pages, you can either use one of the full editions of Visual Studio or the free [Visual Studio Express for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.</span></span>

<span data-ttu-id="3b15c-118">Son dos características muy útiles que proporciona Visual Studio para la programación con Razor de ASP.NET web pages:</span><span class="sxs-lookup"><span data-stu-id="3b15c-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="3b15c-119">*IntelliSense*.</span><span class="sxs-lookup"><span data-stu-id="3b15c-119">*IntelliSense*.</span></span> <span data-ttu-id="3b15c-120">La característica IntelliSense integrada en Visual Studio es más completa de IntelliSense en WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="3b15c-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="3b15c-121">*Depurador*.</span><span class="sxs-lookup"><span data-stu-id="3b15c-121">*Debugger*.</span></span> <span data-ttu-id="3b15c-122">El depurador le permite solucionar problemas del código mediante la detención de un programa mientras se está ejecutando, examinar las variables y recorrer el código línea por línea.</span><span class="sxs-lookup"><span data-stu-id="3b15c-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="3b15c-123">Con Visual Studio con distintas versiones de las páginas Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3b15c-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="3b15c-124">Visual Studio 2012 y Visual Studio 2013 incluyen compatibilidad para ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="3b15c-124">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="3b15c-125">(Los paquetes que son necesarios para admitir las páginas Web ASP.NET se instalan al instalar Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="3b15c-125">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="3b15c-126">Visual Studio 2010 no incluye compatibilidad predeterminada para ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="3b15c-126">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="3b15c-127">Para usar las páginas Web ASP.NET con Visual Studio 2010, debe instalar el paquete de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3b15c-127">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="3b15c-128">Para obtener ASP.NET Web Pages 2, instalar ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="3b15c-128">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="3b15c-129">En la tabla siguiente resume la compatibilidad para ASP.NET Web Pages en diferentes versiones de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3b15c-129">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="3b15c-130">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="3b15c-130">Visual Studio 2010</span></span> | <span data-ttu-id="3b15c-131">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="3b15c-131">Visual Studio 2012</span></span> | <span data-ttu-id="3b15c-132">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3b15c-132">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3b15c-133">**ASP.NET Web Pages 2**</span><span class="sxs-lookup"><span data-stu-id="3b15c-133">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="3b15c-134">Instalar ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="3b15c-134">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="3b15c-135">(Incluido)</span><span class="sxs-lookup"><span data-stu-id="3b15c-135">(Included)</span></span> | <span data-ttu-id="3b15c-136">(Incluido)</span><span class="sxs-lookup"><span data-stu-id="3b15c-136">(Included)</span></span> |
| <span data-ttu-id="3b15c-137">**3 de ASP.NET Web Pages**</span><span class="sxs-lookup"><span data-stu-id="3b15c-137">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="3b15c-138">Actualización a ASP.NET Web Pages 3 NuGet a través</span><span class="sxs-lookup"><span data-stu-id="3b15c-138">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="3b15c-139">(Incluido)</span><span class="sxs-lookup"><span data-stu-id="3b15c-139">(Included)</span></span> |

<span data-ttu-id="3b15c-140">Para trabajar con Visual Studio 2010, consulte [instalar compatibilidad para ASP.NET Web Pages en Visual Studio 2010](#vs2010support).</span><span class="sxs-lookup"><span data-stu-id="3b15c-140">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="3b15c-141">Iniciar Visual Studio desde WebMatrix</span><span class="sxs-lookup"><span data-stu-id="3b15c-141">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="3b15c-142">Si ha iniciado un proyecto en WebMatrix y desea cambiar a Visual Studio, WebMatrix proporciona un botón para abrir fácilmente el proyecto en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3b15c-142">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="3b15c-143">Debe tener instalado Visual Studio en el equipo para que este botón esté habilitado.</span><span class="sxs-lookup"><span data-stu-id="3b15c-143">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="3b15c-144">La siguiente imagen muestra el botón de WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="3b15c-144">The following image shows the button in WebMatrix.</span></span>

![Inicie Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="3b15c-146">Al hacer clic en el botón, se abre el proyecto en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3b15c-146">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="3b15c-147">Puede alternar entre WebMatrix y Visual Studio sin problemas.</span><span class="sxs-lookup"><span data-stu-id="3b15c-147">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="3b15c-148">Se le notificará si los archivos han cambiado en el otro entorno y necesitan volver a cargar para obtener los últimos cambios.</span><span class="sxs-lookup"><span data-stu-id="3b15c-148">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="3b15c-149">Creando el sitio de ASP.NET Razor en Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3b15c-149">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="3b15c-150">Para crear un sitio Web de ASP.NET Razor en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="3b15c-150">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="3b15c-151">Inicie Visual Studio o Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="3b15c-151">Start Visual Studio or Visual Web Developer.</span></span>
2. <span data-ttu-id="3b15c-152">En el **archivo** menú, haga clic en **nuevo sitio Web**.</span><span class="sxs-lookup"><span data-stu-id="3b15c-152">In the **File** menu, click **New Web Site**.</span></span>

    ![Crear nuevo sitio web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="3b15c-154">En el **nuevo sitio Web** cuadro de diálogo, seleccione el idioma que se usará (Visual C# o Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="3b15c-154">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="3b15c-155">Seleccione el **sitio Web de ASP.NET (Razor)** plantilla.</span><span class="sxs-lookup"><span data-stu-id="3b15c-155">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![sitio de Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="3b15c-157">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="3b15c-157">Click **OK**.</span></span>

<span data-ttu-id="3b15c-158">El nuevo proyecto existe y se rellena con algunas páginas de web predeterminada que le ayudarán a empezar a trabajar.</span><span class="sxs-lookup"><span data-stu-id="3b15c-158">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="3b15c-159">Using IntelliSense</span><span class="sxs-lookup"><span data-stu-id="3b15c-159">Using IntelliSense</span></span>

<span data-ttu-id="3b15c-160">Ahora que ha creado un sitio, puede ver el funcionamiento de IntelliSense en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3b15c-160">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="3b15c-161">En el sitio Web que acaba de crear, abrir el *Default.cshtml* página.</span><span class="sxs-lookup"><span data-stu-id="3b15c-161">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="3b15c-162">Después de la `<h3>` etiquetas en la página, escriba `@ServerInfo.` (incluido el punto).</span><span class="sxs-lookup"><span data-stu-id="3b15c-162">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="3b15c-163">Observe cómo IntelliSense muestra los métodos disponibles para el `ServerInfo` auxiliar en una lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="3b15c-163">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span> 

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="3b15c-165">Seleccione el `GetHtml` método en la lista y, a continuación, presione ENTRAR.</span><span class="sxs-lookup"><span data-stu-id="3b15c-165">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="3b15c-166">IntelliSense rellena automáticamente el método.</span><span class="sxs-lookup"><span data-stu-id="3b15c-166">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="3b15c-167">(Como con cualquier método en C#, debe agregar `()` caracteres después del método.)</span><span class="sxs-lookup"><span data-stu-id="3b15c-167">(As with any method in C#, you must add `()` characters after the method.)</span></span>  
   <span data-ttu-id="3b15c-168">El código completo de la `GetHtml` método es similar al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="3b15c-168">The completed code for the `GetHtml` method looks like the following example:</span></span>  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="3b15c-169">Presione Ctrl + F5 para ejecutar la página.</span><span class="sxs-lookup"><span data-stu-id="3b15c-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="3b15c-170">Esto es lo que la página es similar al que se muestra en un explorador:</span><span class="sxs-lookup"><span data-stu-id="3b15c-170">This is what the page looks like when displayed in a browser:</span></span> 

    ![página predeterminada de explorador](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="3b15c-172">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="3b15c-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="3b15c-173">Con el depurador</span><span class="sxs-lookup"><span data-stu-id="3b15c-173">Using the Debugger</span></span>

1. <span data-ttu-id="3b15c-174">En la parte superior de la *Default.cshtml* página después de la línea que comienza con `Page.Title`, agregue la siguiente línea de código:</span><span class="sxs-lookup"><span data-stu-id="3b15c-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span> 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="3b15c-175">En el margen gris del editor a la izquierda del código, haga clic en junto a esta nueva línea con el fin de agregar un *punto de interrupción*.</span><span class="sxs-lookup"><span data-stu-id="3b15c-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="3b15c-176">Un punto de interrupción es un marcador que indica al depurador que deje de ejecutarse en ese momento el programa para que pueda ver lo que sucede.</span><span class="sxs-lookup"><span data-stu-id="3b15c-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![Establecer punto de interrupción](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="3b15c-178">Quite la llamada a la `ServerInfo.GetHtml` método y agregue una llamada a la `@myTime` variables en su lugar.</span><span class="sxs-lookup"><span data-stu-id="3b15c-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="3b15c-179">Esta llamada muestra el valor de tiempo actual devuelto por la nueva línea de código.</span><span class="sxs-lookup"><span data-stu-id="3b15c-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="3b15c-180">Presione F5 para ejecutar la página en el depurador.</span><span class="sxs-lookup"><span data-stu-id="3b15c-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="3b15c-181">La página se detiene en el punto de interrupción que estableció.</span><span class="sxs-lookup"><span data-stu-id="3b15c-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="3b15c-182">La siguiente imagen muestra la página de aspecto en el editor con el punto de interrupción (amarillo).</span><span class="sxs-lookup"><span data-stu-id="3b15c-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span> 

    ![punto de interrupción de depuración](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="3b15c-184">En la barra de herramientas de depuración, haga clic en el **paso a paso** botón (o presione F11) para ejecutar la siguiente línea de código.</span><span class="sxs-lookup"><span data-stu-id="3b15c-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="3b15c-185">Cada vez que haga clic en este botón, la ejecución puede avanzar a la siguiente línea de código.</span><span class="sxs-lookup"><span data-stu-id="3b15c-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![ENTRAR en el botón](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="3b15c-187">Examine el valor de la `myTime` variable manteniendo el puntero del mouse sobre él o mediante la inspección de los valores mostrados en la **variables locales** y **pila de llamadas** windows.</span><span class="sxs-lookup"><span data-stu-id="3b15c-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="3b15c-188">Visual Studio muestra el valor de la variable.</span><span class="sxs-lookup"><span data-stu-id="3b15c-188">Visual Studio display the value of the variable.</span></span>

    ![valor de tiempo de presentación](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="3b15c-190">Cuando haya terminado el examen de la variable y recorrer el código, presione F5 para continuar la ejecución de la página sin detenerse en cada línea.</span><span class="sxs-lookup"><span data-stu-id="3b15c-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="3b15c-191">Cuando haya terminado de recorrer todo el código, el explorador muestra la página.</span><span class="sxs-lookup"><span data-stu-id="3b15c-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="3b15c-192">Para obtener más información acerca del depurador y depuración de código en Visual Studio, vea [Tutorial: depuración de páginas Web en Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span><span class="sxs-lookup"><span data-stu-id="3b15c-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="3b15c-193">Uso de Razor en proyectos de ASP.NET MVC con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3b15c-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="3b15c-194">La sintaxis de Razor también se usa habitualmente en proyectos de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3b15c-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="3b15c-195">MVC es una manera eficaz, basado en patrones para crear sitios Web dinámicos.</span><span class="sxs-lookup"><span data-stu-id="3b15c-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="3b15c-196">Si su sitio ASP.NET Web Pages pasa a ser difícil de mantener, es posible que desee considere convertirla en una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3b15c-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="3b15c-197">Para obtener un ejemplo de creación de una aplicación MVC, consulte [Introducción a ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="3b15c-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="3b15c-198">Instalar compatibilidad para las páginas Web ASP.NET en Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="3b15c-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="3b15c-199">En esta sección se muestra cómo instalar Visual Web Developer Express 2010 y las herramientas de ASP.NET Web Pages para Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3b15c-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="3b15c-200">Si aún no tiene el instalador de plataforma Web, puede descargarlo desde la dirección URL siguiente:</span><span class="sxs-lookup"><span data-stu-id="3b15c-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="3b15c-201">Ejecute al instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="3b15c-201">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="3b15c-202">Haga clic en el **productos** ficha.</span><span class="sxs-lookup"><span data-stu-id="3b15c-202">Click the **Products** tab.</span></span>

    ![Pestaña productos de WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="3b15c-204">Busque **ASP.NET MVC 4** (para ASP.NET Web Pages 2) y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="3b15c-204">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="3b15c-205">Estos productos incluyen herramientas de Visual Studio para compilar sitios Web de ASP.NET Razor.</span><span class="sxs-lookup"><span data-stu-id="3b15c-205">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![Opciones de instalación de WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="3b15c-207">Haga clic en **instalar** para completar la instalación.</span><span class="sxs-lookup"><span data-stu-id="3b15c-207">Click **Install** to complete the installation.</span></span>
