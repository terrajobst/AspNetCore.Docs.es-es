---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Aplicación auxiliar con ASP.NET Web Pages de Twitter | Documentos de Microsoft
author: tfitzmac
description: Este tema y la aplicación muestran cómo agregar una aplicación auxiliar de Twitter para el proyecto de 3 de WebMatrix. Contiene el código de aplicación auxiliar de Twitter y muestra cómo llamar a la aplicación auxiliar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 07d9c4d485c42b78a42c54c9740af5f67cb44763
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2018
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="f2638-104">Aplicación auxiliar de Twitter con páginas Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f2638-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="f2638-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f2638-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f2638-106">Este tema y la aplicación muestran cómo agregar una aplicación auxiliar de Twitter para el proyecto de 3 de WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="f2638-106">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="f2638-107">Contiene el código de aplicación auxiliar de Twitter y muestra cómo llamar a los métodos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="f2638-107">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="f2638-108">Este código para el archivo Twitter.cshtml fue desarrollado por **Tian Pan** de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="f2638-108">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f2638-109">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="f2638-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f2638-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="f2638-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="f2638-111">Este tutorial también funciona con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="f2638-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="f2638-112">Introducción</span><span class="sxs-lookup"><span data-stu-id="f2638-112">Introduction</span></span>

<span data-ttu-id="f2638-113">Este tema muestra cómo agregar una aplicación auxiliar de Twitter para la aplicación y utilizar la sintaxis de Razor para llamar a los métodos de aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="f2638-113">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="f2638-114">La aplicación auxiliar de Twitter facilita la incorporar Twitter botones y los widgets de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f2638-114">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="f2638-115">Para utilizar un widget de Twitter, por ejemplo, la escala de tiempo de un usuario o los resultados de búsqueda de una hashtag, primero debe crear el [widget en Twitter](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="f2638-115">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="f2638-116">Después de crear el widget, recibirá un identificador de widget. Pasar este Id. de widget como un parámetro al llamar a los métodos auxiliares que muestran el widget.</span><span class="sxs-lookup"><span data-stu-id="f2638-116">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="f2638-117">En este tema se escribió para la versión 1.1 de la API de Twitter.</span><span class="sxs-lookup"><span data-stu-id="f2638-117">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="f2638-118">Agregar directamente el código de aplicación auxiliar de Twitter a su proyecto, puede actualizar el código auxiliar si cambia de la API de Twitter.</span><span class="sxs-lookup"><span data-stu-id="f2638-118">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="f2638-119">Para obtener información acerca de cómo instalar WebMatrix, consulte [Introducción a ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="f2638-119">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="f2638-120">Agregar aplicación auxiliar de Twitter al proyecto</span><span class="sxs-lookup"><span data-stu-id="f2638-120">Add Twitter Helper to your project</span></span>

<span data-ttu-id="f2638-121">Para agregar la aplicación auxiliar de Twitter, en primer lugar, agregue una carpeta denominada **aplicación\_código** al proyecto.</span><span class="sxs-lookup"><span data-stu-id="f2638-121">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="f2638-122">A continuación, cree un archivo denominado **Twitter.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="f2638-122">Then, create a file named **Twitter.cshtml**.</span></span>

![Carpeta App_Code](twitter-helper/_static/image1.png)

<span data-ttu-id="f2638-124">Reemplace el código predeterminado en Twitter.cshtml con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="f2638-124">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="f2638-125">Llamar a métodos de Twitter desde las páginas web</span><span class="sxs-lookup"><span data-stu-id="f2638-125">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="f2638-126">En el ejemplo siguiente se muestra cómo usar los métodos de aplicación auxiliar de Twitter de una página en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="f2638-126">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="f2638-127">En el proyecto, desea reemplazar los valores de parámetro con valores que son relevantes para sus necesidades.</span><span class="sxs-lookup"><span data-stu-id="f2638-127">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="f2638-128">Puede usar los identificadores de widget proporcionado para explorar cómo funcionan los métodos, pero desea generar sus propios widgets para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="f2638-128">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="f2638-129">No todos los parámetros que se muestra a continuación son necesarios.</span><span class="sxs-lookup"><span data-stu-id="f2638-129">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="f2638-130">Los parámetros opcionales se utilizan para personalizar cómo se muestra el botón o el widget.</span><span class="sxs-lookup"><span data-stu-id="f2638-130">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="f2638-131">Por ejemplo, el botón siga solo requiere el nombre de usuario debe seguir, pero en el ejemplo se muestra cómo incluir el número de seguidores de y cómo especificar el tamaño del botón y el idioma.</span><span class="sxs-lookup"><span data-stu-id="f2638-131">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="f2638-132">Ver los resultados</span><span class="sxs-lookup"><span data-stu-id="f2638-132">See the results</span></span>

<span data-ttu-id="f2638-133">El código anterior genera los siguientes botones y widgets.</span><span class="sxs-lookup"><span data-stu-id="f2638-133">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="f2638-134">Estos botones y widgets son totalmente funcionales, no capturas de pantalla.</span><span class="sxs-lookup"><span data-stu-id="f2638-134">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="f2638-135">Se muestra el botón siga en español porque el parámetro de idioma se estableció en **es**.</span><span class="sxs-lookup"><span data-stu-id="f2638-135">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="f2638-136">Siga el botón</span><span class="sxs-lookup"><span data-stu-id="f2638-136">Follow Button</span></span>

[<span data-ttu-id="f2638-137">Siga @aspnet)</span><span class="sxs-lookup"><span data-stu-id="f2638-137">Follow @aspnet)</span></span>](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>

### <a name="tweet-button"></a><span data-ttu-id="f2638-138">Botón tweet</span><span class="sxs-lookup"><span data-stu-id="f2638-138">Tweet Button</span></span>

[<span data-ttu-id="f2638-139">Tweet</span><span class="sxs-lookup"><span data-stu-id="f2638-139">Tweet</span></span>](https://twitter.com/share)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>

### <a name="user-timeline-profile"></a><span data-ttu-id="f2638-140">Escala de tiempo de usuario (perfil)</span><span class="sxs-lookup"><span data-stu-id="f2638-140">User Timeline (Profile)</span></span>

[<span data-ttu-id="f2638-141">TWEETS por @aspnet</span><span class="sxs-lookup"><span data-stu-id="f2638-141">Tweets by @aspnet</span></span>](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="favorites"></a><span data-ttu-id="f2638-142">Favoritos</span><span class="sxs-lookup"><span data-stu-id="f2638-142">Favorites</span></span>

[<span data-ttu-id="f2638-143">Favoritos Tweets por @Microsoft</span><span class="sxs-lookup"><span data-stu-id="f2638-143">Favorite Tweets by @Microsoft</span></span>](https://twitter.com/Microsoft/favorites)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="list"></a><span data-ttu-id="f2638-144">Lista</span><span class="sxs-lookup"><span data-stu-id="f2638-144">List</span></span>

[<span data-ttu-id="f2638-145">TWEETS de @Microsoft/MS \_consumidor\_bandas</span><span class="sxs-lookup"><span data-stu-id="f2638-145">Tweets from @Microsoft/MS\_Consumer\_Bands</span></span>](https://twitter.com/microsoft/ms-consumer-brands/)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="search"></a><span data-ttu-id="f2638-146">Buscar</span><span class="sxs-lookup"><span data-stu-id="f2638-146">Search</span></span>

[<span data-ttu-id="f2638-147">Tuits acerca de &quot;#asp.net&quot;</span><span class="sxs-lookup"><span data-stu-id="f2638-147">Tweets about &quot;#asp.net&quot;</span></span>](https://twitter.com/search?q=%23asp.net)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>
