---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Parte 1: Archivo -> Nuevo proyecto | Documentos de Microsoft'
author: JoeStagner
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks. Parte 1 cubre información general y archivo/nuevo proyecto.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: a1b9681516e626b6a0eec420b168a74e05d88afb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892470"
---
<a name="part-1-file--new-project"></a><span data-ttu-id="7f878-104">Parte 1: Archivo -> Nuevo proyecto</span><span class="sxs-lookup"><span data-stu-id="7f878-104">Part 1: File-> New Project</span></span>
====================
<span data-ttu-id="7f878-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="7f878-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="7f878-106">Tailspin Spyworks muestra cómo extraordinariamente simple es crear aplicaciones eficaces y escalables para la plataforma. NET.</span><span class="sxs-lookup"><span data-stu-id="7f878-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="7f878-107">Muestra cómo usar las características nuevas en ASP.NET 4 para crear una tienda en línea, incluidos los de la compra, la desprotección y la administración.</span><span class="sxs-lookup"><span data-stu-id="7f878-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="7f878-108">Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="7f878-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="7f878-109">Parte 1 cubre información general y archivo/nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="7f878-109">Part 1 covers Overview and File/New Project.</span></span>


## <a id="_Toc260221666"></a>  <span data-ttu-id="7f878-110">Información general</span><span class="sxs-lookup"><span data-stu-id="7f878-110">Overview</span></span>

<span data-ttu-id="7f878-111">Este tutorial es una introducción a los formularios Web Forms de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7f878-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="7f878-112">Se comenzará lentamente, por lo que la experiencia de desarrollo de nivel web principiantes es correcto.</span><span class="sxs-lookup"><span data-stu-id="7f878-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="7f878-113">La aplicación que desarrollaremos es una tienda en línea simple.</span><span class="sxs-lookup"><span data-stu-id="7f878-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)


<span data-ttu-id="7f878-114">Los visitantes pueden examinar los productos por categoría:</span><span class="sxs-lookup"><span data-stu-id="7f878-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="7f878-115">Pueden ver un único producto y agregarlo a su carro:</span><span class="sxs-lookup"><span data-stu-id="7f878-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="7f878-116">Puede revisar su carro de la eliminación de elementos que ya no desean:</span><span class="sxs-lookup"><span data-stu-id="7f878-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="7f878-117">Continuar con la desprotección les pedirá que</span><span class="sxs-lookup"><span data-stu-id="7f878-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="7f878-118">Después de ordenar, verán una pantalla de confirmación simple:</span><span class="sxs-lookup"><span data-stu-id="7f878-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)


<span data-ttu-id="7f878-119">Comenzaremos por crear un nuevo proyecto de formularios Web Forms de ASP.NET en Visual Studio 2010 e incrementalmente vamos a agregar características para crear una aplicación operativa completa.</span><span class="sxs-lookup"><span data-stu-id="7f878-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="7f878-120">Al hacerlo, hablaremos sobre acceso de base de datos, vistas de lista y la cuadrícula, páginas de actualización de datos, validación de datos, mediante páginas maestras para diseño de página coherente, AJAX, validación, pertenencia a usuarios y mucho más.</span><span class="sxs-lookup"><span data-stu-id="7f878-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="7f878-121">Puede seguir a lo largo de paso a paso, o bien puede descargar la aplicación completa de [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="7f878-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="7f878-122">Puede usar Visual Studio 2010 o el libre Visual Web Developer 2010 desde [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="7f878-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="7f878-123">Para compilar la aplicación, puede usar SQL Server o la libre SQL Server Express al host de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7f878-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a>  <span data-ttu-id="7f878-124">Archivo / nuevo proyecto</span><span class="sxs-lookup"><span data-stu-id="7f878-124">File / New Project</span></span>

<span data-ttu-id="7f878-125">Comenzaremos seleccionando el nuevo proyecto en el menú archivo en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7f878-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="7f878-126">Se abrirá el cuadro de diálogo nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="7f878-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="7f878-127">Seleccione Visual C# / plantillas Web grupo a la izquierda y, a continuación, elija la plantilla de "Aplicación Web de ASP.NET" en la columna central.</span><span class="sxs-lookup"><span data-stu-id="7f878-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="7f878-128">Denomine el proyecto TailspinSpyworks y presione el botón Aceptar.</span><span class="sxs-lookup"><span data-stu-id="7f878-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="7f878-129">Esto creará el proyecto.</span><span class="sxs-lookup"><span data-stu-id="7f878-129">This will create our project.</span></span> <span data-ttu-id="7f878-130">¡Eche un vistazo a las carpetas que se incluyen en nuestra aplicación en el Explorador de soluciones en el lado derecho.</span><span class="sxs-lookup"><span data-stu-id="7f878-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="7f878-131">La solución vacía no está completamente vacía: agrega una estructura de carpetas básico:</span><span class="sxs-lookup"><span data-stu-id="7f878-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="7f878-132">Tenga en cuenta las convenciones de implementada por la plantilla de proyecto predeterminada de ASP.NET 4.</span><span class="sxs-lookup"><span data-stu-id="7f878-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="7f878-133">La carpeta "Cuenta", implementa una interfaz de usuario básica para ASP. Subsistema de pertenencia de NET.</span><span class="sxs-lookup"><span data-stu-id="7f878-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="7f878-134">La carpeta "Scripts" actúa como repositorio para los archivos de JavaScript del lado de cliente y los archivos .js de jQuery de núcleo se ponen a disposición de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="7f878-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="7f878-135">La carpeta "Styles" se usa para organizar nuestros elementos visuales del sitio web (hojas de estilos CSS)</span><span class="sxs-lookup"><span data-stu-id="7f878-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="7f878-136">Cuando se presiona F5 para ejecutar la aplicación y presentar la página default.aspx vemos lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="7f878-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="7f878-137">Nuestro primer mejora de aplicaciones estará para reemplazar el archivo Style.css de la plantilla de formularios Web Forms predeterminada con los archivos de imagen asociada que se representarán el asthetics visual que queramos para nuestra aplicación Tailspin Spyworks y clases CSS.</span><span class="sxs-lookup"><span data-stu-id="7f878-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="7f878-138">Una vez hecho esto, nuestra página default.aspx representa similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="7f878-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="7f878-139">Tenga en cuenta los vínculos de imagen en la parte superior derecha de la página y los elementos de menú que se han agregado a la página maestra.</span><span class="sxs-lookup"><span data-stu-id="7f878-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="7f878-140">Solo los vínculos "inicio de sesión" y "Cuenta" apuntan a las páginas que existen (generadas por la plantilla predeterminada) y el resto de las páginas que se va a implementar mientras compilamos nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="7f878-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="7f878-141">También vamos a cambiar la ubicación de la página maestra en el directorio de estilos.</span><span class="sxs-lookup"><span data-stu-id="7f878-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="7f878-142">Aunque esto es sólo una preferencia puede hacer cosas un poco más fácil si se decide que nuestra aplicación "skinable" en el futuro.</span><span class="sxs-lookup"><span data-stu-id="7f878-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="7f878-143">Una vez hecho esto que será necesario cambiar la página maestra referencias en todos los archivos .aspx generan por el valor predeterminado páginas de formularios Web Forms de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7f878-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7f878-144">Siguiente</span><span class="sxs-lookup"><span data-stu-id="7f878-144">Next</span></span>](tailspin-spyworks-part-2.md)
