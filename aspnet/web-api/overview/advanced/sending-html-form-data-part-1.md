---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Enviar datos de formulario HTML en ASP.NET Web API: datos de formato Form-urlencoded | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/15/2012
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 617b16da20f448bf86e4b99841ad6eeaf8aafe4d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818079"
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a><span data-ttu-id="8fcc0-102">Enviar datos de formulario HTML en ASP.NET Web API: datos de formato Form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="8fcc0-102">Sending HTML Form Data in ASP.NET Web API: Form-urlencoded Data</span></span>
====================
<span data-ttu-id="8fcc0-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8fcc0-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-1-form-urlencoded-data"></a><span data-ttu-id="8fcc0-104">Parte 1: Datos de formato Form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="8fcc0-104">Part 1: Form-urlencoded Data</span></span>

<span data-ttu-id="8fcc0-105">En este artículo se muestra cómo publicar datos de formato form-urlencoded para un controlador Web API.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-105">This article shows how to post form-urlencoded data to a Web API controller.</span></span>

- [<span data-ttu-id="8fcc0-106">Información general de formularios HTML</span><span class="sxs-lookup"><span data-stu-id="8fcc0-106">Overview of HTML Forms</span></span>](#overview_of_html_forms)
- [<span data-ttu-id="8fcc0-107">Envío de tipos complejos</span><span class="sxs-lookup"><span data-stu-id="8fcc0-107">Sending Complex Types</span></span>](#sending_complex_types)
- [<span data-ttu-id="8fcc0-108">Enviar datos de formulario a través de AJAX</span><span class="sxs-lookup"><span data-stu-id="8fcc0-108">Sending Form Data via AJAX</span></span>](#sending_form_data_via_ajax)
- [<span data-ttu-id="8fcc0-109">Envío de tipos simples</span><span class="sxs-lookup"><span data-stu-id="8fcc0-109">Sending Simple Types</span></span>](#sending_simple_types)

> [!NOTE]
> <span data-ttu-id="8fcc0-110">[Descargue el proyecto completado](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span><span class="sxs-lookup"><span data-stu-id="8fcc0-110">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span></span>


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a><span data-ttu-id="8fcc0-111">Información general de formularios HTML</span><span class="sxs-lookup"><span data-stu-id="8fcc0-111">Overview of HTML Forms</span></span>

<span data-ttu-id="8fcc0-112">Uso de formularios HTML ya sea GET o POST para enviar datos al servidor.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-112">HTML forms use either GET or POST to send data to the server.</span></span> <span data-ttu-id="8fcc0-113">El **método** atributo de la **formulario** elemento proporciona el método HTTP:</span><span class="sxs-lookup"><span data-stu-id="8fcc0-113">The **method** attribute of the **form** element gives the HTTP method:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

<span data-ttu-id="8fcc0-114">El método predeterminado es GET.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-114">The default method is GET.</span></span> <span data-ttu-id="8fcc0-115">Si el formulario usa GET, el formulario de datos se codifican en el URI como una cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-115">If the form uses GET, the form data is encoded in the URI as a query string.</span></span> <span data-ttu-id="8fcc0-116">Si el formulario utiliza POST, los datos del formulario se colocan en el cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-116">If the form uses POST, the form data is placed in the request body.</span></span> <span data-ttu-id="8fcc0-117">Para el envío de los datos, el **enctype** atributo especifica el formato del cuerpo de la solicitud:</span><span class="sxs-lookup"><span data-stu-id="8fcc0-117">For POSTed data, the **enctype** attribute specifies the format of the request body:</span></span>

| <span data-ttu-id="8fcc0-118">Enctype</span><span class="sxs-lookup"><span data-stu-id="8fcc0-118">enctype</span></span> | <span data-ttu-id="8fcc0-119">Descripción</span><span class="sxs-lookup"><span data-stu-id="8fcc0-119">Description</span></span> |
| --- | --- |
| <span data-ttu-id="8fcc0-120">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="8fcc0-120">application/x-www-form-urlencoded</span></span> | <span data-ttu-id="8fcc0-121">Datos del formulario se codifican como pares nombre/valor, similares a una cadena de consulta URI.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-121">Form data is encoded as name/value pairs, similar to a URI query string.</span></span> <span data-ttu-id="8fcc0-122">Este es el formato predeterminado de POST.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-122">This is the default format for POST.</span></span> |
| <span data-ttu-id="8fcc0-123">varias partes/datos de formulario</span><span class="sxs-lookup"><span data-stu-id="8fcc0-123">multipart/form-data</span></span> | <span data-ttu-id="8fcc0-124">Datos del formulario se codifican como un mensaje MIME de varias partes.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-124">Form data is encoded as a multipart MIME message.</span></span> <span data-ttu-id="8fcc0-125">Utilice este formato si va a cargar un archivo en el servidor.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-125">Use this format if you are uploading a file to the server.</span></span> |

<span data-ttu-id="8fcc0-126">Parte 1 de este artículo se examina formato x--www-form-urlencoded.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-126">Part 1 of this article looks at x-www-form-urlencoded format.</span></span> <span data-ttu-id="8fcc0-127">[Parte 2](sending-html-form-data-part-2.md) describe varias partes MIME.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-127">[Part 2](sending-html-form-data-part-2.md) describes multipart MIME.</span></span>

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a><span data-ttu-id="8fcc0-128">Envío de tipos complejos</span><span class="sxs-lookup"><span data-stu-id="8fcc0-128">Sending Complex Types</span></span>

<span data-ttu-id="8fcc0-129">Normalmente, se enviará a un tipo complejo, compuesto por valores procedentes de varios controles de formulario.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-129">Typically, you will send a complex type, composed of values taken from several form controls.</span></span> <span data-ttu-id="8fcc0-130">Tenga en cuenta el siguiente modelo que representa una actualización de estado:</span><span class="sxs-lookup"><span data-stu-id="8fcc0-130">Consider the following model that represents a status update:</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

<span data-ttu-id="8fcc0-131">Este es un controlador de API Web que acepta un `Update` objeto a través de POST.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-131">Here is a Web API controller that accepts an `Update` object via POST.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="8fcc0-132">Este controlador usa [enrutamiento basado en acción](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), por lo que es la plantilla de ruta &quot;api / {controller} / {action} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-132">This controller uses [action-based routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), so the route template is &quot;api/{controller}/{action}/{id}&quot;.</span></span> <span data-ttu-id="8fcc0-133">El cliente enviará los datos a &quot;/api/updates/complex&quot;.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-133">The client will post the data to &quot;/api/updates/complex&quot;.</span></span>


<span data-ttu-id="8fcc0-134">Ahora vamos a escribir un formulario HTML para que los usuarios enviar una actualización de estado.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-134">Now let's write an HTML form for users to submit a status update.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

<span data-ttu-id="8fcc0-135">Tenga en cuenta que el **acción** atributo en el formulario es el URI de acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-135">Notice that the **action** attribute on the form is the URI of our controller action.</span></span> <span data-ttu-id="8fcc0-136">Este es el formulario con algunos valores escritos en:</span><span class="sxs-lookup"><span data-stu-id="8fcc0-136">Here is the form with some values entered in:</span></span>

![](sending-html-form-data-part-1/_static/image1.png)

<span data-ttu-id="8fcc0-137">Cuando el usuario hace clic en enviar, el explorador envía una solicitud HTTP similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="8fcc0-137">When the user clicks Submit, the browser sends an HTTP request similar to the following:</span></span>

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

<span data-ttu-id="8fcc0-138">Tenga en cuenta que el cuerpo de solicitud contiene los datos del formulario, con formato de pares nombre/valor.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-138">Notice that the request body contains the form data, formatted as name/value pairs.</span></span> <span data-ttu-id="8fcc0-139">API Web convierte automáticamente los pares nombre/valor en una instancia de la `Update` clase.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-139">Web API automatically converts the name/value pairs into an instance of the `Update` class.</span></span>

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a><span data-ttu-id="8fcc0-140">Enviar datos de formulario a través de AJAX</span><span class="sxs-lookup"><span data-stu-id="8fcc0-140">Sending Form Data via AJAX</span></span>

<span data-ttu-id="8fcc0-141">Cuando un usuario envía un formulario, el explorador se desplaza fuera de la página actual y representa el cuerpo del mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-141">When a user submits a form, the browser navigates away from the current page and renders the body of the response message.</span></span> <span data-ttu-id="8fcc0-142">Eso está bien cuando la respuesta es una página HTML.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-142">That's OK when the response is an HTML page.</span></span> <span data-ttu-id="8fcc0-143">Con una API web, sin embargo, el cuerpo de respuesta es normalmente vacía o contiene datos estructurados, como JSON.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-143">With a web API, however, the response body is usually either empty or contains structured data, such as JSON.</span></span> <span data-ttu-id="8fcc0-144">En ese caso, tiene más sentido para enviar la solicitan de los datos del formulario con AJAX, para que la página pueda procesar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-144">In that case, it makes more sense to send the form data using an AJAX request, so that the page can process the response.</span></span>

<span data-ttu-id="8fcc0-145">El código siguiente muestra cómo publicar datos del formulario mediante jQuery.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-145">The following code shows how to post form data using jQuery.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

<span data-ttu-id="8fcc0-146">JQuery **enviar** función reemplaza la acción de formulario con una nueva función.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-146">The jQuery **submit** function replaces the form action with a new function.</span></span> <span data-ttu-id="8fcc0-147">Esto invalida el comportamiento predeterminado del botón Enviar.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-147">This overrides the default behavior of the Submit button.</span></span> <span data-ttu-id="8fcc0-148">El **serializar** función serializa los datos del formulario en pares nombre/valor.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-148">The **serialize** function serializes the form data into name/value pairs.</span></span> <span data-ttu-id="8fcc0-149">Para enviar los datos del formulario al servidor, llame a `$.post()`.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-149">To send the form data to the server, call `$.post()`.</span></span>

<span data-ttu-id="8fcc0-150">Cuando se completa la solicitud, el `.success()` o `.error()` controlador muestra un mensaje adecuado al usuario.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-150">When the request completes, the `.success()` or `.error()` handler displays an appropriate message to the user.</span></span>

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a><span data-ttu-id="8fcc0-151">Envío de tipos simples</span><span class="sxs-lookup"><span data-stu-id="8fcc0-151">Sending Simple Types</span></span>

<span data-ttu-id="8fcc0-152">En las secciones anteriores, se envía un tipo complejo, que Web API deserializa en una instancia de una clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-152">In the previous sections, we sent a complex type, which Web API deserialized to an instance of a model class.</span></span> <span data-ttu-id="8fcc0-153">También puede enviar tipos simples, como una cadena.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-153">You can also send simple types, such as a string.</span></span>

> [!NOTE]
> <span data-ttu-id="8fcc0-154">Antes de enviar un tipo simple, considere la posibilidad de ajustar el valor en un tipo complejo en su lugar.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-154">Before sending a simple type, consider wrapping the value in a complex type instead.</span></span> <span data-ttu-id="8fcc0-155">Esto le ofrece las ventajas de la validación del modelo en el servidor y hace que sea más fácil de ampliar el modelo si es necesario.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-155">This gives you the benefits of model validation on the server side, and makes it easier to extend your model if needed.</span></span>


<span data-ttu-id="8fcc0-156">Los pasos básicos para enviar un tipo simple son los mismos, pero hay dos diferencias sutiles.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-156">The basic steps to send a simple type are the same, but there are two subtle differences.</span></span> <span data-ttu-id="8fcc0-157">En primer lugar, en el controlador, debe decorar el nombre del parámetro con el **FromBody** atributo.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-157">First, in the controller, you must decorate the parameter name with the **FromBody** attribute.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

<span data-ttu-id="8fcc0-158">De forma predeterminada, API Web intenta obtener los tipos simples desde el URI de solicitud.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-158">By default, Web API tries to get simple types from the request URI.</span></span> <span data-ttu-id="8fcc0-159">El **FromBody** atributo indica a Web API para leer el valor desde el cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-159">The **FromBody** attribute tells Web API to read the value from the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="8fcc0-160">API Web lee el cuerpo de respuesta a lo sumo una vez, por lo que solo un parámetro de una acción puede proceder de cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-160">Web API reads the response body at most once, so only one parameter of an action can come from the request body.</span></span> <span data-ttu-id="8fcc0-161">Si necesita obtener varios valores desde el cuerpo de solicitud, defina un tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-161">If you need to get multiple values from the request body, define a complex type.</span></span>


<span data-ttu-id="8fcc0-162">En segundo lugar, el cliente debe enviar el valor con el formato siguiente:</span><span class="sxs-lookup"><span data-stu-id="8fcc0-162">Second, the client needs to send the value with the following format:</span></span>

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

<span data-ttu-id="8fcc0-163">En concreto, la parte del nombre del par nombre/valor debe estar vacía para un tipo simple.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-163">Specifically, the name portion of the name/value pair must be empty for a simple type.</span></span> <span data-ttu-id="8fcc0-164">No todos los exploradores admiten por los formularios HTML, pero crear este formato de secuencia de comandos como sigue:</span><span class="sxs-lookup"><span data-stu-id="8fcc0-164">Not all browsers support this for HTML forms, but you create this format in script as follows:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

<span data-ttu-id="8fcc0-165">Este es un formulario de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8fcc0-165">Here is an example form:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

<span data-ttu-id="8fcc0-166">Y aquí está la secuencia de comandos para enviar el valor del formulario.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-166">And here is the script to submit the form value.</span></span> <span data-ttu-id="8fcc0-167">La única diferencia de la secuencia de comandos anterior es el argumento pasado a la **registrar** función.</span><span class="sxs-lookup"><span data-stu-id="8fcc0-167">The only difference from the previous script is the argument passed into the **post** function.</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

<span data-ttu-id="8fcc0-168">Puede usar el mismo enfoque para enviar una matriz de tipos simples:</span><span class="sxs-lookup"><span data-stu-id="8fcc0-168">You can use the same approach to send an array of simple types:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a><span data-ttu-id="8fcc0-169">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8fcc0-169">Additional Resources</span></span>

[<span data-ttu-id="8fcc0-170">Parte 2: Carga de archivos y MIME de varias partes</span><span class="sxs-lookup"><span data-stu-id="8fcc0-170">Part 2: File Upload and Multipart MIME</span></span>](sending-html-form-data-part-2.md)
