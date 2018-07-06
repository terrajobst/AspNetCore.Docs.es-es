---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Instalar una aplicación auxiliar en ASP.NET Web Pages (Razor) sitio | Microsoft Docs
author: tfitzmac
description: En este artículo se describe cómo instalar una aplicación auxiliar en un sitio Web de ASP.NET Web Pages (Razor). Una aplicación auxiliar es un componente reutilizable que incluye código y marcado en por...
ms.author: aspnetcontent
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: db3dff9f2d70577bb0618335c0100b9899e87727
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838619"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="1f2d8-104">Instalar una aplicación auxiliar en un sitio Web de ASP.NET Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="1f2d8-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="1f2d8-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1f2d8-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1f2d8-106">En este artículo se describe cómo instalar una aplicación auxiliar en un sitio Web de ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="1f2d8-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="1f2d8-107">Un *auxiliar* es un componente reutilizable que incluye el código y marcado para realizar una tarea que puede ser complejo o una tarea tediosa.</span><span class="sxs-lookup"><span data-stu-id="1f2d8-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="1f2d8-108">Lo que aprenderá:</span><span class="sxs-lookup"><span data-stu-id="1f2d8-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="1f2d8-109">Cómo instalar una aplicación auxiliar en un sitio Web creado mediante WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="1f2d8-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="1f2d8-110">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="1f2d8-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="1f2d8-111">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="1f2d8-111">WebMatrix 3</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="1f2d8-112">Información general de las aplicaciones auxiliares</span><span class="sxs-lookup"><span data-stu-id="1f2d8-112">Overview of Helpers</span></span>

<span data-ttu-id="1f2d8-113">Algunas tareas que las personas desean hacer en las páginas web a menudo requieren una gran cantidad de código o requieren conocimientos adicionales.</span><span class="sxs-lookup"><span data-stu-id="1f2d8-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="1f2d8-114">Algunos ejemplos son mostrar un gráfico para los datos; colocar un botón "Seguir" de Twitter en una página; envío de correo electrónico desde su sitio Web; recortar o cambiar el tamaño de imágenes. uso de PayPal para su sitio.</span><span class="sxs-lookup"><span data-stu-id="1f2d8-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="1f2d8-115">Para que sea fácil de hacer este tipo de cosas, ASP.NET Web Pages le permite usar *aplicaciones auxiliares de*.</span><span class="sxs-lookup"><span data-stu-id="1f2d8-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="1f2d8-116">Las aplicaciones auxiliares son componentes que instale un sitio y que permiten realizan tareas habituales mediante solo una o dos líneas de código Razor.</span><span class="sxs-lookup"><span data-stu-id="1f2d8-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="1f2d8-117">ASP.NET Web Pages tiene algunas aplicaciones auxiliares integradas.</span><span class="sxs-lookup"><span data-stu-id="1f2d8-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="1f2d8-118">Sin embargo, muchas aplicaciones auxiliares están disponibles en paquetes (complementos) que se proporcionan con el Administrador de paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="1f2d8-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="1f2d8-119">NuGet le permite seleccionar un paquete para instalarlo y, a continuación, se encarga de todos los detalles de la instalación.</span><span class="sxs-lookup"><span data-stu-id="1f2d8-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="1f2d8-120">Instalar una aplicación auxiliar de WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="1f2d8-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="1f2d8-121">En WebMatrix 3, haga clic en el **NuGet** botón.</span><span class="sxs-lookup"><span data-stu-id="1f2d8-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![Cuadro de diálogo de la Galería de NuGet en WebMatrix](installing-helpers/_static/image1.png)
2. <span data-ttu-id="1f2d8-123">Esto inicia el Administrador de paquetes de NuGet y muestra los paquetes disponibles.</span><span class="sxs-lookup"><span data-stu-id="1f2d8-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="1f2d8-124">En el cuadro de búsqueda, escriba una palabra clave para la aplicación auxiliar que desea instalar.</span><span class="sxs-lookup"><span data-stu-id="1f2d8-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![Cuadro de diálogo de la Galería de NuGet en WebMatrix](installing-helpers/_static/image2.png)
3. <span data-ttu-id="1f2d8-126">Seleccione el paquete y, a continuación, haga clic en **instalar**.</span><span class="sxs-lookup"><span data-stu-id="1f2d8-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="1f2d8-127">Haga clic en **Sí** cuando se le pregunte si desea instalar el paquete e indicar que acepta los términos.</span><span class="sxs-lookup"><span data-stu-id="1f2d8-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

     <span data-ttu-id="1f2d8-128">Si se trata de la primera vez que se ha instalado una aplicación auxiliar, NuGet crea carpetas en su sitio Web para el código que conforma la aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="1f2d8-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
4. <span data-ttu-id="1f2d8-129">Para desinstalar una aplicación auxiliar, haga clic en el **galería** botón, haga clic en el **instalado** pestaña y seleccione el paquete que desea desinstalar.</span><span class="sxs-lookup"><span data-stu-id="1f2d8-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="1f2d8-130">Instalar la aplicación auxiliar de Twitter</span><span class="sxs-lookup"><span data-stu-id="1f2d8-130">Installing the Twitter helper</span></span>

<span data-ttu-id="1f2d8-131">La versión más reciente de la API de Twitter no es compatible con la aplicación auxiliar de Twitter que se instala a través de NuGet.</span><span class="sxs-lookup"><span data-stu-id="1f2d8-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="1f2d8-132">En su lugar, consulte el [aplicación auxiliar de Twitter con WebMatrix](twitter-helper.md) tema para obtener información acerca de cómo configurar la aplicación auxiliar de Twitter en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="1f2d8-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="1f2d8-133">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="1f2d8-133">Additional Resources</span></span>


[<span data-ttu-id="1f2d8-134">Presentación de ASP.NET Web Pages 2: conceptos básicos de programación</span><span class="sxs-lookup"><span data-stu-id="1f2d8-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="1f2d8-135">Aplicación auxiliar de Twitter con WebMatrix</span><span class="sxs-lookup"><span data-stu-id="1f2d8-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
