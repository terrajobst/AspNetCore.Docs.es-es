---
title: Control de errores en ASP.NET Core
author: ardalis
description: "Descubra cómo controlar errores en aplicaciones ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/error-handling
ms.openlocfilehash: 5b0cda7b79b8a9523d1ba6a9b321d22d3ccc753a
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a>Introducción al control de errores en ASP.NET Core

Por [Steve Smith](https://ardalis.com/) y [Tom Dykstra](https://github.com/tdykstra/)

Este artículo trata sobre los métodos comunes para controlar errores en aplicaciones ASP.NET Core.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="the-developer-exception-page"></a>Página de excepciones para el desarrollador

Para configurar una aplicación de modo que muestre una página con información detallada sobre las excepciones, instale el paquete NuGet `Microsoft.AspNetCore.Diagnostics` y agregue una línea al [método Configure de la clase Startup](startup.md):

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

Coloque `UseDeveloperExceptionPage` antes del software intermedio en el que quiera capturar excepciones, como `app.UseMvc`.

>[!WARNING]
> Habilite la página de excepciones para el desarrollador **solo cuando la aplicación se ejecute en el entorno de desarrollo**. No le interesa compartir públicamente información detallada sobre las excepciones cuando la aplicación se ejecute en producción. [Más información sobre la configuración de entornos](environments.md).

Para ver la página de excepciones para el desarrollador, ejecute la aplicación de ejemplo con el entorno establecido en `Development` y agregue `?throw=true` a la URL base de la aplicación. La página incluye varias pestañas con información sobre la excepción y la solicitud. La primera pestaña incluye un seguimiento de la pila. 

![Seguimiento de la pila](error-handling/_static/developer-exception-page.png)

En la pestaña siguiente se muestran los parámetros de cadena de consulta, si los hay.

![Parámetros de cadena de consulta](error-handling/_static/developer-exception-page-query.png)

Esta solicitud no tenía cookies, pero en caso de que las tuviera, aparecerían en la pestaña **Cookies**. Puede ver los encabezados que se han pasado en la última pestaña.

![Encabezados](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>Configuración de una página personalizada de control de excepciones

Se recomienda configurar una página de controlador de excepciones para usarla cuando la aplicación no se ejecute en el entorno `Development`.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

En una aplicación MVC, no decore explícitamente el método de acción del controlador de errores con atributos de método HTTP, como `HttpGet`. El uso de verbos explícitos podría impedir que algunas solicitudes llegasen al método.

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a>Configuración de páginas de códigos de estado

De manera predeterminada, la aplicación no proporciona una página de códigos de estado enriquecida para códigos de estado HTTP como 500 (Error interno del servidor) o 404 (No encontrado). Para configurar `StatusCodePagesMiddleware`, agregue una línea al método `Configure`:

```csharp
app.UseStatusCodePages();
```

De forma predeterminada, este software intermedio agrega controladores simples de solo texto para códigos de estado comunes, como 404:

![Página 404](error-handling/_static/default-404-status-code.png)

El software intermedio admite diversos métodos de extensión. Uno toma una expresión lambda, el otro toma un tipo de contenido y una cadena de formato.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

También hay métodos de extensión de redireccionamiento. Uno envía un código de estado 302 al cliente, mientras que el otro devuelve el código de estado original al cliente, pero también ejecuta el controlador para la dirección URL de redireccionamiento.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

Si necesita deshabilitar las páginas de códigos de estado para algunas solicitudes, puede hacerlo de la manera siguiente:

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a>Código de control de excepciones

El código de las páginas de control de excepciones puede producir excepciones. Es recomendable que las páginas de errores de producción incluyan únicamente contenido estático.

Además, tenga en cuenta que una vez que se hayan enviado los encabezados de una respuesta, no se podrá cambiar el código de estado de la respuesta ni se podrá ejecutar ninguna página de excepción o controlador. Deberá completarse la respuesta o anularse la conexión.

## <a name="server-exception-handling"></a>Control de excepciones del servidor

Además de la lógica de control de excepciones de la aplicación, el [servidor](servers/index.md) que hospede la aplicación llevará a cabo el control de algunas excepciones. Si el servidor detecta una excepción antes de que se envíen los encabezados, enviará una respuesta "500 Error interno del servidor" sin cuerpo. Si el servidor detecta una excepción después de que se hayan enviado los encabezados, cerrará la conexión. El servidor controla las solicitudes que no controla la aplicación. El control de excepciones del servidor controla todas las excepciones que se producen. Las páginas de error personalizadas y el software intermedio o filtros de control de excepciones que se hayan configurado no afectarán a este comportamiento.

## <a name="startup-exception-handling"></a>Control de excepciones de inicio

Solo el nivel de hospedaje puede controlar las excepciones que tienen lugar durante el inicio de la aplicación. También puede [configurar cómo se comporta el host en respuesta a los errores durante el inicio](hosting.md#detailed-errors) con `captureStartupErrors` y la clave `detailedErrors`.

El hospedaje solo puede mostrar una página de error para un error de inicio capturado si este se produce después del enlace de puerto/dirección del host. Si se produce un error en algún enlace por cualquier motivo, el nivel de hospedaje registra una excepción crítica, el proceso de dotnet se bloquea y no se muestra ninguna página de error.

## <a name="aspnet-mvc-error-handling"></a>Control de errores de ASP.NET MVC

Las aplicaciones [MVC](xref:mvc/overview) tienen algunas opciones adicionales para controlar errores, como la configuración de filtros de excepciones y la realización de la validación del modelo.

### <a name="exception-filters"></a>Filtros de excepciones

Los filtros de excepciones se pueden configurar globalmente, o bien por controlador o por acción, en una aplicación MVC. Estos filtros controlan todas las excepciones no controladas que se hayan producido durante la ejecución de una acción de controlador o de otro filtro; en caso contrario, no se les llama. Encontrará más información sobre los filtros de excepciones en [Filtros](../mvc/controllers/filters.md).

>[!TIP]
> Los filtros de excepciones están indicados para capturar las excepciones que se producen en las acciones de MVC, pero no son tan flexibles como el software intermedio de control de errores. Use de preferencia el software intermedio para los casos generales y aplique filtros solo cuando deba realizar el control de errores *de manera diferente* según la acción de MVC elegida.

### <a name="handling-model-state-errors"></a>Control de errores de estado del modelo

La [validación de modelos](../mvc/models/validation.md) se produce antes de invocar cada acción de controlador, y es el método de acción el encargado de inspeccionar `ModelState.IsValid` y reaccionar de manera apropiada.

Algunas aplicaciones optarán por seguir una convención estándar para tratar los errores de validación de modelos, en cuyo caso un [filtro](../mvc/controllers/filters.md) podría ser el lugar adecuado para implementar esta directiva. Debe probar cómo se comportan las acciones con estados de modelo no válidos. Obtenga más información en [Probar la lógica del controlador](../mvc/controllers/testing.md).



