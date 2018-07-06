---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Parte 1: Archivo -> Nuevo proyecto | Microsoft Docs'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks. Parte 1 abarca información general y archivo/nuevo proyecto.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: 1c8ba561745c9c02b5513e326b4739ec97338393
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825692"
---
<a name="part-1-file--new-project"></a><span data-ttu-id="326d4-104">Parte 1: Archivo -> Nuevo proyecto</span><span class="sxs-lookup"><span data-stu-id="326d4-104">Part 1: File-> New Project</span></span>
====================
<span data-ttu-id="326d4-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="326d4-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="326d4-106">Tailspin Spyworks demuestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET.</span><span class="sxs-lookup"><span data-stu-id="326d4-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="326d4-107">Resalta cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluida la compra, la desprotección y la administración.</span><span class="sxs-lookup"><span data-stu-id="326d4-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="326d4-108">Esta serie de tutoriales detalla todos los pasos que se tarda en compilar la aplicación de ejemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="326d4-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="326d4-109">Parte 1 abarca información general y archivo/nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="326d4-109">Part 1 covers Overview and File/New Project.</span></span>


## <a id="_Toc260221666"></a>  <span data-ttu-id="326d4-110">Información general</span><span class="sxs-lookup"><span data-stu-id="326d4-110">Overview</span></span>

<span data-ttu-id="326d4-111">Este tutorial es una introducción a ASP.NET WebForms.</span><span class="sxs-lookup"><span data-stu-id="326d4-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="326d4-112">Se comenzará lentamente, por lo que la experiencia de desarrollo de web de nivel principiante es correcto.</span><span class="sxs-lookup"><span data-stu-id="326d4-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="326d4-113">La aplicación que se va a compilar es una tienda en línea sencilla.</span><span class="sxs-lookup"><span data-stu-id="326d4-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)


<span data-ttu-id="326d4-114">Los visitantes pueden examinar los productos por categoría:</span><span class="sxs-lookup"><span data-stu-id="326d4-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="326d4-115">Pueden ver un producto único y agregarlo a su carro de compra:</span><span class="sxs-lookup"><span data-stu-id="326d4-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="326d4-116">Puede revisar su carro de compra, quitar todos los elementos que ya no desean:</span><span class="sxs-lookup"><span data-stu-id="326d4-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="326d4-117">Continuar con la desprotección le pedirá que</span><span class="sxs-lookup"><span data-stu-id="326d4-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="326d4-118">Después de la ordenación, verán una pantalla de confirmación simple:</span><span class="sxs-lookup"><span data-stu-id="326d4-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)


<span data-ttu-id="326d4-119">Comenzaremos por crear un nuevo proyecto de formularios Web Forms de ASP.NET en Visual Studio 2010 y vamos a agregar gradualmente funciones para crear una aplicación operativa completa.</span><span class="sxs-lookup"><span data-stu-id="326d4-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="326d4-120">Por el camino, hablaremos sobre acceso a la base de datos, vistas de lista y la cuadrícula, las páginas de actualización de datos, validación de datos, mediante páginas maestras para diseño de página coherente, AJAX, validación, pertenencia a usuario y mucho más.</span><span class="sxs-lookup"><span data-stu-id="326d4-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="326d4-121">Puede seguir paso a paso, o bien puede descargar la aplicación completa de [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="326d4-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="326d4-122">Puede usar Visual Studio 2010 o el gratuita Visual Web Developer 2010 desde [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="326d4-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="326d4-123">Para compilar la aplicación, puede usar SQL Server o el libre SQL Server Express para hospedar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="326d4-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a>  <span data-ttu-id="326d4-124">Archivo / nuevo proyecto</span><span class="sxs-lookup"><span data-stu-id="326d4-124">File / New Project</span></span>

<span data-ttu-id="326d4-125">Comenzaremos seleccionando el nuevo proyecto en el menú archivo en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="326d4-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="326d4-126">Se abrirá el cuadro de diálogo nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="326d4-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="326d4-127">Seleccionaremos Visual C# / Web plantillas de grupo a la izquierda y, a continuación, elija la plantilla "Aplicación Web de ASP.NET" en la columna central.</span><span class="sxs-lookup"><span data-stu-id="326d4-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="326d4-128">Nombre del proyecto TailspinSpyworks y presione el botón Aceptar.</span><span class="sxs-lookup"><span data-stu-id="326d4-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="326d4-129">Esto creará el proyecto.</span><span class="sxs-lookup"><span data-stu-id="326d4-129">This will create our project.</span></span> <span data-ttu-id="326d4-130">Echemos un vistazo a las carpetas que se incluyen en nuestra aplicación en el Explorador de soluciones en el lado derecho.</span><span class="sxs-lookup"><span data-stu-id="326d4-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="326d4-131">La solución vacía no está completamente vacía, agrega una estructura de carpetas básica:</span><span class="sxs-lookup"><span data-stu-id="326d4-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="326d4-132">Tenga en cuenta las convenciones que se implementa mediante la plantilla de proyecto predeterminada ASP.NET 4.</span><span class="sxs-lookup"><span data-stu-id="326d4-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="326d4-133">La carpeta "Account", implementa una interfaz de usuario básica para ASP. Subsistema de pertenencia de la red.</span><span class="sxs-lookup"><span data-stu-id="326d4-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="326d4-134">La carpeta "Scripts" sirve como repositorio para los archivos de JavaScript del lado cliente y los archivos de .js de jQuery core se ponen a disposición de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="326d4-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="326d4-135">La carpeta "Estilos" se usa para organizar nuestros gráficos del sitio web (hojas de estilos CSS)</span><span class="sxs-lookup"><span data-stu-id="326d4-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="326d4-136">Cuando se presiona F5 para ejecutar la aplicación y presentar la página default.aspx se ve lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="326d4-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="326d4-137">Será nuestro primer mejora de aplicaciones reemplazar el archivo Style.css desde la plantilla predeterminada de formularios Web Forms con las clases CSS y archivos de imagen asociado que se representarán el asthetics visual que queramos para nuestra aplicación Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="326d4-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="326d4-138">Una vez hecho esto, nuestra página default.aspx representa similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="326d4-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="326d4-139">Tenga en cuenta los vínculos de imagen en la parte superior derecha de la página y los elementos de menú que se han agregado a la página maestra.</span><span class="sxs-lookup"><span data-stu-id="326d4-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="326d4-140">Solo los vínculos "Iniciar sesión" y "Cuenta" apuntan a las páginas que existen (generadas por la plantilla predeterminada) y el resto de las páginas que se implementará a medida que construimos nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="326d4-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="326d4-141">También vamos a cambiar la ubicación de la página maestra en el directorio de estilos.</span><span class="sxs-lookup"><span data-stu-id="326d4-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="326d4-142">Aunque esto es sólo una preferencia puede hacer las cosas un poco más fácil si decidimos que nuestra aplicación "skinable" en el futuro.</span><span class="sxs-lookup"><span data-stu-id="326d4-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="326d4-143">Una vez hecho esto, deberá cambiar la página maestra referencias en todos los archivos .aspx generan por el valor predeterminado las páginas de formularios Web Forms de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="326d4-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="326d4-144">Siguiente</span><span class="sxs-lookup"><span data-stu-id="326d4-144">Next</span></span>](tailspin-spyworks-part-2.md)
