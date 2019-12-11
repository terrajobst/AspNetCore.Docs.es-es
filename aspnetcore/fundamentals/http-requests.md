---
title: Realización de solicitudes HTTP mediante IHttpClientFactory en ASP.NET Core
author: stevejgordon
description: Obtenga información sobre cómo usar la interfaz IHttpClientFactory para administrar instancias de HttpClient lógicas en ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/27/2019
uid: fundamentals/http-requests
ms.openlocfilehash: f33444b8fc08dc022da7700af53a218600290162
ms.sourcegitcommit: 169ea5116de729c803685725d96450a270bc55b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74733926"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="b3209-103">Realización de solicitudes HTTP mediante IHttpClientFactory en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b3209-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b3209-104">Por [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT) y [Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="b3209-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="b3209-105">Se puede registrar y usar una interfaz <xref:System.Net.Http.IHttpClientFactory> para crear y configurar instancias de <xref:System.Net.Http.HttpClient> en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="b3209-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="b3209-106">`IHttpClientFactory` ofrece las ventajas siguientes:</span><span class="sxs-lookup"><span data-stu-id="b3209-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="b3209-107">Proporciona una ubicación central para denominar y configurar instancias de `HttpClient` lógicas.</span><span class="sxs-lookup"><span data-stu-id="b3209-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="b3209-108">Por ejemplo, se podría registrar un cliente *github* y configurarlo para acceder a [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="b3209-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="b3209-109">Se puede registrar un cliente predeterminado para el acceso general.</span><span class="sxs-lookup"><span data-stu-id="b3209-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="b3209-110">Codifica el concepto de middleware de salida a través de la delegación de controladores en `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="b3209-111">Proporciona extensiones para el middleware basado en Polly a fin de aprovechar los controladores de delegación en `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="b3209-112">Administra la agrupación y la duración de las instancias de `HttpClientMessageHandler` subyacentes.</span><span class="sxs-lookup"><span data-stu-id="b3209-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="b3209-113">La administración automática evita los problemas comunes de DNS (Sistema de nombres de dominio) que se producen al administrar la duración de `HttpClient` de forma manual.</span><span class="sxs-lookup"><span data-stu-id="b3209-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="b3209-114">Agrega una experiencia de registro configurable (a través de `ILogger`) en todas las solicitudes enviadas a través de los clientes creados por Factory.</span><span class="sxs-lookup"><span data-stu-id="b3209-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="b3209-115">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="b3209-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="b3209-116">En el código de ejemplo de la versión este tema se usa <xref:System.Text.Json> para deserializar el contenido JSON devuelto en las respuestas HTTP.</span><span class="sxs-lookup"><span data-stu-id="b3209-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="b3209-117">Para obtener ejemplos en los que se usan `Json.NET` y `ReadAsAsync<T>`, utilice el selector de versión para seleccionar una versión 2.x de este tema.</span><span class="sxs-lookup"><span data-stu-id="b3209-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="b3209-118">Patrones de consumo</span><span class="sxs-lookup"><span data-stu-id="b3209-118">Consumption patterns</span></span>

<span data-ttu-id="b3209-119">`IHttpClientFactory` se puede usar de varias formas en una aplicación:</span><span class="sxs-lookup"><span data-stu-id="b3209-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="b3209-120">Uso básico</span><span class="sxs-lookup"><span data-stu-id="b3209-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="b3209-121">Clientes con nombre</span><span class="sxs-lookup"><span data-stu-id="b3209-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="b3209-122">Clientes con tipo</span><span class="sxs-lookup"><span data-stu-id="b3209-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="b3209-123">Clientes generados</span><span class="sxs-lookup"><span data-stu-id="b3209-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="b3209-124">El mejor enfoque depende de los requisitos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b3209-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="b3209-125">Uso básico</span><span class="sxs-lookup"><span data-stu-id="b3209-125">Basic usage</span></span>

<span data-ttu-id="b3209-126">`IHttpClientFactory` se puede registrar mediante una llamada a `AddHttpClient`:</span><span class="sxs-lookup"><span data-stu-id="b3209-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="b3209-127">Se puede solicitar una instancia de `IHttpClientFactory` mediante la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b3209-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="b3209-128">En el código siguiente se usa `IHttpClientFactory` para crear una instancia de `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="b3209-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="b3209-129">El uso de `IHttpClientFactory` como en el ejemplo anterior es una buena manera de refactorizar una aplicación existente.</span><span class="sxs-lookup"><span data-stu-id="b3209-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="b3209-130">No tiene efecto alguno en la forma de usar `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="b3209-131">En aquellos sitios de una aplicación existente en los que ya se hayan creado instancias de `HttpClient`, reemplace esas repeticiones por llamadas a <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="b3209-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="b3209-132">Clientes con nombre</span><span class="sxs-lookup"><span data-stu-id="b3209-132">Named clients</span></span>

<span data-ttu-id="b3209-133">Los clientes con nombre son una buena opción cuando:</span><span class="sxs-lookup"><span data-stu-id="b3209-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="b3209-134">La aplicación requiere muchos usos distintos de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="b3209-135">Muchas instancias `HttpClient` de tienen otra configuración.</span><span class="sxs-lookup"><span data-stu-id="b3209-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="b3209-136">La configuración de un objeto `HttpClient` con nombre se puede realizar durante la fase de registro en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b3209-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="b3209-137">En el código anterior, el cliente está configurado con:</span><span class="sxs-lookup"><span data-stu-id="b3209-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="b3209-138">La dirección base. `https://api.github.com/`.</span><span class="sxs-lookup"><span data-stu-id="b3209-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="b3209-139">Dos encabezados necesarios para trabajar con la API de GitHub.</span><span class="sxs-lookup"><span data-stu-id="b3209-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="b3209-140">CreateClient</span><span class="sxs-lookup"><span data-stu-id="b3209-140">CreateClient</span></span>

<span data-ttu-id="b3209-141">Cada vez que se llama a <xref:System.Net.Http.IHttpClientFactory.CreateClient*>:</span><span class="sxs-lookup"><span data-stu-id="b3209-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="b3209-142">Se crea una instancia de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="b3209-143">Se llama a la acción de configuración.</span><span class="sxs-lookup"><span data-stu-id="b3209-143">The configuration action is called.</span></span>

<span data-ttu-id="b3209-144">Para crear un cliente con nombre, pase su nombre a `CreateClient`:</span><span class="sxs-lookup"><span data-stu-id="b3209-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="b3209-145">En el código anterior, no es necesario especificar un nombre de host en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b3209-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="b3209-146">El código puede pasar solo la ruta de acceso, ya que se usa la dirección base configurada del cliente.</span><span class="sxs-lookup"><span data-stu-id="b3209-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="b3209-147">Clientes con tipo</span><span class="sxs-lookup"><span data-stu-id="b3209-147">Typed clients</span></span>

<span data-ttu-id="b3209-148">Clientes con tipo:</span><span class="sxs-lookup"><span data-stu-id="b3209-148">Typed clients:</span></span>

* <span data-ttu-id="b3209-149">Proporcionan las mismas funciones que los clientes con nombre sin la necesidad de usar cadenas como claves.</span><span class="sxs-lookup"><span data-stu-id="b3209-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="b3209-150">Ofrecen ayuda relativa al compilador e IntelliSense al consumir clientes.</span><span class="sxs-lookup"><span data-stu-id="b3209-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="b3209-151">Facilitan una sola ubicación para configurar un elemento `HttpClient` determinado e interactuar con él.</span><span class="sxs-lookup"><span data-stu-id="b3209-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="b3209-152">Por ejemplo, es posible usar un solo cliente con tipo:</span><span class="sxs-lookup"><span data-stu-id="b3209-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="b3209-153">Para un único punto de conexión de back-end.</span><span class="sxs-lookup"><span data-stu-id="b3209-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="b3209-154">Para encapsular toda la lógica relacionada con el punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="b3209-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="b3209-155">Funcionan con la inserción de dependencias y se pueden insertar en la aplicación cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="b3209-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="b3209-156">Un cliente con tipo acepta un parámetro `HttpClient` en su constructor:</span><span class="sxs-lookup"><span data-stu-id="b3209-156">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="b3209-157">En el código anterior:</span><span class="sxs-lookup"><span data-stu-id="b3209-157">In the preceding code:</span></span>

* <span data-ttu-id="b3209-158">La configuración se mueve al cliente con tipo.</span><span class="sxs-lookup"><span data-stu-id="b3209-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="b3209-159">El objeto `HttpClient` se expone como una propiedad pública.</span><span class="sxs-lookup"><span data-stu-id="b3209-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="b3209-160">Se pueden crear métodos específicos de la API que exponen la funcionalidad de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="b3209-161">Por ejemplo, el método `GetAspNetDocsIssues` encapsula el código para recuperar incidencias abiertas.</span><span class="sxs-lookup"><span data-stu-id="b3209-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="b3209-162">En el código siguiente se llama a <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> en `Startup.ConfigureServices` para registrar una clase de cliente con tipo:</span><span class="sxs-lookup"><span data-stu-id="b3209-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="b3209-163">El cliente con tipo se registra como transitorio con inserción con dependencias,</span><span class="sxs-lookup"><span data-stu-id="b3209-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="b3209-164">y se puede insertar y consumir directamente:</span><span class="sxs-lookup"><span data-stu-id="b3209-164">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="b3209-165">La configuración de un cliente con tipo se puede especificar durante su registro en `Startup.ConfigureServices`, en lugar de en su constructor:</span><span class="sxs-lookup"><span data-stu-id="b3209-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="b3209-166">`HttpClient` se puede encapsular dentro de un cliente con tipo.</span><span class="sxs-lookup"><span data-stu-id="b3209-166">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="b3209-167">En lugar de exponerlo como una propiedad, defina un método que llame a la instancia de `HttpClient` internamente:</span><span class="sxs-lookup"><span data-stu-id="b3209-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="b3209-168">En el código anterior, `HttpClient` se almacena en un campo privado.</span><span class="sxs-lookup"><span data-stu-id="b3209-168">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="b3209-169">El acceso a `HttpClient` se realiza mediante el método `GetRepos` público.</span><span class="sxs-lookup"><span data-stu-id="b3209-169">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="b3209-170">Clientes generados</span><span class="sxs-lookup"><span data-stu-id="b3209-170">Generated clients</span></span>

<span data-ttu-id="b3209-171">`IHttpClientFactory` se puede usar en combinación con bibliotecas de terceros, como [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="b3209-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="b3209-172">Refit es una biblioteca de REST para .NET</span><span class="sxs-lookup"><span data-stu-id="b3209-172">Refit is a REST library for .NET.</span></span> <span data-ttu-id="b3209-173">que convierte las API de REST en interfaces en vivo.</span><span class="sxs-lookup"><span data-stu-id="b3209-173">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="b3209-174">Se genera una implementación de la interfaz dinámicamente por medio de `RestService`, usando `HttpClient` para realizar las llamadas HTTP externas.</span><span class="sxs-lookup"><span data-stu-id="b3209-174">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="b3209-175">Se define una interfaz y una respuesta para representar la API externa y su correspondiente respuesta:</span><span class="sxs-lookup"><span data-stu-id="b3209-175">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="b3209-176">Un cliente con tipo se puede agregar usando Refit para generar la implementación:</span><span class="sxs-lookup"><span data-stu-id="b3209-176">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddControllers();
}
```

<span data-ttu-id="b3209-177">La interfaz definida se puede usar cuando sea preciso con la implementación proporcionada por la inserción de dependencias y Refit:</span><span class="sxs-lookup"><span data-stu-id="b3209-177">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="b3209-178">Middleware de solicitud saliente</span><span class="sxs-lookup"><span data-stu-id="b3209-178">Outgoing request middleware</span></span>

<span data-ttu-id="b3209-179">`HttpClient` tiene el concepto de controladores de delegación, que se pueden vincular entre sí para las solicitudes HTTP salientes.</span><span class="sxs-lookup"><span data-stu-id="b3209-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="b3209-180">`IHttpClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="b3209-180">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="b3209-181">simplifica la definición de controladores que se aplicarán por cada cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="b3209-181">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="b3209-182">Admite el registro y encadenamiento de varios controladores para crear una canalización de middleware de solicitud saliente.</span><span class="sxs-lookup"><span data-stu-id="b3209-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="b3209-183">Cada uno de estos controladores es capaz de realizar la tarea antes y después de la solicitud de salida.</span><span class="sxs-lookup"><span data-stu-id="b3209-183">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="b3209-184">Este patrón:</span><span class="sxs-lookup"><span data-stu-id="b3209-184">This pattern:</span></span>

  * <span data-ttu-id="b3209-185">Es similar a la canalización de middleware de entrada de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b3209-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="b3209-186">Proporciona un mecanismo para administrar los intereses transversales relacionados con las solicitudes HTTP, como:</span><span class="sxs-lookup"><span data-stu-id="b3209-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="b3209-187">el almacenamiento en caché</span><span class="sxs-lookup"><span data-stu-id="b3209-187">caching</span></span>
    * <span data-ttu-id="b3209-188">el control de errores</span><span class="sxs-lookup"><span data-stu-id="b3209-188">error handling</span></span>
    * <span data-ttu-id="b3209-189">la serialización</span><span class="sxs-lookup"><span data-stu-id="b3209-189">serialization</span></span>
    * <span data-ttu-id="b3209-190">logging</span><span class="sxs-lookup"><span data-stu-id="b3209-190">logging</span></span>

<span data-ttu-id="b3209-191">Para crear un controlador de delegación:</span><span class="sxs-lookup"><span data-stu-id="b3209-191">To create a delegating handler:</span></span>

* <span data-ttu-id="b3209-192">Derívelo de <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="b3209-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="b3209-193">Reemplace <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span><span class="sxs-lookup"><span data-stu-id="b3209-193">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="b3209-194">Ejecute el código antes de pasar la solicitud al siguiente controlador de la canalización:</span><span class="sxs-lookup"><span data-stu-id="b3209-194">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="b3209-195">El código anterior comprueba si el encabezado `X-API-KEY` está en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b3209-195">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="b3209-196">Si falta `X-API-KEY`, se devuelve <xref:System.Net.HttpStatusCode.BadRequest>.</span><span class="sxs-lookup"><span data-stu-id="b3209-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="b3209-197">Se puede agregar más de un controlador a la configuración de una instancia de `HttpClient` con <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span><span class="sxs-lookup"><span data-stu-id="b3209-197">More than one handler can be added to the configuration for an `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="b3209-198">En el código anterior, `ValidateHeaderHandler` se ha registrado con inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="b3209-198">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="b3209-199">`IHttpClientFactory` crea un ámbito de inserción de dependencias independiente para cada controlador.</span><span class="sxs-lookup"><span data-stu-id="b3209-199">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="b3209-200">Los controladores pueden depender de servicios de cualquier ámbito.</span><span class="sxs-lookup"><span data-stu-id="b3209-200">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="b3209-201">Los servicios de los que dependen los controladores se eliminan cuando se elimina el controlador.</span><span class="sxs-lookup"><span data-stu-id="b3209-201">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="b3209-202">Una vez registrado, se puede llamar a <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, pasando el tipo del controlador.</span><span class="sxs-lookup"><span data-stu-id="b3209-202">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="b3209-203">Se pueden registrar varios controladores en el orden en que deben ejecutarse.</span><span class="sxs-lookup"><span data-stu-id="b3209-203">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="b3209-204">Cada controlador contiene el siguiente controlador hasta que el último `HttpClientHandler` ejecuta la solicitud:</span><span class="sxs-lookup"><span data-stu-id="b3209-204">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="b3209-205">Use uno de los siguientes enfoques para compartir el estado por solicitud con controladores de mensajes:</span><span class="sxs-lookup"><span data-stu-id="b3209-205">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="b3209-206">Pase datos al controlador mediante [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span><span class="sxs-lookup"><span data-stu-id="b3209-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="b3209-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> para acceder a la solicitud actual.</span><span class="sxs-lookup"><span data-stu-id="b3209-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="b3209-208">Cree un objeto de almacenamiento <xref:System.Threading.AsyncLocal`1> personalizado para pasar los datos.</span><span class="sxs-lookup"><span data-stu-id="b3209-208">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="b3209-209">Usar controladores basados en Polly</span><span class="sxs-lookup"><span data-stu-id="b3209-209">Use Polly-based handlers</span></span>

<span data-ttu-id="b3209-210">`IHttpClientFactory` se integra con la biblioteca de terceros [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="b3209-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="b3209-211">Polly es una biblioteca con capacidades de resistencia y control de errores transitorios para .NET.</span><span class="sxs-lookup"><span data-stu-id="b3209-211">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="b3209-212">Permite a los desarrolladores expresar directivas como, por ejemplo, de reintento, interruptor, tiempo de espera, aislamiento compartimentado y reserva de forma fluida y segura para los subprocesos.</span><span class="sxs-lookup"><span data-stu-id="b3209-212">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="b3209-213">Se proporcionan métodos de extensión para hacer posible el uso de directivas de Polly con instancias de `HttpClient` configuradas.</span><span class="sxs-lookup"><span data-stu-id="b3209-213">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="b3209-214">Las extensiones de Polly permiten agregar controladores basados en Polly a los clientes.</span><span class="sxs-lookup"><span data-stu-id="b3209-214">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="b3209-215">Polly requiere el paquete NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="b3209-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="b3209-216">Control de errores transitorios</span><span class="sxs-lookup"><span data-stu-id="b3209-216">Handle transient faults</span></span>

<span data-ttu-id="b3209-217">Los errores se suelen producir cuando las llamadas HTTP externas son transitorias.</span><span class="sxs-lookup"><span data-stu-id="b3209-217">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="b3209-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> permite definir una directiva para controlar los errores transitorios.</span><span class="sxs-lookup"><span data-stu-id="b3209-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="b3209-219">Las directivas configuradas con `AddTransientHttpErrorPolicy` controlan las respuestas siguientes:</span><span class="sxs-lookup"><span data-stu-id="b3209-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="b3209-220">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="b3209-220">HTTP 5xx</span></span>
* <span data-ttu-id="b3209-221">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="b3209-221">HTTP 408</span></span>

<span data-ttu-id="b3209-222">`AddTransientHttpErrorPolicy` proporciona acceso a un objeto `PolicyBuilder` configurado para controlar los errores que representan un posible error transitorio:</span><span class="sxs-lookup"><span data-stu-id="b3209-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="b3209-223">En el código anterior, se define una directiva `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="b3209-223">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="b3209-224">Las solicitudes erróneas se reintentan hasta tres veces con un retardo de 600 ms entre intentos.</span><span class="sxs-lookup"><span data-stu-id="b3209-224">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="b3209-225">Seleccionar directivas dinámicamente</span><span class="sxs-lookup"><span data-stu-id="b3209-225">Dynamically select policies</span></span>

<span data-ttu-id="b3209-226">Los métodos de extensión se proporcionan para agregar controladores basados en Polly, por ejemplo, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span><span class="sxs-lookup"><span data-stu-id="b3209-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="b3209-227">La siguiente sobrecarga de `AddPolicyHandler` inspecciona la solicitud para decidir qué directiva se debe aplicar:</span><span class="sxs-lookup"><span data-stu-id="b3209-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="b3209-228">En el código anterior, si la solicitud GET saliente es del tipo HTTP, se aplica un tiempo de espera de 10 segundos.</span><span class="sxs-lookup"><span data-stu-id="b3209-228">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="b3209-229">En cualquier otro método HTTP, se usa un tiempo de espera de 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="b3209-229">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="b3209-230">Agregar varios controladores de Polly</span><span class="sxs-lookup"><span data-stu-id="b3209-230">Add multiple Polly handlers</span></span>

<span data-ttu-id="b3209-231">Es común anidar las directivas de Polly:</span><span class="sxs-lookup"><span data-stu-id="b3209-231">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="b3209-232">En el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="b3209-232">In the preceding example:</span></span>

* <span data-ttu-id="b3209-233">Se agregan dos controladores.</span><span class="sxs-lookup"><span data-stu-id="b3209-233">Two handlers are added.</span></span>
* <span data-ttu-id="b3209-234">El primer controlador usa <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> para agregar una directiva de reintentos.</span><span class="sxs-lookup"><span data-stu-id="b3209-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="b3209-235">Las solicitudes con error se reintentan hasta tres veces.</span><span class="sxs-lookup"><span data-stu-id="b3209-235">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="b3209-236">La segunda llamada a `AddTransientHttpErrorPolicy` agrega una directiva de interruptor.</span><span class="sxs-lookup"><span data-stu-id="b3209-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="b3209-237">Las solicitudes externas adicionales se bloquean durante 30 segundos si se producen cinco intentos con error seguidos.</span><span class="sxs-lookup"><span data-stu-id="b3209-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="b3209-238">Las directivas de interruptor tienen estado.</span><span class="sxs-lookup"><span data-stu-id="b3209-238">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="b3209-239">Así, todas las llamadas realizadas a través de este cliente comparten el mismo estado de circuito.</span><span class="sxs-lookup"><span data-stu-id="b3209-239">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="b3209-240">Agregar directivas desde el Registro de Polly</span><span class="sxs-lookup"><span data-stu-id="b3209-240">Add policies from the Polly registry</span></span>

<span data-ttu-id="b3209-241">Una forma de administrar las directivas usadas habitualmente consiste en definirlas una vez y registrarlas con `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="b3209-241">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="b3209-242">En el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="b3209-242">In the following code:</span></span>

* <span data-ttu-id="b3209-243">Se agregan las directivas "regular" y "long".</span><span class="sxs-lookup"><span data-stu-id="b3209-243">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="b3209-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*> agrega las directivas "regular" y "long" del registro.</span><span class="sxs-lookup"><span data-stu-id="b3209-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="b3209-245">Para más información sobre `IHttpClientFactory` y las integraciones de Polly, vea la [wiki de Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="b3209-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="b3209-246">HttpClient y administración de la duración</span><span class="sxs-lookup"><span data-stu-id="b3209-246">HttpClient and lifetime management</span></span>

<span data-ttu-id="b3209-247">Cada vez que se llama a `CreateClient` en `IHttpClientFactory`, se devuelve una nueva instancia de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-247">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="b3209-248">Se crea un objeto <xref:System.Net.Http.HttpMessageHandler> por cada cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="b3209-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="b3209-249">La fábrica administra la duración de las instancias de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="b3209-249">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="b3209-250">`IHttpClientFactory` agrupa las instancias de `HttpMessageHandler` creadas por Factory para reducir el consumo de recursos.</span><span class="sxs-lookup"><span data-stu-id="b3209-250">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="b3209-251">Se puede reutilizar una instancia de `HttpMessageHandler` del grupo al crear una instancia de `HttpClient` si su duración aún no ha expirado.</span><span class="sxs-lookup"><span data-stu-id="b3209-251">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="b3209-252">Se recomienda agrupar controladores porque cada uno de ellos normalmente administra sus propias conexiones HTTP subyacentes.</span><span class="sxs-lookup"><span data-stu-id="b3209-252">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="b3209-253">Crear más controladores de los necesarios puede provocar retrasos en la conexión.</span><span class="sxs-lookup"><span data-stu-id="b3209-253">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="b3209-254">Además, algunos controladores dejan las conexiones abiertas de forma indefinida, lo que puede impedir que el controlador reaccione ante los cambios de DNS (Sistema de nombres de dominio).</span><span class="sxs-lookup"><span data-stu-id="b3209-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="b3209-255">La duración de controlador predeterminada es dos minutos.</span><span class="sxs-lookup"><span data-stu-id="b3209-255">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="b3209-256">El valor predeterminado se puede reemplazar de forma individual en cada cliente con nombre:</span><span class="sxs-lookup"><span data-stu-id="b3209-256">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="b3209-257">Normalmente, las instancias de `HttpClient` se pueden trata como objetos de .NET que **no** requieren eliminación.</span><span class="sxs-lookup"><span data-stu-id="b3209-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="b3209-258">ya que se cancelan las solicitudes salientes y la instancia de `HttpClient` determinada no se puede usar después de llamar a <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="b3209-258">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="b3209-259">`IHttpClientFactory` realiza un seguimiento y elimina los recursos que usan las instancias de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-259">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="b3209-260">Mantener una sola instancia de `HttpClient` activa durante un período prolongado es un patrón común que se utiliza antes de la concepción de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="b3209-260">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="b3209-261">Este patrón se convierte en innecesario tras la migración a `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="b3209-261">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="b3209-262">Alternativas a IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="b3209-262">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="b3209-263">El uso de `IHttpClientFactory` en una aplicación habilitada para la inserción de dependencias evita lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b3209-263">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="b3209-264">Problemas de agotamiento de recursos mediante la agrupación de instancias de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="b3209-264">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="b3209-265">Problemas de DNS obsoletos al recorrer las instancias de `HttpMessageHandler` a intervalos regulares.</span><span class="sxs-lookup"><span data-stu-id="b3209-265">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="b3209-266">Existen formas alternativas de solucionar los problemas anteriores mediante una instancia de <xref:System.Net.Http.SocketsHttpHandler> de larga duración.</span><span class="sxs-lookup"><span data-stu-id="b3209-266">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="b3209-267">Cree una instancia de `SocketsHttpHandler` al iniciar la aplicación y úsela para la vida útil de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b3209-267">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="b3209-268">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> con un valor adecuado en función de los tiempos de actualización de DNS.</span><span class="sxs-lookup"><span data-stu-id="b3209-268">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="b3209-269">Cree instancias de `HttpClient` mediante `new HttpClient(handler, dispostHandler: false)` según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="b3209-269">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="b3209-270">Los enfoques anteriores solucionan los problemas de administración de recursos que `IHttpClientFactory` resuelve de forma similar.</span><span class="sxs-lookup"><span data-stu-id="b3209-270">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="b3209-271">`SocketsHttpHandler` comparte las conexiones entre las instancias de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-271">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="b3209-272">Este uso compartido impide el agotamiento del socket.</span><span class="sxs-lookup"><span data-stu-id="b3209-272">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="b3209-273">`SocketsHttpHandler` recorre las conexiones según `PooledConnectionLifetime` para evitar problemas de DNS obsoletos.</span><span class="sxs-lookup"><span data-stu-id="b3209-273">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="b3209-274">Cookies</span><span class="sxs-lookup"><span data-stu-id="b3209-274">Cookies</span></span>

<span data-ttu-id="b3209-275">Las instancias de `HttpMessageHandler` agrupadas generan objetos `CookieContainer` que se comparten.</span><span class="sxs-lookup"><span data-stu-id="b3209-275">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="b3209-276">El uso compartido de objetos `CookieContainer` no previsto suele generar código incorrecto.</span><span class="sxs-lookup"><span data-stu-id="b3209-276">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="b3209-277">En el caso de las aplicaciones que requieren cookies, tenga en cuenta lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b3209-277">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="b3209-278">Deshabilitar el control automático de las cookies</span><span class="sxs-lookup"><span data-stu-id="b3209-278">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="b3209-279">Evitar `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="b3209-279">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="b3209-280">Llame a <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> para deshabilitar el control automático de cookies:</span><span class="sxs-lookup"><span data-stu-id="b3209-280">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="b3209-281">Registro</span><span class="sxs-lookup"><span data-stu-id="b3209-281">Logging</span></span>

<span data-ttu-id="b3209-282">Los clientes que se han creado a través de `IHttpClientFactory` registran mensajes de registro de todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="b3209-282">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="b3209-283">Habilite el nivel de información adecuado en la configuración del registro para ver los mensajes de registro predeterminados.</span><span class="sxs-lookup"><span data-stu-id="b3209-283">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="b3209-284">El registro de más información, como el registro de encabezados de solicitud, solo se incluye en el nivel de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="b3209-284">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="b3209-285">La categoría de registro usada en cada cliente incluye el nombre del cliente.</span><span class="sxs-lookup"><span data-stu-id="b3209-285">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="b3209-286">Un cliente denominado *MyNamedClient*, por ejemplo, registra mensajes con una categoría de "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span><span class="sxs-lookup"><span data-stu-id="b3209-286">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="b3209-287">Los mensajes con el sufijo *LogicalHandler* se producen fuera de la canalización de controlador de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b3209-287">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="b3209-288">En la solicitud, los mensajes se registran antes de que cualquier otro controlador de la canalización haya procesado la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b3209-288">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="b3209-289">En la respuesta, los mensajes se registran después de que cualquier otro controlador de la canalización haya recibido la respuesta.</span><span class="sxs-lookup"><span data-stu-id="b3209-289">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="b3209-290">El registro también se produce dentro de la canalización de controlador de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b3209-290">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="b3209-291">En el ejemplo *MyNamedClient*, esos mensajes se registran con la categoría de registro "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span><span class="sxs-lookup"><span data-stu-id="b3209-291">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="b3209-292">En la solicitud, esto tiene lugar después de que todos los demás controladores se hayan ejecutado y justo antes de que se envíe la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b3209-292">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="b3209-293">En la respuesta, este registro incluye el estado de la respuesta antes de que vuelva a pasar por la canalización del controlador.</span><span class="sxs-lookup"><span data-stu-id="b3209-293">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="b3209-294">Al habilitar el registro tanto dentro como fuera de la canalización, se podrán inspeccionar los cambios realizados por otros controladores de la canalización.</span><span class="sxs-lookup"><span data-stu-id="b3209-294">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="b3209-295">Esto puede incluir cambios en los encabezados de solicitud o en el código de estado de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="b3209-295">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="b3209-296">La inclusión del nombre del cliente en la categoría de registro permite filtrar el registro para clientes con nombre específicos.</span><span class="sxs-lookup"><span data-stu-id="b3209-296">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="b3209-297">Configurar HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="b3209-297">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="b3209-298">Puede que sea necesario controlar la configuración del elemento `HttpMessageHandler` interno usado por un cliente.</span><span class="sxs-lookup"><span data-stu-id="b3209-298">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="b3209-299">Se devuelve un `IHttpClientBuilder` cuando se agregan clientes con nombre o con tipo.</span><span class="sxs-lookup"><span data-stu-id="b3209-299">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="b3209-300">Se puede usar el método de extensión <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> para definir un delegado.</span><span class="sxs-lookup"><span data-stu-id="b3209-300">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="b3209-301">Este delegado servirá para crear y configurar el elemento principal `HttpMessageHandler` que ese cliente usa:</span><span class="sxs-lookup"><span data-stu-id="b3209-301">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="b3209-302">Uso de IHttpClientFactory en una aplicación de consola</span><span class="sxs-lookup"><span data-stu-id="b3209-302">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="b3209-303">En una aplicación de consola, agregue las siguientes referencias de paquete al proyecto:</span><span class="sxs-lookup"><span data-stu-id="b3209-303">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="b3209-304">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="b3209-304">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="b3209-305">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="b3209-305">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="b3209-306">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b3209-306">In the following example:</span></span>

* <span data-ttu-id="b3209-307"><xref:System.Net.Http.IHttpClientFactory> está registrado en el contenedor de servicios del [host genérico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="b3209-307"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="b3209-308">`MyService` crea una instancia de generador de clientes a partir del servicio, que se usa para crear un elemento `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-308">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="b3209-309">`HttpClient` se utiliza para recuperar una página web.</span><span class="sxs-lookup"><span data-stu-id="b3209-309">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="b3209-310">`Main` crea un ámbito para ejecutar el método `GetPage` del servicio y escribe los primeros 500 caracteres del contenido de la página web en la consola.</span><span class="sxs-lookup"><span data-stu-id="b3209-310">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="b3209-311">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b3209-311">Additional resources</span></span>

* [<span data-ttu-id="b3209-312">Uso de HttpClientFactory para implementar solicitudes HTTP resistentes</span><span class="sxs-lookup"><span data-stu-id="b3209-312">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="b3209-313">Implementación de reintentos de llamada HTTP con retroceso exponencial con HttpClientFactory y las directivas de Polly</span><span class="sxs-lookup"><span data-stu-id="b3209-313">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="b3209-314">Implementación del patrón de interruptor</span><span class="sxs-lookup"><span data-stu-id="b3209-314">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="b3209-315">Por [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) y [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="b3209-315">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="b3209-316">Se puede registrar y usar una interfaz <xref:System.Net.Http.IHttpClientFactory> para crear y configurar instancias de <xref:System.Net.Http.HttpClient> en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="b3209-316">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="b3209-317">Esto reporta las siguientes ventajas:</span><span class="sxs-lookup"><span data-stu-id="b3209-317">It offers the following benefits:</span></span>

* <span data-ttu-id="b3209-318">Proporciona una ubicación central para denominar y configurar instancias de `HttpClient` lógicas.</span><span class="sxs-lookup"><span data-stu-id="b3209-318">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="b3209-319">Así, por ejemplo, se puede registrar y configurar un cliente *github* para tener acceso a [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="b3209-319">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="b3209-320">y, de igual modo, registrar otro cliente predeterminado para otros fines.</span><span class="sxs-lookup"><span data-stu-id="b3209-320">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="b3209-321">Codifica el concepto de middleware saliente a través de controladores de delegación en `HttpClient` y proporciona extensiones para middleware basado en Polly para poder sacar partido de este.</span><span class="sxs-lookup"><span data-stu-id="b3209-321">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="b3209-322">Administra la agrupación y duración de las instancias de `HttpClientMessageHandler` subyacentes para evitar los problemas de DNS que suelen producirse al administrar las duraciones de `HttpClient` manualmente.</span><span class="sxs-lookup"><span data-stu-id="b3209-322">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="b3209-323">Agrega una experiencia de registro configurable (a través de `ILogger`) en todas las solicitudes enviadas a través de los clientes creados por Factory.</span><span class="sxs-lookup"><span data-stu-id="b3209-323">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="b3209-324">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b3209-324">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="b3209-325">Patrones de consumo</span><span class="sxs-lookup"><span data-stu-id="b3209-325">Consumption patterns</span></span>

<span data-ttu-id="b3209-326">`IHttpClientFactory` se puede usar de varias formas en una aplicación:</span><span class="sxs-lookup"><span data-stu-id="b3209-326">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="b3209-327">Uso básico</span><span class="sxs-lookup"><span data-stu-id="b3209-327">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="b3209-328">Clientes con nombre</span><span class="sxs-lookup"><span data-stu-id="b3209-328">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="b3209-329">Clientes con tipo</span><span class="sxs-lookup"><span data-stu-id="b3209-329">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="b3209-330">Clientes generados</span><span class="sxs-lookup"><span data-stu-id="b3209-330">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="b3209-331">Ninguno de ellos es rigurosamente superior a otro,</span><span class="sxs-lookup"><span data-stu-id="b3209-331">None of them are strictly superior to another.</span></span> <span data-ttu-id="b3209-332">sino que el mejor método dependerá de las restricciones particulares de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b3209-332">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="b3209-333">Uso básico</span><span class="sxs-lookup"><span data-stu-id="b3209-333">Basic usage</span></span>

<span data-ttu-id="b3209-334">Se puede registrar un `IHttpClientFactory` llamando al método de extensión `AddHttpClient` en `IServiceCollection`, dentro del método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b3209-334">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="b3209-335">Una vez registrado, el código puede aceptar una interfaz `IHttpClientFactory` en cualquier parte donde se puedan insertar servicios por medio de la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b3209-335">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="b3209-336">`IHttpClientFactory` se puede usar para crear una instancia de `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="b3209-336">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="b3209-337">Cuando `IHttpClientFactory` se usa de este modo, constituye una excelente manera de refactorizar una aplicación existente.</span><span class="sxs-lookup"><span data-stu-id="b3209-337">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="b3209-338">No tiene efecto alguno en la forma en que `HttpClient` se usa.</span><span class="sxs-lookup"><span data-stu-id="b3209-338">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="b3209-339">En aquellos sitios en los que ya se hayan creado instancias de `HttpClient`, reemplace esas apariciones por una llamada a <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="b3209-339">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="b3209-340">Clientes con nombre</span><span class="sxs-lookup"><span data-stu-id="b3209-340">Named clients</span></span>

<span data-ttu-id="b3209-341">Si una aplicación necesita usar `HttpClient` de diversas maneras, cada una con una configuración diferente, una opción consiste en usar **clientes con nombre**.</span><span class="sxs-lookup"><span data-stu-id="b3209-341">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="b3209-342">La configuración de un `HttpClient` con nombre se puede realizar durante la fase de registro en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b3209-342">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="b3209-343">En el código anterior, se llama a `AddHttpClient` usando el nombre *github*.</span><span class="sxs-lookup"><span data-stu-id="b3209-343">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="b3209-344">Este cliente tiene aplicadas algunas configuraciones predeterminadas, a saber, la dirección base y dos encabezados necesarios para trabajar con la API de GitHub.</span><span class="sxs-lookup"><span data-stu-id="b3209-344">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="b3209-345">Cada vez que se llama a `CreateClient`, se crea otra instancia de `HttpClient` y se llama a la acción de configuración.</span><span class="sxs-lookup"><span data-stu-id="b3209-345">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="b3209-346">Para consumir un cliente con nombre, se puede pasar un parámetro de cadena a `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-346">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="b3209-347">Especifique el nombre del cliente que se va a crear:</span><span class="sxs-lookup"><span data-stu-id="b3209-347">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="b3209-348">En el código anterior, no es necesario especificar un nombre de host en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b3209-348">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="b3209-349">Basta con pasar solo la ruta de acceso, ya que se usa la dirección base configurada del cliente.</span><span class="sxs-lookup"><span data-stu-id="b3209-349">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="b3209-350">Clientes con tipo</span><span class="sxs-lookup"><span data-stu-id="b3209-350">Typed clients</span></span>

<span data-ttu-id="b3209-351">Clientes con tipo:</span><span class="sxs-lookup"><span data-stu-id="b3209-351">Typed clients:</span></span>

* <span data-ttu-id="b3209-352">Proporcionan las mismas funciones que los clientes con nombre sin la necesidad de usar cadenas como claves.</span><span class="sxs-lookup"><span data-stu-id="b3209-352">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="b3209-353">Ofrecen ayuda relativa al compilador e IntelliSense al consumir clientes.</span><span class="sxs-lookup"><span data-stu-id="b3209-353">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="b3209-354">Facilitan una sola ubicación para configurar un elemento `HttpClient` determinado e interactuar con él.</span><span class="sxs-lookup"><span data-stu-id="b3209-354">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="b3209-355">Por ejemplo, el mismo cliente con tipo se puede usar para un punto de conexión back-end único y encapsular toda la lógica que se ocupa de ese punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="b3209-355">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="b3209-356">Funcionan con la inserción de dependencias y se pueden insertar en la aplicación cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="b3209-356">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="b3209-357">Un cliente con tipo acepta un parámetro `HttpClient` en su constructor:</span><span class="sxs-lookup"><span data-stu-id="b3209-357">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="b3209-358">En el código anterior, la configuración se mueve al cliente con tipo.</span><span class="sxs-lookup"><span data-stu-id="b3209-358">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="b3209-359">El objeto `HttpClient` se expone como una propiedad pública.</span><span class="sxs-lookup"><span data-stu-id="b3209-359">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="b3209-360">Se pueden definir métodos específicos de API que exponen la funcionalidad `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-360">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="b3209-361">El método `GetAspNetDocsIssues` encapsula el código necesario para consultar y analizar los últimos problemas abiertos de un repositorio de GitHub.</span><span class="sxs-lookup"><span data-stu-id="b3209-361">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="b3209-362">Para registrar un cliente con tipo, se puede usar el método de extensión genérico <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> dentro de `Startup.ConfigureServices`, especificando la clase del cliente con tipo:</span><span class="sxs-lookup"><span data-stu-id="b3209-362">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="b3209-363">El cliente con tipo se registra como transitorio con inserción con dependencias,</span><span class="sxs-lookup"><span data-stu-id="b3209-363">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="b3209-364">y se puede insertar y consumir directamente:</span><span class="sxs-lookup"><span data-stu-id="b3209-364">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="b3209-365">Si lo prefiere, la configuración de un cliente con nombre se puede especificar durante su registro en `Startup.ConfigureServices`, en lugar de en su constructor:</span><span class="sxs-lookup"><span data-stu-id="b3209-365">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="b3209-366">El `HttpClient` se puede encapsular completamente dentro de un cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="b3209-366">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="b3209-367">En lugar de exponerlo como una propiedad, se pueden proporcionar métodos públicos que llamen a la instancia de `HttpClient` internamente.</span><span class="sxs-lookup"><span data-stu-id="b3209-367">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="b3209-368">En el código anterior, el `HttpClient` se almacena como un campo privado.</span><span class="sxs-lookup"><span data-stu-id="b3209-368">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="b3209-369">Todo el acceso para realizar llamadas externas pasa por el método `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="b3209-369">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="b3209-370">Clientes generados</span><span class="sxs-lookup"><span data-stu-id="b3209-370">Generated clients</span></span>

<span data-ttu-id="b3209-371">`IHttpClientFactory` se puede usar en combinación con otras bibliotecas de terceros, como [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="b3209-371">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="b3209-372">Refit es una biblioteca de REST para .NET</span><span class="sxs-lookup"><span data-stu-id="b3209-372">Refit is a REST library for .NET.</span></span> <span data-ttu-id="b3209-373">que convierte las API de REST en interfaces en vivo.</span><span class="sxs-lookup"><span data-stu-id="b3209-373">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="b3209-374">Se genera una implementación de la interfaz dinámicamente por medio de `RestService`, usando `HttpClient` para realizar las llamadas HTTP externas.</span><span class="sxs-lookup"><span data-stu-id="b3209-374">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="b3209-375">Se define una interfaz y una respuesta para representar la API externa y su correspondiente respuesta:</span><span class="sxs-lookup"><span data-stu-id="b3209-375">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="b3209-376">Un cliente con tipo se puede agregar usando Refit para generar la implementación:</span><span class="sxs-lookup"><span data-stu-id="b3209-376">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("https://localhost:5001");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="b3209-377">La interfaz definida se puede usar cuando sea preciso con la implementación proporcionada por la inserción de dependencias y Refit:</span><span class="sxs-lookup"><span data-stu-id="b3209-377">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="b3209-378">Middleware de solicitud saliente</span><span class="sxs-lookup"><span data-stu-id="b3209-378">Outgoing request middleware</span></span>

<span data-ttu-id="b3209-379">`HttpClient` ya posee el concepto de controladores de delegación, que se pueden vincular entre sí para las solicitudes HTTP salientes.</span><span class="sxs-lookup"><span data-stu-id="b3209-379">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="b3209-380">`IHttpClientFactory` permite definir fácilmente los controladores que se usarán en cada cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="b3209-380">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="b3209-381">Admite el registro y encadenamiento de varios controladores para crear una canalización de middleware de solicitud saliente.</span><span class="sxs-lookup"><span data-stu-id="b3209-381">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="b3209-382">Cada uno de estos controladores es capaz de realizar la tarea antes y después de la solicitud de salida.</span><span class="sxs-lookup"><span data-stu-id="b3209-382">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="b3209-383">Este patrón es similar a la canalización de middleware de entrada de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b3209-383">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="b3209-384">Dicho patrón proporciona un mecanismo para administrar cuestiones transversales relativas a las solicitudes HTTP, como el almacenamiento en caché, el control de errores, la serialización y el registro.</span><span class="sxs-lookup"><span data-stu-id="b3209-384">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="b3209-385">Para crear un controlador, defina una clase que se derive de <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="b3209-385">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="b3209-386">Invalide el método `SendAsync` para ejecutar el código antes de pasar la solicitud al siguiente controlador de la canalización:</span><span class="sxs-lookup"><span data-stu-id="b3209-386">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="b3209-387">El código anterior define un controlador básico.</span><span class="sxs-lookup"><span data-stu-id="b3209-387">The preceding code defines a basic handler.</span></span> <span data-ttu-id="b3209-388">Comprueba si se ha incluido un encabezado `X-API-KEY` en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b3209-388">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="b3209-389">Si no está presente, puede evitar la llamada HTTP y devolver una respuesta adecuada.</span><span class="sxs-lookup"><span data-stu-id="b3209-389">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="b3209-390">Durante el registro, se pueden agregar uno o varios controladores a la configuración de una instancia de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-390">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="b3209-391">Esta tarea se realiza a través de métodos de extensión en <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b3209-391">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="b3209-392">En el código anterior, `ValidateHeaderHandler` se ha registrado con inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="b3209-392">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="b3209-393">`IHttpClientFactory` crea un ámbito de inserción de dependencias independiente para cada controlador.</span><span class="sxs-lookup"><span data-stu-id="b3209-393">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="b3209-394">Los controladores pueden depender de servicios de cualquier ámbito.</span><span class="sxs-lookup"><span data-stu-id="b3209-394">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="b3209-395">Los servicios de los que dependen los controladores se eliminan cuando se elimina el controlador.</span><span class="sxs-lookup"><span data-stu-id="b3209-395">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="b3209-396">Una vez registrado, se puede llamar a <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, pasando el tipo del controlador.</span><span class="sxs-lookup"><span data-stu-id="b3209-396">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="b3209-397">Se pueden registrar varios controladores en el orden en que deben ejecutarse.</span><span class="sxs-lookup"><span data-stu-id="b3209-397">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="b3209-398">Cada controlador contiene el siguiente controlador hasta que el último `HttpClientHandler` ejecuta la solicitud:</span><span class="sxs-lookup"><span data-stu-id="b3209-398">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="b3209-399">Use uno de los siguientes enfoques para compartir el estado por solicitud con controladores de mensajes:</span><span class="sxs-lookup"><span data-stu-id="b3209-399">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="b3209-400">Pase datos al controlador usando `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="b3209-400">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="b3209-401">Use `IHttpContextAccessor` para acceder a la solicitud actual.</span><span class="sxs-lookup"><span data-stu-id="b3209-401">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="b3209-402">Cree un objeto de almacenamiento `AsyncLocal` personalizado para pasar los datos.</span><span class="sxs-lookup"><span data-stu-id="b3209-402">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="b3209-403">Usar controladores basados en Polly</span><span class="sxs-lookup"><span data-stu-id="b3209-403">Use Polly-based handlers</span></span>

<span data-ttu-id="b3209-404">`IHttpClientFactory` se integra con una biblioteca de terceros muy conocida denominada [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="b3209-404">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="b3209-405">Polly es una biblioteca con capacidades de resistencia y control de errores transitorios para .NET.</span><span class="sxs-lookup"><span data-stu-id="b3209-405">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="b3209-406">Permite a los desarrolladores expresar directivas como, por ejemplo, de reintento, interruptor, tiempo de espera, aislamiento compartimentado y reserva de forma fluida y segura para los subprocesos.</span><span class="sxs-lookup"><span data-stu-id="b3209-406">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="b3209-407">Se proporcionan métodos de extensión para hacer posible el uso de directivas de Polly con instancias de `HttpClient` configuradas.</span><span class="sxs-lookup"><span data-stu-id="b3209-407">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="b3209-408">Las extensiones de Polly:</span><span class="sxs-lookup"><span data-stu-id="b3209-408">The Polly extensions:</span></span>

* <span data-ttu-id="b3209-409">permiten agregar controladores basados en Polly a los clientes;</span><span class="sxs-lookup"><span data-stu-id="b3209-409">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="b3209-410">se pueden usar tras instalar el paquete NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/),</span><span class="sxs-lookup"><span data-stu-id="b3209-410">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="b3209-411">aunque este no está incluido en la plataforma compartida ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b3209-411">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="b3209-412">Control de errores transitorios</span><span class="sxs-lookup"><span data-stu-id="b3209-412">Handle transient faults</span></span>

<span data-ttu-id="b3209-413">Los errores más comunes se producen cuando las llamadas HTTP externas son transitorias.</span><span class="sxs-lookup"><span data-stu-id="b3209-413">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="b3209-414">Por ello, se incluye un método de extensión muy práctico denominado `AddTransientHttpErrorPolicy`, que permite definir una directiva para controlar los errores transitorios.</span><span class="sxs-lookup"><span data-stu-id="b3209-414">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="b3209-415">Las directivas que se configuran con este método de extensión controlan `HttpRequestException`, las respuestas HTTP 5xx y las respuestas HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="b3209-415">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="b3209-416">La extensión `AddTransientHttpErrorPolicy` se puede usar en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b3209-416">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b3209-417">La extensión da acceso a un objeto `PolicyBuilder`, configurado para controlar los errores que pueden constituir un posible error transitorio:</span><span class="sxs-lookup"><span data-stu-id="b3209-417">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="b3209-418">En el código anterior, se define una directiva `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="b3209-418">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="b3209-419">Las solicitudes erróneas se reintentan hasta tres veces con un retardo de 600 ms entre intentos.</span><span class="sxs-lookup"><span data-stu-id="b3209-419">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="b3209-420">Seleccionar directivas dinámicamente</span><span class="sxs-lookup"><span data-stu-id="b3209-420">Dynamically select policies</span></span>

<span data-ttu-id="b3209-421">Existen más métodos de extensión que pueden servir para agregar controladores basados en Polly.</span><span class="sxs-lookup"><span data-stu-id="b3209-421">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="b3209-422">Una de esas extensiones es `AddPolicyHandler`, que tiene varias sobrecargas.</span><span class="sxs-lookup"><span data-stu-id="b3209-422">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="b3209-423">Una de esas sobrecargas permite inspeccionar la solicitud al dilucidar qué directiva aplicar:</span><span class="sxs-lookup"><span data-stu-id="b3209-423">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="b3209-424">En el código anterior, si la solicitud GET saliente es del tipo HTTP, se aplica un tiempo de espera de 10 segundos.</span><span class="sxs-lookup"><span data-stu-id="b3209-424">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="b3209-425">En cualquier otro método HTTP, se usa un tiempo de espera de 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="b3209-425">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="b3209-426">Agregar varios controladores de Polly</span><span class="sxs-lookup"><span data-stu-id="b3209-426">Add multiple Polly handlers</span></span>

<span data-ttu-id="b3209-427">Es habitual anidar directivas de Polly para proporcionar una funcionalidad mejorada:</span><span class="sxs-lookup"><span data-stu-id="b3209-427">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="b3209-428">En el ejemplo anterior, se agregan dos controladores.</span><span class="sxs-lookup"><span data-stu-id="b3209-428">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="b3209-429">En el primer ejemplo se usa la extensión `AddTransientHttpErrorPolicy` para agregar una directiva de reintento.</span><span class="sxs-lookup"><span data-stu-id="b3209-429">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="b3209-430">Las solicitudes con error se reintentan hasta tres veces.</span><span class="sxs-lookup"><span data-stu-id="b3209-430">Failed requests are retried up to three times.</span></span> <span data-ttu-id="b3209-431">La segunda llamada a `AddTransientHttpErrorPolicy` agrega una directiva de interruptor.</span><span class="sxs-lookup"><span data-stu-id="b3209-431">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="b3209-432">Las solicitudes externas subsiguientes se bloquean durante 30 segundos si se producen cinco intentos infructuosos seguidos.</span><span class="sxs-lookup"><span data-stu-id="b3209-432">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="b3209-433">Las directivas de interruptor tienen estado.</span><span class="sxs-lookup"><span data-stu-id="b3209-433">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="b3209-434">Así, todas las llamadas realizadas a través de este cliente comparten el mismo estado de circuito.</span><span class="sxs-lookup"><span data-stu-id="b3209-434">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="b3209-435">Agregar directivas desde el Registro de Polly</span><span class="sxs-lookup"><span data-stu-id="b3209-435">Add policies from the Polly registry</span></span>

<span data-ttu-id="b3209-436">Una forma de administrar las directivas usadas habitualmente consiste en definirlas una vez y registrarlas con `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="b3209-436">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="b3209-437">Se proporciona un método de extensión que permite agregar un controlador por medio de una directiva del Registro:</span><span class="sxs-lookup"><span data-stu-id="b3209-437">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="b3209-438">En el código anterior, se registran dos directivas cuando se agrega `PolicyRegistry` a `ServiceCollection`.</span><span class="sxs-lookup"><span data-stu-id="b3209-438">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="b3209-439">Para usar una directiva del Registro, se usa el método `AddPolicyHandlerFromRegistry` pasando el nombre de la directiva que se va a aplicar.</span><span class="sxs-lookup"><span data-stu-id="b3209-439">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="b3209-440">Encontrará más información sobre `IHttpClientFactory` y las integraciones de Polly en la [wiki de Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="b3209-440">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="b3209-441">HttpClient y administración de la duración</span><span class="sxs-lookup"><span data-stu-id="b3209-441">HttpClient and lifetime management</span></span>

<span data-ttu-id="b3209-442">Cada vez que se llama a `CreateClient` en `IHttpClientFactory`, se devuelve una nueva instancia de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-442">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="b3209-443">Hay un controlador <xref:System.Net.Http.HttpMessageHandler> por cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="b3209-443">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="b3209-444">La fábrica administra la duración de las instancias de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="b3209-444">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="b3209-445">`IHttpClientFactory` agrupa las instancias de `HttpMessageHandler` creadas por Factory para reducir el consumo de recursos.</span><span class="sxs-lookup"><span data-stu-id="b3209-445">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="b3209-446">Se puede reutilizar una instancia de `HttpMessageHandler` del grupo al crear una instancia de `HttpClient` si su duración aún no ha expirado.</span><span class="sxs-lookup"><span data-stu-id="b3209-446">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="b3209-447">Se recomienda agrupar controladores porque cada uno de ellos normalmente administra sus propias conexiones HTTP subyacentes.</span><span class="sxs-lookup"><span data-stu-id="b3209-447">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="b3209-448">Crear más controladores de los necesarios puede provocar retrasos en la conexión.</span><span class="sxs-lookup"><span data-stu-id="b3209-448">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="b3209-449">Además, algunos controladores dejan las conexiones abiertas de forma indefinida, lo que puede ser un obstáculo a la hora de reaccionar ante los cambios de DNS.</span><span class="sxs-lookup"><span data-stu-id="b3209-449">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="b3209-450">La duración de controlador predeterminada es dos minutos.</span><span class="sxs-lookup"><span data-stu-id="b3209-450">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="b3209-451">El valor predeterminado se puede reemplazar individualmente en cada cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="b3209-451">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="b3209-452">Para ello, llame a <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> en el `IHttpClientBuilder` que se devuelve cuando se crea el cliente:</span><span class="sxs-lookup"><span data-stu-id="b3209-452">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="b3209-453">No hace falta eliminar el cliente,</span><span class="sxs-lookup"><span data-stu-id="b3209-453">Disposal of the client isn't required.</span></span> <span data-ttu-id="b3209-454">ya que se cancelan las solicitudes salientes y la instancia de `HttpClient` determinada no se puede usar después de llamar a <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="b3209-454">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="b3209-455">`IHttpClientFactory` realiza un seguimiento y elimina los recursos que usan las instancias de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-455">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="b3209-456">Normalmente, las instancias de `HttpClient` pueden tratarse como objetos de .NET que no requieren eliminación.</span><span class="sxs-lookup"><span data-stu-id="b3209-456">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="b3209-457">Mantener una sola instancia de `HttpClient` activa durante un período prolongado es un patrón común que se utiliza antes de la concepción de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="b3209-457">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="b3209-458">Este patrón se convierte en innecesario tras la migración a `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="b3209-458">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="b3209-459">Alternativas a IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="b3209-459">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="b3209-460">El uso de `IHttpClientFactory` en una aplicación habilitada para la inserción de dependencias evita lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b3209-460">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="b3209-461">Problemas de agotamiento de recursos mediante la agrupación de instancias de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="b3209-461">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="b3209-462">Problemas de DNS obsoletos al recorrer las instancias de `HttpMessageHandler` a intervalos regulares.</span><span class="sxs-lookup"><span data-stu-id="b3209-462">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="b3209-463">Existen formas alternativas de solucionar los problemas anteriores mediante una instancia de <xref:System.Net.Http.SocketsHttpHandler> de larga duración.</span><span class="sxs-lookup"><span data-stu-id="b3209-463">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="b3209-464">Cree una instancia de `SocketsHttpHandler` al iniciar la aplicación y úsela para la vida útil de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b3209-464">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="b3209-465">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> con un valor adecuado en función de los tiempos de actualización de DNS.</span><span class="sxs-lookup"><span data-stu-id="b3209-465">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="b3209-466">Cree instancias de `HttpClient` mediante `new HttpClient(handler, dispostHandler: false)` según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="b3209-466">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="b3209-467">Los enfoques anteriores solucionan los problemas de administración de recursos que `IHttpClientFactory` resuelve de forma similar.</span><span class="sxs-lookup"><span data-stu-id="b3209-467">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="b3209-468">`SocketsHttpHandler` comparte las conexiones entre las instancias de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-468">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="b3209-469">Este uso compartido impide el agotamiento del socket.</span><span class="sxs-lookup"><span data-stu-id="b3209-469">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="b3209-470">`SocketsHttpHandler` recorre las conexiones según `PooledConnectionLifetime` para evitar problemas de DNS obsoletos.</span><span class="sxs-lookup"><span data-stu-id="b3209-470">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="b3209-471">Cookies</span><span class="sxs-lookup"><span data-stu-id="b3209-471">Cookies</span></span>

<span data-ttu-id="b3209-472">Las instancias de `HttpMessageHandler` agrupadas generan objetos `CookieContainer` que se comparten.</span><span class="sxs-lookup"><span data-stu-id="b3209-472">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="b3209-473">El uso compartido de objetos `CookieContainer` no previsto suele generar código incorrecto.</span><span class="sxs-lookup"><span data-stu-id="b3209-473">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="b3209-474">En el caso de las aplicaciones que requieren cookies, tenga en cuenta lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b3209-474">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="b3209-475">Deshabilitar el control automático de las cookies</span><span class="sxs-lookup"><span data-stu-id="b3209-475">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="b3209-476">Evitar `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="b3209-476">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="b3209-477">Llame a <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> para deshabilitar el control automático de cookies:</span><span class="sxs-lookup"><span data-stu-id="b3209-477">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="b3209-478">Registro</span><span class="sxs-lookup"><span data-stu-id="b3209-478">Logging</span></span>

<span data-ttu-id="b3209-479">Los clientes que se han creado a través de `IHttpClientFactory` registran mensajes de registro de todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="b3209-479">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="b3209-480">Habilite el nivel de información adecuado en la configuración del registro para ver los mensajes de registro predeterminados.</span><span class="sxs-lookup"><span data-stu-id="b3209-480">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="b3209-481">El registro de más información, como el registro de encabezados de solicitud, solo se incluye en el nivel de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="b3209-481">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="b3209-482">La categoría de registro usada en cada cliente incluye el nombre del cliente.</span><span class="sxs-lookup"><span data-stu-id="b3209-482">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="b3209-483">Así, por ejemplo, un cliente llamado *MyNamedClient* registra mensajes con una categoría `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="b3209-483">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="b3209-484">Los mensajes con el sufijo *LogicalHandler* se producen fuera de la canalización de controlador de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b3209-484">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="b3209-485">En la solicitud, los mensajes se registran antes de que cualquier otro controlador de la canalización haya procesado la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b3209-485">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="b3209-486">En la respuesta, los mensajes se registran después de que cualquier otro controlador de la canalización haya recibido la respuesta.</span><span class="sxs-lookup"><span data-stu-id="b3209-486">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="b3209-487">El registro también se produce dentro de la canalización de controlador de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b3209-487">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="b3209-488">En nuestro caso de ejemplo de *MyNamedClient*, esos mensajes se registran en la categoría de registro `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="b3209-488">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="b3209-489">En la solicitud, esto tiene lugar después de que todos los demás controladores se hayan ejecutado y justo antes de que la solicitud se envíe por la red.</span><span class="sxs-lookup"><span data-stu-id="b3209-489">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="b3209-490">En la respuesta, este registro incluye el estado de la respuesta antes de que vuelva a pasar por la canalización del controlador.</span><span class="sxs-lookup"><span data-stu-id="b3209-490">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="b3209-491">Al habilitar el registro tanto dentro como fuera de la canalización, se podrán inspeccionar los cambios realizados por otros controladores de la canalización.</span><span class="sxs-lookup"><span data-stu-id="b3209-491">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="b3209-492">Esto puede englobar cambios, por ejemplo, en los encabezados de solicitud o en el código de estado de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="b3209-492">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="b3209-493">Si el nombre del cliente se incluye en la categoría de registro, dicho registro se podrá filtrar para encontrar clientes con nombre específicos cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="b3209-493">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="b3209-494">Configurar HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="b3209-494">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="b3209-495">Puede que sea necesario controlar la configuración del elemento `HttpMessageHandler` interno usado por un cliente.</span><span class="sxs-lookup"><span data-stu-id="b3209-495">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="b3209-496">Se devuelve un `IHttpClientBuilder` cuando se agregan clientes con nombre o con tipo.</span><span class="sxs-lookup"><span data-stu-id="b3209-496">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="b3209-497">Se puede usar el método de extensión <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> para definir un delegado.</span><span class="sxs-lookup"><span data-stu-id="b3209-497">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="b3209-498">Este delegado servirá para crear y configurar el elemento principal `HttpMessageHandler` que ese cliente usa:</span><span class="sxs-lookup"><span data-stu-id="b3209-498">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="b3209-499">Uso de IHttpClientFactory en una aplicación de consola</span><span class="sxs-lookup"><span data-stu-id="b3209-499">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="b3209-500">En una aplicación de consola, agregue las siguientes referencias de paquete al proyecto:</span><span class="sxs-lookup"><span data-stu-id="b3209-500">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="b3209-501">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="b3209-501">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="b3209-502">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="b3209-502">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="b3209-503">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b3209-503">In the following example:</span></span>

* <span data-ttu-id="b3209-504"><xref:System.Net.Http.IHttpClientFactory> está registrado en el contenedor de servicios del [host genérico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="b3209-504"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="b3209-505">`MyService` crea una instancia de generador de clientes a partir del servicio, que se usa para crear un elemento `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-505">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="b3209-506">`HttpClient` se utiliza para recuperar una página web.</span><span class="sxs-lookup"><span data-stu-id="b3209-506">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="b3209-507">`Main` crea un ámbito para ejecutar el método `GetPage` del servicio y escribe los primeros 500 caracteres del contenido de la página web en la consola.</span><span class="sxs-lookup"><span data-stu-id="b3209-507">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="b3209-508">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b3209-508">Additional resources</span></span>

* [<span data-ttu-id="b3209-509">Uso de HttpClientFactory para implementar solicitudes HTTP resistentes</span><span class="sxs-lookup"><span data-stu-id="b3209-509">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="b3209-510">Implementación de reintentos de llamada HTTP con retroceso exponencial con HttpClientFactory y las directivas de Polly</span><span class="sxs-lookup"><span data-stu-id="b3209-510">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="b3209-511">Implementación del patrón de interruptor</span><span class="sxs-lookup"><span data-stu-id="b3209-511">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<span data-ttu-id="b3209-512">Por [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) y [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="b3209-512">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="b3209-513">Se puede registrar y usar una interfaz <xref:System.Net.Http.IHttpClientFactory> para crear y configurar instancias de <xref:System.Net.Http.HttpClient> en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="b3209-513">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="b3209-514">Esto reporta las siguientes ventajas:</span><span class="sxs-lookup"><span data-stu-id="b3209-514">It offers the following benefits:</span></span>

* <span data-ttu-id="b3209-515">Proporciona una ubicación central para denominar y configurar instancias de `HttpClient` lógicas.</span><span class="sxs-lookup"><span data-stu-id="b3209-515">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="b3209-516">Así, por ejemplo, se puede registrar y configurar un cliente *github* para tener acceso a [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="b3209-516">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="b3209-517">y, de igual modo, registrar otro cliente predeterminado para otros fines.</span><span class="sxs-lookup"><span data-stu-id="b3209-517">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="b3209-518">Codifica el concepto de middleware saliente a través de controladores de delegación en `HttpClient` y proporciona extensiones para middleware basado en Polly para poder sacar partido de este.</span><span class="sxs-lookup"><span data-stu-id="b3209-518">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="b3209-519">Administra la agrupación y duración de las instancias de `HttpClientMessageHandler` subyacentes para evitar los problemas de DNS que suelen producirse al administrar las duraciones de `HttpClient` manualmente.</span><span class="sxs-lookup"><span data-stu-id="b3209-519">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="b3209-520">Agrega una experiencia de registro configurable (a través de `ILogger`) en todas las solicitudes enviadas a través de los clientes creados por Factory.</span><span class="sxs-lookup"><span data-stu-id="b3209-520">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="b3209-521">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b3209-521">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b3209-522">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="b3209-522">Prerequisites</span></span>

<span data-ttu-id="b3209-523">Los proyectos para .NET Framework requieren instalar el paquete NuGet [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/).</span><span class="sxs-lookup"><span data-stu-id="b3209-523">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="b3209-524">Los proyectos para .NET Core y que hagan referencia al [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ya incluyen el paquete `Microsoft.Extensions.Http`.</span><span class="sxs-lookup"><span data-stu-id="b3209-524">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="b3209-525">Patrones de consumo</span><span class="sxs-lookup"><span data-stu-id="b3209-525">Consumption patterns</span></span>

<span data-ttu-id="b3209-526">`IHttpClientFactory` se puede usar de varias formas en una aplicación:</span><span class="sxs-lookup"><span data-stu-id="b3209-526">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="b3209-527">Uso básico</span><span class="sxs-lookup"><span data-stu-id="b3209-527">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="b3209-528">Clientes con nombre</span><span class="sxs-lookup"><span data-stu-id="b3209-528">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="b3209-529">Clientes con tipo</span><span class="sxs-lookup"><span data-stu-id="b3209-529">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="b3209-530">Clientes generados</span><span class="sxs-lookup"><span data-stu-id="b3209-530">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="b3209-531">Ninguno de ellos es rigurosamente superior a otro,</span><span class="sxs-lookup"><span data-stu-id="b3209-531">None of them are strictly superior to another.</span></span> <span data-ttu-id="b3209-532">sino que el mejor método dependerá de las restricciones particulares de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b3209-532">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="b3209-533">Uso básico</span><span class="sxs-lookup"><span data-stu-id="b3209-533">Basic usage</span></span>

<span data-ttu-id="b3209-534">Se puede registrar un `IHttpClientFactory` llamando al método de extensión `AddHttpClient` en `IServiceCollection`, dentro del método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b3209-534">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="b3209-535">Una vez registrado, el código puede aceptar una interfaz `IHttpClientFactory` en cualquier parte donde se puedan insertar servicios por medio de la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b3209-535">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="b3209-536">`IHttpClientFactory` se puede usar para crear una instancia de `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="b3209-536">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="b3209-537">Cuando `IHttpClientFactory` se usa de este modo, constituye una excelente manera de refactorizar una aplicación existente.</span><span class="sxs-lookup"><span data-stu-id="b3209-537">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="b3209-538">No tiene efecto alguno en la forma en que `HttpClient` se usa.</span><span class="sxs-lookup"><span data-stu-id="b3209-538">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="b3209-539">En aquellos sitios en los que ya se hayan creado instancias de `HttpClient`, reemplace esas apariciones por una llamada a <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="b3209-539">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="b3209-540">Clientes con nombre</span><span class="sxs-lookup"><span data-stu-id="b3209-540">Named clients</span></span>

<span data-ttu-id="b3209-541">Si una aplicación necesita usar `HttpClient` de diversas maneras, cada una con una configuración diferente, una opción consiste en usar **clientes con nombre**.</span><span class="sxs-lookup"><span data-stu-id="b3209-541">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="b3209-542">La configuración de un `HttpClient` con nombre se puede realizar durante la fase de registro en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b3209-542">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="b3209-543">En el código anterior, se llama a `AddHttpClient` usando el nombre *github*.</span><span class="sxs-lookup"><span data-stu-id="b3209-543">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="b3209-544">Este cliente tiene aplicadas algunas configuraciones predeterminadas, a saber, la dirección base y dos encabezados necesarios para trabajar con la API de GitHub.</span><span class="sxs-lookup"><span data-stu-id="b3209-544">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="b3209-545">Cada vez que se llama a `CreateClient`, se crea otra instancia de `HttpClient` y se llama a la acción de configuración.</span><span class="sxs-lookup"><span data-stu-id="b3209-545">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="b3209-546">Para consumir un cliente con nombre, se puede pasar un parámetro de cadena a `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-546">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="b3209-547">Especifique el nombre del cliente que se va a crear:</span><span class="sxs-lookup"><span data-stu-id="b3209-547">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="b3209-548">En el código anterior, no es necesario especificar un nombre de host en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b3209-548">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="b3209-549">Basta con pasar solo la ruta de acceso, ya que se usa la dirección base configurada del cliente.</span><span class="sxs-lookup"><span data-stu-id="b3209-549">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="b3209-550">Clientes con tipo</span><span class="sxs-lookup"><span data-stu-id="b3209-550">Typed clients</span></span>

<span data-ttu-id="b3209-551">Clientes con tipo:</span><span class="sxs-lookup"><span data-stu-id="b3209-551">Typed clients:</span></span>

* <span data-ttu-id="b3209-552">Proporcionan las mismas funciones que los clientes con nombre sin la necesidad de usar cadenas como claves.</span><span class="sxs-lookup"><span data-stu-id="b3209-552">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="b3209-553">Ofrecen ayuda relativa al compilador e IntelliSense al consumir clientes.</span><span class="sxs-lookup"><span data-stu-id="b3209-553">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="b3209-554">Facilitan una sola ubicación para configurar un elemento `HttpClient` determinado e interactuar con él.</span><span class="sxs-lookup"><span data-stu-id="b3209-554">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="b3209-555">Por ejemplo, el mismo cliente con tipo se puede usar para un punto de conexión back-end único y encapsular toda la lógica que se ocupa de ese punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="b3209-555">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="b3209-556">Funcionan con la inserción de dependencias y se pueden insertar en la aplicación cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="b3209-556">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="b3209-557">Un cliente con tipo acepta un parámetro `HttpClient` en su constructor:</span><span class="sxs-lookup"><span data-stu-id="b3209-557">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="b3209-558">En el código anterior, la configuración se mueve al cliente con tipo.</span><span class="sxs-lookup"><span data-stu-id="b3209-558">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="b3209-559">El objeto `HttpClient` se expone como una propiedad pública.</span><span class="sxs-lookup"><span data-stu-id="b3209-559">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="b3209-560">Se pueden definir métodos específicos de API que exponen la funcionalidad `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-560">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="b3209-561">El método `GetAspNetDocsIssues` encapsula el código necesario para consultar y analizar los últimos problemas abiertos de un repositorio de GitHub.</span><span class="sxs-lookup"><span data-stu-id="b3209-561">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="b3209-562">Para registrar un cliente con tipo, se puede usar el método de extensión genérico <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> dentro de `Startup.ConfigureServices`, especificando la clase del cliente con tipo:</span><span class="sxs-lookup"><span data-stu-id="b3209-562">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="b3209-563">El cliente con tipo se registra como transitorio con inserción con dependencias,</span><span class="sxs-lookup"><span data-stu-id="b3209-563">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="b3209-564">y se puede insertar y consumir directamente:</span><span class="sxs-lookup"><span data-stu-id="b3209-564">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="b3209-565">Si lo prefiere, la configuración de un cliente con nombre se puede especificar durante su registro en `Startup.ConfigureServices`, en lugar de en su constructor:</span><span class="sxs-lookup"><span data-stu-id="b3209-565">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="b3209-566">El `HttpClient` se puede encapsular completamente dentro de un cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="b3209-566">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="b3209-567">En lugar de exponerlo como una propiedad, se pueden proporcionar métodos públicos que llamen a la instancia de `HttpClient` internamente.</span><span class="sxs-lookup"><span data-stu-id="b3209-567">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="b3209-568">En el código anterior, el `HttpClient` se almacena como un campo privado.</span><span class="sxs-lookup"><span data-stu-id="b3209-568">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="b3209-569">Todo el acceso para realizar llamadas externas pasa por el método `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="b3209-569">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="b3209-570">Clientes generados</span><span class="sxs-lookup"><span data-stu-id="b3209-570">Generated clients</span></span>

<span data-ttu-id="b3209-571">`IHttpClientFactory` se puede usar en combinación con otras bibliotecas de terceros, como [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="b3209-571">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="b3209-572">Refit es una biblioteca de REST para .NET</span><span class="sxs-lookup"><span data-stu-id="b3209-572">Refit is a REST library for .NET.</span></span> <span data-ttu-id="b3209-573">que convierte las API de REST en interfaces en vivo.</span><span class="sxs-lookup"><span data-stu-id="b3209-573">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="b3209-574">Se genera una implementación de la interfaz dinámicamente por medio de `RestService`, usando `HttpClient` para realizar las llamadas HTTP externas.</span><span class="sxs-lookup"><span data-stu-id="b3209-574">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="b3209-575">Se define una interfaz y una respuesta para representar la API externa y su correspondiente respuesta:</span><span class="sxs-lookup"><span data-stu-id="b3209-575">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="b3209-576">Un cliente con tipo se puede agregar usando Refit para generar la implementación:</span><span class="sxs-lookup"><span data-stu-id="b3209-576">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="b3209-577">La interfaz definida se puede usar cuando sea preciso con la implementación proporcionada por la inserción de dependencias y Refit:</span><span class="sxs-lookup"><span data-stu-id="b3209-577">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="b3209-578">Middleware de solicitud saliente</span><span class="sxs-lookup"><span data-stu-id="b3209-578">Outgoing request middleware</span></span>

<span data-ttu-id="b3209-579">`HttpClient` ya posee el concepto de controladores de delegación, que se pueden vincular entre sí para las solicitudes HTTP salientes.</span><span class="sxs-lookup"><span data-stu-id="b3209-579">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="b3209-580">`IHttpClientFactory` permite definir fácilmente los controladores que se usarán en cada cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="b3209-580">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="b3209-581">Admite el registro y encadenamiento de varios controladores para crear una canalización de middleware de solicitud saliente.</span><span class="sxs-lookup"><span data-stu-id="b3209-581">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="b3209-582">Cada uno de estos controladores es capaz de realizar la tarea antes y después de la solicitud de salida.</span><span class="sxs-lookup"><span data-stu-id="b3209-582">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="b3209-583">Este patrón es similar a la canalización de middleware de entrada de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b3209-583">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="b3209-584">Dicho patrón proporciona un mecanismo para administrar cuestiones transversales relativas a las solicitudes HTTP, como el almacenamiento en caché, el control de errores, la serialización y el registro.</span><span class="sxs-lookup"><span data-stu-id="b3209-584">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="b3209-585">Para crear un controlador, defina una clase que se derive de <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="b3209-585">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="b3209-586">Invalide el método `SendAsync` para ejecutar el código antes de pasar la solicitud al siguiente controlador de la canalización:</span><span class="sxs-lookup"><span data-stu-id="b3209-586">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="b3209-587">El código anterior define un controlador básico.</span><span class="sxs-lookup"><span data-stu-id="b3209-587">The preceding code defines a basic handler.</span></span> <span data-ttu-id="b3209-588">Comprueba si se ha incluido un encabezado `X-API-KEY` en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b3209-588">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="b3209-589">Si no está presente, puede evitar la llamada HTTP y devolver una respuesta adecuada.</span><span class="sxs-lookup"><span data-stu-id="b3209-589">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="b3209-590">Durante el registro, se pueden agregar uno o varios controladores a la configuración de una instancia de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-590">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="b3209-591">Esta tarea se realiza a través de métodos de extensión en <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b3209-591">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="b3209-592">En el código anterior, `ValidateHeaderHandler` se ha registrado con inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="b3209-592">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="b3209-593">El controlador **debe** estar registrado en la inserción de dependencias como servicio transitorio, nunca como servicio con ámbito.</span><span class="sxs-lookup"><span data-stu-id="b3209-593">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="b3209-594">Si el controlador se registra como servicio con ámbito y se pueden eliminar los servicios de los que depende el controlador:</span><span class="sxs-lookup"><span data-stu-id="b3209-594">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="b3209-595">Los servicios del controlador se pueden eliminar antes de que el controlador deje de estar en el ámbito.</span><span class="sxs-lookup"><span data-stu-id="b3209-595">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="b3209-596">Los servicios del controlador que se eliminen provocarán un error en él.</span><span class="sxs-lookup"><span data-stu-id="b3209-596">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="b3209-597">Una vez registrado, se puede llamar a <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, con lo que se pasa el tipo de controlador.</span><span class="sxs-lookup"><span data-stu-id="b3209-597">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="b3209-598">Se pueden registrar varios controladores en el orden en que deben ejecutarse.</span><span class="sxs-lookup"><span data-stu-id="b3209-598">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="b3209-599">Cada controlador contiene el siguiente controlador hasta que el último `HttpClientHandler` ejecuta la solicitud:</span><span class="sxs-lookup"><span data-stu-id="b3209-599">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="b3209-600">Use uno de los siguientes enfoques para compartir el estado por solicitud con controladores de mensajes:</span><span class="sxs-lookup"><span data-stu-id="b3209-600">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="b3209-601">Pase datos al controlador usando `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="b3209-601">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="b3209-602">Use `IHttpContextAccessor` para acceder a la solicitud actual.</span><span class="sxs-lookup"><span data-stu-id="b3209-602">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="b3209-603">Cree un objeto de almacenamiento `AsyncLocal` personalizado para pasar los datos.</span><span class="sxs-lookup"><span data-stu-id="b3209-603">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="b3209-604">Usar controladores basados en Polly</span><span class="sxs-lookup"><span data-stu-id="b3209-604">Use Polly-based handlers</span></span>

<span data-ttu-id="b3209-605">`IHttpClientFactory` se integra con una biblioteca de terceros muy conocida denominada [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="b3209-605">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="b3209-606">Polly es una biblioteca con capacidades de resistencia y control de errores transitorios para .NET.</span><span class="sxs-lookup"><span data-stu-id="b3209-606">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="b3209-607">Permite a los desarrolladores expresar directivas como, por ejemplo, de reintento, interruptor, tiempo de espera, aislamiento compartimentado y reserva de forma fluida y segura para los subprocesos.</span><span class="sxs-lookup"><span data-stu-id="b3209-607">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="b3209-608">Se proporcionan métodos de extensión para hacer posible el uso de directivas de Polly con instancias de `HttpClient` configuradas.</span><span class="sxs-lookup"><span data-stu-id="b3209-608">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="b3209-609">Las extensiones de Polly:</span><span class="sxs-lookup"><span data-stu-id="b3209-609">The Polly extensions:</span></span>

* <span data-ttu-id="b3209-610">permiten agregar controladores basados en Polly a los clientes;</span><span class="sxs-lookup"><span data-stu-id="b3209-610">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="b3209-611">se pueden usar tras instalar el paquete NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/),</span><span class="sxs-lookup"><span data-stu-id="b3209-611">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="b3209-612">aunque este no está incluido en la plataforma compartida ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b3209-612">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="b3209-613">Control de errores transitorios</span><span class="sxs-lookup"><span data-stu-id="b3209-613">Handle transient faults</span></span>

<span data-ttu-id="b3209-614">Los errores más comunes se producen cuando las llamadas HTTP externas son transitorias.</span><span class="sxs-lookup"><span data-stu-id="b3209-614">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="b3209-615">Por ello, se incluye un método de extensión muy práctico denominado `AddTransientHttpErrorPolicy`, que permite definir una directiva para controlar los errores transitorios.</span><span class="sxs-lookup"><span data-stu-id="b3209-615">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="b3209-616">Las directivas que se configuran con este método de extensión controlan `HttpRequestException`, las respuestas HTTP 5xx y las respuestas HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="b3209-616">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="b3209-617">La extensión `AddTransientHttpErrorPolicy` se puede usar en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b3209-617">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b3209-618">La extensión da acceso a un objeto `PolicyBuilder`, configurado para controlar los errores que pueden constituir un posible error transitorio:</span><span class="sxs-lookup"><span data-stu-id="b3209-618">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="b3209-619">En el código anterior, se define una directiva `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="b3209-619">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="b3209-620">Las solicitudes erróneas se reintentan hasta tres veces con un retardo de 600 ms entre intentos.</span><span class="sxs-lookup"><span data-stu-id="b3209-620">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="b3209-621">Seleccionar directivas dinámicamente</span><span class="sxs-lookup"><span data-stu-id="b3209-621">Dynamically select policies</span></span>

<span data-ttu-id="b3209-622">Existen más métodos de extensión que pueden servir para agregar controladores basados en Polly.</span><span class="sxs-lookup"><span data-stu-id="b3209-622">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="b3209-623">Una de esas extensiones es `AddPolicyHandler`, que tiene varias sobrecargas.</span><span class="sxs-lookup"><span data-stu-id="b3209-623">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="b3209-624">Una de esas sobrecargas permite inspeccionar la solicitud al dilucidar qué directiva aplicar:</span><span class="sxs-lookup"><span data-stu-id="b3209-624">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="b3209-625">En el código anterior, si la solicitud GET saliente es del tipo HTTP, se aplica un tiempo de espera de 10 segundos.</span><span class="sxs-lookup"><span data-stu-id="b3209-625">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="b3209-626">En cualquier otro método HTTP, se usa un tiempo de espera de 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="b3209-626">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="b3209-627">Agregar varios controladores de Polly</span><span class="sxs-lookup"><span data-stu-id="b3209-627">Add multiple Polly handlers</span></span>

<span data-ttu-id="b3209-628">Es habitual anidar directivas de Polly para proporcionar una funcionalidad mejorada:</span><span class="sxs-lookup"><span data-stu-id="b3209-628">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="b3209-629">En el ejemplo anterior, se agregan dos controladores.</span><span class="sxs-lookup"><span data-stu-id="b3209-629">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="b3209-630">En el primer ejemplo se usa la extensión `AddTransientHttpErrorPolicy` para agregar una directiva de reintento.</span><span class="sxs-lookup"><span data-stu-id="b3209-630">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="b3209-631">Las solicitudes con error se reintentan hasta tres veces.</span><span class="sxs-lookup"><span data-stu-id="b3209-631">Failed requests are retried up to three times.</span></span> <span data-ttu-id="b3209-632">La segunda llamada a `AddTransientHttpErrorPolicy` agrega una directiva de interruptor.</span><span class="sxs-lookup"><span data-stu-id="b3209-632">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="b3209-633">Las solicitudes externas subsiguientes se bloquean durante 30 segundos si se producen cinco intentos infructuosos seguidos.</span><span class="sxs-lookup"><span data-stu-id="b3209-633">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="b3209-634">Las directivas de interruptor tienen estado.</span><span class="sxs-lookup"><span data-stu-id="b3209-634">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="b3209-635">Así, todas las llamadas realizadas a través de este cliente comparten el mismo estado de circuito.</span><span class="sxs-lookup"><span data-stu-id="b3209-635">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="b3209-636">Agregar directivas desde el Registro de Polly</span><span class="sxs-lookup"><span data-stu-id="b3209-636">Add policies from the Polly registry</span></span>

<span data-ttu-id="b3209-637">Una forma de administrar las directivas usadas habitualmente consiste en definirlas una vez y registrarlas con `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="b3209-637">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="b3209-638">Se proporciona un método de extensión que permite agregar un controlador por medio de una directiva del Registro:</span><span class="sxs-lookup"><span data-stu-id="b3209-638">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="b3209-639">En el código anterior, se registran dos directivas cuando se agrega `PolicyRegistry` a `ServiceCollection`.</span><span class="sxs-lookup"><span data-stu-id="b3209-639">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="b3209-640">Para usar una directiva del Registro, se usa el método `AddPolicyHandlerFromRegistry` pasando el nombre de la directiva que se va a aplicar.</span><span class="sxs-lookup"><span data-stu-id="b3209-640">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="b3209-641">Encontrará más información sobre `IHttpClientFactory` y las integraciones de Polly en la [wiki de Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="b3209-641">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="b3209-642">HttpClient y administración de la duración</span><span class="sxs-lookup"><span data-stu-id="b3209-642">HttpClient and lifetime management</span></span>

<span data-ttu-id="b3209-643">Cada vez que se llama a `CreateClient` en `IHttpClientFactory`, se devuelve una nueva instancia de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-643">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="b3209-644">Hay un controlador <xref:System.Net.Http.HttpMessageHandler> por cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="b3209-644">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="b3209-645">La fábrica administra la duración de las instancias de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="b3209-645">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="b3209-646">`IHttpClientFactory` agrupa las instancias de `HttpMessageHandler` creadas por Factory para reducir el consumo de recursos.</span><span class="sxs-lookup"><span data-stu-id="b3209-646">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="b3209-647">Se puede reutilizar una instancia de `HttpMessageHandler` del grupo al crear una instancia de `HttpClient` si su duración aún no ha expirado.</span><span class="sxs-lookup"><span data-stu-id="b3209-647">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="b3209-648">Se recomienda agrupar controladores porque cada uno de ellos normalmente administra sus propias conexiones HTTP subyacentes.</span><span class="sxs-lookup"><span data-stu-id="b3209-648">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="b3209-649">Crear más controladores de los necesarios puede provocar retrasos en la conexión.</span><span class="sxs-lookup"><span data-stu-id="b3209-649">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="b3209-650">Además, algunos controladores dejan las conexiones abiertas de forma indefinida, lo que puede ser un obstáculo a la hora de reaccionar ante los cambios de DNS.</span><span class="sxs-lookup"><span data-stu-id="b3209-650">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="b3209-651">La duración de controlador predeterminada es dos minutos.</span><span class="sxs-lookup"><span data-stu-id="b3209-651">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="b3209-652">El valor predeterminado se puede reemplazar individualmente en cada cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="b3209-652">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="b3209-653">Para ello, llame a <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> en el `IHttpClientBuilder` que se devuelve cuando se crea el cliente:</span><span class="sxs-lookup"><span data-stu-id="b3209-653">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="b3209-654">No hace falta eliminar el cliente,</span><span class="sxs-lookup"><span data-stu-id="b3209-654">Disposal of the client isn't required.</span></span> <span data-ttu-id="b3209-655">ya que se cancelan las solicitudes salientes y la instancia de `HttpClient` determinada no se puede usar después de llamar a <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="b3209-655">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="b3209-656">`IHttpClientFactory` realiza un seguimiento y elimina los recursos que usan las instancias de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-656">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="b3209-657">Normalmente, las instancias de `HttpClient` pueden tratarse como objetos de .NET que no requieren eliminación.</span><span class="sxs-lookup"><span data-stu-id="b3209-657">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="b3209-658">Mantener una sola instancia de `HttpClient` activa durante un período prolongado es un patrón común que se utiliza antes de la concepción de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="b3209-658">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="b3209-659">Este patrón se convierte en innecesario tras la migración a `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="b3209-659">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="b3209-660">Alternativas a IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="b3209-660">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="b3209-661">El uso de `IHttpClientFactory` en una aplicación habilitada para la inserción de dependencias evita lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b3209-661">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="b3209-662">Problemas de agotamiento de recursos mediante la agrupación de instancias de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="b3209-662">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="b3209-663">Problemas de DNS obsoletos al recorrer las instancias de `HttpMessageHandler` a intervalos regulares.</span><span class="sxs-lookup"><span data-stu-id="b3209-663">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="b3209-664">Existen formas alternativas de solucionar los problemas anteriores mediante una instancia de <xref:System.Net.Http.SocketsHttpHandler> de larga duración.</span><span class="sxs-lookup"><span data-stu-id="b3209-664">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="b3209-665">Cree una instancia de `SocketsHttpHandler` al iniciar la aplicación y úsela para la vida útil de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b3209-665">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="b3209-666">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> con un valor adecuado en función de los tiempos de actualización de DNS.</span><span class="sxs-lookup"><span data-stu-id="b3209-666">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="b3209-667">Cree instancias de `HttpClient` mediante `new HttpClient(handler, dispostHandler: false)` según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="b3209-667">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="b3209-668">Los enfoques anteriores solucionan los problemas de administración de recursos que `IHttpClientFactory` resuelve de forma similar.</span><span class="sxs-lookup"><span data-stu-id="b3209-668">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="b3209-669">`SocketsHttpHandler` comparte las conexiones entre las instancias de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-669">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="b3209-670">Este uso compartido impide el agotamiento del socket.</span><span class="sxs-lookup"><span data-stu-id="b3209-670">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="b3209-671">`SocketsHttpHandler` recorre las conexiones según `PooledConnectionLifetime` para evitar problemas de DNS obsoletos.</span><span class="sxs-lookup"><span data-stu-id="b3209-671">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="b3209-672">Cookies</span><span class="sxs-lookup"><span data-stu-id="b3209-672">Cookies</span></span>

<span data-ttu-id="b3209-673">Las instancias de `HttpMessageHandler` agrupadas generan objetos `CookieContainer` que se comparten.</span><span class="sxs-lookup"><span data-stu-id="b3209-673">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="b3209-674">El uso compartido de objetos `CookieContainer` no previsto suele generar código incorrecto.</span><span class="sxs-lookup"><span data-stu-id="b3209-674">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="b3209-675">En el caso de las aplicaciones que requieren cookies, tenga en cuenta lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b3209-675">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="b3209-676">Deshabilitar el control automático de las cookies</span><span class="sxs-lookup"><span data-stu-id="b3209-676">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="b3209-677">Evitar `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="b3209-677">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="b3209-678">Llame a <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> para deshabilitar el control automático de cookies:</span><span class="sxs-lookup"><span data-stu-id="b3209-678">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="b3209-679">Registro</span><span class="sxs-lookup"><span data-stu-id="b3209-679">Logging</span></span>

<span data-ttu-id="b3209-680">Los clientes que se han creado a través de `IHttpClientFactory` registran mensajes de registro de todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="b3209-680">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="b3209-681">Habilite el nivel de información adecuado en la configuración del registro para ver los mensajes de registro predeterminados.</span><span class="sxs-lookup"><span data-stu-id="b3209-681">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="b3209-682">El registro de más información, como el registro de encabezados de solicitud, solo se incluye en el nivel de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="b3209-682">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="b3209-683">La categoría de registro usada en cada cliente incluye el nombre del cliente.</span><span class="sxs-lookup"><span data-stu-id="b3209-683">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="b3209-684">Así, por ejemplo, un cliente llamado *MyNamedClient* registra mensajes con una categoría `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="b3209-684">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="b3209-685">Los mensajes con el sufijo *LogicalHandler* se producen fuera de la canalización de controlador de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b3209-685">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="b3209-686">En la solicitud, los mensajes se registran antes de que cualquier otro controlador de la canalización haya procesado la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b3209-686">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="b3209-687">En la respuesta, los mensajes se registran después de que cualquier otro controlador de la canalización haya recibido la respuesta.</span><span class="sxs-lookup"><span data-stu-id="b3209-687">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="b3209-688">El registro también se produce dentro de la canalización de controlador de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b3209-688">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="b3209-689">En nuestro caso de ejemplo de *MyNamedClient*, esos mensajes se registran en la categoría de registro `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="b3209-689">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="b3209-690">En la solicitud, esto tiene lugar después de que todos los demás controladores se hayan ejecutado y justo antes de que la solicitud se envíe por la red.</span><span class="sxs-lookup"><span data-stu-id="b3209-690">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="b3209-691">En la respuesta, este registro incluye el estado de la respuesta antes de que vuelva a pasar por la canalización del controlador.</span><span class="sxs-lookup"><span data-stu-id="b3209-691">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="b3209-692">Al habilitar el registro tanto dentro como fuera de la canalización, se podrán inspeccionar los cambios realizados por otros controladores de la canalización.</span><span class="sxs-lookup"><span data-stu-id="b3209-692">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="b3209-693">Esto puede englobar cambios, por ejemplo, en los encabezados de solicitud o en el código de estado de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="b3209-693">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="b3209-694">Si el nombre del cliente se incluye en la categoría de registro, dicho registro se podrá filtrar para encontrar clientes con nombre específicos cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="b3209-694">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="b3209-695">Configurar HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="b3209-695">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="b3209-696">Puede que sea necesario controlar la configuración del elemento `HttpMessageHandler` interno usado por un cliente.</span><span class="sxs-lookup"><span data-stu-id="b3209-696">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="b3209-697">Se devuelve un `IHttpClientBuilder` cuando se agregan clientes con nombre o con tipo.</span><span class="sxs-lookup"><span data-stu-id="b3209-697">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="b3209-698">Se puede usar el método de extensión <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> para definir un delegado.</span><span class="sxs-lookup"><span data-stu-id="b3209-698">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="b3209-699">Este delegado servirá para crear y configurar el elemento principal `HttpMessageHandler` que ese cliente usa:</span><span class="sxs-lookup"><span data-stu-id="b3209-699">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="b3209-700">Uso de IHttpClientFactory en una aplicación de consola</span><span class="sxs-lookup"><span data-stu-id="b3209-700">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="b3209-701">En una aplicación de consola, agregue las siguientes referencias de paquete al proyecto:</span><span class="sxs-lookup"><span data-stu-id="b3209-701">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="b3209-702">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="b3209-702">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="b3209-703">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="b3209-703">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="b3209-704">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b3209-704">In the following example:</span></span>

* <span data-ttu-id="b3209-705"><xref:System.Net.Http.IHttpClientFactory> está registrado en el contenedor de servicios del [host genérico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="b3209-705"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="b3209-706">`MyService` crea una instancia de generador de clientes a partir del servicio, que se usa para crear un elemento `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="b3209-706">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="b3209-707">`HttpClient` se utiliza para recuperar una página web.</span><span class="sxs-lookup"><span data-stu-id="b3209-707">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="b3209-708">`Main` crea un ámbito para ejecutar el método `GetPage` del servicio y escribe los primeros 500 caracteres del contenido de la página web en la consola.</span><span class="sxs-lookup"><span data-stu-id="b3209-708">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="b3209-709">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b3209-709">Additional resources</span></span>

* [<span data-ttu-id="b3209-710">Uso de HttpClientFactory para implementar solicitudes HTTP resistentes</span><span class="sxs-lookup"><span data-stu-id="b3209-710">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="b3209-711">Implementación de reintentos de llamada HTTP con retroceso exponencial con HttpClientFactory y las directivas de Polly</span><span class="sxs-lookup"><span data-stu-id="b3209-711">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="b3209-712">Implementación del patrón de interruptor</span><span class="sxs-lookup"><span data-stu-id="b3209-712">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
