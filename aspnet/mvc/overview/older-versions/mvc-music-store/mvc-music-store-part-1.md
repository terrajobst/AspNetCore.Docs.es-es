---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Parte 1: Información general y archivo -> Nuevo proyecto | Microsoft Docs'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 1 abarca información general y archivo -> Nuevo proyecto.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: d84a525221e40b79853be55069367134d17fb5ec
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829768"
---
<a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="6aefe-104">Parte 1: Información general y archivo -> Nuevo proyecto</span><span class="sxs-lookup"><span data-stu-id="6aefe-104">Part 1: Overview and File->New Project</span></span>
====================
<span data-ttu-id="6aefe-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="6aefe-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="6aefe-106">El Store de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="6aefe-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="6aefe-107">El Store de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa la administración básica del sitio, inicio de sesión de usuario y funcionalidad del carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="6aefe-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="6aefe-108">Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="6aefe-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="6aefe-109">Parte 1 abarca información general y archivo -&gt;nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="6aefe-109">Part 1 covers Overview and File-&gt;New Project.</span></span>


## <a name="overview"></a><span data-ttu-id="6aefe-110">Información general</span><span class="sxs-lookup"><span data-stu-id="6aefe-110">Overview</span></span>

<span data-ttu-id="6aefe-111">El Store de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Web Developer para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="6aefe-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="6aefe-112">Se comenzará lentamente, por lo que la experiencia de desarrollo de web de nivel principiante es correcto.</span><span class="sxs-lookup"><span data-stu-id="6aefe-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="6aefe-113">La aplicación que se va a compilar es una tienda de música simple.</span><span class="sxs-lookup"><span data-stu-id="6aefe-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="6aefe-114">Hay tres partes principales a la aplicación: la compra, la desprotección y la administración.</span><span class="sxs-lookup"><span data-stu-id="6aefe-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="6aefe-115">Los visitantes pueden examinar álbumes por género:</span><span class="sxs-lookup"><span data-stu-id="6aefe-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="6aefe-116">Pueden ver un álbum y agregarlo a su carro de compra:</span><span class="sxs-lookup"><span data-stu-id="6aefe-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="6aefe-117">Puede revisar su carro de compra, quitar todos los elementos que ya no desean:</span><span class="sxs-lookup"><span data-stu-id="6aefe-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="6aefe-118">Continuar con la desprotección le pedirá que inicie sesión o regístrese para una cuenta de usuario.</span><span class="sxs-lookup"><span data-stu-id="6aefe-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="6aefe-119">Después de crear una cuenta, puede completar el pedido al rellenar la información de pago y envío.</span><span class="sxs-lookup"><span data-stu-id="6aefe-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="6aefe-120">Para simplificar las cosas, estamos ejecutando una promoción increíble: todo está disponible si se especifica el código de promoción "Gratis".</span><span class="sxs-lookup"><span data-stu-id="6aefe-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="6aefe-121">Después de la ordenación, verán una pantalla de confirmación simple:</span><span class="sxs-lookup"><span data-stu-id="6aefe-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="6aefe-122">Además de las páginas del cliente faceing, también generar una sección de administrador que se muestra una lista de álbumes desde el que los administradores pueden crear, editar y eliminar álbumes:</span><span class="sxs-lookup"><span data-stu-id="6aefe-122">In addition to customer-faceing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="6aefe-123">1. Archivo -&gt; nuevo proyecto</span><span class="sxs-lookup"><span data-stu-id="6aefe-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="6aefe-124">Instalación del software</span><span class="sxs-lookup"><span data-stu-id="6aefe-124">Installing the software</span></span>

<span data-ttu-id="6aefe-125">En este tutorial comenzará creando un nuevo proyecto de ASP.NET MVC 3 con la gratuita Visual Web Developer 2010 Express (que es gratuito) y, a continuación, vamos a agregar gradualmente funciones para crear una aplicación operativa completa.</span><span class="sxs-lookup"><span data-stu-id="6aefe-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="6aefe-126">En el camino, hablaremos sobre acceso a la base de datos, escenarios de registro de formato, validación de datos, mediante páginas maestras para el diseño de página coherente, con AJAX para las actualizaciones de página y validación, el inicio de sesión de usuario y mucho más.</span><span class="sxs-lookup"><span data-stu-id="6aefe-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="6aefe-127">Puede seguir paso a paso, o bien puede descargar la aplicación completa de [MVC-música-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="6aefe-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="6aefe-128">Puede usar Visual Studio 2010 SP1 o Visual Web Developer 2010 Express SP1 (una versión gratuita de Visual Studio 2010) para compilar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6aefe-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="6aefe-129">Vamos a usar SQL Server Compact (también disponible) para hospedar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6aefe-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="6aefe-130">Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación.</span><span class="sxs-lookup"><span data-stu-id="6aefe-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>


- <span data-ttu-id="6aefe-131">[Requisitos previos de visual Studio Web Developer Express SP1]</span><span class="sxs-lookup"><span data-stu-id="6aefe-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="6aefe-132">[ASP.NET MVC 3 Tools Update]</span><span class="sxs-lookup"><span data-stu-id="6aefe-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="6aefe-133">[SQL Server Compact 4.0] - incluida la compatibilidad en tiempo de ejecución y herramientas</span><span class="sxs-lookup"><span data-stu-id="6aefe-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>


### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="6aefe-134">Crear un nuevo proyecto de ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="6aefe-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="6aefe-135">Comenzaremos seleccionando "Nuevo proyecto" en el menú archivo en Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="6aefe-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="6aefe-136">Se abrirá el cuadro de diálogo nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="6aefe-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="6aefe-137">Seleccionaremos Visual C# -&gt; Web plantillas de grupo a la izquierda y, después, elija la plantilla "Aplicación Web de ASP.NET MVC 3" en la columna central.</span><span class="sxs-lookup"><span data-stu-id="6aefe-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="6aefe-138">Nombre del proyecto MvcMusicStore y presione el botón Aceptar.</span><span class="sxs-lookup"><span data-stu-id="6aefe-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="6aefe-139">Se mostrará un cuadro de diálogo secundario que nos permite realizar algunas configuraciones específicas de MVC para nuestro proyecto.</span><span class="sxs-lookup"><span data-stu-id="6aefe-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="6aefe-140">Seleccione lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6aefe-140">Select the following:</span></span>

<span data-ttu-id="6aefe-141">Plantilla de proyecto: seleccione vacío</span><span class="sxs-lookup"><span data-stu-id="6aefe-141">Project Template - select Empty</span></span>

<span data-ttu-id="6aefe-142">Ver motor: selección de Razor</span><span class="sxs-lookup"><span data-stu-id="6aefe-142">View Engine - select Razor</span></span>

<span data-ttu-id="6aefe-143">Usar marcado semántico HTML5: activada</span><span class="sxs-lookup"><span data-stu-id="6aefe-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="6aefe-144">Compruebe que la configuración tal como se muestra a continuación y presione el botón Aceptar.</span><span class="sxs-lookup"><span data-stu-id="6aefe-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="6aefe-145">Esto creará el proyecto.</span><span class="sxs-lookup"><span data-stu-id="6aefe-145">This will create our project.</span></span> <span data-ttu-id="6aefe-146">Echemos un vistazo a las carpetas que se han agregado a la aplicación en el Explorador de soluciones en el lado derecho.</span><span class="sxs-lookup"><span data-stu-id="6aefe-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="6aefe-147">La plantilla vacía MVC 3 no está completamente vacía, agrega una estructura de carpetas básica:</span><span class="sxs-lookup"><span data-stu-id="6aefe-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="6aefe-148">ASP.NET MVC hace uso de algunas convenciones de nomenclatura básicas para los nombres de carpeta:</span><span class="sxs-lookup"><span data-stu-id="6aefe-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="6aefe-149">**Carpeta**</span><span class="sxs-lookup"><span data-stu-id="6aefe-149">**Folder**</span></span> | <span data-ttu-id="6aefe-150">**Propósito**</span><span class="sxs-lookup"><span data-stu-id="6aefe-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="6aefe-151">**Y controladores**</span><span class="sxs-lookup"><span data-stu-id="6aefe-151">**/Controllers**</span></span> | <span data-ttu-id="6aefe-152">Los controladores de responden a la entrada desde el explorador, decidir qué hacer con él y devolver la respuesta al usuario.</span><span class="sxs-lookup"><span data-stu-id="6aefe-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="6aefe-153">**O vistas**</span><span class="sxs-lookup"><span data-stu-id="6aefe-153">**/Views**</span></span> | <span data-ttu-id="6aefe-154">Las vistas contienen nuestras plantillas de interfaz de usuario</span><span class="sxs-lookup"><span data-stu-id="6aefe-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="6aefe-155">**/ Modelos**</span><span class="sxs-lookup"><span data-stu-id="6aefe-155">**/Models**</span></span> | <span data-ttu-id="6aefe-156">Modelos contienen y manipulan datos</span><span class="sxs-lookup"><span data-stu-id="6aefe-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="6aefe-157">**/ Content**</span><span class="sxs-lookup"><span data-stu-id="6aefe-157">**/Content**</span></span> | <span data-ttu-id="6aefe-158">Esta carpeta contiene nuestras imágenes, CSS y cualquier otro contenido estático</span><span class="sxs-lookup"><span data-stu-id="6aefe-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="6aefe-159">**/ Scripts**</span><span class="sxs-lookup"><span data-stu-id="6aefe-159">**/Scripts**</span></span> | <span data-ttu-id="6aefe-160">Esta carpeta contiene los archivos de JavaScript</span><span class="sxs-lookup"><span data-stu-id="6aefe-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="6aefe-161">Estas carpetas se incluyen incluso en una aplicación MVC de ASP.NET vacía porque el marco de ASP.NET MVC predeterminada usa un enfoque de "Convención sobre configuración" y hace algunas suposiciones predeterminadas según las convenciones de nomenclatura de carpeta.</span><span class="sxs-lookup"><span data-stu-id="6aefe-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="6aefe-162">Por ejemplo, los controladores de buscarán las vistas en la carpeta Views predeterminada sin tener que especificarlo explícitamente en el código.</span><span class="sxs-lookup"><span data-stu-id="6aefe-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="6aefe-163">Ceñirse a las convenciones predeterminadas reduce la cantidad de código que necesita para escribir, y puede también facilitan para otros desarrolladores a comprender el proyecto.</span><span class="sxs-lookup"><span data-stu-id="6aefe-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="6aefe-164">Explicaremos estas convenciones más a medida que construimos nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="6aefe-164">We'll explain these conventions more as we build our application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="6aefe-165">Siguiente</span><span class="sxs-lookup"><span data-stu-id="6aefe-165">Next</span></span>](mvc-music-store-part-2.md)
