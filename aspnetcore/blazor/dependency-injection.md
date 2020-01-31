---
title: ASP.NET Core Blazor la inserción de dependencias
author: guardrex
description: Vea cómo Blazor aplicaciones pueden insertar servicios en los componentes.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2020
no-loc:
- Blazor
- SignalR
uid: blazor/dependency-injection
ms.openlocfilehash: 859fd484fc00104575f176fa7d3bf752895475a0
ms.sourcegitcommit: c81ef12a1b6e6ac838e5e07042717cf492e6635b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76885494"
---
# <a name="aspnet-core-blazor-dependency-injection"></a><span data-ttu-id="512f6-103">Inyección de dependencia de ASP.NET Core extraordinaria</span><span class="sxs-lookup"><span data-stu-id="512f6-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="512f6-104">Por [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="512f6-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="512f6-105">El increíble es compatible con la [inserción de dependencias (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="512f6-105">Blazor supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="512f6-106">Las aplicaciones pueden usar servicios integrados mediante su inserción en componentes.</span><span class="sxs-lookup"><span data-stu-id="512f6-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="512f6-107">Las aplicaciones también pueden definir y registrar servicios personalizados y hacer que estén disponibles en toda la aplicación a través de DI.</span><span class="sxs-lookup"><span data-stu-id="512f6-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="512f6-108">DI es una técnica para tener acceso a los servicios configurados en una ubicación central.</span><span class="sxs-lookup"><span data-stu-id="512f6-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="512f6-109">Esto puede ser útil en aplicaciones increíbles para:</span><span class="sxs-lookup"><span data-stu-id="512f6-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="512f6-110">Compartir una única instancia de una clase de servicio entre varios componentes, conocido como servicio *Singleton* .</span><span class="sxs-lookup"><span data-stu-id="512f6-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="512f6-111">Desacoplar componentes de clases de servicio concretas mediante el uso de abstracciones de referencia.</span><span class="sxs-lookup"><span data-stu-id="512f6-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="512f6-112">Por ejemplo, considere una interfaz `IDataAccess` para tener acceso a los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="512f6-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="512f6-113">La interfaz se implementa mediante una clase de `DataAccess` concreta y se registra como un servicio en el contenedor de servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="512f6-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="512f6-114">Cuando un componente usa DI para recibir una `IDataAccess` implementación, el componente no se acopla al tipo concreto.</span><span class="sxs-lookup"><span data-stu-id="512f6-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="512f6-115">La implementación se puede intercambiar, quizás para una implementación ficticia en pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="512f6-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="512f6-116">Servicios predeterminados</span><span class="sxs-lookup"><span data-stu-id="512f6-116">Default services</span></span>

<span data-ttu-id="512f6-117">Los servicios predeterminados se agregan automáticamente a la colección de servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="512f6-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="512f6-118">Servicio</span><span class="sxs-lookup"><span data-stu-id="512f6-118">Service</span></span> | <span data-ttu-id="512f6-119">Período de duración</span><span class="sxs-lookup"><span data-stu-id="512f6-119">Lifetime</span></span> | <span data-ttu-id="512f6-120">Descripción</span><span class="sxs-lookup"><span data-stu-id="512f6-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="512f6-121">Singleton</span><span class="sxs-lookup"><span data-stu-id="512f6-121">Singleton</span></span> | <span data-ttu-id="512f6-122">Proporciona métodos para enviar solicitudes HTTP y recibir respuestas HTTP de un recurso identificado por un URI.</span><span class="sxs-lookup"><span data-stu-id="512f6-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span><br><br><span data-ttu-id="512f6-123">La instancia de `HttpClient` en una aplicación de webassembly increíblemente ligera utiliza el explorador para administrar el tráfico HTTP en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="512f6-123">The instance of `HttpClient` in a Blazor WebAssembly app uses the browser for handling the HTTP traffic in the background.</span></span><br><br><span data-ttu-id="512f6-124">Las aplicaciones de servidor increíbles no incluyen un `HttpClient` configurado como servicio de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="512f6-124">Blazor Server apps don't include an `HttpClient` configured as a service by default.</span></span> <span data-ttu-id="512f6-125">Proporcione una `HttpClient` a una aplicación de servidor más brillante.</span><span class="sxs-lookup"><span data-stu-id="512f6-125">Provide an `HttpClient` to a Blazor Server app.</span></span><br><br><span data-ttu-id="512f6-126">Para obtener más información, vea <xref:blazor/call-web-api>.</span><span class="sxs-lookup"><span data-stu-id="512f6-126">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="512f6-127">Singleton (webassembly increíblemente)</span><span class="sxs-lookup"><span data-stu-id="512f6-127">Singleton (Blazor WebAssembly)</span></span><br><span data-ttu-id="512f6-128">Con ámbito (servidor increíble)</span><span class="sxs-lookup"><span data-stu-id="512f6-128">Scoped (Blazor Server)</span></span> | <span data-ttu-id="512f6-129">Representa una instancia de un Runtime de JavaScript en la que se envían las llamadas de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="512f6-129">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="512f6-130">Para obtener más información, vea <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="512f6-130">For more information, see <xref:blazor/javascript-interop>.</span></span> |
| `NavigationManager` | <span data-ttu-id="512f6-131">Singleton (webassembly increíblemente)</span><span class="sxs-lookup"><span data-stu-id="512f6-131">Singleton (Blazor WebAssembly)</span></span><br><span data-ttu-id="512f6-132">Con ámbito (servidor increíble)</span><span class="sxs-lookup"><span data-stu-id="512f6-132">Scoped (Blazor Server)</span></span> | <span data-ttu-id="512f6-133">Contiene aplicaciones auxiliares para trabajar con URI y el estado de navegación.</span><span class="sxs-lookup"><span data-stu-id="512f6-133">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="512f6-134">Para obtener más información, vea [aplicaciones auxiliares de URI y de estado de navegación](xref:blazor/routing#uri-and-navigation-state-helpers).</span><span class="sxs-lookup"><span data-stu-id="512f6-134">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="512f6-135">Un proveedor de servicios personalizado no proporciona automáticamente los servicios predeterminados que aparecen en la tabla.</span><span class="sxs-lookup"><span data-stu-id="512f6-135">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="512f6-136">Si utiliza un proveedor de servicios personalizado y requiere cualquiera de los servicios que se muestran en la tabla, agregue los servicios necesarios al nuevo proveedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="512f6-136">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="512f6-137">Agregar servicios a una aplicación</span><span class="sxs-lookup"><span data-stu-id="512f6-137">Add services to an app</span></span>

### <a name="blazor-webassembly"></a><span data-ttu-id="512f6-138">WebAssembly de Blazor</span><span class="sxs-lookup"><span data-stu-id="512f6-138">Blazor WebAssembly</span></span>

<span data-ttu-id="512f6-139">Configure los servicios de la colección de servicios de la aplicación en el método `Main` de *Program.CS*.</span><span class="sxs-lookup"><span data-stu-id="512f6-139">Configure services for the app's service collection in the `Main` method of *Program.cs*.</span></span> <span data-ttu-id="512f6-140">En el ejemplo siguiente, la implementación de `MyDependency` se registra para `IMyDependency`:</span><span class="sxs-lookup"><span data-stu-id="512f6-140">In the following example, the `MyDependency` implementation is registered for `IMyDependency`:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<IMyDependency, MyDependency>();
        builder.RootComponents.Add<App>("app");

        await builder.Build().RunAsync();
    }
}
```

<span data-ttu-id="512f6-141">Una vez compilado el host, se puede tener acceso a los servicios desde el ámbito de DI raíz antes de que se representen los componentes.</span><span class="sxs-lookup"><span data-stu-id="512f6-141">Once the host is built, services can be accessed from the root DI scope before any components are rendered.</span></span> <span data-ttu-id="512f6-142">Esto puede ser útil para ejecutar la lógica de inicialización antes de representar el contenido:</span><span class="sxs-lookup"><span data-stu-id="512f6-142">This can be useful for running initialization logic before rendering content:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<WeatherService>();
        builder.RootComponents.Add<App>("app");

        var host = builder.Build();

        var weatherService = host.Services.GetRequiredService<WeatherService>();
        await weatherService.InitializeWeatherAsync();

        await host.RunAsync();
    }
}
```

<span data-ttu-id="512f6-143">El host también proporciona una instancia de configuración central para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="512f6-143">The host also provides a central configuration instance for the app.</span></span> <span data-ttu-id="512f6-144">Basándose en el ejemplo anterior, la dirección URL del servicio meteorológico se pasa de un origen de configuración predeterminado (por ejemplo, *appSettings. JSON*) a `InitializeWeatherAsync`:</span><span class="sxs-lookup"><span data-stu-id="512f6-144">Building on the preceding example, the weather service's URL is passed from a default configuration source (for example, *appsettings.json*) to `InitializeWeatherAsync`:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<WeatherService>();
        builder.RootComponents.Add<App>("app");

        var host = builder.Build();

        var weatherService = host.Services.GetRequiredService<WeatherService>();
        await weatherService.InitializeWeatherAsync(
            host.Configuration["WeatherServiceUrl"]);

        await host.RunAsync();
    }
}
```

### <a name="blazor-server"></a><span data-ttu-id="512f6-145">Servidor Blazor</span><span class="sxs-lookup"><span data-stu-id="512f6-145">Blazor Server</span></span>

<span data-ttu-id="512f6-146">Después de crear una nueva aplicación, examine el método `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="512f6-146">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="512f6-147">Al método `ConfigureServices` se le pasa una <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, que es una lista de objetos de descriptor de servicio (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span><span class="sxs-lookup"><span data-stu-id="512f6-147">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="512f6-148">Para agregar los servicios, se proporcionan descriptores de servicio a la colección de servicios.</span><span class="sxs-lookup"><span data-stu-id="512f6-148">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="512f6-149">En el ejemplo siguiente se muestra el concepto con la interfaz `IDataAccess` y su `DataAccess`de implementación concreta:</span><span class="sxs-lookup"><span data-stu-id="512f6-149">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

### <a name="service-lifetime"></a><span data-ttu-id="512f6-150">Duración del servicio</span><span class="sxs-lookup"><span data-stu-id="512f6-150">Service lifetime</span></span>

<span data-ttu-id="512f6-151">Los servicios se pueden configurar con las duraciones que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="512f6-151">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="512f6-152">Período de duración</span><span class="sxs-lookup"><span data-stu-id="512f6-152">Lifetime</span></span> | <span data-ttu-id="512f6-153">Descripción</span><span class="sxs-lookup"><span data-stu-id="512f6-153">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor<span data-ttu-id="512f6-154"> aplicaciones webassembly no tienen actualmente un concepto de ámbitos de DI.</span><span class="sxs-lookup"><span data-stu-id="512f6-154"> WebAssembly apps don't currently have a concept of DI scopes.</span></span> <span data-ttu-id="512f6-155">los servicios registrados `Scoped`se comportan como `Singleton` Services.</span><span class="sxs-lookup"><span data-stu-id="512f6-155">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="512f6-156">Sin embargo, el modelo de hospedaje del servidor de Blazor admite la duración del `Scoped`.</span><span class="sxs-lookup"><span data-stu-id="512f6-156">However, the Blazor Server hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="512f6-157">En Blazor aplicaciones de servidor, el ámbito de un registro de servicio de ámbito es la *conexión*.</span><span class="sxs-lookup"><span data-stu-id="512f6-157">In Blazor Server apps, a scoped service registration is scoped to the *connection*.</span></span> <span data-ttu-id="512f6-158">Por esta razón, se prefiere el uso de servicios con ámbito para los servicios que deben tener el ámbito del usuario actual, aunque la intención actual sea ejecutar el lado cliente en el explorador.</span><span class="sxs-lookup"><span data-stu-id="512f6-158">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="512f6-159">DI crea una *única instancia* del servicio.</span><span class="sxs-lookup"><span data-stu-id="512f6-159">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="512f6-160">Todos los componentes que requieren un servicio `Singleton` reciben una instancia del mismo servicio.</span><span class="sxs-lookup"><span data-stu-id="512f6-160">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="512f6-161">Cada vez que un componente obtiene una instancia de un servicio `Transient` del contenedor de servicios, recibe una *nueva instancia* del servicio.</span><span class="sxs-lookup"><span data-stu-id="512f6-161">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="512f6-162">El sistema DI se basa en el sistema DI en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="512f6-162">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="512f6-163">Para obtener más información, vea <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="512f6-163">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="512f6-164">Solicitar un servicio en un componente</span><span class="sxs-lookup"><span data-stu-id="512f6-164">Request a service in a component</span></span>

<span data-ttu-id="512f6-165">Una vez agregados los servicios a la colección de servicios, inserte los servicios en los componentes mediante el\@Directiva Razor de [inserción](xref:mvc/views/razor#inject) .</span><span class="sxs-lookup"><span data-stu-id="512f6-165">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="512f6-166">`@inject` tiene dos parámetros:</span><span class="sxs-lookup"><span data-stu-id="512f6-166">`@inject` has two parameters:</span></span>

* <span data-ttu-id="512f6-167">Escriba &ndash; tipo de servicio que se va a insertar.</span><span class="sxs-lookup"><span data-stu-id="512f6-167">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="512f6-168">Propiedad &ndash; el nombre de la propiedad que recibe la aplicación insertada.</span><span class="sxs-lookup"><span data-stu-id="512f6-168">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="512f6-169">La propiedad no requiere la creación manual.</span><span class="sxs-lookup"><span data-stu-id="512f6-169">The property doesn't require manual creation.</span></span> <span data-ttu-id="512f6-170">El compilador crea la propiedad.</span><span class="sxs-lookup"><span data-stu-id="512f6-170">The compiler creates the property.</span></span>

<span data-ttu-id="512f6-171">Para obtener más información, vea <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="512f6-171">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="512f6-172">Use varias instrucciones `@inject` para insertar distintos servicios.</span><span class="sxs-lookup"><span data-stu-id="512f6-172">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="512f6-173">En el ejemplo siguiente se muestra cómo utilizar `@inject`.</span><span class="sxs-lookup"><span data-stu-id="512f6-173">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="512f6-174">El servicio que implementa `Services.IDataAccess` se inserta en el `DataRepository`de propiedades del componente.</span><span class="sxs-lookup"><span data-stu-id="512f6-174">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="512f6-175">Observe cómo el código solo usa la abstracción `IDataAccess`:</span><span class="sxs-lookup"><span data-stu-id="512f6-175">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-razor[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="512f6-176">Internamente, la propiedad generada (`DataRepository`) utiliza el atributo `InjectAttribute`.</span><span class="sxs-lookup"><span data-stu-id="512f6-176">Internally, the generated property (`DataRepository`) uses the `InjectAttribute` attribute.</span></span> <span data-ttu-id="512f6-177">Normalmente, este atributo no se usa directamente.</span><span class="sxs-lookup"><span data-stu-id="512f6-177">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="512f6-178">Si se requiere una clase base para los componentes y las propiedades insertadas también son necesarias para la clase base, agregue manualmente el `InjectAttribute`:</span><span class="sxs-lookup"><span data-stu-id="512f6-178">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="512f6-179">En los componentes derivados de la clase base, no se requiere la Directiva `@inject`.</span><span class="sxs-lookup"><span data-stu-id="512f6-179">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="512f6-180">La `InjectAttribute` de la clase base es suficiente:</span><span class="sxs-lookup"><span data-stu-id="512f6-180">The `InjectAttribute` of the base class is sufficient:</span></span>

```razor
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="512f6-181">Usar DI en servicios</span><span class="sxs-lookup"><span data-stu-id="512f6-181">Use DI in services</span></span>

<span data-ttu-id="512f6-182">Los servicios complejos pueden requerir servicios adicionales.</span><span class="sxs-lookup"><span data-stu-id="512f6-182">Complex services might require additional services.</span></span> <span data-ttu-id="512f6-183">En el ejemplo anterior, `DataAccess` podría requerir el `HttpClient` servicio predeterminado.</span><span class="sxs-lookup"><span data-stu-id="512f6-183">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="512f6-184">`@inject` (o el `InjectAttribute`) no están disponibles para su uso en los servicios de.</span><span class="sxs-lookup"><span data-stu-id="512f6-184">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="512f6-185">En su lugar, se debe usar la *inserción de constructores* .</span><span class="sxs-lookup"><span data-stu-id="512f6-185">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="512f6-186">Los servicios necesarios se agregan agregando parámetros al constructor del servicio.</span><span class="sxs-lookup"><span data-stu-id="512f6-186">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="512f6-187">Cuando DI crea el servicio, reconoce los servicios que requiere en el constructor y los proporciona en consecuencia.</span><span class="sxs-lookup"><span data-stu-id="512f6-187">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

```csharp
public class DataAccess : IDataAccess
{
    // The constructor receives an HttpClient via dependency
    // injection. HttpClient is a default service.
    public DataAccess(HttpClient client)
    {
        ...
    }
}
```

<span data-ttu-id="512f6-188">Requisitos previos para la inserción de constructores:</span><span class="sxs-lookup"><span data-stu-id="512f6-188">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="512f6-189">Debe existir un constructor cuyos argumentos se puedan cumplir con DI.</span><span class="sxs-lookup"><span data-stu-id="512f6-189">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="512f6-190">Los parámetros adicionales que no están incluidos en DI se permiten si especifican valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="512f6-190">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="512f6-191">El constructor aplicable debe ser *público*.</span><span class="sxs-lookup"><span data-stu-id="512f6-191">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="512f6-192">Debe existir un constructor aplicable.</span><span class="sxs-lookup"><span data-stu-id="512f6-192">One applicable constructor must exist.</span></span> <span data-ttu-id="512f6-193">En caso de ambigüedad, DI produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="512f6-193">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a><span data-ttu-id="512f6-194">Clases de componentes base de la utilidad para administrar un ámbito de DI</span><span class="sxs-lookup"><span data-stu-id="512f6-194">Utility base component classes to manage a DI scope</span></span>

<span data-ttu-id="512f6-195">En ASP.NET Core aplicaciones, el ámbito de los servicios de ámbito suele ser la solicitud actual.</span><span class="sxs-lookup"><span data-stu-id="512f6-195">In ASP.NET Core apps, scoped services are typically scoped to the current request.</span></span> <span data-ttu-id="512f6-196">Una vez completada la solicitud, el sistema DI elimina todos los servicios de ámbito o transitorios.</span><span class="sxs-lookup"><span data-stu-id="512f6-196">After the request completes, any scoped or transient services are disposed by the DI system.</span></span> <span data-ttu-id="512f6-197">En Blazor las aplicaciones de servidor, el ámbito de la solicitud dura la duración de la conexión de cliente, lo que puede dar lugar a que los servicios transitorios y de ámbito duren mucho más tiempo del esperado.</span><span class="sxs-lookup"><span data-stu-id="512f6-197">In Blazor Server apps, the request scope lasts for the duration of the client connection, which can result in transient and scoped services living much longer than expected.</span></span>

<span data-ttu-id="512f6-198">Para limitar los servicios a la duración de un componente, puede usar las clases base `OwningComponentBase` y `OwningComponentBase<TService>`.</span><span class="sxs-lookup"><span data-stu-id="512f6-198">To scope services to the lifetime of a component, you can use the `OwningComponentBase` and `OwningComponentBase<TService>` base classes.</span></span> <span data-ttu-id="512f6-199">Estas clases base exponen una propiedad `ScopedServices` de tipo `IServiceProvider` que resuelven los servicios cuyo ámbito es la duración del componente.</span><span class="sxs-lookup"><span data-stu-id="512f6-199">These base classes expose a `ScopedServices` property of type `IServiceProvider` that resolve services that are scoped to the lifetime of the component.</span></span> <span data-ttu-id="512f6-200">Para crear un componente que herede de una clase base en Razor, use la Directiva `@inherits`.</span><span class="sxs-lookup"><span data-stu-id="512f6-200">To author a component that inherits from a base class in Razor, use the `@inherits` directive.</span></span>

```razor
@page "/users"
@attribute [Authorize]
@inherits OwningComponentBase<Data.ApplicationDbContext>

<h1>Users (@Service.Users.Count())</h1>
<ul>
    @foreach (var user in Service.Users)
    {
        <li>@user.UserName</li>
    }
</ul>
```

> [!NOTE]
> <span data-ttu-id="512f6-201">Los servicios insertados en el componente mediante `@inject` o el `InjectAttribute` no se crean en el ámbito del componente y están vinculados al ámbito de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="512f6-201">Services injected into the component using `@inject` or the `InjectAttribute` aren't created in the component's scope and are tied to the request scope.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="512f6-202">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="512f6-202">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
