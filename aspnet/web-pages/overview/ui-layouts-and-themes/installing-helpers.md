---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Instalar una aplicación auxiliar en un ASP.NET Web Pages (Razor) sitio | Documentos de Microsoft
author: tfitzmac
description: En este artículo se describe cómo instalar una aplicación auxiliar en un sitio Web de ASP.NET Web Pages (Razor). Una aplicación auxiliar es un componente reutilizable que incluye código y marcado en por...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 766fbb87ae8bcb8917eb8fa7f552c00792242cf6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896788"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="b6d73-104">Instalar una aplicación auxiliar en un sitio de ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="b6d73-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="b6d73-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b6d73-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b6d73-106">En este artículo se describe cómo instalar una aplicación auxiliar en un sitio Web de ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="b6d73-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="b6d73-107">A *auxiliar* es un componente reutilizable que incluye código y marcado para realizar una tarea que podría ser una tarea tediosa o complejas.</span><span class="sxs-lookup"><span data-stu-id="b6d73-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="b6d73-108">Lo que aprenderá:</span><span class="sxs-lookup"><span data-stu-id="b6d73-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="b6d73-109">Cómo instalar una aplicación auxiliar en un sitio Web creado con 3 de WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="b6d73-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b6d73-110">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="b6d73-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="b6d73-111">3 de WebMatrix</span><span class="sxs-lookup"><span data-stu-id="b6d73-111">WebMatrix 3</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="b6d73-112">Información general de las aplicaciones auxiliares</span><span class="sxs-lookup"><span data-stu-id="b6d73-112">Overview of Helpers</span></span>

<span data-ttu-id="b6d73-113">Algunas tareas que personas a menudo es conveniente en las páginas web requieren una gran cantidad de código o requieren conocimientos adicionales.</span><span class="sxs-lookup"><span data-stu-id="b6d73-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="b6d73-114">Algunos ejemplos son mostrar un gráfico de datos; colocar un botón de Twitter "Seguir" en una página; al enviar correo electrónico de su sitio Web; recortar o cambiar el tamaño de las imágenes; uso de PayPal para su sitio.</span><span class="sxs-lookup"><span data-stu-id="b6d73-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="b6d73-115">Para que sea fácil de hacer este tipo de cosas, ASP.NET Web Pages le permite usar *aplicaciones auxiliares*.</span><span class="sxs-lookup"><span data-stu-id="b6d73-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="b6d73-116">Aplicaciones auxiliares son componentes que instalar para un sitio y que permiten realizan tareas habituales con solo una o dos líneas de código Razor.</span><span class="sxs-lookup"><span data-stu-id="b6d73-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="b6d73-117">ASP.NET Web Pages tiene algunas aplicaciones auxiliares integradas.</span><span class="sxs-lookup"><span data-stu-id="b6d73-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="b6d73-118">Sin embargo, muchas aplicaciones auxiliares están disponibles en paquetes (complementos) que se proporcionan con el Administrador de paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="b6d73-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="b6d73-119">NuGet permite seleccionar un paquete que desea instalar y, a continuación, se encarga de todos los detalles de la instalación.</span><span class="sxs-lookup"><span data-stu-id="b6d73-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="b6d73-120">Instalar una aplicación auxiliar de WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="b6d73-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="b6d73-121">En WebMatrix 3, haga clic en el **NuGet** botón.</span><span class="sxs-lookup"><span data-stu-id="b6d73-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![Cuadro de diálogo de la Galería de NuGet en WebMatrix](installing-helpers/_static/image1.png)
2. <span data-ttu-id="b6d73-123">Esto inicia el Administrador de paquetes de NuGet y muestra los paquetes disponibles.</span><span class="sxs-lookup"><span data-stu-id="b6d73-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="b6d73-124">En el cuadro de búsqueda, escriba una palabra clave para la aplicación auxiliar que se va a instalar.</span><span class="sxs-lookup"><span data-stu-id="b6d73-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![Cuadro de diálogo de la Galería de NuGet en WebMatrix](installing-helpers/_static/image2.png)
3. <span data-ttu-id="b6d73-126">Seleccione el paquete y, a continuación, haga clic en **instalar**.</span><span class="sxs-lookup"><span data-stu-id="b6d73-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="b6d73-127">Haga clic en **Sí** cuando se le pregunte si desea instalar el paquete e indique que acepta los términos.</span><span class="sxs-lookup"><span data-stu-id="b6d73-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

     <span data-ttu-id="b6d73-128">Si se trata de la primera vez que se ha instalado una aplicación auxiliar, NuGet crea carpetas en el sitio Web para el código que constituye la aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="b6d73-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
4. <span data-ttu-id="b6d73-129">Para desinstalar una aplicación auxiliar, haga clic en el **galería** botón, haga clic en el **instalado** pestaña y elegir el paquete que desea desinstalar.</span><span class="sxs-lookup"><span data-stu-id="b6d73-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="b6d73-130">Instalar la aplicación auxiliar de Twitter</span><span class="sxs-lookup"><span data-stu-id="b6d73-130">Installing the Twitter helper</span></span>

<span data-ttu-id="b6d73-131">La versión más reciente de la API de Twitter no es compatible con la aplicación auxiliar de Twitter que se instala a través de NuGet.</span><span class="sxs-lookup"><span data-stu-id="b6d73-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="b6d73-132">En su lugar, vea el [aplicación auxiliar de Twitter con WebMatrix](twitter-helper.md) tema para obtener información acerca de cómo configurar la aplicación auxiliar de Twitter en su proyecto.</span><span class="sxs-lookup"><span data-stu-id="b6d73-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="b6d73-133">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b6d73-133">Additional Resources</span></span>


[<span data-ttu-id="b6d73-134">2: conceptos básicos de programación de presentación ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="b6d73-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="b6d73-135">Aplicación auxiliar de Twitter con WebMatrix</span><span class="sxs-lookup"><span data-stu-id="b6d73-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
