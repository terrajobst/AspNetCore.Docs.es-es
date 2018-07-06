---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: En ASP.NET Web API la negociación de contenido | Microsoft Docs
author: MikeWasson
description: Describe cómo ASP.NET Web API implementa la negociación de contenido HTTP.
ms.author: aspnetcontent
ms.date: 05/20/2012
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: 2314a263a12c74e80c08391ae03425955a82458a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810322"
---
<a name="content-negotiation-in-aspnet-web-api"></a>Negociación de contenido en ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

En este artículo se describe cómo ASP.NET Web API implementa la negociación de contenido.

La especificación de HTTP (RFC 2616) define la negociación de contenido como "el proceso de seleccionar la mejor representación para una respuesta determinada cuando hay varias representaciones". El mecanismo principal para la negociación de contenido en HTTP son estos encabezados de solicitud:

- **Aceptar:** qué tipos de medios son aceptables para la respuesta, por ejemplo, "application/json", "application/xml" o un tipo de medio personalizado como &quot;application/vnd.example+xml&quot;
- **Accept-Charset:** qué juegos de caracteres son aceptables, como UTF-8 o ISO 8859-1.
- **Codificación aceptada:** qué codificaciones de contenido son aceptables, como gzip.
- **Aceptar-lenguaje:** el idioma preferido natural, como "en-us".

El servidor también puede mirar otras partes de la solicitud HTTP. Por ejemplo, si la solicitud contiene un encabezado X-Requested-With, que indica una solicitud AJAX, el servidor podría predeterminado para JSON si no hay ningún encabezado Accept.

En este artículo, daremos un vistazo a cómo la API Web utiliza los encabezados Accept y Accept-Charset. (En este momento, no hay ninguna compatibilidad integrada para Accept-Language o Accept-Encoding.)

## <a name="serialization"></a>Serialización

Si un controlador de API Web devuelve un recurso como tipo de CLR, la canalización serializa el valor devuelto y lo escribe en el cuerpo de respuesta HTTP.

Por ejemplo, considere la siguiente acción del controlador:

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

Un cliente puede enviar esta solicitud HTTP:

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

En respuesta, el servidor podría enviar:

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

En este ejemplo, el cliente solicitó JSON, Javascript o "cualquier cosa" (\*/\*). El servidor responde con una representación JSON de la `Product` objeto. Tenga en cuenta que el encabezado Content-Type en la respuesta se establece en &quot;application/json&quot;.

También puede devolver un controlador de un **HttpResponseMessage** objeto. Para especificar un objeto CLR para el cuerpo de respuesta, llame a la **CreateResponse** método de extensión:

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

Esta opción le ofrece más control sobre los detalles de la respuesta. Puede establecer el código de estado, agregue los encabezados HTTP y así sucesivamente.

El objeto que serializa el recurso se denomina un *formateador media*. Formateadores de contenido multimedia que se derivan de la **elemento MediaTypeFormatter** clase. API Web proporciona los formateadores de contenido multimedia para XML y JSON, y puede crear formateadores personalizados para admitir otros tipos de medios. Para obtener información sobre cómo escribir un formateador personalizado, vea [formateadores de contenido multimedia](media-formatters.md).

## <a name="how-content-negotiation-works"></a>Contenido cómo funciona la negociación

En primer lugar, la canalización Obtiene el **IContentNegotiator** desde el **HttpConfiguration** objeto. También obtiene la lista de formateadores de contenido multimedia desde el **HttpConfiguration.Formatters** colección.

A continuación, llama la canalización **IContentNegotiatior.Negotiate**, pasando:

- El tipo de objeto que se va a serializar
- La colección de formateadores de contenido multimedia
- La solicitud HTTP

El **Negotiate** método devuelve dos fragmentos de información:

- El formateador que se va a utilizar
- El tipo de medio para la respuesta

Si no se encuentra ningún formateador, el **Negotiate** devuelve del método **null**y el cliente recibe HTTP 406 (no aceptable).

El código siguiente muestra cómo un controlador puede invocar directamente la negociación de contenido:

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

Este código es equivalente a lo que hace automáticamente la canalización.

## <a name="default-content-negotiator"></a>Negociador de contenido predeterminado

El **DefaultContentNegotiator** clase proporciona la implementación predeterminada de **IContentNegotiator**. Usa varios criterios para seleccionar a un formateador.

En primer lugar, el formateador debe ser capaz de serializar el tipo. Esto se comprueba mediante una llamada a **MediaTypeFormatter.CanWriteType**.

A continuación, el negociador de contenido examina cada formateador y evalúa coincide con la solicitud HTTP. Para evaluar a la coincidencia, el negociador de contenido examina dos cosas en el formateador:

- El **SupportedMediaTypes** colección, que contiene una lista de tipos de medios admitidos. El negociador de contenido que se intenta hacer coincidir esta lista en el encabezado Accept de la solicitud. Tenga en cuenta que el encabezado Accept puede incluir intervalos. Por ejemplo, "text/plain" es una coincidencia de texto /\* o \* / \*.
- El **MediaTypeMappings** colección, que contiene una lista de **MediaTypeMapping** objetos. El **MediaTypeMapping** clase proporciona una forma genérica para que coincida con las solicitudes HTTP con tipos de medios. Por ejemplo, podría asignar un encabezado HTTP personalizado para un tipo de medio determinado.

Si hay varios coincide, la coincidencia con el más alto gana factor de calidad. Por ejemplo:

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

En este ejemplo, application/json tiene un factor de calidad implícita de 1.0, por lo que es preferible a la aplicación/xml.

Si se encuentra ninguna coincidencia, el negociador de contenido intenta hacer coincidir en el tipo de medio del cuerpo de la solicitud, si existe. Por ejemplo, si la solicitud contiene datos JSON, busca el negociador de contenido para un formateador JSON.

Si aún no hay ninguna coincidencia, el negociador de contenido simplemente toma al primer formateador que puede serializar el tipo.

## <a name="selecting-a-character-encoding"></a>Seleccionar una codificación de caracteres

Una vez seleccionado un formateador, el negociador de contenido elige la mejor codificación de caracteres examinando el **SupportedEncodings** propiedad en el formateador y hacerla coincidir con el encabezado Accept-Charset en la solicitud (si existe).
