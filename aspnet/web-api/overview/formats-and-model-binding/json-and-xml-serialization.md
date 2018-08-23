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
<a name="json-and-xml-serialization-in-aspnet-web-api"></a>Serialización JSON y XML en ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

En este artículo se describe a los formateadores JSON y XML en ASP.NET Web API.

En ASP.NET Web API, un *formateador de tipo de medio* es un objeto que puede:

- Cuerpo del mensaje de los objetos de CLR de lectura de HTTP
- Escribir objetos CLR en un cuerpo de mensaje HTTP

API Web proporciona a los formateadores de tipo de medio para JSON y XML. El marco de trabajo inserta a estos formateadores en la canalización de forma predeterminada. Los clientes pueden solicitar JSON o XML en el encabezado Accept de la solicitud HTTP.

## <a name="contents"></a>Contenido

- [Formateador de tipo de medio JSON](#json_media_type_formatter)

    - [Propiedades de solo lectura](#json_readonly)
    - [Fechas](#json_dates)
    - [Aplicar sangría](#json_indenting)
    - [Uso de mayúsculas y minúsculas](#json_camelcasing)
    - [Objetos anónimos y débilmente tipada](#json_anon)
- [Formateador de tipo de medio XML](#xml_media_type_formatter)

    - [Propiedades de solo lectura](#xml_readonly)
    - [Fechas](#xml_dates)
    - [Aplicar sangría](#xml_indenting)
    - [Serializadores XML de configuración por tipo](#xml_pertype)
- [Quitar el JSON o un formateador XML](#removing_the_json_or_xml_formatter)
- [Control de referencias circulares a objetos](#handling_circular_object_references)
- [Serialización de objetos de prueba](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>Formateador de tipo de medio JSON

Formato JSON proporcionado por el **JsonMediaTypeFormatter** clase. De forma predeterminada, **JsonMediaTypeFormatter** usa el [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) biblioteca para realizar la serialización. Json.NET es un proyecto de código abierto de terceros.

Si lo prefiere, puede configurar el **JsonMediaTypeFormatter** clase se debe utilizar el **DataContractJsonSerializer** en lugar de Json.NET. Para ello, establezca el **UseDataContractJsonSerializer** propiedad **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>Serialización de JSON

Esta sección describen algunos comportamientos específicos del formateador JSON, con el valor predeterminado [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializador. Esto no pretende ser una documentación completa de la biblioteca Json.NET; Para obtener más información, consulte el [documentación Json.NET](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>¿Lo que se serializa?

De forma predeterminada, todas las propiedades públicas y campos se incluyen en el JSON serializado. Para omitir un campo o propiedad, decórela con el **JsonIgnore** atributo.

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Si prefiere un &quot;participación&quot; enfocar, decore la clase con el **DataContract** atributo. Si este atributo está presente, se omiten los miembros a menos que tengan la **DataMember**. También puede usar **DataMember** para serializar los miembros privados.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Propiedades de solo lectura

Propiedades de solo lectura se serializan de forma predeterminada.

<a id="json_dates"></a>
### <a name="dates"></a>Fechas

De forma predeterminada, Json.NET escribe las fechas [ISO 8601](http://www.w3.org/TR/NOTE-datetime) formato. Las fechas en formato UTC (Coordinated Universal Time) se escriben con un sufijo "Z". Las fechas en la hora local incluyen un desplazamiento de zona horaria. Por ejemplo:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

De forma predeterminada, Json.NET conserva la zona horaria. Puede invalidar esto estableciendo la propiedad DateTimeZoneHandling:

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Si prefiere usar [formato de fecha JSON Microsoft](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) en lugar de ISO 8601, establezca el **DateFormatHandling** propiedad en la configuración del serializador:

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Aplicar sangría

Para escribir JSON con sangría, establezca el **formato** si se establece en **Formatting.Indented**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Uso de mayúsculas y minúsculas

Para escribir los nombres de propiedad JSON con el uso de mayúsculas y minúsculas, sin cambiar el modelo de datos, establezca el **CamelCasePropertyNamesContractResolver** en el serializador:

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Objetos anónimos y débilmente tipada

Un método de acción puede devolver un objeto anónimo y serializarlo en JSON. Por ejemplo:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

El cuerpo del mensaje de respuesta contendrá el JSON siguiente:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Si su web API recibe imprecisa estructurados objetos JSON de los clientes, puede deserializar el cuerpo de solicitud a un **Newtonsoft.Json.Linq.JObject** tipo.

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

Sin embargo, es mejor usar objetos de datos fuertemente tipados. A continuación, no es necesario analizar los datos usted mismo y obtener las ventajas de la validación del modelo.

El serializador XML no admite tipos anónimos o **JObject** instancias. Si utiliza estas características para los datos JSON, debe quitar al formateador XML de la canalización, como se describe más adelante en este artículo.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>Formateador de tipo de medio XML

Aplicación de formato XML proporcionada por el **XmlMediaTypeFormatter** clase. De forma predeterminada, **XmlMediaTypeFormatter** usa el **DataContractSerializer** clase para realizar la serialización.

Si lo prefiere, puede configurar el **XmlMediaTypeFormatter** para usar el **XmlSerializer** en lugar de la **DataContractSerializer**. Para ello, establezca el **/usexmlserializer** propiedad **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

El **XmlSerializer** clase admite un conjunto más estrecho de tipos que **DataContractSerializer**, pero proporciona mayor control sobre el XML resultante. Considere el uso de **XmlSerializer** si tiene que coincidir con un esquema XML existente.

### <a name="xml-serialization"></a>Serialización XML

Esta sección describen algunos comportamientos específicos del formateador XML, utilizando el valor predeterminado **DataContractSerializer**.

De forma predeterminada, DataContractSerializer se comporta como sigue:

- Se serializan todos los campos y propiedades públicas de lectura/escritura. Para omitir un campo o propiedad, decórela con el **IgnoreDataMember** atributo.
- No se serializan los miembros privados y protegidos.
- No se serializan las propiedades de solo lectura. (No obstante, se serializa el contenido de una propiedad de colección de solo lectura).
- Nombres de clase y miembro se escriben en el XML exactamente como aparecen en la declaración de clase.
- Se utiliza el espacio de nombres XML predeterminado.

Si necesita más control sobre la serialización, puede decorar la clase con el **DataContract** atributo. Cuando este atributo está presente, la clase se serializa como sigue:

- &quot;Participar en&quot; enfoque: no se serializan las propiedades y campos de forma predeterminada. Para serializar una propiedad o campo, decórela con el **DataMember** atributo.
- Para serializar un miembro privado o protegido, decórela con el **DataMember** atributo.
- No se serializan las propiedades de solo lectura.
- Para cambiar cómo aparece el nombre de clase en el archivo XML, establezca el *nombre* parámetro en el **DataContract** atributo.
- Para cambiar la apariencia de un nombre de miembro en el archivo XML, establezca el *nombre* parámetro en el **DataMember** atributo.
- Para cambiar el espacio de nombres XML, establezca el *Namespace* parámetro en el **DataContract** clase.

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Propiedades de solo lectura

No se serializan las propiedades de solo lectura. Si una propiedad de solo lectura tiene un campo de respaldo privado, puede marcar el campo privado con la **DataMember** atributo. Este enfoque requiere el **DataContract** atributo de la clase.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>Fechas

Las fechas se escriben en formato ISO 8601. Por ejemplo, &quot;2012-05-23T20:21:37.9116538Z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Aplicar sangría

Para escribir XML con sangría, establezca el **sangría** propiedad **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Serializadores XML de configuración por tipo

Puede establecer los serializadores XML diferentes para distintos tipos CLR. Por ejemplo, podría tener un objeto de datos concreto requiere **XmlSerializer** para compatibilidad con versiones anteriores. Puede usar **XmlSerializer** para este objeto y seguir usando **DataContractSerializer** para otros tipos.

Para establecer un serializador XML para un tipo determinado, llame a **SetSerializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

Puede especificar un **XmlSerializer** o cualquier objeto que se deriva de **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Quitar el JSON o un formateador XML

Puede quitar el formateador JSON o el formateador XML en la lista de formateadores, si no desea usarlos. Las razones principales para hacerlo son:

- Para restringir las respuestas de API web a un tipo de medio determinado. Por ejemplo, podría decidir admitir sólo las respuestas JSON y quitar al formateador XML.
- Para reemplazar al formateador predeterminado con un formateador personalizado. Por ejemplo, podría reemplazar al formateador JSON con su propia implementación personalizada de un formateador JSON.

El código siguiente muestra cómo quitar a los formateadores de forma predeterminada. Llamar a este método desde su **aplicación\_iniciar** método, definido en Global.asax.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>Control de referencias circulares a objetos

De forma predeterminada, los formateadores JSON y XML escriben todos los objetos como valores. Si dos propiedades que hacen referencia al mismo objeto, o si el mismo objeto aparece dos veces en una colección, el formateador serializará el objeto dos veces. Esto es un problema determinado si el gráfico de objetos contiene ciclos, porque el serializador producirá una excepción cuando detecta un bucle en el gráfico.

Tenga en cuenta los siguientes modelos de objetos y el controlador.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

Invocar esta acción hará que al formateador que se produce una excepción, que se traduce en un estado código 500 (Error interno del servidor) de respuesta al cliente.

Para conservar las referencias de objeto en JSON, agregue el código siguiente al **aplicación\_iniciar** método en el archivo Global.asax:

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

Ahora, la acción de controlador devolverá JSON que tiene este aspecto:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Tenga en cuenta que el serializador agrega un &quot;$id&quot; propiedad a ambos objetos. Además, detecta que la propiedad Employee.Department crea un bucle, por lo que reemplaza el valor con una referencia de objeto: {&quot;$ref&quot;:&quot;1&quot;}.

> [!NOTE]
> Las referencias a objetos no son estándar en JSON. Antes de usar esta característica, considere si los clientes podrán analizar los resultados. Podría ser preferible simplemente quitar ciclos del gráfico. Por ejemplo, el vínculo desde el empleado al departamento no es necesario realmente en este ejemplo.


Para conservar las referencias de objeto en XML, tiene dos opciones. La opción más sencilla consiste en agregar `[DataContract(IsReference=true)]` a la clase de modelo. El *IsReference* parámetro permite referencias a objetos. Recuerde que **DataContract** hace serialización participar en, por lo que también deberá agregar **DataMember** atributos a las propiedades:

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Ahora el formateador generará XML similar al siguiente:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Si desea evitar atributos en la clase de modelo, hay otra opción: crear un nuevo específicos del tipo **DataContractSerializer** de instancia y establecer *preserveObjectReferences* a **true**  en el constructor. A continuación, establezca esta instancia como un serializador por tipo en el formateador de tipo de medio XML. El código siguiente se muestra cómo hacerlo:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Serialización de objetos de prueba

Al diseñar la API web, es útil probar cómo se va a serializar los objetos de datos. Puede hacerlo sin crear un controlador o invocar una acción de controlador.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
