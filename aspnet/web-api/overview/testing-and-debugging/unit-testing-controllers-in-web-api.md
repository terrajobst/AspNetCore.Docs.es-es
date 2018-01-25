---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Pruebas unitarias de controladores en ASP.NET Web API 2 | Documentos de Microsoft
author: MikeWasson
description: "En este tema se describe algunas técnicas específicas para pruebas unitarias de controladores en API Web 2. Antes de leer este tema, debe leer el tutorial unidad..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: bda5148a4c1553d70f3173de66371fbb8576e83f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a>Pruebas unitarias de controladores en ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

> En este tema se describe algunas técnicas específicas para pruebas unitarias de controladores en API Web 2. Antes de leer este tema, debe leer el tutorial [unidad pruebas ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), que muestra cómo agregar un proyecto de prueba unitaria a la solución.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Web API 2
> - [Moq](https://github.com/Moq) 4.5.30

> [!NOTE]
> He usado Moq, pero la misma idea se aplica a cualquier marco de simulación. Moq 4.5.30 (y versiones posteriores) es compatible con Visual Studio de 2017, Roslyn y .NET 4.5 y versiones posteriores.

Es un modelo frecuente en pruebas unitarias &quot;organizar-act-assert&quot;:

- Organizar: Configurar los requisitos previos para ejecutar la prueba.
- ACT: Realizar la prueba.
- Aserción: Compruebe que la prueba se realizó correctamente.

En el paso de organización, se usará con frecuencia ficticios u objetos de código auxiliar. Que minimiza el número de dependencias, por lo que la prueba se centró en probar algo.

Hay algunas cuestiones que debe prueba unitaria en los controladores de API Web:

- La acción devuelve el tipo correcto de respuesta.
- Parámetros no válidos devuelven la respuesta de error correcto.
- La acción llama al método correcto en el nivel de servicio o repositorio.
- Si la respuesta incluye un modelo de dominio, compruebe el tipo de modelo.

Estas son algunas de las cosas generales para probar, pero los detalles dependerán de la implementación de controlador. En concreto, supone una gran diferencia si se devuelven las acciones de controlador **HttpResponseMessage** o **IHttpActionResult**. Para obtener más información acerca de estos tipos de resultados, vea [resultados de la acción en Api Web 2](../getting-started-with-aspnet-web-api/action-results.md).

## <a name="testing-actions-that-return-httpresponsemessage"></a>Acciones de prueba que devuelven HttpResponseMessage

Este es un ejemplo de un controlador cuyo valor devuelto de acciones **HttpResponseMessage**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

Observe que el controlador usa inyección de dependencia para insertar un `IProductRepository`. Esto hace que el controlador puede probar, porque puede insertar un repositorio ficticio. La siguiente prueba unitaria comprueba que la `Get` método escribe un `Product` para el cuerpo de respuesta. Se asume que `repository` es un simulacro `IProductRepository`.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

Es importante establecer **solicitar** y **configuración** en el controlador. En caso contrario, se producirá un error la prueba con un **ArgumentNullException** o **InvalidOperationException**.

## <a name="testing-link-generation"></a>Pruebas de generación de vínculo

El `Post` llamadas al método **UrlHelper.Link** para crear vínculos en la respuesta. Esto requiere un poco más preparación en la prueba unitaria:

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

El **UrlHelper** clase necesita los datos de ruta y la dirección URL de solicitud, por lo que la prueba tiene que establecer valores para estos. Otra opción es ficticios o código auxiliar **UrlHelper**. Con este enfoque, reemplace el valor predeterminado de [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) con una versión de simulacro o código auxiliar que devuelve un valor fijo.

Vamos a volver a escribir la prueba mediante la [Moq](https://github.com/Moq) framework. Instalar el `Moq` paquete de NuGet en el proyecto de prueba.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

En esta versión, no es necesario configurar los datos de ruta, porque el simulacro **UrlHelper** devuelve una cadena de constante.


## <a name="testing-actions-that-return-ihttpactionresult"></a>Acciones de prueba que devuelven IHttpActionResult

En la API Web 2, puede devolver una acción de controlador **IHttpActionResult**, que es análogo a **ActionResult** en ASP.NET MVC. El **IHttpActionResult** interfaz define un modelo de comando para crear las respuestas HTTP. En lugar de crear directamente la respuesta, el controlador devuelve un **IHttpActionResult**. Más adelante, se invoca la canalización de la **IHttpActionResult** para crear la respuesta. Este enfoque resulta más fácil escribir pruebas unitarias, porque puede omitir una gran parte del programa de instalación que se necesita para **HttpResponseMessage**.

Este es un controlador de ejemplo cuya devolución de acciones **IHttpActionResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

En este ejemplo se muestra algunos modelos comunes mediante **IHttpActionResult**. Veamos cómo a unidad probarlas.

### <a name="action-returns-200-ok-with-a-response-body"></a>Acción devuelve 200 (OK) con un cuerpo de respuesta

El `Get` llamadas al método `Ok(product)` si se encuentra el producto. En la prueba unitaria, asegúrese de que el tipo de valor devuelto es **OkNegotiatedContentResult** y el producto devuelto tiene el identificador correcto.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

Tenga en cuenta que la prueba unitaria no ejecuta el resultado de acción. Se puede suponer que el resultado de acción crea la respuesta HTTP correctamente. (Por eso el marco Web API tiene sus propias pruebas unitarias!)

### <a name="action-returns-404-not-found"></a>Acción devuelve 404 (no encontrado)

El `Get` llamadas al método `NotFound()` si no se encuentra el producto. En este caso, la prueba unitaria solo comprueba si el tipo de valor devuelto es **NotFoundResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>Acción devuelve 200 (OK) sin ningún cuerpo de respuesta

El `Delete` llamadas al método `Ok()` para devolver una respuesta HTTP 200 vacía. Al igual que el ejemplo anterior, la prueba unitaria comprueba el tipo de valor devuelto, en este caso **OkResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>Acción devuelve 201 (creado) con un encabezado de ubicación

El `Post` llamadas al método `CreatedAtRoute` para devolver una respuesta HTTP 201 con un URI en el encabezado Location. En la prueba unitaria, compruebe que la acción establece los valores correctos de enrutamientos.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>Acción devuelve 2xx otra con un cuerpo de respuesta

El `Put` llamadas al método `Content` para devolver una respuesta HTTP 202 (aceptado) con un cuerpo de respuesta. En este caso es similar al devolver 200 (correcto), pero la prueba unitaria también debe comprobar el código de estado.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>Recursos adicionales

- [Simulación de Entity Framework cuando ASP.NET Web API 2 de pruebas unitarias](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [Escribir pruebas para un servicio ASP.NET Web API](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog de Youssef Moussaoui).
- [Depuración ASP.NET Web API con Route Debugger](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
