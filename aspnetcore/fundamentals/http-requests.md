---
title: Inicio de solicitudes HTTP
author: stevejgordon
description: Obtenga información sobre cómo usar la interfaz IHttpClientFactory para administrar instancias de HttpClient lógicas en ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 05/02/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/http-requests
ms.openlocfilehash: 30ac239a38376feecffc3010387ec5e0009b6db6
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
---
# <a name="initiate-http-requests"></a><span data-ttu-id="960a9-103">Inicio de solicitudes HTTP</span><span class="sxs-lookup"><span data-stu-id="960a9-103">Initiate HTTP requests</span></span>

<span data-ttu-id="960a9-104">Por [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) y [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="960a9-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

[!INCLUDE[](~/includes/2.1.md)]

<span data-ttu-id="960a9-105">Se puede registrar y usar un `IHttpClientFactory` para crear y configurar instancias de [HttpClient](/dotnet/api/system.net.http.httpclient) en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="960a9-105">An `IHttpClientFactory` can be registered and used to configure and create [HttpClient](/dotnet/api/system.net.http.httpclient) instances in an app.</span></span> <span data-ttu-id="960a9-106">Esto reporta las siguientes ventajas:</span><span class="sxs-lookup"><span data-stu-id="960a9-106">It offers the following benefits:</span></span>

* <span data-ttu-id="960a9-107">Proporciona una ubicación central para denominar y configurar instancias de `HttpClient` lógicas.</span><span class="sxs-lookup"><span data-stu-id="960a9-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="960a9-108">Así, por ejemplo, se podría registrar y configurar un cliente "github" para tener acceso a GitHub</span><span class="sxs-lookup"><span data-stu-id="960a9-108">For example, a "github" client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="960a9-109">y, de igual modo, registrar otro cliente predeterminado para otros fines.</span><span class="sxs-lookup"><span data-stu-id="960a9-109">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="960a9-110">Codifica el concepto de middleware saliente a través de controladores de delegación en `HttpClient` y proporciona extensiones para middleware basado en Polly para poder sacar partido de este.</span><span class="sxs-lookup"><span data-stu-id="960a9-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="960a9-111">Administra la agrupación y duración de las instancias de `HttpClientMessageHandler` subyacentes para evitar los problemas de DNS que suelen producirse al administrar las duraciones de `HttpClient` manualmente.</span><span class="sxs-lookup"><span data-stu-id="960a9-111">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="960a9-112">Agrega una experiencia de registro configurable (a través de `ILogger`) en todas las solicitudes enviadas a través de los clientes creados por Factory.</span><span class="sxs-lookup"><span data-stu-id="960a9-112">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="960a9-113">Patrones de consumo</span><span class="sxs-lookup"><span data-stu-id="960a9-113">Consumption patterns</span></span>

<span data-ttu-id="960a9-114">`IHttpClientFactory` se puede usar de varias formas en una aplicación:</span><span class="sxs-lookup"><span data-stu-id="960a9-114">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="960a9-115">Uso básico</span><span class="sxs-lookup"><span data-stu-id="960a9-115">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="960a9-116">Clientes con nombre</span><span class="sxs-lookup"><span data-stu-id="960a9-116">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="960a9-117">Clientes con tipo</span><span class="sxs-lookup"><span data-stu-id="960a9-117">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="960a9-118">Clientes generados</span><span class="sxs-lookup"><span data-stu-id="960a9-118">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="960a9-119">Ninguno de ellos es rigurosamente superior a otro,</span><span class="sxs-lookup"><span data-stu-id="960a9-119">None of them are strictly superior to another.</span></span> <span data-ttu-id="960a9-120">sino que el mejor método dependerá de las restricciones particulares de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="960a9-120">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="960a9-121">Uso básico</span><span class="sxs-lookup"><span data-stu-id="960a9-121">Basic usage</span></span>

<span data-ttu-id="960a9-122">Se puede registrar un `IHttpClientFactory` llamando al método de extensión `AddHttpClient` en `IServiceCollection`, dentro del método `ConfigureServices` en Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="960a9-122">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `ConfigureServices` method in Startup.cs.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet1)]

<span data-ttu-id="960a9-123">Una vez registrado, el código puede aceptar un `IHttpClientFactory` en cualquier parte donde se puedan insertar servicios por medio de la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="960a9-123">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="960a9-124">El `IHttpClientFactory` se puede usar para crear una instancia de `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="960a9-124">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,20)]

<span data-ttu-id="960a9-125">Cuando `IHttpClientFactory` se usa de este modo, constituye una excelente manera de refactorizar una aplicación existente.</span><span class="sxs-lookup"><span data-stu-id="960a9-125">Using `IHttpClientFactory` in this fashion is a great way to refactor an existing app.</span></span> <span data-ttu-id="960a9-126">No tiene efecto alguno en la forma en que `HttpClient` se usa.</span><span class="sxs-lookup"><span data-stu-id="960a9-126">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="960a9-127">En aquellos sitios en los que ya se hayan creado instancias de `HttpClient`, reemplace esas apariciones por una llamada a `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="960a9-127">In places where `HttpClient` instances are currently created, replace those occurrences with a call to `CreateClient`.</span></span>

### <a name="named-clients"></a><span data-ttu-id="960a9-128">Clientes con nombre</span><span class="sxs-lookup"><span data-stu-id="960a9-128">Named clients</span></span>

<span data-ttu-id="960a9-129">Si una aplicación necesita usar `HttpClient` de diversas maneras, cada una con una configuración diferente, una opción consiste en usar **clientes con nombre**.</span><span class="sxs-lookup"><span data-stu-id="960a9-129">If an app requires multiple distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="960a9-130">La configuración de un `HttpClient` con nombre se puede realizar durante la fase de registro en `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="960a9-130">Configuration for a named `HttpClient` can be specified during registration in `ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet2)]

<span data-ttu-id="960a9-131">En el código anterior, se llama a `AddHttpClient` usando el nombre "github".</span><span class="sxs-lookup"><span data-stu-id="960a9-131">In the preceding code, `AddHttpClient` is called, providing the name "github".</span></span> <span data-ttu-id="960a9-132">Este cliente tiene aplicadas algunas configuraciones predeterminadas, a saber, la dirección base y dos encabezados necesarios para trabajar con la API de GitHub.</span><span class="sxs-lookup"><span data-stu-id="960a9-132">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="960a9-133">Cada vez que se llama a `CreateClient`, se crea otra instancia de `HttpClient` y se llama a la acción de configuración.</span><span class="sxs-lookup"><span data-stu-id="960a9-133">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="960a9-134">Para consumir un cliente con nombre, se puede pasar un parámetro de cadena a `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="960a9-134">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="960a9-135">Especifique el nombre del cliente que se va a crear:</span><span class="sxs-lookup"><span data-stu-id="960a9-135">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=20)]

<span data-ttu-id="960a9-136">En el código anterior, no es necesario especificar un nombre de host en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="960a9-136">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="960a9-137">Basta con pasar solo la ruta de acceso, ya que se usa la dirección base configurada del cliente.</span><span class="sxs-lookup"><span data-stu-id="960a9-137">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="960a9-138">Clientes con tipo</span><span class="sxs-lookup"><span data-stu-id="960a9-138">Typed clients</span></span>

<span data-ttu-id="960a9-139">Los clientes con tipo proporcionan las mismas funciones que los clientes con nombre sin la necesidad de usar cadenas como claves.</span><span class="sxs-lookup"><span data-stu-id="960a9-139">Typed clients provide the same capabilities as named clients without the need to use strings as keys.</span></span> <span data-ttu-id="960a9-140">El método del cliente con tipo proporciona ayuda de compilador e IntelliSense al consumir clientes.</span><span class="sxs-lookup"><span data-stu-id="960a9-140">The typed client approach provides IntelliSense and compiler help when consuming clients.</span></span> <span data-ttu-id="960a9-141">Ofrecen una sola ubicación para configurar un determinado `HttpClient` e interactuar con él.</span><span class="sxs-lookup"><span data-stu-id="960a9-141">They provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="960a9-142">Por ejemplo, el mismo cliente con tipo se puede usar para un punto de conexión back-end único y encapsular toda la lógica que se ocupa de ese punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="960a9-142">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span> <span data-ttu-id="960a9-143">Otra ventaja es que funcionan con la inserción de dependencias, de modo que se pueden insertar cuando sea necesario en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="960a9-143">Another advantage is that they work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="960a9-144">Un cliente con tipo acepta un parámetro `HttpClient` en su constructor:</span><span class="sxs-lookup"><span data-stu-id="960a9-144">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="960a9-145">En el código anterior, la configuración se mueve al cliente con tipo.</span><span class="sxs-lookup"><span data-stu-id="960a9-145">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="960a9-146">El objeto `HttpClient` se expone como una propiedad pública.</span><span class="sxs-lookup"><span data-stu-id="960a9-146">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="960a9-147">Se pueden definir métodos específicos de API que exponen la funcionalidad `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="960a9-147">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="960a9-148">El método `GetLatestDocsIssue` encapsula el código necesario para consultar y analizar el último problema de repositorio de GitHub.</span><span class="sxs-lookup"><span data-stu-id="960a9-148">The `GetLatestDocsIssue` method encapsulates the code needed to query for and parse out the latest issue from a GitHub repository.</span></span>

<span data-ttu-id="960a9-149">Para registrar un cliente con tipo, se puede usar el método de extensión genérico `AddHttpClient` dentro de `ConfigureServices`, especificando la clase del cliente con tipo:</span><span class="sxs-lookup"><span data-stu-id="960a9-149">To register a typed client, the generic `AddHttpClient` extension method can be used within `ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet3)]

<span data-ttu-id="960a9-150">El cliente con tipo se registra como transitorio con inserción con dependencias,</span><span class="sxs-lookup"><span data-stu-id="960a9-150">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="960a9-151">y se puede insertar y consumir directamente:</span><span class="sxs-lookup"><span data-stu-id="960a9-151">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="960a9-152">Si lo prefiere, la configuración de un cliente con nombre se puede especificar durante su registro en `ConfigureServices`, en lugar de en su constructor:</span><span class="sxs-lookup"><span data-stu-id="960a9-152">If preferred, the configuration for a typed client can be specified during registration in `ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet4)]

<span data-ttu-id="960a9-153">El `HttpClient` se puede encapsular completamente dentro de un cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="960a9-153">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="960a9-154">En lugar de exponerlo como una propiedad, se pueden proporcionar métodos públicos que llamen a la instancia de `HttpClient` internamente.</span><span class="sxs-lookup"><span data-stu-id="960a9-154">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/GitHub/RepoService.cs?name=snippet1&highlight=3)]

<span data-ttu-id="960a9-155">En el código anterior, el `HttpClient` se almacena como un campo privado.</span><span class="sxs-lookup"><span data-stu-id="960a9-155">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="960a9-156">Todo el acceso para realizar llamadas externas pasa por el método `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="960a9-156">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="960a9-157">Clientes generados</span><span class="sxs-lookup"><span data-stu-id="960a9-157">Generated clients</span></span>

<span data-ttu-id="960a9-158">`IHttpClientFactory` se puede usar en combinación con otras bibliotecas de terceros, como [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="960a9-158">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="960a9-159">Refit es una biblioteca de REST para .NET</span><span class="sxs-lookup"><span data-stu-id="960a9-159">Refit is a REST library for .NET.</span></span> <span data-ttu-id="960a9-160">que convierte las API de REST en interfaces en vivo.</span><span class="sxs-lookup"><span data-stu-id="960a9-160">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="960a9-161">Se genera una implementación de la interfaz dinámicamente por medio de `RestService`, usando `HttpClient` para realizar las llamadas HTTP externas.</span><span class="sxs-lookup"><span data-stu-id="960a9-161">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="960a9-162">Se define una interfaz y una respuesta para representar la API externa y su correspondiente respuesta:</span><span class="sxs-lookup"><span data-stu-id="960a9-162">An interface and a reply are defined to represent the external API and its response:</span></span>

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

<span data-ttu-id="960a9-163">Un cliente con tipo se puede agregar usando Refit para generar la implementación:</span><span class="sxs-lookup"><span data-stu-id="960a9-163">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="960a9-164">La interfaz definida se puede usar cuando sea preciso con la implementación proporcionada por la inserción de dependencias y Refit:</span><span class="sxs-lookup"><span data-stu-id="960a9-164">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a><span data-ttu-id="960a9-165">Middleware de solicitud saliente</span><span class="sxs-lookup"><span data-stu-id="960a9-165">Outgoing request middleware</span></span>

<span data-ttu-id="960a9-166">`HttpClient` ya posee el concepto de controladores de delegación, que se pueden vincular entre sí para las solicitudes HTTP salientes.</span><span class="sxs-lookup"><span data-stu-id="960a9-166">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="960a9-167">`IHttpClientFactory` permite definir fácilmente los controladores que se usarán en cada cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="960a9-167">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="960a9-168">Admite el registro y encadenamiento de varios controladores para crear una canalización de middleware de solicitud saliente.</span><span class="sxs-lookup"><span data-stu-id="960a9-168">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="960a9-169">Cada uno de estos controladores es capaz de realizar la tarea antes y después de la solicitud de salida.</span><span class="sxs-lookup"><span data-stu-id="960a9-169">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="960a9-170">Este patrón es similar a la canalización de middleware de entrada de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="960a9-170">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="960a9-171">Dicho patrón proporciona un mecanismo para administrar cuestiones transversales relativas a las solicitudes HTTP, como el almacenamiento en caché, el control de errores, la serialización y el registro.</span><span class="sxs-lookup"><span data-stu-id="960a9-171">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="960a9-172">Para crear un controlador, defina una clase que se derive de `DelegatingHandler`.</span><span class="sxs-lookup"><span data-stu-id="960a9-172">To create a handler, define a class deriving from `DelegatingHandler`.</span></span> <span data-ttu-id="960a9-173">Invalide el método `SendAsync` para ejecutar el código antes de pasar la solicitud al siguiente controlador de la canalización:</span><span class="sxs-lookup"><span data-stu-id="960a9-173">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[Main](http-requests/samples/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="960a9-174">El código anterior define un controlador básico.</span><span class="sxs-lookup"><span data-stu-id="960a9-174">The preceding code defines a basic handler.</span></span> <span data-ttu-id="960a9-175">Comprueba si se ha incluido un encabezado X-API-KEY en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="960a9-175">It checks to see if an X-API-KEY header has been included on the request.</span></span> <span data-ttu-id="960a9-176">Si no está presente, puede evitar la llamada HTTP y devolver una respuesta adecuada.</span><span class="sxs-lookup"><span data-stu-id="960a9-176">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="960a9-177">Durante el registro, se pueden agregar uno o varios controladores a la configuración de un `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="960a9-177">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="960a9-178">Esta tarea se realiza a través de métodos de extensión en `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="960a9-178">This task is accomplished via extension methods on the `IHttpClientBuilder`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet5)]

<span data-ttu-id="960a9-179">En el código anterior, `ValidateHeaderHandler` se ha registrado con inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="960a9-179">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="960a9-180">El controlador **debe** estar registrado en la inserción de dependencias como transitorio.</span><span class="sxs-lookup"><span data-stu-id="960a9-180">The handler **must** be registered in DI as transient.</span></span> <span data-ttu-id="960a9-181">Una vez registrado, se puede llamar a `AddHttpMessageHandler`, pasando el tipo del controlador.</span><span class="sxs-lookup"><span data-stu-id="960a9-181">Once registered, `AddHttpMessageHandler` can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="960a9-182">Se pueden registrar varios controladores en el orden en que deben ejecutarse.</span><span class="sxs-lookup"><span data-stu-id="960a9-182">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="960a9-183">Cada controlador contiene el siguiente controlador hasta que el último `HttpClientHandler` ejecuta la solicitud:</span><span class="sxs-lookup"><span data-stu-id="960a9-183">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet6)]

## <a name="use-polly-based-handlers"></a><span data-ttu-id="960a9-184">Usar controladores basados en Polly</span><span class="sxs-lookup"><span data-stu-id="960a9-184">Use Polly-based handlers</span></span>

<span data-ttu-id="960a9-185">`IHttpClientFactory` se integra con una biblioteca de terceros muy conocida denominada [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="960a9-185">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="960a9-186">Polly es una biblioteca con capacidades de resistencia y control de errores transitorios para .NET.</span><span class="sxs-lookup"><span data-stu-id="960a9-186">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="960a9-187">Permite a los desarrolladores expresar directivas como, por ejemplo, de reintento, interruptor, tiempo de espera, aislamiento compartimentado y reserva de forma fluida y segura para los subprocesos.</span><span class="sxs-lookup"><span data-stu-id="960a9-187">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="960a9-188">Se proporcionan métodos de extensión para hacer posible el uso de directivas de Polly con instancias de `HttpClient` configuradas.</span><span class="sxs-lookup"><span data-stu-id="960a9-188">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="960a9-189">Encontrará extensiones de Polly disponibles en un paquete NuGet llamado "Microsoft.Extensions.Http.Polly".</span><span class="sxs-lookup"><span data-stu-id="960a9-189">The Polly extensions are available in a NuGet package called 'Microsoft.Extensions.Http.Polly'.</span></span> <span data-ttu-id="960a9-190">Este paquete no está incluido de forma predeterminada en el metapaquete "Microsoft.AspNetCore.App".</span><span class="sxs-lookup"><span data-stu-id="960a9-190">This package is not included by default by the 'Microsoft.AspNetCore.App' metapackage.</span></span> <span data-ttu-id="960a9-191">Para usar las extensiones, hay que incluir in elemento PackageReference explícitamente en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="960a9-191">To use the extensions, a PackageReference should be explicitly included in the project.</span></span>

[!code-csharp[](http-requests/samples/HttpClientFactorySample.csproj?highlight=9)]

<span data-ttu-id="960a9-192">Tras restaurar este paquete, hay métodos de extensión disponibles para admitir la adición de controladores basados en Polly en clientes.</span><span class="sxs-lookup"><span data-stu-id="960a9-192">After restoring this package, extension methods are available to support adding Polly-based handlers to clients.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="960a9-193">Control de errores transitorios</span><span class="sxs-lookup"><span data-stu-id="960a9-193">Handle transient faults</span></span>

<span data-ttu-id="960a9-194">Los errores más comunes que se puede esperar que ocurran al realizar llamadas HTTP externas serán transitorios.</span><span class="sxs-lookup"><span data-stu-id="960a9-194">The most common faults you may expect to occur when making external HTTP calls will be transient.</span></span> <span data-ttu-id="960a9-195">Por ello, se incluye un método de extensión muy práctico denominado `AddTransientHttpErrorPolicy`, que permite definir una directiva para controlar los errores transitorios.</span><span class="sxs-lookup"><span data-stu-id="960a9-195">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="960a9-196">Las directivas que se configuran con este método de extensión controlan `HttpRequestException`, las respuestas HTTP 5xx y las respuestas HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="960a9-196">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="960a9-197">La extensión `AddTransientHttpErrorPolicy` se puede usar en `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="960a9-197">The `AddTransientHttpErrorPolicy` extension can be used within `ConfigureServices`.</span></span> <span data-ttu-id="960a9-198">La extensión da acceso a un objeto `PolicyBuilder`, configurado para controlar los errores que pueden constituir un posible error transitorio:</span><span class="sxs-lookup"><span data-stu-id="960a9-198">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet7)]

<span data-ttu-id="960a9-199">En el código anterior, se define una directiva `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="960a9-199">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="960a9-200">Las solicitudes erróneas se reintentan hasta tres veces con un retardo de 600 ms entre intentos.</span><span class="sxs-lookup"><span data-stu-id="960a9-200">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="960a9-201">Seleccionar directivas dinámicamente</span><span class="sxs-lookup"><span data-stu-id="960a9-201">Dynamically select policies</span></span>

<span data-ttu-id="960a9-202">Existen más métodos de extensión que pueden servir para agregar controladores basados en Polly.</span><span class="sxs-lookup"><span data-stu-id="960a9-202">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="960a9-203">Una de esas extensiones es `AddPolicyHandler`, que tiene varias sobrecargas.</span><span class="sxs-lookup"><span data-stu-id="960a9-203">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="960a9-204">Una de esas sobrecargas permite inspeccionar la solicitud al dilucidar qué directiva aplicar:</span><span class="sxs-lookup"><span data-stu-id="960a9-204">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet8)]

<span data-ttu-id="960a9-205">En el código anterior, si la solicitud de salida es GET, se aplica un tiempo de espera de 10 segundos.</span><span class="sxs-lookup"><span data-stu-id="960a9-205">In the preceding code, if the outgoing request is a GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="960a9-206">En cualquier otro método HTTP, se usa un tiempo de espera de 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="960a9-206">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="960a9-207">Agregar varios controladores de Polly</span><span class="sxs-lookup"><span data-stu-id="960a9-207">Add multiple Polly handlers</span></span>

<span data-ttu-id="960a9-208">Es habitual anidar directivas de Polly para proporcionar una mejor funcionalidad:</span><span class="sxs-lookup"><span data-stu-id="960a9-208">It is common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet9)]

<span data-ttu-id="960a9-209">En el ejemplo anterior, se agregan dos controladores.</span><span class="sxs-lookup"><span data-stu-id="960a9-209">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="960a9-210">En el primer ejemplo se usa la extensión `AddTransientHttpErrorPolicy` para agregar una directiva de reintento.</span><span class="sxs-lookup"><span data-stu-id="960a9-210">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="960a9-211">Las solicitudes con error se reintentan hasta tres veces.</span><span class="sxs-lookup"><span data-stu-id="960a9-211">Failed requests are retried up to three times.</span></span> <span data-ttu-id="960a9-212">La segunda llamada a `AddTransientHttpErrorPolicy` agrega una directiva de interruptor.</span><span class="sxs-lookup"><span data-stu-id="960a9-212">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="960a9-213">Las solicitudes externas subsiguientes se bloquean durante 30 segundos si se producen cinco intentos infructuosos seguidos.</span><span class="sxs-lookup"><span data-stu-id="960a9-213">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="960a9-214">Las directivas de interruptor tienen estado.</span><span class="sxs-lookup"><span data-stu-id="960a9-214">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="960a9-215">Así, todas las llamadas realizadas a través de este cliente comparten el mismo estado de circuito.</span><span class="sxs-lookup"><span data-stu-id="960a9-215">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="960a9-216">Agregar directivas desde el Registro de Polly</span><span class="sxs-lookup"><span data-stu-id="960a9-216">Add policies from the Polly registry</span></span>

<span data-ttu-id="960a9-217">Una forma de administrar las directivas usadas habitualmente consiste en definirlas una vez y registrarlas con `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="960a9-217">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="960a9-218">Se proporciona un método de extensión que permite agregar un controlador por medio de una directiva del Registro:</span><span class="sxs-lookup"><span data-stu-id="960a9-218">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet10)]

<span data-ttu-id="960a9-219">En el código anterior, se agrega un elemento PolicyRegistry a `ServiceCollection` y, con él, se registran dos directivas.</span><span class="sxs-lookup"><span data-stu-id="960a9-219">In the preceding code, a PolicyRegistry is added to the `ServiceCollection` and two policies are registered with it.</span></span> <span data-ttu-id="960a9-220">Para poder usar una directiva del Registro, se usa el método `AddPolicyHandlerFromRegistry`, pasando el nombre de la directiva que se va a aplicar.</span><span class="sxs-lookup"><span data-stu-id="960a9-220">In order to use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="960a9-221">Encontrará más información sobre `IHttpClientFactory` y las integraciones de Polly en la [wiki de Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="960a9-221">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="960a9-222">HttpClient y administración de la duración</span><span class="sxs-lookup"><span data-stu-id="960a9-222">HttpClient and lifetime management</span></span>

<span data-ttu-id="960a9-223">Cada vez que se llama a `CreateClient` en `IHttpClientFactory`, se devuelve una nueva instancia de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="960a9-223">Each time `CreateClient` is called on the `IHttpClientFactory`, a new instance of a `HttpClient` is returned.</span></span> <span data-ttu-id="960a9-224">Habrá un `HttpMessageHandler` por cada cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="960a9-224">There will be a `HttpMessageHandler` per named client.</span></span> <span data-ttu-id="960a9-225">`IHttpClientFactory` agrupará las instancias de `HttpMessageHandler` creadas por Factory para reducir el consumo de recursos.</span><span class="sxs-lookup"><span data-stu-id="960a9-225">`IHttpClientFactory` will pool the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="960a9-226">Se puede reutilizar una instancia de `HttpMessageHandler` del grupo al crear una instancia de `HttpClient` si su duración aún no ha expirado.</span><span class="sxs-lookup"><span data-stu-id="960a9-226">A `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span> 

<span data-ttu-id="960a9-227">La agrupación de controladores es conveniente porque cada controlador suele administrar sus propias conexiones HTTP subyacentes. Crear más controladores de lo necesario puede provocar retrasos en la conexión.</span><span class="sxs-lookup"><span data-stu-id="960a9-227">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections; creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="960a9-228">Además, algunos controladores dejan las conexiones abiertas de forma indefinida, lo que puede ser un obstáculo a la hora de reaccionar ante los cambios de DNS.</span><span class="sxs-lookup"><span data-stu-id="960a9-228">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="960a9-229">La duración de controlador predeterminada es dos minutos.</span><span class="sxs-lookup"><span data-stu-id="960a9-229">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="960a9-230">El valor predeterminado se puede reemplazar individualmente en cada cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="960a9-230">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="960a9-231">Para ello, llame a `SetHandlerLifetime` en el `IHttpClientBuilder` que se devuelve cuando se crea el cliente:</span><span class="sxs-lookup"><span data-stu-id="960a9-231">To override it, call `SetHandlerLifetime` on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet11)]

## <a name="logging"></a><span data-ttu-id="960a9-232">Registro</span><span class="sxs-lookup"><span data-stu-id="960a9-232">Logging</span></span>

<span data-ttu-id="960a9-233">Los clientes que se han creado a través de `IHttpClientFactory` registran mensajes de registro de todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="960a9-233">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="960a9-234">Deberá habilitar el nivel de información adecuado en la configuración del registro para ver los mensajes de registro predeterminados.</span><span class="sxs-lookup"><span data-stu-id="960a9-234">You'll need to enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="960a9-235">El registro de más información, como el registro de encabezados de solicitud, solo se incluye en el nivel de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="960a9-235">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="960a9-236">La categoría de registro usada en cada cliente incluye el nombre del cliente.</span><span class="sxs-lookup"><span data-stu-id="960a9-236">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="960a9-237">Así, por ejemplo, un cliente llamado "MyNamedClient" registra mensajes con una categoría `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="960a9-237">A client named "MyNamedClient", for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="960a9-238">Los mensajes con el sufijo "LogicalHandler" se producen fuera de la canalización de controlador de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="960a9-238">Messages with the suffix of "LogicalHandler" occur on the outside of request handler pipeline.</span></span> <span data-ttu-id="960a9-239">En la solicitud, los mensajes se registran antes de que cualquier otro controlador de la canalización haya procesado la solicitud.</span><span class="sxs-lookup"><span data-stu-id="960a9-239">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="960a9-240">En la respuesta, los mensajes se registran después de que cualquier otro controlador de la canalización haya recibido la respuesta.</span><span class="sxs-lookup"><span data-stu-id="960a9-240">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="960a9-241">El registro también se produce dentro de la canalización de controlador de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="960a9-241">Logging also occurs on the inside of the request handler pipeline.</span></span> <span data-ttu-id="960a9-242">En nuestro caso de ejemplo de "MyNamedClient", esos mensajes se registran en la categoría de registro `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="960a9-242">In the case of the "MyNamedClient" example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="960a9-243">En la solicitud, esto tiene lugar después de que todos los demás controladores se hayan ejecutado y justo antes de que la solicitud se envíe por la red.</span><span class="sxs-lookup"><span data-stu-id="960a9-243">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="960a9-244">En la respuesta, este registro incluye el estado de la respuesta antes de que vuelva a pasar por la canalización del controlador.</span><span class="sxs-lookup"><span data-stu-id="960a9-244">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="960a9-245">Si se habilita el registro tanto dentro como fuera de la canalización, se podrán inspeccionar los cambios realizados por otros controladores de la canalización.</span><span class="sxs-lookup"><span data-stu-id="960a9-245">Enabling logging on the outside and inside of the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="960a9-246">Esto puede englobar cambios, por ejemplo, en los encabezados de solicitud o en el código de estado de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="960a9-246">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="960a9-247">Si el nombre del cliente se incluye en la categoría de registro, dicho registro se podrá filtrar para encontrar clientes con nombre específicos cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="960a9-247">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="960a9-248">Configurar HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="960a9-248">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="960a9-249">Puede que sea necesario controlar la configuración del elemento `HttpMessageHandler` interno usado por un cliente.</span><span class="sxs-lookup"><span data-stu-id="960a9-249">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="960a9-250">Se devuelve un `IHttpClientBuilder` cuando se agregan clientes con nombre o con tipo.</span><span class="sxs-lookup"><span data-stu-id="960a9-250">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="960a9-251">Se puede usar el método de extensión `ConfigurePrimaryHttpMessageHandler` para definir un delegado.</span><span class="sxs-lookup"><span data-stu-id="960a9-251">The `ConfigurePrimaryHttpMessageHandler` extension method can be used to define a delegate.</span></span> <span data-ttu-id="960a9-252">Este delegado servirá para crear y configurar el elemento principal `HttpMessageHandler` que ese cliente usa:</span><span class="sxs-lookup"><span data-stu-id="960a9-252">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet12)]
