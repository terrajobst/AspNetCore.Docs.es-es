---
title: Inserción de dependencias en ASP.NET Core
author: guardrex
description: Obtenga información sobre la manera en que ASP.NET Core implementa la inserción de dependencias y cómo se usa.
ms.author: riande
ms.custom: mvc
ms.date: 02/25/2019
uid: fundamentals/dependency-injection
ms.openlocfilehash: cc020d7397b03f8ecd6cebf98a14b4aaebb47940
ms.sourcegitcommit: 687ffb15ebe65379f75c84739ea851d5a0d788b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488694"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="02af9-103">Inserción de dependencias en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="02af9-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="02af9-104">Por [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="02af9-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="02af9-105">ASP.NET Core admite el patrón de diseño de software de inserción de dependencias (DI), que es una técnica para conseguir la [inversión de control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) entre clases y sus dependencias.</span><span class="sxs-lookup"><span data-stu-id="02af9-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="02af9-106">Para más información específica sobre la inserción de dependencias en los controladores MVC, vea <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="02af9-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="02af9-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="02af9-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="02af9-108">Información general sobre la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="02af9-108">Overview of dependency injection</span></span>

<span data-ttu-id="02af9-109">Una *dependencia* es cualquier objeto requerido por otro objeto.</span><span class="sxs-lookup"><span data-stu-id="02af9-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="02af9-110">Examine la siguiente clase `MyDependency` con un método `WriteMessage` del que dependen otras clases de una aplicación:</span><span class="sxs-lookup"><span data-stu-id="02af9-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

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

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="02af9-111">Se puede crear una instancia de la clase `MyDependency` para hacer que el método `WriteMessage` esté disponible para una clase.</span><span class="sxs-lookup"><span data-stu-id="02af9-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="02af9-112">La clase `MyDependency` es una dependencia de la clase `IndexModel`:</span><span class="sxs-lookup"><span data-stu-id="02af9-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

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

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="02af9-113">Se puede crear una instancia de la clase `MyDependency` para hacer que el método `WriteMessage` esté disponible para una clase.</span><span class="sxs-lookup"><span data-stu-id="02af9-113">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="02af9-114">La clase `MyDependency` es una dependencia de la clase `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="02af9-114">The `MyDependency` class is a dependency of the `HomeController` class:</span></span>

```csharp
public class HomeController : Controller
{
    MyDependency _dependency = new MyDependency();

    public async Task<IActionResult> Index()
    {
        await _dependency.WriteMessage(
            "HomeController.Index created this message.");

        return View();
    }
}
```

::: moniker-end

<span data-ttu-id="02af9-115">La clase crea y depende directamente de la instancia `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="02af9-115">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="02af9-116">Las dependencias de código (como en el ejemplo anterior) son problemáticas y deben evitarse por las siguientes razones:</span><span class="sxs-lookup"><span data-stu-id="02af9-116">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="02af9-117">Para reemplazar `MyDependency` con una implementación diferente, se debe modificar la clase.</span><span class="sxs-lookup"><span data-stu-id="02af9-117">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="02af9-118">Si `MyDependency` tiene dependencias, deben configurarse según la clase.</span><span class="sxs-lookup"><span data-stu-id="02af9-118">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="02af9-119">En un proyecto grande con varias clases que dependen de `MyDependency`, el código de configuración se dispersa por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="02af9-119">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="02af9-120">Esta implementación es difícil para realizar pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="02af9-120">This implementation is difficult to unit test.</span></span> <span data-ttu-id="02af9-121">La aplicación debe usar una clase `MyDependency` como boceto o código auxiliar, que no es posible con este enfoque.</span><span class="sxs-lookup"><span data-stu-id="02af9-121">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="02af9-122">La inserción de dependencias aborda estos problemas mediante:</span><span class="sxs-lookup"><span data-stu-id="02af9-122">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="02af9-123">El uso de una interfaz para abstraer la implementación de dependencias.</span><span class="sxs-lookup"><span data-stu-id="02af9-123">The use of an interface to abstract the dependency implementation.</span></span>
* <span data-ttu-id="02af9-124">Registro de la dependencia en un contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="02af9-124">Registration of the dependency in a service container.</span></span> <span data-ttu-id="02af9-125">ASP.NET Core proporciona un contenedor de servicios integrado, [IServiceProvider](/dotnet/api/system.iserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="02af9-125">ASP.NET Core provides a built-in service container, [IServiceProvider](/dotnet/api/system.iserviceprovider).</span></span> <span data-ttu-id="02af9-126">Los servicios se registran en el método `Startup.ConfigureServices` de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="02af9-126">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="02af9-127">*Inserción* del servicio en el constructor de la clase en la que se usa.</span><span class="sxs-lookup"><span data-stu-id="02af9-127">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="02af9-128">El marco de trabajo asume la responsabilidad de crear una instancia de la dependencia y de desecharla cuando ya no es necesaria.</span><span class="sxs-lookup"><span data-stu-id="02af9-128">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="02af9-129">En la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), la interfaz `IMyDependency` define un método que el servicio proporciona a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="02af9-129">In the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="02af9-130">Esta interfaz se implementa mediante un tipo concreto, `MyDependency`:</span><span class="sxs-lookup"><span data-stu-id="02af9-130">This interface is implemented by a concrete type, `MyDependency`:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="02af9-131">`MyDependency` solicita una instancia de [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) en su constructor.</span><span class="sxs-lookup"><span data-stu-id="02af9-131">`MyDependency` requests an [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) in its constructor.</span></span> <span data-ttu-id="02af9-132">No es raro usar la inserción de dependencias de forma encadenada.</span><span class="sxs-lookup"><span data-stu-id="02af9-132">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="02af9-133">Cada dependencia solicitada a su vez solicita sus propias dependencias.</span><span class="sxs-lookup"><span data-stu-id="02af9-133">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="02af9-134">El contenedor resuelve las dependencias del gráfico y devuelve el servicio totalmente resuelto.</span><span class="sxs-lookup"><span data-stu-id="02af9-134">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="02af9-135">El conjunto colectivo de dependencias que deben resolverse suele denominarse *árbol de dependencias*, *gráfico de dependencias* o *gráfico de objetos*.</span><span class="sxs-lookup"><span data-stu-id="02af9-135">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="02af9-136">`IMyDependency` y `ILogger<TCategoryName>` deben estar registrados en el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="02af9-136">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="02af9-137">`IMyDependency` está registrado en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="02af9-137">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="02af9-138">`ILogger<TCategoryName>` está registrado en la infraestructura de abstracciones de registros, por lo que se trata de un [servicio proporcionado por el marco de trabajo](#framework-provided-services) registrado de forma predeterminada por el marco de trabajo.</span><span class="sxs-lookup"><span data-stu-id="02af9-138">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="02af9-139">El contenedor resuelve `ILogger<TCategoryName>` aprovechando las ventajas de los [tipos abiertos (genéricos)](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), lo que elimina la necesidad de registrar todos los [tipos construidos (genéricos)](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span><span class="sxs-lookup"><span data-stu-id="02af9-139">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<T>), typeof(Logger<T>));
```

<span data-ttu-id="02af9-140">En la aplicación de ejemplo, el servicio `IMyDependency` está registrado con el tipo concreto `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="02af9-140">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="02af9-141">El registro abarca la duración del servicio como la duración de una única solicitud.</span><span class="sxs-lookup"><span data-stu-id="02af9-141">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="02af9-142">Las [duraciones del servicio](#service-lifetimes) se describen más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="02af9-142">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="02af9-143">Cada método de extensión `services.Add{SERVICE_NAME}` agrega servicios (y potencialmente los configura).</span><span class="sxs-lookup"><span data-stu-id="02af9-143">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="02af9-144">Por ejemplo, `services.AddMvc()` agrega los servicios que Razor Pages y MVC requieren.</span><span class="sxs-lookup"><span data-stu-id="02af9-144">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="02af9-145">Se recomienda que las aplicaciones sigan esta convención.</span><span class="sxs-lookup"><span data-stu-id="02af9-145">We recommended that apps follow this convention.</span></span> <span data-ttu-id="02af9-146">Coloque los métodos de extensión en el espacio de nombres [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) para encapsular grupos de registros del servicio.</span><span class="sxs-lookup"><span data-stu-id="02af9-146">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="02af9-147">Si el constructor del servicio requiere un [tipo integrado](/dotnet/csharp/language-reference/keywords/built-in-types-table), como `string`, se puede insertar mediante la [configuración](xref:fundamentals/configuration/index) o el [patrón de opciones](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="02af9-147">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

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

<span data-ttu-id="02af9-148">Se solicita una instancia del servicio mediante el constructor de una clase, en la que se usa el servicio y se asigna a un campo privado.</span><span class="sxs-lookup"><span data-stu-id="02af9-148">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="02af9-149">El campo de utiliza para acceder al servicio, según sea necesario en la clase.</span><span class="sxs-lookup"><span data-stu-id="02af9-149">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="02af9-150">En la aplicación de ejemplo, la instancia `IMyDependency` se solicita y usa para llamar al método `WriteMessage` del servicio:</span><span class="sxs-lookup"><span data-stu-id="02af9-150">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/MyDependencyController.cs?name=snippet1&highlight=3,5-8,13-14)]

::: moniker-end

## <a name="framework-provided-services"></a><span data-ttu-id="02af9-151">Servicios proporcionados por el marco de trabajo</span><span class="sxs-lookup"><span data-stu-id="02af9-151">Framework-provided services</span></span>

<span data-ttu-id="02af9-152">El método `Startup.ConfigureServices` se encarga de definir los servicios que la aplicación usa, incluidas las características de plataforma como Entity Framework Core y ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="02af9-152">The `Startup.ConfigureServices` method is responsible for defining the services the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="02af9-153">Inicialmente, el valor `IServiceCollection` proporcionado a `ConfigureServices` tiene los siguientes servicios definidos (en función de [cómo se configurara el host](xref:fundamentals/index#host)):</span><span class="sxs-lookup"><span data-stu-id="02af9-153">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/index#host)):</span></span>

| <span data-ttu-id="02af9-154">Tipo de servicio</span><span class="sxs-lookup"><span data-stu-id="02af9-154">Service Type</span></span> | <span data-ttu-id="02af9-155">Período de duración</span><span class="sxs-lookup"><span data-stu-id="02af9-155">Lifetime</span></span> |
| ------------ | -------- |
| [<span data-ttu-id="02af9-156">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span><span class="sxs-lookup"><span data-stu-id="02af9-156">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | <span data-ttu-id="02af9-157">Transitorio</span><span class="sxs-lookup"><span data-stu-id="02af9-157">Transient</span></span> |
| [<span data-ttu-id="02af9-158">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="02af9-158">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | <span data-ttu-id="02af9-159">Singleton</span><span class="sxs-lookup"><span data-stu-id="02af9-159">Singleton</span></span> |
| [<span data-ttu-id="02af9-160">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="02af9-160">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | <span data-ttu-id="02af9-161">Singleton</span><span class="sxs-lookup"><span data-stu-id="02af9-161">Singleton</span></span> |
| [<span data-ttu-id="02af9-162">Microsoft.AspNetCore.Hosting.IStartup</span><span class="sxs-lookup"><span data-stu-id="02af9-162">Microsoft.AspNetCore.Hosting.IStartup</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | <span data-ttu-id="02af9-163">Singleton</span><span class="sxs-lookup"><span data-stu-id="02af9-163">Singleton</span></span> |
| [<span data-ttu-id="02af9-164">Microsoft.AspNetCore.Hosting.IStartupFilter</span><span class="sxs-lookup"><span data-stu-id="02af9-164">Microsoft.AspNetCore.Hosting.IStartupFilter</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | <span data-ttu-id="02af9-165">Transitorio</span><span class="sxs-lookup"><span data-stu-id="02af9-165">Transient</span></span> |
| [<span data-ttu-id="02af9-166">Microsoft.AspNetCore.Hosting.Server.IServer</span><span class="sxs-lookup"><span data-stu-id="02af9-166">Microsoft.AspNetCore.Hosting.Server.IServer</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | <span data-ttu-id="02af9-167">Singleton</span><span class="sxs-lookup"><span data-stu-id="02af9-167">Singleton</span></span> |
| [<span data-ttu-id="02af9-168">Microsoft.AspNetCore.Http.IHttpContextFactory</span><span class="sxs-lookup"><span data-stu-id="02af9-168">Microsoft.AspNetCore.Http.IHttpContextFactory</span></span>](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | <span data-ttu-id="02af9-169">Transitorio</span><span class="sxs-lookup"><span data-stu-id="02af9-169">Transient</span></span> |
| [<span data-ttu-id="02af9-170">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="02af9-170">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.logging.ilogger) | <span data-ttu-id="02af9-171">Singleton</span><span class="sxs-lookup"><span data-stu-id="02af9-171">Singleton</span></span> |
| [<span data-ttu-id="02af9-172">Microsoft.Extensions.Logging.ILoggerFactory</span><span class="sxs-lookup"><span data-stu-id="02af9-172">Microsoft.Extensions.Logging.ILoggerFactory</span></span>](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | <span data-ttu-id="02af9-173">Singleton</span><span class="sxs-lookup"><span data-stu-id="02af9-173">Singleton</span></span> |
| [<span data-ttu-id="02af9-174">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span><span class="sxs-lookup"><span data-stu-id="02af9-174">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span></span>](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | <span data-ttu-id="02af9-175">Singleton</span><span class="sxs-lookup"><span data-stu-id="02af9-175">Singleton</span></span> |
| [<span data-ttu-id="02af9-176">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="02af9-176">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | <span data-ttu-id="02af9-177">Transitorio</span><span class="sxs-lookup"><span data-stu-id="02af9-177">Transient</span></span> |
| [<span data-ttu-id="02af9-178">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="02af9-178">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.ioptions-1) | <span data-ttu-id="02af9-179">Singleton</span><span class="sxs-lookup"><span data-stu-id="02af9-179">Singleton</span></span> |
| [<span data-ttu-id="02af9-180">System.Diagnostics.DiagnosticSource</span><span class="sxs-lookup"><span data-stu-id="02af9-180">System.Diagnostics.DiagnosticSource</span></span>](/dotnet/core/api/system.diagnostics.diagnosticsource) | <span data-ttu-id="02af9-181">Singleton</span><span class="sxs-lookup"><span data-stu-id="02af9-181">Singleton</span></span> |
| [<span data-ttu-id="02af9-182">System.Diagnostics.DiagnosticListener</span><span class="sxs-lookup"><span data-stu-id="02af9-182">System.Diagnostics.DiagnosticListener</span></span>](/dotnet/core/api/system.diagnostics.diagnosticlistener) | <span data-ttu-id="02af9-183">Singleton</span><span class="sxs-lookup"><span data-stu-id="02af9-183">Singleton</span></span> |

<span data-ttu-id="02af9-184">Cuando un método de extensión de la colección de servicio está disponible para registrar un servicio (y sus servicios dependientes, si es necesario), la convención consiste en usar un solo método de extensión `Add{SERVICE_NAME}` para registrar todos los servicios requeridos por dicho servicio.</span><span class="sxs-lookup"><span data-stu-id="02af9-184">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="02af9-185">El código siguiente es un ejemplo de cómo agregar servicios adicionales al contenedor mediante los métodos de extensión [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity) y [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc):</span><span class="sxs-lookup"><span data-stu-id="02af9-185">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity), and [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc):</span></span>

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

<span data-ttu-id="02af9-186">Para más información, vea la [clase ServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection) en la documentación de la API.</span><span class="sxs-lookup"><span data-stu-id="02af9-186">For more information, see the [ServiceCollection Class](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection) in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="02af9-187">Duraciones de servicios</span><span class="sxs-lookup"><span data-stu-id="02af9-187">Service lifetimes</span></span>

<span data-ttu-id="02af9-188">Elija una duración adecuada para cada servicio registrado.</span><span class="sxs-lookup"><span data-stu-id="02af9-188">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="02af9-189">Los servicios de ASP.NET Core pueden configurarse con las duraciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="02af9-189">ASP.NET Core services can be configured with the following lifetimes:</span></span>

<span data-ttu-id="02af9-190">**Transitoria**</span><span class="sxs-lookup"><span data-stu-id="02af9-190">**Transient**</span></span>

<span data-ttu-id="02af9-191">Los servicios de duración transitoria se crean cada vez que solicitan.</span><span class="sxs-lookup"><span data-stu-id="02af9-191">Transient lifetime services are created each time they're requested.</span></span> <span data-ttu-id="02af9-192">Esta duración funciona mejor para servicios sin estado ligeros.</span><span class="sxs-lookup"><span data-stu-id="02af9-192">This lifetime works best for lightweight, stateless services.</span></span>

<span data-ttu-id="02af9-193">**Con ámbito**</span><span class="sxs-lookup"><span data-stu-id="02af9-193">**Scoped**</span></span>

<span data-ttu-id="02af9-194">Los servicios de duración con ámbito se crean una vez por solicitud.</span><span class="sxs-lookup"><span data-stu-id="02af9-194">Scoped lifetime services are created once per request.</span></span>

> [!WARNING]
> <span data-ttu-id="02af9-195">Si usa un servicio con ámbito en un middleware, inserte el servicio en el método `Invoke` o `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="02af9-195">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="02af9-196">No lo inserte a través de la inserción de constructores, porque ello hace que el servicio se comporte como un singleton.</span><span class="sxs-lookup"><span data-stu-id="02af9-196">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="02af9-197">Para obtener más información, vea <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="02af9-197">For more information, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="02af9-198">**Singleton**</span><span class="sxs-lookup"><span data-stu-id="02af9-198">**Singleton**</span></span>

<span data-ttu-id="02af9-199">Los servicios con duración Singleton se crean la primera vez que se solicitan, o bien cuando se ejecuta `ConfigureServices` y se especifica una instancia con el registro del servicio.</span><span class="sxs-lookup"><span data-stu-id="02af9-199">Singleton lifetime services are created the first time they're requested (or when `ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="02af9-200">Cada solicitud posterior usa la misma instancia.</span><span class="sxs-lookup"><span data-stu-id="02af9-200">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="02af9-201">Si la aplicación requiere un comportamiento de singleton, se recomienda permitir que el contenedor de servicios administre la duración del servicio.</span><span class="sxs-lookup"><span data-stu-id="02af9-201">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="02af9-202">No implemente el patrón de diseño de singleton y proporcione el código de usuario para administrar la duración del objeto en la clase.</span><span class="sxs-lookup"><span data-stu-id="02af9-202">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="02af9-203">Es peligroso resolver un servicio con ámbito desde un singleton.</span><span class="sxs-lookup"><span data-stu-id="02af9-203">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="02af9-204">Puede dar lugar a que el servicio adopte un estado incorrecto al procesar solicitudes posteriores.</span><span class="sxs-lookup"><span data-stu-id="02af9-204">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

### <a name="constructor-injection-behavior"></a><span data-ttu-id="02af9-205">Comportamiento de inserción de constructor</span><span class="sxs-lookup"><span data-stu-id="02af9-205">Constructor injection behavior</span></span>

<span data-ttu-id="02af9-206">Los servicios se pueden resolver mediante dos mecanismos:</span><span class="sxs-lookup"><span data-stu-id="02af9-206">Services can be resolved by two mechanisms:</span></span>

* `IServiceProvider`
* <span data-ttu-id="02af9-207">[ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; Permite la creación de objetos sin registrar el servicio en el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="02af9-207">[ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="02af9-208">`ActivatorUtilities` se utiliza con abstracciones orientadas al usuario, como asistentes de etiquetas, controladores MVC y enlazadores de modelos.</span><span class="sxs-lookup"><span data-stu-id="02af9-208">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="02af9-209">Los constructores pueden aceptar argumentos que no se proporcionan mediante la inserción de dependencias, pero los argumentos deben asignar valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="02af9-209">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="02af9-210">Cuando se resuelven los servicios mediante `IServiceProvider` o `ActivatorUtilities`, la inserción del constructor requiere un constructor *público*.</span><span class="sxs-lookup"><span data-stu-id="02af9-210">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="02af9-211">Cuando se resuelven los servicios mediante `ActivatorUtilities`, la inserción del constructor requiere que exista solo un constructor aplicable.</span><span class="sxs-lookup"><span data-stu-id="02af9-211">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exists.</span></span> <span data-ttu-id="02af9-212">Se admiten las sobrecargas de constructor, pero solo puede existir una sobrecarga cuyos argumentos pueda cumplir la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="02af9-212">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="02af9-213">Contextos de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="02af9-213">Entity Framework contexts</span></span>

<span data-ttu-id="02af9-214">Los contextos de Entity Framework normalmente se agregan al contenedor de servicios mediante la [duración con ámbito](#service-lifetimes) porque las operaciones de base de datos de aplicación web se suelen limitar a la solicitud.</span><span class="sxs-lookup"><span data-stu-id="02af9-214">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the request.</span></span> <span data-ttu-id="02af9-215">La duración predeterminada se limita si no se especifica una duración mediante una sobrecarga de <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*> al registrar el contexto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="02af9-215">The default lifetime is scoped if a lifetime isn't specified by an <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*> overload when registering the database context.</span></span> <span data-ttu-id="02af9-216">En los servicios de una duración determinada no se debe usar un contexto de base de datos con una duración más corta que el servicio.</span><span class="sxs-lookup"><span data-stu-id="02af9-216">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="02af9-217">Opciones de registro y duración</span><span class="sxs-lookup"><span data-stu-id="02af9-217">Lifetime and registration options</span></span>

<span data-ttu-id="02af9-218">Para mostrar la diferencia entre la duración y las opciones de registro, considere las siguientes interfaces que representan tareas como una operación con un identificador único, `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="02af9-218">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="02af9-219">Según cómo esté configurada la duración de un servicio de operaciones para las interfaces siguientes, el contenedor proporciona la misma instancia del servicio u otra distinta cuando así lo solicita la clase:</span><span class="sxs-lookup"><span data-stu-id="02af9-219">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="02af9-220">Las interfaces se implementan en la clase `Operation`.</span><span class="sxs-lookup"><span data-stu-id="02af9-220">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="02af9-221">El constructor `Operation` genera un GUID en caso de que no se proporcione uno:</span><span class="sxs-lookup"><span data-stu-id="02af9-221">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="02af9-222">Se registra una instancia de `OperationService` que depende de cada uno de los demás tipos `Operation`.</span><span class="sxs-lookup"><span data-stu-id="02af9-222">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="02af9-223">Cuando `OperationService` se solicita con la inserción de dependencias, recibe una instancia nueva de cada servicio o una instancia existente en función de la duración de los servicios dependientes.</span><span class="sxs-lookup"><span data-stu-id="02af9-223">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="02af9-224">Si se crean servicios transitorios cuando se solicitan, el elemento `OperationId` del servicio `IOperationTransient` varía del objeto `OperationId` de `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="02af9-224">If transient services are created when requested, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="02af9-225">`OperationService` recibe una instancia nueva de la clase `IOperationTransient`.</span><span class="sxs-lookup"><span data-stu-id="02af9-225">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="02af9-226">La nueva instancia produce un objeto `OperationId` diferente.</span><span class="sxs-lookup"><span data-stu-id="02af9-226">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="02af9-227">Si se crean servicios con ámbito por cada solicitud, el objeto `OperationId` del servicio `IOperationScoped` es el mismo que para `OperationService` dentro de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="02af9-227">If scoped services are created per request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a request.</span></span> <span data-ttu-id="02af9-228">Entre las solicitudes, ambos servicios comparten un valor `OperationId` diferente.</span><span class="sxs-lookup"><span data-stu-id="02af9-228">Across requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="02af9-229">Si se crean servicios singleton y servicios de instancia singleton una vez y se usan en todas las solicitudes y servicios, el objeto `OperationId` es constante en todas las solicitudes de servicio.</span><span class="sxs-lookup"><span data-stu-id="02af9-229">If singleton and singleton-instance services are created once and used across all requests and all services, the `OperationId` is constant across all service requests.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="02af9-230">En `Startup.ConfigureServices`, cada tipo se agrega al contenedor según su duración con nombre:</span><span class="sxs-lookup"><span data-stu-id="02af9-230">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=12-15,18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

<span data-ttu-id="02af9-231">El servicio `IOperationSingletonInstance` usa una instancia específica con un identificador conocido de `Guid.Empty`.</span><span class="sxs-lookup"><span data-stu-id="02af9-231">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="02af9-232">Resulta evidente identificar cuándo este tipo está en uso (su GUID es todo ceros).</span><span class="sxs-lookup"><span data-stu-id="02af9-232">It's clear when this type is in use (its GUID is all zeroes).</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="02af9-233">La aplicación de ejemplo muestra las duraciones de los objetos dentro y entre las solicitudes individuales.</span><span class="sxs-lookup"><span data-stu-id="02af9-233">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="02af9-234">El objeto `IndexModel` de la aplicación de ejemplo solicita cada tipo de `IOperation` y `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="02af9-234">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="02af9-235">Después, la página muestra todos los valores `OperationId` del servicio y la clase del modelo de página mediante las asignaciones de propiedades:</span><span class="sxs-lookup"><span data-stu-id="02af9-235">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="02af9-236">La aplicación de ejemplo muestra las duraciones de los objetos dentro y entre las solicitudes individuales.</span><span class="sxs-lookup"><span data-stu-id="02af9-236">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="02af9-237">La aplicación de ejemplo incluye un valor `OperationsController` que solicita cada tipo de `IOperation` y `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="02af9-237">The sample app includes an `OperationsController` that requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="02af9-238">La acción `Index` establece el servicio en `ViewBag` para mostrar los valores `OperationId` del servicio:</span><span class="sxs-lookup"><span data-stu-id="02af9-238">The `Index` action sets the services into the `ViewBag` for display of the service's `OperationId` values:</span></span>

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/OperationsController.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="02af9-239">Las dos salidas siguientes muestran el resultado de dos solicitudes:</span><span class="sxs-lookup"><span data-stu-id="02af9-239">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="02af9-240">**Primera solicitud:**</span><span class="sxs-lookup"><span data-stu-id="02af9-240">**First request:**</span></span>

<span data-ttu-id="02af9-241">Operaciones del controlador:</span><span class="sxs-lookup"><span data-stu-id="02af9-241">Controller operations:</span></span>

<span data-ttu-id="02af9-242">Transient: d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="02af9-242">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="02af9-243">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="02af9-243">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="02af9-244">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="02af9-244">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="02af9-245">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="02af9-245">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="02af9-246">Operaciones `OperationService`:</span><span class="sxs-lookup"><span data-stu-id="02af9-246">`OperationService` operations:</span></span>

<span data-ttu-id="02af9-247">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="02af9-247">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="02af9-248">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="02af9-248">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="02af9-249">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="02af9-249">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="02af9-250">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="02af9-250">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="02af9-251">**Segunda solicitud:**</span><span class="sxs-lookup"><span data-stu-id="02af9-251">**Second request:**</span></span>

<span data-ttu-id="02af9-252">Operaciones del controlador:</span><span class="sxs-lookup"><span data-stu-id="02af9-252">Controller operations:</span></span>

<span data-ttu-id="02af9-253">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="02af9-253">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="02af9-254">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="02af9-254">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="02af9-255">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="02af9-255">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="02af9-256">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="02af9-256">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="02af9-257">Operaciones `OperationService`:</span><span class="sxs-lookup"><span data-stu-id="02af9-257">`OperationService` operations:</span></span>

<span data-ttu-id="02af9-258">Transitorio: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="02af9-258">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="02af9-259">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="02af9-259">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="02af9-260">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="02af9-260">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="02af9-261">Instance: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="02af9-261">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="02af9-262">Observe cuál de los valores `OperationId` varía dentro de una solicitud y entre solicitudes:</span><span class="sxs-lookup"><span data-stu-id="02af9-262">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="02af9-263">Los objetos *Transient* siempre son diferentes.</span><span class="sxs-lookup"><span data-stu-id="02af9-263">*Transient* objects are always different.</span></span> <span data-ttu-id="02af9-264">Tenga en cuenta que el valor `OperationId` transitorio de la primera y segunda solicitud varía tanto para las operaciones `OperationService` como entre solicitudes.</span><span class="sxs-lookup"><span data-stu-id="02af9-264">Note that the transient `OperationId` value for both the first and second requests are different for both `OperationService` operations and across requests.</span></span> <span data-ttu-id="02af9-265">Se proporciona una nueva instancia para cada servicio y solicitud.</span><span class="sxs-lookup"><span data-stu-id="02af9-265">A new instance is provided to each service and request.</span></span>
* <span data-ttu-id="02af9-266">Los objetos *con ámbito* son iguales dentro de una solicitud, pero varían entre solicitudes.</span><span class="sxs-lookup"><span data-stu-id="02af9-266">*Scoped* objects are the same within a request but different across requests.</span></span>
* <span data-ttu-id="02af9-267">Los objetos *singleton* son iguales para todos los objetos y solicitudes, independientemente de si se proporciona una instancia `Operation` en `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="02af9-267">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="02af9-268">Llamada a servicios desde main</span><span class="sxs-lookup"><span data-stu-id="02af9-268">Call services from main</span></span>

<span data-ttu-id="02af9-269">Cree un [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) con [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) para resolver un servicio con ámbito dentro del ámbito de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="02af9-269">Create an [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) with [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="02af9-270">Este método resulta útil para tener acceso a un servicio con ámbito durante el inicio para realizar tareas de inicialización.</span><span class="sxs-lookup"><span data-stu-id="02af9-270">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="02af9-271">En el siguiente ejemplo se indica cómo obtener un contexto para `MyScopedService` en `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="02af9-271">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="02af9-272">Validación del ámbito</span><span class="sxs-lookup"><span data-stu-id="02af9-272">Scope validation</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="02af9-273">Cuando la aplicación se ejecuta en el entorno de desarrollo, el proveedor de servicios predeterminado realiza comprobaciones para confirmar lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="02af9-273">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="02af9-274">Los servicios con ámbito no se resuelven directa o indirectamente desde el proveedor de servicios raíz.</span><span class="sxs-lookup"><span data-stu-id="02af9-274">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="02af9-275">Los servicios con ámbito no se insertan directa o indirectamente en singletons.</span><span class="sxs-lookup"><span data-stu-id="02af9-275">Scoped services aren't directly or indirectly injected into singletons.</span></span>

::: moniker-end

<span data-ttu-id="02af9-276">El proveedor de servicios raíz se crea cuando se llama a [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="02af9-276">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="02af9-277">La vigencia del proveedor de servicios raíz es la misma que la de la aplicación o el servidor cuando el proveedor se inicia con la aplicación, y se elimina cuando la aplicación se cierra.</span><span class="sxs-lookup"><span data-stu-id="02af9-277">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="02af9-278">De la eliminación de los servicios con ámbito se encarga el contenedor que los creó.</span><span class="sxs-lookup"><span data-stu-id="02af9-278">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="02af9-279">Si un servicio con ámbito se crea en el contenedor raíz, su vigencia sube a la del singleton, ya que solo lo puede eliminar el contenedor raíz cuando la aplicación o el servidor se cierran.</span><span class="sxs-lookup"><span data-stu-id="02af9-279">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="02af9-280">Al validar los ámbitos de servicio, este tipo de situaciones se detectan cuando se llama a `BuildServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="02af9-280">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="02af9-281">Para obtener más información, vea <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="02af9-281">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="02af9-282">Servicios de solicitud</span><span class="sxs-lookup"><span data-stu-id="02af9-282">Request Services</span></span>

<span data-ttu-id="02af9-283">Los servicios disponibles en una solicitud de ASP.NET Core desde `HttpContext` se exponen mediante la colección [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices).</span><span class="sxs-lookup"><span data-stu-id="02af9-283">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices) collection.</span></span>

<span data-ttu-id="02af9-284">Los servicios de solicitud representan los servicios configurados y solicitados como parte de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="02af9-284">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="02af9-285">Cuando los objetos especifican dependencias, estas se cumplen mediante los tipos que se encuentran en `RequestServices`, no en `ApplicationServices`.</span><span class="sxs-lookup"><span data-stu-id="02af9-285">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="02af9-286">Por lo general, la aplicación no debe usar estas propiedades directamente.</span><span class="sxs-lookup"><span data-stu-id="02af9-286">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="02af9-287">En su lugar, solicite los tipos que las clases requieren mediante el constructor de clases y permita que el marco de trabajo inserte las dependencias.</span><span class="sxs-lookup"><span data-stu-id="02af9-287">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="02af9-288">Esto produce clases que son más fáciles de probar.</span><span class="sxs-lookup"><span data-stu-id="02af9-288">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="02af9-289">Se recomienda que solicite las dependencias como parámetros del constructor para obtener acceso a la colección `RequestServices`.</span><span class="sxs-lookup"><span data-stu-id="02af9-289">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="02af9-290">Diseño de servicios para la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="02af9-290">Design services for dependency injection</span></span>

<span data-ttu-id="02af9-291">Los procedimientos recomendados son:</span><span class="sxs-lookup"><span data-stu-id="02af9-291">Best practices are to:</span></span>

* <span data-ttu-id="02af9-292">Diseñar servicios para usar la inserción de dependencias a fin de obtener sus dependencias.</span><span class="sxs-lookup"><span data-stu-id="02af9-292">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="02af9-293">Evite las llamadas de método estático y con estado.</span><span class="sxs-lookup"><span data-stu-id="02af9-293">Avoid stateful, static method calls.</span></span>
* <span data-ttu-id="02af9-294">Evitar la creación directa de instancias de clases dependientes dentro de los servicios.</span><span class="sxs-lookup"><span data-stu-id="02af9-294">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="02af9-295">La creación directa de instancias se acopla al código de una implementación particular.</span><span class="sxs-lookup"><span data-stu-id="02af9-295">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="02af9-296">Cree clases de aplicación pequeñas, bien factorizadas y probadas con facilidad.</span><span class="sxs-lookup"><span data-stu-id="02af9-296">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="02af9-297">Si una clase parece tener demasiadas dependencias insertadas, suele indicar que la clase tiene demasiadas responsabilidades y que esto infringe el [principio de responsabilidad única (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span><span class="sxs-lookup"><span data-stu-id="02af9-297">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="02af9-298">Trate de mover algunas de las responsabilidades de la clase a una nueva para intentar refactorizarla.</span><span class="sxs-lookup"><span data-stu-id="02af9-298">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="02af9-299">Tenga en cuenta que las clases del modelo de página de Razor Pages y las clases de controlador MVC deben centrarse en aspectos de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="02af9-299">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="02af9-300">Los detalles de implementación de las reglas de negocio y del acceso a datos se deben mantener en las clases pertinentes para [cada uno de estos aspectos](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span><span class="sxs-lookup"><span data-stu-id="02af9-300">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="02af9-301">Eliminación de servicios</span><span class="sxs-lookup"><span data-stu-id="02af9-301">Disposal of services</span></span>

<span data-ttu-id="02af9-302">El contenedor llama a `Dispose` para los tipos `IDisposable` que crea.</span><span class="sxs-lookup"><span data-stu-id="02af9-302">The container calls `Dispose` for the `IDisposable` types it creates.</span></span> <span data-ttu-id="02af9-303">Si se agrega una instancia al contenedor por código de usuario, no se elimina automáticamente.</span><span class="sxs-lookup"><span data-stu-id="02af9-303">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

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

::: moniker range="= aspnetcore-1.0"

> [!NOTE]
> <span data-ttu-id="02af9-304">En ASP.NET Core 1.0, el contenedor llama a Dispose en *todos* los objetos `IDisposable`, incluidos aquellos que no había creado.</span><span class="sxs-lookup"><span data-stu-id="02af9-304">In ASP.NET Core 1.0, the container calls dispose on *all* `IDisposable` objects, including those it didn't create.</span></span>

::: moniker-end

## <a name="default-service-container-replacement"></a><span data-ttu-id="02af9-305">Reemplazo del contenedor de servicios predeterminado</span><span class="sxs-lookup"><span data-stu-id="02af9-305">Default service container replacement</span></span>

<span data-ttu-id="02af9-306">El contenedor de servicios integrado está pensado para atender las necesidades del marco de trabajo y de la mayoría de las aplicaciones de consumidor.</span><span class="sxs-lookup"><span data-stu-id="02af9-306">The built-in service container is meant to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="02af9-307">Se recomienda usar el contenedor integrado a menos que se necesite una característica específica no admitida por el contenedor.</span><span class="sxs-lookup"><span data-stu-id="02af9-307">We recommend using the built-in container unless you need a specific feature that it doesn't support.</span></span> <span data-ttu-id="02af9-308">Algunas de las características admitidas en contenedores de terceros no se incluyen en el contenedor integrado:</span><span class="sxs-lookup"><span data-stu-id="02af9-308">Some of the features supported in 3rd party containers not found in the built-in container:</span></span>

* <span data-ttu-id="02af9-309">Inserción de propiedades</span><span class="sxs-lookup"><span data-stu-id="02af9-309">Property injection</span></span>
* <span data-ttu-id="02af9-310">Inserción basada en nombres</span><span class="sxs-lookup"><span data-stu-id="02af9-310">Injection based on name</span></span>
* <span data-ttu-id="02af9-311">Contenedores secundarios</span><span class="sxs-lookup"><span data-stu-id="02af9-311">Child containers</span></span>
* <span data-ttu-id="02af9-312">Administración personalizada del ciclo de vida</span><span class="sxs-lookup"><span data-stu-id="02af9-312">Custom lifetime management</span></span>
* <span data-ttu-id="02af9-313">Compatibilidad con `Func<T>` para la inicialización diferida</span><span class="sxs-lookup"><span data-stu-id="02af9-313">`Func<T>` support for lazy initialization</span></span>

<span data-ttu-id="02af9-314">Vea el [archivo readme.md de inserción de dependencias](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) para obtener una lista de algunos de los contenedores que admiten adaptadores.</span><span class="sxs-lookup"><span data-stu-id="02af9-314">See the [Dependency Injection readme.md file](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) for a list of some of the containers that support adapters.</span></span>

<span data-ttu-id="02af9-315">En el ejemplo siguiente se reemplaza el contenedor integrado por [Autofac](https://autofac.org/):</span><span class="sxs-lookup"><span data-stu-id="02af9-315">The following sample replaces the built-in container with [Autofac](https://autofac.org/):</span></span>

* <span data-ttu-id="02af9-316">Instale los paquetes de contenedor adecuados:</span><span class="sxs-lookup"><span data-stu-id="02af9-316">Install the appropriate container package(s):</span></span>

  * [<span data-ttu-id="02af9-317">Autofac</span><span class="sxs-lookup"><span data-stu-id="02af9-317">Autofac</span></span>](https://www.nuget.org/packages/Autofac/)
  * [<span data-ttu-id="02af9-318">Autofac.Extensions.DependencyInjection</span><span class="sxs-lookup"><span data-stu-id="02af9-318">Autofac.Extensions.DependencyInjection</span></span>](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)

* <span data-ttu-id="02af9-319">Configure el contenedor en `Startup.ConfigureServices` y devuelva `IServiceProvider`:</span><span class="sxs-lookup"><span data-stu-id="02af9-319">Configure the container in `Startup.ConfigureServices` and return an `IServiceProvider`:</span></span>

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

    <span data-ttu-id="02af9-320">Para usar un contenedor de terceros, `Startup.ConfigureServices` debe devolver `IServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="02af9-320">To use a 3rd party container, `Startup.ConfigureServices` must return `IServiceProvider`.</span></span>

* <span data-ttu-id="02af9-321">Configure Autofac en `DefaultModule`:</span><span class="sxs-lookup"><span data-stu-id="02af9-321">Configure Autofac in `DefaultModule`:</span></span>

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

<span data-ttu-id="02af9-322">En tiempo de ejecución, se usa Autofac para resolver tipos e insertar dependencias.</span><span class="sxs-lookup"><span data-stu-id="02af9-322">At runtime, Autofac is used to resolve types and inject dependencies.</span></span> <span data-ttu-id="02af9-323">Para más información sobre el uso de Autofac con ASP.NET Core, vea la [documentación sobre Autofac](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span><span class="sxs-lookup"><span data-stu-id="02af9-323">To learn more about using Autofac with ASP.NET Core, see the [Autofac documentation](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="02af9-324">Seguridad para subprocesos</span><span class="sxs-lookup"><span data-stu-id="02af9-324">Thread safety</span></span>

<span data-ttu-id="02af9-325">Los servicios de singleton deben ser seguros para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="02af9-325">Singleton services need to be thread safe.</span></span> <span data-ttu-id="02af9-326">Si un servicio de singleton tiene una dependencia en un servicio transitorio, es posible que este también deba ser seguro para subprocesos, según cómo lo use el singleton.</span><span class="sxs-lookup"><span data-stu-id="02af9-326">If a singleton service has a dependency on a transient service, the transient service may also need to be thread safe depending how it's used by the singleton.</span></span>

<span data-ttu-id="02af9-327">El patrón de diseño Factory Method de un servicio único, como el segundo argumento para [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider,TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), no necesita ser seguro para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="02af9-327">The factory method of single service, such as the second argument to [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider,TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), doesn't need to be thread-safe.</span></span> <span data-ttu-id="02af9-328">Al igual que un constructor de tipos (`static`), se garantiza que se le llame una vez mediante un único subproceso.</span><span class="sxs-lookup"><span data-stu-id="02af9-328">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="02af9-329">Recomendaciones</span><span class="sxs-lookup"><span data-stu-id="02af9-329">Recommendations</span></span>

* <span data-ttu-id="02af9-330">No se admite la resolución de servicio basada en `async/await` y `Task`.</span><span class="sxs-lookup"><span data-stu-id="02af9-330">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="02af9-331">C# no admite los constructores asincrónicos, por lo que el patrón recomendado es usar métodos asincrónicos después de resolver el servicio de manera sincrónica.</span><span class="sxs-lookup"><span data-stu-id="02af9-331">C# does not support asynchronous constructors, therefore the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="02af9-332">Evite almacenar datos y configuraciones directamente en el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="02af9-332">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="02af9-333">Por ejemplo, el carro de la compra de un usuario no debería agregarse al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="02af9-333">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="02af9-334">La configuración debe usar el [patrón de opciones](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="02af9-334">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="02af9-335">Del mismo modo, evite los objetos de tipo "contenedor de datos" que solo existen para permitir el acceso a otro objeto.</span><span class="sxs-lookup"><span data-stu-id="02af9-335">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="02af9-336">Es mejor solicitar el elemento real que se necesita mediante la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="02af9-336">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="02af9-337">Evite el acceso estático a servicios (por ejemplo, escribiendo de forma estática [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) para usarlo en otro lugar).</span><span class="sxs-lookup"><span data-stu-id="02af9-337">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) for use elsewhere).</span></span>

* <span data-ttu-id="02af9-338">Evite el uso del *patrón del localizador de servicios*.</span><span class="sxs-lookup"><span data-stu-id="02af9-338">Avoid using the *service locator pattern*.</span></span> <span data-ttu-id="02af9-339">Por ejemplo, no invoque a <xref:System.IServiceProvider.GetService*> para obtener una instancia de servicio si puede usar la inserción de dependencias en su lugar.</span><span class="sxs-lookup"><span data-stu-id="02af9-339">For example, don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead.</span></span> <span data-ttu-id="02af9-340">Otra variación del localizador de servicios que se debe evitar es insertar una fábrica que resuelva dependencias en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="02af9-340">Another service locator variation to avoid is injecting a factory that resolves dependencies at runtime.</span></span> <span data-ttu-id="02af9-341">Estas dos prácticas combinan estrategias de [Inversión de control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion).</span><span class="sxs-lookup"><span data-stu-id="02af9-341">Both of these practices mix [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

* <span data-ttu-id="02af9-342">Evite el acceso estático a `HttpContext` (por ejemplo, [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).</span><span class="sxs-lookup"><span data-stu-id="02af9-342">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).</span></span>

<span data-ttu-id="02af9-343">Al igual que sucede con todas las recomendaciones, podría verse en una situación que le obligue a ignorar alguna de ellas.</span><span class="sxs-lookup"><span data-stu-id="02af9-343">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="02af9-344">Las excepciones son poco frecuentes; principalmente en casos especiales dentro del propio marco de trabajo.</span><span class="sxs-lookup"><span data-stu-id="02af9-344">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="02af9-345">La inserción de dependencias es una *alternativa* a los patrones de acceso a objetos estáticos o globales.</span><span class="sxs-lookup"><span data-stu-id="02af9-345">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="02af9-346">No podrá aprovechar las ventajas de la inserción de dependencias si la combina con el acceso a objetos estáticos.</span><span class="sxs-lookup"><span data-stu-id="02af9-346">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="02af9-347">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="02af9-347">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="02af9-348">Escritura de código limpio en ASP.NET Core con inserción de dependencias (MSDN)</span><span class="sxs-lookup"><span data-stu-id="02af9-348">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="02af9-349">Principio de dependencias explícitas</span><span class="sxs-lookup"><span data-stu-id="02af9-349">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [<span data-ttu-id="02af9-350">Los contenedores de inversión de control y el patrón de inserción de dependencias (Martin Fowler)</span><span class="sxs-lookup"><span data-stu-id="02af9-350">Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
* [<span data-ttu-id="02af9-351">Cómo registrar un servicio con varias interfaces de inserción de dependencias de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="02af9-351">How to register a service with multiple interfaces in ASP.NET Core DI</span></span>](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)
