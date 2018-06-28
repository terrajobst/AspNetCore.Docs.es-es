---
title: Formateadores personalizados en ASP.NET Core Web API
author: rick-anderson
description: Obtenga información sobre cómo crear y utilizar formateadores personalizados para las API web de ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.date: 02/08/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: web-api/advanced/custom-formatters
ms.openlocfilehash: ec38a988a73278481de6535c627b2479a9805387
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35217483"
---
# <a name="custom-formatters-in-aspnet-core-web-api"></a><span data-ttu-id="a5415-103">Formateadores personalizados en ASP.NET Core Web API</span><span class="sxs-lookup"><span data-stu-id="a5415-103">Custom formatters in ASP.NET Core Web API</span></span>

<span data-ttu-id="a5415-104">Por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a5415-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="a5415-105">ASP.NET Core MVC ofrece compatibilidad integrada con el intercambio de datos en las API web mediante el uso de formatos de texto sin formato, JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="a5415-105">ASP.NET Core MVC has built-in support for data exchange in web APIs by using JSON, XML, or plain text formats.</span></span> <span data-ttu-id="a5415-106">En este artículo se muestra cómo agregar compatibilidad con formatos adicionales mediante la creación de formateadores personalizados.</span><span class="sxs-lookup"><span data-stu-id="a5415-106">This article shows how to add support for additional formats by creating custom formatters.</span></span>

<span data-ttu-id="a5415-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a5415-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-custom-formatters"></a><span data-ttu-id="a5415-108">Cuándo usar formateadores personalizados</span><span class="sxs-lookup"><span data-stu-id="a5415-108">When to use custom formatters</span></span>

<span data-ttu-id="a5415-109">Utilice un formateador personalizado cuando quiera que el proceso de [negociación de contenido](xref:web-api/advanced/formatting#content-negotiation) admita un tipo de contenido que no es compatible con los formateadores integrados (JSON, XML y texto sin formato).</span><span class="sxs-lookup"><span data-stu-id="a5415-109">Use a custom formatter when you want the [content negotiation](xref:web-api/advanced/formatting#content-negotiation) process to support a content type that isn't supported by the built-in formatters (JSON, XML, and plain text).</span></span>

<span data-ttu-id="a5415-110">Por ejemplo, si algunos de los clientes de la API web pueden controlar el formato [Protobuf](https://github.com/google/protobuf), quizá quiera usar Protobuf con esos clientes porque le resulte más eficaz.</span><span class="sxs-lookup"><span data-stu-id="a5415-110">For example, if some of the clients for your web API can handle the [Protobuf](https://github.com/google/protobuf) format, you might want to use Protobuf with those clients because it's more efficient.</span></span> <span data-ttu-id="a5415-111">O tal vez desee que la API web envíe nombres y direcciones de contacto en formato [vCard](https://wikipedia.org/wiki/VCard), un formato comúnmente utilizado para intercambiar datos de contacto.</span><span class="sxs-lookup"><span data-stu-id="a5415-111">Or you might want your web API to send contact names and addresses in [vCard](https://wikipedia.org/wiki/VCard) format, a commonly used format for exchanging contact data.</span></span> <span data-ttu-id="a5415-112">La aplicación de ejemplo proporcionada con este artículo implementa a un formateador de vCard simple.</span><span class="sxs-lookup"><span data-stu-id="a5415-112">The sample app provided with this article implements a simple vCard formatter.</span></span>

## <a name="overview-of-how-to-use-a-custom-formatter"></a><span data-ttu-id="a5415-113">Información general sobre cómo utilizar a un formateador personalizado</span><span class="sxs-lookup"><span data-stu-id="a5415-113">Overview of how to use a custom formatter</span></span>

<span data-ttu-id="a5415-114">Estos son los pasos para crear y usar a un formateador personalizado:</span><span class="sxs-lookup"><span data-stu-id="a5415-114">Here are the steps to create and use a custom formatter:</span></span>

* <span data-ttu-id="a5415-115">Cree una clase de formateador de salida si quiere serializar los datos que se van a enviar al cliente.</span><span class="sxs-lookup"><span data-stu-id="a5415-115">Create an output formatter class if you want to serialize data to send to the client.</span></span>
* <span data-ttu-id="a5415-116">Cree una clase de formateador de entrada si quiere deserializar los datos recibidos del cliente.</span><span class="sxs-lookup"><span data-stu-id="a5415-116">Create an input formatter class if you want to deserialize data received from the client.</span></span>
* <span data-ttu-id="a5415-117">Agregue instancias de los formateadores a las colecciones `InputFormatters` y `OutputFormatters` en [MvcOptions](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions).</span><span class="sxs-lookup"><span data-stu-id="a5415-117">Add instances of your formatters to the `InputFormatters` and `OutputFormatters` collections in [MvcOptions](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions).</span></span>

<span data-ttu-id="a5415-118">Las secciones siguientes proporcionan instrucciones y ejemplos de código para cada uno de estos pasos.</span><span class="sxs-lookup"><span data-stu-id="a5415-118">The following sections provide guidance and code examples for each of these steps.</span></span>

## <a name="how-to-create-a-custom-formatter-class"></a><span data-ttu-id="a5415-119">Cómo crear una clase de formateador personalizado</span><span class="sxs-lookup"><span data-stu-id="a5415-119">How to create a custom formatter class</span></span>

<span data-ttu-id="a5415-120">Para crear un formateador:</span><span class="sxs-lookup"><span data-stu-id="a5415-120">To create a formatter:</span></span>

* <span data-ttu-id="a5415-121">Derive la clase de la clase base adecuada.</span><span class="sxs-lookup"><span data-stu-id="a5415-121">Derive the class from the appropriate base class.</span></span>
* <span data-ttu-id="a5415-122">Especifique tipos de medios válidos y codificaciones en el constructor.</span><span class="sxs-lookup"><span data-stu-id="a5415-122">Specify valid media types and encodings in the constructor.</span></span>
* <span data-ttu-id="a5415-123">Invalidar métodos `CanReadType`/`CanWriteType`</span><span class="sxs-lookup"><span data-stu-id="a5415-123">Override `CanReadType`/`CanWriteType` methods</span></span>
* <span data-ttu-id="a5415-124">Invalidar métodos `ReadRequestBodyAsync`/`WriteResponseBodyAsync`</span><span class="sxs-lookup"><span data-stu-id="a5415-124">Override `ReadRequestBodyAsync`/`WriteResponseBodyAsync` methods</span></span>
  
### <a name="derive-from-the-appropriate-base-class"></a><span data-ttu-id="a5415-125">Derivar de la clase base adecuada</span><span class="sxs-lookup"><span data-stu-id="a5415-125">Derive from the appropriate base class</span></span>

<span data-ttu-id="a5415-126">Para los tipos de medios de texto (por ejemplo, vCard), se deriva de la clase base [TextInputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) o [TextOutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter).</span><span class="sxs-lookup"><span data-stu-id="a5415-126">For text media types (for example, vCard), derive from the [TextInputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) or [TextOutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) base class.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

<span data-ttu-id="a5415-127">Para los tipos binarios, se deriva de la clase base [InputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.inputformatter) o [OutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformatter).</span><span class="sxs-lookup"><span data-stu-id="a5415-127">For binary types, derive from the [InputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.inputformatter) or [OutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformatter) base class.</span></span>

### <a name="specify-valid-media-types-and-encodings"></a><span data-ttu-id="a5415-128">Especificar las codificaciones y los tipos de medios válidos</span><span class="sxs-lookup"><span data-stu-id="a5415-128">Specify valid media types and encodings</span></span>

<span data-ttu-id="a5415-129">En el constructor, especifique tipos de medios y codificaciones válidos. Para ello, agréguelos a las colecciones `SupportedMediaTypes` y `SupportedEncodings`.</span><span class="sxs-lookup"><span data-stu-id="a5415-129">In the constructor, specify valid media types and encodings by adding to the `SupportedMediaTypes` and `SupportedEncodings` collections.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]
> <span data-ttu-id="a5415-130">No puede realizar la inserción de dependencias de constructor en una clase de formateador.</span><span class="sxs-lookup"><span data-stu-id="a5415-130">You can't do constructor dependency injection in a formatter class.</span></span> <span data-ttu-id="a5415-131">Por ejemplo, no se puede obtener un registrador mediante la adición de un parámetro de registrador al constructor.</span><span class="sxs-lookup"><span data-stu-id="a5415-131">For example, you can't get a logger by adding a logger parameter to the constructor.</span></span> <span data-ttu-id="a5415-132">Para obtener acceso a los servicios, tendrá que usar el objeto de contexto que se pasa a sus métodos.</span><span class="sxs-lookup"><span data-stu-id="a5415-132">To access services, you have to use the context object that gets passed in to your methods.</span></span> <span data-ttu-id="a5415-133">[A continuación](#read-write) se incluye un código de ejemplo que muestra cómo hacerlo.</span><span class="sxs-lookup"><span data-stu-id="a5415-133">A code example [below](#read-write) shows how to do this.</span></span>

### <a name="override-canreadtypecanwritetype"></a><span data-ttu-id="a5415-134">Invalidar CanReadType/CanWriteType</span><span class="sxs-lookup"><span data-stu-id="a5415-134">Override CanReadType/CanWriteType</span></span>

<span data-ttu-id="a5415-135">Invalide los métodos `CanReadType` o `CanWriteType` para especificar el tipo en el que se puede deserializar o desde el que se puede serializar.</span><span class="sxs-lookup"><span data-stu-id="a5415-135">Specify the type you can deserialize into or serialize from by overriding the `CanReadType` or `CanWriteType` methods.</span></span> <span data-ttu-id="a5415-136">Por ejemplo, quizá solo pueda crear texto de vCard desde un tipo `Contact` y viceversa.</span><span class="sxs-lookup"><span data-stu-id="a5415-136">For example, you might only be able to create vCard text from a `Contact` type and vice versa.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a><span data-ttu-id="a5415-137">Método CanWriteResult</span><span class="sxs-lookup"><span data-stu-id="a5415-137">The CanWriteResult method</span></span>

<span data-ttu-id="a5415-138">En algunos casos tendrá que invalidar `CanWriteResult` en lugar de `CanWriteType`.</span><span class="sxs-lookup"><span data-stu-id="a5415-138">In some scenarios you have to override `CanWriteResult` instead of `CanWriteType`.</span></span> <span data-ttu-id="a5415-139">Use `CanWriteResult` si las condiciones siguientes son verdaderas:</span><span class="sxs-lookup"><span data-stu-id="a5415-139">Use `CanWriteResult` if the following conditions are true:</span></span>

* <span data-ttu-id="a5415-140">El método de acción devuelve una clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="a5415-140">Your action method returns a model class.</span></span>
* <span data-ttu-id="a5415-141">Hay clases derivadas que pueden obtenerse en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="a5415-141">There are derived classes which might be returned at runtime.</span></span>
* <span data-ttu-id="a5415-142">Debe conocer en tiempo de ejecución qué clase derivada fue devuelta por la acción.</span><span class="sxs-lookup"><span data-stu-id="a5415-142">You need to know at runtime which derived class was returned by the action.</span></span>

<span data-ttu-id="a5415-143">Por ejemplo, suponga que la firma del método de acción devuelve un tipo `Person`, pero puede devolver un tipo `Student` o `Instructor` que deriva de `Person`.</span><span class="sxs-lookup"><span data-stu-id="a5415-143">For example, suppose your action method signature returns a `Person` type, but it may return a `Student` or `Instructor` type that derives from `Person`.</span></span> <span data-ttu-id="a5415-144">Si quiere que el formateador únicamente controle objetos `Student`, compruebe el tipo del [objeto](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) en el objeto de contexto proporcionado para el método `CanWriteResult`.</span><span class="sxs-lookup"><span data-stu-id="a5415-144">If you want your formatter to handle only `Student` objects, check the type of [Object](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) in the context object provided to the `CanWriteResult` method.</span></span> <span data-ttu-id="a5415-145">Tenga en cuenta que no es necesario utilizar `CanWriteResult` cuando se devuelve el método de acción `IActionResult`; en ese caso, el método `CanWriteType` recibe el tipo en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="a5415-145">Note that it's not necessary to use `CanWriteResult` when the action method returns `IActionResult`; in that case, the `CanWriteType` method receives the runtime type.</span></span>

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a><span data-ttu-id="a5415-146">Invalidar ReadRequestBodyAsync/WriteResponseBodyAsync</span><span class="sxs-lookup"><span data-stu-id="a5415-146">Override ReadRequestBodyAsync/WriteResponseBodyAsync</span></span>

<span data-ttu-id="a5415-147">Usted se encarga de hacer el trabajo real de deserializar o serializar en `ReadRequestBodyAsync` o `WriteResponseBodyAsync`.</span><span class="sxs-lookup"><span data-stu-id="a5415-147">You do the actual work of deserializing or serializing in `ReadRequestBodyAsync` or `WriteResponseBodyAsync`.</span></span> <span data-ttu-id="a5415-148">Las líneas resaltadas en el ejemplo siguiente muestran cómo obtener los servicios desde el contenedor de inserción de dependencias (no es posible obtenerlos desde a partir de los parámetros del constructor).</span><span class="sxs-lookup"><span data-stu-id="a5415-148">The highlighted lines in the following example show how to get services from the dependency injection container (you can't get them from constructor parameters).</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a><span data-ttu-id="a5415-149">Cómo configurar MVC para usar a un formateador personalizado</span><span class="sxs-lookup"><span data-stu-id="a5415-149">How to configure MVC to use a custom formatter</span></span>

<span data-ttu-id="a5415-150">Para utilizar un formateador personalizado, debe agregar una instancia de la clase de formateador a la colección `InputFormatters` o `OutputFormatters`.</span><span class="sxs-lookup"><span data-stu-id="a5415-150">To use a custom formatter, add an instance of the formatter class to the `InputFormatters` or `OutputFormatters` collection.</span></span>

[!code-csharp[](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

<span data-ttu-id="a5415-151">Los formateadores se evalúan en el orden en que se insertaron.</span><span class="sxs-lookup"><span data-stu-id="a5415-151">Formatters are evaluated in the order you insert them.</span></span> <span data-ttu-id="a5415-152">El primero de ellos tiene prioridad.</span><span class="sxs-lookup"><span data-stu-id="a5415-152">The first one takes precedence.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a5415-153">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="a5415-153">Next steps</span></span>

<span data-ttu-id="a5415-154">Consulte la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample), en la que se implementan formateadores de entrada y salida de vCard.</span><span class="sxs-lookup"><span data-stu-id="a5415-154">See the [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample), which implements simple vCard input and output formatters.</span></span> <span data-ttu-id="a5415-155">La aplicación lee y escribe tarjetas vCard que tienen un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="a5415-155">The application reads and writes vCards that look like the following example:</span></span>

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

<span data-ttu-id="a5415-156">Para ver la salida de vCard, ejecute la aplicación y envíe una solicitud Get con encabezado Accept "texto/vcard" a `http://localhost:63313/api/contacts/` (cuando se ejecuta desde Visual Studio) o `http://localhost:5000/api/contacts/` (cuando se ejecuta desde la línea de comandos).</span><span class="sxs-lookup"><span data-stu-id="a5415-156">To see vCard output, run the application and send a Get request with Accept header "text/vcard" to `http://localhost:63313/api/contacts/` (when running from Visual Studio) or `http://localhost:5000/api/contacts/` (when running from the command line).</span></span>

<span data-ttu-id="a5415-157">Para agregar una tarjeta vCard a la colección en memoria de contactos, envíe una solicitud Post a la misma dirección URL, con el encabezado contenido-tipo "texto/vcard" y con texto de vCard en el cuerpo. El formato sería similar al ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="a5415-157">To add a vCard to the in-memory collection of contacts, send a Post request to the same URL, with Content-Type header "text/vcard" and with vCard text in the body, formatted like the example above.</span></span>
