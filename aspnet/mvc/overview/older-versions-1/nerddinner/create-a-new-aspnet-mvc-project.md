---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Cree un nuevo proyecto de ASP.NET MVC | Microsoft Docs
author: microsoft
description: Paso 1 muestra cómo colocar la estructura básica de la aplicación NerdDinner en su lugar.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 3f34f17aa35dbfed2d52daf615c8dc81be6e7847
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837351"
---
<a name="create-a-new-aspnet-mvc-project"></a><span data-ttu-id="49ca9-103">Cree un nuevo proyecto de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="49ca9-103">Create a New ASP.NET MVC Project</span></span>
====================
<span data-ttu-id="49ca9-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="49ca9-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="49ca9-105">Descargar PDF</span><span class="sxs-lookup"><span data-stu-id="49ca9-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="49ca9-106">Este es el paso 1 de manera gratuita ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que le guía a través cómo compilar un pequeño, pero completa, aplicación web mediante ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="49ca9-106">This is step 1 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="49ca9-107">Paso 1 muestra cómo colocar la estructura básica de la aplicación NerdDinner en su lugar.</span><span class="sxs-lookup"><span data-stu-id="49ca9-107">Step 1 shows you how to put the basic NerdDinner application structure in place.</span></span>
> 
> <span data-ttu-id="49ca9-108">Si usa ASP.NET MVC 3, se recomienda que siga el [Introducción a trabajar con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.</span><span class="sxs-lookup"><span data-stu-id="49ca9-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-1-file-gtnew-project"></a><span data-ttu-id="49ca9-109">Paso 1 de NerdDinner: Archivo -&gt;nuevo proyecto</span><span class="sxs-lookup"><span data-stu-id="49ca9-109">NerdDinner Step 1: File-&gt;New Project</span></span>

<span data-ttu-id="49ca9-110">Comenzaremos también nuestra aplicación NerdDinner seleccionando el **archivo -&gt;nuevo proyecto** elemento de menú dentro de Visual Studio 2008 o el gratuita Visual Web Developer 2008 Express.</span><span class="sxs-lookup"><span data-stu-id="49ca9-110">We'll begin our NerdDinner application by selecting the **File-&gt;New Project** menu item within either Visual Studio 2008 or the free Visual Web Developer 2008 Express.</span></span>

<span data-ttu-id="49ca9-111">Se abrirá el cuadro de diálogo "Nuevo proyecto".</span><span class="sxs-lookup"><span data-stu-id="49ca9-111">This will bring up the "New Project" dialog.</span></span> <span data-ttu-id="49ca9-112">Para crear una nueva aplicación MVC de ASP.NET, se deberá seleccionar el nodo "Web" en el lado izquierdo del cuadro de diálogo y, a continuación, elija la plantilla de proyecto "Aplicación Web de ASP.NET MVC" de la derecha:</span><span class="sxs-lookup"><span data-stu-id="49ca9-112">To create a new ASP.NET MVC application, we'll select the "Web" node on the left-hand side of the dialog and then choose the "ASP.NET MVC Web Application" project template on the right:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image1.png)

<span data-ttu-id="49ca9-113">*Importante: Asegúrese de que ha descargado e instalado ASP.NET MVC: en caso contrario no se mostrará en el cuadro de diálogo nuevo proyecto. Puede usar la versión V2 de la [instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) si aún no ha instalado todavía (ASP.NET MVC está disponible dentro de la "plataforma Web -&gt;marcos de trabajo y los tiempos de ejecución" sección).*</span><span class="sxs-lookup"><span data-stu-id="49ca9-113">*Important: Make sure you've downloaded and installed ASP.NET MVC - otherwise it won't show up in the New Project dialog. You can use V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) if you haven't installed it yet (ASP.NET MVC is available within the "Web Platform-&gt;Frameworks and Runtimes" section).*</span></span>

<span data-ttu-id="49ca9-114">Llamaremos al nuevo proyecto, vamos a crear "NerdDinner" y, a continuación, haga clic en el botón "Aceptar" para crearlo.</span><span class="sxs-lookup"><span data-stu-id="49ca9-114">We'll name the new project we are going to create "NerdDinner" and then click the "ok" button to create it.</span></span>

<span data-ttu-id="49ca9-115">Cuando hacemos clic en "Aceptar" se abrirá un cuadro de diálogo adicional que nos pedirá que, opcionalmente, cree un proyecto de prueba unitaria para la nueva aplicación en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="49ca9-115">When we click "ok" Visual Studio will bring up an additional dialog that prompts us to optionally create a unit test project for the new application as well.</span></span> <span data-ttu-id="49ca9-116">Este proyecto de prueba unitaria nos permite crear pruebas automatizadas que comprueben la funcionalidad y el comportamiento de nuestra aplicación (algo que trataremos cómo la tarea más adelante en este tutorial).</span><span class="sxs-lookup"><span data-stu-id="49ca9-116">This unit test project enables us to create automated tests that verify the functionality and behavior of our application (something we'll cover how to-do later in this tutorial).</span></span>

![](create-a-new-aspnet-mvc-project/_static/image2.png)

<span data-ttu-id="49ca9-117">La lista desplegable de "Marco de pruebas" en el cuadro de diálogo anterior se rellena con todos los disponibles ASP.NET MVC proyecto plantillas de pruebas unitarias instaladas en el equipo.</span><span class="sxs-lookup"><span data-stu-id="49ca9-117">The "Test framework" dropdown in the above dialog is populated with all available ASP.NET MVC unit test project templates installed on the machine.</span></span> <span data-ttu-id="49ca9-118">Las versiones se pueden descargar para NUnit, MBUnit y XUnit.</span><span class="sxs-lookup"><span data-stu-id="49ca9-118">Versions can be downloaded for NUnit, MBUnit, and XUnit.</span></span> <span data-ttu-id="49ca9-119">También se admite el marco de pruebas unitarias de Visual Studio integrado.</span><span class="sxs-lookup"><span data-stu-id="49ca9-119">The built-in Visual Studio Unit Test framework is also supported.</span></span>

<span data-ttu-id="49ca9-120">*Nota: El marco de pruebas unitarias de Visual Studio solo está disponible con Visual Studio 2008 Professional y versiones posteriores. Si usa VS 2008 Standard Edition o Visual Web Developer 2008 Express deberá descargar e instalar las extensiones de NUnit, MBUnit o XUnit para ASP.NET MVC para que se mostrará este cuadro de diálogo. No se mostrará el cuadro de diálogo si no existen los marcos de pruebas instalados.*</span><span class="sxs-lookup"><span data-stu-id="49ca9-120">*Note: The Visual Studio Unit Test Framework is only available with Visual Studio 2008 Professional and higher versions. If you are using VS 2008 Standard Edition or Visual Web Developer 2008 Express you will need to download and install the NUnit, MBUnit or XUnit extensions for ASP.NET MVC in order for this dialog to be shown. The dialog will not display if there aren't any test frameworks installed.*</span></span>

<span data-ttu-id="49ca9-121">Se usará el nombre de "NerdDinner.Tests" predeterminado para el proyecto de prueba que se crea y use la opción de framework "Visual Studio de prueba unitaria".</span><span class="sxs-lookup"><span data-stu-id="49ca9-121">We'll use the default "NerdDinner.Tests" name for the test project we create, and use the "Visual Studio Unit Test" framework option.</span></span> <span data-ttu-id="49ca9-122">Cuando hacemos clic en el botón "Aceptar" Visual Studio creará una solución para nosotros con dos proyectos en ella: uno para nuestra aplicación web y otro para nuestras pruebas unitarias:</span><span class="sxs-lookup"><span data-stu-id="49ca9-122">When we click the "ok" button Visual Studio will create a solution for us with two projects in it - one for our web application and one for our unit tests:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a><span data-ttu-id="49ca9-123">Examinar la estructura de directorios de NerdDinner</span><span class="sxs-lookup"><span data-stu-id="49ca9-123">Examining the NerdDinner directory structure</span></span>

<span data-ttu-id="49ca9-124">Cuando se crea una nueva aplicación MVC de ASP.NET con Visual Studio, agrega automáticamente un número de archivos y directorios al proyecto:</span><span class="sxs-lookup"><span data-stu-id="49ca9-124">When you create a new ASP.NET MVC application with Visual Studio, it automatically adds a number of files and directories to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image4.png)

<span data-ttu-id="49ca9-125">Proyectos de ASP.NET MVC predeterminada tienen seis directorios de nivel superior:</span><span class="sxs-lookup"><span data-stu-id="49ca9-125">ASP.NET MVC projects by default have six top-level directories:</span></span>

| <span data-ttu-id="49ca9-126">**Directorio**</span><span class="sxs-lookup"><span data-stu-id="49ca9-126">**Directory**</span></span> | <span data-ttu-id="49ca9-127">**Propósito**</span><span class="sxs-lookup"><span data-stu-id="49ca9-127">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="49ca9-128">**Y controladores**</span><span class="sxs-lookup"><span data-stu-id="49ca9-128">**/Controllers**</span></span> | <span data-ttu-id="49ca9-129">En la que colocar las clases de controlador que controlan las solicitudes URL</span><span class="sxs-lookup"><span data-stu-id="49ca9-129">Where you put Controller classes that handle URL requests</span></span> |
| <span data-ttu-id="49ca9-130">**/ Modelos**</span><span class="sxs-lookup"><span data-stu-id="49ca9-130">**/Models**</span></span> | <span data-ttu-id="49ca9-131">En la que colocar las clases que representan y manipulan datos</span><span class="sxs-lookup"><span data-stu-id="49ca9-131">Where you put classes that represent and manipulate data</span></span> |
| <span data-ttu-id="49ca9-132">**O vistas**</span><span class="sxs-lookup"><span data-stu-id="49ca9-132">**/Views**</span></span> | <span data-ttu-id="49ca9-133">En la que colocar los archivos de plantilla de la interfaz de usuario que son responsables de la salida de representación</span><span class="sxs-lookup"><span data-stu-id="49ca9-133">Where you put UI template files that are responsible for rendering output</span></span> |
| <span data-ttu-id="49ca9-134">**/ Scripts**</span><span class="sxs-lookup"><span data-stu-id="49ca9-134">**/Scripts**</span></span> | <span data-ttu-id="49ca9-135">En la que colocar los archivos de biblioteca de JavaScript y los scripts (.js)</span><span class="sxs-lookup"><span data-stu-id="49ca9-135">Where you put JavaScript library files and scripts (.js)</span></span> |
| <span data-ttu-id="49ca9-136">**/ Content**</span><span class="sxs-lookup"><span data-stu-id="49ca9-136">**/Content**</span></span> | <span data-ttu-id="49ca9-137">Dónde colocar CSS y archivos de imagen y otro contenido que no sean-dinámicos no admiten de JavaScript</span><span class="sxs-lookup"><span data-stu-id="49ca9-137">Where you put CSS and image files, and other non-dynamic/non-JavaScript content</span></span> |
| <span data-ttu-id="49ca9-138">**/ Aplicación\_datos**</span><span class="sxs-lookup"><span data-stu-id="49ca9-138">**/App\_Data**</span></span> | <span data-ttu-id="49ca9-139">Almacenar archivos de datos que desea lectura/escritura.</span><span class="sxs-lookup"><span data-stu-id="49ca9-139">Where you store data files you want to read/write.</span></span> |

<span data-ttu-id="49ca9-140">ASP.NET MVC no requiere esta estructura.</span><span class="sxs-lookup"><span data-stu-id="49ca9-140">ASP.NET MVC does not require this structure.</span></span> <span data-ttu-id="49ca9-141">De hecho, los desarrolladores que trabajan en aplicaciones de gran tamaño se normalmente realizan particiones de la aplicación de seguridad en varios proyectos para que sea más fácil de administrar (por ejemplo: clases de modelo de datos entran a menudo en un proyecto de biblioteca de clases independiente de la aplicación web).</span><span class="sxs-lookup"><span data-stu-id="49ca9-141">In fact, developers working on large applications will typically partition the application up across multiple projects to make it more manageable (for example: data model classes often go in a separate class library project from the web application).</span></span> <span data-ttu-id="49ca9-142">Sin embargo, la estructura del proyecto de forma predeterminada, proporcionar una convención de directorio predeterminada agradable que podemos usar para mantener limpio nuestros problemas de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="49ca9-142">The default project structure, however, does provide a nice default directory convention that we can use to keep our application concerns clean.</span></span>

<span data-ttu-id="49ca9-143">Cuando se expande el directorio /Controllers, encontraremos que Visual Studio agrega dos clases de controlador: HomeController y AccountController, de forma predeterminada al proyecto:</span><span class="sxs-lookup"><span data-stu-id="49ca9-143">When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image5.png)

<span data-ttu-id="49ca9-144">Cuando se expande el directorio aquí se, buscaremos tres subdirectorios: / Home, / Account y /Shared –, así como plantilla de varios archivos dentro de ellos también se agregaron al proyecto de forma predeterminada:</span><span class="sxs-lookup"><span data-stu-id="49ca9-144">When we expand the /Views directory, we'll find three sub-directories – /Home, /Account and /Shared – as well as several template files within them were also added to the project by default:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image6.png)

<span data-ttu-id="49ca9-145">Cuando se expande el/Content y directorios / scripts, se encontrará un archivo Site.css que se usa para definir el estilo de todo el código HTML en el sitio, así como las bibliotecas de JavaScript que pueden habilitar ASP.NET AJAX y jQuery se admiten dentro de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="49ca9-145">When we expand the /Content and /Scripts directories, we'll find a Site.css file that is used to style all HTML on the site, as well as JavaScript libraries that can enable ASP.NET AJAX and jQuery support within the application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image7.png)

<span data-ttu-id="49ca9-146">Cuando se expande el proyecto NerdDinner.Tests nos encontrará dos clases que contienen pruebas unitarias para nuestras clases de controlador:</span><span class="sxs-lookup"><span data-stu-id="49ca9-146">When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image8.png)

<span data-ttu-id="49ca9-147">Estos archivos de forma predeterminada, Visual Studio agregados nos proporcionan una estructura básica para una aplicación de trabajo - junto con la página de inicio, acerca de la página, las páginas de inicio de sesión y cierre de sesión/registro de cuenta y una página de error no controlado (todo conectada y funcionan de fábrica).</span><span class="sxs-lookup"><span data-stu-id="49ca9-147">These default files added by Visual Studio provide us with a basic structure for a working application - complete with home page, about page, account login/logout/registration pages, and an unhandled error page (all wired-up and working out of the box).</span></span>

### <a name="running-the-nerddinner-application"></a><span data-ttu-id="49ca9-148">Ejecutar la aplicación NerdDinner</span><span class="sxs-lookup"><span data-stu-id="49ca9-148">Running the NerdDinner Application</span></span>

<span data-ttu-id="49ca9-149">Podemos ejecutar el proyecto al seleccionar el **Debug -&gt;Iniciar depuración** o **Debug -&gt;iniciar sin depurar** elementos de menú:</span><span class="sxs-lookup"><span data-stu-id="49ca9-149">We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image9.png)

<span data-ttu-id="49ca9-150">Esto inicia la ASP.NET Web-servidor integrado que se incluye con Visual Studio y ejecutar la aplicación:</span><span class="sxs-lookup"><span data-stu-id="49ca9-150">This will launch the built-in ASP.NET Web-server that comes with Visual Studio, and run our application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image10.png)

<span data-ttu-id="49ca9-151">A continuación aparece la página principal de nuestro nuevo proyecto (dirección URL: "/") cuando se ejecuta:</span><span class="sxs-lookup"><span data-stu-id="49ca9-151">Below is the home page for our new project (URL: "/") when it runs:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image11.png)

<span data-ttu-id="49ca9-152">Al hacer clic en la pestaña "About" muestra una acerca de la página (dirección URL: "/ Home/About"):</span><span class="sxs-lookup"><span data-stu-id="49ca9-152">Clicking the "About" tab displays an about page (URL: "/Home/About"):</span></span>

![](create-a-new-aspnet-mvc-project/_static/image12.png)

<span data-ttu-id="49ca9-153">Al hacer clic en el vínculo "Iniciar sesión" en la parte superior derecha nos lleva a una página de inicio de sesión (dirección URL: "/ cuenta/inicio de sesión")</span><span class="sxs-lookup"><span data-stu-id="49ca9-153">Clicking the "Log On" link on the top-right takes us to a Login page (URL: "/Account/LogOn")</span></span>

![](create-a-new-aspnet-mvc-project/_static/image13.png)

<span data-ttu-id="49ca9-154">Si no tenemos que podemos hacer clic en el vínculo registrar una cuenta de inicio de sesión (dirección URL: "/ cuenta/Register") para crear uno:</span><span class="sxs-lookup"><span data-stu-id="49ca9-154">If we don't have a login account we can click the register link (URL: "/Account/Register") to create one:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image14.png)

<span data-ttu-id="49ca9-155">El código para implementar la principal anterior, sobre y el cierre de sesión / register funcionalidad se ha agregado de forma predeterminada, cuando creamos nuestro nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="49ca9-155">The code to implement the above home, about, and logout/ register functionality was added by default when we created our new project.</span></span> <span data-ttu-id="49ca9-156">Vamos a usar como punto de partida de nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="49ca9-156">We'll use it as the starting point of our application.</span></span>

### <a name="testing-the-nerddinner-application"></a><span data-ttu-id="49ca9-157">Probar la aplicación NerdDinner</span><span class="sxs-lookup"><span data-stu-id="49ca9-157">Testing the NerdDinner Application</span></span>

<span data-ttu-id="49ca9-158">Si usamos la Professional Edition o una versión posterior de Visual Studio 2008, podemos usar la unidad integrada, las pruebas de compatibilidad del IDE de Visual Studio para el proyecto de prueba:</span><span class="sxs-lookup"><span data-stu-id="49ca9-158">If we are using the Professional Edition or higher version of Visual Studio 2008, we can use the built-in unit testing IDE support within Visual Studio to test the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image15.png)

<span data-ttu-id="49ca9-159">Elegir una de las opciones anteriores se abra el panel "Los resultados de pruebas" en el IDE y nos proporcione con estado correcto o incorrecto en las pruebas 27 unitarias incluido en nuestro nuevo proyecto que cubren la funcionalidad integrada:</span><span class="sxs-lookup"><span data-stu-id="49ca9-159">Choosing one of the above options will open the "Test Results" pane within the IDE and provide us with pass/fail status on the 27 unit tests included in our new project that cover the built-in functionality:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image16.png)

<span data-ttu-id="49ca9-160">Más adelante en este tutorial crearemos un poco más acerca de las pruebas automatizadas y agregaremos pruebas unitarias adicionales que cubren la funcionalidad de la aplicación que se implemente.</span><span class="sxs-lookup"><span data-stu-id="49ca9-160">Later in this tutorial we'll talk more about automated testing and add additional unit tests that cover the application functionality we implement.</span></span>

### <a name="next-step"></a><span data-ttu-id="49ca9-161">Paso siguiente</span><span class="sxs-lookup"><span data-stu-id="49ca9-161">Next Step</span></span>

<span data-ttu-id="49ca9-162">Ahora tenemos una estructura de una aplicación básica en su lugar.</span><span class="sxs-lookup"><span data-stu-id="49ca9-162">We've now got a basic application structure in place.</span></span> <span data-ttu-id="49ca9-163">Ahora vamos a [crear una base de datos para almacenar los datos de aplicación](create-a-database.md).</span><span class="sxs-lookup"><span data-stu-id="49ca9-163">Let's now [create a database to store our application data](create-a-database.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="49ca9-164">[Anterior](introducing-the-nerddinner-tutorial.md)
> [Siguiente](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="49ca9-164">[Previous](introducing-the-nerddinner-tutorial.md)
[Next](create-a-database.md)</span></span>
