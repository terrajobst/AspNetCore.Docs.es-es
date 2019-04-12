---
title: Inserción de dependencias de los componentes de Razor
author: guardrex
description: Vea cómo Blazor y componentes de Razor de aplicaciones pueden usar los servicios inyectándolas en los componentes.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
uid: razor-components/dependency-injection
ms.openlocfilehash: 40aec2e3a5032039c7d921f67d7d333b03c07fb1
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2019
ms.locfileid: "59515610"
---
# <a name="razor-components-dependency-injection"></a><span data-ttu-id="582ed-103">Inserción de dependencias de los componentes de Razor</span><span class="sxs-lookup"><span data-stu-id="582ed-103">Razor Components dependency injection</span></span>

<span data-ttu-id="582ed-104">Por [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="582ed-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="582ed-105">Es compatible con los componentes de Razor [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="582ed-105">Razor Components supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="582ed-106">Las aplicaciones pueden usar los servicios integrados insertando en componentes.</span><span class="sxs-lookup"><span data-stu-id="582ed-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="582ed-107">Las aplicaciones también pueden definir y registrar los servicios personalizados y que estén disponibles en toda la aplicación a través de DI.</span><span class="sxs-lookup"><span data-stu-id="582ed-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="582ed-108">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="582ed-108">Dependency injection</span></span>

<span data-ttu-id="582ed-109">Inserción de dependencias es una técnica para tener acceso a los servicios configurados en una ubicación central.</span><span class="sxs-lookup"><span data-stu-id="582ed-109">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="582ed-110">Esto puede ser útil en aplicaciones de componentes de Razor para:</span><span class="sxs-lookup"><span data-stu-id="582ed-110">This can be useful in Razor Components apps to:</span></span>

* <span data-ttu-id="582ed-111">Compartir una única instancia de una clase de servicio a través de muchos componentes, conocidos como un *singleton* service.</span><span class="sxs-lookup"><span data-stu-id="582ed-111">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="582ed-112">Desacoplar componentes de las clases de servicio concreta mediante el uso de las abstracciones de referencia.</span><span class="sxs-lookup"><span data-stu-id="582ed-112">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="582ed-113">Por ejemplo, considere la posibilidad de una interfaz `IDataAccess` para tener acceso a datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="582ed-113">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="582ed-114">La interfaz se implementa mediante un hormigón `DataAccess` clase y registrado como un servicio en contenedor de servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="582ed-114">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="582ed-115">Cuando un componente usa DI para recibir un `IDataAccess` implementación, el componente no es estrictamente con el tipo concreto.</span><span class="sxs-lookup"><span data-stu-id="582ed-115">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="582ed-116">La implementación se puede intercambiar, quizás a una implementación ficticia en pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="582ed-116">The implementation can be swapped, perhaps to a mock implementation in unit tests.</span></span>

<span data-ttu-id="582ed-117">Para obtener más información, consulta <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="582ed-117">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="add-services-to-di"></a><span data-ttu-id="582ed-118">Agregar servicios a la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="582ed-118">Add services to DI</span></span>

<span data-ttu-id="582ed-119">Después de crear una nueva aplicación, examine el `Startup.ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="582ed-119">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="582ed-120">El `ConfigureServices` se pasa al método un <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, que es una lista de objetos de descriptor de servicio (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span><span class="sxs-lookup"><span data-stu-id="582ed-120">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="582ed-121">Los servicios se agregan al proporcionar descriptores del servicio a la colección de servicios.</span><span class="sxs-lookup"><span data-stu-id="582ed-121">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="582ed-122">En el ejemplo siguiente se muestra el concepto con la `IDataAccess` interfaz y su implementación concreta `DataAccess`:</span><span class="sxs-lookup"><span data-stu-id="582ed-122">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="582ed-123">Servicios pueden configurarse con las duraciones que se muestra en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="582ed-123">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="582ed-124">Período de duración</span><span class="sxs-lookup"><span data-stu-id="582ed-124">Lifetime</span></span> | <span data-ttu-id="582ed-125">Descripción</span><span class="sxs-lookup"><span data-stu-id="582ed-125">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="582ed-126">Inserción de dependencias se crea un *única instancia* del servicio.</span><span class="sxs-lookup"><span data-stu-id="582ed-126">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="582ed-127">Todos los componentes que requieren un `Singleton` servicio recibe una instancia del mismo servicio.</span><span class="sxs-lookup"><span data-stu-id="582ed-127">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="582ed-128">Cada vez que un componente obtiene una instancia de un `Transient` servicio desde el contenedor de servicios, recibe un *nueva instancia* del servicio.</span><span class="sxs-lookup"><span data-stu-id="582ed-128">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | <span data-ttu-id="582ed-129">Del lado cliente Blazor actualmente no tiene el concepto de ámbitos de DI.</span><span class="sxs-lookup"><span data-stu-id="582ed-129">Client-side Blazor doesn't currently have the concept of DI scopes.</span></span> <span data-ttu-id="582ed-130">`Scoped` se comporta como `Singleton`.</span><span class="sxs-lookup"><span data-stu-id="582ed-130">`Scoped` behaves like `Singleton`.</span></span> <span data-ttu-id="582ed-131">Sin embargo, los componentes de Razor de ASP.NET Core admiten la `Scoped` duración.</span><span class="sxs-lookup"><span data-stu-id="582ed-131">However, ASP.NET Core Razor Components support the `Scoped` lifetime.</span></span> <span data-ttu-id="582ed-132">En un componente de Razor, un registro de servicio con ámbito se limita a la conexión.</span><span class="sxs-lookup"><span data-stu-id="582ed-132">In a Razor Component, a scoped service registration is scoped to the connection.</span></span> <span data-ttu-id="582ed-133">Por este motivo, utilizando los servicios de ámbito es preferible para los servicios que se deben acotar al usuario actual, incluso si la intención actual es ejecutar el cliente en el explorador.</span><span class="sxs-lookup"><span data-stu-id="582ed-133">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |

<span data-ttu-id="582ed-134">El sistema de DI se basa en el sistema de DI en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="582ed-134">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="582ed-135">Para obtener más información, consulta <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="582ed-135">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="default-services"></a><span data-ttu-id="582ed-136">Servicios predeterminados</span><span class="sxs-lookup"><span data-stu-id="582ed-136">Default services</span></span>

<span data-ttu-id="582ed-137">Servicios predeterminados se agregan automáticamente a la colección de servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="582ed-137">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="582ed-138">web de Office</span><span class="sxs-lookup"><span data-stu-id="582ed-138">Service</span></span> | <span data-ttu-id="582ed-139">Descripción</span><span class="sxs-lookup"><span data-stu-id="582ed-139">Description</span></span> |
| ------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="582ed-140">Proporciona métodos para enviar solicitudes HTTP y recibir respuestas HTTP de un recurso identificado por un URI (singleton).</span><span class="sxs-lookup"><span data-stu-id="582ed-140">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI (singleton).</span></span> <span data-ttu-id="582ed-141">Tenga en cuenta que esta instancia de `HttpClient` utiliza el explorador para controlar el tráfico HTTP en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="582ed-141">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="582ed-142">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) se establece automáticamente en el prefijo URI base de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="582ed-142">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="582ed-143">`HttpClient` solo se proporciona a las aplicaciones de Blazor del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="582ed-143">`HttpClient` is only provided to client-side Blazor apps.</span></span> |
| `IJSRuntime` | <span data-ttu-id="582ed-144">Representa una instancia de un tiempo de ejecución de JavaScript a la que se pueden enviar las llamadas.</span><span class="sxs-lookup"><span data-stu-id="582ed-144">Represents an instance of a JavaScript runtime to which calls may be dispatched.</span></span> <span data-ttu-id="582ed-145">Para obtener más información, consulta <xref:razor-components/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="582ed-145">For more information, see <xref:razor-components/javascript-interop>.</span></span> |
| `IUriHelper` | <span data-ttu-id="582ed-146">Contiene aplicaciones auxiliares para trabajar con el estado de los URI y la navegación (singleton).</span><span class="sxs-lookup"><span data-stu-id="582ed-146">Contains helpers for working with URIs and navigation state (singleton).</span></span> <span data-ttu-id="582ed-147">`IUriHelper` se proporciona a las aplicaciones Blazor y componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="582ed-147">`IUriHelper` is provided to both Blazor and Razor Components apps.</span></span> |

<span data-ttu-id="582ed-148">Es posible utilizar un proveedor de servicio personalizado en lugar del proveedor de servicio predeterminada agregado por la plantilla predeterminada.</span><span class="sxs-lookup"><span data-stu-id="582ed-148">It's possible to use a custom service provider instead of the default service provider added by the default template.</span></span> <span data-ttu-id="582ed-149">Un proveedor de servicios personalizada no proporciona automáticamente los servicios predeterminados que se muestran en la tabla.</span><span class="sxs-lookup"><span data-stu-id="582ed-149">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="582ed-150">Si usa un proveedor de servicios personalizados y requieren cualquiera de los servicios que se muestra en la tabla, agregue los servicios necesarios para el nuevo proveedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="582ed-150">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="582ed-151">Solicitar un servicio en un componente</span><span class="sxs-lookup"><span data-stu-id="582ed-151">Request a service in a component</span></span>

<span data-ttu-id="582ed-152">Después de que los servicios se agregan a la colección de servicios, insertar los servicios en plantillas de Razor de los componentes mediante la [ @inject ](xref:mvc/views/razor#section-4) directiva Razor.</span><span class="sxs-lookup"><span data-stu-id="582ed-152">After services are added to the service collection, inject the services into the components' Razor templates using the [@inject](xref:mvc/views/razor#section-4) Razor directive.</span></span> <span data-ttu-id="582ed-153">`@inject` tiene dos parámetros:</span><span class="sxs-lookup"><span data-stu-id="582ed-153">`@inject` has two parameters:</span></span>

* <span data-ttu-id="582ed-154">Nombre del tipo: El tipo del servicio que se va a insertar.</span><span class="sxs-lookup"><span data-stu-id="582ed-154">Type name: The type of the service to inject.</span></span>
* <span data-ttu-id="582ed-155">Nombre de propiedad: El nombre de la propiedad recibe el servicio de aplicación insertado.</span><span class="sxs-lookup"><span data-stu-id="582ed-155">Property name: The name of the property receiving the injected app service.</span></span> <span data-ttu-id="582ed-156">Tenga en cuenta que la propiedad no requiere la creación manual.</span><span class="sxs-lookup"><span data-stu-id="582ed-156">Note that the property doesn't require manual creation.</span></span> <span data-ttu-id="582ed-157">El compilador crea la propiedad.</span><span class="sxs-lookup"><span data-stu-id="582ed-157">The compiler creates the property.</span></span>

<span data-ttu-id="582ed-158">Para obtener más información, consulta <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="582ed-158">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="582ed-159">Usar varios `@inject` instrucciones para insertar los diferentes servicios.</span><span class="sxs-lookup"><span data-stu-id="582ed-159">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="582ed-160">En el ejemplo siguiente se muestra cómo utilizar `@inject`.</span><span class="sxs-lookup"><span data-stu-id="582ed-160">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="582ed-161">El servicio que implementa `Services.IDataAccess` se inserta en la propiedad del componente `DataRepository`.</span><span class="sxs-lookup"><span data-stu-id="582ed-161">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="582ed-162">Tenga en cuenta cómo el código solo usa el `IDataAccess` abstracción:</span><span class="sxs-lookup"><span data-stu-id="582ed-162">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.cshtml?highlight=2-3,23)]

<span data-ttu-id="582ed-163">Internamente, la propiedad generada (`DataRepository`) está decorada con el `InjectAttribute` atributo.</span><span class="sxs-lookup"><span data-stu-id="582ed-163">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="582ed-164">Normalmente, este atributo no se usa directamente.</span><span class="sxs-lookup"><span data-stu-id="582ed-164">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="582ed-165">Si es necesaria para los componentes de una clase base e insertadas propiedades también son necesarias para la clase base, `InjectAttribute` pueden agregarse manualmente:</span><span class="sxs-lookup"><span data-stu-id="582ed-165">If a base class is required for components and injected properties are also required for the base class, `InjectAttribute` can be manually added:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // Dependency injection works even if using the
    // InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="582ed-166">En los componentes derivados de la clase base, el `@inject` directiva no es necesaria.</span><span class="sxs-lookup"><span data-stu-id="582ed-166">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="582ed-167">El `InjectAttribute` de la clase base es suficiente:</span><span class="sxs-lookup"><span data-stu-id="582ed-167">The `InjectAttribute` of the base class is sufficient:</span></span>

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="dependency-injection-in-services"></a><span data-ttu-id="582ed-168">Inserción de dependencias en servicios</span><span class="sxs-lookup"><span data-stu-id="582ed-168">Dependency injection in services</span></span>

<span data-ttu-id="582ed-169">Servicios complejos pueden requerir servicios adicionales.</span><span class="sxs-lookup"><span data-stu-id="582ed-169">Complex services might require additional services.</span></span> <span data-ttu-id="582ed-170">En el ejemplo anterior, `DataAccess` podría requerir el `HttpClient` servicio predeterminado.</span><span class="sxs-lookup"><span data-stu-id="582ed-170">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="582ed-171">`@inject` (o `InjectAttribute`) no está disponible para su uso en los servicios.</span><span class="sxs-lookup"><span data-stu-id="582ed-171">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="582ed-172">*Inserción de constructor* debe usarse en su lugar.</span><span class="sxs-lookup"><span data-stu-id="582ed-172">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="582ed-173">Los servicios necesarios se agregan mediante la adición de parámetros al constructor del servicio.</span><span class="sxs-lookup"><span data-stu-id="582ed-173">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="582ed-174">Cuando la inserción de dependencias, crea el servicio, reconoce los servicios que requiere en el constructor y las proporciona según corresponda.</span><span class="sxs-lookup"><span data-stu-id="582ed-174">When dependency injection creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="582ed-175">Requisitos previos para la inyección de constructor:</span><span class="sxs-lookup"><span data-stu-id="582ed-175">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="582ed-176">Debe haber un constructor cuyos argumentos pueden cumplir la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="582ed-176">There must be one constructor whose arguments can all be fulfilled by dependency injection.</span></span> <span data-ttu-id="582ed-177">Tenga en cuenta que se permiten parámetros adicionales no cubiertos por la inserción de dependencias si especifican los valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="582ed-177">Note that additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="582ed-178">El constructor correspondiente debe ser *pública*.</span><span class="sxs-lookup"><span data-stu-id="582ed-178">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="582ed-179">Solo debe haber un constructor aplicable.</span><span class="sxs-lookup"><span data-stu-id="582ed-179">There must only be one applicable constructor.</span></span> <span data-ttu-id="582ed-180">En el caso de ambigüedad, inserción de dependencias produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="582ed-180">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="582ed-181">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="582ed-181">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
