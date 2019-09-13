---
title: Inyección de dependencia de ASP.NET Core extraordinaria
author: guardrex
description: Vea cómo las aplicaciones más extraordinarias pueden insertar servicios en los componentes.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: blazor/dependency-injection
ms.openlocfilehash: 6c01fdc390cc9150cf81673c717b73c4b10c31f1
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2019
ms.locfileid: "70963979"
---
# <a name="aspnet-core-blazor-dependency-injection"></a><span data-ttu-id="33955-103">Inyección de dependencia de ASP.NET Core extraordinaria</span><span class="sxs-lookup"><span data-stu-id="33955-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="33955-104">Por [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="33955-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="33955-105">El increíble es compatible con la [inserción de dependencias (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="33955-105">Blazor supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="33955-106">Las aplicaciones pueden usar servicios integrados mediante su inserción en componentes.</span><span class="sxs-lookup"><span data-stu-id="33955-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="33955-107">Las aplicaciones también pueden definir y registrar servicios personalizados y hacer que estén disponibles en toda la aplicación a través de DI.</span><span class="sxs-lookup"><span data-stu-id="33955-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="33955-108">DI es una técnica para tener acceso a los servicios configurados en una ubicación central.</span><span class="sxs-lookup"><span data-stu-id="33955-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="33955-109">Esto puede ser útil en aplicaciones increíbles para:</span><span class="sxs-lookup"><span data-stu-id="33955-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="33955-110">Compartir una única instancia de una clase de servicio entre varios componentes, conocido como servicio *Singleton* .</span><span class="sxs-lookup"><span data-stu-id="33955-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="33955-111">Desacoplar componentes de clases de servicio concretas mediante el uso de abstracciones de referencia.</span><span class="sxs-lookup"><span data-stu-id="33955-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="33955-112">Por ejemplo, considere una interfaz `IDataAccess` para tener acceso a los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="33955-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="33955-113">La interfaz se implementa mediante una clase `DataAccess` concreta y se registra como un servicio en el contenedor de servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="33955-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="33955-114">Cuando un componente usa di para recibir una `IDataAccess` implementación, el componente no se acopla al tipo concreto.</span><span class="sxs-lookup"><span data-stu-id="33955-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="33955-115">La implementación se puede intercambiar, quizás para una implementación ficticia en pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="33955-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="33955-116">Servicios predeterminados</span><span class="sxs-lookup"><span data-stu-id="33955-116">Default services</span></span>

<span data-ttu-id="33955-117">Los servicios predeterminados se agregan automáticamente a la colección de servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="33955-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="33955-118">Servicio</span><span class="sxs-lookup"><span data-stu-id="33955-118">Service</span></span> | <span data-ttu-id="33955-119">Período de duración</span><span class="sxs-lookup"><span data-stu-id="33955-119">Lifetime</span></span> | <span data-ttu-id="33955-120">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="33955-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="33955-121">Singleton</span><span class="sxs-lookup"><span data-stu-id="33955-121">Singleton</span></span> | <span data-ttu-id="33955-122">Proporciona métodos para enviar solicitudes HTTP y recibir respuestas HTTP de un recurso identificado por un URI.</span><span class="sxs-lookup"><span data-stu-id="33955-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span> <span data-ttu-id="33955-123">Tenga en cuenta que esta `HttpClient` instancia de usa el explorador para controlar el tráfico http en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="33955-123">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="33955-124">[HttpClient. BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) se establece automáticamente en el prefijo de URI base de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="33955-124">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="33955-125">Para obtener más información, consulta <xref:blazor/call-web-api>.</span><span class="sxs-lookup"><span data-stu-id="33955-125">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="33955-126">Singleton</span><span class="sxs-lookup"><span data-stu-id="33955-126">Singleton</span></span> | <span data-ttu-id="33955-127">Representa una instancia de un Runtime de JavaScript en la que se envían las llamadas de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="33955-127">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="33955-128">Para obtener más información, consulta <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="33955-128">For more information, see <xref:blazor/javascript-interop>.</span></span> |
| `NavigationManager` | <span data-ttu-id="33955-129">Singleton</span><span class="sxs-lookup"><span data-stu-id="33955-129">Singleton</span></span> | <span data-ttu-id="33955-130">Contiene aplicaciones auxiliares para trabajar con URI y el estado de navegación.</span><span class="sxs-lookup"><span data-stu-id="33955-130">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="33955-131">Para obtener más información, vea [aplicaciones auxiliares de URI y de estado de navegación](xref:blazor/routing#uri-and-navigation-state-helpers).</span><span class="sxs-lookup"><span data-stu-id="33955-131">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="33955-132">Un proveedor de servicios personalizado no proporciona automáticamente los servicios predeterminados que aparecen en la tabla.</span><span class="sxs-lookup"><span data-stu-id="33955-132">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="33955-133">Si utiliza un proveedor de servicios personalizado y requiere cualquiera de los servicios que se muestran en la tabla, agregue los servicios necesarios al nuevo proveedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="33955-133">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="33955-134">Agregar servicios a una aplicación</span><span class="sxs-lookup"><span data-stu-id="33955-134">Add services to an app</span></span>

<span data-ttu-id="33955-135">Después de crear una nueva aplicación, examine `Startup.ConfigureServices` el método:</span><span class="sxs-lookup"><span data-stu-id="33955-135">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="33955-136">Al `ConfigureServices` método se le pasa <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>una, que es una lista de objetos de descriptor de servicio (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span><span class="sxs-lookup"><span data-stu-id="33955-136">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="33955-137">Para agregar los servicios, se proporcionan descriptores de servicio a la colección de servicios.</span><span class="sxs-lookup"><span data-stu-id="33955-137">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="33955-138">En el ejemplo siguiente se muestra el concepto `IDataAccess` con la interfaz y su `DataAccess`implementación concreta:</span><span class="sxs-lookup"><span data-stu-id="33955-138">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="33955-139">Los servicios se pueden configurar con las duraciones que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="33955-139">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="33955-140">Período de duración</span><span class="sxs-lookup"><span data-stu-id="33955-140">Lifetime</span></span> | <span data-ttu-id="33955-141">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="33955-141">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | <span data-ttu-id="33955-142">Las aplicaciones de webassembly increíbles no tienen actualmente un concepto de ámbito de DI.</span><span class="sxs-lookup"><span data-stu-id="33955-142">Blazor WebAssembly apps don't currently have a concept of DI scopes.</span></span> <span data-ttu-id="33955-143">`Scoped`: los servicios registrados se `Singleton` comportan como servicios.</span><span class="sxs-lookup"><span data-stu-id="33955-143">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="33955-144">Sin embargo, el modelo de hospedaje del servidor más `Scoped` rápido admite la duración.</span><span class="sxs-lookup"><span data-stu-id="33955-144">However, the Blazor Server hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="33955-145">En las aplicaciones de servidor increíbles, el ámbito de un registro de servicio de ámbito es la *conexión*.</span><span class="sxs-lookup"><span data-stu-id="33955-145">In Blazor Server apps, a scoped service registration is scoped to the *connection*.</span></span> <span data-ttu-id="33955-146">Por esta razón, se prefiere el uso de servicios con ámbito para los servicios que deben tener el ámbito del usuario actual, aunque la intención actual sea ejecutar el lado cliente en el explorador.</span><span class="sxs-lookup"><span data-stu-id="33955-146">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="33955-147">DI crea una *única instancia* del servicio.</span><span class="sxs-lookup"><span data-stu-id="33955-147">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="33955-148">Todos los componentes que requieren `Singleton` un servicio reciben una instancia del mismo servicio.</span><span class="sxs-lookup"><span data-stu-id="33955-148">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="33955-149">Cada vez que un componente obtiene una instancia de `Transient` un servicio del contenedor de servicios, recibe una *nueva instancia* del servicio.</span><span class="sxs-lookup"><span data-stu-id="33955-149">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="33955-150">El sistema DI se basa en el sistema DI en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="33955-150">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="33955-151">Para obtener más información, consulta <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="33955-151">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="33955-152">Solicitar un servicio en un componente</span><span class="sxs-lookup"><span data-stu-id="33955-152">Request a service in a component</span></span>

<span data-ttu-id="33955-153">Una vez agregados los servicios a la colección de servicios, inserte los servicios en los componentes mediante la Directiva de [ \@inserción](xref:mvc/views/razor#inject) de Razor.</span><span class="sxs-lookup"><span data-stu-id="33955-153">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="33955-154">`@inject`tiene dos parámetros:</span><span class="sxs-lookup"><span data-stu-id="33955-154">`@inject` has two parameters:</span></span>

* <span data-ttu-id="33955-155">Escriba &ndash; el tipo de servicio que se va a insertar.</span><span class="sxs-lookup"><span data-stu-id="33955-155">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="33955-156">Propiedad &ndash; nombre de la propiedad que recibe la aplicación insertada.</span><span class="sxs-lookup"><span data-stu-id="33955-156">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="33955-157">La propiedad no requiere la creación manual.</span><span class="sxs-lookup"><span data-stu-id="33955-157">The property doesn't require manual creation.</span></span> <span data-ttu-id="33955-158">El compilador crea la propiedad.</span><span class="sxs-lookup"><span data-stu-id="33955-158">The compiler creates the property.</span></span>

<span data-ttu-id="33955-159">Para obtener más información, consulta <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="33955-159">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="33955-160">Use varias `@inject` instrucciones para insertar distintos servicios.</span><span class="sxs-lookup"><span data-stu-id="33955-160">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="33955-161">En el ejemplo siguiente se muestra cómo utilizar `@inject`.</span><span class="sxs-lookup"><span data-stu-id="33955-161">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="33955-162">El servicio que `Services.IDataAccess` implementa se inserta en la propiedad `DataRepository`del componente.</span><span class="sxs-lookup"><span data-stu-id="33955-162">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="33955-163">Observe cómo el código solo usa la `IDataAccess` abstracción:</span><span class="sxs-lookup"><span data-stu-id="33955-163">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="33955-164">Internamente, la propiedad generada`DataRepository`() se decora con `InjectAttribute` el atributo.</span><span class="sxs-lookup"><span data-stu-id="33955-164">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="33955-165">Normalmente, este atributo no se usa directamente.</span><span class="sxs-lookup"><span data-stu-id="33955-165">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="33955-166">Si se requiere una clase base para los componentes y las propiedades insertadas también son necesarias para la clase base, agregue `InjectAttribute`manualmente el:</span><span class="sxs-lookup"><span data-stu-id="33955-166">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="33955-167">En los componentes derivados de la clase base, `@inject` no se requiere la Directiva.</span><span class="sxs-lookup"><span data-stu-id="33955-167">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="33955-168">El `InjectAttribute` de la clase base es suficiente:</span><span class="sxs-lookup"><span data-stu-id="33955-168">The `InjectAttribute` of the base class is sufficient:</span></span>

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="33955-169">Usar DI en servicios</span><span class="sxs-lookup"><span data-stu-id="33955-169">Use DI in services</span></span>

<span data-ttu-id="33955-170">Los servicios complejos pueden requerir servicios adicionales.</span><span class="sxs-lookup"><span data-stu-id="33955-170">Complex services might require additional services.</span></span> <span data-ttu-id="33955-171">En el ejemplo anterior, `DataAccess` podría requerir el `HttpClient` servicio predeterminado.</span><span class="sxs-lookup"><span data-stu-id="33955-171">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="33955-172">`@inject`(o) `InjectAttribute`no está disponible para su uso en los servicios de.</span><span class="sxs-lookup"><span data-stu-id="33955-172">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="33955-173">En su lugar, se debe usar la *inserción de constructores* .</span><span class="sxs-lookup"><span data-stu-id="33955-173">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="33955-174">Los servicios necesarios se agregan agregando parámetros al constructor del servicio.</span><span class="sxs-lookup"><span data-stu-id="33955-174">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="33955-175">Cuando DI crea el servicio, reconoce los servicios que requiere en el constructor y los proporciona en consecuencia.</span><span class="sxs-lookup"><span data-stu-id="33955-175">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="33955-176">Requisitos previos para la inserción de constructores:</span><span class="sxs-lookup"><span data-stu-id="33955-176">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="33955-177">Debe existir un constructor cuyos argumentos se puedan cumplir con DI.</span><span class="sxs-lookup"><span data-stu-id="33955-177">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="33955-178">Los parámetros adicionales que no están incluidos en DI se permiten si especifican valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="33955-178">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="33955-179">El constructor aplicable debe ser *público*.</span><span class="sxs-lookup"><span data-stu-id="33955-179">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="33955-180">Debe existir un constructor aplicable.</span><span class="sxs-lookup"><span data-stu-id="33955-180">One applicable constructor must exist.</span></span> <span data-ttu-id="33955-181">En caso de ambigüedad, DI produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="33955-181">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a><span data-ttu-id="33955-182">Clases de componentes base de la utilidad para administrar un ámbito de DI</span><span class="sxs-lookup"><span data-stu-id="33955-182">Utility base component classes to manage a DI scope</span></span>

<span data-ttu-id="33955-183">En ASP.NET Core aplicaciones, el ámbito de los servicios de ámbito suele ser la solicitud actual.</span><span class="sxs-lookup"><span data-stu-id="33955-183">In ASP.NET Core apps, scoped services are typically scoped to the current request.</span></span> <span data-ttu-id="33955-184">Una vez completada la solicitud, el sistema DI elimina todos los servicios de ámbito o transitorios.</span><span class="sxs-lookup"><span data-stu-id="33955-184">After the request completes, any scoped or transient services are disposed by the DI system.</span></span> <span data-ttu-id="33955-185">En las aplicaciones de servidor increíbles, el ámbito de la solicitud se mantiene durante la conexión del cliente, lo que puede dar lugar a que los servicios transitorios y de ámbito duren mucho más tiempo del esperado.</span><span class="sxs-lookup"><span data-stu-id="33955-185">In Blazor Server apps, the request scope lasts for the duration of the client connection, which can result in transient and scoped services living much longer than expected.</span></span>

<span data-ttu-id="33955-186">Para limitar los servicios a la duración de un componente, puede usar `OwningComponentBase` las `OwningComponentBase<TService>` clases base y.</span><span class="sxs-lookup"><span data-stu-id="33955-186">To scope services to the lifetime of a component, can use the `OwningComponentBase` and `OwningComponentBase<TService>` base classes.</span></span> <span data-ttu-id="33955-187">Estas clases base exponen una `ScopedServices` propiedad de tipo `IServiceProvider` que resuelve los servicios cuyo ámbito es la duración del componente.</span><span class="sxs-lookup"><span data-stu-id="33955-187">These base classes expose a `ScopedServices` property of type `IServiceProvider` that resolve services that are scoped to the lifetime of the component.</span></span> <span data-ttu-id="33955-188">Para crear un componente que herede de una clase base en Razor, use la `@inherits` Directiva.</span><span class="sxs-lookup"><span data-stu-id="33955-188">To author a component that inherits from a base class in Razor, use the `@inherits` directive.</span></span>

```cshtml
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
> <span data-ttu-id="33955-189">Los servicios insertados en el componente `@inject` con `InjectAttribute` o no se crean en el ámbito del componente y están vinculados al ámbito de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="33955-189">Services injected into the component using `@inject` or the `InjectAttribute` aren't created in the component's scope and are tied to the request scope.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="33955-190">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="33955-190">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
