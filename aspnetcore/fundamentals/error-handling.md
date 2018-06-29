---
title: Controlar errores en ASP.NET Core
author: ardalis
description: Descubra cómo controlar errores en aplicaciones ASP.NET Core.
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
uid: fundamentals/error-handling
ms.openlocfilehash: 2fe46ecc32d61a7fafb2ad6e2a35456476608251
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273714"
---
# <a name="handle-errors-in-aspnet-core"></a>Controlar errores en ASP.NET Core

Por [Steve Smith](https://ardalis.com/) y [Tom Dykstra](https://github.com/tdykstra/)

Este artículo trata sobre los métodos comunes para controlar errores en aplicaciones ASP.NET Core.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="the-developer-exception-page"></a>Página de excepciones para el desarrollador

Para configurar una aplicación de modo que muestre una página con información detallada sobre las excepciones, instale el paquete NuGet `Microsoft.AspNetCore.Diagnostics` y agregue una línea al [método Configure de la clase Startup](xref:fundamentals/startup):

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

Coloque `UseDeveloperExceptionPage` antes del software intermedio en el que quiera capturar excepciones, como `app.UseMvc`.

>[!WARNING]
> Habilite la página de excepciones para el desarrollador **solo cuando la aplicación se ejecute en el entorno de desarrollo**. No le interesa compartir públicamente información detallada sobre las excepciones cuando la aplicación se ejecute en producción. [Más información sobre la configuración de entornos](xref:fundamentals/environments).

Para ver la página de excepciones para el desarrollador, ejecute la aplicación de ejemplo con el entorno establecido en `Development` y agregue `?throw=true` a la URL base de la aplicación. La página incluye varias pestañas con información sobre la excepción y la solicitud. La primera pestaña incluye un seguimiento de la pila. 

![Seguimiento de la pila](error-handling/_static/developer-exception-page.png)

En la pestaña siguiente se muestran los parámetros de cadena de consulta, si los hay.

![Parámetros de cadena de consulta](error-handling/_static/developer-exception-page-query.png)

Esta solicitud no tenía cookies, pero en caso de que las tuviera, aparecerían en la pestaña **Cookies**. Puede ver los encabezados que se han pasado en la última pestaña.

![Encabezados](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>Configuración de una página personalizada de control de excepciones

Configure una página de controlador de excepciones para usarla cuando la aplicación no se ejecute en el entorno `Development`.

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

En una aplicación de Razor Pages, la plantilla de Razor Pages [dotnet new](/dotnet/core/tools/dotnet-new) facilita una página de error y una clase de modelo de página `ErrorModel` en la carpeta *Pages*.

En una aplicación MVC, no decore el método de acción del controlador de errores con atributos de método HTTP, como `HttpGet`. Los verbos explícitos impiden que algunas solicitudes lleguen al método. Permita el acceso anónimo al método para que los usuarios no autenticados puedan recibir y ver el error.

Por ejemplo, la plantilla de MVC [dotnet new](/dotnet/core/tools/dotnet-new) proporciona el siguiente método de controlador de errores y aparece en el controlador Home:

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a>Configuración de páginas de códigos de estado

Una aplicación no proporciona de forma predeterminada una página de códigos de estado enriquecida para códigos de estado HTTP como *404 No encontrado*. Para proporcionarlas, configure el middleware de páginas de código de estado agregando una línea al método `Startup.Configure`:

```csharp
app.UseStatusCodePages();
```

El middleware de páginas de código de estado agrega de forma predeterminada controladores simples de solo texto para códigos de estado comunes, como 404:

![Página 404](error-handling/_static/default-404-status-code.png)

El middleware admite diversos métodos de extensión. Uno de ellos es una expresión lambda:

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

Otro toma un tipo de contenido y una cadena de formato:

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

También hay métodos de extensión de redireccionamiento y de nueva ejecución. El método de redireccionamiento envía al cliente un código de estado 302:

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

El método de nueva ejecución devuelve el código de estado original al cliente, pero también ejecuta el controlador para la dirección URL de redireccionamiento:

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

Las páginas de códigos de estado se pueden deshabilitar en solicitudes específicas en un método de controlador de páginas de Razor o en un controlador MVC. Para deshabilitar páginas de códigos de estado, intente recuperar [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) de la colección [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) de la solicitud y deshabilite la característica si está disponible:

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

Si se usa una sobrecarga `UseStatusCodePages*` que apunta a un punto de conexión dentro de la aplicación, cree una vista de MVC o una página de Razor Pages para ese punto de conexión. Por ejemplo, la plantilla [dotnet new](/dotnet/core/tools/dotnet-new) de una aplicación de Razor Pages genera la página y la clase de modelo de página siguientes:

*Error.cshtml*:

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to <strong>Development</strong> environment will display more detailed information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications</strong>, as it can result in sensitive information from exceptions being displayed to end users. For local debugging, development environment can be enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to <strong>Development</strong>, and restarting the application.
</p>
```

*Error.cshtml.cs*:

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

## <a name="exception-handling-code"></a>Código de control de excepciones

El código de las páginas de control de excepciones puede producir excepciones. Es recomendable que las páginas de errores de producción incluyan únicamente contenido estático.

Además, tenga en cuenta que una vez que se hayan enviado los encabezados de una respuesta, no se podrá cambiar el código de estado de la respuesta ni se podrá ejecutar ninguna página de excepción o controlador. Deberá completarse la respuesta o anularse la conexión.

## <a name="server-exception-handling"></a>Control de excepciones del servidor

Además de la lógica de control de excepciones de la aplicación, el [servidor](xref:fundamentals/servers/index) que hospede la aplicación llevará a cabo el control de algunas excepciones. Si el servidor detecta una excepción antes de que se envíen los encabezados, enviará una respuesta *500 Error interno del servidor* sin cuerpo. Si el servidor detecta una excepción después de que se hayan enviado los encabezados, cerrará la conexión. El servidor controla las solicitudes que no controla la aplicación. El control de excepciones del servidor controla todas las excepciones que se producen. Las páginas de error personalizadas y el software intermedio o filtros de control de excepciones que se hayan configurado no afectarán a este comportamiento.

## <a name="startup-exception-handling"></a>Control de excepciones de inicio

Solo el nivel de hospedaje puede controlar las excepciones que tienen lugar durante el inicio de la aplicación. Mediante el [host de web](xref:fundamentals/host/web-host) también puede [configurar cómo se comporta el host en respuesta a los errores durante el inicio](xref:fundamentals/host/web-host#detailed-errors) con las claves `captureStartupErrors` y `detailedErrors`.

El hospedaje solo puede mostrar una página de error para un error de inicio capturado si este se produce después del enlace de puerto/dirección del host. Si se produce un error en algún enlace por cualquier motivo, el nivel de hospedaje registra una excepción crítica, el proceso de dotnet se bloquea y no se muestra ninguna página de error si la aplicación se está ejecutando en el servidor de [Kestrel](xref:fundamentals/servers/kestrel).

Si se ejecuta en [IIS](/iis) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) devuelve un *error de proceso 502.5* en caso de que el proceso no se pueda iniciar. Siga los consejos para solucionar problemas del tema [Troubleshoot ASP.NET Core on IIS](xref:host-and-deploy/iis/troubleshoot) (Solución de problemas de ASP.NET Core en IIS).

## <a name="aspnet-mvc-error-handling"></a>Control de errores de ASP.NET MVC

Las aplicaciones [MVC](xref:mvc/overview) tienen algunas opciones adicionales para controlar errores, como la configuración de filtros de excepciones y la realización de la validación del modelo.

### <a name="exception-filters"></a>Filtros de excepciones

Los filtros de excepciones se pueden configurar globalmente, o bien por controlador o por acción, en una aplicación MVC. Estos filtros controlan todas las excepciones no controladas que se hayan producido durante la ejecución de una acción de controlador o de otro filtro; en caso contrario, no se les llama. Encontrará más información sobre los filtros de excepciones en [Filtros](xref:mvc/controllers/filters).

> [!TIP]
> Los filtros de excepciones están indicados para capturar las excepciones que se producen en las acciones de MVC, pero no son tan flexibles como el software intermedio de control de errores. Use de preferencia el software intermedio para los casos generales y aplique filtros solo cuando deba realizar el control de errores *de manera diferente* según la acción de MVC elegida.

### <a name="handling-model-state-errors"></a>Control de errores de estado del modelo

La [validación de modelos](xref:mvc/models/validation) se produce antes de invocar cada acción de controlador, y es el método de acción el encargado de inspeccionar `ModelState.IsValid` y reaccionar de manera apropiada.

Algunas aplicaciones optarán por seguir una convención estándar para tratar los errores de validación de modelos, en cuyo caso un [filtro](xref:mvc/controllers/filters) podría ser el lugar adecuado para implementar esta directiva. Debe probar cómo se comportan las acciones con estados de modelo no válidos. Obtenga más información en [Probar la lógica del controlador](xref:mvc/controllers/testing).
