---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Usar AJAX para entregar actualizaciones dinámicas | Microsoft Docs
author: microsoft
description: Paso 10 implementa compatibilidad con los usuarios conectados a RSVP su interés por asistir a una cena, mediante un enfoque basado en Ajax integrado en los detalles de la cena...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 9f11c4c15c0ac9bab8d53b18a4e07be4b864b2c7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825206"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="9a617-103">Usar AJAX para entregar actualizaciones dinámicas</span><span class="sxs-lookup"><span data-stu-id="9a617-103">Use AJAX to Deliver Dynamic Updates</span></span>
====================
<span data-ttu-id="9a617-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="9a617-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="9a617-105">Descargar PDF</span><span class="sxs-lookup"><span data-stu-id="9a617-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="9a617-106">Este es el paso 10 de manera gratuita ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que le guía a través cómo compilar un pequeño, pero completa, aplicación web mediante ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="9a617-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="9a617-107">Paso 10 implementa compatibilidad con los usuarios conectados a RSVP su interés por asistir a una cena, mediante un enfoque basado en Ajax integrado en la página de detalles de la cena.</span><span class="sxs-lookup"><span data-stu-id="9a617-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="9a617-108">Si usa ASP.NET MVC 3, se recomienda que siga el [Introducción a trabajar con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.</span><span class="sxs-lookup"><span data-stu-id="9a617-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="9a617-109">NerdDinner paso 10: Acepta habilitación de confirmaciones de asistencia de AJAX</span><span class="sxs-lookup"><span data-stu-id="9a617-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="9a617-110">Implementemos ahora soporte técnico para los usuarios conectados a RSVP su interés por asistir a una cena.</span><span class="sxs-lookup"><span data-stu-id="9a617-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="9a617-111">Esto Habilitaremos mediante un enfoque basado en AJAX integrado en la página de detalles de la cena.</span><span class="sxs-lookup"><span data-stu-id="9a617-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="9a617-112">Que indica si el usuario está confirmado</span><span class="sxs-lookup"><span data-stu-id="9a617-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="9a617-113">Los usuarios pueden visitar el */dinners/detalles / [Id. de*] dirección URL para ver detalles sobre una cena en particular:</span><span class="sxs-lookup"><span data-stu-id="9a617-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="9a617-114">El Details() se implementa el método de acción de este modo:</span><span class="sxs-lookup"><span data-stu-id="9a617-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="9a617-115">Nuestro primer paso para implementar la compatibilidad RSVP será agregar un método de aplicación auxiliar "IsUserRegistered(username)" a nuestro objeto cena (dentro de la clase parcial Dinner.cs que hemos creado anteriormente).</span><span class="sxs-lookup"><span data-stu-id="9a617-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="9a617-116">Este método auxiliar devuelve true o false dependiendo de si el usuario está confirmado actualmente para la cena:</span><span class="sxs-lookup"><span data-stu-id="9a617-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="9a617-117">El código siguiente, a continuación, podemos agregar a nuestra plantilla de vista Details.aspx para mostrar un mensaje adecuado que indica si el usuario está registrado o no para el evento:</span><span class="sxs-lookup"><span data-stu-id="9a617-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="9a617-118">Y ahora cuando un usuario visita una cena están registrados para verá este mensaje:</span><span class="sxs-lookup"><span data-stu-id="9a617-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="9a617-119">Y cuando se visite una cena no están registrados para que verá el siguiente mensaje:</span><span class="sxs-lookup"><span data-stu-id="9a617-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="9a617-120">Implementación del método de acción de registro</span><span class="sxs-lookup"><span data-stu-id="9a617-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="9a617-121">Ahora agreguemos la funcionalidad necesaria para permitir que los usuarios a RSVP una cena desde la página de detalles.</span><span class="sxs-lookup"><span data-stu-id="9a617-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="9a617-122">Para implementar esto, vamos a crear una nueva clase "RSVPController" con el botón secundario en el directorio \Controllers y eligiendo agregar -&gt;comando de menú del controlador.</span><span class="sxs-lookup"><span data-stu-id="9a617-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="9a617-123">Implementaremos un método de acción "Register" dentro de la nueva clase RSVPController que toma un identificador para una cena como argumento, recupera el objeto apropiado de cena, comprueba si el usuario ha iniciado sesión actualmente está en la lista de usuarios que se han registrado para él y si no se agrega un objeto de confirmación de asistencia para ellos:</span><span class="sxs-lookup"><span data-stu-id="9a617-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="9a617-124">Tenga en cuenta sobre cómo nos estamos devolver una cadena sencilla como la salida del método de acción.</span><span class="sxs-lookup"><span data-stu-id="9a617-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="9a617-125">Nos podríamos tener insertados este mensaje en una plantilla de vista: pero, ya que es tan pequeño, utilizaremos el método auxiliar Content() en la clase base del controlador y devuelven un mensaje de cadena del tipo anteriormente.</span><span class="sxs-lookup"><span data-stu-id="9a617-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="9a617-126">Una llamada al método de acción RSVPForEvent con AJAX</span><span class="sxs-lookup"><span data-stu-id="9a617-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="9a617-127">Vamos a usar AJAX para invocar el método de acción Register de la vista de detalles.</span><span class="sxs-lookup"><span data-stu-id="9a617-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="9a617-128">Esta implementación es bastante fácil.</span><span class="sxs-lookup"><span data-stu-id="9a617-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="9a617-129">En primer lugar, vamos a agregar dos referencias de biblioteca de secuencias de comandos:</span><span class="sxs-lookup"><span data-stu-id="9a617-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="9a617-130">La primera biblioteca hace referencia a la biblioteca de script de cliente AJAX de ASP.NET core.</span><span class="sxs-lookup"><span data-stu-id="9a617-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="9a617-131">Este archivo es aproximadamente 24 KB de tamaño (comprimido) y contiene la funcionalidad de AJAX del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="9a617-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="9a617-132">La segunda biblioteca contiene funciones de utilidad que se integran con integrados auxiliar los métodos de AJAX de ASP.NET MVC (lo que vamos a usar en breve).</span><span class="sxs-lookup"><span data-stu-id="9a617-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="9a617-133">Se puede, a continuación, se agregó anteriormente para que en lugar de un mensaje "No están registrados para este evento" outputing, en su lugar representamos un vínculo que cuando inserta el código de plantilla de vista de actualización realiza una llamada de AJAX que invoca el método de acción RSVPForEvent en nuestro controlador de RSVP y RSVPs el usuario:</span><span class="sxs-lookup"><span data-stu-id="9a617-133">We can then update the view template code we added earlier so that instead of outputing a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="9a617-134">El método auxiliar de Ajax.ActionLink() usado anteriormente está integrada en ASP.NET MVC y es similar al método auxiliar Html.ActionLink(), salvo que en lugar de realizar una exploración estándar realiza una llamada AJAX al método de acción cuando se hace clic en el vínculo.</span><span class="sxs-lookup"><span data-stu-id="9a617-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="9a617-135">Anteriormente estamos llamando al método de acción de "Register" en el controlador "RSVP" y pasándole el DinnerID como el parámetro "id".</span><span class="sxs-lookup"><span data-stu-id="9a617-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="9a617-136">El último parámetro AjaxOptions que estamos pasando indica que deseamos tomar el contenido devuelto por el método de acción y actualizar el código HTML &lt;div&gt; elemento en la página cuyo identificador es "rsvpmsg".</span><span class="sxs-lookup"><span data-stu-id="9a617-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="9a617-137">Y ahora, cuando un usuario explora una cena no están registrados para aún verán un vínculo a RSVP para él:</span><span class="sxs-lookup"><span data-stu-id="9a617-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="9a617-138">Si hace clic en el vínculo "RSVP para este evento" hará una llamada AJAX al método de acción de registro en el controlador de RSVP, y cuando se complete, verán un mensaje actualizado como el siguiente:</span><span class="sxs-lookup"><span data-stu-id="9a617-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="9a617-139">El ancho de banda y el tráfico que tienen lugar al realizar esta llamada AJAX está realmente ligera.</span><span class="sxs-lookup"><span data-stu-id="9a617-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="9a617-140">Cuando el usuario hace clic en el vínculo "RSVP para este evento", se realiza una solicitud de red pequeña de HTTP POST a la */Dinners/Register/1* dirección URL que se parece a continuación en la conexión:</span><span class="sxs-lookup"><span data-stu-id="9a617-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="9a617-141">Y la respuesta de nuestro método de acción Register es simplemente:</span><span class="sxs-lookup"><span data-stu-id="9a617-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="9a617-142">Esta llamada ligera es rápida y funcionará incluso a través de una red lenta.</span><span class="sxs-lookup"><span data-stu-id="9a617-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="9a617-143">Adición de una animación de jQuery</span><span class="sxs-lookup"><span data-stu-id="9a617-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="9a617-144">La funcionalidad de AJAX que implementamos funciona bien y rápida.</span><span class="sxs-lookup"><span data-stu-id="9a617-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="9a617-145">A veces puede ocurrir tan rápido, sin embargo, que un usuario no es posible que observe que el vínculo RSVP ha sido reemplazado por texto nuevo.</span><span class="sxs-lookup"><span data-stu-id="9a617-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="9a617-146">Para hacer que el resultado sea un poco más obvio podemos agregar una animación sencilla para llamar la atención sobre el mensaje de actualización.</span><span class="sxs-lookup"><span data-stu-id="9a617-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="9a617-147">El valor predeterminado de la plantilla de proyecto de ASP.NET MVC incluye jQuery: una biblioteca de JavaScript de código abierto de excelente (y muy popular) que también es compatible con Microsoft.</span><span class="sxs-lookup"><span data-stu-id="9a617-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="9a617-148">jQuery proporciona una serie de características, como una biblioteca de efectos y selección de DOM de HTML agradable.</span><span class="sxs-lookup"><span data-stu-id="9a617-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="9a617-149">Para usar jQuery vamos a agregar una referencia de script a ella.</span><span class="sxs-lookup"><span data-stu-id="9a617-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="9a617-150">Puesto que vamos a usar jQuery en una variedad de lugares dentro de nuestro sitio, vamos a agregar la referencia de script dentro de nuestro archivo de página maestra Site.master para que todas las páginas pueden usarlo.</span><span class="sxs-lookup"><span data-stu-id="9a617-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="9a617-151">*Sugerencia: asegúrese de que ha instalado la revisión de intellisense de JavaScript de VS 2008 SP1 que permite mayor compatibilidad de intellisense para archivos de JavaScript (como jQuery). Puede descargarlo desde: http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="9a617-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="9a617-152">El código escrito mediante JQuery a menudo usa un "$ ()" global método JavaScript que recupera uno o varios elementos HTML mediante un selector de CSS.</span><span class="sxs-lookup"><span data-stu-id="9a617-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="9a617-153">Por ejemplo, <em>$("#rsvpmsg")</em> selecciona cualquier elemento HTML con el Id. de rsvpmsg, mientras que <em>$(".something")</em> seleccionaría todos los elementos con el "algo" CSS nombre de clase.</span><span class="sxs-lookup"><span data-stu-id="9a617-153">For example, <em>$("#rsvpmsg")</em> selects any HTML element with the id of rsvpmsg, while <em>$(".something")</em> would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="9a617-154">También puede escribir consultas más avanzadas, como "devolver todos los botones de radio checked" mediante una consulta de selector como: <em>$("entrada [@type= radio] [@checked]")</em>.</span><span class="sxs-lookup"><span data-stu-id="9a617-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: <em>$("input[@type=radio][@checked]")</em>.</span></span>

<span data-ttu-id="9a617-155">Una vez que haya seleccionado los elementos, puede llamar a métodos en ellos para llevar a cabo acciones como ocultarlas: *"$(#rsvpmsg").hide();*</span><span class="sxs-lookup"><span data-stu-id="9a617-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="9a617-156">Este escenario de RSVP, definiremos una simple función de JavaScript denominada "AnimateRSVPMessage" que selecciona el "rsvpmsg" &lt;div&gt; y anima el tamaño de su contenido de texto.</span><span class="sxs-lookup"><span data-stu-id="9a617-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="9a617-157">El código siguiente inicia el texto pequeño y, a continuación, las causas que aumente su a través de un período de 400 milisegundos:</span><span class="sxs-lookup"><span data-stu-id="9a617-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="9a617-158">Nos podemos, a continuación, cable copia de esta función de JavaScript que se llamará después nuestra llamada AJAX finaliza correctamente pasando su nombre al método auxiliar Ajax.ActionLink() (a través de la AjaxOptions "OnSuccess" propiedad del evento):</span><span class="sxs-lookup"><span data-stu-id="9a617-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="9a617-159">Y ahora cuando se hace clic en el vínculo "RSVP para este evento" y nuestra llamada AJAX finaliza correctamente, el mensaje contenido enviado back se animar y aumentan de tamaño:</span><span class="sxs-lookup"><span data-stu-id="9a617-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="9a617-160">Además de proporcionar un evento "OnSuccess", el objeto AjaxOptions expone métodos OnBegin OnFailure y OnComplete eventos que puede controlar (junto con una variedad de otras propiedades y opciones útiles).</span><span class="sxs-lookup"><span data-stu-id="9a617-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="9a617-161">Limpieza - refactorizar una vista parcial de RSVP</span><span class="sxs-lookup"><span data-stu-id="9a617-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="9a617-162">Nuestra plantilla de vista de detalles está comenzando a obtener un poco largo, qué horas extra resultará un poco más difícil de entender.</span><span class="sxs-lookup"><span data-stu-id="9a617-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="9a617-163">Para ayudar a mejorar la legibilidad del código, vamos a terminar mediante la creación de una vista parcial: RSVPStatus.ascx – que encapsular todo el código de la vista RSVP para nuestra página de detalles.</span><span class="sxs-lookup"><span data-stu-id="9a617-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="9a617-164">Podemos hacer esto con el botón secundario en la carpeta \Views\Dinners y, a continuación, eligiendo Add -&gt;ver el comando de menú.</span><span class="sxs-lookup"><span data-stu-id="9a617-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="9a617-165">Él tendremos toman un objeto de la cena como el modelo de vista fuertemente tipada.</span><span class="sxs-lookup"><span data-stu-id="9a617-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="9a617-166">Se puede, a continuación, copiar y pegar el contenido RSVP desde nuestra vista Details.aspx en él.</span><span class="sxs-lookup"><span data-stu-id="9a617-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="9a617-167">Una vez hecho eso, vamos a crear también otra vista parcial: EditAndDeleteLinks.ascx - que encapsula nuestra vínculo Editar y eliminar, ver código.</span><span class="sxs-lookup"><span data-stu-id="9a617-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="9a617-168">También tendremos toman un objeto de la cena como el modelo de vista fuertemente tipada y copiar y pegar la lógica de edición y eliminación de nuestra vista Details.aspx en él.</span><span class="sxs-lookup"><span data-stu-id="9a617-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="9a617-169">Nuestros detalles de la plantilla puede de ver a continuación, simplemente incluyen dos llamadas al método Html.RenderPartial() en la parte inferior:</span><span class="sxs-lookup"><span data-stu-id="9a617-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="9a617-170">Esto hace que el código más limpio para leer y mantener.</span><span class="sxs-lookup"><span data-stu-id="9a617-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="9a617-171">Paso siguiente</span><span class="sxs-lookup"><span data-stu-id="9a617-171">Next Step</span></span>

<span data-ttu-id="9a617-172">Ahora veamos cómo podemos usar AJAX aún más lejos y agregar compatibilidad con la asignación interactivo a nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="9a617-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9a617-173">[Anterior](secure-applications-using-authentication-and-authorization.md)
> [Siguiente](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="9a617-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
