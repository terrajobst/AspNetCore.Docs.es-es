---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: Negociación en ASP.NET Web API de contenido | Documentos de Microsoft
author: MikeWasson
description: Describe cómo ASP.NET Web API implementa negociación de contenido HTTP.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/20/2012
ms.topic: article
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: ca373af6754e82889dc100b63f73b76aaa4e4f27
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "26507024"
---
<a name="content-negotiation-in-aspnet-web-api"></a>Negociación de contenido en ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este artículo describe cómo ASP.NET Web API implementa negociación de contenido.

La especificación de HTTP (RFC 2616) define la negociación de contenido como "el proceso de selección de la mejor representación una respuesta determinada cuando existen varias representaciones". El mecanismo principal para la negociación de contenido en HTTP son estos encabezados de solicitud:

- **Aceptar:** qué tipos de medios son aceptables para la respuesta, como "application/json", "application/xml" o un tipo de medio personalizado como &quot;application/vnd.example+xml&quot;
- **Accept-Charset:** los juegos de caracteres son aceptables, como UTF-8 o ISO 8859-1.
- **Codificación aceptada:** las codificaciones de contenido son aceptables, por ejemplo, gzip.
- **Accept-Language:** el idioma preferido de natural, como "en-us".

El servidor también puede buscar en otras partes de la solicitud HTTP. Por ejemplo, si la solicitud contiene un encabezado X-Requested-With, que indica una solicitud AJAX, el servidor podría predeterminado para JSON si no hay ningún encabezado Accept.

En este artículo, analizaremos cómo API Web utiliza los encabezados de aceptación y Accept-Charset. (En este momento, no hay ninguna compatibilidad integrada para Accept-Encoding o Accept-Language.)

## <a name="serialization"></a>Serialización

Si un controlador de Web API devuelve un recurso como tipo de CLR, la canalización, serializa el valor devuelto y lo escribe en el cuerpo de respuesta HTTP.

Por ejemplo, considere la siguiente acción del controlador:

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

Un cliente puede enviar esta solicitud HTTP:

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

En respuesta, el servidor puede enviar:

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

En este ejemplo, el cliente solicitó JSON, Javascript o "cualquier" (\*/\*). El servidor respondió con una representación JSON de la `Product` objeto. Tenga en cuenta que el encabezado Content-Type de la respuesta se establece en &quot;application/json&quot;.

También puede devolver un controlador de un **HttpResponseMessage** objeto. Para especificar un objeto CLR para el cuerpo de respuesta, llame a la **CreateResponse** método de extensión:

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

Esta opción proporciona mayor control sobre los detalles de la respuesta. Puede establecer el código de estado, agregar encabezados HTTP y así sucesivamente.

El objeto que serializa el recurso se denomina un *formateador del contenido multimedia*. Formateadores de contenido multimedia que se derivan de la **elemento MediaTypeFormatter** clase. API Web proporciona a formateadores de contenido multimedia para XML y JSON, y puede crear formateadores personalizados para admitir otros tipos de medios. Para obtener información sobre cómo escribir un formateador personalizado, consulte [formateadores de contenido multimedia](media-formatters.md).

## <a name="how-content-negotiation-works"></a>Administración de contenido funciona de negociación

En primer lugar, la canalización Obtiene la **IContentNegotiator** desde el **HttpConfiguration** objeto. También obtiene la lista de formateadores de contenido multimedia desde el **HttpConfiguration.Formatters** colección.

A continuación, llama a la canalización **IContentNegotiatior.Negotiate**, pasando:

- El tipo de objeto que se va a serializar
- La colección de formateadores de contenido multimedia
- La solicitud HTTP

El **Negotiate** método devuelve dos piezas de información:

- Formateador que va a utilizar
- El tipo de medio de la respuesta

Si no se encuentra ningún formateador, la **Negotiate** método **null**y el error de cliente recibe HTTP 406 (no aceptable).

El código siguiente muestra cómo un controlador puede invocar directamente negociación de contenido:

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

Este código es equivalente a lo que el que la canalización realiza de forma automática.

## <a name="default-content-negotiator"></a>Negociador de contenido predeterminado

El **DefaultContentNegotiator** clase proporciona la implementación predeterminada de **IContentNegotiator**. Utiliza varios criterios para seleccionar a un formateador.

En primer lugar, el formateador debe ser capaz de serializar el tipo. Esto se comprueba mediante una llamada a **MediaTypeFormatter.CanWriteType**.

A continuación, el negociador de contenido examina cada formateador y da como resultado coincide con la solicitud HTTP. Para evaluar la búsqueda de coincidencias, el negociador de contenido examina dos cosas en el formateador:

- El **SupportedMediaTypes** colección, que contiene una lista de tipos de medios admitidos. El negociador de contenido intenta hacer coincidir esta lista en el encabezado de aceptación de solicitud. Tenga en cuenta que el encabezado Accept puede incluir intervalos. Por ejemplo, "text/plain" es una coincidencia para el texto /\* o \* / \*.
- El **MediaTypeMappings** colección, que contiene una lista de **MediaTypeMapping** objetos. El **MediaTypeMapping** clase proporciona una forma genérica para que coincida con las solicitudes HTTP con tipos de medios. Por ejemplo, puede asignar un encabezado HTTP personalizado para un tipo de medio determinado.

Si hay varias coincide, la coincidencia con el más alto gana factor de calidad. Por ejemplo:

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

En este ejemplo, application/json tiene un factor de calidad implícita de 1,0, por lo que es preferible a través de la aplicación/xml.

Si se encuentra ninguna coincidencia, el negociador de contenido intenta hacer coincidir en el tipo de medio del cuerpo de la solicitud, si lo hay. Por ejemplo, si la solicitud contiene datos JSON, examina el negociador de contenido para un formateador JSON.

Si todavía no hay ninguna coincidencia, el negociador de contenido simplemente toma al primer formateador que puede serializar el tipo.

## <a name="selecting-a-character-encoding"></a>Al seleccionar una codificación de caracteres

Una vez que se selecciona un formateador, el negociador de contenido elige la mejor codificación de caracteres examinando el **SupportedEncodings** propiedad en el formateador y hacerla coincidir con el encabezado Accept-Charset en la solicitud (si existe).
