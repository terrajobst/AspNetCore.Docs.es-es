---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Aplicación auxiliar con ASP.NET Web Pages de Twitter | Microsoft Docs
author: Rick-Anderson
description: Este tema y la aplicación muestran cómo agregar una aplicación auxiliar de Twitter al proyecto WebMatrix 3. Contiene el código de aplicación auxiliar de Twitter y se muestra cómo llamar a la aplicación auxiliar...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: fabe9f2b84d278a766dc8d8b7bfc00e1eb967127
ms.sourcegitcommit: 710fc5fcac258cc8415976dc66bdb355b3e061d5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "52299435"
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="46ea4-104">Aplicación auxiliar de Twitter con ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="46ea4-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="46ea4-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="46ea4-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="46ea4-106">Las aplicaciones auxiliares de Twitter están obsoletas.</span><span class="sxs-lookup"><span data-stu-id="46ea4-106">Twitter Helpers are obsolete.</span></span> <span data-ttu-id="46ea4-107">Para obtener herramientas engagement más recientes de Twitter para los sitios Web, consulte [Twitter para información general de los sitios Web](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span><span class="sxs-lookup"><span data-stu-id="46ea4-107">For Twitter's latest engagement tools for websites, see [Twitter for Websites Overview](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span></span>

> <span data-ttu-id="46ea4-108">Este tema y la aplicación muestran cómo agregar una aplicación auxiliar de Twitter al proyecto WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="46ea4-108">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="46ea4-109">Contiene el código de aplicación auxiliar de Twitter y se muestra cómo llamar a los métodos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="46ea4-109">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="46ea4-110">Este código para el archivo Twitter.cshtml fue desarrollado por **Tian Pan** de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="46ea4-110">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="46ea4-111">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="46ea4-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="46ea4-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="46ea4-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="46ea4-113">Este tutorial también funciona con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="46ea4-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="46ea4-114">Introducción</span><span class="sxs-lookup"><span data-stu-id="46ea4-114">Introduction</span></span>

<span data-ttu-id="46ea4-115">En este tema se muestra cómo agregar una aplicación auxiliar de Twitter a la aplicación y usar la sintaxis de Razor para llamar a los métodos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="46ea4-115">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="46ea4-116">La aplicación auxiliar de Twitter facilita incorporar los botones de Twitter y widgets en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="46ea4-116">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="46ea4-117">Para utilizar un widget de Twitter, por ejemplo, la escala de tiempo de un usuario o los resultados de búsqueda para un hashtag, primero debe crear el [widget en Twitter](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="46ea4-117">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="46ea4-118">Después de crear el widget, recibirá un identificador de widget. Pasar el identificador de este widget como un parámetro al llamar a los métodos auxiliares que muestran el widget.</span><span class="sxs-lookup"><span data-stu-id="46ea4-118">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="46ea4-119">En este tema se escribió para la versión 1.1 de la API de Twitter.</span><span class="sxs-lookup"><span data-stu-id="46ea4-119">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="46ea4-120">Al agregar directamente el código de aplicación auxiliar de Twitter al proyecto, puede actualizar el código auxiliar si cambia de la API de Twitter.</span><span class="sxs-lookup"><span data-stu-id="46ea4-120">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="46ea4-121">Para obtener información acerca de cómo instalar WebMatrix, consulte [Introducción a ASP.NET Web Pages 2 - Introducción a](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="46ea4-121">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="46ea4-122">Agregar la aplicación auxiliar de Twitter al proyecto</span><span class="sxs-lookup"><span data-stu-id="46ea4-122">Add Twitter Helper to your project</span></span>

<span data-ttu-id="46ea4-123">Para agregar la aplicación auxiliar de Twitter, en primer lugar, agregue una carpeta denominada **aplicación\_código** al proyecto.</span><span class="sxs-lookup"><span data-stu-id="46ea4-123">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="46ea4-124">A continuación, cree un archivo denominado **Twitter.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="46ea4-124">Then, create a file named **Twitter.cshtml**.</span></span>

![Carpeta App_Code](twitter-helper/_static/image1.png)

<span data-ttu-id="46ea4-126">Reemplace el código predeterminado en Twitter.cshtml con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="46ea4-126">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="46ea4-127">Llamar a métodos de Twitter desde las páginas web</span><span class="sxs-lookup"><span data-stu-id="46ea4-127">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="46ea4-128">El ejemplo siguiente muestra cómo usar los métodos de aplicación auxiliar de Twitter desde una página en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="46ea4-128">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="46ea4-129">En el proyecto, desea reemplazar los valores de parámetro con valores que son relevantes para sus necesidades.</span><span class="sxs-lookup"><span data-stu-id="46ea4-129">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="46ea4-130">Puede usar los identificadores de widget proporcionado para explorar cómo funcionan los métodos, pero desea generar sus propios widgets para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="46ea4-130">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="46ea4-131">No todos los parámetros mostrados a continuación son necesarios.</span><span class="sxs-lookup"><span data-stu-id="46ea4-131">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="46ea4-132">Los parámetros opcionales se usan para personalizar cómo se muestra el botón o el widget.</span><span class="sxs-lookup"><span data-stu-id="46ea4-132">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="46ea4-133">Por ejemplo, el botón seguir sólo requiere el nombre de usuario para seguir, pero el ejemplo muestra cómo incluir el número de seguidores y cómo especificar el tamaño del botón y el idioma.</span><span class="sxs-lookup"><span data-stu-id="46ea4-133">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="46ea4-134">Ver los resultados</span><span class="sxs-lookup"><span data-stu-id="46ea4-134">See the results</span></span>

<span data-ttu-id="46ea4-135">El código anterior genera los siguientes botones y widgets.</span><span class="sxs-lookup"><span data-stu-id="46ea4-135">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="46ea4-136">Estos botones y los widgets son totalmente funcionales, no capturas de pantalla.</span><span class="sxs-lookup"><span data-stu-id="46ea4-136">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="46ea4-137">Se muestra el botón seguir en español porque se estableció el parámetro de idioma en **es**.</span><span class="sxs-lookup"><span data-stu-id="46ea4-137">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="46ea4-138">Siga el botón</span><span class="sxs-lookup"><span data-stu-id="46ea4-138">Follow Button</span></span>

<span data-ttu-id="46ea4-139">[Siga @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="46ea4-139">[Follow @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="tweet-button"></a><span data-ttu-id="46ea4-140">Botón de tweet</span><span class="sxs-lookup"><span data-stu-id="46ea4-140">Tweet Button</span></span>

<span data-ttu-id="46ea4-141">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="46ea4-141">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="46ea4-142">Escala de tiempo de usuario (perfil)</span><span class="sxs-lookup"><span data-stu-id="46ea4-142">User Timeline (Profile)</span></span>

<span data-ttu-id="46ea4-143">[TWEETS por @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="46ea4-143">[Tweets by @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="favorites"></a><span data-ttu-id="46ea4-144">Favoritos</span><span class="sxs-lookup"><span data-stu-id="46ea4-144">Favorites</span></span>

<span data-ttu-id="46ea4-145">[Tweets favoritas por @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="46ea4-145">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="list"></a><span data-ttu-id="46ea4-146">Lista</span><span class="sxs-lookup"><span data-stu-id="46ea4-146">List</span></span>

<span data-ttu-id="46ea4-147">[TWEETS de @Microsoft/MS \_consumidor\_bandas](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="46ea4-147">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="search"></a><span data-ttu-id="46ea4-148">Buscar</span><span class="sxs-lookup"><span data-stu-id="46ea4-148">Search</span></span>

<span data-ttu-id="46ea4-149">[TWEETS sobre &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="46ea4-149">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>
