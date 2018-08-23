---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: Serialización de XML en ASP.NET Web API y JSON | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 05/30/2012
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: 47967e6e1dd0e84b6059c07d7544c0e755fdf510
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836405"
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="f2963-102">Serialización JSON y XML en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="f2963-102">JSON and XML Serialization in ASP.NET Web API</span></span>
====================
<span data-ttu-id="f2963-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f2963-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f2963-104">En este artículo se describe a los formateadores JSON y XML en ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="f2963-104">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="f2963-105">En ASP.NET Web API, un *formateador de tipo de medio* es un objeto que puede:</span><span class="sxs-lookup"><span data-stu-id="f2963-105">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="f2963-106">Cuerpo del mensaje de los objetos de CLR de lectura de HTTP</span><span class="sxs-lookup"><span data-stu-id="f2963-106">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="f2963-107">Escribir objetos CLR en un cuerpo de mensaje HTTP</span><span class="sxs-lookup"><span data-stu-id="f2963-107">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="f2963-108">API Web proporciona a los formateadores de tipo de medio para JSON y XML.</span><span class="sxs-lookup"><span data-stu-id="f2963-108">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="f2963-109">El marco de trabajo inserta a estos formateadores en la canalización de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="f2963-109">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="f2963-110">Los clientes pueden solicitar JSON o XML en el encabezado Accept de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="f2963-110">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="f2963-111">Contenido</span><span class="sxs-lookup"><span data-stu-id="f2963-111">Contents</span></span>

- [<span data-ttu-id="f2963-112">Formateador de tipo de medio JSON</span><span class="sxs-lookup"><span data-stu-id="f2963-112">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="f2963-113">Propiedades de solo lectura</span><span class="sxs-lookup"><span data-stu-id="f2963-113">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="f2963-114">Fechas</span><span class="sxs-lookup"><span data-stu-id="f2963-114">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="f2963-115">Aplicar sangría</span><span class="sxs-lookup"><span data-stu-id="f2963-115">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="f2963-116">Uso de mayúsculas y minúsculas</span><span class="sxs-lookup"><span data-stu-id="f2963-116">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="f2963-117">Objetos anónimos y débilmente tipada</span><span class="sxs-lookup"><span data-stu-id="f2963-117">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="f2963-118">Formateador de tipo de medio XML</span><span class="sxs-lookup"><span data-stu-id="f2963-118">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="f2963-119">Propiedades de solo lectura</span><span class="sxs-lookup"><span data-stu-id="f2963-119">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="f2963-120">Fechas</span><span class="sxs-lookup"><span data-stu-id="f2963-120">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="f2963-121">Aplicar sangría</span><span class="sxs-lookup"><span data-stu-id="f2963-121">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="f2963-122">Serializadores XML de configuración por tipo</span><span class="sxs-lookup"><span data-stu-id="f2963-122">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="f2963-123">Quitar el JSON o un formateador XML</span><span class="sxs-lookup"><span data-stu-id="f2963-123">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="f2963-124">Control de referencias circulares a objetos</span><span class="sxs-lookup"><span data-stu-id="f2963-124">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="f2963-125">Serialización de objetos de prueba</span><span class="sxs-lookup"><span data-stu-id="f2963-125">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="f2963-126">Formateador de tipo de medio JSON</span><span class="sxs-lookup"><span data-stu-id="f2963-126">JSON Media-Type Formatter</span></span>

<span data-ttu-id="f2963-127">Formato JSON proporcionado por el **JsonMediaTypeFormatter** clase.</span><span class="sxs-lookup"><span data-stu-id="f2963-127">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="f2963-128">De forma predeterminada, **JsonMediaTypeFormatter** usa el [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) biblioteca para realizar la serialización.</span><span class="sxs-lookup"><span data-stu-id="f2963-128">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="f2963-129">Json.NET es un proyecto de código abierto de terceros.</span><span class="sxs-lookup"><span data-stu-id="f2963-129">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="f2963-130">Si lo prefiere, puede configurar el **JsonMediaTypeFormatter** clase se debe utilizar el **DataContractJsonSerializer** en lugar de Json.NET.</span><span class="sxs-lookup"><span data-stu-id="f2963-130">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="f2963-131">Para ello, establezca el **UseDataContractJsonSerializer** propiedad **true**:</span><span class="sxs-lookup"><span data-stu-id="f2963-131">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="f2963-132">Serialización de JSON</span><span class="sxs-lookup"><span data-stu-id="f2963-132">JSON Serialization</span></span>

<span data-ttu-id="f2963-133">Esta sección describen algunos comportamientos específicos del formateador JSON, con el valor predeterminado [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializador.</span><span class="sxs-lookup"><span data-stu-id="f2963-133">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="f2963-134">Esto no pretende ser una documentación completa de la biblioteca Json.NET; Para obtener más información, consulte el [documentación Json.NET](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="f2963-134">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="f2963-135">¿Lo que se serializa?</span><span class="sxs-lookup"><span data-stu-id="f2963-135">What Gets Serialized?</span></span>

<span data-ttu-id="f2963-136">De forma predeterminada, todas las propiedades públicas y campos se incluyen en el JSON serializado.</span><span class="sxs-lookup"><span data-stu-id="f2963-136">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="f2963-137">Para omitir un campo o propiedad, decórela con el **JsonIgnore** atributo.</span><span class="sxs-lookup"><span data-stu-id="f2963-137">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="f2963-138">Si prefiere un &quot;participación&quot; enfocar, decore la clase con el **DataContract** atributo.</span><span class="sxs-lookup"><span data-stu-id="f2963-138">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="f2963-139">Si este atributo está presente, se omiten los miembros a menos que tengan la **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="f2963-139">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="f2963-140">También puede usar **DataMember** para serializar los miembros privados.</span><span class="sxs-lookup"><span data-stu-id="f2963-140">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="f2963-141">Propiedades de solo lectura</span><span class="sxs-lookup"><span data-stu-id="f2963-141">Read-Only Properties</span></span>

<span data-ttu-id="f2963-142">Propiedades de solo lectura se serializan de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="f2963-142">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="f2963-143">Fechas</span><span class="sxs-lookup"><span data-stu-id="f2963-143">Dates</span></span>

<span data-ttu-id="f2963-144">De forma predeterminada, Json.NET escribe las fechas [ISO 8601](http://www.w3.org/TR/NOTE-datetime) formato.</span><span class="sxs-lookup"><span data-stu-id="f2963-144">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="f2963-145">Las fechas en formato UTC (Coordinated Universal Time) se escriben con un sufijo "Z".</span><span class="sxs-lookup"><span data-stu-id="f2963-145">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="f2963-146">Las fechas en la hora local incluyen un desplazamiento de zona horaria.</span><span class="sxs-lookup"><span data-stu-id="f2963-146">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="f2963-147">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f2963-147">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="f2963-148">De forma predeterminada, Json.NET conserva la zona horaria.</span><span class="sxs-lookup"><span data-stu-id="f2963-148">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="f2963-149">Puede invalidar esto estableciendo la propiedad DateTimeZoneHandling:</span><span class="sxs-lookup"><span data-stu-id="f2963-149">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="f2963-150">Si prefiere usar [formato de fecha JSON Microsoft](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) en lugar de ISO 8601, establezca el **DateFormatHandling** propiedad en la configuración del serializador:</span><span class="sxs-lookup"><span data-stu-id="f2963-150">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="f2963-151">Aplicar sangría</span><span class="sxs-lookup"><span data-stu-id="f2963-151">Indenting</span></span>

<span data-ttu-id="f2963-152">Para escribir JSON con sangría, establezca el **formato** si se establece en **Formatting.Indented**:</span><span class="sxs-lookup"><span data-stu-id="f2963-152">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="f2963-153">Uso de mayúsculas y minúsculas</span><span class="sxs-lookup"><span data-stu-id="f2963-153">Camel Casing</span></span>

<span data-ttu-id="f2963-154">Para escribir los nombres de propiedad JSON con el uso de mayúsculas y minúsculas, sin cambiar el modelo de datos, establezca el **CamelCasePropertyNamesContractResolver** en el serializador:</span><span class="sxs-lookup"><span data-stu-id="f2963-154">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="f2963-155">Objetos anónimos y débilmente tipada</span><span class="sxs-lookup"><span data-stu-id="f2963-155">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="f2963-156">Un método de acción puede devolver un objeto anónimo y serializarlo en JSON.</span><span class="sxs-lookup"><span data-stu-id="f2963-156">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="f2963-157">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f2963-157">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="f2963-158">El cuerpo del mensaje de respuesta contendrá el JSON siguiente:</span><span class="sxs-lookup"><span data-stu-id="f2963-158">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="f2963-159">Si su web API recibe imprecisa estructurados objetos JSON de los clientes, puede deserializar el cuerpo de solicitud a un **Newtonsoft.Json.Linq.JObject** tipo.</span><span class="sxs-lookup"><span data-stu-id="f2963-159">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="f2963-160">Sin embargo, es mejor usar objetos de datos fuertemente tipados.</span><span class="sxs-lookup"><span data-stu-id="f2963-160">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="f2963-161">A continuación, no es necesario analizar los datos usted mismo y obtener las ventajas de la validación del modelo.</span><span class="sxs-lookup"><span data-stu-id="f2963-161">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="f2963-162">El serializador XML no admite tipos anónimos o **JObject** instancias.</span><span class="sxs-lookup"><span data-stu-id="f2963-162">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="f2963-163">Si utiliza estas características para los datos JSON, debe quitar al formateador XML de la canalización, como se describe más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="f2963-163">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="f2963-164">Formateador de tipo de medio XML</span><span class="sxs-lookup"><span data-stu-id="f2963-164">XML Media-Type Formatter</span></span>

<span data-ttu-id="f2963-165">Aplicación de formato XML proporcionada por el **XmlMediaTypeFormatter** clase.</span><span class="sxs-lookup"><span data-stu-id="f2963-165">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="f2963-166">De forma predeterminada, **XmlMediaTypeFormatter** usa el **DataContractSerializer** clase para realizar la serialización.</span><span class="sxs-lookup"><span data-stu-id="f2963-166">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="f2963-167">Si lo prefiere, puede configurar el **XmlMediaTypeFormatter** para usar el **XmlSerializer** en lugar de la **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="f2963-167">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="f2963-168">Para ello, establezca el **/usexmlserializer** propiedad **true**:</span><span class="sxs-lookup"><span data-stu-id="f2963-168">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="f2963-169">El **XmlSerializer** clase admite un conjunto más estrecho de tipos que **DataContractSerializer**, pero proporciona mayor control sobre el XML resultante.</span><span class="sxs-lookup"><span data-stu-id="f2963-169">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="f2963-170">Considere el uso de **XmlSerializer** si tiene que coincidir con un esquema XML existente.</span><span class="sxs-lookup"><span data-stu-id="f2963-170">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="f2963-171">Serialización XML</span><span class="sxs-lookup"><span data-stu-id="f2963-171">XML Serialization</span></span>

<span data-ttu-id="f2963-172">Esta sección describen algunos comportamientos específicos del formateador XML, utilizando el valor predeterminado **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="f2963-172">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="f2963-173">De forma predeterminada, DataContractSerializer se comporta como sigue:</span><span class="sxs-lookup"><span data-stu-id="f2963-173">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="f2963-174">Se serializan todos los campos y propiedades públicas de lectura/escritura.</span><span class="sxs-lookup"><span data-stu-id="f2963-174">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="f2963-175">Para omitir un campo o propiedad, decórela con el **IgnoreDataMember** atributo.</span><span class="sxs-lookup"><span data-stu-id="f2963-175">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="f2963-176">No se serializan los miembros privados y protegidos.</span><span class="sxs-lookup"><span data-stu-id="f2963-176">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="f2963-177">No se serializan las propiedades de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="f2963-177">Read-only properties are not serialized.</span></span> <span data-ttu-id="f2963-178">(No obstante, se serializa el contenido de una propiedad de colección de solo lectura).</span><span class="sxs-lookup"><span data-stu-id="f2963-178">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="f2963-179">Nombres de clase y miembro se escriben en el XML exactamente como aparecen en la declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="f2963-179">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="f2963-180">Se utiliza el espacio de nombres XML predeterminado.</span><span class="sxs-lookup"><span data-stu-id="f2963-180">A default XML namespace is used.</span></span>

<span data-ttu-id="f2963-181">Si necesita más control sobre la serialización, puede decorar la clase con el **DataContract** atributo.</span><span class="sxs-lookup"><span data-stu-id="f2963-181">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="f2963-182">Cuando este atributo está presente, la clase se serializa como sigue:</span><span class="sxs-lookup"><span data-stu-id="f2963-182">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="f2963-183">&quot;Participar en&quot; enfoque: no se serializan las propiedades y campos de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="f2963-183">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="f2963-184">Para serializar una propiedad o campo, decórela con el **DataMember** atributo.</span><span class="sxs-lookup"><span data-stu-id="f2963-184">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="f2963-185">Para serializar un miembro privado o protegido, decórela con el **DataMember** atributo.</span><span class="sxs-lookup"><span data-stu-id="f2963-185">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="f2963-186">No se serializan las propiedades de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="f2963-186">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="f2963-187">Para cambiar cómo aparece el nombre de clase en el archivo XML, establezca el *nombre* parámetro en el **DataContract** atributo.</span><span class="sxs-lookup"><span data-stu-id="f2963-187">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="f2963-188">Para cambiar la apariencia de un nombre de miembro en el archivo XML, establezca el *nombre* parámetro en el **DataMember** atributo.</span><span class="sxs-lookup"><span data-stu-id="f2963-188">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="f2963-189">Para cambiar el espacio de nombres XML, establezca el *Namespace* parámetro en el **DataContract** clase.</span><span class="sxs-lookup"><span data-stu-id="f2963-189">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="f2963-190">Propiedades de solo lectura</span><span class="sxs-lookup"><span data-stu-id="f2963-190">Read-Only Properties</span></span>

<span data-ttu-id="f2963-191">No se serializan las propiedades de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="f2963-191">Read-only properties are not serialized.</span></span> <span data-ttu-id="f2963-192">Si una propiedad de solo lectura tiene un campo de respaldo privado, puede marcar el campo privado con la **DataMember** atributo.</span><span class="sxs-lookup"><span data-stu-id="f2963-192">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="f2963-193">Este enfoque requiere el **DataContract** atributo de la clase.</span><span class="sxs-lookup"><span data-stu-id="f2963-193">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="f2963-194">Fechas</span><span class="sxs-lookup"><span data-stu-id="f2963-194">Dates</span></span>

<span data-ttu-id="f2963-195">Las fechas se escriben en formato ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="f2963-195">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="f2963-196">Por ejemplo, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="f2963-196">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="f2963-197">Aplicar sangría</span><span class="sxs-lookup"><span data-stu-id="f2963-197">Indenting</span></span>

<span data-ttu-id="f2963-198">Para escribir XML con sangría, establezca el **sangría** propiedad **true**:</span><span class="sxs-lookup"><span data-stu-id="f2963-198">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="f2963-199">Serializadores XML de configuración por tipo</span><span class="sxs-lookup"><span data-stu-id="f2963-199">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="f2963-200">Puede establecer los serializadores XML diferentes para distintos tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="f2963-200">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="f2963-201">Por ejemplo, podría tener un objeto de datos concreto requiere **XmlSerializer** para compatibilidad con versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="f2963-201">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="f2963-202">Puede usar **XmlSerializer** para este objeto y seguir usando **DataContractSerializer** para otros tipos.</span><span class="sxs-lookup"><span data-stu-id="f2963-202">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="f2963-203">Para establecer un serializador XML para un tipo determinado, llame a **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="f2963-203">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="f2963-204">Puede especificar un **XmlSerializer** o cualquier objeto que se deriva de **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="f2963-204">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="f2963-205">Quitar el JSON o un formateador XML</span><span class="sxs-lookup"><span data-stu-id="f2963-205">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="f2963-206">Puede quitar el formateador JSON o el formateador XML en la lista de formateadores, si no desea usarlos.</span><span class="sxs-lookup"><span data-stu-id="f2963-206">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="f2963-207">Las razones principales para hacerlo son:</span><span class="sxs-lookup"><span data-stu-id="f2963-207">The main reasons to do this are:</span></span>

- <span data-ttu-id="f2963-208">Para restringir las respuestas de API web a un tipo de medio determinado.</span><span class="sxs-lookup"><span data-stu-id="f2963-208">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="f2963-209">Por ejemplo, podría decidir admitir sólo las respuestas JSON y quitar al formateador XML.</span><span class="sxs-lookup"><span data-stu-id="f2963-209">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="f2963-210">Para reemplazar al formateador predeterminado con un formateador personalizado.</span><span class="sxs-lookup"><span data-stu-id="f2963-210">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="f2963-211">Por ejemplo, podría reemplazar al formateador JSON con su propia implementación personalizada de un formateador JSON.</span><span class="sxs-lookup"><span data-stu-id="f2963-211">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="f2963-212">El código siguiente muestra cómo quitar a los formateadores de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="f2963-212">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="f2963-213">Llamar a este método desde su **aplicación\_iniciar** método, definido en Global.asax.</span><span class="sxs-lookup"><span data-stu-id="f2963-213">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="f2963-214">Control de referencias circulares a objetos</span><span class="sxs-lookup"><span data-stu-id="f2963-214">Handling Circular Object References</span></span>

<span data-ttu-id="f2963-215">De forma predeterminada, los formateadores JSON y XML escriben todos los objetos como valores.</span><span class="sxs-lookup"><span data-stu-id="f2963-215">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="f2963-216">Si dos propiedades que hacen referencia al mismo objeto, o si el mismo objeto aparece dos veces en una colección, el formateador serializará el objeto dos veces.</span><span class="sxs-lookup"><span data-stu-id="f2963-216">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="f2963-217">Esto es un problema determinado si el gráfico de objetos contiene ciclos, porque el serializador producirá una excepción cuando detecta un bucle en el gráfico.</span><span class="sxs-lookup"><span data-stu-id="f2963-217">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="f2963-218">Tenga en cuenta los siguientes modelos de objetos y el controlador.</span><span class="sxs-lookup"><span data-stu-id="f2963-218">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="f2963-219">Invocar esta acción hará que al formateador que se produce una excepción, que se traduce en un estado código 500 (Error interno del servidor) de respuesta al cliente.</span><span class="sxs-lookup"><span data-stu-id="f2963-219">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="f2963-220">Para conservar las referencias de objeto en JSON, agregue el código siguiente al **aplicación\_iniciar** método en el archivo Global.asax:</span><span class="sxs-lookup"><span data-stu-id="f2963-220">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="f2963-221">Ahora, la acción de controlador devolverá JSON que tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="f2963-221">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="f2963-222">Tenga en cuenta que el serializador agrega un &quot;$id&quot; propiedad a ambos objetos.</span><span class="sxs-lookup"><span data-stu-id="f2963-222">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="f2963-223">Además, detecta que la propiedad Employee.Department crea un bucle, por lo que reemplaza el valor con una referencia de objeto: {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="f2963-223">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="f2963-224">Las referencias a objetos no son estándar en JSON.</span><span class="sxs-lookup"><span data-stu-id="f2963-224">Object references are not standard in JSON.</span></span> <span data-ttu-id="f2963-225">Antes de usar esta característica, considere si los clientes podrán analizar los resultados.</span><span class="sxs-lookup"><span data-stu-id="f2963-225">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="f2963-226">Podría ser preferible simplemente quitar ciclos del gráfico.</span><span class="sxs-lookup"><span data-stu-id="f2963-226">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="f2963-227">Por ejemplo, el vínculo desde el empleado al departamento no es necesario realmente en este ejemplo.</span><span class="sxs-lookup"><span data-stu-id="f2963-227">For example, the link from Employee back to Department is not really needed in this example.</span></span>


<span data-ttu-id="f2963-228">Para conservar las referencias de objeto en XML, tiene dos opciones.</span><span class="sxs-lookup"><span data-stu-id="f2963-228">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="f2963-229">La opción más sencilla consiste en agregar `[DataContract(IsReference=true)]` a la clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="f2963-229">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="f2963-230">El *IsReference* parámetro permite referencias a objetos.</span><span class="sxs-lookup"><span data-stu-id="f2963-230">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="f2963-231">Recuerde que **DataContract** hace serialización participar en, por lo que también deberá agregar **DataMember** atributos a las propiedades:</span><span class="sxs-lookup"><span data-stu-id="f2963-231">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="f2963-232">Ahora el formateador generará XML similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="f2963-232">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="f2963-233">Si desea evitar atributos en la clase de modelo, hay otra opción: crear un nuevo específicos del tipo **DataContractSerializer** de instancia y establecer *preserveObjectReferences* a **true**  en el constructor.</span><span class="sxs-lookup"><span data-stu-id="f2963-233">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="f2963-234">A continuación, establezca esta instancia como un serializador por tipo en el formateador de tipo de medio XML.</span><span class="sxs-lookup"><span data-stu-id="f2963-234">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="f2963-235">El código siguiente se muestra cómo hacerlo:</span><span class="sxs-lookup"><span data-stu-id="f2963-235">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="f2963-236">Serialización de objetos de prueba</span><span class="sxs-lookup"><span data-stu-id="f2963-236">Testing Object Serialization</span></span>

<span data-ttu-id="f2963-237">Al diseñar la API web, es útil probar cómo se va a serializar los objetos de datos.</span><span class="sxs-lookup"><span data-stu-id="f2963-237">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="f2963-238">Puede hacerlo sin crear un controlador o invocar una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="f2963-238">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
