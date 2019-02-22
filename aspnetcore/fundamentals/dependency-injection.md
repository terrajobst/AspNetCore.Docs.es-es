---
title: Inserción de dependencias en ASP.NET Core
author: guardrex
description: Obtenga información sobre la manera en que ASP.NET Core implementa la inserción de dependencias y cómo se usa.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: fundamentals/dependency-injection
ms.openlocfilehash: 5e5b9746da9bbc13a147b807aabfd3d9ab90a0ca
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410513"
---
# <a name="dependency-injection-in-aspnet-core"></a>Inserción de dependencias en ASP.NET Core

Por [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com) y [Luke Latham](https://github.com/guardrex)

ASP.NET Core admite el patrón de diseño de software de inserción de dependencias (DI), que es una técnica para conseguir la [inversión de control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) entre clases y sus dependencias.

Para más información específica sobre la inserción de dependencias en los controladores MVC, vea <xref:mvc/controllers/dependency-injection>.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="overview-of-dependency-injection"></a>Información general sobre la inserción de dependencias

Una *dependencia* es cualquier objeto requerido por otro objeto. Examine la siguiente clase `MyDependency` con un método `WriteMessage` del que dependen otras clases de una aplicación:

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

Se puede crear una instancia de la clase `MyDependency` para hacer que el método `WriteMessage` esté disponible para una clase. La clase `MyDependency` es una dependencia de la clase `IndexModel`:

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

Se puede crear una instancia de la clase `MyDependency` para hacer que el método `WriteMessage` esté disponible para una clase. La clase `MyDependency` es una dependencia de la clase `HomeController`:

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

La clase crea y depende directamente de la instancia `MyDependency`. Las dependencias de código (como en el ejemplo anterior) son problemáticas y deben evitarse por las siguientes razones:

* Para reemplazar `MyDependency` con una implementación diferente, se debe modificar la clase.
* Si `MyDependency` tiene dependencias, deben configurarse según la clase. En un proyecto grande con varias clases que dependen de `MyDependency`, el código de configuración se dispersa por la aplicación.
* Esta implementación es difícil para realizar pruebas unitarias. La aplicación debe usar una clase `MyDependency` como boceto o código auxiliar, que no es posible con este enfoque.

La inserción de dependencias aborda estos problemas mediante:

* El uso de una interfaz para abstraer la implementación de dependencias.
* Registro de la dependencia en un contenedor de servicios. ASP.NET Core proporciona un contenedor de servicios integrado, [IServiceProvider](/dotnet/api/system.iserviceprovider). Los servicios se registran en el método `Startup.ConfigureServices` de la aplicación.
* *Inserción* del servicio en el constructor de la clase en la que se usa. El marco de trabajo asume la responsabilidad de crear una instancia de la dependencia y de desecharla cuando ya no es necesaria.

En la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), la interfaz `IMyDependency` define un método que el servicio proporciona a la aplicación:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

Esta interfaz se implementa mediante un tipo concreto, `MyDependency`:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

`MyDependency` solicita una instancia de [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) en su constructor. No es raro usar la inserción de dependencias de forma encadenada. Cada dependencia solicitada a su vez solicita sus propias dependencias. El contenedor resuelve las dependencias del gráfico y devuelve el servicio totalmente resuelto. El conjunto colectivo de dependencias que deben resolverse suele denominarse *árbol de dependencias*, *gráfico de dependencias* o *gráfico de objetos*.

`IMyDependency` y `ILogger<TCategoryName>` deben estar registrados en el contenedor de servicios. `IMyDependency` está registrado en `Startup.ConfigureServices`. `ILogger<TCategoryName>` está registrado en la infraestructura de abstracciones de registros, por lo que se trata de un [servicio proporcionado por el marco de trabajo](#framework-provided-services) registrado de forma predeterminada por el marco de trabajo.

En la aplicación de ejemplo, el servicio `IMyDependency` está registrado con el tipo concreto `MyDependency`. El registro abarca la duración del servicio como la duración de una única solicitud. Las [duraciones del servicio](#service-lifetimes) se describen más adelante en este tema.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

> [!NOTE]
> Cada método de extensión `services.Add{SERVICE_NAME}` agrega servicios (y potencialmente los configura). Por ejemplo, `services.AddMvc()` agrega los servicios que Razor Pages y MVC requieren. Se recomienda que las aplicaciones sigan esta convención. Coloque los métodos de extensión en el espacio de nombres [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) para encapsular grupos de registros del servicio.

Si el constructor de un servicio requiere un primitivo, como `string`, se puede insertar mediante la [configuración](xref:fundamentals/configuration/index) o el [patrón de opciones](xref:fundamentals/configuration/options).

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

Se solicita una instancia del servicio mediante el constructor de una clase, en la que se usa el servicio y se asigna a un campo privado. El campo de utiliza para acceder al servicio, según sea necesario en la clase.

En la aplicación de ejemplo, la instancia `IMyDependency` se solicita y usa para llamar al método `WriteMessage` del servicio:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/MyDependencyController.cs?name=snippet1&highlight=3,5-8,13-14)]

::: moniker-end

## <a name="framework-provided-services"></a>Servicios proporcionados por el marco de trabajo

El método `Startup.ConfigureServices` se encarga de definir los servicios que la aplicación usa, incluidas las características de plataforma como Entity Framework Core y ASP.NET Core MVC. Inicialmente, el valor `IServiceCollection` proporcionado a `ConfigureServices` tiene los siguientes servicios definidos (en función de [cómo se configurara el host](xref:fundamentals/host/index)):

| Tipo de servicio | Período de duración |
| ------------ | -------- |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | Transitorio |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | Singleton |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartup](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | Transitorio |
| [Microsoft.AspNetCore.Hosting.Server.IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | Singleton |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | Transitorio |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](/dotnet/api/microsoft.extensions.logging.ilogger) | Singleton |
| [Microsoft.Extensions.Logging.ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | Singleton |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | Singleton |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | Transitorio |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.ioptions-1) | Singleton |
| [System.Diagnostics.DiagnosticSource](/dotnet/core/api/system.diagnostics.diagnosticsource) | Singleton |
| [System.Diagnostics.DiagnosticListener](/dotnet/core/api/system.diagnostics.diagnosticlistener) | Singleton |

Cuando un método de extensión de la colección de servicio está disponible para registrar un servicio (y sus servicios dependientes, si es necesario), la convención consiste en usar un solo método de extensión `Add{SERVICE_NAME}` para registrar todos los servicios requeridos por dicho servicio. El código siguiente es un ejemplo de cómo agregar servicios adicionales al contenedor mediante los métodos de extensión [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity) y [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc):

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

Para más información, vea la [clase ServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection) en la documentación de la API.

## <a name="service-lifetimes"></a>Duraciones de servicios

Elija una duración adecuada para cada servicio registrado. Los servicios de ASP.NET Core pueden configurarse con las duraciones siguientes:

**Transitoria**

Los servicios de duración transitoria se crean cada vez que solicitan. Esta duración funciona mejor para servicios sin estado ligeros.

**Con ámbito**

Los servicios de duración con ámbito se crean una vez por solicitud.

> [!WARNING]
> Si usa un servicio con ámbito en un middleware, inserte el servicio en el método `Invoke` o `InvokeAsync`. No lo inserte a través de la inserción de constructores, porque ello hace que el servicio se comporte como un singleton. Para obtener más información, vea <xref:fundamentals/middleware/index>.

**Singleton**

Los servicios con duración Singleton se crean la primera vez que se solicitan, o bien cuando se ejecuta `ConfigureServices` y se especifica una instancia con el registro del servicio. Cada solicitud posterior usa la misma instancia. Si la aplicación requiere un comportamiento de singleton, se recomienda permitir que el contenedor de servicios administre la duración del servicio. No implemente el patrón de diseño de singleton y proporcione el código de usuario para administrar la duración del objeto en la clase.

> [!WARNING]
> Es peligroso resolver un servicio con ámbito desde un singleton. Puede dar lugar a que el servicio adopte un estado incorrecto al procesar solicitudes posteriores.

### <a name="constructor-injection-behavior"></a>Comportamiento de inserción de constructor

Los servicios se pueden resolver mediante dos mecanismos:

* `IServiceProvider`
* [ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; Permite la creación de objetos sin registrar el servicio en el contenedor de inserción de dependencias. `ActivatorUtilities` se utiliza con abstracciones orientadas al usuario, como asistentes de etiquetas, controladores MVC y enlazadores de modelos.

Los constructores pueden aceptar argumentos que no se proporcionan mediante la inserción de dependencias, pero los argumentos deben asignar valores predeterminados.

Cuando se resuelven los servicios mediante `IServiceProvider` o `ActivatorUtilities`, la inserción del constructor requiere un constructor *público*.

Cuando se resuelven los servicios mediante `ActivatorUtilities`, la inserción del constructor requiere que exista solo un constructor aplicable. Se admiten las sobrecargas de constructor, pero solo puede existir una sobrecarga cuyos argumentos pueda cumplir la inserción de dependencias.

## <a name="entity-framework-contexts"></a>Contextos de Entity Framework

Es necesario agregar contextos de Entity Framework al contenedor de servicios mediante la duración con ámbito. Esto se controla automáticamente con una llamada al método [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) al registrar el contexto de la base de datos. Los servicios que usan el contexto de la base de datos deben usar también la duración con ámbito.

## <a name="lifetime-and-registration-options"></a>Opciones de registro y duración

Para mostrar la diferencia entre la duración y las opciones de registro, considere las siguientes interfaces que representan tareas como una operación con un identificador único, `OperationId`. Según cómo esté configurada la duración de un servicio de operaciones para las interfaces siguientes, el contenedor proporciona la misma instancia del servicio u otra distinta cuando así lo solicita la clase:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

Las interfaces se implementan en la clase `Operation`. El constructor `Operation` genera un GUID en caso de que no se proporcione uno:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

Se registra una instancia de `OperationService` que depende de cada uno de los demás tipos `Operation`. Cuando `OperationService` se solicita con la inserción de dependencias, recibe una instancia nueva de cada servicio o una instancia existente en función de la duración de los servicios dependientes.

* Si se crean servicios transitorios cuando se solicitan, el elemento `OperationId` del servicio `IOperationTransient` varía del objeto `OperationId` de `OperationService`. `OperationService` recibe una instancia nueva de la clase `IOperationTransient`. La nueva instancia produce un objeto `OperationId` diferente.
* Si se crean servicios con ámbito por cada solicitud, el objeto `OperationId` del servicio `IOperationScoped` es el mismo que para `OperationService` dentro de la solicitud. Entre las solicitudes, ambos servicios comparten un valor `OperationId` diferente.
* Si se crean servicios singleton y servicios de instancia singleton una vez y se usan en todas las solicitudes y servicios, el objeto `OperationId` es constante en todas las solicitudes de servicio.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

En `Startup.ConfigureServices`, cada tipo se agrega al contenedor según su duración con nombre:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=12-15,18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

El servicio `IOperationSingletonInstance` usa una instancia específica con un identificador conocido de `Guid.Empty`. Resulta evidente identificar cuándo este tipo está en uso (su GUID es todo ceros).

::: moniker range=">= aspnetcore-2.1"

La aplicación de ejemplo muestra las duraciones de los objetos dentro y entre las solicitudes individuales. El objeto `IndexModel` de la aplicación de ejemplo solicita cada tipo de `IOperation` y `OperationService`. Después, la página muestra todos los valores `OperationId` del servicio y la clase del modelo de página mediante las asignaciones de propiedades:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

La aplicación de ejemplo muestra las duraciones de los objetos dentro y entre las solicitudes individuales. La aplicación de ejemplo incluye un valor `OperationsController` que solicita cada tipo de `IOperation` y `OperationService`. La acción `Index` establece el servicio en `ViewBag` para mostrar los valores `OperationId` del servicio:

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/OperationsController.cs?name=snippet1)]

::: moniker-end

Las dos salidas siguientes muestran el resultado de dos solicitudes:

**Primera solicitud:**

Operaciones del controlador:

Transient: d233e165-f417-469b-a866-1cf1935d2518  
Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Instance: 00000000-0000-0000-0000-000000000000

Operaciones `OperationService`:

Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64  
Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Instance: 00000000-0000-0000-0000-000000000000

**Segunda solicitud:**

Operaciones del controlador:

Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0  
Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Instance: 00000000-0000-0000-0000-000000000000

Operaciones `OperationService`:

Transitorio: c4cbacb8-36a2-436d-81c8-8c1b78808aaf  
Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Instance: 00000000-0000-0000-0000-000000000000

Observe cuál de los valores `OperationId` varía dentro de una solicitud y entre solicitudes:

* Los objetos *Transient* siempre son diferentes. Tenga en cuenta que el valor `OperationId` transitorio de la primera y segunda solicitud varía tanto para las operaciones `OperationService` como entre solicitudes. Se proporciona una nueva instancia para cada servicio y solicitud.
* Los objetos *con ámbito* son iguales dentro de una solicitud, pero varían entre solicitudes.
* Los objetos *singleton* son iguales para todos los objetos y solicitudes, independientemente de si se proporciona una instancia `Operation` en `ConfigureServices`.

## <a name="call-services-from-main"></a>Llamada a servicios desde main

Cree un [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) con [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) para resolver un servicio con ámbito dentro del ámbito de la aplicación. Este método resulta útil para tener acceso a un servicio con ámbito durante el inicio para realizar tareas de inicialización. En el siguiente ejemplo se indica cómo obtener un contexto para `MyScopedService` en `Program.Main`:

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

## <a name="scope-validation"></a>Validación del ámbito

::: moniker range=">= aspnetcore-2.0"

Cuando la aplicación se ejecuta en el entorno de desarrollo, el proveedor de servicios predeterminado realiza comprobaciones para confirmar lo siguiente:

* Los servicios con ámbito no se resuelven directa o indirectamente desde el proveedor de servicios raíz.
* Los servicios con ámbito no se insertan directa o indirectamente en singletons.

::: moniker-end

El proveedor de servicios raíz se crea cuando se llama a [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider). La vigencia del proveedor de servicios raíz es la misma que la de la aplicación o el servidor cuando el proveedor se inicia con la aplicación, y se elimina cuando la aplicación se cierra.

De la eliminación de los servicios con ámbito se encarga el contenedor que los creó. Si un servicio con ámbito se crea en el contenedor raíz, su vigencia sube a la del singleton, ya que solo lo puede eliminar el contenedor raíz cuando la aplicación o el servidor se cierran. Al validar los ámbitos de servicio, este tipo de situaciones se detectan cuando se llama a `BuildServiceProvider`.

Para obtener más información, vea <xref:fundamentals/host/web-host#scope-validation>.

## <a name="request-services"></a>Servicios de solicitud

Los servicios disponibles en una solicitud de ASP.NET Core desde `HttpContext` se exponen mediante la colección [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices).

Los servicios de solicitud representan los servicios configurados y solicitados como parte de la aplicación. Cuando los objetos especifican dependencias, estas se cumplen mediante los tipos que se encuentran en `RequestServices`, no en `ApplicationServices`.

Por lo general, la aplicación no debe usar estas propiedades directamente. En su lugar, solicite los tipos que las clases requieren mediante el constructor de clases y permita que el marco de trabajo inserte las dependencias. Esto produce clases que son más fáciles de probar.

> [!NOTE]
> Se recomienda que solicite las dependencias como parámetros del constructor para obtener acceso a la colección `RequestServices`.

## <a name="design-services-for-dependency-injection"></a>Diseño de servicios para la inserción de dependencias

Los procedimientos recomendados son:

* Diseñar servicios para usar la inserción de dependencias a fin de obtener sus dependencias.
* Evite las llamadas de método estático y con estado.
* Evitar la creación directa de instancias de clases dependientes dentro de los servicios. La creación directa de instancias se acopla al código de una implementación particular.
* Cree clases de aplicación pequeñas, bien factorizadas y probadas con facilidad.

Si una clase parece tener demasiadas dependencias insertadas, suele indicar que la clase tiene demasiadas responsabilidades y que esto infringe el [principio de responsabilidad única (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility). Trate de mover algunas de las responsabilidades de la clase a una nueva para intentar refactorizarla. Tenga en cuenta que las clases del modelo de página de Razor Pages y las clases de controlador MVC deben centrarse en aspectos de la interfaz de usuario. Los detalles de implementación de las reglas de negocio y del acceso a datos se deben mantener en las clases pertinentes para [cada uno de estos aspectos](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).

### <a name="disposal-of-services"></a>Eliminación de servicios

El contenedor llama a `Dispose` para los tipos `IDisposable` que crea. Si se agrega una instancia al contenedor por código de usuario, no se elimina automáticamente.

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
> En ASP.NET Core 1.0, el contenedor llama a Dispose en *todos* los objetos `IDisposable`, incluidos aquellos que no había creado.

::: moniker-end

## <a name="default-service-container-replacement"></a>Reemplazo del contenedor de servicios predeterminado

El contenedor de servicios integrado está pensado para atender las necesidades del marco de trabajo y de la mayoría de las aplicaciones de consumidor. Se recomienda usar el contenedor integrado a menos que se necesite una característica específica no admitida por el contenedor. Algunas de las características admitidas en contenedores de terceros no se incluyen en el contenedor integrado:

* Inserción de propiedades
* Inserción basada en nombres
* Contenedores secundarios
* Administración personalizada del ciclo de vida
* Compatibilidad con `Func<T>` para la inicialización diferida

Vea el [archivo readme.md de inserción de dependencias](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) para obtener una lista de algunos de los contenedores que admiten adaptadores.

En el ejemplo siguiente se reemplaza el contenedor integrado por [Autofac](https://autofac.org/):

* Instale los paquetes de contenedor adecuados:

  * [Autofac](https://www.nuget.org/packages/Autofac/)
  * [Autofac.Extensions.DependencyInjection](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)

* Configure el contenedor en `Startup.ConfigureServices` y devuelva `IServiceProvider`:

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

    Para usar un contenedor de terceros, `Startup.ConfigureServices` debe devolver `IServiceProvider`.

* Configure Autofac en `DefaultModule`:

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

En tiempo de ejecución, se usa Autofac para resolver tipos e insertar dependencias. Para más información sobre el uso de Autofac con ASP.NET Core, vea la [documentación sobre Autofac](https://docs.autofac.org/en/latest/integration/aspnetcore.html).

### <a name="thread-safety"></a>Seguridad para subprocesos

Los servicios de singleton deben ser seguros para subprocesos. Si un servicio de singleton tiene una dependencia en un servicio transitorio, es posible que este también deba ser seguro para subprocesos, según cómo lo use el singleton.

El patrón de diseño Factory Method de un servicio único, como el segundo argumento para [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider,TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), no necesita ser seguro para subprocesos. Al igual que un constructor de tipos (`static`), se garantiza que se le llame una vez mediante un único subproceso.

## <a name="recommendations"></a>Recomendaciones

* No se admite la resolución de servicio basada en `async/await` y `Task`. C# no admite los constructores asincrónicos, por lo que el patrón recomendado es usar métodos asincrónicos después de resolver el servicio de manera sincrónica.

* Evite almacenar datos y configuraciones directamente en el contenedor de servicios. Por ejemplo, el carro de la compra de un usuario no debería agregarse al contenedor de servicios. La configuración debe usar el [patrón de opciones](xref:fundamentals/configuration/options). Del mismo modo, evite los objetos de tipo "contenedor de datos" que solo existen para permitir el acceso a otro objeto. Es mejor solicitar el elemento real que se necesita mediante la inserción de dependencias.

* Evite el acceso estático a servicios (por ejemplo, escribiendo de forma estática [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) para usarlo en otro lugar).

* Evite el uso del *patrón del localizador de servicios*. Por ejemplo, no invoque a <xref:System.IServiceProvider.GetService*> para obtener una instancia de servicio si puede usar la inserción de dependencias en su lugar. Otra variación del localizador de servicios que se debe evitar es insertar una fábrica que resuelva dependencias en tiempo de ejecución. Estas dos prácticas combinan estrategias de [Inversión de control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion).

* Evite el acceso estático a `HttpContext` (por ejemplo, [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).

Al igual que sucede con todas las recomendaciones, podría verse en una situación que le obligue a ignorar alguna de ellas. Las excepciones son poco frecuentes; principalmente en casos especiales dentro del propio marco de trabajo.

La inserción de dependencias es una *alternativa* a los patrones de acceso a objetos estáticos o globales. No podrá aprovechar las ventajas de la inserción de dependencias si la combina con el acceso a objetos estáticos.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [Escritura de código limpio en ASP.NET Core con inserción de dependencias (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [Container-Managed Application Design, Prelude: Where does the Container Belong?](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/) (Diseño de aplicaciones administradas por contenedor, preludio: ¿a qué lugar pertenece el contenedor?)
* [Principio de dependencias explícitas](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [Los contenedores de inversión de control y el patrón de inserción de dependencias (Martin Fowler)](https://www.martinfowler.com/articles/injection.html)
* [Cómo registrar un servicio con varias interfaces de inserción de dependencias de ASP.NET Core](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)
