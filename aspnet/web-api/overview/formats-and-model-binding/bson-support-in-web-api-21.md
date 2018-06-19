---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Compatibilidad con BSON en ASP.NET Web API 2.1 | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 53ad705fad6d2225cecca4d73355bd6ebfcf56d5
ms.sourcegitcommit: 459cb3289741a3f46325e605a617dc926ee0563d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2018
ms.locfileid: "27984588"
---
<a name="bson-support-in-aspnet-web-api-21"></a>Compatibilidad con BSON en ASP.NET Web API 2.1
====================
por [Mike Wasson](https://github.com/MikeWasson)

Web API 2.1 incorpora BSON. Este tema muestra cómo usar BSON en el controlador de Web API (servidor) y en una aplicación de cliente. NET.

## <a name="what-is-bson"></a>¿Qué es BSON?

[BSON](http://bsonspec.org/) es un formato de serialización binaria. "BSON" es el acrónimo "JSON binario", pero BSON y JSON se serializan de forma muy diferente. Como BSON solo es "JSON-like", porque los objetos se representan como pares de nombre / valor, similares a JSON. A diferencia de JSON, los tipos de datos numéricos se almacenan como bytes, no cadenas

BSON está diseñado para ser ligera, fácil de explorar y rápida para codificar y descodificar.

- BSON es comparable en cuanto a tamaño a JSON. Dependiendo de los datos, una carga BSON puede ser menor o mayor que una carga JSON. Para serializar datos binarios, como un archivo de imagen, BSON es menor que JSON, porque los datos binarios no están codificado en base64.
- Documentos BSON son fáciles de buscar porque elementos van precedidos de un campo de longitud, por lo que un analizador puede omitir los elementos sin descodificación de ellos.
- Codificación y descodificación son eficaces, porque los tipos de datos numéricos se almacenan como números, no las cadenas.

Clientes nativos, como aplicaciones de cliente de. NET, pueden beneficiarse de usar BSON en lugar de los formatos basados en texto, como JSON o XML. Para los clientes de explorador, probablemente deseará opte por JSON, porque JavaScript puede convertir directamente la carga de JSON.

Afortunadamente, usa la API Web [negociación de contenido](content-negotiation.md), por lo que puede admitir ambos formatos y permita que el cliente elija su API.

## <a name="enabling-bson-on-the-server"></a>Habilitar BSON en el servidor

En la configuración de la API Web, agregue el **BsonMediaTypeFormatter** a la colección de formateadores.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

Ahora, si el cliente solicita "application/bson", API Web usará al formateador BSON.

Para asociar BSON a otros tipos de medios, agréguelos a la colección SupportedMediaTypes. El código siguiente agrega "application/vnd.contoso" para los tipos de medios admitidos:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>Sesión de HTTP de ejemplo

En este ejemplo, vamos a usar la siguiente clase de modelo más de un controlador de API Web simple:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

Un cliente puede enviar la solicitud HTTP siguiente:

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

Esta es la respuesta:

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

Aquí se han reemplazado los datos binarios con &quot;.&quot; caracteres. La siguiente captura de pantalla muestra Fiddler los valores sin formato hexadecimales.

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>Usar BSON con HttpClient

Aplicaciones de los clientes de .NET pueden utilizar el formateador BSON con **HttpClient**. Para obtener más información acerca de **HttpClient**, consulte [al llamar a un Web API desde un cliente .NET](../advanced/calling-a-web-api-from-a-net-client.md).

El código siguiente envía una solicitud GET que acepta BSON y, a continuación, deserializa la carga BSON en la respuesta.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

Para solicitar BSON desde el servidor, establezca el encabezado Accept a "application/bson":

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

Para deserializar el cuerpo de respuesta, utilice la **BsonMediaTypeFormatter**. Este formateador no está en la colección de formateadores de forma predeterminada, por lo que debe indicar al leer el cuerpo de respuesta:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

En el ejemplo siguiente se muestra cómo enviar una solicitud POST que contiene BSON.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

Gran parte de este código es el mismo que el ejemplo anterior. Sin embargo, en la **PostAsync** método, especifique **BsonMediaTypeFormatter** como el formateador:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>Serializar los tipos primitivos de nivel superior

Cada documento BSON es una lista de pares clave/valor. La especificación de BSON no define una sintaxis para serializar un único valor sin formato, como un entero o cadena.

Para evitar esta limitación, la **BsonMediaTypeFormatter** trata los tipos primitivos como un caso especial. Antes de serializar, convierte el valor en un par de clave/valor con la clave "Value". Por ejemplo, suponga que el controlador de API devuelve un entero:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

Antes de serializar, el formateador BSON lo convierte en el siguiente par de clave/valor:

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

Al deserializar, el formateador convierte los datos en el valor original. Sin embargo, los clientes que usan un analizador BSON diferente necesitará controlar este caso, si la API web devuelve los valores sin formato. En general, debería considerar devolver datos estructurados, en lugar de valores sin procesar.

## <a name="additional-resources"></a>Recursos adicionales

[Ejemplo de API BSON de Web](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[Formateadores de contenido multimedia](media-formatters.md)
