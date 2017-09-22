---
title: Aplicar formato a los datos de respuesta en MVC de ASP.NET Core
author: ardalis
description: "Obtenga información acerca de cómo dar formato a los datos de respuesta en MVC de ASP.NET Core."
keywords: "Núcleo de ASP.NET, datos de respuesta, IOutputFormatter, IActionResult"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: c056df45-d013-4826-91a1-4a092bae1ea5
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/formatting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 91ba2456178fe806b90f27bbd2940773da950423
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a>Introducción a los datos de respuesta de formato en MVC de ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

Núcleo de ASP.NET MVC tiene compatibilidad integrada para dar formato a los datos de respuesta, con formatos fijos o en respuesta a las especificaciones del cliente.

[Ver o descargar el ejemplo desde GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).

## <a name="format-specific-action-results"></a>Resultados de acción específica de formato

Algunos tipos de resultado de acción son específicos de un formato determinado, como `JsonResult` y `ContentResult`. Las acciones pueden devolver resultados específicos que siempre se da formato de una manera determinada. Por ejemplo, devolver un `JsonResult` devolverá datos con formato JSON, independientemente de las preferencias del cliente. Del mismo modo, devolver un `ContentResult` devolverá datos de cadena en formato de texto sin formato (así como devolver simplemente una cadena).

> [!NOTE]
> No es necesario una acción para devolver ningún tipo en particular; MVC es compatible con cualquier valor de objeto del valor devuelto. Si una acción devuelve un `IActionResult` hereda de implementación y el controlador de `Controller`, los desarrolladores tienen muchos métodos auxiliares correspondientes a muchas de las opciones. Resultados de las acciones que devuelven objetos que no son `IActionResult` tipos se serializará con la correspondiente `IOutputFormatter` implementación.

Se devuelven los datos en un formato específico de un controlador que hereda de la `Controller` clase base, utilice el método de aplicación auxiliar integradas `Json` para devolver JSON y `Content` de texto sin formato. El método de acción debe devolver el tipo de resultado específico (por ejemplo, `JsonResult`) o `IActionResult`.

Devolver datos con formato JSON:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

Respuesta de ejemplo de esta acción:

![Pestaña red de las herramientas de desarrollo con Microsoft Edge, que muestra el tipo de contenido de la respuesta es application/json](formatting/_static/json-response.png)

Tenga en cuenta que el tipo de contenido de la respuesta es `application/json`, como se muestra en la lista de las solicitudes de red y en la sección de encabezados de respuesta. Tenga en cuenta también la lista de opciones que se presentan por el explorador (en este caso, Microsoft Edge) en el encabezado Accept en la sección de encabezados de solicitud. La técnica actual está omitiendo este encabezado; seguir se describe a continuación.

Para devolver datos de texto con formato, use `ContentResult` y `Content` auxiliar:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

Una respuesta de esta acción:

![Pestaña red de las herramientas de desarrollo con Microsoft Edge, que muestra el tipo de contenido de la respuesta es texto/sin formato](formatting/_static/text-response.png)

Tenga en cuenta en este caso el `Content-Type` devuelto es `text/plain`. También se puede lograr este mismo comportamiento usando simplemente un tipo de respuesta de cadena:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> Para acciones no trivial con varios, opciones (por ejemplo, códigos de estado HTTP distintos en función del resultado de las operaciones realizadas) o tipos de retorno, prefiera `IActionResult` como el tipo de valor devuelto.

## <a name="content-negotiation"></a>Negociación de contenido

Negociación de contenido (*conneg* abreviado) se produce cuando el cliente especifica un [encabezado Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html). El formato predeterminado utilizado por núcleo de ASP.NET MVC es JSON. Negociación de contenido se implementa mediante `ObjectResult`. También se integra en el código de estado que devuelven resultados de acción específica de los métodos auxiliares (que se basan en `ObjectResult`). También puede devolver un tipo de modelo (es decir, una clase que haya definido como el tipo de transferencia de datos) y el marco de trabajo ajustará automáticamente en un `ObjectResult` automáticamente.

El siguiente método de acción utiliza el `Ok` y `NotFound` métodos auxiliares:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

Se devolverá una respuesta con formato JSON a menos que se solicitó otro formato y el servidor puede devolver el formato solicitado. Puede usar una herramienta como [Fiddler](http://www.telerik.com/fiddler) para crear una solicitud que incluye un encabezado de aceptación y especifique otro formato. En ese caso, si el servidor tiene un *formateador* que puede producir una respuesta en el formato solicitado, se devolverá el resultado en el formato preferido por el cliente.

![Consola de Fiddler que muestra un creados manualmente obtener la solicitud con un valor de encabezado de aceptación de aplicación/xml](formatting/_static/fiddler-composer.png)

En la captura de pantalla anterior, se ha utilizado el Compositor de Fiddler para generar una solicitud, especificar `Accept: application/xml`. De forma predeterminada, MVC de ASP.NET Core solo admite JSON, incluso cuando se especifica otro formato, el resultado devuelto es todavía con formato JSON. Verá cómo agregar los formateadores adicionales en la sección siguiente.

Acciones del controlador pueden devolver POCOs (sin formato antiguo objetos CLR), en cuyo caso ASP.NET MVC creará automáticamente un `ObjectResult` que ajusta el objeto. El cliente reciba el objeto serializado con formato (formato JSON es el valor predeterminado; puede configurar otros formatos o XML). Si el objeto que se va a devolver es `null`, el marco de trabajo se devolverá un `204 No Content` respuesta.

Devuelve un tipo de objeto:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

En el ejemplo, una solicitud de un alias válido autor recibirá una respuesta 200 OK con datos del autor. Una solicitud de un alias válido recibirá una respuesta 204 Sin contenido. A continuación se muestran capturas de pantalla que muestra la respuesta en formatos XML y JSON.

### <a name="content-negotiation-process"></a>Proceso de negociación de contenido

Contenido *negociación* solo se lleva a cabo si una `Accept` encabezado aparece en la solicitud. Cuando una solicitud contiene un encabezado de aceptación, el marco de trabajo enumerará los tipos de medios en el encabezado accept en orden de preferencia e intentará encontrar a un formateador que puede generar una respuesta en uno de los formatos especificados por el encabezado accept. En caso de que no se encuentra ningún formateador que puede satisfacer la solicitud del cliente, el marco de trabajo intentará encontrar el primer formateador que puede generar una respuesta (a menos que el desarrollador ha configurado la opción de `MvcOptions` para devolver 406 No aceptable en su lugar). Si la solicitud especifica XML, pero no se ha configurado el formateador de XML, se usará el formateador JSON. Por lo general, si no se ha configurado ningún formateador que puede proporcionar el formato solicitado, a continuación, se utiliza el primer formateador que puede dar formato al objeto. Si no se especifica ningún encabezado, se utilizará el primer formateador que puede controlar el objeto que se va a devolver para serializar la respuesta. En este caso, no hay ninguna negociación que tienen lugar: el servidor es determinar qué formato va a usar.

> [!NOTE]
> Si el encabezado Accept contiene `*/*`, el encabezado se omitirá a menos que `RespectBrowserAcceptHeader` está establecida en true en `MvcOptions`.

### <a name="browsers-and-content-negotiation"></a>Exploradores y negociación de contenido

A diferencia de los clientes de API típicos, exploradores web tienden a proporcionar `Accept` encabezados que incluyen una amplia gama de formatos, incluidos los caracteres comodín. De forma predeterminada, cuando el marco de trabajo detecta que la solicitud procede de un explorador, omitirá el `Accept` encabezado y valor devuelto en su lugar, el contenido de la aplicación configurar formato predeterminado (JSON, a menos que se configure de otra). Esto proporciona una experiencia más coherente al utilizar otros exploradores para utilizar las API.

Si prefiere el Explorador de honor aplicación encabezados de aceptación, puede configurar esto como parte de la configuración de MVC estableciendo `RespectBrowserAcceptHeader` a `true` en el `ConfigureServices` método *Startup.cs*.

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
}
```

## <a name="configuring-formatters"></a>Configuración de formateadores

Si la aplicación necesita admitir formatos adicionales más allá del valor predeterminado es JSON, puede agregar paquetes de NuGet y configurar MVC para admitirlas. Hay formateadores independientes para la entrada y salida. Formateadores de entrada se utilizan por [enlace de modelos](model-binding.md); formateadores de salida se utilizan para dar formato a las respuestas. También puede configurar [personalizado formateadores](../advanced/custom-formatters.md).

### <a name="adding-xml-format-support"></a>Agregar compatibilidad con el formato XML

Para agregar compatibilidad para dar formato a XML, instale el `Microsoft.AspNetCore.Mvc.Formatters.Xml` paquete NuGet.

Agregue el XmlSerializerFormatters a configuración de MVC en *Startup.cs*:

[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

Como alternativa, puede agregar al formateador de salida:

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

Estos dos enfoques serializará resultados con `System.Xml.Serialization.XmlSerializer`. Si lo prefiere, puede usar el `System.Runtime.Serialization.DataContractSerializer` agregando su formateador asociado:

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

Una vez que se ha agregado compatibilidad con formato XML, los métodos de controlador deben devolver el formato adecuado en función de la solicitud `Accept` encabezado, como en este ejemplo se muestra de Fiddler:

![Consola de Fiddler: pestaña sin el formato de la solicitud muestra el valor del encabezado Accept es application/xml. La pestaña sin formato para la respuesta muestra el valor del encabezado Content-Type de aplicación/xml.](formatting/_static/xml-response.png)

Puede ver en la ficha de inspectores que se realizó la solicitud GET sin formato con un `Accept: application/xml` conjunto de encabezados. Se muestra en el panel respuesta el `Content-Type: application/xml` encabezado y el `Author` objeto se ha serializado en XML.

Utilice la ficha compositor para modificar la solicitud para especificar `application/json` en el `Accept` encabezado. Ejecutar la solicitud y la respuesta se formatearán como JSON:

![Consola de Fiddler: pestaña sin el formato de la solicitud muestra el valor del encabezado Accept es application/json. La pestaña sin formato para la respuesta muestra el valor del encabezado Content-Type de aplicación/json.](formatting/_static/json-response-fiddler.png)

En esta captura de pantalla, puede ver la solicitud establece un encabezado de `Accept: application/json` y especifica la respuesta de la misma que su `Content-Type`. La `Author` objeto se muestra en el cuerpo de la respuesta, en formato JSON.

### <a name="forcing-a-particular-format"></a>Si fuerza un formato determinado

Si desea restringir los formatos de respuesta para una acción concreta puede, puede aplicar el `[Produces]` filtro. El `[Produces]` filtro especifica los formatos de respuesta para una acción específica (o controlador). Al igual que la mayoría [filtros](../controllers/filters.md), esto puede aplicarse a la acción, el controlador o el ámbito global.

```csharp
[Produces("application/json")]
public class AuthorsController
```

El `[Produces]` filtro forzará todas las acciones dentro de la `AuthorsController` para devolver las respuestas con formato JSON, incluso si otros formateadores estaban configurados para la aplicación y el cliente proporciona un `Accept` encabezado solicita diferentes, disponibles formato. Vea [filtros](../controllers/filters.md) para obtener más información, incluida la forma de aplicar filtros globales.

### <a name="special-case-formatters"></a>Formateadores de casos especiales

Algunos casos especiales se implementan mediante formateadores integrados. De forma predeterminada, `string` tipos de valor devuelto se formatearán como *texto/sin formato* (*texto/html* si se solicita a través de `Accept` encabezado). Este comportamiento se puede quitar mediante la eliminación de la `TextOutputFormatter`. Quitar formateadores en el `Configure` método *Startup.cs* (se muestra a continuación). Las acciones que tienen un objeto de modelo devuelven tipo no devolverá un 204 contenido respuesta al devolver `null`. Este comportamiento se puede quitar mediante la eliminación de la `HttpNoContentOutputFormatter`. El siguiente código quita la `TextOutputFormatter` y `HttpNoContentOutputFormatter`.

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

Sin el `TextOutputFormatter`, `string` devuelven tipos devuelven 406 No aceptable, por ejemplo. Tenga en cuenta que si existe un formateador XML, dará formato `string` tipos de valor devuelto si el `TextOutputFormatter` se quita.

Sin el `HttpNoContentOutputFormatter`, se da formato a objetos nulos utilizando el formateador configurado. Por ejemplo, el formateador JSON simplemente devuelven una respuesta con un cuerpo de `null`, mientras que el formateador XML devolverá un elemento XML vacío con el atributo `xsi:nil="true"` establecido.

## <a name="response-format-url-mappings"></a>Asignaciones de direcciones URL de formato de respuesta

Los clientes pueden solicitar un formato determinado como parte de la dirección URL, como en la cadena de consulta o una parte de la ruta de acceso, o mediante el uso de una extensión de archivo de formato específico, como .xml o .json. La asignación de ruta de acceso de solicitud debe especificarse en la ruta que es utilizando la API. Por ejemplo:

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

Esta ruta permitiría que el formato solicitado estar especificado como una extensión de archivo opcional. El `[FormatFilter]` atributo comprueba la existencia del valor de formato en el `RouteData` y asignará el formato de respuesta al formateador adecuado cuando se crea la respuesta.

| Ruta                      | Formateador                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | El formateador de salida predeterminado       |
| `/products/GetById/5.json` | El formateador JSON (si está configurado) |
| `/products/GetById/5.xml`  | El formateador XML (si está configurado)  |
