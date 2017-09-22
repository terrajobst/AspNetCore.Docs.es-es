---
title: Formateadores personalizados en las API web de MVC de ASP.NET Core
author: tdykstra
description: "Obtenga información acerca de cómo crear y utilizar a formateadores personalizados para las API web de ASP.NET Core."
keywords: "Núcleo de ASP.NET, web api, formateadores personalizados"
ms.author: tdykstra
manager: wpickett
ms.date: 02/08/2017
ms.topic: article
ms.assetid: 1fb6fdc2-e199-4469-9012-b909d1913422
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/custom-formatters
ms.openlocfilehash: 5e665abe10fd7444c3fd5f20cfeca3ef0a5f79d3
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="custom-formatters-in-aspnet-core-mvc-web-apis"></a>Formateadores personalizados en las API web de MVC de ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra)

Núcleo de ASP.NET MVC tiene compatibilidad integrada para el intercambio de datos en las API web mediante el uso de formatos de texto sin formato, JSON o XML. Este artículo muestra cómo agregar compatibilidad con formatos adicionales mediante la creación de formateadores personalizados.

[Ver o descargar el ejemplo desde GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).

## <a name="when-to-use-custom-formatters"></a>Cuándo utilizar a formateadores personalizados

Utilizar un formateador personalizado cuando desee que el [negociación de contenido](xref:mvc/models/formatting) proceso para admitir un tipo de contenido que no es compatible con los formateadores integrados (JSON, XML y texto sin formato).

Por ejemplo, si algunos de los clientes para la API web pueden controlar la [Protobuf](https://github.com/google/protobuf) formato, puede usar Protobuf con dichos clientes porque es más eficaz.  También puede enviar contacto nombres y direcciones en la API de web [vCard](https://wikipedia.org/wiki/VCard) formato, un formato comúnmente utilizado para intercambiar datos de contacto. La aplicación de ejemplo proporcionada con este artículo implementa a un formateador de vCard simple.

## <a name="overview-of-how-to-use-a-custom-formatter"></a>Información general sobre cómo utilizar a un formateador personalizado

Estos son los pasos para crear y utilizar a un formateador personalizado:

* Crear una clase de formateador de salida si desea serializar los datos que se va a enviar al cliente.
* Crear una clase de formateador de entrada si desea deserializar los datos recibidos del cliente. 
* Agregar instancias de los formateadores para la `InputFormatters` y `OutputFormatters` colecciones en [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).

Las secciones siguientes proporcionan instrucciones y ejemplos de código para cada uno de estos pasos.

## <a name="how-to-create-a-custom-formatter-class"></a>Cómo crear una clase de formateador personalizado

Para crear a un formateador de:

* Derivar la clase de la clase base correspondiente.
* Especificar tipos de medios válido y codificaciones en el constructor.
* Invalidar `CanReadType` / `CanWriteType` métodos
* Invalidar `ReadRequestBodyAsync` / `WriteResponseBodyAsync` métodos
  
### <a name="derive-from-the-appropriate-base-class"></a>Derivar de la clase base adecuada

Para los tipos de medios de texto (por ejemplo, vCard), que se derivan de la [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) o [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) clase base.

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

Para los tipos binarios, que se derivan de la [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) o [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) clase base.

### <a name="specify-valid-media-types-and-encodings"></a>Especificar las codificaciones y tipos de medios válido

En el constructor, especificar tipos de medios válido y codificaciones, agregando a la `SupportedMediaTypes` y `SupportedEncodings` colecciones.

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]  
> No puede realizar la inserción de dependencias de constructor en una clase de formateador. Por ejemplo, no se puede obtener un registrador mediante la adición de un parámetro de registrador al constructor. Para obtener acceso a servicios, tendrá que usar el objeto de contexto que se pasa a los métodos. Un ejemplo de código [a continuación](#read-write) muestra cómo hacerlo.

### <a name="override-canreadtypecanwritetype"></a>Invalidar CanReadType/CanWriteType 

Especificar el tipo se puede deserializar en o serializar desde invalidando el `CanReadType` o `CanWriteType` métodos. Por ejemplo, solo puede crear texto de vCard desde un `Contact` tipo, y viceversa.

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a>El método CanWriteResult

En algunos casos tendrá que invalidar `CanWriteResult` en lugar de `CanWriteType`. Use `CanWriteResult` si se cumplen las condiciones siguientes:

  * El método de acción devuelve una clase de modelo.
  * Hay clases derivadas que pueden obtenerse en tiempo de ejecución.
  * Debe conocer en tiempo de ejecución que deriva la clase devuelto por la acción.  

Por ejemplo, suponga que la firma del método de acción devuelve un `Person` tipo, pero se puede devolver un `Student` o `Instructor` tipo que deriva de `Person`. Si desea que el formateador para controlar sólo `Student` objetos, comprobar el tipo de [objeto](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) en el objeto de contexto proporcionado para el `CanWriteResult` método. Tenga en cuenta que no es necesario utilizar `CanWriteResult` cuando se devuelve el método de acción `IActionResult`; en ese caso, el `CanWriteType` método recibe el tipo en tiempo de ejecución.

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a>Invalidar ReadRequestBodyAsync/WriteResponseBodyAsync 

Hace que el trabajo real de deserializar o serializar en `ReadRequestBodyAsync` o `WriteResponseBodyAsync`.  Las líneas resaltadas en el ejemplo siguiente muestran cómo obtener los servicios desde el contenedor de inyección de dependencia (no se puede obtener a partir de los parámetros del constructor).

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a>Cómo configurar MVC para utilizar a un formateador personalizado
 
Para utilizar un formateador personalizado, debe agregar una instancia de la clase de formateador para la `InputFormatters` o `OutputFormatters` colección.

[!code-csharp[Main](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

Formateadores se evalúan en el orden en que se insertaron. La primera de ellas tiene prioridad. 

## <a name="next-steps"></a>Pasos siguientes

Consulte la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), que implementa vCard simple entrada y salida formateadores.  La aplicación lee y escribe vCards que tengan un aspecto similar al ejemplo siguiente:

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

Para ver vCard de salida, ejecute la aplicación y enviar una solicitud Get con Accept encabezado "texto/vcard" a `http://localhost:63313/api/contacts/` (cuando se ejecuta desde Visual Studio) o `http://localhost:5000/api/contacts/` (cuando se ejecuta desde la línea de comandos).

Para agregar una tarjeta vCard a la colección en memoria de contactos, enviar una solicitud Post a la misma dirección URL, con el encabezado Content-Type "texto/vcard" y con texto de vCard en el cuerpo, con formato similar al ejemplo anterior.
