---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Formateadores de contenido multimedia en ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 01/20/2014
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 9776adee19a62be4be38a4fb963c4978da3ed912
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803290"
---
<a name="media-formatters-in-aspnet-web-api-2"></a>Formateadores de contenido multimedia en ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este tutorial muestra cómo admitir formatos multimedia adicionales en ASP.NET Web API.

## <a name="internet-media-types"></a>Tipos de medios de Internet

Un tipo de medio, también denominado tipo MIME, identifica el formato de un elemento de datos. Tipos de medios en HTTP, describen el formato del cuerpo del mensaje. Un tipo de medio consta de dos cadenas, un tipo y un subtipo. Por ejemplo:

- texto/html
- image/png
- application/json

Cuando un mensaje HTTP contiene un cuerpo de entidad, el encabezado Content-Type especifica el formato del cuerpo del mensaje. Esto indica que el receptor a cómo analizar el contenido del cuerpo del mensaje.

Por ejemplo, si una respuesta HTTP contiene una imagen PNG, la respuesta podría tener los siguientes encabezados.

[!code-console[Main](media-formatters/samples/sample1.cmd)]

Cuando el cliente envía un mensaje de solicitud, puede incluir un encabezado Accept. El encabezado Accept indica que el servidor de tipos de medios que el cliente desea desde el servidor. Por ejemplo:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

Este encabezado indica al servidor que desea que el cliente HTML, XHTML o XML.

El tipo de medio determina cómo la API Web serializa y deserializa el cuerpo del mensaje HTTP. API Web tiene compatibilidad integrada para XML, JSON, BSON y datos de formato form-urlencoded y puede admitir tipos de medios adicionales escribiendo un *formateador media*.

Para crear a un formateador de medios, que se derivan de una de estas clases:

- [Elemento MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx). Esta lectura asincrónica de usos de clases y métodos de escritura.
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). Esta clase se deriva de **elemento MediaTypeFormatter** pero utiliza los métodos de lectura/escritura sychronous.

Derivar de **BufferedMediaTypeFormatter** es más sencillo, porque no hay ningún código asincrónico, pero también significa que puede bloquear el subproceso de llamada durante la E/S.

## <a name="example-creating-a-csv-media-formatter"></a>Ejemplo: Creación de un formateador de medios CSV

El ejemplo siguiente muestra un formateador de tipo de medios que puede serializar un objeto de producto a un formato de valores separados por comas (CSV). Este ejemplo utiliza el tipo de producto definido en el tutorial [crear una API Web que admite operaciones de CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md). Esta es la definición del objeto de producto:

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

Para implementar un formateador CSV, defina una clase que deriva de **BufferedMediaTypeFormater**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

En el constructor, agregue los tipos de medios que admite el formateador. En este ejemplo, el formateador admite un único tipo de medio, &quot;texto o csv&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

Invalidar el **CanWriteType** método para indicar que escribe el formateador puede serializar:

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

En este ejemplo, el formateador puede serializar solo `Product` objetos, así como las colecciones de `Product` objetos.

De forma similar, reemplace el **CanReadType** método para indicar que escribe el formateador puede deserializar. En este ejemplo, el formateador no admite la deserialización, por lo que simplemente devuelve el método **false**.

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

Por último, reemplace el **WriteToStream** método. Este método serializa un tipo escribiendo en una secuencia. Si el formateador admite la deserialización, también invalidar el **ReadFromStream** método.

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Agregar a un formateador de medios a la canalización de API Web

Para agregar un tipo de medio formateador a la canalización de Web API, use el **formateadores** propiedad en el **HttpConfiguration** objeto.

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>Codificaciones de caracteres

Opcionalmente, un formateador de medios puede admitir varias codificaciones de caracteres, como UTF-8 o ISO 8859-1.

En el constructor, agregue uno o varios [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) tipos a la **SupportedEncodings** colección. Coloque el valor predeterminado de codificación primero.

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

En el **WriteToStream** y **ReadFromStream** llamar métodos, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) para seleccionar la codificación de caracteres preferida. Este método coincide con los encabezados de solicitud con la lista de codificaciones compatibles. Utilice el valor devuelto **Encoding** al leer o escribir desde la secuencia:

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
