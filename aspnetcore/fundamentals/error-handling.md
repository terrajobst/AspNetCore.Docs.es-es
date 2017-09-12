---
title: Control de errores en ASP.NET Core
author: ardalis
description: "Explica cómo controlar los errores en las aplicaciones de ASP.NET Core"
keywords: "Núcleo de ASP.NET, control de errores, control de excepciones"
ms.author: tdykstra
manager: wpickett
ms.date: 11/30/2016
ms.topic: article
ms.assetid: 4db51023-c8a6-4119-bbbe-3917e272c260
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/error-handling
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 96a4fed19887a7a9eba08ec70296147f22e41569
ms.sourcegitcommit: 368aabde4de3728a8e5a8c016a2ec61f9c0854bf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a>Introducción a control de errores en ASP.NET Core

Por [Steve Smith](https://ardalis.com/) y [Tom Dykstra](https://github.com/tdykstra/)

Este artículo tratan appoaches común para controlar los errores en las aplicaciones de ASP.NET Core.

[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample)

## <a name="the-developer-exception-page"></a>La página de excepción para desarrolladores

Para configurar una aplicación para mostrar una página que muestra información detallada sobre las excepciones, instale el `Microsoft.AspNetCore.Diagnostics` NuGet empaquetar y agregue una línea a la [Configurar método de la clase de inicio](startup.md):

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

Colocar `UseDeveloperExceptionPage` antes de cualquier middleware que desea detectar excepciones, como `app.UseMvc`.

>[!WARNING]
> Habilitar la página de excepción para desarrolladores **sólo cuando la aplicación se ejecuta en el entorno de desarrollo**. No desea compartir públicamente información de excepción detallada cuando se ejecute la aplicación en producción. [Más información acerca de cómo configurar entornos](environments.md).

Para ver la página de excepción para desarrolladores, ejecute la aplicación de ejemplo con el entorno establecido en `Development`y agregue `?throw=true` a la dirección URL base de la aplicación. La página incluye varias pestañas con información sobre la excepción y la solicitud. La primera ficha incluye un seguimiento de pila. 

![Seguimiento de la pila](error-handling/_static/developer-exception-page.png)

La ficha siguiente muestra la consulta de parámetros de cadena, si existe.

![Parámetros de cadena de consulta](error-handling/_static/developer-exception-page-query.png)

Esta solicitud no tenía ninguna cookie, pero si lo hiciera, aparecerían en el **Cookies** ficha. Puede ver los encabezados que se pasaron en la última ficha.

![Encabezados](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>Configurar una página de control de excepciones personalizadas

Es una buena idea para configurar una página de controlador de excepción que se usará cuando la aplicación no se está ejecutando el `Development` entorno.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

En una aplicación MVC, no explícitamente decorar el método de acción de controlador de errores con atributos del método HTTP, como `HttpGet`. Uso de verbos explícitos podría impedir que algunas solicitudes de alcanzar el método.

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a>Configuración de páginas de códigos de estado

De forma predeterminada, la aplicación no proporcionará una página de códigos de estado enriquecido para los códigos de estado HTTP como 500 (Error interno del servidor) o 404 (no encontrado). Puede configurar la `StatusCodePagesMiddleware` agregando una línea a la `Configure` método:

```csharp
app.UseStatusCodePages();
```

De forma predeterminada, este middleware agrega controladores simples, de sólo texto para los códigos de estado comunes, por ejemplo, 404:

![página 404](error-handling/_static/default-404-status-code.png)

El middleware admite varios métodos de extensión distinto. Una toma una expresión lambda, el otro toma una cadena de contenido del tipo y el formato.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

También hay métodos de extensión de redirección. Uno envía un código de 302 estado al cliente, y uno devuelve el código de estado original al cliente, pero también ejecuta el controlador para la dirección URL de redireccionamiento.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

Si tiene que deshabilitar páginas de códigos de estado para algunas solicitudes, puede hacerlo:

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a>Código de control de excepciones

Código de páginas de control de excepciones puede producir excepciones. A menudo resulta una buena idea de páginas de errores de producción para que se componga de contenido estático únicamente.

Además, tenga en cuenta que una vez que se ha enviado los encabezados de respuesta, no se puede cambiar el código de estado de la respuesta, ni puede ejecutar ningún controlador o páginas de excepción. La respuesta debe completarse o anuló la conexión.

## <a name="server-exception-handling"></a>Control de excepciones de servidor

Además de la lógica de la aplicación de control de excepciones del [server](servers/index.md) hospede su aplicación realiza algunas excepciones. Si el servidor detecta una excepción antes de que se envían los encabezados, el servidor envía una respuesta de Error de servidor interno 500 sin cuerpo. Si el servidor detecta una excepción después de han enviado los encabezados, el servidor cierra la conexión. El servidor controla las solicitudes no están administradas por la aplicación. Cualquier excepción que se está controlando de excepción del servidor de control. Cualquier configurar páginas de errores personalizadas o middleware de control de excepciones o filtros no afectan a este comportamiento.

## <a name="startup-exception-handling"></a>Control de excepciones de inicio

Solo el nivel de hospedaje puede controlar las excepciones que tienen lugar durante el inicio de la aplicación. También puede [configurar cómo se comporta el host en respuesta a los errores durante el inicio](hosting.md#detailed-errors) con `captureStartupErrors` y `detailedErrors` clave.

Hospedaje sólo puede mostrar una página de error para un error de inicio capturada si el error se produce después de enlace de dirección/puerto de host. Si se produce un error en cualquier enlace por cualquier motivo, el nivel de hospedaje registra una excepción crítica, el bloqueo del proceso de dotnet, y no se muestra ninguna página de error.

## <a name="aspnet-mvc-error-handling"></a>Control de errores de ASP.NET MVC

[MVC](../mvc/index.md) aplicaciones tienen algunas opciones adicionales para controlar los errores, como la configuración de filtros de excepciones y realizar la validación del modelo.

### <a name="exception-filters"></a>Filtros de excepciones

Filtros de excepciones se pueden configurar globalmente o según una por controlador o por cada acción en una aplicación MVC. Estos filtros administrar cualquier excepción no controlada que se produce durante la ejecución de una acción de controlador o de otro filtro y no se llaman en caso contrario. Más información acerca de los filtros de excepción en [filtros](../mvc/controllers/filters.md).

>[!TIP]
> Los filtros de excepción son buenos para la captura de excepciones que se producen dentro de las acciones de MVC, pero no son lo más flexibles middleware de control de errores. Prefiere middleware para el caso general y usar filtros sólo donde es necesario para realizar el control de errores *diferente* en función de qué acción de MVC se ha elegido.

### <a name="handling-model-state-errors"></a>Estado del modelo de control de errores

[Validación de modelos](../mvc/models/validation.md) se produce antes de cada acción de controlador que se invoca, y es responsabilidad del método de acción para inspeccionar `ModelState.IsValid` y reaccionar de manera apropiada.

Algunas aplicaciones opte por seguir una convención estándar para tratar los errores de validación del modelo, en cuyo caso una [filtro](../mvc/controllers/filters.md) puede ser un lugar adecuado para implementar esta directiva. Debe probar cómo se comportan las acciones con los Estados del modelo no válido. Obtenga más información en [para probar la lógica de controlador](../mvc/controllers/testing.md).



