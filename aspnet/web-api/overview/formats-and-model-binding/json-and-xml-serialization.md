---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: "Serialización de XML en ASP.NET Web API y JSON | Documentos de Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2012
ms.topic: article
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: 7aafe4823d3a6090fae4a63f1a66fb2670ecb025
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a>Serialización de XML en ASP.NET Web API y JSON
====================
por [Mike Wasson](https://github.com/MikeWasson)

Este artículo describen a los formateadores JSON y XML en ASP.NET Web API.

En ASP.NET Web API, un *formateador de tipo de medio* es un objeto que puede:

- Cuerpo del mensaje de objetos CLR de lectura de HTTP
- Escribir objetos CLR en un cuerpo del mensaje HTTP

API Web proporciona a los formateadores de tipo de medio para JSON y XML. El marco de trabajo inserta a estos formateadores se basan en la canalización predeterminada. Los clientes pueden solicitar JSON o XML en el encabezado de aceptación de la solicitud HTTP.

## <a name="contents"></a>Contenido

- [Formateador de tipo de medio JSON](#json_media_type_formatter)

    - [Propiedades de solo lectura](#json_readonly)
    - [Fechas](#json_dates)
    - [Aplicar sangría](#json_indenting)
    - [Mayúsculas y minúsculas tipo Camel](#json_camelcasing)
    - [Objetos anónimos y débilmente tipada](#json_anon)
- [Formateador de tipo de medio XML](#xml_media_type_formatter)

    - [Propiedades de solo lectura](#xml_readonly)
    - [Fechas](#xml_dates)
    - [Aplicar sangría](#xml_indenting)
    - [Serializadores XML de configuración por tipo](#xml_pertype)
- [Quitar el JSON o formateador XML](#removing_the_json_or_xml_formatter)
- [Control de referencias circulares a objetos](#handling_circular_object_references)
- [Probar la serialización de objetos](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>Formateador de tipo de medio JSON

Aplicación de formato JSON proporcionada por el **JsonMediaTypeFormatter** clase. De forma predeterminada, **JsonMediaTypeFormatter** utiliza la [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) biblioteca que se va a realizar la serialización. Json.NET es un proyecto de código abierto de terceros.

Si lo prefiere, puede configurar la **JsonMediaTypeFormatter** clase se debe utilizar el **DataContractJsonSerializer** en lugar de Json.NET. Para ello, establezca la **UseDataContractJsonSerializer** propiedad **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>Serialización de JSON

Esta sección describen algunos comportamientos específicos del formateador JSON, usar el valor predeterminado [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializador. Esto no pretende ser documentación completa de la biblioteca de Json.NET; Para obtener más información, consulte el [Json.NET documentación](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>¿Qué se serializarán?

De forma predeterminada, todas las propiedades públicas y campos se incluyen en el JSON serializado. Para omitir una propiedad o campo, decorarla con el **JsonIgnore** atributo.

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Si prefiere un &quot;participar en&quot; enfocar, decore la clase con la **DataContract** atributo. Si este atributo está presente, se omiten los miembros a menos que tengan la **DataMember**. También puede usar **DataMember** para serializar los miembros privados.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Propiedades de solo lectura

Propiedades de solo lectura se serializan de forma predeterminada.

<a id="json_dates"></a>
### <a name="dates"></a>fechas

De forma predeterminada, Json.NET escribe fechas [ISO 8601](http://www.w3.org/TR/NOTE-datetime) formato. Las fechas en UTC (hora Universal) se escriben con un sufijo "Z". Las fechas en hora local incluyen un desplazamiento de zona horaria. Por ejemplo:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

De forma predeterminada, Json.NET conserva la zona horaria. Esto se puede invalidar estableciendo la propiedad DateTimeZoneHandling:

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Si prefiere usar [formato de fecha de JSON de Microsoft](https://msdn.microsoft.com/en-us/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) en lugar de ISO 8601, establecer el **DateFormatHandling** propiedad en la configuración del serializador:

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Aplicar sangría

Para escribir JSON con sangría, establezca el **formato** si se establece en **Formatting.Indented**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Mayúsculas y minúsculas tipo Camel

Para escribir los nombres de propiedad JSON con mayúsculas y minúsculas camel, sin cambiar el modelo de datos, establezca la **CamelCasePropertyNamesContractResolver** en el serializador:

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Objetos anónimos y débilmente tipada

Un método de acción puede devolver un objeto anónimo y serializar a JSON. Por ejemplo:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

El cuerpo del mensaje de respuesta contendrá el JSON siguiente:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Si su web API recibe flexible estructurados objetos JSON de los clientes, puede deserializar el cuerpo de solicitud para un **Newtonsoft.Json.Linq.JObject** tipo.

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

Sin embargo, es mejor usar objetos de datos fuertemente tipados. A continuación, no es necesario analizar los datos usted mismo, y obtendrá las ventajas de validación del modelo.

El serializador XML no admite tipos anónimos o **JObject** instancias. Si utiliza estas características para los datos JSON, debe quitar al formateador de XML de la canalización, como se describe más adelante en este artículo.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>Formateador de tipo de medio XML

Aplicación de formato XML proporcionada por el **XmlMediaTypeFormatter** clase. De forma predeterminada, **XmlMediaTypeFormatter** utiliza la **DataContractSerializer** clase para realizar la serialización.

Si lo prefiere, puede configurar la **XmlMediaTypeFormatter** para usar el **XmlSerializer** en lugar de la **DataContractSerializer**. Para ello, establezca la **/usexmlserializer** propiedad **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

El **XmlSerializer** clase admite un conjunto más estrecho de tipos que **DataContractSerializer**, pero proporciona más control sobre el XML resultante. Considere el uso de **XmlSerializer** si tiene que coincidir con un esquema XML existente.

### <a name="xml-serialization"></a>Serialización XML

Esta sección describen algunos comportamientos específicos del formateador XML, con el valor predeterminado **DataContractSerializer**.

De forma predeterminada, DataContractSerializer se comporta como sigue:

- Todas las propiedades de lectura y escritura públicas y campos se serializan. Para omitir una propiedad o campo, decorarla con el **IgnoreDataMember** atributo.
- No se serializan los miembros privados y protegidos.
- No se serializan las propiedades de solo lectura. (No obstante, se serializa el contenido de una propiedad de colección de solo lectura).
- Nombres de clase y miembro se escriben en el archivo XML exactamente como aparecen en la declaración de clase.
- Se utiliza un espacio de nombres XML de forma predeterminada.

Si necesita más control sobre la serialización, puede decorar la clase con la **DataContract** atributo. Cuando este atributo está presente, la clase se serializa como se indica a continuación:

- &quot;Participar en&quot; enfoque: los campos y propiedades no se serializan de forma predeterminada. Para serializar una propiedad o campo, decorarla con el **DataMember** atributo.
- Para serializar un miembro privado o protegido, decorarla con el **DataMember** atributo.
- No se serializan las propiedades de solo lectura.
- Para cambiar cómo aparece el nombre de clase en el archivo XML, establezca el *nombre* parámetro en el **DataContract** atributo.
- Para cambiar la apariencia de un nombre de miembro en el código XML, establezca el *nombre* parámetro en el **DataMember** atributo.
- Para cambiar el espacio de nombres XML, establezca el *Namespace* parámetro en el **DataContract** clase.

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Propiedades de solo lectura

No se serializan las propiedades de solo lectura. Si una propiedad de solo lectura tiene un campo privado de copias de seguridad, puede marcar el campo privado con la **DataMember** atributo. Este enfoque requiere la **DataContract** atributo en la clase.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>fechas

Las fechas se escriben en formato ISO 8601. Por ejemplo, &quot;2012-05-23T20:21:37.9116538Z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Aplicar sangría

Para escribir datos XML con sangría, establezca el **sangría** propiedad **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Serializadores XML de configuración por tipo

Puede establecer los serializadores XML diferentes para distintos tipos CLR. Por ejemplo, podría tener un objeto de datos determinado que requiere **XmlSerializer** por compatibilidad con versiones anteriores. Puede usar **XmlSerializer** para este objeto y seguir usando **DataContractSerializer** para otros tipos.

Para establecer un serializador de XML para un tipo determinado, llame a **SetSerializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

Puede especificar un **XmlSerializer** o cualquier objeto que se deriva de **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Quitar el JSON o formateador XML

Puede quitar el formateador JSON o el formateador de XML de la lista de formateadores, si no desea utilizarlos. Las principales razones para ello son:

- Para restringir las respuestas de API web a un tipo de medio determinado. Por ejemplo, podría decidir admitir solo respuestas en JSON y quitar al formateador de XML.
- Para reemplazar al formateador predeterminado con un formateador personalizado. Por ejemplo, podría reemplazar al formateador JSON con su propia implementación personalizada de un formateador JSON.

El código siguiente muestra cómo quitar a los formateadores de forma predeterminada. Llamar a este método desde su **aplicación\_iniciar** método, definido en Global.asax.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>Control de referencias circulares a objetos

De forma predeterminada, los formateadores JSON y XML escribir todos los objetos como valores. Si dos propiedades que hacen referencia al mismo objeto, o si el mismo objeto aparece dos veces en una colección, el formateador serializará dos veces el objeto. Esto es un problema determinado si los objetos del gráfico contiene ciclos, porque el serializador producirá una excepción cuando detecta un bucle en el gráfico.

Tenga en cuenta los siguientes modelos de objetos y el controlador.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

Invocar esta acción hará que al formateador que se produce una excepción, lo que se traduce en un estado código 500 (Error interno del servidor) de respuesta al cliente.

Para conservar las referencias de objeto en JSON, agregue el código siguiente a **aplicación\_iniciar** método en el archivo Global.asax:

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

Ahora, la acción del controlador devolverá JSON que el siguiente aspecto:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Tenga en cuenta que el serializador agrega un &quot;$id&quot; propiedad a ambos objetos. Asimismo, detecta que la propiedad Employee.Department crea un bucle, por lo que reemplaza el valor con una referencia de objeto: {&quot;$ref&quot;:&quot;1&quot;}.

> [!NOTE]
> Referencias a objetos no son estándar en JSON. Antes de usar esta característica, tenga en cuenta si los clientes podrán realizar analizar los resultados. Podría ser mejor simplemente quitar ciclos del gráfico. Por ejemplo, el vínculo desde el empleado al departamento no se necesita realmente en este ejemplo.


Para conservar las referencias de objeto en XML, tiene dos opciones. La opción más sencilla consiste en agregar `[DataContract(IsReference=true)]` a la clase de modelo. El *IsReference* parámetro permite referencias a objetos. Recuerde que **DataContract** hace serialización participar en, por lo que también necesitará agregar **DataMember** atributos a las propiedades:

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Ahora el formateador generará código XML similar al siguiente:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Si desea evitar atributos en la clase de modelo, hay otra opción: crear una nueva específica del tipo **DataContractSerializer** instancia y establecer *preserveObjectReferences* a **es true**  en el constructor. A continuación, establecer esta instancia como un serializador por tipo en el formateador de tipo de medio XML. El código siguiente muestra cómo hacerlo:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Probar la serialización de objetos

Al diseñar su API web, resulta útil probar cómo se van a serializar los objetos de datos. Puede hacerlo sin crear un controlador o invocar una acción de controlador.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
