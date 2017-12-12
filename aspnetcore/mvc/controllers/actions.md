---
title: "Solicitudes de administración con los controladores de MVC de ASP.NET Core"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 07/03/2017
ms.topic: article
ms.assetid: 9da9eb52-8583-4069-af91-155ba3529d7f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/actions
ms.openlocfilehash: 5dc6c7dc70027bb79875f389d535119a2543b873
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="handling-requests-with-controllers-in-aspnet-core-mvc"></a>Solicitudes de administración con los controladores de MVC de ASP.NET Core

Por [Steve Smith](https://ardalis.com/) y [Scott Addie](https://github.com/scottaddie)

Controladores, acciones y resultados de la acción son una parte fundamental de cómo los desarrolladores crear aplicaciones con MVC de ASP.NET Core.

## <a name="what-is-a-controller"></a>¿Qué es un controlador?

Un controlador se usa para definir y agrupar un conjunto de acciones. Una acción (o *método de acción*) es un método en un controlador que administra las solicitudes. Controladores de agrupan lógicamente acciones similares. Esta agregación de acciones permite comunes conjuntos de reglas, como el enrutamiento, el almacenamiento en caché y autorización, que se aplicará de forma conjunta. Las solicitudes se asignan a las acciones a través de [enrutamiento](xref:mvc/controllers/routing).

Por convención, las clases de controlador:
* Residen en el nivel de raíz del proyecto *controladores* carpeta
* Heredar de`Microsoft.AspNetCore.Mvc.Controller`

Un controlador es una clase se pueden crear instancias en las que al menos una de las siguientes condiciones es true:
* El nombre de clase se utiliza como sufijo "Controller"
* La clase hereda de una clase cuyo nombre lleva el sufijo "Controller"
* La clase se decora con el `[Controller]` atributo

Una clase de controlador no debe tener asociado un `[NonController]` atributo.

Los controladores deben seguir la [principio de dependencias explícitas](http://deviq.com/explicit-dependencies-principle/). Existen dos enfoques para implementar este principio. Si varias acciones de controlador requieren el mismo servicio, considere la posibilidad de usar [inyección de constructor](xref:mvc/controllers/dependency-injection#constructor-injection) para solicitar esas dependencias. Si el servicio es necesario sólo un método de acción única, considere la posibilidad de usar [acción inyección](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) para solicitar la dependencia.

En el **M**odelo -**V**er -**C**ontroller patrón, un controlador es responsable de la creación de instancias del modelo y el procesamiento inicial de la solicitud. Por lo general, las decisiones empresariales deben realizarse dentro del modelo.

El controlador toma el resultado de que el modelo de procesamiento (si existe) y devuelve la vista correcta y sus datos de vista asociada o el resultado de la llamada API. Obtenga más información en [información general de ASP.NET MVC Core](xref:mvc/overview) y [Introducción a ASP.NET MVC de núcleo y Visual Studio](xref:tutorials/first-mvc-app/start-mvc).

El controlador es un *nivel de interfaz de usuario* abstracción. Sus responsabilidades son para asegurarse de datos de la solicitud están válidas y elegir qué vista (o el resultado de una API) se debe devolver. En las aplicaciones factorizadas correctamente, no incluyen directamente datos acceso o la lógica empresarial. En su lugar, el controlador se delega a estas responsabilidades de control de servicios.

## <a name="defining-actions"></a>Definir acciones

Métodos públicos en un controlador, excepto las decorada con el `[NonAction]` de atributo, se muestran algunas acciones. Parámetros de acciones están enlazados a datos de la solicitud y se validan mediante [enlace de modelo](xref:mvc/models/model-binding). Se produce la validación de modelo para todo lo que está enlazada a un modelo. La `ModelState.IsValid` valor de la propiedad indica si el enlace de modelos y la validación se realizó correctamente.

Métodos de acción deben contener lógica para asignar una solicitud a una entidad de negocio. Normalmente se deben representar problemas empresariales como los servicios que el controlador tiene acceso a través de [inyección de dependencia](xref:mvc/controllers/dependency-injection). Acciones, a continuación, asignan el resultado de la acción de negocios en un estado de aplicación.

Acciones pueden devolver nada, pero con frecuencia devolver una instancia de `IActionResult` (o `Task<IActionResult>` de los métodos asincrónicos) que genera una respuesta. El método de acción es responsable de elegir *qué tipo de respuesta*. Resultado de la acción *el responde*.

### <a name="controller-helper-methods"></a>Métodos de aplicación auxiliar de controlador

Controladores normalmente se heredan de [controlador](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller), aunque esto no es necesario. Derivar de `Controller` proporciona acceso a tres categorías de métodos auxiliares:

#### <a name="1-methods-resulting-in-an-empty-response-body"></a>1. Métodos resultante en un cuerpo de respuesta vacío

Ya no `Content-Type` encabezado de respuesta HTTP se incluye, puesto que el cuerpo de respuesta no tiene contenido para describir.

Hay dos tipos de resultado de esta categoría: redireccionamiento y código de estado HTTP.

* **Código de estado HTTP**

    Este tipo devuelve un código de estado HTTP. Dos métodos auxiliares de este tipo son `BadRequest`, `NotFound`, y `Ok`. Por ejemplo, `return BadRequest();` genera un código de 400 estado cuando se ejecuta. Cuando los métodos como `BadRequest`, `NotFound`, y `Ok` están sobrecargados, ya no se consideran los servicios de respuesta de código de estado HTTP, puesto que la negociación de contenido lleva a cabo.

* **Redirigir**

    Este tipo, devuelve una redirección a una acción o destino (mediante `Redirect`, `LocalRedirect`, `RedirectToAction`, o `RedirectToRoute`). Por ejemplo, `return RedirectToAction("Complete", new {id = 123});` redirige a `Complete`, pasando un objeto anónimo.

    El tipo de resultado de redirección difiere del tipo de código de estado HTTP principalmente en la adición de un `Location` encabezado de respuesta HTTP.

#### <a name="2-methods-resulting-in-a-non-empty-response-body-with-a-predefined-content-type"></a>2. Métodos resultante en el cuerpo de respuesta no vacía con un tipo de contenido predefinido

Mayoría de los métodos auxiliares de esta categoría incluye una `ContentType` propiedad, lo que le permite establecer el `Content-Type` encabezado de respuesta para describir el cuerpo de respuesta.

Hay dos tipos de resultado de esta categoría: [vista](xref:mvc/views/overview) y [respuesta con formato](xref:mvc/models/formatting).

* **Vista**

    Este tipo devuelve una vista que utiliza un modelo para representar HTML. Por ejemplo, `return View(customer);` pasa de un modelo a la vista de enlace de datos.

* **Respuesta con formato**

    Este tipo devuelve JSON o un formato de intercambio de datos similares para representar un objeto de una forma específica. Por ejemplo, `return Json(customer);` serializa el objeto proporcionado en formato JSON.
    
    Otros métodos comunes de este tipo son `File`, `PhysicalFile`, y `VirtualFile`. Por ejemplo, `return PhysicalFile(customerFilePath, "text/xml");` devuelve un archivo XML descrito por un `Content-Type` valor de encabezado de respuesta de "text/xml".

#### <a name="3-methods-resulting-in-a-non-empty-response-body-formatted-in-a-content-type-negotiated-with-the-client"></a>3. Métodos resultante en un cuerpo de respuesta no vacía con formato en un tipo de contenido que se negocian con el cliente

Esta categoría es mejor conocido como **negociación de contenido**. [Negociación de contenido](xref:mvc/models/formatting#content-negotiation) se aplica siempre que una acción devuelve un [ObjectResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.objectresult) tipo o algo distinto de un [IActionResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iactionresult) implementación. Una acción que devuelve no`IActionResult` implementación (por ejemplo, `object`) también devuelve una respuesta con formato.

Algunos métodos de aplicación auxiliar de este tipo incluyen `BadRequest`, `CreatedAtRoute`, y `Ok`. Algunos ejemplos de estos métodos son `return BadRequest(modelState);`, `return CreatedAtRoute("routename", values, newobject);`, y `return Ok(value);`, respectivamente. Tenga en cuenta que `BadRequest` y `Ok` realizan la negociación de contenido solo cuando se pasa un valor; sin tener que pasar un valor, en su lugar, actúan como tipos de resultado de código de estado HTTP. El `CreatedAtRoute` método, por otro lado, siempre realiza una negociación de contenido desde las sobrecargas todos requieren que se pasa un valor.

### <a name="cross-cutting-concerns"></a>Problemas de corte del cruce

Normalmente, las aplicaciones compartan partes de su flujo de trabajo. Por ejemplo, una aplicación que requiere autenticación para tener acceso a la cesta o una aplicación que almacena en caché datos en algunas páginas. Para llevar a cabo lógica antes o después de un método de acción, use un *filtro*. Usar [filtros](xref:mvc/controllers/filters) en cuestiones de corte del cruce puede reducir la duplicación, lo que les permite seguir el [principio no repita usted mismo (SECA)](http://deviq.com/don-t-repeat-yourself/).

Filtrar más los atributos, como `[Authorize]`, se puede aplicar en el nivel de controlador o acción según el nivel deseado de granularidad.

Control de errores y las respuestas en caché son a menudo preocupaciones transversales:
   * [Control de errores](xref:mvc/controllers/filters#exception-filters)
   * [Almacenamiento en caché de respuestas](xref:performance/caching/response)

Muchos problemas de corte del cruce pueden controlarse mediante filtros o personalizado [middleware](xref:fundamentals/middleware).
