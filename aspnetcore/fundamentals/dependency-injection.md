---
title: Inserción de dependencias en ASP.NET Core
author: guardrex
description: Obtenga información sobre la manera en que ASP.NET Core implementa la inserción de dependencias y cómo se usa.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/06/2019
uid: fundamentals/dependency-injection
ms.openlocfilehash: 27ae8ac979c267c666d6d63f4d1dd862ff20edba
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2019
ms.locfileid: "68819860"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="7d1a8-103">Inserción de dependencias en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d1a8-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="7d1a8-104">Por [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7d1a8-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7d1a8-105">ASP.NET Core admite el patrón de diseño de software de inserción de dependencias (DI), que es una técnica para conseguir la [inversión de control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) entre clases y sus dependencias.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="7d1a8-106">Para más información específica sobre la inserción de dependencias en los controladores MVC, vea <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="7d1a8-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7d1a8-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="7d1a8-108">Información general sobre la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="7d1a8-108">Overview of dependency injection</span></span>

<span data-ttu-id="7d1a8-109">Una *dependencia* es cualquier objeto requerido por otro objeto.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="7d1a8-110">Examine la siguiente clase `MyDependency` con un método `WriteMessage` del que dependen otras clases de una aplicación:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

```csharp
public class MyDependency
{
    public MyDependency()
    {
    }

    public Task WriteMessage(string message)
    {
        Console.WriteLine(
            $"MyDependency.WriteMessage called. Message: {message}");

        return Task.FromResult(0);
    }
}
```

<span data-ttu-id="7d1a8-111">Se puede crear una instancia de la clase `MyDependency` para hacer que el método `WriteMessage` esté disponible para una clase.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="7d1a8-112">La clase `MyDependency` es una dependencia de la clase `IndexModel`:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

```csharp
public class IndexModel : PageModel
{
    MyDependency _dependency = new MyDependency();

    public async Task OnGetAsync()
    {
        await _dependency.WriteMessage(
            "IndexModel.OnGetAsync created this message.");
    }
}
```

<span data-ttu-id="7d1a8-113">La clase crea y depende directamente de la instancia `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-113">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="7d1a8-114">Las dependencias de código (como en el ejemplo anterior) son problemáticas y deben evitarse por las siguientes razones:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-114">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="7d1a8-115">Para reemplazar `MyDependency` con una implementación diferente, se debe modificar la clase.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-115">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="7d1a8-116">Si `MyDependency` tiene dependencias, deben configurarse según la clase.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-116">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="7d1a8-117">En un proyecto grande con varias clases que dependen de `MyDependency`, el código de configuración se dispersa por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-117">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="7d1a8-118">Esta implementación es difícil para realizar pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-118">This implementation is difficult to unit test.</span></span> <span data-ttu-id="7d1a8-119">La aplicación debe usar una clase `MyDependency` como boceto o código auxiliar, que no es posible con este enfoque.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-119">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="7d1a8-120">La inserción de dependencias aborda estos problemas mediante:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-120">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="7d1a8-121">Uso de una interfaz o clase base para abstraer la implementación de dependencias.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-121">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="7d1a8-122">Registro de la dependencia en un contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-122">Registration of the dependency in a service container.</span></span> <span data-ttu-id="7d1a8-123">ASP.NET Core proporciona un contenedor de servicios integrado, <xref:System.IServiceProvider>.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-123">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="7d1a8-124">Los servicios se registran en el método `Startup.ConfigureServices` de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-124">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="7d1a8-125">*Inserción* del servicio en el constructor de la clase en la que se usa.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-125">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="7d1a8-126">El marco de trabajo asume la responsabilidad de crear una instancia de la dependencia y de desecharla cuando ya no es necesaria.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-126">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="7d1a8-127">En la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), la interfaz `IMyDependency` define un método que el servicio proporciona a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-127">In the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

<span data-ttu-id="7d1a8-128">Esta interfaz se implementa mediante un tipo concreto, `MyDependency`:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-128">This interface is implemented by a concrete type, `MyDependency`:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

<span data-ttu-id="7d1a8-129">`MyDependency` solicita <xref:Microsoft.Extensions.Logging.ILogger`1> en su constructor.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-129">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="7d1a8-130">No es raro usar la inserción de dependencias de forma encadenada.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-130">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="7d1a8-131">Cada dependencia solicitada a su vez solicita sus propias dependencias.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-131">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="7d1a8-132">El contenedor resuelve las dependencias del gráfico y devuelve el servicio totalmente resuelto.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-132">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="7d1a8-133">El conjunto colectivo de dependencias que deben resolverse suele denominarse *árbol de dependencias*, *gráfico de dependencias* o *gráfico de objetos*.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-133">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="7d1a8-134">`IMyDependency` y `ILogger<TCategoryName>` deben estar registrados en el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-134">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="7d1a8-135">`IMyDependency` está registrado en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-135">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="7d1a8-136">`ILogger<TCategoryName>` está registrado en la infraestructura de abstracciones de registros, por lo que se trata de un [servicio proporcionado por el marco de trabajo](#framework-provided-services) registrado de forma predeterminada por el marco de trabajo.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-136">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="7d1a8-137">El contenedor resuelve `ILogger<TCategoryName>` aprovechando las ventajas de los [tipos abiertos (genéricos)](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), lo que elimina la necesidad de registrar todos los [tipos construidos (genéricos)](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span><span class="sxs-lookup"><span data-stu-id="7d1a8-137">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<T>), typeof(Logger<T>));
```

<span data-ttu-id="7d1a8-138">En la aplicación de ejemplo, el servicio `IMyDependency` está registrado con el tipo concreto `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-138">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="7d1a8-139">El registro abarca la duración del servicio como la duración de una única solicitud.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-139">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="7d1a8-140">Las [duraciones del servicio](#service-lifetimes) se describen más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-140">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> <span data-ttu-id="7d1a8-141">Cada método de extensión `services.Add{SERVICE_NAME}` agrega servicios (y potencialmente los configura).</span><span class="sxs-lookup"><span data-stu-id="7d1a8-141">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="7d1a8-142">Por ejemplo, `services.AddMvc()` agrega los servicios que Razor Pages y MVC requieren.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-142">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="7d1a8-143">Se recomienda que las aplicaciones sigan esta convención.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-143">We recommended that apps follow this convention.</span></span> <span data-ttu-id="7d1a8-144">Coloque los métodos de extensión en el espacio de nombres [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) para encapsular grupos de registros del servicio.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-144">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="7d1a8-145">Si el constructor del servicio requiere un [tipo integrado](/dotnet/csharp/language-reference/keywords/built-in-types-table), como `string`, se puede insertar mediante la [configuración](xref:fundamentals/configuration/index) o el [patrón de opciones](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="7d1a8-145">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

```csharp
public class MyDependency : IMyDependency
{
    public MyDependency(IConfiguration config)
    {
        var myStringValue = config["MyStringKey"];

        // Use myStringValue
    }

    ...
}
```

<span data-ttu-id="7d1a8-146">Se solicita una instancia del servicio mediante el constructor de una clase, en la que se usa el servicio y se asigna a un campo privado.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-146">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="7d1a8-147">El campo de utiliza para acceder al servicio, según sea necesario en la clase.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-147">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="7d1a8-148">En la aplicación de ejemplo, la instancia `IMyDependency` se solicita y usa para llamar al método `WriteMessage` del servicio:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-148">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="framework-provided-services"></a><span data-ttu-id="7d1a8-149">Servicios proporcionados por el marco de trabajo</span><span class="sxs-lookup"><span data-stu-id="7d1a8-149">Framework-provided services</span></span>

<span data-ttu-id="7d1a8-150">El método `Startup.ConfigureServices` se encarga de definir los servicios que la aplicación usa, incluidas las características de plataforma como Entity Framework Core y ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-150">The `Startup.ConfigureServices` method is responsible for defining the services the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="7d1a8-151">Inicialmente, el valor `IServiceCollection` proporcionado a `ConfigureServices` tiene los siguientes servicios definidos (en función de [cómo se configurara el host](xref:fundamentals/index#host)):</span><span class="sxs-lookup"><span data-stu-id="7d1a8-151">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/index#host)):</span></span>

| <span data-ttu-id="7d1a8-152">Tipo de servicio</span><span class="sxs-lookup"><span data-stu-id="7d1a8-152">Service Type</span></span> | <span data-ttu-id="7d1a8-153">Período de duración</span><span class="sxs-lookup"><span data-stu-id="7d1a8-153">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="7d1a8-154">Transitorio</span><span class="sxs-lookup"><span data-stu-id="7d1a8-154">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> | <span data-ttu-id="7d1a8-155">Singleton</span><span class="sxs-lookup"><span data-stu-id="7d1a8-155">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> | <span data-ttu-id="7d1a8-156">Singleton</span><span class="sxs-lookup"><span data-stu-id="7d1a8-156">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="7d1a8-157">Singleton</span><span class="sxs-lookup"><span data-stu-id="7d1a8-157">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="7d1a8-158">Transitorio</span><span class="sxs-lookup"><span data-stu-id="7d1a8-158">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="7d1a8-159">Singleton</span><span class="sxs-lookup"><span data-stu-id="7d1a8-159">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="7d1a8-160">Transitorio</span><span class="sxs-lookup"><span data-stu-id="7d1a8-160">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="7d1a8-161">Singleton</span><span class="sxs-lookup"><span data-stu-id="7d1a8-161">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="7d1a8-162">Singleton</span><span class="sxs-lookup"><span data-stu-id="7d1a8-162">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="7d1a8-163">Singleton</span><span class="sxs-lookup"><span data-stu-id="7d1a8-163">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="7d1a8-164">Transitorio</span><span class="sxs-lookup"><span data-stu-id="7d1a8-164">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="7d1a8-165">Singleton</span><span class="sxs-lookup"><span data-stu-id="7d1a8-165">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="7d1a8-166">Singleton</span><span class="sxs-lookup"><span data-stu-id="7d1a8-166">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="7d1a8-167">Singleton</span><span class="sxs-lookup"><span data-stu-id="7d1a8-167">Singleton</span></span> |

<span data-ttu-id="7d1a8-168">Cuando un método de extensión de la colección de servicio está disponible para registrar un servicio (y sus servicios dependientes, si es necesario), la convención consiste en usar un solo método de extensión `Add{SERVICE_NAME}` para registrar todos los servicios requeridos por dicho servicio.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-168">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="7d1a8-169">El código siguiente es un ejemplo de cómo agregar servicios adicionales al contenedor mediante los métodos de extensión [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*> y <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-169">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>, and <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddMvc();
}
```

<span data-ttu-id="7d1a8-170">Para obtener más información, consulte la clase <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> en la documentación de la API.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-170">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> class in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="7d1a8-171">Duraciones de servicios</span><span class="sxs-lookup"><span data-stu-id="7d1a8-171">Service lifetimes</span></span>

<span data-ttu-id="7d1a8-172">Elija una duración adecuada para cada servicio registrado.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-172">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="7d1a8-173">Los servicios de ASP.NET Core pueden configurarse con las duraciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-173">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="7d1a8-174">Transitorio</span><span class="sxs-lookup"><span data-stu-id="7d1a8-174">Transient</span></span>

<span data-ttu-id="7d1a8-175">Los servicios de duración transitoria (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) se crean cada vez que el contenedor del servicio los solicita.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-175">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="7d1a8-176">Esta duración funciona mejor para servicios sin estado ligeros.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-176">This lifetime works best for lightweight, stateless services.</span></span>

### <a name="scoped"></a><span data-ttu-id="7d1a8-177">Con ámbito</span><span class="sxs-lookup"><span data-stu-id="7d1a8-177">Scoped</span></span>

<span data-ttu-id="7d1a8-178">Los servicios de duración con ámbito (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) se crean una vez por solicitud del cliente (conexión).</span><span class="sxs-lookup"><span data-stu-id="7d1a8-178">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="7d1a8-179">Si usa un servicio con ámbito en un middleware, inserte el servicio en el método `Invoke` o `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-179">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="7d1a8-180">No lo inserte a través de la inserción de constructores, porque ello hace que el servicio se comporte como un singleton.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-180">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="7d1a8-181">Para más información, consulte <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-181">For more information, see <xref:fundamentals/middleware/index>.</span></span>

### <a name="singleton"></a><span data-ttu-id="7d1a8-182">Singleton</span><span class="sxs-lookup"><span data-stu-id="7d1a8-182">Singleton</span></span>

<span data-ttu-id="7d1a8-183">Los servicios con duración Singleton (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) se crean la primera vez que se solicitan, o bien al ejecutar `Startup.ConfigureServices` y especificar una instancia con el registro del servicio.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-183">Singleton lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="7d1a8-184">Cada solicitud posterior usa la misma instancia.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-184">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="7d1a8-185">Si la aplicación requiere un comportamiento de singleton, se recomienda permitir que el contenedor de servicios administre la duración del servicio.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-185">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="7d1a8-186">No implemente el patrón de diseño de singleton y proporcione el código de usuario para administrar la duración del objeto en la clase.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-186">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="7d1a8-187">Es peligroso resolver un servicio con ámbito desde un singleton.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-187">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="7d1a8-188">Puede dar lugar a que el servicio adopte un estado incorrecto al procesar solicitudes posteriores.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-188">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="7d1a8-189">Métodos de registro del servicio</span><span class="sxs-lookup"><span data-stu-id="7d1a8-189">Service registration methods</span></span>

<span data-ttu-id="7d1a8-190">Cada método de extensión de registro del servicio ofrece sobrecargas útiles en escenarios específicos.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-190">Each service registration extension method offers overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="7d1a8-191">Método</span><span class="sxs-lookup"><span data-stu-id="7d1a8-191">Method</span></span> | <span data-ttu-id="7d1a8-192">Automático</span><span class="sxs-lookup"><span data-stu-id="7d1a8-192">Automatic</span></span><br><span data-ttu-id="7d1a8-193">objeto</span><span class="sxs-lookup"><span data-stu-id="7d1a8-193">object</span></span><br><span data-ttu-id="7d1a8-194">eliminación</span><span class="sxs-lookup"><span data-stu-id="7d1a8-194">disposal</span></span> | <span data-ttu-id="7d1a8-195">Múltiple</span><span class="sxs-lookup"><span data-stu-id="7d1a8-195">Multiple</span></span><br><span data-ttu-id="7d1a8-196">implementaciones</span><span class="sxs-lookup"><span data-stu-id="7d1a8-196">implementations</span></span> | <span data-ttu-id="7d1a8-197">Transferencia de argumentos</span><span class="sxs-lookup"><span data-stu-id="7d1a8-197">Pass args</span></span> |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><span data-ttu-id="7d1a8-198">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-198">Example:</span></span><br>`services.AddScoped<IMyDep, MyDep>();` | <span data-ttu-id="7d1a8-199">Sí</span><span class="sxs-lookup"><span data-stu-id="7d1a8-199">Yes</span></span> | <span data-ttu-id="7d1a8-200">Sí</span><span class="sxs-lookup"><span data-stu-id="7d1a8-200">Yes</span></span> | <span data-ttu-id="7d1a8-201">No</span><span class="sxs-lookup"><span data-stu-id="7d1a8-201">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><span data-ttu-id="7d1a8-202">Ejemplos:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-202">Examples:</span></span><br>`services.AddScoped<IMyDep>(sp => new MyDep());`<br>`services.AddScoped<IMyDep>(sp => new MyDep("A string!"));` | <span data-ttu-id="7d1a8-203">Sí</span><span class="sxs-lookup"><span data-stu-id="7d1a8-203">Yes</span></span> | <span data-ttu-id="7d1a8-204">Sí</span><span class="sxs-lookup"><span data-stu-id="7d1a8-204">Yes</span></span> | <span data-ttu-id="7d1a8-205">Sí</span><span class="sxs-lookup"><span data-stu-id="7d1a8-205">Yes</span></span> |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><span data-ttu-id="7d1a8-206">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-206">Example:</span></span><br>`services.AddScoped<MyDep>();` | <span data-ttu-id="7d1a8-207">Sí</span><span class="sxs-lookup"><span data-stu-id="7d1a8-207">Yes</span></span> | <span data-ttu-id="7d1a8-208">No</span><span class="sxs-lookup"><span data-stu-id="7d1a8-208">No</span></span> | <span data-ttu-id="7d1a8-209">No</span><span class="sxs-lookup"><span data-stu-id="7d1a8-209">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(new {IMPLEMENTATION})`<br><span data-ttu-id="7d1a8-210">Ejemplos:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-210">Examples:</span></span><br>`services.AddScoped<IMyDep>(new MyDep());`<br>`services.AddScoped<IMyDep>(new MyDep("A string!"));` | <span data-ttu-id="7d1a8-211">No</span><span class="sxs-lookup"><span data-stu-id="7d1a8-211">No</span></span> | <span data-ttu-id="7d1a8-212">Sí</span><span class="sxs-lookup"><span data-stu-id="7d1a8-212">Yes</span></span> | <span data-ttu-id="7d1a8-213">Sí</span><span class="sxs-lookup"><span data-stu-id="7d1a8-213">Yes</span></span> |
| `Add{LIFETIME}(new {IMPLEMENTATION})`<br><span data-ttu-id="7d1a8-214">Ejemplos:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-214">Examples:</span></span><br>`services.AddScoped(new MyDep());`<br>`services.AddScoped(new MyDep("A string!"));` | <span data-ttu-id="7d1a8-215">No</span><span class="sxs-lookup"><span data-stu-id="7d1a8-215">No</span></span> | <span data-ttu-id="7d1a8-216">No</span><span class="sxs-lookup"><span data-stu-id="7d1a8-216">No</span></span> | <span data-ttu-id="7d1a8-217">Sí</span><span class="sxs-lookup"><span data-stu-id="7d1a8-217">Yes</span></span> |

<span data-ttu-id="7d1a8-218">Para obtener más información sobre el tipo de eliminación, consulte la sección [Eliminación de servicios](#disposal-of-services).</span><span class="sxs-lookup"><span data-stu-id="7d1a8-218">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="7d1a8-219">Un escenario común para varias implementaciones es [utilizar tipos de simulación para las pruebas](xref:test/integration-tests#inject-mock-services).</span><span class="sxs-lookup"><span data-stu-id="7d1a8-219">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="7d1a8-220">Los métodos `TryAdd{LIFETIME}` solo registran el servicio si no hay ya una implementación registrada.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-220">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="7d1a8-221">En el ejemplo siguiente, la primera línea registra `MyDependency` para `IMyDependency`.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-221">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="7d1a8-222">La segunda línea no tiene ningún efecto porque `IMyDependency` ya tiene una implementación registrada:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-222">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="7d1a8-223">Para obtener más información, consulte:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-223">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="7d1a8-224">Los métodos [TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) registran solo el servicio si no hay ya una implementación *del mismo tipo*.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-224">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="7d1a8-225">A través de `IEnumerable<{SERVICE}>` se resuelven varios servicios.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-225">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="7d1a8-226">Al registrar los servicios, el desarrollador solo quiere agregar una instancia si no se ha agregado ya una del mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-226">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="7d1a8-227">Por lo general, este método lo utilizan los creadores de bibliotecas para evitar registrar dos copias de una instancia en el contenedor.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-227">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="7d1a8-228">En el ejemplo siguiente, la primera línea registra `MyDep` para `IMyDep1`.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-228">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="7d1a8-229">La segunda línea registra `MyDep` para `IMyDep2`.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-229">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="7d1a8-230">La tercera línea no tiene ningún efecto porque `IMyDep1` ya tiene una implementación registrada de `MyDep`:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-230">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="7d1a8-231">Comportamiento de inserción de constructor</span><span class="sxs-lookup"><span data-stu-id="7d1a8-231">Constructor injection behavior</span></span>

<span data-ttu-id="7d1a8-232">Los servicios se pueden resolver mediante dos mecanismos:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-232">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="7d1a8-233"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities>: permite la creación de objetos sin registrar el servicio en el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-233"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="7d1a8-234">`ActivatorUtilities` se utiliza con abstracciones orientadas al usuario, como asistentes de etiquetas, controladores MVC y enlazadores de modelos.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-234">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="7d1a8-235">Los constructores pueden aceptar argumentos que no se proporcionan mediante la inserción de dependencias, pero los argumentos deben asignar valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-235">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="7d1a8-236">Cuando se resuelven los servicios mediante `IServiceProvider` o `ActivatorUtilities`, la inserción del constructor requiere un constructor *público*.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-236">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="7d1a8-237">Cuando se resuelven los servicios mediante `ActivatorUtilities`, la inserción del constructor requiere que exista solo un constructor aplicable.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-237">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exists.</span></span> <span data-ttu-id="7d1a8-238">Se admiten las sobrecargas de constructor, pero solo puede existir una sobrecarga cuyos argumentos pueda cumplir la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-238">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="7d1a8-239">Contextos de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="7d1a8-239">Entity Framework contexts</span></span>

<span data-ttu-id="7d1a8-240">Los contextos de Entity Framework normalmente se agregan al contenedor de servicios mediante la [duración con ámbito](#service-lifetimes) porque las operaciones de base de datos de aplicación web se suelen limitar a la solicitud de cliente.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-240">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="7d1a8-241">La duración predeterminada se limita si no se especifica una duración mediante una sobrecarga de [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) al registrar el contexto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-241">The default lifetime is scoped if a lifetime isn't specified by an [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) overload when registering the database context.</span></span> <span data-ttu-id="7d1a8-242">En los servicios de una duración determinada no se debe usar un contexto de base de datos con una duración más corta que el servicio.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-242">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="7d1a8-243">Opciones de registro y duración</span><span class="sxs-lookup"><span data-stu-id="7d1a8-243">Lifetime and registration options</span></span>

<span data-ttu-id="7d1a8-244">Para mostrar la diferencia entre la duración y las opciones de registro, considere las siguientes interfaces que representan tareas como una operación con un identificador único, `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-244">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="7d1a8-245">Según cómo esté configurada la duración de un servicio de operaciones para las interfaces siguientes, el contenedor proporciona la misma instancia del servicio u otra distinta cuando así lo solicita la clase:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-245">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

<span data-ttu-id="7d1a8-246">Las interfaces se implementan en la clase `Operation`.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-246">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="7d1a8-247">El constructor `Operation` genera un GUID en caso de que no se proporcione uno:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-247">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

<span data-ttu-id="7d1a8-248">Se registra una instancia de `OperationService` que depende de cada uno de los demás tipos `Operation`.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-248">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="7d1a8-249">Cuando `OperationService` se solicita con la inserción de dependencias, recibe una instancia nueva de cada servicio o una instancia existente en función de la duración de los servicios dependientes.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-249">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="7d1a8-250">Si se crean servicios transitorios cuando se solicitan al contenedor, el elemento `OperationId` del servicio `IOperationTransient` es diferente del objeto `OperationId` de `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-250">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="7d1a8-251">`OperationService` recibe una instancia nueva de la clase `IOperationTransient`.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-251">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="7d1a8-252">La nueva instancia produce un objeto `OperationId` diferente.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-252">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="7d1a8-253">Si se crean servicios con ámbito por solicitud de cliente, el objeto `OperationId` del servicio `IOperationScoped` es el mismo que para `OperationService` dentro de la solicitud de cliente.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-253">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="7d1a8-254">Entre las solicitudes de cliente, ambos servicios comparten un valor `OperationId` diferente.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-254">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="7d1a8-255">Si se crean servicios singleton y servicios de instancia singleton una vez y se usan en todas las solicitudes de cliente y servicios, el objeto `OperationId` es constante en todas las solicitudes de servicio.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-255">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

<span data-ttu-id="7d1a8-256">En `Startup.ConfigureServices`, cada tipo se agrega al contenedor según su duración con nombre:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-256">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

<span data-ttu-id="7d1a8-257">El servicio `IOperationSingletonInstance` usa una instancia específica con un identificador conocido de `Guid.Empty`.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-257">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="7d1a8-258">Resulta evidente identificar cuándo este tipo está en uso (su GUID es todo ceros).</span><span class="sxs-lookup"><span data-stu-id="7d1a8-258">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="7d1a8-259">La aplicación de ejemplo muestra las duraciones de los objetos dentro y entre las solicitudes individuales.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-259">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="7d1a8-260">El objeto `IndexModel` de la aplicación de ejemplo solicita cada tipo de `IOperation` y `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-260">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="7d1a8-261">Después, la página muestra todos los valores `OperationId` del servicio y la clase del modelo de página mediante las asignaciones de propiedades:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-261">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

<span data-ttu-id="7d1a8-262">Las dos salidas siguientes muestran el resultado de dos solicitudes:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-262">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="7d1a8-263">**Primera solicitud:**</span><span class="sxs-lookup"><span data-stu-id="7d1a8-263">**First request:**</span></span>

<span data-ttu-id="7d1a8-264">Operaciones del controlador:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-264">Controller operations:</span></span>

<span data-ttu-id="7d1a8-265">Transient: d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="7d1a8-265">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="7d1a8-266">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="7d1a8-266">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="7d1a8-267">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="7d1a8-267">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="7d1a8-268">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="7d1a8-268">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="7d1a8-269">Operaciones `OperationService`:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-269">`OperationService` operations:</span></span>

<span data-ttu-id="7d1a8-270">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="7d1a8-270">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="7d1a8-271">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="7d1a8-271">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="7d1a8-272">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="7d1a8-272">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="7d1a8-273">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="7d1a8-273">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="7d1a8-274">**Segunda solicitud:**</span><span class="sxs-lookup"><span data-stu-id="7d1a8-274">**Second request:**</span></span>

<span data-ttu-id="7d1a8-275">Operaciones del controlador:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-275">Controller operations:</span></span>

<span data-ttu-id="7d1a8-276">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="7d1a8-276">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="7d1a8-277">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="7d1a8-277">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="7d1a8-278">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="7d1a8-278">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="7d1a8-279">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="7d1a8-279">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="7d1a8-280">Operaciones `OperationService`:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-280">`OperationService` operations:</span></span>

<span data-ttu-id="7d1a8-281">Transitorio: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="7d1a8-281">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="7d1a8-282">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="7d1a8-282">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="7d1a8-283">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="7d1a8-283">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="7d1a8-284">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="7d1a8-284">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="7d1a8-285">Observe cuál de los valores `OperationId` varía dentro de una solicitud y entre solicitudes:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-285">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="7d1a8-286">Los objetos *Transient* siempre son diferentes.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-286">*Transient* objects are always different.</span></span> <span data-ttu-id="7d1a8-287">El valor `OperationId` transitorio de la primera y la segunda solicitud de cliente varía tanto para las operaciones `OperationService` como entre solicitudes de cliente.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-287">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="7d1a8-288">Se proporciona una nueva instancia para cada solicitud de servicio y solicitud de cliente.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-288">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="7d1a8-289">Los objetos *con ámbito* son iguales dentro de una solicitud de cliente, pero varían entre solicitudes de cliente.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-289">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="7d1a8-290">Los objetos *singleton* son iguales para todos los objetos y solicitudes, independientemente de si se proporciona una instancia `Operation` en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-290">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="7d1a8-291">Llamada a servicios desde main</span><span class="sxs-lookup"><span data-stu-id="7d1a8-291">Call services from main</span></span>

<span data-ttu-id="7d1a8-292">Cree un elemento <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> con [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) para resolver un servicio con ámbito dentro del ámbito de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-292">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="7d1a8-293">Este método resulta útil para tener acceso a un servicio con ámbito durante el inicio para realizar tareas de inicialización.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-293">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="7d1a8-294">En el siguiente ejemplo se indica cómo obtener un contexto para `MyScopedService` en `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-294">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = CreateWebHostBuilder(args).Build();

    using (var serviceScope = host.Services.CreateScope())
    {
        var services = serviceScope.ServiceProvider;

        try
        {
            var serviceContext = services.GetRequiredService<MyScopedService>();
            // Use the context here
        }
        catch (Exception ex)
        {
            var logger = services.GetRequiredService<ILogger<Program>>();
            logger.LogError(ex, "An error occurred.");
        }
    }

    host.Run();
}
```

## <a name="scope-validation"></a><span data-ttu-id="7d1a8-295">Validación del ámbito</span><span class="sxs-lookup"><span data-stu-id="7d1a8-295">Scope validation</span></span>

<span data-ttu-id="7d1a8-296">Cuando la aplicación se ejecuta en el entorno de desarrollo, el proveedor de servicios predeterminado realiza comprobaciones para confirmar lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-296">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="7d1a8-297">Los servicios con ámbito no se resuelven directa o indirectamente desde el proveedor de servicios raíz.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-297">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="7d1a8-298">Los servicios con ámbito no se insertan directa o indirectamente en singletons.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-298">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="7d1a8-299">El proveedor de servicios raíz se crea cuando se llama a <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*>.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-299">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="7d1a8-300">La vigencia del proveedor de servicios raíz es la misma que la de la aplicación o el servidor cuando el proveedor se inicia con la aplicación, y se elimina cuando la aplicación se cierra.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-300">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="7d1a8-301">De la eliminación de los servicios con ámbito se encarga el contenedor que los creó.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-301">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="7d1a8-302">Si un servicio con ámbito se crea en el contenedor raíz, su vigencia sube a la del singleton, ya que solo lo puede eliminar el contenedor raíz cuando la aplicación o el servidor se cierran.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-302">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="7d1a8-303">Al validar los ámbitos de servicio, este tipo de situaciones se detectan cuando se llama a `BuildServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-303">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="7d1a8-304">Para más información, consulte <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-304">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="7d1a8-305">Servicios de solicitud</span><span class="sxs-lookup"><span data-stu-id="7d1a8-305">Request Services</span></span>

<span data-ttu-id="7d1a8-306">Los servicios disponibles en una solicitud de ASP.NET Core desde `HttpContext` se exponen mediante la colección [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices).</span><span class="sxs-lookup"><span data-stu-id="7d1a8-306">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="7d1a8-307">Los servicios de solicitud representan los servicios configurados y solicitados como parte de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-307">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="7d1a8-308">Cuando los objetos especifican dependencias, estas se cumplen mediante los tipos que se encuentran en `RequestServices`, no en `ApplicationServices`.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-308">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="7d1a8-309">Por lo general, la aplicación no debe usar estas propiedades directamente.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-309">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="7d1a8-310">En su lugar, solicite los tipos que las clases requieren mediante el constructor de clases y permita que el marco de trabajo inserte las dependencias.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-310">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="7d1a8-311">Esto produce clases que son más fáciles de probar.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-311">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="7d1a8-312">Se recomienda que solicite las dependencias como parámetros del constructor para obtener acceso a la colección `RequestServices`.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-312">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="7d1a8-313">Diseño de servicios para la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="7d1a8-313">Design services for dependency injection</span></span>

<span data-ttu-id="7d1a8-314">Los procedimientos recomendados son:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-314">Best practices are to:</span></span>

* <span data-ttu-id="7d1a8-315">Diseñar servicios para usar la inserción de dependencias a fin de obtener sus dependencias.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-315">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="7d1a8-316">Evite las llamadas de método estático y con estado.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-316">Avoid stateful, static method calls.</span></span>
* <span data-ttu-id="7d1a8-317">Evitar la creación directa de instancias de clases dependientes dentro de los servicios.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-317">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="7d1a8-318">La creación directa de instancias se acopla al código de una implementación particular.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-318">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="7d1a8-319">Cree clases de aplicación pequeñas, bien factorizadas y probadas con facilidad.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-319">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="7d1a8-320">Si una clase parece tener demasiadas dependencias insertadas, suele indicar que la clase tiene demasiadas responsabilidades y que esto infringe el [principio de responsabilidad única (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span><span class="sxs-lookup"><span data-stu-id="7d1a8-320">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="7d1a8-321">Trate de mover algunas de las responsabilidades de la clase a una nueva para intentar refactorizarla.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-321">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="7d1a8-322">Tenga en cuenta que las clases del modelo de página de Razor Pages y las clases de controlador MVC deben centrarse en aspectos de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-322">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="7d1a8-323">Los detalles de implementación de las reglas de negocio y del acceso a datos se deben mantener en las clases pertinentes para [cada uno de estos aspectos](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span><span class="sxs-lookup"><span data-stu-id="7d1a8-323">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="7d1a8-324">Eliminación de servicios</span><span class="sxs-lookup"><span data-stu-id="7d1a8-324">Disposal of services</span></span>

<span data-ttu-id="7d1a8-325">El contenedor llama a <xref:System.IDisposable.Dispose*> para los tipos <xref:System.IDisposable> que crea.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-325">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="7d1a8-326">Si se agrega una instancia al contenedor por código de usuario, no se elimina automáticamente.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-326">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

```csharp
// Services that implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    // The container creates the following instances and disposes them automatically:
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // The container doesn't create the following instances, so it doesn't dispose of
    // the instances automatically:
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

## <a name="default-service-container-replacement"></a><span data-ttu-id="7d1a8-327">Reemplazo del contenedor de servicios predeterminado</span><span class="sxs-lookup"><span data-stu-id="7d1a8-327">Default service container replacement</span></span>

<span data-ttu-id="7d1a8-328">El contenedor de servicios integrado está pensado para atender las necesidades del marco de trabajo y de la mayoría de las aplicaciones de consumidor.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-328">The built-in service container is meant to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="7d1a8-329">Se recomienda usar el contenedor integrado a menos que se necesite una característica específica no admitida por el contenedor.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-329">We recommend using the built-in container unless you need a specific feature that it doesn't support.</span></span> <span data-ttu-id="7d1a8-330">Algunas de las características admitidas en contenedores de terceros no se incluyen en el contenedor integrado:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-330">Some of the features supported in 3rd party containers not found in the built-in container:</span></span>

* <span data-ttu-id="7d1a8-331">Inserción de propiedades</span><span class="sxs-lookup"><span data-stu-id="7d1a8-331">Property injection</span></span>
* <span data-ttu-id="7d1a8-332">Inserción basada en nombres</span><span class="sxs-lookup"><span data-stu-id="7d1a8-332">Injection based on name</span></span>
* <span data-ttu-id="7d1a8-333">Contenedores secundarios</span><span class="sxs-lookup"><span data-stu-id="7d1a8-333">Child containers</span></span>
* <span data-ttu-id="7d1a8-334">Administración personalizada del ciclo de vida</span><span class="sxs-lookup"><span data-stu-id="7d1a8-334">Custom lifetime management</span></span>
* <span data-ttu-id="7d1a8-335">Compatibilidad con `Func<T>` para la inicialización diferida</span><span class="sxs-lookup"><span data-stu-id="7d1a8-335">`Func<T>` support for lazy initialization</span></span>

<span data-ttu-id="7d1a8-336">Vea el [archivo readme.md de inserción de dependencias](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) para obtener una lista de algunos de los contenedores que admiten adaptadores.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-336">See the [Dependency Injection readme.md file](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) for a list of some of the containers that support adapters.</span></span>

<span data-ttu-id="7d1a8-337">En el ejemplo siguiente se reemplaza el contenedor integrado por [Autofac](https://autofac.org/):</span><span class="sxs-lookup"><span data-stu-id="7d1a8-337">The following sample replaces the built-in container with [Autofac](https://autofac.org/):</span></span>

* <span data-ttu-id="7d1a8-338">Instale los paquetes de contenedor adecuados:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-338">Install the appropriate container package(s):</span></span>

  * [<span data-ttu-id="7d1a8-339">Autofac</span><span class="sxs-lookup"><span data-stu-id="7d1a8-339">Autofac</span></span>](https://www.nuget.org/packages/Autofac/)
  * [<span data-ttu-id="7d1a8-340">Autofac.Extensions.DependencyInjection</span><span class="sxs-lookup"><span data-stu-id="7d1a8-340">Autofac.Extensions.DependencyInjection</span></span>](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)

* <span data-ttu-id="7d1a8-341">Configure el contenedor en `Startup.ConfigureServices` y devuelva `IServiceProvider`:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-341">Configure the container in `Startup.ConfigureServices` and return an `IServiceProvider`:</span></span>

    ```csharp
    public IServiceProvider ConfigureServices(IServiceCollection services)
    {
        services.AddMvc();
        // Add other framework services

        // Add Autofac
        var containerBuilder = new ContainerBuilder();
        containerBuilder.RegisterModule<DefaultModule>();
        containerBuilder.Populate(services);
        var container = containerBuilder.Build();
        return new AutofacServiceProvider(container);
    }
    ```

    <span data-ttu-id="7d1a8-342">Para usar un contenedor de terceros, `Startup.ConfigureServices` debe devolver `IServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-342">To use a 3rd party container, `Startup.ConfigureServices` must return `IServiceProvider`.</span></span>

* <span data-ttu-id="7d1a8-343">Configure Autofac en `DefaultModule`:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-343">Configure Autofac in `DefaultModule`:</span></span>

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

<span data-ttu-id="7d1a8-344">En tiempo de ejecución, se usa Autofac para resolver tipos e insertar dependencias.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-344">At runtime, Autofac is used to resolve types and inject dependencies.</span></span> <span data-ttu-id="7d1a8-345">Para más información sobre el uso de Autofac con ASP.NET Core, vea la [documentación sobre Autofac](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span><span class="sxs-lookup"><span data-stu-id="7d1a8-345">To learn more about using Autofac with ASP.NET Core, see the [Autofac documentation](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="7d1a8-346">Seguridad para subprocesos</span><span class="sxs-lookup"><span data-stu-id="7d1a8-346">Thread safety</span></span>

<span data-ttu-id="7d1a8-347">Cree servicios de singleton seguros para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-347">Create thread-safe singleton services.</span></span> <span data-ttu-id="7d1a8-348">Si un servicio de singleton tiene una dependencia en un servicio transitorio, es posible que este también deba ser seguro para subprocesos, según cómo lo use el singleton.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-348">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="7d1a8-349">El patrón de diseño Factory Method de un servicio único, como el segundo argumento para [AddSingleton\<TService (IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), no necesita ser seguro para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-349">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="7d1a8-350">Al igual que un constructor de tipos (`static`), se garantiza que se le llame una vez mediante un único subproceso.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-350">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="7d1a8-351">Recomendaciones</span><span class="sxs-lookup"><span data-stu-id="7d1a8-351">Recommendations</span></span>

* <span data-ttu-id="7d1a8-352">No se admite la resolución de servicio basada en `async/await` y `Task`.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-352">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="7d1a8-353">C# no admite los constructores asincrónicos, por lo que el patrón recomendado es usar métodos asincrónicos después de resolver el servicio de manera sincrónica.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-353">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="7d1a8-354">Evite almacenar datos y configuraciones directamente en el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-354">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="7d1a8-355">Por ejemplo, el carro de la compra de un usuario no debería agregarse al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-355">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="7d1a8-356">La configuración debe usar el [patrón de opciones](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="7d1a8-356">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="7d1a8-357">Del mismo modo, evite los objetos de tipo "contenedor de datos" que solo existen para permitir el acceso a otro objeto.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-357">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="7d1a8-358">Es mejor solicitar el elemento real que se necesita mediante la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-358">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="7d1a8-359">Evite el acceso estático a servicios (por ejemplo, escribiendo de forma estática [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) para usarlo en otro lugar).</span><span class="sxs-lookup"><span data-stu-id="7d1a8-359">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="7d1a8-360">Evite el uso del *patrón del localizador de servicios*.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-360">Avoid using the *service locator pattern*.</span></span> <span data-ttu-id="7d1a8-361">Por ejemplo, no invoque a <xref:System.IServiceProvider.GetService*> para obtener una instancia de servicio si puede usar la inserción de dependencias en su lugar:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-361">For example, don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

  <span data-ttu-id="7d1a8-362">**Incorrecto:**</span><span class="sxs-lookup"><span data-stu-id="7d1a8-362">**Incorrect:**</span></span>

  ```csharp
  public class MyClass()
  {
      public void MyMethod()
      {
          var optionsMonitor = 
              _services.GetService<IOptionsMonitor<MyOptions>>();
          var option = optionsMonitor.CurrentValue.Option;

          ...
      }
  }
  ```

  <span data-ttu-id="7d1a8-363">**Correcto**:</span><span class="sxs-lookup"><span data-stu-id="7d1a8-363">**Correct**:</span></span>

  ```csharp
  public class MyClass
  {
      private readonly IOptionsMonitor<MyOptions> _optionsMonitor;

      public MyClass(IOptionsMonitor<MyOptions> optionsMonitor)
      {
          _optionsMonitor = optionsMonitor;
      }

      public void MyMethod()
      {
          var option = _optionsMonitor.CurrentValue.Option;

          ...
      }
  }
  ```

* <span data-ttu-id="7d1a8-364">Otra variación del localizador de servicios que se debe evitar es insertar una fábrica que resuelva dependencias en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-364">Another service locator variation to avoid is injecting a factory that resolves dependencies at runtime.</span></span> <span data-ttu-id="7d1a8-365">Estas dos prácticas combinan estrategias de [Inversión de control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion).</span><span class="sxs-lookup"><span data-stu-id="7d1a8-365">Both of these practices mix [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

* <span data-ttu-id="7d1a8-366">Evite el acceso estático a `HttpContext` (por ejemplo, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span><span class="sxs-lookup"><span data-stu-id="7d1a8-366">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="7d1a8-367">Al igual que sucede con todas las recomendaciones, podría verse en una situación que le obligue a ignorar alguna de ellas.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-367">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="7d1a8-368">Las excepciones son poco frecuentes; principalmente en casos especiales dentro del propio marco de trabajo.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-368">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="7d1a8-369">La inserción de dependencias es una *alternativa* a los patrones de acceso a objetos estáticos o globales.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-369">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="7d1a8-370">No podrá aprovechar las ventajas de la inserción de dependencias si la combina con el acceso a objetos estáticos.</span><span class="sxs-lookup"><span data-stu-id="7d1a8-370">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7d1a8-371">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="7d1a8-371">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="7d1a8-372">Escritura de código limpio en ASP.NET Core con inserción de dependencias (MSDN)</span><span class="sxs-lookup"><span data-stu-id="7d1a8-372">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="7d1a8-373">Principio de dependencias explícitas</span><span class="sxs-lookup"><span data-stu-id="7d1a8-373">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [<span data-ttu-id="7d1a8-374">Los contenedores de inversión de control y el patrón de inserción de dependencias (Martin Fowler)</span><span class="sxs-lookup"><span data-stu-id="7d1a8-374">Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
* [<span data-ttu-id="7d1a8-375">Cómo registrar un servicio con varias interfaces de inserción de dependencias de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d1a8-375">How to register a service with multiple interfaces in ASP.NET Core DI</span></span>](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)
