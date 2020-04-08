---
title: Realización de solicitudes HTTP mediante IHttpClientFactory en ASP.NET Core
author: stevejgordon
description: Obtenga información sobre cómo usar la interfaz IHttpClientFactory para administrar instancias de HttpClient lógicas en ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/09/2020
uid: fundamentals/http-requests
ms.openlocfilehash: 912be34ae0ee25837a94aab65443f15b17ab4556
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "78648299"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="37b4c-103">Realización de solicitudes HTTP mediante IHttpClientFactory en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="37b4c-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="37b4c-104">Por [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT) y [Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="37b4c-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="37b4c-105">Se puede registrar y usar una interfaz <xref:System.Net.Http.IHttpClientFactory> para crear y configurar instancias de <xref:System.Net.Http.HttpClient> en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="37b4c-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="37b4c-106">`IHttpClientFactory` ofrece las ventajas siguientes:</span><span class="sxs-lookup"><span data-stu-id="37b4c-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="37b4c-107">Proporciona una ubicación central para denominar y configurar instancias de `HttpClient` lógicas.</span><span class="sxs-lookup"><span data-stu-id="37b4c-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="37b4c-108">Por ejemplo, se podría registrar un cliente *github* y configurarlo para acceder a [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="37b4c-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="37b4c-109">Se puede registrar un cliente predeterminado para el acceso general.</span><span class="sxs-lookup"><span data-stu-id="37b4c-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="37b4c-110">Codifica el concepto de middleware de salida a través de la delegación de controladores en `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="37b4c-111">Proporciona extensiones para el middleware basado en Polly a fin de aprovechar los controladores de delegación en `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="37b4c-112">Administra la agrupación y la duración de las instancias de `HttpClientMessageHandler` subyacentes.</span><span class="sxs-lookup"><span data-stu-id="37b4c-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="37b4c-113">La administración automática evita los problemas comunes de DNS (Sistema de nombres de dominio) que se producen al administrar la duración de `HttpClient` de forma manual.</span><span class="sxs-lookup"><span data-stu-id="37b4c-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="37b4c-114">Agrega una experiencia de registro configurable (a través de `ILogger`) en todas las solicitudes enviadas a través de los clientes creados por Factory.</span><span class="sxs-lookup"><span data-stu-id="37b4c-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="37b4c-115">[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="37b4c-115">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="37b4c-116">En el código de ejemplo de la versión este tema se usa <xref:System.Text.Json> para deserializar el contenido JSON devuelto en las respuestas HTTP.</span><span class="sxs-lookup"><span data-stu-id="37b4c-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="37b4c-117">Para obtener ejemplos en los que se usan `Json.NET` y `ReadAsAsync<T>`, utilice el selector de versión para seleccionar una versión 2.x de este tema.</span><span class="sxs-lookup"><span data-stu-id="37b4c-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="37b4c-118">Patrones de consumo</span><span class="sxs-lookup"><span data-stu-id="37b4c-118">Consumption patterns</span></span>

<span data-ttu-id="37b4c-119">`IHttpClientFactory` se puede usar de varias formas en una aplicación:</span><span class="sxs-lookup"><span data-stu-id="37b4c-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="37b4c-120">Uso básico</span><span class="sxs-lookup"><span data-stu-id="37b4c-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="37b4c-121">Clientes con nombre</span><span class="sxs-lookup"><span data-stu-id="37b4c-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="37b4c-122">Clientes con tipo</span><span class="sxs-lookup"><span data-stu-id="37b4c-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="37b4c-123">Clientes generados</span><span class="sxs-lookup"><span data-stu-id="37b4c-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="37b4c-124">El mejor enfoque depende de los requisitos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="37b4c-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="37b4c-125">Uso básico</span><span class="sxs-lookup"><span data-stu-id="37b4c-125">Basic usage</span></span>

<span data-ttu-id="37b4c-126">`IHttpClientFactory` se puede registrar mediante una llamada a `AddHttpClient`:</span><span class="sxs-lookup"><span data-stu-id="37b4c-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="37b4c-127">Se puede solicitar una instancia de `IHttpClientFactory` mediante la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="37b4c-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="37b4c-128">En el código siguiente se usa `IHttpClientFactory` para crear una instancia de `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="37b4c-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="37b4c-129">El uso de `IHttpClientFactory` como en el ejemplo anterior es una buena manera de refactorizar una aplicación existente.</span><span class="sxs-lookup"><span data-stu-id="37b4c-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="37b4c-130">No tiene efecto alguno en la forma de usar `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="37b4c-131">En aquellos sitios de una aplicación existente en los que ya se hayan creado instancias de `HttpClient`, reemplace esas repeticiones por llamadas a <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="37b4c-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="37b4c-132">Clientes con nombre</span><span class="sxs-lookup"><span data-stu-id="37b4c-132">Named clients</span></span>

<span data-ttu-id="37b4c-133">Los clientes con nombre son una buena opción cuando:</span><span class="sxs-lookup"><span data-stu-id="37b4c-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="37b4c-134">La aplicación requiere muchos usos distintos de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="37b4c-135">Muchas instancias `HttpClient` de tienen otra configuración.</span><span class="sxs-lookup"><span data-stu-id="37b4c-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="37b4c-136">La configuración de un objeto `HttpClient` con nombre se puede realizar durante la fase de registro en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="37b4c-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="37b4c-137">En el código anterior, el cliente está configurado con:</span><span class="sxs-lookup"><span data-stu-id="37b4c-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="37b4c-138">La dirección base. `https://api.github.com/`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="37b4c-139">Dos encabezados necesarios para trabajar con la API de GitHub.</span><span class="sxs-lookup"><span data-stu-id="37b4c-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="37b4c-140">CreateClient</span><span class="sxs-lookup"><span data-stu-id="37b4c-140">CreateClient</span></span>

<span data-ttu-id="37b4c-141">Cada vez que se llama a <xref:System.Net.Http.IHttpClientFactory.CreateClient*>:</span><span class="sxs-lookup"><span data-stu-id="37b4c-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="37b4c-142">Se crea una instancia de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="37b4c-143">Se llama a la acción de configuración.</span><span class="sxs-lookup"><span data-stu-id="37b4c-143">The configuration action is called.</span></span>

<span data-ttu-id="37b4c-144">Para crear un cliente con nombre, pase su nombre a `CreateClient`:</span><span class="sxs-lookup"><span data-stu-id="37b4c-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="37b4c-145">En el código anterior, no es necesario especificar un nombre de host en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="37b4c-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="37b4c-146">El código puede pasar solo la ruta de acceso, ya que se usa la dirección base configurada del cliente.</span><span class="sxs-lookup"><span data-stu-id="37b4c-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="37b4c-147">Clientes con tipo</span><span class="sxs-lookup"><span data-stu-id="37b4c-147">Typed clients</span></span>

<span data-ttu-id="37b4c-148">Clientes con tipo:</span><span class="sxs-lookup"><span data-stu-id="37b4c-148">Typed clients:</span></span>

* <span data-ttu-id="37b4c-149">Proporcionan las mismas funciones que los clientes con nombre sin la necesidad de usar cadenas como claves.</span><span class="sxs-lookup"><span data-stu-id="37b4c-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="37b4c-150">Ofrecen ayuda relativa al compilador e IntelliSense al consumir clientes.</span><span class="sxs-lookup"><span data-stu-id="37b4c-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="37b4c-151">Facilitan una sola ubicación para configurar un elemento `HttpClient` determinado e interactuar con él.</span><span class="sxs-lookup"><span data-stu-id="37b4c-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="37b4c-152">Por ejemplo, es posible usar un solo cliente con tipo:</span><span class="sxs-lookup"><span data-stu-id="37b4c-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="37b4c-153">Para un único punto de conexión de back-end.</span><span class="sxs-lookup"><span data-stu-id="37b4c-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="37b4c-154">Para encapsular toda la lógica relacionada con el punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="37b4c-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="37b4c-155">Funcionan con la inserción de dependencias y se pueden insertar en la aplicación cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="37b4c-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="37b4c-156">Un cliente con tipo acepta un parámetro `HttpClient` en su constructor:</span><span class="sxs-lookup"><span data-stu-id="37b4c-156">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="37b4c-157">En el código anterior:</span><span class="sxs-lookup"><span data-stu-id="37b4c-157">In the preceding code:</span></span>

* <span data-ttu-id="37b4c-158">La configuración se mueve al cliente con tipo.</span><span class="sxs-lookup"><span data-stu-id="37b4c-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="37b4c-159">El objeto `HttpClient` se expone como una propiedad pública.</span><span class="sxs-lookup"><span data-stu-id="37b4c-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="37b4c-160">Se pueden crear métodos específicos de la API que exponen la funcionalidad de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="37b4c-161">Por ejemplo, el método `GetAspNetDocsIssues` encapsula el código para recuperar incidencias abiertas.</span><span class="sxs-lookup"><span data-stu-id="37b4c-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="37b4c-162">En el código siguiente se llama a <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> en `Startup.ConfigureServices` para registrar una clase de cliente con tipo:</span><span class="sxs-lookup"><span data-stu-id="37b4c-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="37b4c-163">El cliente con tipo se registra como transitorio con inserción con dependencias,</span><span class="sxs-lookup"><span data-stu-id="37b4c-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="37b4c-164">En el código anterior, `AddHttpClient` registra `GitHubService` como servicio transitorio.</span><span class="sxs-lookup"><span data-stu-id="37b4c-164">In the preceding code, `AddHttpClient` registers `GitHubService` as a transient service.</span></span> <span data-ttu-id="37b4c-165">Este registro usa un Factory Method para:</span><span class="sxs-lookup"><span data-stu-id="37b4c-165">This registration uses a factory method to:</span></span>

1. <span data-ttu-id="37b4c-166">Crea una instancia de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-166">Create an instance of `HttpClient`.</span></span>
1. <span data-ttu-id="37b4c-167">Cree una instancia de `GitHubService`, pasando la instancia de `HttpClient` a su constructor.</span><span class="sxs-lookup"><span data-stu-id="37b4c-167">Create an instance of `GitHubService`, passing in the instance of `HttpClient` to its constructor.</span></span>

<span data-ttu-id="37b4c-168">y se puede insertar y consumir directamente:</span><span class="sxs-lookup"><span data-stu-id="37b4c-168">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="37b4c-169">La configuración de un cliente con tipo se puede especificar durante su registro en `Startup.ConfigureServices`, en lugar de en su constructor:</span><span class="sxs-lookup"><span data-stu-id="37b4c-169">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="37b4c-170">`HttpClient` se puede encapsular dentro de un cliente con tipo.</span><span class="sxs-lookup"><span data-stu-id="37b4c-170">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="37b4c-171">En lugar de exponerlo como una propiedad, defina un método que llame a la instancia de `HttpClient` internamente:</span><span class="sxs-lookup"><span data-stu-id="37b4c-171">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="37b4c-172">En el código anterior, `HttpClient` se almacena en un campo privado.</span><span class="sxs-lookup"><span data-stu-id="37b4c-172">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="37b4c-173">El acceso a `HttpClient` se realiza mediante el método `GetRepos` público.</span><span class="sxs-lookup"><span data-stu-id="37b4c-173">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="37b4c-174">Clientes generados</span><span class="sxs-lookup"><span data-stu-id="37b4c-174">Generated clients</span></span>

<span data-ttu-id="37b4c-175">`IHttpClientFactory` se puede usar en combinación con bibliotecas de terceros, como [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="37b4c-175">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="37b4c-176">Refit es una biblioteca de REST para .NET</span><span class="sxs-lookup"><span data-stu-id="37b4c-176">Refit is a REST library for .NET.</span></span> <span data-ttu-id="37b4c-177">que convierte las API de REST en interfaces en vivo.</span><span class="sxs-lookup"><span data-stu-id="37b4c-177">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="37b4c-178">Se genera una implementación de la interfaz dinámicamente por medio de `RestService`, usando `HttpClient` para realizar las llamadas HTTP externas.</span><span class="sxs-lookup"><span data-stu-id="37b4c-178">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="37b4c-179">Se define una interfaz y una respuesta para representar la API externa y su correspondiente respuesta:</span><span class="sxs-lookup"><span data-stu-id="37b4c-179">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="37b4c-180">Un cliente con tipo se puede agregar usando Refit para generar la implementación:</span><span class="sxs-lookup"><span data-stu-id="37b4c-180">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="37b4c-181">La interfaz definida se puede usar cuando sea preciso con la implementación proporcionada por la inserción de dependencias y Refit:</span><span class="sxs-lookup"><span data-stu-id="37b4c-181">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="37b4c-182">Middleware de solicitud saliente</span><span class="sxs-lookup"><span data-stu-id="37b4c-182">Outgoing request middleware</span></span>

<span data-ttu-id="37b4c-183">`HttpClient` tiene el concepto de controladores de delegación, que se pueden vincular entre sí para las solicitudes HTTP salientes.</span><span class="sxs-lookup"><span data-stu-id="37b4c-183">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="37b4c-184">`IHttpClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="37b4c-184">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="37b4c-185">simplifica la definición de controladores que se aplicarán por cada cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="37b4c-185">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="37b4c-186">Admite el registro y encadenamiento de varios controladores para crear una canalización de middleware de solicitud saliente.</span><span class="sxs-lookup"><span data-stu-id="37b4c-186">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="37b4c-187">Cada uno de estos controladores es capaz de realizar la tarea antes y después de la solicitud de salida.</span><span class="sxs-lookup"><span data-stu-id="37b4c-187">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="37b4c-188">Este patrón:</span><span class="sxs-lookup"><span data-stu-id="37b4c-188">This pattern:</span></span>

  * <span data-ttu-id="37b4c-189">Es similar a la canalización de middleware de entrada de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="37b4c-189">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="37b4c-190">Proporciona un mecanismo para administrar los intereses transversales relacionados con las solicitudes HTTP, como:</span><span class="sxs-lookup"><span data-stu-id="37b4c-190">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="37b4c-191">el almacenamiento en caché</span><span class="sxs-lookup"><span data-stu-id="37b4c-191">caching</span></span>
    * <span data-ttu-id="37b4c-192">el control de errores</span><span class="sxs-lookup"><span data-stu-id="37b4c-192">error handling</span></span>
    * <span data-ttu-id="37b4c-193">la serialización</span><span class="sxs-lookup"><span data-stu-id="37b4c-193">serialization</span></span>
    * <span data-ttu-id="37b4c-194">el registro</span><span class="sxs-lookup"><span data-stu-id="37b4c-194">logging</span></span>

<span data-ttu-id="37b4c-195">Para crear un controlador de delegación:</span><span class="sxs-lookup"><span data-stu-id="37b4c-195">To create a delegating handler:</span></span>

* <span data-ttu-id="37b4c-196">Derívelo de <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="37b4c-196">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="37b4c-197">Reemplace <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span><span class="sxs-lookup"><span data-stu-id="37b4c-197">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="37b4c-198">Ejecute el código antes de pasar la solicitud al siguiente controlador de la canalización:</span><span class="sxs-lookup"><span data-stu-id="37b4c-198">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="37b4c-199">El código anterior comprueba si el encabezado `X-API-KEY` está en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="37b4c-199">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="37b4c-200">Si falta `X-API-KEY`, se devuelve <xref:System.Net.HttpStatusCode.BadRequest>.</span><span class="sxs-lookup"><span data-stu-id="37b4c-200">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="37b4c-201">Se puede agregar más de un controlador a la configuración de una instancia de `HttpClient` con <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span><span class="sxs-lookup"><span data-stu-id="37b4c-201">More than one handler can be added to the configuration for an `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="37b4c-202">En el código anterior, `ValidateHeaderHandler` se ha registrado con inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="37b4c-202">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="37b4c-203">`IHttpClientFactory` crea un ámbito de inserción de dependencias independiente para cada controlador.</span><span class="sxs-lookup"><span data-stu-id="37b4c-203">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="37b4c-204">Los controladores pueden depender de servicios de cualquier ámbito.</span><span class="sxs-lookup"><span data-stu-id="37b4c-204">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="37b4c-205">Los servicios de los que dependen los controladores se eliminan cuando se elimina el controlador.</span><span class="sxs-lookup"><span data-stu-id="37b4c-205">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="37b4c-206">Una vez registrado, se puede llamar a <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, pasando el tipo del controlador.</span><span class="sxs-lookup"><span data-stu-id="37b4c-206">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="37b4c-207">Se pueden registrar varios controladores en el orden en que deben ejecutarse.</span><span class="sxs-lookup"><span data-stu-id="37b4c-207">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="37b4c-208">Cada controlador contiene el siguiente controlador hasta que el último `HttpClientHandler` ejecuta la solicitud:</span><span class="sxs-lookup"><span data-stu-id="37b4c-208">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="37b4c-209">Use uno de los siguientes enfoques para compartir el estado por solicitud con controladores de mensajes:</span><span class="sxs-lookup"><span data-stu-id="37b4c-209">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="37b4c-210">Pase datos al controlador mediante [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span><span class="sxs-lookup"><span data-stu-id="37b4c-210">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="37b4c-211">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> para acceder a la solicitud actual.</span><span class="sxs-lookup"><span data-stu-id="37b4c-211">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="37b4c-212">Cree un objeto de almacenamiento <xref:System.Threading.AsyncLocal`1> personalizado para pasar los datos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-212">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="37b4c-213">Usar controladores basados en Polly</span><span class="sxs-lookup"><span data-stu-id="37b4c-213">Use Polly-based handlers</span></span>

<span data-ttu-id="37b4c-214">`IHttpClientFactory` se integra con la biblioteca de terceros [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="37b4c-214">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="37b4c-215">Polly es una biblioteca con capacidades de resistencia y control de errores transitorios para .NET.</span><span class="sxs-lookup"><span data-stu-id="37b4c-215">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="37b4c-216">Permite a los desarrolladores expresar directivas como, por ejemplo, de reintento, interruptor, tiempo de espera, aislamiento compartimentado y reserva de forma fluida y segura para los subprocesos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-216">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="37b4c-217">Se proporcionan métodos de extensión para hacer posible el uso de directivas de Polly con instancias de `HttpClient` configuradas.</span><span class="sxs-lookup"><span data-stu-id="37b4c-217">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="37b4c-218">Las extensiones de Polly permiten agregar controladores basados en Polly a los clientes.</span><span class="sxs-lookup"><span data-stu-id="37b4c-218">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="37b4c-219">Polly requiere el paquete NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="37b4c-219">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="37b4c-220">Control de errores transitorios</span><span class="sxs-lookup"><span data-stu-id="37b4c-220">Handle transient faults</span></span>

<span data-ttu-id="37b4c-221">Los errores se suelen producir cuando las llamadas HTTP externas son transitorias.</span><span class="sxs-lookup"><span data-stu-id="37b4c-221">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="37b4c-222"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> permite definir una directiva para controlar los errores transitorios.</span><span class="sxs-lookup"><span data-stu-id="37b4c-222"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="37b4c-223">Las directivas configuradas con `AddTransientHttpErrorPolicy` controlan las respuestas siguientes:</span><span class="sxs-lookup"><span data-stu-id="37b4c-223">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="37b4c-224">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="37b4c-224">HTTP 5xx</span></span>
* <span data-ttu-id="37b4c-225">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="37b4c-225">HTTP 408</span></span>

<span data-ttu-id="37b4c-226">`AddTransientHttpErrorPolicy` proporciona acceso a un objeto `PolicyBuilder` configurado para controlar los errores que representan un posible error transitorio:</span><span class="sxs-lookup"><span data-stu-id="37b4c-226">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="37b4c-227">En el código anterior, se define una directiva `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-227">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="37b4c-228">Las solicitudes erróneas se reintentan hasta tres veces con un retardo de 600 ms entre intentos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-228">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="37b4c-229">Seleccionar directivas dinámicamente</span><span class="sxs-lookup"><span data-stu-id="37b4c-229">Dynamically select policies</span></span>

<span data-ttu-id="37b4c-230">Los métodos de extensión se proporcionan para agregar controladores basados en Polly, por ejemplo, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span><span class="sxs-lookup"><span data-stu-id="37b4c-230">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="37b4c-231">La siguiente sobrecarga de `AddPolicyHandler` inspecciona la solicitud para decidir qué directiva se debe aplicar:</span><span class="sxs-lookup"><span data-stu-id="37b4c-231">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="37b4c-232">En el código anterior, si la solicitud GET saliente es del tipo HTTP, se aplica un tiempo de espera de 10 segundos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-232">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="37b4c-233">En cualquier otro método HTTP, se usa un tiempo de espera de 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-233">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="37b4c-234">Agregar varios controladores de Polly</span><span class="sxs-lookup"><span data-stu-id="37b4c-234">Add multiple Polly handlers</span></span>

<span data-ttu-id="37b4c-235">Es común anidar las directivas de Polly:</span><span class="sxs-lookup"><span data-stu-id="37b4c-235">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="37b4c-236">En el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="37b4c-236">In the preceding example:</span></span>

* <span data-ttu-id="37b4c-237">Se agregan dos controladores.</span><span class="sxs-lookup"><span data-stu-id="37b4c-237">Two handlers are added.</span></span>
* <span data-ttu-id="37b4c-238">El primer controlador usa <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> para agregar una directiva de reintentos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-238">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="37b4c-239">Las solicitudes con error se reintentan hasta tres veces.</span><span class="sxs-lookup"><span data-stu-id="37b4c-239">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="37b4c-240">La segunda llamada a `AddTransientHttpErrorPolicy` agrega una directiva de interruptor.</span><span class="sxs-lookup"><span data-stu-id="37b4c-240">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="37b4c-241">Las solicitudes externas adicionales se bloquean durante 30 segundos si se producen cinco intentos con error seguidos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-241">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="37b4c-242">Las directivas de interruptor tienen estado.</span><span class="sxs-lookup"><span data-stu-id="37b4c-242">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="37b4c-243">Así, todas las llamadas realizadas a través de este cliente comparten el mismo estado de circuito.</span><span class="sxs-lookup"><span data-stu-id="37b4c-243">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="37b4c-244">Agregar directivas desde el Registro de Polly</span><span class="sxs-lookup"><span data-stu-id="37b4c-244">Add policies from the Polly registry</span></span>

<span data-ttu-id="37b4c-245">Una forma de administrar las directivas usadas habitualmente consiste en definirlas una vez y registrarlas con `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-245">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="37b4c-246">En el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="37b4c-246">In the following code:</span></span>

* <span data-ttu-id="37b4c-247">Se agregan las directivas "regular" y "long".</span><span class="sxs-lookup"><span data-stu-id="37b4c-247">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="37b4c-248"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*> agrega las directivas "regular" y "long" del registro.</span><span class="sxs-lookup"><span data-stu-id="37b4c-248"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="37b4c-249">Para más información sobre `IHttpClientFactory` y las integraciones de Polly, vea la [wiki de Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="37b4c-249">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="37b4c-250">HttpClient y administración de la duración</span><span class="sxs-lookup"><span data-stu-id="37b4c-250">HttpClient and lifetime management</span></span>

<span data-ttu-id="37b4c-251">Cada vez que se llama a `HttpClient` en `CreateClient`, se devuelve una nueva instancia de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-251">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="37b4c-252">Se crea un objeto <xref:System.Net.Http.HttpMessageHandler> por cada cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="37b4c-252">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="37b4c-253">La fábrica administra la duración de las instancias de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-253">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="37b4c-254">`IHttpClientFactory` agrupa las instancias de `HttpMessageHandler` creadas por Factory para reducir el consumo de recursos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-254">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="37b4c-255">Se puede reutilizar una instancia de `HttpMessageHandler` del grupo al crear una instancia de `HttpClient` si su duración aún no ha expirado.</span><span class="sxs-lookup"><span data-stu-id="37b4c-255">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="37b4c-256">Se recomienda agrupar controladores porque cada uno de ellos normalmente administra sus propias conexiones HTTP subyacentes.</span><span class="sxs-lookup"><span data-stu-id="37b4c-256">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="37b4c-257">Crear más controladores de los necesarios puede provocar retrasos en la conexión.</span><span class="sxs-lookup"><span data-stu-id="37b4c-257">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="37b4c-258">Además, algunos controladores dejan las conexiones abiertas de forma indefinida, lo que puede impedir que el controlador reaccione ante los cambios de DNS (Sistema de nombres de dominio).</span><span class="sxs-lookup"><span data-stu-id="37b4c-258">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="37b4c-259">La duración de controlador predeterminada es dos minutos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-259">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="37b4c-260">El valor predeterminado se puede reemplazar de forma individual en cada cliente con nombre:</span><span class="sxs-lookup"><span data-stu-id="37b4c-260">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="37b4c-261">Normalmente, las instancias de `HttpClient` se pueden trata como objetos de .NET que **no** requieren eliminación.</span><span class="sxs-lookup"><span data-stu-id="37b4c-261">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="37b4c-262">ya que se cancelan las solicitudes salientes y la instancia de `HttpClient` determinada no se puede usar después de llamar a <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="37b4c-262">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="37b4c-263">`IHttpClientFactory` realiza un seguimiento y elimina los recursos que usan las instancias de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-263">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="37b4c-264">Mantener una sola instancia de `HttpClient` activa durante un período prolongado es un patrón común que se utiliza antes de la concepción de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-264">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="37b4c-265">Este patrón se convierte en innecesario tras la migración a `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-265">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="37b4c-266">Alternativas a IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="37b4c-266">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="37b4c-267">El uso de `IHttpClientFactory` en una aplicación habilitada para la inserción de dependencias evita lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="37b4c-267">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="37b4c-268">Problemas de agotamiento de recursos mediante la agrupación de instancias de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-268">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="37b4c-269">Problemas de DNS obsoletos al recorrer las instancias de `HttpMessageHandler` a intervalos regulares.</span><span class="sxs-lookup"><span data-stu-id="37b4c-269">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="37b4c-270">Existen formas alternativas de solucionar los problemas anteriores mediante una instancia de <xref:System.Net.Http.SocketsHttpHandler> de larga duración.</span><span class="sxs-lookup"><span data-stu-id="37b4c-270">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="37b4c-271">Cree una instancia de `SocketsHttpHandler` al iniciar la aplicación y úsela para la vida útil de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="37b4c-271">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="37b4c-272">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> con un valor adecuado en función de los tiempos de actualización de DNS.</span><span class="sxs-lookup"><span data-stu-id="37b4c-272">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="37b4c-273">Cree instancias de `HttpClient` mediante `new HttpClient(handler, disposeHandler: false)` según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="37b4c-273">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="37b4c-274">Los enfoques anteriores solucionan los problemas de administración de recursos que `IHttpClientFactory` resuelve de forma similar.</span><span class="sxs-lookup"><span data-stu-id="37b4c-274">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="37b4c-275">`SocketsHttpHandler` comparte las conexiones entre las instancias de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-275">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="37b4c-276">Este uso compartido impide el agotamiento del socket.</span><span class="sxs-lookup"><span data-stu-id="37b4c-276">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="37b4c-277">`SocketsHttpHandler` recorre las conexiones según `PooledConnectionLifetime` para evitar problemas de DNS obsoletos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-277">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="37b4c-278">Cookies</span><span class="sxs-lookup"><span data-stu-id="37b4c-278">Cookies</span></span>

<span data-ttu-id="37b4c-279">Las instancias de `HttpMessageHandler` agrupadas generan objetos `CookieContainer` que se comparten.</span><span class="sxs-lookup"><span data-stu-id="37b4c-279">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="37b4c-280">El uso compartido de objetos `CookieContainer` no previsto suele generar código incorrecto.</span><span class="sxs-lookup"><span data-stu-id="37b4c-280">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="37b4c-281">En el caso de las aplicaciones que requieren cookies, tenga en cuenta lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="37b4c-281">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="37b4c-282">Deshabilitar el control automático de las cookies</span><span class="sxs-lookup"><span data-stu-id="37b4c-282">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="37b4c-283">Evitar `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="37b4c-283">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="37b4c-284">Llame a <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> para deshabilitar el control automático de cookies:</span><span class="sxs-lookup"><span data-stu-id="37b4c-284">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="37b4c-285">Registro</span><span class="sxs-lookup"><span data-stu-id="37b4c-285">Logging</span></span>

<span data-ttu-id="37b4c-286">Los clientes que se han creado a través de `IHttpClientFactory` registran mensajes de registro de todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="37b4c-286">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="37b4c-287">Habilite el nivel de información adecuado en la configuración del registro para ver los mensajes de registro predeterminados.</span><span class="sxs-lookup"><span data-stu-id="37b4c-287">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="37b4c-288">El registro de más información, como el registro de encabezados de solicitud, solo se incluye en el nivel de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="37b4c-288">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="37b4c-289">La categoría de registro usada en cada cliente incluye el nombre del cliente.</span><span class="sxs-lookup"><span data-stu-id="37b4c-289">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="37b4c-290">Un cliente denominado *MyNamedClient*, por ejemplo, registra mensajes con una categoría de "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span><span class="sxs-lookup"><span data-stu-id="37b4c-290">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="37b4c-291">Los mensajes con el sufijo *LogicalHandler* se producen fuera de la canalización de controlador de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="37b4c-291">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="37b4c-292">En la solicitud, los mensajes se registran antes de que cualquier otro controlador de la canalización haya procesado la solicitud.</span><span class="sxs-lookup"><span data-stu-id="37b4c-292">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="37b4c-293">En la respuesta, los mensajes se registran después de que cualquier otro controlador de la canalización haya recibido la respuesta.</span><span class="sxs-lookup"><span data-stu-id="37b4c-293">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="37b4c-294">El registro también se produce dentro de la canalización de controlador de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="37b4c-294">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="37b4c-295">En el ejemplo *MyNamedClient*, esos mensajes se registran con la categoría de registro "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span><span class="sxs-lookup"><span data-stu-id="37b4c-295">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="37b4c-296">En la solicitud, esto tiene lugar después de que todos los demás controladores se hayan ejecutado y justo antes de que se envíe la solicitud.</span><span class="sxs-lookup"><span data-stu-id="37b4c-296">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="37b4c-297">En la respuesta, este registro incluye el estado de la respuesta antes de que vuelva a pasar por la canalización del controlador.</span><span class="sxs-lookup"><span data-stu-id="37b4c-297">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="37b4c-298">Al habilitar el registro tanto dentro como fuera de la canalización, se podrán inspeccionar los cambios realizados por otros controladores de la canalización.</span><span class="sxs-lookup"><span data-stu-id="37b4c-298">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="37b4c-299">Esto puede incluir cambios en los encabezados de solicitud o en el código de estado de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="37b4c-299">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="37b4c-300">La inclusión del nombre del cliente en la categoría de registro permite filtrar el registro para clientes con nombre específicos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-300">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="37b4c-301">Configurar HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="37b4c-301">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="37b4c-302">Puede que sea necesario controlar la configuración del elemento `HttpMessageHandler` interno usado por un cliente.</span><span class="sxs-lookup"><span data-stu-id="37b4c-302">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="37b4c-303">Se devuelve un `IHttpClientBuilder` cuando se agregan clientes con nombre o con tipo.</span><span class="sxs-lookup"><span data-stu-id="37b4c-303">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="37b4c-304">Se puede usar el método de extensión <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> para definir un delegado.</span><span class="sxs-lookup"><span data-stu-id="37b4c-304">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="37b4c-305">Este delegado servirá para crear y configurar el elemento principal `HttpMessageHandler` que ese cliente usa:</span><span class="sxs-lookup"><span data-stu-id="37b4c-305">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="37b4c-306">Uso de IHttpClientFactory en una aplicación de consola</span><span class="sxs-lookup"><span data-stu-id="37b4c-306">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="37b4c-307">En una aplicación de consola, agregue las siguientes referencias de paquete al proyecto:</span><span class="sxs-lookup"><span data-stu-id="37b4c-307">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="37b4c-308">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="37b4c-308">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="37b4c-309">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="37b4c-309">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="37b4c-310">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="37b4c-310">In the following example:</span></span>

* <span data-ttu-id="37b4c-311"><xref:System.Net.Http.IHttpClientFactory> está registrado en el contenedor de servicios del [host genérico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="37b4c-311"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="37b4c-312">`MyService` crea una instancia de generador de clientes a partir del servicio, que se usa para crear un elemento `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-312">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="37b4c-313">`HttpClient` se utiliza para recuperar una página web.</span><span class="sxs-lookup"><span data-stu-id="37b4c-313">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="37b4c-314">`Main` crea un ámbito para ejecutar el método `GetPage` del servicio y escribe los primeros 500 caracteres del contenido de la página web en la consola.</span><span class="sxs-lookup"><span data-stu-id="37b4c-314">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="header-propagation-middleware"></a><span data-ttu-id="37b4c-315">Middleware de propagación de encabezados</span><span class="sxs-lookup"><span data-stu-id="37b4c-315">Header propagation middleware</span></span>

<span data-ttu-id="37b4c-316">La propagación de encabezados es un middleware ASP.NET Core que se usa para propagar encabezados HTTP de la solicitud entrante a las solicitudes de cliente HTTP salientes.</span><span class="sxs-lookup"><span data-stu-id="37b4c-316">Header propagation is an ASP.NET Core middleware to propagate HTTP headers from the incoming request to the outgoing HTTP Client requests.</span></span> <span data-ttu-id="37b4c-317">Para utilizar la propagación de encabezados:</span><span class="sxs-lookup"><span data-stu-id="37b4c-317">To use header propagation:</span></span>

* <span data-ttu-id="37b4c-318">Haga referencia al paquete [Microsoft.AspNetCore.HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation).</span><span class="sxs-lookup"><span data-stu-id="37b4c-318">Reference the [Microsoft.AspNetCore.HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation) package.</span></span>
* <span data-ttu-id="37b4c-319">Configure el middleware y `HttpClient` en `Startup`:</span><span class="sxs-lookup"><span data-stu-id="37b4c-319">Configure the middleware and `HttpClient` in `Startup`:</span></span>

  [!code-csharp[](http-requests/samples/3.x/Startup.cs?highlight=5-9,21&name=snippet)]

* <span data-ttu-id="37b4c-320">El cliente incluye los encabezados configurados en las solicitudes salientes:</span><span class="sxs-lookup"><span data-stu-id="37b4c-320">The client includes the configured headers on outbound requests:</span></span>

  ```csharp
  var client = clientFactory.CreateClient("MyForwardingClient");
  var response = client.GetAsync(...);
  ```

## <a name="additional-resources"></a><span data-ttu-id="37b4c-321">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="37b4c-321">Additional resources</span></span>

* [<span data-ttu-id="37b4c-322">Uso de HttpClientFactory para implementar solicitudes HTTP resistentes</span><span class="sxs-lookup"><span data-stu-id="37b4c-322">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="37b4c-323">Implementación de reintentos de llamada HTTP con retroceso exponencial con HttpClientFactory y las directivas de Polly</span><span class="sxs-lookup"><span data-stu-id="37b4c-323">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="37b4c-324">Implementación del patrón de interruptor</span><span class="sxs-lookup"><span data-stu-id="37b4c-324">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)
* [<span data-ttu-id="37b4c-325">Procedimiento para serializar y deserializar JSON en .NET</span><span class="sxs-lookup"><span data-stu-id="37b4c-325">How to serialize and deserialize JSON in .NET</span></span>](/dotnet/standard/serialization/system-text-json-how-to)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="37b4c-326">Por [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) y [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="37b4c-326">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="37b4c-327">Se puede registrar y usar una interfaz <xref:System.Net.Http.IHttpClientFactory> para crear y configurar instancias de <xref:System.Net.Http.HttpClient> en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="37b4c-327">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="37b4c-328">Esto reporta las siguientes ventajas:</span><span class="sxs-lookup"><span data-stu-id="37b4c-328">It offers the following benefits:</span></span>

* <span data-ttu-id="37b4c-329">Proporciona una ubicación central para denominar y configurar instancias de `HttpClient` lógicas.</span><span class="sxs-lookup"><span data-stu-id="37b4c-329">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="37b4c-330">Así, por ejemplo, se puede registrar y configurar un cliente *github* para tener acceso a [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="37b4c-330">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="37b4c-331">y, de igual modo, registrar otro cliente predeterminado para otros fines.</span><span class="sxs-lookup"><span data-stu-id="37b4c-331">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="37b4c-332">Codifica el concepto de middleware saliente a través de controladores de delegación en `HttpClient` y proporciona extensiones para middleware basado en Polly para poder sacar partido de este.</span><span class="sxs-lookup"><span data-stu-id="37b4c-332">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="37b4c-333">Administra la agrupación y duración de las instancias de `HttpClientMessageHandler` subyacentes para evitar los problemas de DNS que suelen producirse al administrar las duraciones de `HttpClient` manualmente.</span><span class="sxs-lookup"><span data-stu-id="37b4c-333">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="37b4c-334">Agrega una experiencia de registro configurable (a través de `ILogger`) en todas las solicitudes enviadas a través de los clientes creados por Factory.</span><span class="sxs-lookup"><span data-stu-id="37b4c-334">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="37b4c-335">[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="37b4c-335">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="37b4c-336">Patrones de consumo</span><span class="sxs-lookup"><span data-stu-id="37b4c-336">Consumption patterns</span></span>

<span data-ttu-id="37b4c-337">`IHttpClientFactory` se puede usar de varias formas en una aplicación:</span><span class="sxs-lookup"><span data-stu-id="37b4c-337">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="37b4c-338">Uso básico</span><span class="sxs-lookup"><span data-stu-id="37b4c-338">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="37b4c-339">Clientes con nombre</span><span class="sxs-lookup"><span data-stu-id="37b4c-339">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="37b4c-340">Clientes con tipo</span><span class="sxs-lookup"><span data-stu-id="37b4c-340">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="37b4c-341">Clientes generados</span><span class="sxs-lookup"><span data-stu-id="37b4c-341">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="37b4c-342">Ninguno de ellos es rigurosamente superior a otro,</span><span class="sxs-lookup"><span data-stu-id="37b4c-342">None of them are strictly superior to another.</span></span> <span data-ttu-id="37b4c-343">sino que el mejor método dependerá de las restricciones particulares de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="37b4c-343">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="37b4c-344">Uso básico</span><span class="sxs-lookup"><span data-stu-id="37b4c-344">Basic usage</span></span>

<span data-ttu-id="37b4c-345">Se puede registrar un `IHttpClientFactory` llamando al método de extensión `AddHttpClient` en `IServiceCollection`, dentro del método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-345">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="37b4c-346">Una vez registrado, el código puede aceptar una interfaz `IHttpClientFactory` en cualquier parte donde se puedan insertar servicios por medio de la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="37b4c-346">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="37b4c-347">`IHttpClientFactory` se puede usar para crear una instancia de `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="37b4c-347">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="37b4c-348">Cuando `IHttpClientFactory` se usa de este modo, constituye una excelente manera de refactorizar una aplicación existente.</span><span class="sxs-lookup"><span data-stu-id="37b4c-348">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="37b4c-349">No tiene efecto alguno en la forma en que `HttpClient` se usa.</span><span class="sxs-lookup"><span data-stu-id="37b4c-349">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="37b4c-350">En aquellos sitios en los que ya se hayan creado instancias de `HttpClient`, reemplace esas apariciones por una llamada a <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="37b4c-350">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="37b4c-351">Clientes con nombre</span><span class="sxs-lookup"><span data-stu-id="37b4c-351">Named clients</span></span>

<span data-ttu-id="37b4c-352">Si una aplicación necesita usar `HttpClient` de diversas maneras, cada una con una configuración diferente, una opción consiste en usar **clientes con nombre**.</span><span class="sxs-lookup"><span data-stu-id="37b4c-352">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="37b4c-353">La configuración de un `HttpClient` con nombre se puede realizar durante la fase de registro en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-353">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="37b4c-354">En el código anterior, se llama a `AddHttpClient` usando el nombre *github*.</span><span class="sxs-lookup"><span data-stu-id="37b4c-354">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="37b4c-355">Este cliente tiene aplicadas algunas configuraciones predeterminadas, a saber, la dirección base y dos encabezados necesarios para trabajar con la API de GitHub.</span><span class="sxs-lookup"><span data-stu-id="37b4c-355">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="37b4c-356">Cada vez que se llama a `CreateClient`, se crea otra instancia de `HttpClient` y se llama a la acción de configuración.</span><span class="sxs-lookup"><span data-stu-id="37b4c-356">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="37b4c-357">Para consumir un cliente con nombre, se puede pasar un parámetro de cadena a `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-357">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="37b4c-358">Especifique el nombre del cliente que se va a crear:</span><span class="sxs-lookup"><span data-stu-id="37b4c-358">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="37b4c-359">En el código anterior, no es necesario especificar un nombre de host en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="37b4c-359">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="37b4c-360">Basta con pasar solo la ruta de acceso, ya que se usa la dirección base configurada del cliente.</span><span class="sxs-lookup"><span data-stu-id="37b4c-360">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="37b4c-361">Clientes con tipo</span><span class="sxs-lookup"><span data-stu-id="37b4c-361">Typed clients</span></span>

<span data-ttu-id="37b4c-362">Clientes con tipo:</span><span class="sxs-lookup"><span data-stu-id="37b4c-362">Typed clients:</span></span>

* <span data-ttu-id="37b4c-363">Proporcionan las mismas funciones que los clientes con nombre sin la necesidad de usar cadenas como claves.</span><span class="sxs-lookup"><span data-stu-id="37b4c-363">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="37b4c-364">Ofrecen ayuda relativa al compilador e IntelliSense al consumir clientes.</span><span class="sxs-lookup"><span data-stu-id="37b4c-364">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="37b4c-365">Facilitan una sola ubicación para configurar un elemento `HttpClient` determinado e interactuar con él.</span><span class="sxs-lookup"><span data-stu-id="37b4c-365">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="37b4c-366">Por ejemplo, el mismo cliente con tipo se puede usar para un punto de conexión back-end único y encapsular toda la lógica que se ocupa de ese punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="37b4c-366">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="37b4c-367">Funcionan con la inserción de dependencias y se pueden insertar en la aplicación cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="37b4c-367">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="37b4c-368">Un cliente con tipo acepta un parámetro `HttpClient` en su constructor:</span><span class="sxs-lookup"><span data-stu-id="37b4c-368">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="37b4c-369">En el código anterior, la configuración se mueve al cliente con tipo.</span><span class="sxs-lookup"><span data-stu-id="37b4c-369">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="37b4c-370">El objeto `HttpClient` se expone como una propiedad pública.</span><span class="sxs-lookup"><span data-stu-id="37b4c-370">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="37b4c-371">Se pueden definir métodos específicos de API que exponen la funcionalidad `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-371">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="37b4c-372">El método `GetAspNetDocsIssues` encapsula el código necesario para consultar y analizar los últimos problemas abiertos de un repositorio de GitHub.</span><span class="sxs-lookup"><span data-stu-id="37b4c-372">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="37b4c-373">Para registrar un cliente con tipo, se puede usar el método de extensión genérico <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> dentro de `Startup.ConfigureServices`, especificando la clase del cliente con tipo:</span><span class="sxs-lookup"><span data-stu-id="37b4c-373">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="37b4c-374">El cliente con tipo se registra como transitorio con inserción con dependencias,</span><span class="sxs-lookup"><span data-stu-id="37b4c-374">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="37b4c-375">y se puede insertar y consumir directamente:</span><span class="sxs-lookup"><span data-stu-id="37b4c-375">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="37b4c-376">Si lo prefiere, la configuración de un cliente con nombre se puede especificar durante su registro en `Startup.ConfigureServices`, en lugar de en su constructor:</span><span class="sxs-lookup"><span data-stu-id="37b4c-376">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="37b4c-377">El `HttpClient` se puede encapsular completamente dentro de un cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="37b4c-377">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="37b4c-378">En lugar de exponerlo como una propiedad, se pueden proporcionar métodos públicos que llamen a la instancia de `HttpClient` internamente.</span><span class="sxs-lookup"><span data-stu-id="37b4c-378">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="37b4c-379">En el código anterior, el `HttpClient` se almacena como un campo privado.</span><span class="sxs-lookup"><span data-stu-id="37b4c-379">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="37b4c-380">Todo el acceso para realizar llamadas externas pasa por el método `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-380">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="37b4c-381">Clientes generados</span><span class="sxs-lookup"><span data-stu-id="37b4c-381">Generated clients</span></span>

<span data-ttu-id="37b4c-382">`IHttpClientFactory` se puede usar en combinación con otras bibliotecas de terceros, como [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="37b4c-382">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="37b4c-383">Refit es una biblioteca de REST para .NET</span><span class="sxs-lookup"><span data-stu-id="37b4c-383">Refit is a REST library for .NET.</span></span> <span data-ttu-id="37b4c-384">que convierte las API de REST en interfaces en vivo.</span><span class="sxs-lookup"><span data-stu-id="37b4c-384">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="37b4c-385">Se genera una implementación de la interfaz dinámicamente por medio de `RestService`, usando `HttpClient` para realizar las llamadas HTTP externas.</span><span class="sxs-lookup"><span data-stu-id="37b4c-385">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="37b4c-386">Se define una interfaz y una respuesta para representar la API externa y su correspondiente respuesta:</span><span class="sxs-lookup"><span data-stu-id="37b4c-386">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="37b4c-387">Un cliente con tipo se puede agregar usando Refit para generar la implementación:</span><span class="sxs-lookup"><span data-stu-id="37b4c-387">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="37b4c-388">La interfaz definida se puede usar cuando sea preciso con la implementación proporcionada por la inserción de dependencias y Refit:</span><span class="sxs-lookup"><span data-stu-id="37b4c-388">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="37b4c-389">Middleware de solicitud saliente</span><span class="sxs-lookup"><span data-stu-id="37b4c-389">Outgoing request middleware</span></span>

<span data-ttu-id="37b4c-390">`HttpClient` ya posee el concepto de controladores de delegación, que se pueden vincular entre sí para las solicitudes HTTP salientes.</span><span class="sxs-lookup"><span data-stu-id="37b4c-390">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="37b4c-391">`IHttpClientFactory` permite definir fácilmente los controladores que se usarán en cada cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="37b4c-391">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="37b4c-392">Admite el registro y encadenamiento de varios controladores para crear una canalización de middleware de solicitud saliente.</span><span class="sxs-lookup"><span data-stu-id="37b4c-392">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="37b4c-393">Cada uno de estos controladores es capaz de realizar la tarea antes y después de la solicitud de salida.</span><span class="sxs-lookup"><span data-stu-id="37b4c-393">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="37b4c-394">Este patrón es similar a la canalización de middleware de entrada de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="37b4c-394">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="37b4c-395">Dicho patrón proporciona un mecanismo para administrar cuestiones transversales relativas a las solicitudes HTTP, como el almacenamiento en caché, el control de errores, la serialización y el registro.</span><span class="sxs-lookup"><span data-stu-id="37b4c-395">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="37b4c-396">Para crear un controlador, defina una clase que se derive de <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="37b4c-396">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="37b4c-397">Invalide el método `SendAsync` para ejecutar el código antes de pasar la solicitud al siguiente controlador de la canalización:</span><span class="sxs-lookup"><span data-stu-id="37b4c-397">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="37b4c-398">El código anterior define un controlador básico.</span><span class="sxs-lookup"><span data-stu-id="37b4c-398">The preceding code defines a basic handler.</span></span> <span data-ttu-id="37b4c-399">Comprueba si se ha incluido un encabezado `X-API-KEY` en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="37b4c-399">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="37b4c-400">Si no está presente, puede evitar la llamada HTTP y devolver una respuesta adecuada.</span><span class="sxs-lookup"><span data-stu-id="37b4c-400">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="37b4c-401">Durante el registro, se pueden agregar uno o varios controladores a la configuración de una instancia de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-401">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="37b4c-402">Esta tarea se realiza a través de métodos de extensión en <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="37b4c-402">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="37b4c-403">En el código anterior, `ValidateHeaderHandler` se ha registrado con inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="37b4c-403">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="37b4c-404">`IHttpClientFactory` crea un ámbito de inserción de dependencias independiente para cada controlador.</span><span class="sxs-lookup"><span data-stu-id="37b4c-404">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="37b4c-405">Los controladores pueden depender de servicios de cualquier ámbito.</span><span class="sxs-lookup"><span data-stu-id="37b4c-405">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="37b4c-406">Los servicios de los que dependen los controladores se eliminan cuando se elimina el controlador.</span><span class="sxs-lookup"><span data-stu-id="37b4c-406">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="37b4c-407">Una vez registrado, se puede llamar a <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, pasando el tipo del controlador.</span><span class="sxs-lookup"><span data-stu-id="37b4c-407">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="37b4c-408">Se pueden registrar varios controladores en el orden en que deben ejecutarse.</span><span class="sxs-lookup"><span data-stu-id="37b4c-408">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="37b4c-409">Cada controlador contiene el siguiente controlador hasta que el último `HttpClientHandler` ejecuta la solicitud:</span><span class="sxs-lookup"><span data-stu-id="37b4c-409">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="37b4c-410">Use uno de los siguientes enfoques para compartir el estado por solicitud con controladores de mensajes:</span><span class="sxs-lookup"><span data-stu-id="37b4c-410">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="37b4c-411">Pase datos al controlador usando `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-411">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="37b4c-412">Use `IHttpContextAccessor` para acceder a la solicitud actual.</span><span class="sxs-lookup"><span data-stu-id="37b4c-412">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="37b4c-413">Cree un objeto de almacenamiento `AsyncLocal` personalizado para pasar los datos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-413">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="37b4c-414">Usar controladores basados en Polly</span><span class="sxs-lookup"><span data-stu-id="37b4c-414">Use Polly-based handlers</span></span>

<span data-ttu-id="37b4c-415">`IHttpClientFactory` se integra con una biblioteca de terceros muy conocida denominada [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="37b4c-415">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="37b4c-416">Polly es una biblioteca con capacidades de resistencia y control de errores transitorios para .NET.</span><span class="sxs-lookup"><span data-stu-id="37b4c-416">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="37b4c-417">Permite a los desarrolladores expresar directivas como, por ejemplo, de reintento, interruptor, tiempo de espera, aislamiento compartimentado y reserva de forma fluida y segura para los subprocesos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-417">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="37b4c-418">Se proporcionan métodos de extensión para hacer posible el uso de directivas de Polly con instancias de `HttpClient` configuradas.</span><span class="sxs-lookup"><span data-stu-id="37b4c-418">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="37b4c-419">Las extensiones de Polly:</span><span class="sxs-lookup"><span data-stu-id="37b4c-419">The Polly extensions:</span></span>

* <span data-ttu-id="37b4c-420">permiten agregar controladores basados en Polly a los clientes;</span><span class="sxs-lookup"><span data-stu-id="37b4c-420">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="37b4c-421">se pueden usar tras instalar el paquete NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/),</span><span class="sxs-lookup"><span data-stu-id="37b4c-421">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="37b4c-422">aunque este no está incluido en la plataforma compartida ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="37b4c-422">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="37b4c-423">Control de errores transitorios</span><span class="sxs-lookup"><span data-stu-id="37b4c-423">Handle transient faults</span></span>

<span data-ttu-id="37b4c-424">Los errores más comunes se producen cuando las llamadas HTTP externas son transitorias.</span><span class="sxs-lookup"><span data-stu-id="37b4c-424">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="37b4c-425">Por ello, se incluye un método de extensión muy práctico denominado `AddTransientHttpErrorPolicy`, que permite definir una directiva para controlar los errores transitorios.</span><span class="sxs-lookup"><span data-stu-id="37b4c-425">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="37b4c-426">Las directivas que se configuran con este método de extensión controlan `HttpRequestException`, las respuestas HTTP 5xx y las respuestas HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="37b4c-426">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="37b4c-427">La extensión `AddTransientHttpErrorPolicy` se puede usar en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-427">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="37b4c-428">La extensión da acceso a un objeto `PolicyBuilder`, configurado para controlar los errores que pueden constituir un posible error transitorio:</span><span class="sxs-lookup"><span data-stu-id="37b4c-428">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="37b4c-429">En el código anterior, se define una directiva `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-429">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="37b4c-430">Las solicitudes erróneas se reintentan hasta tres veces con un retardo de 600 ms entre intentos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-430">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="37b4c-431">Seleccionar directivas dinámicamente</span><span class="sxs-lookup"><span data-stu-id="37b4c-431">Dynamically select policies</span></span>

<span data-ttu-id="37b4c-432">Existen más métodos de extensión que pueden servir para agregar controladores basados en Polly.</span><span class="sxs-lookup"><span data-stu-id="37b4c-432">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="37b4c-433">Una de esas extensiones es `AddPolicyHandler`, que tiene varias sobrecargas.</span><span class="sxs-lookup"><span data-stu-id="37b4c-433">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="37b4c-434">Una de esas sobrecargas permite inspeccionar la solicitud al dilucidar qué directiva aplicar:</span><span class="sxs-lookup"><span data-stu-id="37b4c-434">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="37b4c-435">En el código anterior, si la solicitud GET saliente es del tipo HTTP, se aplica un tiempo de espera de 10 segundos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-435">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="37b4c-436">En cualquier otro método HTTP, se usa un tiempo de espera de 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-436">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="37b4c-437">Agregar varios controladores de Polly</span><span class="sxs-lookup"><span data-stu-id="37b4c-437">Add multiple Polly handlers</span></span>

<span data-ttu-id="37b4c-438">Es habitual anidar directivas de Polly para proporcionar una funcionalidad mejorada:</span><span class="sxs-lookup"><span data-stu-id="37b4c-438">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="37b4c-439">En el ejemplo anterior, se agregan dos controladores.</span><span class="sxs-lookup"><span data-stu-id="37b4c-439">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="37b4c-440">En el primer ejemplo se usa la extensión `AddTransientHttpErrorPolicy` para agregar una directiva de reintento.</span><span class="sxs-lookup"><span data-stu-id="37b4c-440">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="37b4c-441">Las solicitudes con error se reintentan hasta tres veces.</span><span class="sxs-lookup"><span data-stu-id="37b4c-441">Failed requests are retried up to three times.</span></span> <span data-ttu-id="37b4c-442">La segunda llamada a `AddTransientHttpErrorPolicy` agrega una directiva de interruptor.</span><span class="sxs-lookup"><span data-stu-id="37b4c-442">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="37b4c-443">Las solicitudes externas subsiguientes se bloquean durante 30 segundos si se producen cinco intentos infructuosos seguidos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-443">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="37b4c-444">Las directivas de interruptor tienen estado.</span><span class="sxs-lookup"><span data-stu-id="37b4c-444">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="37b4c-445">Así, todas las llamadas realizadas a través de este cliente comparten el mismo estado de circuito.</span><span class="sxs-lookup"><span data-stu-id="37b4c-445">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="37b4c-446">Agregar directivas desde el Registro de Polly</span><span class="sxs-lookup"><span data-stu-id="37b4c-446">Add policies from the Polly registry</span></span>

<span data-ttu-id="37b4c-447">Una forma de administrar las directivas usadas habitualmente consiste en definirlas una vez y registrarlas con `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-447">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="37b4c-448">Se proporciona un método de extensión que permite agregar un controlador por medio de una directiva del Registro:</span><span class="sxs-lookup"><span data-stu-id="37b4c-448">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="37b4c-449">En el código anterior, se registran dos directivas cuando se agrega `PolicyRegistry` a `ServiceCollection`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-449">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="37b4c-450">Para usar una directiva del Registro, se usa el método `AddPolicyHandlerFromRegistry` pasando el nombre de la directiva que se va a aplicar.</span><span class="sxs-lookup"><span data-stu-id="37b4c-450">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="37b4c-451">Encontrará más información sobre `IHttpClientFactory` y las integraciones de Polly en la [wiki de Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="37b4c-451">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="37b4c-452">HttpClient y administración de la duración</span><span class="sxs-lookup"><span data-stu-id="37b4c-452">HttpClient and lifetime management</span></span>

<span data-ttu-id="37b4c-453">Cada vez que se llama a `HttpClient` en `CreateClient`, se devuelve una nueva instancia de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-453">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="37b4c-454">Hay un controlador <xref:System.Net.Http.HttpMessageHandler> por cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="37b4c-454">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="37b4c-455">La fábrica administra la duración de las instancias de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-455">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="37b4c-456">`IHttpClientFactory` agrupa las instancias de `HttpMessageHandler` creadas por Factory para reducir el consumo de recursos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-456">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="37b4c-457">Se puede reutilizar una instancia de `HttpMessageHandler` del grupo al crear una instancia de `HttpClient` si su duración aún no ha expirado.</span><span class="sxs-lookup"><span data-stu-id="37b4c-457">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="37b4c-458">Se recomienda agrupar controladores porque cada uno de ellos normalmente administra sus propias conexiones HTTP subyacentes.</span><span class="sxs-lookup"><span data-stu-id="37b4c-458">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="37b4c-459">Crear más controladores de los necesarios puede provocar retrasos en la conexión.</span><span class="sxs-lookup"><span data-stu-id="37b4c-459">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="37b4c-460">Además, algunos controladores dejan las conexiones abiertas de forma indefinida, lo que puede ser un obstáculo a la hora de reaccionar ante los cambios de DNS.</span><span class="sxs-lookup"><span data-stu-id="37b4c-460">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="37b4c-461">La duración de controlador predeterminada es dos minutos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-461">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="37b4c-462">El valor predeterminado se puede reemplazar individualmente en cada cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="37b4c-462">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="37b4c-463">Para ello, llame a <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> en el `IHttpClientBuilder` que se devuelve cuando se crea el cliente:</span><span class="sxs-lookup"><span data-stu-id="37b4c-463">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="37b4c-464">No hace falta eliminar el cliente,</span><span class="sxs-lookup"><span data-stu-id="37b4c-464">Disposal of the client isn't required.</span></span> <span data-ttu-id="37b4c-465">ya que se cancelan las solicitudes salientes y la instancia de `HttpClient` determinada no se puede usar después de llamar a <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="37b4c-465">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="37b4c-466">`IHttpClientFactory` realiza un seguimiento y elimina los recursos que usan las instancias de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-466">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="37b4c-467">Normalmente, las instancias de `HttpClient` pueden tratarse como objetos de .NET que no requieren eliminación.</span><span class="sxs-lookup"><span data-stu-id="37b4c-467">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="37b4c-468">Mantener una sola instancia de `HttpClient` activa durante un período prolongado es un patrón común que se utiliza antes de la concepción de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-468">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="37b4c-469">Este patrón se convierte en innecesario tras la migración a `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-469">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="37b4c-470">Alternativas a IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="37b4c-470">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="37b4c-471">El uso de `IHttpClientFactory` en una aplicación habilitada para la inserción de dependencias evita lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="37b4c-471">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="37b4c-472">Problemas de agotamiento de recursos mediante la agrupación de instancias de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-472">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="37b4c-473">Problemas de DNS obsoletos al recorrer las instancias de `HttpMessageHandler` a intervalos regulares.</span><span class="sxs-lookup"><span data-stu-id="37b4c-473">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="37b4c-474">Existen formas alternativas de solucionar los problemas anteriores mediante una instancia de <xref:System.Net.Http.SocketsHttpHandler> de larga duración.</span><span class="sxs-lookup"><span data-stu-id="37b4c-474">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="37b4c-475">Cree una instancia de `SocketsHttpHandler` al iniciar la aplicación y úsela para la vida útil de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="37b4c-475">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="37b4c-476">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> con un valor adecuado en función de los tiempos de actualización de DNS.</span><span class="sxs-lookup"><span data-stu-id="37b4c-476">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="37b4c-477">Cree instancias de `HttpClient` mediante `new HttpClient(handler, disposeHandler: false)` según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="37b4c-477">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="37b4c-478">Los enfoques anteriores solucionan los problemas de administración de recursos que `IHttpClientFactory` resuelve de forma similar.</span><span class="sxs-lookup"><span data-stu-id="37b4c-478">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="37b4c-479">`SocketsHttpHandler` comparte las conexiones entre las instancias de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-479">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="37b4c-480">Este uso compartido impide el agotamiento del socket.</span><span class="sxs-lookup"><span data-stu-id="37b4c-480">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="37b4c-481">`SocketsHttpHandler` recorre las conexiones según `PooledConnectionLifetime` para evitar problemas de DNS obsoletos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-481">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="37b4c-482">Cookies</span><span class="sxs-lookup"><span data-stu-id="37b4c-482">Cookies</span></span>

<span data-ttu-id="37b4c-483">Las instancias de `HttpMessageHandler` agrupadas generan objetos `CookieContainer` que se comparten.</span><span class="sxs-lookup"><span data-stu-id="37b4c-483">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="37b4c-484">El uso compartido de objetos `CookieContainer` no previsto suele generar código incorrecto.</span><span class="sxs-lookup"><span data-stu-id="37b4c-484">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="37b4c-485">En el caso de las aplicaciones que requieren cookies, tenga en cuenta lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="37b4c-485">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="37b4c-486">Deshabilitar el control automático de las cookies</span><span class="sxs-lookup"><span data-stu-id="37b4c-486">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="37b4c-487">Evitar `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="37b4c-487">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="37b4c-488">Llame a <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> para deshabilitar el control automático de cookies:</span><span class="sxs-lookup"><span data-stu-id="37b4c-488">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="37b4c-489">Registro</span><span class="sxs-lookup"><span data-stu-id="37b4c-489">Logging</span></span>

<span data-ttu-id="37b4c-490">Los clientes que se han creado a través de `IHttpClientFactory` registran mensajes de registro de todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="37b4c-490">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="37b4c-491">Habilite el nivel de información adecuado en la configuración del registro para ver los mensajes de registro predeterminados.</span><span class="sxs-lookup"><span data-stu-id="37b4c-491">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="37b4c-492">El registro de más información, como el registro de encabezados de solicitud, solo se incluye en el nivel de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="37b4c-492">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="37b4c-493">La categoría de registro usada en cada cliente incluye el nombre del cliente.</span><span class="sxs-lookup"><span data-stu-id="37b4c-493">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="37b4c-494">Así, por ejemplo, un cliente llamado *MyNamedClient* registra mensajes con una categoría `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-494">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="37b4c-495">Los mensajes con el sufijo *LogicalHandler* se producen fuera de la canalización de controlador de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="37b4c-495">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="37b4c-496">En la solicitud, los mensajes se registran antes de que cualquier otro controlador de la canalización haya procesado la solicitud.</span><span class="sxs-lookup"><span data-stu-id="37b4c-496">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="37b4c-497">En la respuesta, los mensajes se registran después de que cualquier otro controlador de la canalización haya recibido la respuesta.</span><span class="sxs-lookup"><span data-stu-id="37b4c-497">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="37b4c-498">El registro también se produce dentro de la canalización de controlador de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="37b4c-498">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="37b4c-499">En nuestro caso de ejemplo de *MyNamedClient*, esos mensajes se registran en la categoría de registro `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-499">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="37b4c-500">En la solicitud, esto tiene lugar después de que todos los demás controladores se hayan ejecutado y justo antes de que la solicitud se envíe por la red.</span><span class="sxs-lookup"><span data-stu-id="37b4c-500">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="37b4c-501">En la respuesta, este registro incluye el estado de la respuesta antes de que vuelva a pasar por la canalización del controlador.</span><span class="sxs-lookup"><span data-stu-id="37b4c-501">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="37b4c-502">Al habilitar el registro tanto dentro como fuera de la canalización, se podrán inspeccionar los cambios realizados por otros controladores de la canalización.</span><span class="sxs-lookup"><span data-stu-id="37b4c-502">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="37b4c-503">Esto puede englobar cambios, por ejemplo, en los encabezados de solicitud o en el código de estado de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="37b4c-503">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="37b4c-504">Si el nombre del cliente se incluye en la categoría de registro, dicho registro se podrá filtrar para encontrar clientes con nombre específicos cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="37b4c-504">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="37b4c-505">Configurar HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="37b4c-505">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="37b4c-506">Puede que sea necesario controlar la configuración del elemento `HttpMessageHandler` interno usado por un cliente.</span><span class="sxs-lookup"><span data-stu-id="37b4c-506">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="37b4c-507">Se devuelve un `IHttpClientBuilder` cuando se agregan clientes con nombre o con tipo.</span><span class="sxs-lookup"><span data-stu-id="37b4c-507">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="37b4c-508">Se puede usar el método de extensión <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> para definir un delegado.</span><span class="sxs-lookup"><span data-stu-id="37b4c-508">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="37b4c-509">Este delegado servirá para crear y configurar el elemento principal `HttpMessageHandler` que ese cliente usa:</span><span class="sxs-lookup"><span data-stu-id="37b4c-509">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="37b4c-510">Uso de IHttpClientFactory en una aplicación de consola</span><span class="sxs-lookup"><span data-stu-id="37b4c-510">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="37b4c-511">En una aplicación de consola, agregue las siguientes referencias de paquete al proyecto:</span><span class="sxs-lookup"><span data-stu-id="37b4c-511">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="37b4c-512">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="37b4c-512">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="37b4c-513">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="37b4c-513">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="37b4c-514">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="37b4c-514">In the following example:</span></span>

* <span data-ttu-id="37b4c-515"><xref:System.Net.Http.IHttpClientFactory> está registrado en el contenedor de servicios del [host genérico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="37b4c-515"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="37b4c-516">`MyService` crea una instancia de generador de clientes a partir del servicio, que se usa para crear un elemento `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-516">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="37b4c-517">`HttpClient` se utiliza para recuperar una página web.</span><span class="sxs-lookup"><span data-stu-id="37b4c-517">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="37b4c-518">`Main` crea un ámbito para ejecutar el método `GetPage` del servicio y escribe los primeros 500 caracteres del contenido de la página web en la consola.</span><span class="sxs-lookup"><span data-stu-id="37b4c-518">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="37b4c-519">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="37b4c-519">Additional resources</span></span>

* [<span data-ttu-id="37b4c-520">Uso de HttpClientFactory para implementar solicitudes HTTP resistentes</span><span class="sxs-lookup"><span data-stu-id="37b4c-520">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="37b4c-521">Implementación de reintentos de llamada HTTP con retroceso exponencial con HttpClientFactory y las directivas de Polly</span><span class="sxs-lookup"><span data-stu-id="37b4c-521">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="37b4c-522">Implementación del patrón de interruptor</span><span class="sxs-lookup"><span data-stu-id="37b4c-522">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="37b4c-523">Por [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) y [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="37b4c-523">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="37b4c-524">Se puede registrar y usar una interfaz <xref:System.Net.Http.IHttpClientFactory> para crear y configurar instancias de <xref:System.Net.Http.HttpClient> en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="37b4c-524">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="37b4c-525">Esto reporta las siguientes ventajas:</span><span class="sxs-lookup"><span data-stu-id="37b4c-525">It offers the following benefits:</span></span>

* <span data-ttu-id="37b4c-526">Proporciona una ubicación central para denominar y configurar instancias de `HttpClient` lógicas.</span><span class="sxs-lookup"><span data-stu-id="37b4c-526">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="37b4c-527">Así, por ejemplo, se puede registrar y configurar un cliente *github* para tener acceso a [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="37b4c-527">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="37b4c-528">y, de igual modo, registrar otro cliente predeterminado para otros fines.</span><span class="sxs-lookup"><span data-stu-id="37b4c-528">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="37b4c-529">Codifica el concepto de middleware saliente a través de controladores de delegación en `HttpClient` y proporciona extensiones para middleware basado en Polly para poder sacar partido de este.</span><span class="sxs-lookup"><span data-stu-id="37b4c-529">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="37b4c-530">Administra la agrupación y duración de las instancias de `HttpClientMessageHandler` subyacentes para evitar los problemas de DNS que suelen producirse al administrar las duraciones de `HttpClient` manualmente.</span><span class="sxs-lookup"><span data-stu-id="37b4c-530">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="37b4c-531">Agrega una experiencia de registro configurable (a través de `ILogger`) en todas las solicitudes enviadas a través de los clientes creados por Factory.</span><span class="sxs-lookup"><span data-stu-id="37b4c-531">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="37b4c-532">[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="37b4c-532">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="37b4c-533">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="37b4c-533">Prerequisites</span></span>

<span data-ttu-id="37b4c-534">Los proyectos para .NET Framework requieren instalar el paquete NuGet [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/).</span><span class="sxs-lookup"><span data-stu-id="37b4c-534">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="37b4c-535">Los proyectos para .NET Core y que hagan referencia al [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ya incluyen el paquete `Microsoft.Extensions.Http`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-535">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="37b4c-536">Patrones de consumo</span><span class="sxs-lookup"><span data-stu-id="37b4c-536">Consumption patterns</span></span>

<span data-ttu-id="37b4c-537">`IHttpClientFactory` se puede usar de varias formas en una aplicación:</span><span class="sxs-lookup"><span data-stu-id="37b4c-537">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="37b4c-538">Uso básico</span><span class="sxs-lookup"><span data-stu-id="37b4c-538">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="37b4c-539">Clientes con nombre</span><span class="sxs-lookup"><span data-stu-id="37b4c-539">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="37b4c-540">Clientes con tipo</span><span class="sxs-lookup"><span data-stu-id="37b4c-540">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="37b4c-541">Clientes generados</span><span class="sxs-lookup"><span data-stu-id="37b4c-541">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="37b4c-542">Ninguno de ellos es rigurosamente superior a otro,</span><span class="sxs-lookup"><span data-stu-id="37b4c-542">None of them are strictly superior to another.</span></span> <span data-ttu-id="37b4c-543">sino que el mejor método dependerá de las restricciones particulares de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="37b4c-543">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="37b4c-544">Uso básico</span><span class="sxs-lookup"><span data-stu-id="37b4c-544">Basic usage</span></span>

<span data-ttu-id="37b4c-545">Se puede registrar un `IHttpClientFactory` llamando al método de extensión `AddHttpClient` en `IServiceCollection`, dentro del método `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-545">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="37b4c-546">Una vez registrado, el código puede aceptar una interfaz `IHttpClientFactory` en cualquier parte donde se puedan insertar servicios por medio de la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="37b4c-546">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="37b4c-547">`IHttpClientFactory` se puede usar para crear una instancia de `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="37b4c-547">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="37b4c-548">Cuando `IHttpClientFactory` se usa de este modo, constituye una excelente manera de refactorizar una aplicación existente.</span><span class="sxs-lookup"><span data-stu-id="37b4c-548">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="37b4c-549">No tiene efecto alguno en la forma en que `HttpClient` se usa.</span><span class="sxs-lookup"><span data-stu-id="37b4c-549">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="37b4c-550">En aquellos sitios en los que ya se hayan creado instancias de `HttpClient`, reemplace esas apariciones por una llamada a <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="37b4c-550">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="37b4c-551">Clientes con nombre</span><span class="sxs-lookup"><span data-stu-id="37b4c-551">Named clients</span></span>

<span data-ttu-id="37b4c-552">Si una aplicación necesita usar `HttpClient` de diversas maneras, cada una con una configuración diferente, una opción consiste en usar **clientes con nombre**.</span><span class="sxs-lookup"><span data-stu-id="37b4c-552">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="37b4c-553">La configuración de un `HttpClient` con nombre se puede realizar durante la fase de registro en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-553">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="37b4c-554">En el código anterior, se llama a `AddHttpClient` usando el nombre *github*.</span><span class="sxs-lookup"><span data-stu-id="37b4c-554">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="37b4c-555">Este cliente tiene aplicadas algunas configuraciones predeterminadas, a saber, la dirección base y dos encabezados necesarios para trabajar con la API de GitHub.</span><span class="sxs-lookup"><span data-stu-id="37b4c-555">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="37b4c-556">Cada vez que se llama a `CreateClient`, se crea otra instancia de `HttpClient` y se llama a la acción de configuración.</span><span class="sxs-lookup"><span data-stu-id="37b4c-556">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="37b4c-557">Para consumir un cliente con nombre, se puede pasar un parámetro de cadena a `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-557">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="37b4c-558">Especifique el nombre del cliente que se va a crear:</span><span class="sxs-lookup"><span data-stu-id="37b4c-558">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="37b4c-559">En el código anterior, no es necesario especificar un nombre de host en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="37b4c-559">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="37b4c-560">Basta con pasar solo la ruta de acceso, ya que se usa la dirección base configurada del cliente.</span><span class="sxs-lookup"><span data-stu-id="37b4c-560">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="37b4c-561">Clientes con tipo</span><span class="sxs-lookup"><span data-stu-id="37b4c-561">Typed clients</span></span>

<span data-ttu-id="37b4c-562">Clientes con tipo:</span><span class="sxs-lookup"><span data-stu-id="37b4c-562">Typed clients:</span></span>

* <span data-ttu-id="37b4c-563">Proporcionan las mismas funciones que los clientes con nombre sin la necesidad de usar cadenas como claves.</span><span class="sxs-lookup"><span data-stu-id="37b4c-563">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="37b4c-564">Ofrecen ayuda relativa al compilador e IntelliSense al consumir clientes.</span><span class="sxs-lookup"><span data-stu-id="37b4c-564">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="37b4c-565">Facilitan una sola ubicación para configurar un elemento `HttpClient` determinado e interactuar con él.</span><span class="sxs-lookup"><span data-stu-id="37b4c-565">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="37b4c-566">Por ejemplo, el mismo cliente con tipo se puede usar para un punto de conexión back-end único y encapsular toda la lógica que se ocupa de ese punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="37b4c-566">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="37b4c-567">Funcionan con la inserción de dependencias y se pueden insertar en la aplicación cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="37b4c-567">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="37b4c-568">Un cliente con tipo acepta un parámetro `HttpClient` en su constructor:</span><span class="sxs-lookup"><span data-stu-id="37b4c-568">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="37b4c-569">En el código anterior, la configuración se mueve al cliente con tipo.</span><span class="sxs-lookup"><span data-stu-id="37b4c-569">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="37b4c-570">El objeto `HttpClient` se expone como una propiedad pública.</span><span class="sxs-lookup"><span data-stu-id="37b4c-570">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="37b4c-571">Se pueden definir métodos específicos de API que exponen la funcionalidad `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-571">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="37b4c-572">El método `GetAspNetDocsIssues` encapsula el código necesario para consultar y analizar los últimos problemas abiertos de un repositorio de GitHub.</span><span class="sxs-lookup"><span data-stu-id="37b4c-572">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="37b4c-573">Para registrar un cliente con tipo, se puede usar el método de extensión genérico <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> dentro de `Startup.ConfigureServices`, especificando la clase del cliente con tipo:</span><span class="sxs-lookup"><span data-stu-id="37b4c-573">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="37b4c-574">El cliente con tipo se registra como transitorio con inserción con dependencias,</span><span class="sxs-lookup"><span data-stu-id="37b4c-574">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="37b4c-575">y se puede insertar y consumir directamente:</span><span class="sxs-lookup"><span data-stu-id="37b4c-575">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="37b4c-576">Si lo prefiere, la configuración de un cliente con nombre se puede especificar durante su registro en `Startup.ConfigureServices`, en lugar de en su constructor:</span><span class="sxs-lookup"><span data-stu-id="37b4c-576">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="37b4c-577">El `HttpClient` se puede encapsular completamente dentro de un cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="37b4c-577">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="37b4c-578">En lugar de exponerlo como una propiedad, se pueden proporcionar métodos públicos que llamen a la instancia de `HttpClient` internamente.</span><span class="sxs-lookup"><span data-stu-id="37b4c-578">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="37b4c-579">En el código anterior, el `HttpClient` se almacena como un campo privado.</span><span class="sxs-lookup"><span data-stu-id="37b4c-579">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="37b4c-580">Todo el acceso para realizar llamadas externas pasa por el método `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-580">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="37b4c-581">Clientes generados</span><span class="sxs-lookup"><span data-stu-id="37b4c-581">Generated clients</span></span>

<span data-ttu-id="37b4c-582">`IHttpClientFactory` se puede usar en combinación con otras bibliotecas de terceros, como [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="37b4c-582">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="37b4c-583">Refit es una biblioteca de REST para .NET</span><span class="sxs-lookup"><span data-stu-id="37b4c-583">Refit is a REST library for .NET.</span></span> <span data-ttu-id="37b4c-584">que convierte las API de REST en interfaces en vivo.</span><span class="sxs-lookup"><span data-stu-id="37b4c-584">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="37b4c-585">Se genera una implementación de la interfaz dinámicamente por medio de `RestService`, usando `HttpClient` para realizar las llamadas HTTP externas.</span><span class="sxs-lookup"><span data-stu-id="37b4c-585">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="37b4c-586">Se define una interfaz y una respuesta para representar la API externa y su correspondiente respuesta:</span><span class="sxs-lookup"><span data-stu-id="37b4c-586">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="37b4c-587">Un cliente con tipo se puede agregar usando Refit para generar la implementación:</span><span class="sxs-lookup"><span data-stu-id="37b4c-587">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="37b4c-588">La interfaz definida se puede usar cuando sea preciso con la implementación proporcionada por la inserción de dependencias y Refit:</span><span class="sxs-lookup"><span data-stu-id="37b4c-588">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="37b4c-589">Middleware de solicitud saliente</span><span class="sxs-lookup"><span data-stu-id="37b4c-589">Outgoing request middleware</span></span>

<span data-ttu-id="37b4c-590">`HttpClient` ya posee el concepto de controladores de delegación, que se pueden vincular entre sí para las solicitudes HTTP salientes.</span><span class="sxs-lookup"><span data-stu-id="37b4c-590">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="37b4c-591">`IHttpClientFactory` permite definir fácilmente los controladores que se usarán en cada cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="37b4c-591">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="37b4c-592">Admite el registro y encadenamiento de varios controladores para crear una canalización de middleware de solicitud saliente.</span><span class="sxs-lookup"><span data-stu-id="37b4c-592">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="37b4c-593">Cada uno de estos controladores es capaz de realizar la tarea antes y después de la solicitud de salida.</span><span class="sxs-lookup"><span data-stu-id="37b4c-593">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="37b4c-594">Este patrón es similar a la canalización de middleware de entrada de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="37b4c-594">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="37b4c-595">Dicho patrón proporciona un mecanismo para administrar cuestiones transversales relativas a las solicitudes HTTP, como el almacenamiento en caché, el control de errores, la serialización y el registro.</span><span class="sxs-lookup"><span data-stu-id="37b4c-595">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="37b4c-596">Para crear un controlador, defina una clase que se derive de <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="37b4c-596">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="37b4c-597">Invalide el método `SendAsync` para ejecutar el código antes de pasar la solicitud al siguiente controlador de la canalización:</span><span class="sxs-lookup"><span data-stu-id="37b4c-597">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="37b4c-598">El código anterior define un controlador básico.</span><span class="sxs-lookup"><span data-stu-id="37b4c-598">The preceding code defines a basic handler.</span></span> <span data-ttu-id="37b4c-599">Comprueba si se ha incluido un encabezado `X-API-KEY` en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="37b4c-599">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="37b4c-600">Si no está presente, puede evitar la llamada HTTP y devolver una respuesta adecuada.</span><span class="sxs-lookup"><span data-stu-id="37b4c-600">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="37b4c-601">Durante el registro, se pueden agregar uno o varios controladores a la configuración de una instancia de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-601">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="37b4c-602">Esta tarea se realiza a través de métodos de extensión en <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="37b4c-602">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="37b4c-603">En el código anterior, `ValidateHeaderHandler` se ha registrado con inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="37b4c-603">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="37b4c-604">El controlador **debe** estar registrado en la inserción de dependencias como servicio transitorio, nunca como servicio con ámbito.</span><span class="sxs-lookup"><span data-stu-id="37b4c-604">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="37b4c-605">Si el controlador se registra como servicio con ámbito y se pueden eliminar los servicios de los que depende el controlador:</span><span class="sxs-lookup"><span data-stu-id="37b4c-605">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="37b4c-606">Los servicios del controlador se pueden eliminar antes de que el controlador deje de estar en el ámbito.</span><span class="sxs-lookup"><span data-stu-id="37b4c-606">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="37b4c-607">Los servicios del controlador que se eliminen provocarán un error en él.</span><span class="sxs-lookup"><span data-stu-id="37b4c-607">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="37b4c-608">Una vez registrado, se puede llamar a <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, con lo que se pasa el tipo de controlador.</span><span class="sxs-lookup"><span data-stu-id="37b4c-608">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="37b4c-609">Se pueden registrar varios controladores en el orden en que deben ejecutarse.</span><span class="sxs-lookup"><span data-stu-id="37b4c-609">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="37b4c-610">Cada controlador contiene el siguiente controlador hasta que el último `HttpClientHandler` ejecuta la solicitud:</span><span class="sxs-lookup"><span data-stu-id="37b4c-610">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="37b4c-611">Use uno de los siguientes enfoques para compartir el estado por solicitud con controladores de mensajes:</span><span class="sxs-lookup"><span data-stu-id="37b4c-611">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="37b4c-612">Pase datos al controlador usando `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-612">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="37b4c-613">Use `IHttpContextAccessor` para acceder a la solicitud actual.</span><span class="sxs-lookup"><span data-stu-id="37b4c-613">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="37b4c-614">Cree un objeto de almacenamiento `AsyncLocal` personalizado para pasar los datos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-614">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="37b4c-615">Usar controladores basados en Polly</span><span class="sxs-lookup"><span data-stu-id="37b4c-615">Use Polly-based handlers</span></span>

<span data-ttu-id="37b4c-616">`IHttpClientFactory` se integra con una biblioteca de terceros muy conocida denominada [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="37b4c-616">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="37b4c-617">Polly es una biblioteca con capacidades de resistencia y control de errores transitorios para .NET.</span><span class="sxs-lookup"><span data-stu-id="37b4c-617">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="37b4c-618">Permite a los desarrolladores expresar directivas como, por ejemplo, de reintento, interruptor, tiempo de espera, aislamiento compartimentado y reserva de forma fluida y segura para los subprocesos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-618">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="37b4c-619">Se proporcionan métodos de extensión para hacer posible el uso de directivas de Polly con instancias de `HttpClient` configuradas.</span><span class="sxs-lookup"><span data-stu-id="37b4c-619">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="37b4c-620">Las extensiones de Polly:</span><span class="sxs-lookup"><span data-stu-id="37b4c-620">The Polly extensions:</span></span>

* <span data-ttu-id="37b4c-621">permiten agregar controladores basados en Polly a los clientes;</span><span class="sxs-lookup"><span data-stu-id="37b4c-621">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="37b4c-622">se pueden usar tras instalar el paquete NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/),</span><span class="sxs-lookup"><span data-stu-id="37b4c-622">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="37b4c-623">aunque este no está incluido en la plataforma compartida ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="37b4c-623">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="37b4c-624">Control de errores transitorios</span><span class="sxs-lookup"><span data-stu-id="37b4c-624">Handle transient faults</span></span>

<span data-ttu-id="37b4c-625">Los errores más comunes se producen cuando las llamadas HTTP externas son transitorias.</span><span class="sxs-lookup"><span data-stu-id="37b4c-625">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="37b4c-626">Por ello, se incluye un método de extensión muy práctico denominado `AddTransientHttpErrorPolicy`, que permite definir una directiva para controlar los errores transitorios.</span><span class="sxs-lookup"><span data-stu-id="37b4c-626">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="37b4c-627">Las directivas que se configuran con este método de extensión controlan `HttpRequestException`, las respuestas HTTP 5xx y las respuestas HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="37b4c-627">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="37b4c-628">La extensión `AddTransientHttpErrorPolicy` se puede usar en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-628">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="37b4c-629">La extensión da acceso a un objeto `PolicyBuilder`, configurado para controlar los errores que pueden constituir un posible error transitorio:</span><span class="sxs-lookup"><span data-stu-id="37b4c-629">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="37b4c-630">En el código anterior, se define una directiva `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-630">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="37b4c-631">Las solicitudes erróneas se reintentan hasta tres veces con un retardo de 600 ms entre intentos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-631">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="37b4c-632">Seleccionar directivas dinámicamente</span><span class="sxs-lookup"><span data-stu-id="37b4c-632">Dynamically select policies</span></span>

<span data-ttu-id="37b4c-633">Existen más métodos de extensión que pueden servir para agregar controladores basados en Polly.</span><span class="sxs-lookup"><span data-stu-id="37b4c-633">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="37b4c-634">Una de esas extensiones es `AddPolicyHandler`, que tiene varias sobrecargas.</span><span class="sxs-lookup"><span data-stu-id="37b4c-634">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="37b4c-635">Una de esas sobrecargas permite inspeccionar la solicitud al dilucidar qué directiva aplicar:</span><span class="sxs-lookup"><span data-stu-id="37b4c-635">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="37b4c-636">En el código anterior, si la solicitud GET saliente es del tipo HTTP, se aplica un tiempo de espera de 10 segundos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-636">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="37b4c-637">En cualquier otro método HTTP, se usa un tiempo de espera de 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-637">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="37b4c-638">Agregar varios controladores de Polly</span><span class="sxs-lookup"><span data-stu-id="37b4c-638">Add multiple Polly handlers</span></span>

<span data-ttu-id="37b4c-639">Es habitual anidar directivas de Polly para proporcionar una funcionalidad mejorada:</span><span class="sxs-lookup"><span data-stu-id="37b4c-639">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="37b4c-640">En el ejemplo anterior, se agregan dos controladores.</span><span class="sxs-lookup"><span data-stu-id="37b4c-640">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="37b4c-641">En el primer ejemplo se usa la extensión `AddTransientHttpErrorPolicy` para agregar una directiva de reintento.</span><span class="sxs-lookup"><span data-stu-id="37b4c-641">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="37b4c-642">Las solicitudes con error se reintentan hasta tres veces.</span><span class="sxs-lookup"><span data-stu-id="37b4c-642">Failed requests are retried up to three times.</span></span> <span data-ttu-id="37b4c-643">La segunda llamada a `AddTransientHttpErrorPolicy` agrega una directiva de interruptor.</span><span class="sxs-lookup"><span data-stu-id="37b4c-643">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="37b4c-644">Las solicitudes externas subsiguientes se bloquean durante 30 segundos si se producen cinco intentos infructuosos seguidos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-644">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="37b4c-645">Las directivas de interruptor tienen estado.</span><span class="sxs-lookup"><span data-stu-id="37b4c-645">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="37b4c-646">Así, todas las llamadas realizadas a través de este cliente comparten el mismo estado de circuito.</span><span class="sxs-lookup"><span data-stu-id="37b4c-646">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="37b4c-647">Agregar directivas desde el Registro de Polly</span><span class="sxs-lookup"><span data-stu-id="37b4c-647">Add policies from the Polly registry</span></span>

<span data-ttu-id="37b4c-648">Una forma de administrar las directivas usadas habitualmente consiste en definirlas una vez y registrarlas con `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-648">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="37b4c-649">Se proporciona un método de extensión que permite agregar un controlador por medio de una directiva del Registro:</span><span class="sxs-lookup"><span data-stu-id="37b4c-649">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="37b4c-650">En el código anterior, se registran dos directivas cuando se agrega `PolicyRegistry` a `ServiceCollection`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-650">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="37b4c-651">Para usar una directiva del Registro, se usa el método `AddPolicyHandlerFromRegistry` pasando el nombre de la directiva que se va a aplicar.</span><span class="sxs-lookup"><span data-stu-id="37b4c-651">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="37b4c-652">Encontrará más información sobre `IHttpClientFactory` y las integraciones de Polly en la [wiki de Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="37b4c-652">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="37b4c-653">HttpClient y administración de la duración</span><span class="sxs-lookup"><span data-stu-id="37b4c-653">HttpClient and lifetime management</span></span>

<span data-ttu-id="37b4c-654">Cada vez que se llama a `HttpClient` en `CreateClient`, se devuelve una nueva instancia de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-654">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="37b4c-655">Hay un controlador <xref:System.Net.Http.HttpMessageHandler> por cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="37b4c-655">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="37b4c-656">La fábrica administra la duración de las instancias de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-656">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="37b4c-657">`IHttpClientFactory` agrupa las instancias de `HttpMessageHandler` creadas por Factory para reducir el consumo de recursos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-657">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="37b4c-658">Se puede reutilizar una instancia de `HttpMessageHandler` del grupo al crear una instancia de `HttpClient` si su duración aún no ha expirado.</span><span class="sxs-lookup"><span data-stu-id="37b4c-658">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="37b4c-659">Se recomienda agrupar controladores porque cada uno de ellos normalmente administra sus propias conexiones HTTP subyacentes.</span><span class="sxs-lookup"><span data-stu-id="37b4c-659">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="37b4c-660">Crear más controladores de los necesarios puede provocar retrasos en la conexión.</span><span class="sxs-lookup"><span data-stu-id="37b4c-660">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="37b4c-661">Además, algunos controladores dejan las conexiones abiertas de forma indefinida, lo que puede ser un obstáculo a la hora de reaccionar ante los cambios de DNS.</span><span class="sxs-lookup"><span data-stu-id="37b4c-661">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="37b4c-662">La duración de controlador predeterminada es dos minutos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-662">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="37b4c-663">El valor predeterminado se puede reemplazar individualmente en cada cliente con nombre.</span><span class="sxs-lookup"><span data-stu-id="37b4c-663">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="37b4c-664">Para ello, llame a <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> en el `IHttpClientBuilder` que se devuelve cuando se crea el cliente:</span><span class="sxs-lookup"><span data-stu-id="37b4c-664">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="37b4c-665">No hace falta eliminar el cliente,</span><span class="sxs-lookup"><span data-stu-id="37b4c-665">Disposal of the client isn't required.</span></span> <span data-ttu-id="37b4c-666">ya que se cancelan las solicitudes salientes y la instancia de `HttpClient` determinada no se puede usar después de llamar a <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="37b4c-666">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="37b4c-667">`IHttpClientFactory` realiza un seguimiento y elimina los recursos que usan las instancias de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-667">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="37b4c-668">Normalmente, las instancias de `HttpClient` pueden tratarse como objetos de .NET que no requieren eliminación.</span><span class="sxs-lookup"><span data-stu-id="37b4c-668">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="37b4c-669">Mantener una sola instancia de `HttpClient` activa durante un período prolongado es un patrón común que se utiliza antes de la concepción de `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-669">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="37b4c-670">Este patrón se convierte en innecesario tras la migración a `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-670">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="37b4c-671">Alternativas a IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="37b4c-671">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="37b4c-672">El uso de `IHttpClientFactory` en una aplicación habilitada para la inserción de dependencias evita lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="37b4c-672">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="37b4c-673">Problemas de agotamiento de recursos mediante la agrupación de instancias de `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-673">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="37b4c-674">Problemas de DNS obsoletos al recorrer las instancias de `HttpMessageHandler` a intervalos regulares.</span><span class="sxs-lookup"><span data-stu-id="37b4c-674">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="37b4c-675">Existen formas alternativas de solucionar los problemas anteriores mediante una instancia de <xref:System.Net.Http.SocketsHttpHandler> de larga duración.</span><span class="sxs-lookup"><span data-stu-id="37b4c-675">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="37b4c-676">Cree una instancia de `SocketsHttpHandler` al iniciar la aplicación y úsela para la vida útil de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="37b4c-676">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="37b4c-677">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> con un valor adecuado en función de los tiempos de actualización de DNS.</span><span class="sxs-lookup"><span data-stu-id="37b4c-677">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="37b4c-678">Cree instancias de `HttpClient` mediante `new HttpClient(handler, disposeHandler: false)` según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="37b4c-678">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="37b4c-679">Los enfoques anteriores solucionan los problemas de administración de recursos que `IHttpClientFactory` resuelve de forma similar.</span><span class="sxs-lookup"><span data-stu-id="37b4c-679">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="37b4c-680">`SocketsHttpHandler` comparte las conexiones entre las instancias de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-680">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="37b4c-681">Este uso compartido impide el agotamiento del socket.</span><span class="sxs-lookup"><span data-stu-id="37b4c-681">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="37b4c-682">`SocketsHttpHandler` recorre las conexiones según `PooledConnectionLifetime` para evitar problemas de DNS obsoletos.</span><span class="sxs-lookup"><span data-stu-id="37b4c-682">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="37b4c-683">Cookies</span><span class="sxs-lookup"><span data-stu-id="37b4c-683">Cookies</span></span>

<span data-ttu-id="37b4c-684">Las instancias de `HttpMessageHandler` agrupadas generan objetos `CookieContainer` que se comparten.</span><span class="sxs-lookup"><span data-stu-id="37b4c-684">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="37b4c-685">El uso compartido de objetos `CookieContainer` no previsto suele generar código incorrecto.</span><span class="sxs-lookup"><span data-stu-id="37b4c-685">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="37b4c-686">En el caso de las aplicaciones que requieren cookies, tenga en cuenta lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="37b4c-686">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="37b4c-687">Deshabilitar el control automático de las cookies</span><span class="sxs-lookup"><span data-stu-id="37b4c-687">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="37b4c-688">Evitar `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="37b4c-688">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="37b4c-689">Llame a <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> para deshabilitar el control automático de cookies:</span><span class="sxs-lookup"><span data-stu-id="37b4c-689">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="37b4c-690">Registro</span><span class="sxs-lookup"><span data-stu-id="37b4c-690">Logging</span></span>

<span data-ttu-id="37b4c-691">Los clientes que se han creado a través de `IHttpClientFactory` registran mensajes de registro de todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="37b4c-691">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="37b4c-692">Habilite el nivel de información adecuado en la configuración del registro para ver los mensajes de registro predeterminados.</span><span class="sxs-lookup"><span data-stu-id="37b4c-692">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="37b4c-693">El registro de más información, como el registro de encabezados de solicitud, solo se incluye en el nivel de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="37b4c-693">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="37b4c-694">La categoría de registro usada en cada cliente incluye el nombre del cliente.</span><span class="sxs-lookup"><span data-stu-id="37b4c-694">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="37b4c-695">Así, por ejemplo, un cliente llamado *MyNamedClient* registra mensajes con una categoría `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-695">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="37b4c-696">Los mensajes con el sufijo *LogicalHandler* se producen fuera de la canalización de controlador de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="37b4c-696">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="37b4c-697">En la solicitud, los mensajes se registran antes de que cualquier otro controlador de la canalización haya procesado la solicitud.</span><span class="sxs-lookup"><span data-stu-id="37b4c-697">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="37b4c-698">En la respuesta, los mensajes se registran después de que cualquier otro controlador de la canalización haya recibido la respuesta.</span><span class="sxs-lookup"><span data-stu-id="37b4c-698">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="37b4c-699">El registro también se produce dentro de la canalización de controlador de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="37b4c-699">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="37b4c-700">En nuestro caso de ejemplo de *MyNamedClient*, esos mensajes se registran en la categoría de registro `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-700">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="37b4c-701">En la solicitud, esto tiene lugar después de que todos los demás controladores se hayan ejecutado y justo antes de que la solicitud se envíe por la red.</span><span class="sxs-lookup"><span data-stu-id="37b4c-701">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="37b4c-702">En la respuesta, este registro incluye el estado de la respuesta antes de que vuelva a pasar por la canalización del controlador.</span><span class="sxs-lookup"><span data-stu-id="37b4c-702">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="37b4c-703">Al habilitar el registro tanto dentro como fuera de la canalización, se podrán inspeccionar los cambios realizados por otros controladores de la canalización.</span><span class="sxs-lookup"><span data-stu-id="37b4c-703">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="37b4c-704">Esto puede englobar cambios, por ejemplo, en los encabezados de solicitud o en el código de estado de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="37b4c-704">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="37b4c-705">Si el nombre del cliente se incluye en la categoría de registro, dicho registro se podrá filtrar para encontrar clientes con nombre específicos cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="37b4c-705">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="37b4c-706">Configurar HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="37b4c-706">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="37b4c-707">Puede que sea necesario controlar la configuración del elemento `HttpMessageHandler` interno usado por un cliente.</span><span class="sxs-lookup"><span data-stu-id="37b4c-707">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="37b4c-708">Se devuelve un `IHttpClientBuilder` cuando se agregan clientes con nombre o con tipo.</span><span class="sxs-lookup"><span data-stu-id="37b4c-708">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="37b4c-709">Se puede usar el método de extensión <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> para definir un delegado.</span><span class="sxs-lookup"><span data-stu-id="37b4c-709">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="37b4c-710">Este delegado servirá para crear y configurar el elemento principal `HttpMessageHandler` que ese cliente usa:</span><span class="sxs-lookup"><span data-stu-id="37b4c-710">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="37b4c-711">Uso de IHttpClientFactory en una aplicación de consola</span><span class="sxs-lookup"><span data-stu-id="37b4c-711">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="37b4c-712">En una aplicación de consola, agregue las siguientes referencias de paquete al proyecto:</span><span class="sxs-lookup"><span data-stu-id="37b4c-712">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="37b4c-713">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="37b4c-713">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="37b4c-714">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="37b4c-714">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="37b4c-715">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="37b4c-715">In the following example:</span></span>

* <span data-ttu-id="37b4c-716"><xref:System.Net.Http.IHttpClientFactory> está registrado en el contenedor de servicios del [host genérico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="37b4c-716"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="37b4c-717">`MyService` crea una instancia de generador de clientes a partir del servicio, que se usa para crear un elemento `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="37b4c-717">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="37b4c-718">`HttpClient` se utiliza para recuperar una página web.</span><span class="sxs-lookup"><span data-stu-id="37b4c-718">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="37b4c-719">`Main` crea un ámbito para ejecutar el método `GetPage` del servicio y escribe los primeros 500 caracteres del contenido de la página web en la consola.</span><span class="sxs-lookup"><span data-stu-id="37b4c-719">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="header-propagation-middleware"></a><span data-ttu-id="37b4c-720">Middleware de propagación de encabezados</span><span class="sxs-lookup"><span data-stu-id="37b4c-720">Header propagation middleware</span></span>

<span data-ttu-id="37b4c-721">La propagación de encabezado es un middleware compatible con la comunidad que se usa para propagar encabezados HTTP de la solicitud entrante a las solicitudes de cliente HTTP salientes.</span><span class="sxs-lookup"><span data-stu-id="37b4c-721">Header propagation is a community supported middleware to propagate HTTP headers from the incoming request to the outgoing HTTP Client requests.</span></span> <span data-ttu-id="37b4c-722">Para utilizar la propagación de encabezados:</span><span class="sxs-lookup"><span data-stu-id="37b4c-722">To use header propagation:</span></span>

* <span data-ttu-id="37b4c-723">Haga referencia al puerto de comunidad admitido del paquete [HeaderPropagation](https://www.nuget.org/packages/HeaderPropagation).</span><span class="sxs-lookup"><span data-stu-id="37b4c-723">Reference the community supported port of the package [HeaderPropagation](https://www.nuget.org/packages/HeaderPropagation).</span></span> <span data-ttu-id="37b4c-724">ASP.NET Core 3.1 y las versiones posteriores admiten [Microsoft.AspNetCore.HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation).</span><span class="sxs-lookup"><span data-stu-id="37b4c-724">ASP.NET Core 3.1 and later supports [Microsoft.AspNetCore.HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation).</span></span>

* <span data-ttu-id="37b4c-725">Configure el middleware y `HttpClient` en `Startup`:</span><span class="sxs-lookup"><span data-stu-id="37b4c-725">Configure the middleware and `HttpClient` in `Startup`:</span></span>

  [!code-csharp[](http-requests/samples/2.x/Startup21.cs?highlight=5-9,25&name=snippet)]

* <span data-ttu-id="37b4c-726">El cliente incluye los encabezados configurados en las solicitudes salientes:</span><span class="sxs-lookup"><span data-stu-id="37b4c-726">The client includes the configured headers on outbound requests:</span></span>

  ```csharp
  var client = clientFactory.CreateClient("MyForwardingClient");
  var response = client.GetAsync(...);
  ```

## <a name="additional-resources"></a><span data-ttu-id="37b4c-727">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="37b4c-727">Additional resources</span></span>

* [<span data-ttu-id="37b4c-728">Uso de HttpClientFactory para implementar solicitudes HTTP resistentes</span><span class="sxs-lookup"><span data-stu-id="37b4c-728">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="37b4c-729">Implementación de reintentos de llamada HTTP con retroceso exponencial con HttpClientFactory y las directivas de Polly</span><span class="sxs-lookup"><span data-stu-id="37b4c-729">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="37b4c-730">Implementación del patrón de interruptor</span><span class="sxs-lookup"><span data-stu-id="37b4c-730">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
