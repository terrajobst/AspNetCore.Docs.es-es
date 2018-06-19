---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Parte 1: Información general y archivo -> Nuevo proyecto | Documentos de Microsoft'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 1 cubre información general y archivo -> Nuevo proyecto.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 2082927d18c95563893da199d60347fa15952446
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868976"
---
<a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="6aedb-104">Parte 1: Información general y archivo -> Nuevo proyecto</span><span class="sxs-lookup"><span data-stu-id="6aedb-104">Part 1: Overview and File->New Project</span></span>
====================
<span data-ttu-id="6aedb-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="6aedb-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="6aedb-106">La tienda de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="6aedb-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="6aedb-107">La tienda de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa administración básica del sitio, en el inicio de sesión de usuario y la funcionalidad del carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="6aedb-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="6aedb-108">Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6aedb-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="6aedb-109">Parte 1 cubre información general y archivo -&gt;nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="6aedb-109">Part 1 covers Overview and File-&gt;New Project.</span></span>


## <a name="overview"></a><span data-ttu-id="6aedb-110">Información general</span><span class="sxs-lookup"><span data-stu-id="6aedb-110">Overview</span></span>

<span data-ttu-id="6aedb-111">La tienda de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Web Developer para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="6aedb-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="6aedb-112">Se comenzará lentamente, por lo que la experiencia de desarrollo de nivel web principiantes es correcto.</span><span class="sxs-lookup"><span data-stu-id="6aedb-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="6aedb-113">La aplicación que desarrollaremos es una tienda de música simple.</span><span class="sxs-lookup"><span data-stu-id="6aedb-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="6aedb-114">Hay tres partes principales para la aplicación: la compra, la desprotección y administración.</span><span class="sxs-lookup"><span data-stu-id="6aedb-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="6aedb-115">Los visitantes pueden examinar álbumes por género:</span><span class="sxs-lookup"><span data-stu-id="6aedb-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="6aedb-116">Pueden ver un sólo álbum y agregarlo a su carro:</span><span class="sxs-lookup"><span data-stu-id="6aedb-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="6aedb-117">Puede revisar su carro de la eliminación de elementos que ya no desean:</span><span class="sxs-lookup"><span data-stu-id="6aedb-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="6aedb-118">Continuar con la desprotección les pedirá que inicie sesión o regístrese para una cuenta de usuario.</span><span class="sxs-lookup"><span data-stu-id="6aedb-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="6aedb-119">Después de crear una cuenta, puede completar el pedido rellenando la información de pago y envío.</span><span class="sxs-lookup"><span data-stu-id="6aedb-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="6aedb-120">Para no complicar las cosas, estamos ejecutando una promoción increíble: todo es gratis Cuando especifique el código de promoción "Gratuito".</span><span class="sxs-lookup"><span data-stu-id="6aedb-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="6aedb-121">Después de ordenar, verán una pantalla de confirmación simple:</span><span class="sxs-lookup"><span data-stu-id="6aedb-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="6aedb-122">Además de las páginas de faceing de cliente, se deberá crear una sección de administrador que muestra una lista de álbumes desde la que los administradores pueden crear, editar y eliminar álbumes:</span><span class="sxs-lookup"><span data-stu-id="6aedb-122">In addition to customer-faceing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="6aedb-123">1. Archivo -&gt; nuevo proyecto</span><span class="sxs-lookup"><span data-stu-id="6aedb-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="6aedb-124">Instalar el software</span><span class="sxs-lookup"><span data-stu-id="6aedb-124">Installing the software</span></span>

<span data-ttu-id="6aedb-125">Este tutorial se iniciará mediante la creación de un nuevo proyecto de ASP.NET MVC 3 con el libre Visual Web Developer 2010 Express (que es gratuito) y, a continuación, vamos a agregar características para crear una aplicación operativa completa incrementalmente.</span><span class="sxs-lookup"><span data-stu-id="6aedb-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="6aedb-126">Al hacerlo, hablaremos sobre acceso a la base de datos, los escenarios de envío de formulario, validación de datos, mediante páginas maestras para el diseño de página coherente, con AJAX para actualizaciones de la página y validación, el inicio de sesión de usuario y mucho más.</span><span class="sxs-lookup"><span data-stu-id="6aedb-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="6aedb-127">Puede seguir a lo largo de paso a paso, o bien puede descargar la aplicación completa de [tienda de música de MVC](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="6aedb-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="6aedb-128">Puede usar Visual Studio 2010 SP1 o Visual Web Developer 2010 Express SP1 (es decir, una versión gratuita de Visual Studio 2010) para compilar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6aedb-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="6aedb-129">Usaremos SQL Server Compact (también disponible) para hospedar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6aedb-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="6aedb-130">Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación.</span><span class="sxs-lookup"><span data-stu-id="6aedb-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>


- <span data-ttu-id="6aedb-131">[Requisitos previos de visual Studio Web Developer Express SP1]</span><span class="sxs-lookup"><span data-stu-id="6aedb-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="6aedb-132">[ASP.NET MVC 3 Tools Update]</span><span class="sxs-lookup"><span data-stu-id="6aedb-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="6aedb-133">[SQL Server Compact 4.0] - incluida la compatibilidad en tiempo de ejecución y herramientas</span><span class="sxs-lookup"><span data-stu-id="6aedb-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>


### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="6aedb-134">Crear un nuevo proyecto de ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="6aedb-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="6aedb-135">Comenzaremos seleccionando "Nuevo proyecto" en el menú archivo en Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="6aedb-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="6aedb-136">Se abrirá el cuadro de diálogo nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="6aedb-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="6aedb-137">Seleccione Visual C# -&gt; plantillas Web grupo a la izquierda, a continuación, elija la plantilla de "Aplicación Web de ASP.NET MVC 3" en la columna central.</span><span class="sxs-lookup"><span data-stu-id="6aedb-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="6aedb-138">Denomine el proyecto MvcMusicStore y presione el botón Aceptar.</span><span class="sxs-lookup"><span data-stu-id="6aedb-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="6aedb-139">Se mostrará un cuadro de diálogo secundario que nos permite realizar algunas MVC una configuración específica para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="6aedb-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="6aedb-140">Seleccione lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6aedb-140">Select the following:</span></span>

<span data-ttu-id="6aedb-141">Plantilla de proyecto: seleccione vacío</span><span class="sxs-lookup"><span data-stu-id="6aedb-141">Project Template - select Empty</span></span>

<span data-ttu-id="6aedb-142">Ver motor: seleccione Razor</span><span class="sxs-lookup"><span data-stu-id="6aedb-142">View Engine - select Razor</span></span>

<span data-ttu-id="6aedb-143">Usar marcado semántico de HTML5: activada</span><span class="sxs-lookup"><span data-stu-id="6aedb-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="6aedb-144">Compruebe que la configuración forman tal y como se muestra a continuación, presione el botón Aceptar.</span><span class="sxs-lookup"><span data-stu-id="6aedb-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="6aedb-145">Esto creará el proyecto.</span><span class="sxs-lookup"><span data-stu-id="6aedb-145">This will create our project.</span></span> <span data-ttu-id="6aedb-146">¡Eche un vistazo a las carpetas que se han agregado a la aplicación en el Explorador de soluciones en el lado derecho.</span><span class="sxs-lookup"><span data-stu-id="6aedb-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="6aedb-147">La plantilla vacía MVC 3 no está completamente vacía, agrega una estructura de carpetas básico:</span><span class="sxs-lookup"><span data-stu-id="6aedb-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="6aedb-148">ASP.NET MVC hace uso de algunas convenciones de nomenclatura básicas para los nombres de carpeta:</span><span class="sxs-lookup"><span data-stu-id="6aedb-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="6aedb-149">**Folder**</span><span class="sxs-lookup"><span data-stu-id="6aedb-149">**Folder**</span></span> | <span data-ttu-id="6aedb-150">**Purpose**</span><span class="sxs-lookup"><span data-stu-id="6aedb-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="6aedb-151">**/ Controladores**</span><span class="sxs-lookup"><span data-stu-id="6aedb-151">**/Controllers**</span></span> | <span data-ttu-id="6aedb-152">Controladores de responden a los datos proporcionados por el explorador, decidir qué hacer con él y devolver la respuesta al usuario.</span><span class="sxs-lookup"><span data-stu-id="6aedb-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="6aedb-153">**/ Vistas**</span><span class="sxs-lookup"><span data-stu-id="6aedb-153">**/Views**</span></span> | <span data-ttu-id="6aedb-154">Vistas mantenga nuestras plantillas de interfaz de usuario</span><span class="sxs-lookup"><span data-stu-id="6aedb-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="6aedb-155">**/Models**</span><span class="sxs-lookup"><span data-stu-id="6aedb-155">**/Models**</span></span> | <span data-ttu-id="6aedb-156">Modelos de mantengan y manipulan datos</span><span class="sxs-lookup"><span data-stu-id="6aedb-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="6aedb-157">**/Content**</span><span class="sxs-lookup"><span data-stu-id="6aedb-157">**/Content**</span></span> | <span data-ttu-id="6aedb-158">Esta carpeta contiene nuestras imágenes, CSS y cualquier otro contenido estático</span><span class="sxs-lookup"><span data-stu-id="6aedb-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="6aedb-159">**/ Secuencias de comandos**</span><span class="sxs-lookup"><span data-stu-id="6aedb-159">**/Scripts**</span></span> | <span data-ttu-id="6aedb-160">Esta carpeta contiene los archivos de JavaScript</span><span class="sxs-lookup"><span data-stu-id="6aedb-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="6aedb-161">Estas carpetas se incluyen incluso en una aplicación MVC de ASP.NET vacía porque el marco de MVC de ASP.NET de forma predeterminada, usa un enfoque de "Convención sobre configuración" y hace algunas suposiciones predeterminado basados en convenciones de nomenclatura de la carpeta.</span><span class="sxs-lookup"><span data-stu-id="6aedb-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="6aedb-162">Por ejemplo, controladores de buscarán vistas en la carpeta Views de forma predeterminada sin tener que especificarlo explícitamente en el código.</span><span class="sxs-lookup"><span data-stu-id="6aedb-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="6aedb-163">Quede con las convenciones predeterminadas reduce la cantidad de código que necesita para escribir, y también puede hacer sea más fácil para otros programadores a comprender el proyecto.</span><span class="sxs-lookup"><span data-stu-id="6aedb-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="6aedb-164">Explicaremos estas convenciones más mientras compilamos nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="6aedb-164">We'll explain these conventions more as we build our application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="6aedb-165">Siguiente</span><span class="sxs-lookup"><span data-stu-id="6aedb-165">Next</span></span>](mvc-music-store-part-2.md)
