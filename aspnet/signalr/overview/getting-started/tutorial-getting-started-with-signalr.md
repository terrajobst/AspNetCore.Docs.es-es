---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Tutorial: Introducción a SignalR 2 | Microsoft Docs'
author: pfletcher
description: En este tutorial se muestra cómo usar SignalR para crear una aplicación de chat en tiempo real. Agregará SignalR a una aplicación web ASP.NET vacía y cree a un pa HTML...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 647dab496acd63dc774236ed448bd6b37b19c707
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837282"
---
<a name="tutorial-getting-started-with-signalr-2"></a><span data-ttu-id="a283c-104">Tutorial: Introducción a SignalR 2</span><span class="sxs-lookup"><span data-stu-id="a283c-104">Tutorial: Getting Started with SignalR 2</span></span>
====================
<span data-ttu-id="a283c-105">por [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="a283c-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="a283c-106">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="a283c-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> <span data-ttu-id="a283c-107">En este tutorial se muestra cómo usar SignalR para crear una aplicación de chat en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="a283c-107">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="a283c-108">Agregará SignalR a una aplicación web ASP.NET vacía y cree una página HTML para enviar y mostrar mensajes.</span><span class="sxs-lookup"><span data-stu-id="a283c-108">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span> 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a283c-109">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="a283c-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="a283c-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="a283c-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="a283c-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="a283c-111">.NET 4.5</span></span>
> - <span data-ttu-id="a283c-112">Versión 2 de SignalR</span><span class="sxs-lookup"><span data-stu-id="a283c-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="a283c-113">Uso de Visual Studio 2012 con este tutorial</span><span class="sxs-lookup"><span data-stu-id="a283c-113">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="a283c-114">Para usar Visual Studio 2012 con este tutorial, realice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a283c-114">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="a283c-115">Actualización de su [Administrador de paquetes](http://docs.nuget.org/docs/start-here/installing-nuget) a la versión más reciente.</span><span class="sxs-lookup"><span data-stu-id="a283c-115">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="a283c-116">Instalar el [instalador de plataforma Web](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="a283c-116">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="a283c-117">En el instalador de plataforma Web, buscar e instalar **ASP.NET y Web Tools 2013.1 para Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="a283c-117">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="a283c-118">Este modo instalará como plantillas de Visual Studio para las clases de SignalR **concentrador**.</span><span class="sxs-lookup"><span data-stu-id="a283c-118">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="a283c-119">Algunas plantillas (como **clase de inicio OWIN**) no estará disponible; para ello, use un archivo de clase en su lugar.</span><span class="sxs-lookup"><span data-stu-id="a283c-119">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="a283c-120">Versiones de tutoriales</span><span class="sxs-lookup"><span data-stu-id="a283c-120">Tutorial versions</span></span>
> 
> <span data-ttu-id="a283c-121">Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="a283c-121">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="a283c-122">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="a283c-122">Questions and comments</span></span>
> 
> <span data-ttu-id="a283c-123">Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="a283c-123">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="a283c-124">Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="a283c-124">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="a283c-125">Información general</span><span class="sxs-lookup"><span data-stu-id="a283c-125">Overview</span></span>

<span data-ttu-id="a283c-126">Este tutorial presenta SignalR desarrollo mostrándole cómo compilar una aplicación de chat basada en explorador sencillo.</span><span class="sxs-lookup"><span data-stu-id="a283c-126">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="a283c-127">Agregará la biblioteca SignalR para una aplicación web vacía de ASP.NET, cree una clase de hub para enviar mensajes a los clientes y crear una página HTML que permite a los usuarios enviar y recibir mensajes de chat.</span><span class="sxs-lookup"><span data-stu-id="a283c-127">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="a283c-128">Para ver un tutorial similar que muestra cómo crear una aplicación de chat en MVC 5 con una vista de MVC, consulte [Introducción a SignalR 2 y MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="a283c-128">For a similar tutorial that shows how to create a chat application in MVC 5 using an MVC view, see [Getting Started with SignalR 2 and MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

> [!NOTE]
> <span data-ttu-id="a283c-129">Este tutorial muestra cómo crear aplicaciones SignalR en la versión 2.</span><span class="sxs-lookup"><span data-stu-id="a283c-129">This tutorial demonstrates how to create SignalR applications in version 2.</span></span> <span data-ttu-id="a283c-130">Para obtener más información sobre los cambios entre SignalR 1.x y 2, consulte [SignalR actualizar proyectos de 1.x](../releases/upgrading-signalr-1x-projects-to-20.md) y [notas de la versión de Visual Studio 2013](../../../visual-studio/overview/2013/release-notes.md#TOC13).</span><span class="sxs-lookup"><span data-stu-id="a283c-130">For details on changes between SignalR 1.x and 2, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md) and [Visual Studio 2013 Release Notes](../../../visual-studio/overview/2013/release-notes.md#TOC13).</span></span>

<span data-ttu-id="a283c-131">SignalR es una biblioteca de .NET de código abierto para compilar aplicaciones web que requieren la interacción del usuario en directo o actualizaciones de datos en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="a283c-131">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="a283c-132">Algunos ejemplos son aplicaciones sociales, juegos multiusuario, meteorológicos de colaboración y de noticias, business o aplicaciones financieras de actualización.</span><span class="sxs-lookup"><span data-stu-id="a283c-132">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="a283c-133">A menudo se denominan aplicaciones en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="a283c-133">These are often called real-time applications.</span></span>

<span data-ttu-id="a283c-134">SignalR simplifica el proceso de creación de aplicaciones en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="a283c-134">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="a283c-135">Incluye una biblioteca de servidor ASP.NET y una biblioteca de cliente de JavaScript para facilitar la administración de conexiones de cliente / servidor e insertar las actualizaciones de contenido a los clientes.</span><span class="sxs-lookup"><span data-stu-id="a283c-135">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="a283c-136">Puede agregar la biblioteca de SignalR a una aplicación ASP.NET existente para obtener la funcionalidad en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="a283c-136">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="a283c-137">El tutorial muestra las siguientes tareas de desarrollo de SignalR:</span><span class="sxs-lookup"><span data-stu-id="a283c-137">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="a283c-138">Agregar la biblioteca de SignalR a una aplicación web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a283c-138">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="a283c-139">Creación de una clase de concentrador para insertar contenido en los clientes.</span><span class="sxs-lookup"><span data-stu-id="a283c-139">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="a283c-140">Creación de una clase de inicio OWIN para configurar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a283c-140">Creating an OWIN startup class to configure the application.</span></span>
- <span data-ttu-id="a283c-141">Uso de la biblioteca de jQuery SignalR en una página web para enviar mensajes y mostrar las actualizaciones desde el centro.</span><span class="sxs-lookup"><span data-stu-id="a283c-141">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="a283c-142">Captura de pantalla siguiente muestra la aplicación de chat que se ejecuta en un explorador.</span><span class="sxs-lookup"><span data-stu-id="a283c-142">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="a283c-143">Cada nuevo usuario puede publicar comentarios y ver comentarios agregados después de que el usuario une a la conversación.</span><span class="sxs-lookup"><span data-stu-id="a283c-143">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Instancias del chat](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="a283c-145">Secciones:</span><span class="sxs-lookup"><span data-stu-id="a283c-145">Sections:</span></span>

- [<span data-ttu-id="a283c-146">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="a283c-146">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="a283c-147">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="a283c-147">Run the Sample</span></span>](#run)
- [<span data-ttu-id="a283c-148">Examine el código</span><span class="sxs-lookup"><span data-stu-id="a283c-148">Examine the Code</span></span>](#code)
- [<span data-ttu-id="a283c-149">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="a283c-149">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="a283c-150">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="a283c-150">Set up the Project</span></span>

<span data-ttu-id="a283c-151">En esta sección se muestra cómo usar Visual Studio 2013 y la versión 2 de SignalR para crear una aplicación web ASP.NET vacía, agregar SignalR y crear la aplicación de chat.</span><span class="sxs-lookup"><span data-stu-id="a283c-151">This section shows how to use Visual Studio 2013 and SignalR version 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="a283c-152">Requisitos previos:</span><span class="sxs-lookup"><span data-stu-id="a283c-152">Prerequisites:</span></span>

- <span data-ttu-id="a283c-153">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="a283c-153">Visual Studio 2013.</span></span> <span data-ttu-id="a283c-154">Si no tiene Visual Studio, consulte [descargas de ASP.NET](https://www.asp.net/downloads) para obtener el Visual Studio 2013 Express herramienta de desarrollo gratuita.</span><span class="sxs-lookup"><span data-stu-id="a283c-154">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express Development Tool.</span></span>

<span data-ttu-id="a283c-155">Los pasos siguientes usan Visual Studio 2013 para crear una aplicación Web ASP.NET vacía y agregar la biblioteca SignalR:</span><span class="sxs-lookup"><span data-stu-id="a283c-155">The following steps use Visual Studio 2013 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="a283c-156">En Visual Studio, cree una aplicación Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a283c-156">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Creación de web](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="a283c-158">En el **nuevo proyecto ASP.NET** ventana, deje **vacía** seleccionado y haga clic en **crear proyecto**.</span><span class="sxs-lookup"><span data-stu-id="a283c-158">In the **New ASP.NET Project** window, leave **Empty** selected and click **Create Project**.</span></span>

    ![Crear sitio web vacío](tutorial-getting-started-with-signalr/_static/image3.png)
3. <span data-ttu-id="a283c-160">En **el Explorador de soluciones**, haga clic en el proyecto, seleccione **Add | Clase de concentrador SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="a283c-160">In **Solution Explorer**, right-click the project, select **Add | SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="a283c-161">Nombre de la clase **ChatHub.cs** y agréguelo al proyecto.</span><span class="sxs-lookup"><span data-stu-id="a283c-161">Name the class **ChatHub.cs** and add it to the project.</span></span> <span data-ttu-id="a283c-162">Este paso se crea el **ChatHub** clase y un conjunto de archivos de script y las referencias de ensamblado que admiten SignalR se agrega al proyecto.</span><span class="sxs-lookup"><span data-stu-id="a283c-162">This step creates the **ChatHub** class and adds to the project a set of script files and assembly references that support SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a283c-163">También puede agregar SignalR a un proyecto abriendo el **herramientas | Administrador de paquetes de biblioteca | Consola de administrador de paquetes** y ejecutar un comando:</span><span class="sxs-lookup"><span data-stu-id="a283c-163">You can also add SignalR to a project by opening the **Tools | Library Package Manager | Package Manager Console** and running a command:</span></span>

    `install-package Microsoft.AspNet.SignalR`

    <span data-ttu-id="a283c-164">Si utiliza la consola para agregar SignalR, cree la clase de concentrador SignalR como un paso independiente después de agregar SignalR.</span><span class="sxs-lookup"><span data-stu-id="a283c-164">If you use the console to add SignalR, create the SignalR hub class as a separate step after you add SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a283c-165">Si usa Visual Studio 2012, el **clase de concentrador de SignalR (v2)** plantilla no estará disponible.</span><span class="sxs-lookup"><span data-stu-id="a283c-165">If you are using Visual Studio 2012, the **SignalR Hub Class (v2)** template will not be available.</span></span> <span data-ttu-id="a283c-166">Puede agregar un simple **clase** llamado `ChatHub` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="a283c-166">You can add a plain **Class** called `ChatHub` instead.</span></span>
4. <span data-ttu-id="a283c-167">En **el Explorador de soluciones**, expanda el nodo secuencias de comandos.</span><span class="sxs-lookup"><span data-stu-id="a283c-167">In **Solution Explorer**, expand the Scripts node.</span></span> <span data-ttu-id="a283c-168">Bibliotecas de scripts de jQuery y SignalR están visibles en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="a283c-168">Script libraries for jQuery and SignalR are visible in the project.</span></span>
5. <span data-ttu-id="a283c-169">Reemplace el código en el nuevo **ChatHub** con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="a283c-169">Replace the code in the new **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="a283c-170">En **el Explorador de soluciones**, haga clic en el proyecto y luego haga clic en **Add | Clase de inicio OWIN**.</span><span class="sxs-lookup"><span data-stu-id="a283c-170">In **Solution Explorer**, right-click the project, then click **Add | OWIN Startup Class**.</span></span> <span data-ttu-id="a283c-171">Nombre de la nueva clase `Startup` y haga clic en Aceptar.</span><span class="sxs-lookup"><span data-stu-id="a283c-171">Name the new class `Startup` and click OK.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a283c-172">Si usa Visual Studio 2012, el **clase de inicio OWIN** plantilla no estará disponible.</span><span class="sxs-lookup"><span data-stu-id="a283c-172">If you are using Visual Studio 2012, the **OWIN Startup Class** template will not be available.</span></span> <span data-ttu-id="a283c-173">Puede agregar un simple **clase** llamado `Startup` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="a283c-173">You can add a plain **Class** called `Startup` instead.</span></span>
7. <span data-ttu-id="a283c-174">Cambiar el contenido de la nueva clase de inicio a la siguiente.</span><span class="sxs-lookup"><span data-stu-id="a283c-174">Change the contents of the new Startup class to the following.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="a283c-175">En **el Explorador de soluciones**, haga clic en el proyecto y luego haga clic en **Add | Página HTML**.</span><span class="sxs-lookup"><span data-stu-id="a283c-175">In **Solution Explorer**, right-click the project, then click **Add | HTML Page**.</span></span> <span data-ttu-id="a283c-176">Nombre de la nueva página `index.html`.</span><span class="sxs-lookup"><span data-stu-id="a283c-176">Name the new page `index.html`.</span></span>
    >[!NOTE]
    ><span data-ttu-id="a283c-177">Es posible que deba cambiar los números de versión para las referencias a bibliotecas de JQuery y SignalR</span><span class="sxs-lookup"><span data-stu-id="a283c-177">You might need to change the version numbers for the references to JQuery and SignalR libraries</span></span>
9. <span data-ttu-id="a283c-178">En **el Explorador de soluciones**, haga clic en la página HTML que acaba de crear y haga clic en **establecer como página de inicio**.</span><span class="sxs-lookup"><span data-stu-id="a283c-178">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
10. <span data-ttu-id="a283c-179">Reemplace el código predeterminado en la página HTML con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="a283c-179">Replace the default code in the HTML page with the following code.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a283c-180">Una versión posterior de los scripts de SignalR puede instalarse mediante el Administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="a283c-180">A later version of the SignalR scripts may be installed by the package manager.</span></span> <span data-ttu-id="a283c-181">Compruebe que las referencias del script se corresponden con las versiones de los archivos de script en el proyecto (que será diferentes si agrega mediante NuGet, en lugar de agregar un concentrador de SignalR.)</span><span class="sxs-lookup"><span data-stu-id="a283c-181">Verify that the script references below correspond to the versions of the script files in the project (they will be different if you added SignalR using NuGet rather than adding a hub.)</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. <span data-ttu-id="a283c-182">**Guardar todo** para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="a283c-182">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="a283c-183">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="a283c-183">Run the Sample</span></span>

1. <span data-ttu-id="a283c-184">Presione F5 para ejecutar el proyecto en modo de depuración.</span><span class="sxs-lookup"><span data-stu-id="a283c-184">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="a283c-185">Carga la página HTML en una instancia del explorador y solicitará un nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="a283c-185">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr/_static/image4.png)
2. <span data-ttu-id="a283c-187">Escriba un nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="a283c-187">Enter a user name.</span></span>
3. <span data-ttu-id="a283c-188">Copie la dirección URL desde la línea de dirección del explorador y usarlo para abrir las instancias del explorador más dos.</span><span class="sxs-lookup"><span data-stu-id="a283c-188">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="a283c-189">En cada instancia del explorador, escriba un nombre de usuario único.</span><span class="sxs-lookup"><span data-stu-id="a283c-189">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="a283c-190">En cada instancia del explorador, agregue un comentario y haga clic en **enviar**.</span><span class="sxs-lookup"><span data-stu-id="a283c-190">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="a283c-191">Los comentarios deben aparecer en todas las instancias del explorador.</span><span class="sxs-lookup"><span data-stu-id="a283c-191">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a283c-192">Esta aplicación de chat simple no mantiene el contexto de discusión en el servidor.</span><span class="sxs-lookup"><span data-stu-id="a283c-192">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="a283c-193">El concentrador difunde comentarios a todos los usuarios actuales.</span><span class="sxs-lookup"><span data-stu-id="a283c-193">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="a283c-194">Los usuarios que se unan a la charla más adelante verá mensajes agregados desde el momento en que se unen a.</span><span class="sxs-lookup"><span data-stu-id="a283c-194">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="a283c-195">Captura de pantalla siguiente muestra la aplicación de chat que se ejecutan en tres instancias del explorador, todos ellos se actualizan cuando una instancia envía un mensaje:</span><span class="sxs-lookup"><span data-stu-id="a283c-195">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Exploradores de chat](tutorial-getting-started-with-signalr/_static/image5.png)
5. <span data-ttu-id="a283c-197">En **el Explorador de soluciones**, inspeccionar la **documentos de Script** nodo para la aplicación en ejecución.</span><span class="sxs-lookup"><span data-stu-id="a283c-197">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="a283c-198">Hay un archivo de script denominado **hubs** que la biblioteca SignalR genera dinámicamente en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="a283c-198">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="a283c-199">Este archivo administra la comunicación entre el script de jQuery y el código de servidor.</span><span class="sxs-lookup"><span data-stu-id="a283c-199">This file manages the communication between jQuery script and server-side code.</span></span>

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="a283c-200">Examine el código</span><span class="sxs-lookup"><span data-stu-id="a283c-200">Examine the Code</span></span>

<span data-ttu-id="a283c-201">La aplicación de chat de SignalR muestra dos tareas básicas de desarrollo de SignalR: crear un centro de que el objeto principal de coordinación en el servidor y mediante la biblioteca de jQuery de SignalR para enviar y recibir mensajes.</span><span class="sxs-lookup"><span data-stu-id="a283c-201">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="a283c-202">Concentradores de SignalR</span><span class="sxs-lookup"><span data-stu-id="a283c-202">SignalR Hubs</span></span>

<span data-ttu-id="a283c-203">En el ejemplo de código la **ChatHub** clase se deriva de la **Microsoft.AspNet.SignalR.Hub** clase.</span><span class="sxs-lookup"><span data-stu-id="a283c-203">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="a283c-204">Derivar de la **concentrador** clase es una forma útil de compilar una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="a283c-204">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="a283c-205">Puede crear métodos públicos en la clase de concentrador y, a continuación, obtener acceso a esos métodos al llamarlas desde secuencias de comandos en una página web.</span><span class="sxs-lookup"><span data-stu-id="a283c-205">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="a283c-206">En el código de chat, llame los clientes la **ChatHub.Send** método para enviar un mensaje nuevo.</span><span class="sxs-lookup"><span data-stu-id="a283c-206">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="a283c-207">El concentrador a su vez envía el mensaje a todos los clientes mediante una llamada a **Clients.All.broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="a283c-207">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="a283c-208">El **enviar** método muestra varios conceptos de hub:</span><span class="sxs-lookup"><span data-stu-id="a283c-208">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="a283c-209">Declarar métodos públicos en un centro de modo que los clientes se les pueden llamar.</span><span class="sxs-lookup"><span data-stu-id="a283c-209">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="a283c-210">Use la **Microsoft.AspNet.SignalR.Hub.Clients** propiedades dinámicas para tener acceso a todos los clientes conectados a este centro.</span><span class="sxs-lookup"><span data-stu-id="a283c-210">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="a283c-211">Llamar a una función en el cliente (como el `broadcastMessage` función) para actualizar los clientes.</span><span class="sxs-lookup"><span data-stu-id="a283c-211">Call a function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="a283c-212">SignalR y jQuery</span><span class="sxs-lookup"><span data-stu-id="a283c-212">SignalR and jQuery</span></span>

<span data-ttu-id="a283c-213">La página HTML en el ejemplo de código muestra cómo usar la biblioteca de jQuery SignalR para comunicarse con un concentrador SignalR.</span><span class="sxs-lookup"><span data-stu-id="a283c-213">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="a283c-214">Las tareas esenciales en el código declara un proxy para hacer referencia el centro, declarar una función que el servidor puede llamar para insertar contenido a los clientes y, a partir de una conexión para enviar mensajes al concentrador.</span><span class="sxs-lookup"><span data-stu-id="a283c-214">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="a283c-215">El código siguiente declara una referencia a un proxy de concentrador.</span><span class="sxs-lookup"><span data-stu-id="a283c-215">The following code declares a reference to a hub proxy.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="a283c-216">En JavaScript, la referencia a la clase de servidor y sus miembros está en mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="a283c-216">In JavaScript the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="a283c-217">El ejemplo de código hace referencia a C# **ChatHub** clases en JavaScript como **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="a283c-217">The code sample references the C# **ChatHub** class in JavaScript as **chatHub**.</span></span>


<span data-ttu-id="a283c-218">El código siguiente es cómo crear una función de devolución de llamada en la secuencia de comandos.</span><span class="sxs-lookup"><span data-stu-id="a283c-218">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="a283c-219">La clase de hub en el servidor llama a esta función para insertar las actualizaciones de contenido en cada cliente.</span><span class="sxs-lookup"><span data-stu-id="a283c-219">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="a283c-220">Las dos líneas que codifica como HTML el contenido antes de mostrarla son opcionales y muestran una manera sencilla de evitar la inyección de script.</span><span class="sxs-lookup"><span data-stu-id="a283c-220">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="a283c-221">El código siguiente muestra cómo abrir una conexión con el centro.</span><span class="sxs-lookup"><span data-stu-id="a283c-221">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="a283c-222">El código inicia la conexión y, a continuación, pasa una función para controlar el evento click en la **enviar** botón en la página HTML.</span><span class="sxs-lookup"><span data-stu-id="a283c-222">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="a283c-223">Este enfoque garantiza que la conexión se establece antes de que se ejecuta el controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="a283c-223">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="a283c-224">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="a283c-224">Next Steps</span></span>

<span data-ttu-id="a283c-225">Ha aprendido que SignalR es un marco para crear aplicaciones web en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="a283c-225">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="a283c-226">También ha aprendido varias tareas de desarrollo de SignalR: cómo agregar SignalR a una aplicación ASP.NET, cómo crear una clase de hub y cómo enviar y recibir mensajes desde el centro.</span><span class="sxs-lookup"><span data-stu-id="a283c-226">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="a283c-227">Para ver un tutorial sobre cómo implementar la aplicación de ejemplo SignalR en Azure, consulte [usando SignalR con Web Apps en Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span><span class="sxs-lookup"><span data-stu-id="a283c-227">For a walkthrough on how to deploy the sample SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="a283c-228">Para obtener información detallada sobre cómo implementar un proyecto web de Visual Studio a un sitio Web de Windows Azure, consulte [crear una aplicación web ASP.NET en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="a283c-228">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<span data-ttu-id="a283c-229">Para obtener información sobre los conceptos más avanzados de los desarrollos de SignalR, visite los sitios siguientes para recursos y código fuente de SignalR:</span><span class="sxs-lookup"><span data-stu-id="a283c-229">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="a283c-230">Proyecto de SignalR</span><span class="sxs-lookup"><span data-stu-id="a283c-230">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="a283c-231">SignalR Github y ejemplos</span><span class="sxs-lookup"><span data-stu-id="a283c-231">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="a283c-232">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="a283c-232">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
