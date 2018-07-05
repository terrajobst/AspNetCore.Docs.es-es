---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Compatibilidad con BSON en ASP.NET Web API 2.1 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 7c5e763d92295a83b3431e9ec6e305b07f8a64d2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370499"
---
<a name="bson-support-in-aspnet-web-api-21"></a>Compatibilidad con BSON en ASP.NET Web API 2.1
====================
por [Mike Wasson](https://github.com/MikeWasson)

Web API 2.1 incluye compatibilidad con BSON. Este tema muestra cómo usar BSON en el controlador de API de Web (servidor) y en una aplicación cliente. NET.

## <a name="what-is-bson"></a>¿Qué es BSON?

[BSON](http://bsonspec.org/) es un formato de serialización binaria. "BSON" es el acrónimo "Binario"JSON", pero BSON y JSON se serializan de forma muy diferente. Como BSON solo es "JSON similar a", porque los objetos se representan como pares de nombre-valor, similares a JSON. A diferencia de JSON, los tipos de datos numéricos se almacenan como bytes, cadenas no

BSON se diseñó para ser ligero, fácil de explorar y rápido para la codificación y descodificación.

- BSON es comparable en tamaño a JSON. Dependiendo de los datos, una carga BSON puede ser menor o mayor que una carga JSON. Para serializar los datos binarios, como un archivo de imagen BSON es menor que JSON, porque los datos binarios no están codificado en base64.
- Son fáciles de analizar porque los elementos van precedidos de un campo de longitud, por lo que un analizador puede omitir los elementos sin decodificarlos documentos BSON.
- Codificación y descodificación son eficaces, porque los tipos de datos numéricos se almacenan como números, cadenas no.

Los clientes nativos, como las aplicaciones cliente. NET, pueden beneficiarse del uso BSON en lugar de los formatos basados en texto como JSON o XML. Para los clientes de explorador, probablemente deseará quedarse con JSON, porque JavaScript puede convertir directamente la carga de JSON.

Afortunadamente, la API Web usa [negociación de contenido](content-negotiation.md), por lo que puede admitir ambos formatos y permitir que el cliente elija su API.

## <a name="enabling-bson-on-the-server"></a>Habilitar BSON en el servidor

En la configuración de la API Web, agregue el **BsonMediaTypeFormatter** a la colección de formateadores.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

Ahora, si el cliente solicita "application/bson", API Web usará al formateador BSON.

Para asociar BSON con otros tipos de medios, agregarlos a la colección SupportedMediaTypes. El código siguiente agrega "application/vnd.contoso" para los tipos de medios admitidos:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>Sesión de HTTP de ejemplo

En este ejemplo, vamos a usar un controlador de API Web simple además de la clase del modelo siguiente:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

Un cliente puede enviar la solicitud HTTP siguiente:

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

Esta es la respuesta:

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

Aquí he reemplazado los datos binarios con &quot;.&quot; caracteres. Captura de pantalla siguiente muestra Fiddler los valores sin formato hexadecimales.

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>Uso de BSON con HttpClient

Las aplicaciones de los clientes de .NET pueden utilizar el formateador BSON con **HttpClient**. Para obtener más información acerca de **HttpClient**, consulte [una llamada a una Web API desde un cliente .NET](../advanced/calling-a-web-api-from-a-net-client.md).

El código siguiente envía una solicitud GET que acepta BSON y, a continuación, deserializa la carga BSON en la respuesta.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

Para solicitar BSON desde el servidor, establezca el encabezado Accept a "application/bson":

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

Para deserializar el cuerpo de respuesta, utilice el **BsonMediaTypeFormatter**. Este formateador no está en la colección de formateadores de forma predeterminada, por lo que debe especificarla al leer el cuerpo de respuesta:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

El ejemplo siguiente muestra cómo enviar una solicitud POST que contiene BSON.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

Gran parte de este código es el mismo que el ejemplo anterior. Sin embargo, en el **PostAsync** método, especifique **BsonMediaTypeFormatter** como el formateador:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>Serializar tipos primitivos de nivel superior

Todos los documentos BSON es una lista de pares clave/valor. La especificación de BSON no define una sintaxis para serializar un único valor sin formato, como un entero o cadena.

Para evitar esta limitación, la **BsonMediaTypeFormatter** trata los tipos primitivos como un caso especial. Antes de serializar, convierte el valor en un par clave/valor con la clave "Value". Por ejemplo, suponga que el controlador de API devuelve un entero:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

Antes de serializar, el formateador BSON lo convierte en el siguiente par clave-valor:

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

Al deserializar, el formateador convierte los datos al valor original. Sin embargo, los clientes que usan un analizador BSON diferente necesitará controlar este caso, si su API web devuelve los valores sin formato. En general, debería considerar devolver datos estructurados, en lugar de valores sin procesar.

## <a name="additional-resources"></a>Recursos adicionales

[Ejemplo de la API BSON de Web](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[Formateadores de contenido multimedia](media-formatters.md)
