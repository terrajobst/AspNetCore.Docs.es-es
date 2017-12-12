---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: "Enviar datos de formulario HTML en ASP.NET Web API: datos de formulario codificación | Documentos de Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/15/2012
ms.topic: article
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0ed339c4f9d5854ab5a21cdd077a4d494987101f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>Enviar datos de formulario HTML en ASP.NET Web API: datos de codificación de formulario
====================
por [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>Parte 1: Datos de codificación de formulario

Este artículo muestra cómo publicar datos de codificación de formulario en un controlador de Web API.

- [Información general sobre formularios HTML](#overview_of_html_forms)
- [Enviar tipos complejos](#sending_complex_types)
- [Enviar datos de formulario a través de AJAX](#sending_form_data_via_ajax)
- [Enviar tipos simples](#sending_simple_types)

> [!NOTE]
> [Descargar el proyecto completado](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>Información general sobre formularios HTML

Uso de formularios HTML o GET o POST para enviar datos al servidor. El **método** atributo de la **formulario** elemento ofrece el método HTTP:

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

El método predeterminado es GET. Si el formulario usa GET, el formulario de datos se codifican en el URI como una cadena de consulta. Si el formulario usa POST, los datos del formulario se colocan en el cuerpo de solicitud. Para el envío de los datos, el **enctype** atributo especifica el formato del cuerpo de la solicitud:

| Enctype | Descripción |
| --- | --- |
| application/x-www-form-urlencoded | Datos de formulario se codifican como pares de nombre/valor, similares a una cadena de consulta URI. Este es el formato predeterminado de POST. |
| varias partes/de datos de formulario | Datos del formulario se codifican como un mensaje MIME de varias partes. Utilice este formato si va a cargar un archivo en el servidor. |

Parte 1 de este artículo se examina formato x--www-form-urlencoded. [Parte 2](sending-html-form-data-part-2.md) describe MIME de varias partes.

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>Enviar tipos complejos

Por lo general, se enviará un tipo complejo, formado por valores extraídos de varios controles de formulario. Tenga en cuenta el siguiente modelo que representa una actualización de estado:

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

Este es un controlador de API Web que acepta un `Update` objeto a través de POST.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> Usa este controlador [enrutamiento basado en la acción](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), por lo que la plantilla de ruta &quot;api / {controller} / {action} / {id}&quot;. El cliente registra los datos a &quot;/api/updates/complex&quot;.


Ahora vamos a escribir un formulario HTML para que los usuarios enviar una actualización de estado.

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

Tenga en cuenta que la **acción** atributo en el formulario es el URI de la acción del controlador. Este es el formulario con algunos valores escritos en:

![](sending-html-form-data-part-1/_static/image1.png)

Cuando el usuario hace clic en enviar, el explorador envía una solicitud HTTP similar al siguiente:

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

Observe que el cuerpo de solicitud contiene los datos del formulario, con formato de pares nombre/valor. API Web convierte automáticamente los pares nombre/valor en una instancia de la `Update` clase.

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>Enviar datos de formulario a través de AJAX

Cuando un usuario envía un formulario, el explorador se desplaza fuera de la página actual y representa el cuerpo del mensaje de respuesta. Eso está bien cuando la respuesta es una página HTML. Con una API web, sin embargo, el cuerpo de respuesta sea normalmente vacía o contiene datos estructurados, como JSON. En ese caso, tiene más sentido para enviar los datos del formulario usando una AJAX solicitan, para que la página puede procesar la respuesta.

El código siguiente muestra cómo enviar datos del formulario mediante jQuery.

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

JQuery **enviar** función reemplaza la acción de formulario con una nueva función. Esto invalida el comportamiento predeterminado del botón Enviar. El **serializar** función serializa los datos del formulario en pares nombre/valor. Para enviar los datos del formulario al servidor, llame a `$.post()`.

Cuando se completa la solicitud, el `.success()` o `.error()` controlador muestra un mensaje adecuado para el usuario.

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>Enviar tipos simples

En las secciones anteriores, se envía un tipo complejo, que API Web deserializa una instancia de una clase de modelo. También puede enviar tipos simples, como una cadena.

> [!NOTE]
> Antes de enviar un tipo simple, considere la posibilidad de ajustar el valor en un tipo complejo en su lugar. Esto ofrece las ventajas de validación del modelo en el servidor y hace que sea más fácil ampliar su modelo si es necesario.


Los pasos básicos para enviar un tipo simple son los mismos, pero hay dos diferencias sutiles. En primer lugar, en el controlador, debe decorar con el nombre del parámetro con el **FromBody** atributo.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

De forma predeterminada, API Web intenta obtener tipos simples URI de la solicitud. El **FromBody** atributo indica a API Web para leer el valor desde el cuerpo de solicitud.

> [!NOTE]
> API Web lee el cuerpo de respuesta a lo sumo una vez, por lo que solo un parámetro de una acción puede proceder de cuerpo de la solicitud. Si necesita obtener varios valores desde el cuerpo de solicitud, defina un tipo complejo.


En segundo lugar, el cliente debe enviar el valor con el formato siguiente:

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

En concreto, la parte del nombre del par nombre/valor debe estar vacía para un tipo simple. No todos los exploradores admiten por formularios HTML, pero crear este formato en secuencias de comandos como se indica a continuación:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

Este es un formulario de ejemplo:

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

Y este es el script para enviar el valor de formulario. La única diferencia de la secuencia de comandos anterior es el argumento pasado a la **registrar** función.

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

Puede utilizar el mismo enfoque para enviar una matriz de tipos simples:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>Recursos adicionales

[Parte 2: Cargar el archivo y MIME de varias partes](sending-html-form-data-part-2.md)
