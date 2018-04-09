---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Usar AJAX para entregar las actualizaciones dinámicas | Documentos de Microsoft
author: microsoft
description: Paso 10 implementa la compatibilidad con los usuarios conectados a RSVP su interés en asiste a una cena, mediante un enfoque basado en Ajax integrado dentro de los detalles de la cena...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 7cea3ee2ec52261521941efac484e91a53f6310b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="d3127-103">Usar AJAX para entregar las actualizaciones dinámicas</span><span class="sxs-lookup"><span data-stu-id="d3127-103">Use AJAX to Deliver Dynamic Updates</span></span>
====================
<span data-ttu-id="d3127-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d3127-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="d3127-105">Descarga de PDF</span><span class="sxs-lookup"><span data-stu-id="d3127-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="d3127-106">Este es el paso 10 de una segunda ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que los recorridos de obtención de cómo generar una pequeña, pero completa, la aplicación web mediante ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="d3127-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="d3127-107">Paso 10 implementa la compatibilidad con los usuarios conectados a RSVP su interés en asiste a una cena, mediante un enfoque basado en Ajax integrado dentro de la página de detalles de la cena.</span><span class="sxs-lookup"><span data-stu-id="d3127-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="d3127-108">Si usa ASP.NET MVC 3, se recomienda que siga el [obtener iniciado con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [tienda de música de MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.</span><span class="sxs-lookup"><span data-stu-id="d3127-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="d3127-109">NerdDinner paso 10: Acepta de AJAX habilitar confirmaciones de asistencia</span><span class="sxs-lookup"><span data-stu-id="d3127-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="d3127-110">Implementemos ahora compatibilidad con los usuarios conectados a RSVP su interés en asiste a una cena.</span><span class="sxs-lookup"><span data-stu-id="d3127-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="d3127-111">Se deberá habilitar esta opción mediante un enfoque basado en AJAX integrado dentro de la página de detalles de la cena.</span><span class="sxs-lookup"><span data-stu-id="d3127-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="d3127-112">Que indica si el usuario es para RSVP</span><span class="sxs-lookup"><span data-stu-id="d3127-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="d3127-113">Los usuarios pueden visitar el */Dinners/detalles / [Id. de*] dirección URL para ver detalles sobre una cena determinada:</span><span class="sxs-lookup"><span data-stu-id="d3127-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="d3127-114">El Details() se implementa el método de acción de este modo:</span><span class="sxs-lookup"><span data-stu-id="d3127-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="d3127-115">Nuestro primer paso para implementar la compatibilidad RSVP será para agregar un método de aplicación auxiliar de "IsUserRegistered(username)" a nuestro objeto cena (dentro de la clase parcial Dinner.cs que se generó anteriormente).</span><span class="sxs-lookup"><span data-stu-id="d3127-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="d3127-116">Este método auxiliar devuelve true o false, según si el usuario actualmente para RSVP de la cena:</span><span class="sxs-lookup"><span data-stu-id="d3127-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="d3127-117">A continuación, podemos agregar el código siguiente a la plantilla de vista Details.aspx para mostrar un mensaje adecuado que indica si el usuario está registrado o no para el evento:</span><span class="sxs-lookup"><span data-stu-id="d3127-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="d3127-118">Y ahora cuando un usuario visita una cena están registrados para verá este mensaje:</span><span class="sxs-lookup"><span data-stu-id="d3127-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="d3127-119">Y cuando visite una cena no están registrados para verán el mensaje siguiente:</span><span class="sxs-lookup"><span data-stu-id="d3127-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="d3127-120">Implementar el método de acción de registro</span><span class="sxs-lookup"><span data-stu-id="d3127-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="d3127-121">Ahora agreguemos la funcionalidad necesaria para permitir a los usuarios a RSVP para una cena desde la página de detalles.</span><span class="sxs-lookup"><span data-stu-id="d3127-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="d3127-122">Para hacerlo, vamos a crear una nueva clase de "RSVPController", el botón secundario en el directorio \Controllers y elija Agregar -&gt;comando de menú del controlador.</span><span class="sxs-lookup"><span data-stu-id="d3127-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="d3127-123">Implementaremos un método de acción "Register" dentro de la nueva clase RSVPController que toma un identificador para una cena como argumento, recupera el objeto de cena adecuado, comprueba si el usuario ha iniciado la sesión está actualmente en la lista de usuarios que se hayan registrado para él y si no agrega un objeto RSVP para ellos:</span><span class="sxs-lookup"><span data-stu-id="d3127-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="d3127-124">Tenga en cuenta sobre cómo nos estamos devolver una cadena simple como el resultado del método de acción.</span><span class="sxs-lookup"><span data-stu-id="d3127-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="d3127-125">Se pudo ha incrustado este mensaje dentro de una plantilla de vista: pero ya es tan pequeña, utilizaremos el método de aplicación auxiliar de Content() en la clase de base de controlador y devolver un mensaje de cadena, como por encima.</span><span class="sxs-lookup"><span data-stu-id="d3127-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="d3127-126">Una llamada al método de acción de RSVPForEvent con AJAX</span><span class="sxs-lookup"><span data-stu-id="d3127-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="d3127-127">Vamos a usar AJAX para invocar el método de acción de registro de la vista de detalles.</span><span class="sxs-lookup"><span data-stu-id="d3127-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="d3127-128">Esta implementación es bastante fácil.</span><span class="sxs-lookup"><span data-stu-id="d3127-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="d3127-129">Primero vamos a agregar dos referencias de biblioteca de secuencia de comandos:</span><span class="sxs-lookup"><span data-stu-id="d3127-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="d3127-130">La primera biblioteca hace referencia a la biblioteca básica de script de cliente AJAX de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d3127-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="d3127-131">Este archivo es aproximadamente 24 KB de tamaño (comprimido) y contiene la funcionalidad de AJAX de cliente principal.</span><span class="sxs-lookup"><span data-stu-id="d3127-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="d3127-132">La segunda biblioteca contiene funciones de utilidad que se integran con integrados AJAX auxiliar métodos de ASP.NET MVC (que vamos a usar en breve).</span><span class="sxs-lookup"><span data-stu-id="d3127-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="d3127-133">Se puede, a continuación, el código de plantilla de la vista se agregó anteriormente para que en lugar de outputing un mensaje "No se ha registrado para este evento", en su lugar se procesará un vínculo que cuando envíe la actualización realiza una llamada de AJAX que invoca el método de acción RSVPForEvent en nuestro controlador RSVP y RSVPs el usuario:</span><span class="sxs-lookup"><span data-stu-id="d3127-133">We can then update the view template code we added earlier so that instead of outputing a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="d3127-134">El método de aplicación auxiliar de Ajax.ActionLink() usado anteriormente está integrada en ASP.NET MVC y es similar al método auxiliar Html.ActionLink() salvo que en lugar de realizar una exploración estándar realiza una llamada AJAX para el método de acción cuando se hace clic en el vínculo.</span><span class="sxs-lookup"><span data-stu-id="d3127-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="d3127-135">Anteriormente estamos llamando al método de acción de "Register" en el controlador "RSVP" y pasarle el DinnerID como el parámetro "id".</span><span class="sxs-lookup"><span data-stu-id="d3127-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="d3127-136">El último parámetro AjaxOptions pasamos indica que deseamos tomar el contenido devuelto por el método de acción y actualizar el código HTML &lt;div&gt; elemento de la página cuyo identificador es "rsvpmsg".</span><span class="sxs-lookup"><span data-stu-id="d3127-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="d3127-137">Y ahora cuando un usuario se desplaza a una cena que no están registrados para aún verá un vínculo a RSVP para él:</span><span class="sxs-lookup"><span data-stu-id="d3127-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="d3127-138">Si hacen clic en el vínculo "RSVP para este evento" realizará una llamada AJAX para el método de acción de registro en el controlador RSVP y, cuando se complete verán un mensaje actualizado como las siguientes:</span><span class="sxs-lookup"><span data-stu-id="d3127-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="d3127-139">El tráfico implicados cuando se realiza esta llamada de AJAX y ancho de banda de red es realmente ligero.</span><span class="sxs-lookup"><span data-stu-id="d3127-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="d3127-140">Cuando el usuario hace clic en el vínculo "RSVP para este evento", se realiza una solicitud de red de HTTP POST pequeña para la */Dinners/Register/1* dirección URL que parece que a continuación en la conexión:</span><span class="sxs-lookup"><span data-stu-id="d3127-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="d3127-141">Y la respuesta desde el método de acción de registro es simplemente:</span><span class="sxs-lookup"><span data-stu-id="d3127-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="d3127-142">Esta llamada ligera es rápida y funcionará incluso a través de una red lenta.</span><span class="sxs-lookup"><span data-stu-id="d3127-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="d3127-143">Agregar una animación de jQuery</span><span class="sxs-lookup"><span data-stu-id="d3127-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="d3127-144">La funcionalidad de AJAX implementamos funciona bien y rápido.</span><span class="sxs-lookup"><span data-stu-id="d3127-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="d3127-145">A veces puede ocurrir tan rápido, no obstante, que un usuario no puede observar que el vínculo RSVP se ha reemplazado por otro nuevo.</span><span class="sxs-lookup"><span data-stu-id="d3127-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="d3127-146">Hacer que el resultado un poco más obvio que podemos agregar una animación sencilla para llamar la atención sobre el mensaje de actualización.</span><span class="sxs-lookup"><span data-stu-id="d3127-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="d3127-147">El valor predeterminado de plantilla de proyecto de ASP.NET MVC incluye jQuery: una biblioteca de JavaScript de código abierto excelente (y muy popular) que también es compatible con Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d3127-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="d3127-148">jQuery proporciona una serie de características, como una biblioteca "nice" de selección y efectos de DOM de HTML.</span><span class="sxs-lookup"><span data-stu-id="d3127-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="d3127-149">Para usar jQuery vamos a agregar una referencia de script a él.</span><span class="sxs-lookup"><span data-stu-id="d3127-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="d3127-150">Dado que vamos a usar jQuery en diversos lugares dentro de nuestro sitio, vamos a agregar la referencia de script dentro de nuestro archivo de página maestra Site.master para que todas las páginas pueden utilizarlo.</span><span class="sxs-lookup"><span data-stu-id="d3127-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="d3127-151">*Sugerencia: asegúrese de que ha instalado la revisión de intellisense de JavaScript para VS 2008 SP1 que permite una compatibilidad de intellisense para los archivos JavaScript (incluidos jQuery). Puede descargar desde: http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="d3127-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="d3127-152">El código escrito mediante JQuery a menudo utiliza un "$ ()" global método JavaScript que recupera uno o varios elementos HTML mediante un selector CSS.</span><span class="sxs-lookup"><span data-stu-id="d3127-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="d3127-153">Por ejemplo, <em>$("#rsvpmsg")</em> selecciona cualquier elemento HTML con el Id. de rsvpmsg, mientras que <em>$(".something")</em> seleccionaría todos los elementos con el "elemento" CSS nombre de clase.</span><span class="sxs-lookup"><span data-stu-id="d3127-153">For example, <em>$("#rsvpmsg")</em> selects any HTML element with the id of rsvpmsg, while <em>$(".something")</em> would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="d3127-154">También puede escribir consultas más avanzadas, como "se deben devolver todos los botones de radio comprobado" mediante una consulta de selector como: <em>$("entrada [@type= radio] [@checked]")</em>.</span><span class="sxs-lookup"><span data-stu-id="d3127-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: <em>$("input[@type=radio][@checked]")</em>.</span></span>

<span data-ttu-id="d3127-155">Una vez que haya seleccionado los elementos, puede llamar a métodos en ellas para actuar como ocultarlas: *$("#rsvpmsg").hide();*</span><span class="sxs-lookup"><span data-stu-id="d3127-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="d3127-156">En nuestro caso RSVP, se definirá una función de JavaScript simple denominada "AnimateRSVPMessage" que selecciona "rsvpmsg" &lt;div&gt; y anima el tamaño de su contenido de texto.</span><span class="sxs-lookup"><span data-stu-id="d3127-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="d3127-157">El siguiente código inicia el texto pequeño y, a continuación, hace que aumente a través de un período de tiempo de 400 milisegundos:</span><span class="sxs-lookup"><span data-stu-id="d3127-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="d3127-158">Le podemos, a continuación, el cable telefónico esta función de JavaScript que se llama después de la llamada de AJAX se complete correctamente pasando su nombre para el método de aplicación auxiliar de Ajax.ActionLink() (a través de la AjaxOptions "OnSuccess" propiedad del evento):</span><span class="sxs-lookup"><span data-stu-id="d3127-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="d3127-159">Ahora cuando se hace clic en el vínculo "RSVP para este evento" y la llamada de AJAX completa correctamente, el mensaje contenido enviado back se y animar aumentan de tamaño:</span><span class="sxs-lookup"><span data-stu-id="d3127-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="d3127-160">Además de proporcionar un evento de "OnSuccess", el objeto AjaxOptions expone métodos OnBegin, OnFailure y OnComplete eventos que puede controlar (junto con una variedad de otras propiedades y acciones útiles).</span><span class="sxs-lookup"><span data-stu-id="d3127-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="d3127-161">Limpieza - refactorizar una vista parcial RSVP</span><span class="sxs-lookup"><span data-stu-id="d3127-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="d3127-162">La plantilla de vista de detalles está empezando a un poco tiempo y las horas extraordinarias realizará un poco más difícil de comprender.</span><span class="sxs-lookup"><span data-stu-id="d3127-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="d3127-163">Para ayudar a mejorar la legibilidad del código, vamos a terminar mediante la creación de una vista parcial – RSVPStatus.ascx – que encapsulan todo el código de la vista RSVP para nuestra página de detalles.</span><span class="sxs-lookup"><span data-stu-id="d3127-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="d3127-164">Podemos hacer esto con el botón secundario en la carpeta \Views\Dinners y, a continuación, elija Agregar -&gt;ver el comando de menú.</span><span class="sxs-lookup"><span data-stu-id="d3127-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="d3127-165">Haremos lo que toman un objeto de la cena como su modelo de vista fuertemente tipada.</span><span class="sxs-lookup"><span data-stu-id="d3127-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="d3127-166">Le podemos, a continuación, copiar y pegar el contenido RSVP de nuestra vista Details.aspx en él.</span><span class="sxs-lookup"><span data-stu-id="d3127-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="d3127-167">Una vez que hemos hecho, vamos a crear otra vista parcial: EditAndDeleteLinks.ascx - que encapsula el código de vista de vínculo Editar y eliminar.</span><span class="sxs-lookup"><span data-stu-id="d3127-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="d3127-168">Se tendrá toman un objeto de la cena como su modelo de vista fuertemente tipada y copiar y pegar la lógica de editar y eliminar de la vista Details.aspx en él.</span><span class="sxs-lookup"><span data-stu-id="d3127-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="d3127-169">Nuestro detalles ver plantilla puede, a continuación, simplemente incluyen dos llamadas al método Html.RenderPartial() en la parte inferior:</span><span class="sxs-lookup"><span data-stu-id="d3127-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="d3127-170">Esto hace que el código más limpia de leer y mantener.</span><span class="sxs-lookup"><span data-stu-id="d3127-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="d3127-171">Paso siguiente</span><span class="sxs-lookup"><span data-stu-id="d3127-171">Next Step</span></span>

<span data-ttu-id="d3127-172">Ahora veamos cómo podemos usar AJAX aún más y agregar compatibilidad con la asignación interactivo a nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="d3127-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d3127-173">[Anterior](secure-applications-using-authentication-and-authorization.md)
> [Siguiente](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="d3127-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
