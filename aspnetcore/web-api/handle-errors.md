---
title: Control de errores en API web de ASP.NET Core
author: pranavkm
description: Obtenga información sobre el control de errores con API web de ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: prkrishn
ms.custom: mvc
ms.date: 09/25/2019
uid: web-api/handle-errors
ms.openlocfilehash: 9c5dd2f89e7351f386d1f0633c831952dc58e568
ms.sourcegitcommit: 994da92edb0abf856b1655c18880028b15a28897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71278730"
---
# <a name="handle-errors-in-aspnet-core-web-apis"></a><span data-ttu-id="ed69d-103">Control de errores en API web de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ed69d-103">Handle errors in ASP.NET Core web APIs</span></span>

<span data-ttu-id="ed69d-104">En este artículo se describe cómo administrar y personalizar el control de errores con API web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ed69d-104">This article describes how to handle and customize error handling with ASP.NET Core web APIs.</span></span>

<span data-ttu-id="ed69d-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/handle-errors/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ed69d-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/handle-errors/samples) ([How to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="ed69d-106">Página de excepciones para el desarrollador</span><span class="sxs-lookup"><span data-stu-id="ed69d-106">Developer Exception Page</span></span>

<span data-ttu-id="ed69d-107">La [Página de excepciones para el desarrollador](xref:fundamentals/error-handling) es una herramienta útil para obtener seguimientos de pila detallados de los errores del servidor.</span><span class="sxs-lookup"><span data-stu-id="ed69d-107">The [Developer Exception Page](xref:fundamentals/error-handling) is a useful tool to get detailed stack traces for server errors.</span></span>

<span data-ttu-id="ed69d-108">La Página de excepciones para el desarrollador muestra una respuesta de texto sin formato si el cliente no acepta la salida con formato HTML.</span><span class="sxs-lookup"><span data-stu-id="ed69d-108">The Developer Exception Page displays a plain-text response if the client doesn't accept HTML-formatted output.</span></span> <span data-ttu-id="ed69d-109">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ed69d-109">For example:</span></span>

```
> curl https://localhost:5001/weatherforecast
System.ArgumentException: count
   at errorhandling.Controllers.WeatherForecastController.Get(Int32 x) in D:\work\Samples\samples\aspnetcore\mvc\errorhandling\Controllers\WeatherForecastController.cs:line 35
   at lambda_method(Closure , Object , Object[] )
   at Microsoft.Extensions.Internal.ObjectMethodExecutor.Execute(Object target, Object[] parameters)
...
```

> [!WARNING]
> <span data-ttu-id="ed69d-110">Habilite la página de excepciones para el desarrollador **solo cuando la aplicación se ejecute en el entorno de desarrollo**.</span><span class="sxs-lookup"><span data-stu-id="ed69d-110">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="ed69d-111">No le interesa compartir públicamente información detallada sobre las excepciones cuando la aplicación se ejecute en producción.</span><span class="sxs-lookup"><span data-stu-id="ed69d-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="ed69d-112">Para más información sobre la configuración de entornos, consulte <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="ed69d-112">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

## <a name="exception-handler"></a><span data-ttu-id="ed69d-113">Controlador de excepciones</span><span class="sxs-lookup"><span data-stu-id="ed69d-113">Exception handler</span></span>

<span data-ttu-id="ed69d-114">En entornos que no son de desarrollo, se puede usar [middleware de control de excepciones](xref:fundamentals/error-handling) para producir una carga de error:</span><span class="sxs-lookup"><span data-stu-id="ed69d-114">In non-development environments, [Exception Handling Middleware](xref:fundamentals/error-handling) can be used to produce an error payload:</span></span>

1. <span data-ttu-id="ed69d-115">En `Startup.Configure`, invoque <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> para usar el middleware:</span><span class="sxs-lookup"><span data-stu-id="ed69d-115">In `Startup.Configure`, invoke <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> to use the middleware:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

1. <span data-ttu-id="ed69d-116">Configure una acción de controlador para responder a la ruta `/error`:</span><span class="sxs-lookup"><span data-stu-id="ed69d-116">Configure a controller action to respond to the `/error` route:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

<span data-ttu-id="ed69d-117">La acción `Error` anterior envía una carga compatible con [RFC7807](https://tools.ietf.org/html/rfc7807) al cliente.</span><span class="sxs-lookup"><span data-stu-id="ed69d-117">The preceding `Error` action sends an [RFC7807](https://tools.ietf.org/html/rfc7807)-compliant payload to the client.</span></span>

<span data-ttu-id="ed69d-118">El middleware de control de excepciones también puede proporcionar una salida negociada de contenido más detallada en el entorno de desarrollo local.</span><span class="sxs-lookup"><span data-stu-id="ed69d-118">Exception Handling Middleware can also provide more detailed content-negotiated output in the local development environment.</span></span> <span data-ttu-id="ed69d-119">Use los pasos siguientes para generar un formato de carga coherente en los entornos de desarrollo y producción:</span><span class="sxs-lookup"><span data-stu-id="ed69d-119">Use the following steps to produce a consistent payload format across development and production environments:</span></span>

1. <span data-ttu-id="ed69d-120">En `Startup.Configure`, registre las instancias de middleware de control de excepciones específicas del entorno:</span><span class="sxs-lookup"><span data-stu-id="ed69d-120">In `Startup.Configure`, register environment-specific Exception Handling Middleware instances:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseExceptionHandler("/error-local-development");
        }
        else
        {
            app.UseExceptionHandler("/error");
        }
    }
    ```

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseExceptionHandler("/error-local-development");
        }
        else
        {
            app.UseExceptionHandler("/error");
        }
    }
    ```

    ::: moniker-end

    <span data-ttu-id="ed69d-121">En el código anterior, el middleware se ha registrado con:</span><span class="sxs-lookup"><span data-stu-id="ed69d-121">In the preceding code, the middleware is registered with:</span></span>

    * <span data-ttu-id="ed69d-122">Una ruta de `/error-local-development` en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="ed69d-122">A route of `/error-local-development` in the Development environment.</span></span>
    * <span data-ttu-id="ed69d-123">Una ruta de `/error` en entornos que no son de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="ed69d-123">A route of `/error` in environments that aren't Development.</span></span>
    
1. <span data-ttu-id="ed69d-124">Aplique el enrutamiento de atributos a acciones del controlador:</span><span class="sxs-lookup"><span data-stu-id="ed69d-124">Apply attribute routing to controller actions:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

## <a name="use-exceptions-to-modify-the-response"></a><span data-ttu-id="ed69d-125">Uso de excepciones para modificar la respuesta</span><span class="sxs-lookup"><span data-stu-id="ed69d-125">Use exceptions to modify the response</span></span>

<span data-ttu-id="ed69d-126">El contenido de la respuesta se puede modificar desde fuera del controlador.</span><span class="sxs-lookup"><span data-stu-id="ed69d-126">The contents of the response can be modified from outside of the controller.</span></span> <span data-ttu-id="ed69d-127">En la API web de ASP.NET 4.x, una manera de hacerlo era usar el tipo <xref:System.Web.Http.HttpResponseException>.</span><span class="sxs-lookup"><span data-stu-id="ed69d-127">In ASP.NET 4.x Web API, one way to do this was using the <xref:System.Web.Http.HttpResponseException> type.</span></span> <span data-ttu-id="ed69d-128">ASP.NET Core no incluye un tipo equivalente.</span><span class="sxs-lookup"><span data-stu-id="ed69d-128">ASP.NET Core doesn't include an equivalent type.</span></span> <span data-ttu-id="ed69d-129">Se pueden agregar compatibilidad para `HttpResponseException` con los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="ed69d-129">Support for `HttpResponseException` can be added with the following steps:</span></span>

1. <span data-ttu-id="ed69d-130">Cree un tipo de excepción conocido denominado `HttpResponseException`:</span><span class="sxs-lookup"><span data-stu-id="ed69d-130">Create a well-known exception type named `HttpResponseException`:</span></span>

    [!code-csharp[](handle-errors/samples/3.x/Exceptions/HttpResponseException.cs?name=snippet_HttpResponseException)]

1. <span data-ttu-id="ed69d-131">Cree un filtro de acción denominado `HttpResponseExceptionFilter`:</span><span class="sxs-lookup"><span data-stu-id="ed69d-131">Create an action filter named `HttpResponseExceptionFilter`:</span></span>

    [!code-csharp[](handle-errors/samples/3.x/Filters/HttpResponseExceptionFilter.cs?name=snippet_HttpResponseExceptionFilter)]

1. <span data-ttu-id="ed69d-132">En `Startup.ConfigureServices`, agregue el filtro de acción a la colección de filtros:</span><span class="sxs-lookup"><span data-stu-id="ed69d-132">In `Startup.ConfigureServices`, add the action filter to the filters collection:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.2"
    
    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.1"

    [!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

## <a name="validation-failure-error-response"></a><span data-ttu-id="ed69d-133">Respuesta de error ante errores de validación</span><span class="sxs-lookup"><span data-stu-id="ed69d-133">Validation failure error response</span></span>

<span data-ttu-id="ed69d-134">En el caso de los controladores de API web, MVC responde con un tipo de respuesta <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> cuando se produce un error en la validación del modelo.</span><span class="sxs-lookup"><span data-stu-id="ed69d-134">For web API controllers, MVC responds with a <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> response type when model validation fails.</span></span> <span data-ttu-id="ed69d-135">MVC usa los resultados de <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> para construir la respuesta de error para un error de validación.</span><span class="sxs-lookup"><span data-stu-id="ed69d-135">MVC uses the results of <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to construct the error response for a validation failure.</span></span> <span data-ttu-id="ed69d-136">En el ejemplo siguiente se usa la fábrica para cambiar el tipo de respuesta predeterminado a <xref:Microsoft.AspNetCore.Mvc.SerializableError> en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ed69d-136">The following example uses the factory to change the default response type to <xref:Microsoft.AspNetCore.Mvc.SerializableError> in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=4-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=5-14)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=6-15)]

::: moniker-end

## <a name="client-error-response"></a><span data-ttu-id="ed69d-137">Respuesta de error del cliente</span><span class="sxs-lookup"><span data-stu-id="ed69d-137">Client error response</span></span>

<span data-ttu-id="ed69d-138">Se define un *resultado error* como un resultado con un código de estado HTTP de 400 o superior.</span><span class="sxs-lookup"><span data-stu-id="ed69d-138">An *error result* is defined as a result with an HTTP status code of 400 or higher.</span></span> <span data-ttu-id="ed69d-139">En el caso de los controladores de API web, MVC transforma un resultado de un error en un resultado con <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="ed69d-139">For web API controllers, MVC transforms an error result to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ed69d-140">La respuesta de error se puede configurar de una de las siguientes maneras:</span><span class="sxs-lookup"><span data-stu-id="ed69d-140">The error response can be configured in one of the following ways:</span></span>

1. [<span data-ttu-id="ed69d-141">Implementar ProblemDetailsFactory</span><span class="sxs-lookup"><span data-stu-id="ed69d-141">Implement ProblemDetailsFactory</span></span>](#implement-problemdetailsfactory)
1. [<span data-ttu-id="ed69d-142">Usar ApiBehaviorOptions.ClientErrorMapping</span><span class="sxs-lookup"><span data-stu-id="ed69d-142">Use ApiBehaviorOptions.ClientErrorMapping</span></span>](#use-apibehavioroptionsclienterrormapping)

### <a name="implement-problemdetailsfactory"></a><span data-ttu-id="ed69d-143">Implementación de ProblemDetailsFactory</span><span class="sxs-lookup"><span data-stu-id="ed69d-143">Implement ProblemDetailsFactory</span></span>

<span data-ttu-id="ed69d-144">MVC usa `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory` para generar todas las instancias de <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> y <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="ed69d-144">MVC uses `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory` to produce all instances of <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> and <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="ed69d-145">Esto incluye las respuestas de error del cliente, las respuestas de error ante errores de validación y los métodos auxiliares `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` y <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem>.</span><span class="sxs-lookup"><span data-stu-id="ed69d-145">This includes client error responses, validation failure error responses, and the `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> helper methods.</span></span>

<span data-ttu-id="ed69d-146">Para personalizar la respuesta de detalles del problema, registre una implementación personalizada de `ProblemDetailsFactory` en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ed69d-146">To customize the problem details response, register a custom implementation of `ProblemDetailsFactory` in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection serviceCollection)
{
    services.AddControllers();
    services.AddTransient<ProblemDetailsFactory, CustomProblemDetailsFactory>();
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="ed69d-147">La respuesta de error se puede configurar como se describe en la sección [Uso de ApiBehaviorOptions.ClientErrorMapping](#use-apibehavioroptionsclienterrormapping).</span><span class="sxs-lookup"><span data-stu-id="ed69d-147">The error response can be configured as outlined in the [Use ApiBehaviorOptions.ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) section.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="use-apibehavioroptionsclienterrormapping"></a><span data-ttu-id="ed69d-148">Uso de ApiBehaviorOptions.ClientErrorMapping</span><span class="sxs-lookup"><span data-stu-id="ed69d-148">Use ApiBehaviorOptions.ClientErrorMapping</span></span>

<span data-ttu-id="ed69d-149">Use la propiedad <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> para configurar el contenido de la respuesta `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="ed69d-149">Use the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="ed69d-150">Por ejemplo, el código siguiente de `Startup.ConfigureServices` permite actualizar la propiedad `type` para respuestas 404:</span><span class="sxs-lookup"><span data-stu-id="ed69d-150">For example, the following code in `Startup.ConfigureServices` updates the `type` property for 404 responses:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8-9)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=9-10)]

::: moniker-end
