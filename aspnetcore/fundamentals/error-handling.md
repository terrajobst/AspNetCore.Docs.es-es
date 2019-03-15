---
title: Controlar errores en ASP.NET Core
author: tdykstra
description: Descubra cómo controlar errores en aplicaciones ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/05/2019
uid: fundamentals/error-handling
ms.openlocfilehash: d809c70b3fae6b2d21d5ec0871298d905b873d5d
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665368"
---
# <a name="handle-errors-in-aspnet-core"></a>Controlar errores en ASP.NET Core

[Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex) y [Steve Smith](https://ardalis.com/)

Este artículo trata sobre los métodos comunes para controlar errores en aplicaciones ASP.NET Core.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="developer-exception-page"></a>Página de excepciones para el desarrollador

Para configurar una aplicación de modo que muestre una página con información detallada sobre las excepciones de solicitud, use la *página de excepciones para desarrolladores*. La página está disponible mediante el paquete [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/), a su vez disponible en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Agregue una línea al método `Startup.Configure` cuando la aplicación se ejecuta en el [entorno](xref:fundamentals/environments) de desarrollo:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseDeveloperExceptionPage)]

Realice una llamada a <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> delante de cualquier middleware donde quiera capturar excepciones.

> [!WARNING]
> Habilite la página de excepciones para el desarrollador **solo cuando la aplicación se ejecute en el entorno de desarrollo**. No le interesa compartir públicamente información detallada sobre las excepciones cuando la aplicación se ejecute en producción. Para más información sobre la configuración de entornos, consulte <xref:fundamentals/environments>.

Para ver la página de excepciones para el desarrollador, ejecute la aplicación de ejemplo con el entorno establecido en `Development` y agregue `?throw=true` a la URL base de la aplicación. La página incluye la siguiente información sobre la excepción y la solicitud:

* Seguimiento de la pila
* Parámetros de cadena de consulta (si existen)
* Cookies (si existen)
* Encabezados

## <a name="configure-a-custom-exception-handling-page"></a>Configurar una página personalizada de control de excepciones

Cuando la aplicación no se ejecute en el entorno de desarrollo, llame al método de extensión <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> para agregar middleware de control de excepciones. El middleware:

* Captura las excepciones.
* Registra las excepciones.
* Vuelve a ejecutar la solicitud en una canalización alternativa correspondiente a la página o el controlador indicados. La solicitud no se vuelve a ejecutar si se ha iniciado la respuesta.

En el ejemplo siguiente de la aplicación de ejemplo, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> agrega middleware de control de excepciones en entornos que no son de desarrollo. El método de extensión especifica una página de error o un controlador en el punto de conexión `/Error` para las solicitudes ejecutadas de nuevo después de que se capturan y registran excepciones:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseExceptionHandler1)]

La plantilla de aplicación de Razor Pages proporciona una página de error (*.cshtml*) y una clase <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> (`ErrorModel`) en la carpeta Pages.

En una aplicación MVC, se incluye el siguiente método del controlador de errores en la plantilla de aplicación MVC y aparece en el controlador principal:

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

No decore el método de acción del controlador de errores con atributos de método HTTP, como `HttpGet`. Los verbos explícitos impiden que algunas solicitudes lleguen al método. Permita el acceso anónimo al método para que los usuarios no autenticados puedan recibir y ver el error.

## <a name="access-the-exception"></a>Acceso a la excepción

Use <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> para acceder a la ruta de acceso a la solicitud original o a la excepción en un controlador o página:

* La ruta de acceso está disponible en la propiedad <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature.Path>.
* Lea <xref:System.Exception?displayProperty=fullName> de la propiedad [IExceptionHandlerFeature.Error](xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature.Error) heredada.

```csharp
// using Microsoft.AspNetCore.Diagnostics;

var exceptionHandlerPathFeature = 
    HttpContext.Features.Get<IExceptionHandlerPathFeature>();
var path = exceptionHandlerPathFeature?.Path;
var error = exceptionHandlerPathFeature?.Error;
```

> [!WARNING]
> **No** proporcione información de errores confidencial de <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> o <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> a los clientes. Proporcionar información de los errores es un riesgo para la seguridad.

## <a name="configure-custom-exception-handling-code"></a>Configuración del código personalizado de control de excepciones

Una alternativa a proporcionar un punto de conexión para los errores con una [página personalizada de control de excepciones](#configure-a-custom-exception-handling-page) es proporcionar una función lambda a <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>. Usar una función lambda con <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> permite acceder al error antes de devolver la respuesta.

La aplicación de ejemplo muestra el código personalizado de control de excepciones en `Startup.Configure`. Desencadene una excepción con el vínculo **Iniciar excepción** en la página de índice. Se ejecuta esta función lambda:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseExceptionHandler2)]

> [!WARNING]
> **No** proporcione información de errores confidencial de <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> o <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> a los clientes. Proporcionar información de los errores es un riesgo para la seguridad.

## <a name="configure-status-code-pages"></a>Configurar páginas de códigos de estado

Una aplicación ASP.NET Core no proporciona de forma predeterminada una página de códigos de estado para los códigos de estado HTTP, como *404 - No encontrado*. La aplicación devuelve un código de estado y un cuerpo de respuesta vacío. Para proporcionar páginas de códigos de estado, use el middleware de páginas de código de estado.

El middleware está disponible mediante el paquete [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/), a su vez disponible en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Agregue una línea al método `Startup.Configure`:

```csharp
app.UseStatusCodePages();
```

Llame al método <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> antes del middleware de control de solicitudes (por ejemplo, middleware de archivos estáticos y middleware de MVC).

El middleware de páginas de códigos de estado agrega de forma predeterminada controladores de solo texto para los códigos de estado comunes, como *404 - No encontrado*:

```
Status Code: 404; Not Found
```

El middleware admite varios métodos de extensión que le permiten personalizar su comportamiento.

Una sobrecarga de <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> toma una expresión lambda, que se puede usar para procesar la lógica personalizada de control de errores y escribir manualmente la respuesta:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

Una sobrecarga de <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> toma una cadena de tipo de contenido y formato, que se puede usar para personalizar el tipo de contenido y el texto de respuesta:

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

### <a name="redirect-and-re-execute-extension-methods"></a>Métodos de extensión de redirección y nueva ejecución

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:

* Envía un código de estado *302 - Encontrado* al cliente.
* Redirige al cliente a la ubicación proporcionada en la plantilla de dirección URL.

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> se usa normalmente cuando la aplicación:

* Debe redirigir al cliente a un punto de conexión diferente, normalmente en casos en los que una aplicación diferente procesa el error. En el caso de aplicaciones web, la barra de direcciones del explorador del cliente refleja el punto de conexión redirigido.
* No debe conservar ni devolver el código de estado original con la respuesta de redirección inicial.

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:

* Devuelve el código de estado original al cliente.
* Genera el cuerpo de respuesta, para lo cual vuelve a ejecutar la canalización de solicitud mediante una ruta de acceso alternativa.

```csharp
app.UseStatusCodePagesWithReExecute("/Error/{0}");
```

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> se usa normalmente cuando la aplicación debe:

* Procesar la solicitud sin redirigirla a un punto de conexión diferente. En el caso de aplicaciones web, la barra de direcciones del explorador del cliente refleja el punto de conexión solicitado originalmente.
* Conservar y devolver el código de estado original con la respuesta.

Las plantillas pueden incluir un marcador de posición (`{0}`) relativo al código de estado. La plantilla debe empezar con una barra diagonal (`/`). Cuando se use un marcador de posición, confirme que el punto de conexión (página o controlador) puede procesar el segmento de línea. Por ejemplo, una página de errores de Razor debe aceptar el valor del segmento de línea opcional con la directiva `@page`:

```cshtml
@page "{code?}"
```

Las páginas de códigos de estado se pueden deshabilitar en solicitudes específicas en un método de controlador de páginas de Razor o en un controlador MVC. Para deshabilitar páginas de códigos de estado, intente recuperar <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> de la colección [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) de la solicitud y deshabilite la característica si está disponible:

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

Para usar una sobrecarga <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> que apunta a un punto de conexión dentro de la aplicación, cree una vista de MVC o una página de Razor Pages para ese punto de conexión. Por ejemplo, la plantilla de aplicación de Razor Pages genera la página y la clase de modelo de página siguientes:

*Error.cshtml*:

::: moniker range=">= aspnetcore-2.2"

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
    Swapping to the <strong>Development</strong> environment displays 
    detailed information about the error that occurred.
</p>
<p>
    <strong>The Development environment shouldn't be enabled for deployed 
    applications.</strong> It can result in displaying sensitive information 
    from exceptions to end users. For local debugging, enable the 
    <strong>Development</strong> environment by setting the 
    <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to 
    <strong>Development</strong> and restarting the app.
</p>
```

*Error.cshtml.cs*:

```csharp
[ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

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
    Swapping to <strong>Development</strong> environment will display more detailed 
    information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications
    </strong>, as it can result in sensitive information from exceptions being 
    displayed to end users. For local debugging, development environment can be 
    enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment 
    variable to <strong>Development</strong>, and restarting the application.
</p>
```

*Error.cshtml.cs*:

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, 
        NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

## <a name="exception-handling-code"></a>Código de control de excepciones

El código de las páginas de control de excepciones puede producir excepciones. Es recomendable que las páginas de errores de producción incluyan únicamente contenido estático.

Además, tenga en cuenta que después de que se envían los encabezados de una respuesta:

* La aplicación no puede cambiar el código de estado de la respuesta.
* No se pueden ejecutar páginas o controladores de excepciones. Deberá completarse la respuesta o anularse la conexión.

## <a name="server-exception-handling"></a>Control de excepciones del servidor

Además de la lógica de control de excepciones de la aplicación, la [implementación del servidor](xref:fundamentals/servers/index) puede controlar algunas excepciones. Si el servidor almacena en caché una excepción antes de que se envíen los encabezados de respuesta, envía una respuesta *500 - Error interno del servidor* sin cuerpo. Si el servidor almacena en caché una excepción después de que se envían los encabezados de respuesta, cierra la conexión. El servidor controla las solicitudes que no controla la aplicación. El control de excepciones del servidor controla cualquier excepción que se produzca cuando el servidor controle la solicitud. Las páginas de error personalizadas, el middleware de control de excepciones y los filtros de la aplicación no afectan este comportamiento.

## <a name="startup-exception-handling"></a>Control de excepciones de inicio

Solo el nivel de hospedaje puede controlar las excepciones que tienen lugar durante el inicio de la aplicación. Mediante el [host web](xref:fundamentals/host/web-host), puede [configurar cómo se comporta el host en respuesta a los errores durante el inicio](xref:fundamentals/host/web-host#detailed-errors) con las claves `captureStartupErrors` y `detailedErrors`.

El hospedaje solo puede mostrar una página de error para un error de inicio capturado si este se produce después del enlace de puerto/dirección del host. Si algún enlace produce un error por cualquier motivo:

* La capa de hospedaje registra una excepción crítica.
* El proceso de dotnet se bloquea.
* No se muestra ninguna página de error cuando la aplicación se ejecuta en el servidor [Kestrel](xref:fundamentals/servers/kestrel).

Si se ejecuta en [IIS](/iis) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), el [módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) devuelve un *error de proceso 502.5* si el proceso no se puede iniciar. Para obtener más información, vea <xref:host-and-deploy/iis/troubleshoot>. Para obtener información sobre cómo solucionar problemas de inicio con Azure App Service, vea <xref:host-and-deploy/azure-apps/troubleshoot>.

## <a name="aspnet-core-mvc-error-handling"></a>Control de errores de ASP.NET Core MVC

Las aplicaciones [MVC](xref:mvc/overview) tienen algunas opciones adicionales para controlar errores, como la configuración de filtros de excepciones y la realización de la validación del modelo.

### <a name="exception-filters"></a>Filtros de excepciones

Los filtros de excepciones se pueden configurar globalmente, o bien por controlador o por acción, en una aplicación MVC. Estos filtros controlan todas las excepciones no controladas que se hayan producido durante la ejecución de una acción de controlador o de otro filtro. En caso contrario, no se llama a estos filtros. Para obtener más información, vea <xref:mvc/controllers/filters#exception-filters>.

> [!TIP]
> Los filtros de excepciones son útiles para interceptar las excepciones que se producen en las acciones de MVC, pero no son tan flexibles como el middleware de control de excepciones. Se recomienda usar el middleware. Use filtros únicamente cuando deba realizar el control de errores *de manera diferente* según la acción de MVC elegida.

### <a name="handle-model-state-errors"></a>Control de errores de estado del modelo

La [validación de modelos](xref:mvc/models/validation) se produce antes de invocar cada acción de controlador, y es el método de acción el encargado de inspeccionar [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) y reaccionar de manera apropiada.

Algunas aplicaciones optan por seguir una convención estándar para tratar los errores de [validación de modelos](xref:mvc/models/validation), en cuyo caso un [filtro](xref:mvc/controllers/filters) podría ser el lugar adecuado para implementar esta directiva. Debe probar cómo se comportan las acciones con estados de modelo no válidos. Para obtener más información, vea <xref:mvc/controllers/testing>.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
