---
title: Aplicación de formato a datos de respuesta en ASP.NET Core Web API
author: ardalis
description: Obtenga información sobre cómo dar formato a datos de respuesta en ASP.NET Core Web API.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: web-api/advanced/formatting
ms.openlocfilehash: f25705bcd3a704de12ea24c3e196de1ba1afd492
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35217475"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a>Aplicación de formato a datos de respuesta en ASP.NET Core Web API

Por [Steve Smith](https://ardalis.com/)

ASP.NET Core MVC tiene compatibilidad integrada para dar formato a datos de respuesta, en función de formatos fijos o especificaciones del cliente.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="format-specific-action-results"></a>Resultados de acción específica de formato

Algunos tipos de resultado de acción son específicos de un formato determinado, como `JsonResult` y `ContentResult`. Las acciones pueden devolver resultados específicos a los que siempre se da formato de una manera determinada. Por ejemplo, si se devuelve un `JsonResult`, se devolverán datos con formato JSON, independientemente de las preferencias del cliente. Del mismo modo, si se devuelve un `ContentResult`, se devolverán datos de cadena con formato de texto sin formato (al igual que si se devuelve simplemente una cadena).

> [!NOTE]
> No se requieren acciones para devolver un tipo en particular, ya que MVC es compatible con cualquier valor devuelto por un objeto. Si una acción devuelve una implementación `IActionResult` y el controlador hereda de `Controller`, los desarrolladores disponen de diversos métodos auxiliares que se corresponden con muchas de las opciones. Los resultados de acciones que devuelven objetos que no son tipos `IActionResult` se serializarán con la implementación `IOutputFormatter` correspondiente.

Para devolver datos en un formato específico desde un controlador que hereda de la clase base `Controller`, use el método auxiliar integrado `Json` para devolver JSON y `Content` para texto sin formato. El método de acción debe devolver el tipo de resultado específico (por ejemplo, `JsonResult`) o bien `IActionResult`.

Para devolver datos con formato JSON:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

Respuesta de ejemplo de esta acción:

![Pestaña Red de Herramientas de desarrollo de Microsoft Edge en la que se muestra que el tipo de contenido de la respuesta es application/json](formatting/_static/json-response.png)

Tenga en cuenta que el tipo de contenido de la respuesta es `application/json`, y se muestra en la lista de solicitudes de red y en la sección Encabezados de respuesta. Observe también la lista de opciones que aparecen en el explorador (en este caso, Microsoft Edge) en el encabezado Accept de la sección Encabezados de solicitud. La técnica actual omite este encabezado; más adelante se describe su uso.

Para devolver datos con formato de texto sin formato, use `ContentResult` y el método auxiliar `Content`:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

Una respuesta de esta acción:

![Pestaña Red de Herramientas de desarrollo de Microsoft Edge en la que se muestra que el tipo de contenido de la respuesta es texto/sin formato](formatting/_static/text-response.png)

Observe que en este caso el `Content-Type` devuelto es `text/plain`. También puede obtener este mismo comportamiento mediante el uso de un tipo de respuesta de cadena:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> Para las acciones no triviales con varias opciones o tipos de valor devueltos (por ejemplo, códigos de estado HTTP diferentes en función del resultado de las operaciones realizadas), se recomienda el uso de `IActionResult` como tipo de valor devuelto.

## <a name="content-negotiation"></a>Negociación de contenido

La negociación de contenido (o *conneg*) se produce cuando el cliente especifica un [encabezado Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html). El formato predeterminado que usa ASP.NET Core MVC es JSON. La negociación de contenido se implementa mediante `ObjectResult`. También se integra en los resultados de acción específicos del código de estado devueltos por los métodos auxiliares (que se basan en `ObjectResult`). También puede devolver un tipo de modelo (es decir, una clase que haya definido como tipo de transferencia de datos) y el marco de trabajo lo ajustará automáticamente en un `ObjectResult`.

El siguiente método de acción usa los métodos auxiliares `Ok` y `NotFound`:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

Se devolverá una respuesta con formato JSON, a menos que se haya solicitado otro formato y el servidor pueda devolverlo. Puede usar una herramienta como [Fiddler](http://www.telerik.com/fiddler) para crear una solicitud que incluya un encabezado Accept y especifique otro formato. En ese caso, si el servidor tiene un *formateador* que puede producir una respuesta en el formato solicitado, el resultado se devolverá en el formato preferido por el cliente.

![Consola de Fiddler en la que se muestra una solicitud GET creada manualmente con el valor application/xml para el encabezado Accept](formatting/_static/fiddler-composer.png)

En la captura de pantalla anterior, se ha usado el compositor de Fiddler para generar una solicitud mediante la especificación de `Accept: application/xml`. ASP.NET Core MVC solo admite JSON de forma predeterminada, por lo que, incluso si se especifica otro formato, el resultado devuelto tendrá formato JSON. En la sección siguiente verá cómo agregar otros formateadores.

Las acciones del controlador pueden devolver POCO (objetos CLR estándar), en cuyo caso ASP.NET Core MVC crea automáticamente un `ObjectResult` que ajuste el objeto. El cliente obtendrá el objeto serializado con formato (el formato predeterminado es JSON, pero puede configurar XML u otros formatos). Si el objeto que se devuelve es `null`, el marco de trabajo devolverá una respuesta `204 No Content`.

Devolución de un tipo de objeto:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

En el ejemplo, una solicitud de un alias de autor válido recibirá una respuesta "200 - Correcto" con los datos del autor. Una solicitud de un alias no válido recibirá una respuesta "204 Sin contenido". A continuación se muestran capturas de pantalla en las que aparece la respuesta en formatos XML y JSON.

### <a name="content-negotiation-process"></a>Proceso de negociación de contenido

La *negociación* de contenido solo se lleva a cabo si en la solicitud aparece un encabezado `Accept`. Si una solicitud contiene un encabezado Accept, el marco de trabajo enumerará los tipos de medios en el encabezado Accept en orden de preferencia e intentará encontrar un formateador que pueda generar una respuesta en uno de los formatos especificados por dicho encabezado. En caso de que no se encuentre ningún formateador que satisfaga la solicitud del cliente, el marco de trabajo intentará encontrar el primer formateador que pueda generar una respuesta (a menos que el desarrollador haya configurado la opción en `MvcOptions` para devolver "406 - No aceptable"). Si la solicitud especifica XML pero no se ha configurado el formateador XML, se usará el formateador JSON. Por lo general, si no se ha configurado ningún formateador que proporcione el formato solicitado, se usará el primer formateador que pueda dar formato al objeto. Si no se especifica ningún encabezado, se usará para serializar la respuesta el primer formateador que pueda controlar el objeto que se va a devolver. En este caso no se produce ninguna negociación, ya que es el servidor el que determina qué formato usará.

> [!NOTE]
> Si el encabezado Accept contiene `*/*`, el encabezado se omitirá a menos que `RespectBrowserAcceptHeader` esté establecido en true en `MvcOptions`.

### <a name="browsers-and-content-negotiation"></a>Exploradores y negociación de contenido

A diferencia de los clientes de API típicos, los exploradores web suelen proporcionar encabezados `Accept` que incluyen una amplia gama de formatos, incluidos caracteres comodín. De forma predeterminada, cuando el marco de trabajo detecta que la solicitud procede de un explorador, omite el encabezado `Accept` y devuelve el contenido en el formato predeterminado configurado de la aplicación (JSON, a menos que se haya configurado otro). Esto proporciona una experiencia más coherente al usar exploradores diferentes para el consumo de API.

Si prefiere que la aplicación respete los encabezados Accept del explorador, puede incluirlo en la configuración de MVC mediante el establecimiento de `RespectBrowserAcceptHeader` en `true` en el método `ConfigureServices` de *Startup.cs*.

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a>Configuración de formateadores

Si la aplicación necesita admitir formatos adicionales además del formato JSON predeterminado, puede agregar paquetes NuGet y configurar MVC para que los admita. Hay formateadores independientes para la entrada y la salida. El [enlace de modelos](xref:mvc/models/model-binding) usa formateadores de entrada, mientras que los formateadores de salida se usan para dar formato a las respuestas. También puede configurar [formateadores personalizados](xref:web-api/advanced/custom-formatters).

### <a name="adding-xml-format-support"></a>Agregar compatibilidad con el formato XML

Para agregar compatibilidad con el formato XML, instale el paquete NuGet `Microsoft.AspNetCore.Mvc.Formatters.Xml`.

Agregue los formateadores serializadores de XML a la configuración de MVC en *Startup.cs*:

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

También puede agregar simplemente el formateador de salida:

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

Estos dos métodos serializarán los resultados con `System.Xml.Serialization.XmlSerializer`. Si lo prefiere, puede usar `System.Runtime.Serialization.DataContractSerializer` mediante la adición de su formateador asociado:

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

Una vez que se ha agregado compatibilidad con el formato XML, los métodos de controlador deben devolver el formato adecuado en función del encabezado `Accept` de la solicitud, como se muestra en este ejemplo de Fiddler:

![Consola de Fiddler: en la pestaña Sin formato de la solicitud se ve que el valor del encabezado Accept es application/xml. En la pestaña Sin formato de la respuesta se ve el valor application/xml del encabezado Content-Type.](formatting/_static/xml-response.png)

En la pestaña Inspectores puede ver que la solicitud GET sin formato se ha realizado con un conjunto de encabezados `Accept: application/xml`. En el panel de respuesta se muestra el encabezado `Content-Type: application/xml`, y el objeto `Author` se ha serializado a XML.

Use la pestaña Compositor para modificar la solicitud para especificar `application/json` en el encabezado `Accept`. Ejecute la solicitud y la respuesta se formateará como JSON:

![Consola de Fiddler: en la pestaña Sin formato de la solicitud se ve que el valor del encabezado Accept es application/json. En la pestaña Sin formato de la respuesta se ve el valor application/json del encabezado Content-Type.](formatting/_static/json-response-fiddler.png)

En esta captura de pantalla, puede ver que la solicitud establece un encabezado `Accept: application/json` y la respuesta lo especifica como su `Content-Type`. El objeto `Author` se muestra en el cuerpo de la respuesta, en formato JSON.

### <a name="forcing-a-particular-format"></a>Forzar un formato determinado

Si quiere restringir los formatos de respuesta para una acción concreta, aplique el filtro `[Produces]`. El filtro `[Produces]` especifica los formatos de respuesta para una acción o controlador específicos. Al igual que la mayoría de los [filtros](xref:mvc/controllers/filters), puede aplicarse a la acción, el controlador o el ámbito global.

```csharp
[Produces("application/json")]
public class AuthorsController
```

El filtro `[Produces]` obligará a que todas las acciones dentro de `AuthorsController` devuelvan respuestas con formato JSON, incluso si se han configurado otros formateadores para la aplicación y si el cliente ha proporcionado un encabezado `Accept` que solicita un formato diferente disponible. Vea [Filtros](xref:mvc/controllers/filters) para obtener más información, incluida la forma de aplicar filtros globales.

### <a name="special-case-formatters"></a>Formateadores de casos especiales

Algunos casos especiales se implementan mediante formateadores integrados. De forma predeterminada, los tipos de valor devueltos `string` se formatearán como *texto/sin formato* (*texto/html* si se solicita a través del encabezado `Accept`). Este comportamiento se puede quitar mediante la eliminación de `TextOutputFormatter`. Los formateadores deben quitarse del método `Configure` de *Startup.cs* (como se muestra a continuación). Las acciones que tienen un tipo de valor devuelto de objeto de modelo devolverán una respuesta "204 Sin contenido" al devolver `null`. Este comportamiento se puede quitar mediante la eliminación de `HttpNoContentOutputFormatter`. El código siguiente quita `TextOutputFormatter` y `HttpNoContentOutputFormatter`.

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

Sin `TextOutputFormatter`, los tipos de valor devueltos `string` devuelven "406 - No aceptable", por ejemplo. Tenga en cuenta que si existe un formateador XML, dará formato a los tipos de valor devueltos `string` si se quita `TextOutputFormatter`.

Sin `HttpNoContentOutputFormatter`, se da formato a los objetos nulos mediante el formateador configurado. Por ejemplo, el formateador JSON simplemente devolverá una respuesta con un cuerpo `null`, mientras que el formateador XML devolverá un elemento XML vacío con el atributo `xsi:nil="true"` establecido.

## <a name="response-format-url-mappings"></a>Asignaciones de direcciones URL de formato de respuesta

Los clientes pueden solicitar un formato determinado como parte de la dirección URL, como en la cadena de consulta o en una parte de la ruta de acceso, o mediante el uso de una extensión de archivo de formato específico, como .xml o .json. La asignación de la ruta de acceso de la solicitud debe especificarse en la ruta que use la API. Por ejemplo:

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

Esta ruta permitiría especificar el formato solicitado como una extensión de archivo opcional. El atributo `[FormatFilter]` comprueba la existencia del valor de formato en `RouteData` y asignará el formato de respuesta al formateador adecuado cuando se cree la respuesta.


|           Ruta            |             Formateador              |
|----------------------------|------------------------------------|
|   `/products/GetById/5`    |    Formateador de salida predeterminado    |
| `/products/GetById/5.json` | Formateador JSON (si está configurado) |
| `/products/GetById/5.xml`  | Formateador XML (si está configurado)  |

