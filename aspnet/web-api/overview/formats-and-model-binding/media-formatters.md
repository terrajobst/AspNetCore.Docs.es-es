---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Formateadores de contenido multimedia en ASP.NET Web API 2 | Documentos de Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 9103574597df126a22e21a2f51815f608e46f47f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="media-formatters-in-aspnet-web-api-2"></a>Formateadores de contenido multimedia en ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este tutorial muestra cómo admite formatos multimedia adicionales en ASP.NET Web API.

## <a name="internet-media-types"></a>Tipos de medios de Internet

Un tipo de medio, conocido también como un tipo MIME, identifica el formato de un elemento de datos. En HTTP, tipos de medios describen el formato del cuerpo del mensaje. Un tipo de medio consta de dos cadenas, un tipo y un subtipo. Por ejemplo:

- texto/html
- image/png
- application/json

Cuando un mensaje HTTP contiene un cuerpo de entidad, el encabezado Content-Type especifica el formato del cuerpo del mensaje. Esto indica que el receptor a cómo analizar el contenido del cuerpo del mensaje.

Por ejemplo, si una respuesta HTTP contiene una imagen PNG, la respuesta podría tener los siguientes encabezados.

[!code-console[Main](media-formatters/samples/sample1.cmd)]

Cuando el cliente envía un mensaje de solicitud, puede incluir un encabezado de aceptación. El encabezado Accept indica que el servidor de tipos de los archivos multimedia que el cliente espera el servidor. Por ejemplo:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

Este encabezado indica al servidor que desea que el cliente HTML, XHTML o XML.

El tipo de medio determina cómo API Web serializa y deserializa el cuerpo del mensaje HTTP. API Web tiene compatibilidad integrada para XML, JSON, BSON y datos de codificación de formulario, y puede admitir tipos de medios adicionales mediante la escritura de un *formateador del contenido multimedia*.

Para crear a un formateador de medios, que se derivan de una de estas clases:

- [MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx). Esta clase usa la lectura asincrónica y los métodos de escritura.
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). Esta clase se deriva de **elemento MediaTypeFormatter** pero utiliza métodos de lectura/escritura de sychronous.

Derivar de **BufferedMediaTypeFormatter** es más sencillo, porque no hay ningún código asincrónico, pero también significa que puede bloquear el subproceso que realiza la llamada durante la E/S.

## <a name="example-creating-a-csv-media-formatter"></a>Ejemplo: Crear a un formateador de medios CSV

En el ejemplo siguiente se muestra un formateador de tipo de medio que puede serializar un objeto del producto en un formato de valores separados por comas (CSV). Este ejemplo utiliza el tipo de producto definido en el tutorial [crear una API Web que admite las operaciones CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md). Aquí está la definición del objeto de producto:

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

Para implementar un formateador CSV, defina una clase que deriva de **BufferedMediaTypeFormater**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

En el constructor, agregue los tipos de medios que admite el formateador. En este ejemplo, el formateador admite un único tipo de medio, &quot;texto o csv&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

Invalidar el **CanWriteType** método para indicar que escribe el formateador puede serializar:

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

En este ejemplo, el formateador puede serializar solo `Product` objetos así como las colecciones de `Product` objetos.

De forma similar, invalidar la **CanReadType** método para indicar que escribe el formateador puede deserializar. En este ejemplo, el formateador no admite la deserialización, por lo que el método simplemente devuelve **false**.

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

Finalmente, se invalida la **WriteToStream** método. Este método serializa un tipo mediante la escritura en una secuencia. Si el formateador admite la deserialización, invalidar la **ReadFromStream** método.

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Agregar a un formateador del contenido multimedia a la canalización de API Web

Para agregar un tipo de medio formateador para la canalización Web API, use la **formateadores** propiedad en el **HttpConfiguration** objeto.

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>Codificaciones de caracteres

Opcionalmente, un formateador de medios puede admitir varias codificaciones de caracteres, como UTF-8 o ISO 8859-1.

En el constructor, agregue uno o varios [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) tipos a la **SupportedEncodings** colección. Coloque el primero de codificación predeterminado.

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

En el **WriteToStream** y **ReadFromStream** llamar métodos, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) para seleccionar la codificación de caracteres preferida. Este método devuelve los encabezados de solicitud con la lista de codificaciones admitidas. Utilice el valor devuelto **codificación** al leer o escribir en la secuencia:

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
