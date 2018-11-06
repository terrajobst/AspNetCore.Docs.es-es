---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Aplicación auxiliar con ASP.NET Web Pages de Twitter | Microsoft Docs
author: Rick-Anderson
description: Este tema y la aplicación muestran cómo agregar una aplicación auxiliar de Twitter al proyecto WebMatrix 3. Contiene el código de aplicación auxiliar de Twitter y se muestra cómo llamar a la aplicación auxiliar...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 89c8c520cd32ca2ee24e6cd90e11f7bdf39c7a80
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020578"
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="73f90-104">Aplicación auxiliar de Twitter con ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="73f90-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="73f90-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="73f90-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="73f90-106">Este tema y la aplicación muestran cómo agregar una aplicación auxiliar de Twitter al proyecto WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="73f90-106">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="73f90-107">Contiene el código de aplicación auxiliar de Twitter y se muestra cómo llamar a los métodos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="73f90-107">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="73f90-108">Este código para el archivo Twitter.cshtml fue desarrollado por **Tian Pan** de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="73f90-108">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="73f90-109">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="73f90-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="73f90-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="73f90-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="73f90-111">Este tutorial también funciona con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="73f90-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="73f90-112">Introducción</span><span class="sxs-lookup"><span data-stu-id="73f90-112">Introduction</span></span>

<span data-ttu-id="73f90-113">En este tema se muestra cómo agregar una aplicación auxiliar de Twitter a la aplicación y usar la sintaxis de Razor para llamar a los métodos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="73f90-113">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="73f90-114">La aplicación auxiliar de Twitter facilita incorporar los botones de Twitter y widgets en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="73f90-114">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="73f90-115">Para utilizar un widget de Twitter, por ejemplo, la escala de tiempo de un usuario o los resultados de búsqueda para un hashtag, primero debe crear el [widget en Twitter](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="73f90-115">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="73f90-116">Después de crear el widget, recibirá un identificador de widget. Pasar el identificador de este widget como un parámetro al llamar a los métodos auxiliares que muestran el widget.</span><span class="sxs-lookup"><span data-stu-id="73f90-116">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="73f90-117">En este tema se escribió para la versión 1.1 de la API de Twitter.</span><span class="sxs-lookup"><span data-stu-id="73f90-117">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="73f90-118">Al agregar directamente el código de aplicación auxiliar de Twitter al proyecto, puede actualizar el código auxiliar si cambia de la API de Twitter.</span><span class="sxs-lookup"><span data-stu-id="73f90-118">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="73f90-119">Para obtener información acerca de cómo instalar WebMatrix, consulte [Introducción a ASP.NET Web Pages 2 - Introducción a](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="73f90-119">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="73f90-120">Agregar la aplicación auxiliar de Twitter al proyecto</span><span class="sxs-lookup"><span data-stu-id="73f90-120">Add Twitter Helper to your project</span></span>

<span data-ttu-id="73f90-121">Para agregar la aplicación auxiliar de Twitter, en primer lugar, agregue una carpeta denominada **aplicación\_código** al proyecto.</span><span class="sxs-lookup"><span data-stu-id="73f90-121">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="73f90-122">A continuación, cree un archivo denominado **Twitter.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="73f90-122">Then, create a file named **Twitter.cshtml**.</span></span>

![Carpeta App_Code](twitter-helper/_static/image1.png)

<span data-ttu-id="73f90-124">Reemplace el código predeterminado en Twitter.cshtml con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="73f90-124">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="73f90-125">Llamar a métodos de Twitter desde las páginas web</span><span class="sxs-lookup"><span data-stu-id="73f90-125">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="73f90-126">El ejemplo siguiente muestra cómo usar los métodos de aplicación auxiliar de Twitter desde una página en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="73f90-126">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="73f90-127">En el proyecto, desea reemplazar los valores de parámetro con valores que son relevantes para sus necesidades.</span><span class="sxs-lookup"><span data-stu-id="73f90-127">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="73f90-128">Puede usar los identificadores de widget proporcionado para explorar cómo funcionan los métodos, pero desea generar sus propios widgets para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="73f90-128">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="73f90-129">No todos los parámetros mostrados a continuación son necesarios.</span><span class="sxs-lookup"><span data-stu-id="73f90-129">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="73f90-130">Los parámetros opcionales se usan para personalizar cómo se muestra el botón o el widget.</span><span class="sxs-lookup"><span data-stu-id="73f90-130">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="73f90-131">Por ejemplo, el botón seguir sólo requiere el nombre de usuario para seguir, pero el ejemplo muestra cómo incluir el número de seguidores y cómo especificar el tamaño del botón y el idioma.</span><span class="sxs-lookup"><span data-stu-id="73f90-131">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="73f90-132">Ver los resultados</span><span class="sxs-lookup"><span data-stu-id="73f90-132">See the results</span></span>

<span data-ttu-id="73f90-133">El código anterior genera los siguientes botones y widgets.</span><span class="sxs-lookup"><span data-stu-id="73f90-133">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="73f90-134">Estos botones y los widgets son totalmente funcionales, no capturas de pantalla.</span><span class="sxs-lookup"><span data-stu-id="73f90-134">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="73f90-135">Se muestra el botón seguir en español porque se estableció el parámetro de idioma en **es**.</span><span class="sxs-lookup"><span data-stu-id="73f90-135">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="73f90-136">Siga el botón</span><span class="sxs-lookup"><span data-stu-id="73f90-136">Follow Button</span></span>

<span data-ttu-id="73f90-137">[Siga @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="73f90-137">[Follow @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="tweet-button"></a><span data-ttu-id="73f90-138">Botón de tweet</span><span class="sxs-lookup"><span data-stu-id="73f90-138">Tweet Button</span></span>

<span data-ttu-id="73f90-139">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="73f90-139">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="73f90-140">Escala de tiempo de usuario (perfil)</span><span class="sxs-lookup"><span data-stu-id="73f90-140">User Timeline (Profile)</span></span>

<span data-ttu-id="73f90-141">[TWEETS por @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="73f90-141">[Tweets by @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="favorites"></a><span data-ttu-id="73f90-142">Favoritos</span><span class="sxs-lookup"><span data-stu-id="73f90-142">Favorites</span></span>

<span data-ttu-id="73f90-143">[Tweets favoritas por @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="73f90-143">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="list"></a><span data-ttu-id="73f90-144">Lista</span><span class="sxs-lookup"><span data-stu-id="73f90-144">List</span></span>

<span data-ttu-id="73f90-145">[TWEETS de @Microsoft/MS \_consumidor\_bandas](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="73f90-145">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="search"></a><span data-ttu-id="73f90-146">Buscar</span><span class="sxs-lookup"><span data-stu-id="73f90-146">Search</span></span>

<span data-ttu-id="73f90-147">[TWEETS sobre &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="73f90-147">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>
