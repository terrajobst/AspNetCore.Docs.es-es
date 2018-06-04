---
title: Inserción de dependencias en ASP.NET Core
author: ardalis
description: Obtenga información sobre la manera en que ASP.NET Core implementa la inserción de dependencias y cómo se usa.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/dependency-injection
ms.openlocfilehash: 14c3d464773fe78a563a27776bfcd124c22df134
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "34566963"
---
# <a name="dependency-injection-in-aspnet-core"></a>Inserción de dependencias en ASP.NET Core

<a name="fundamentals-dependency-injection"></a>

Por [Steve Smith](https://ardalis.com/) y [Scott Addie](https://scottaddie.com)

ASP.NET Core está diseñado desde el principio para admitir y aprovechar la inserción de dependencias. Las aplicaciones ASP.NET Core pueden aprovechar los servicios del marco de trabajo integrado si los insertan en métodos de la clase Startup, y los servicios de las aplicaciones también pueden configurarse para la inserción. El contenedor de servicios predeterminados proporcionado por ASP.NET Core incluye un conjunto de característica mínimas y no está diseñado para reemplazar otros contenedores.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-dependency-injection"></a>¿Qué es la inserción de dependencias?

La inserción de dependencias (DI) es una técnica para lograr un acoplamiento flexible entre los objetos y sus colaboradores (o dependencias). En lugar de crear directamente instancias de colaboradores o de usar referencias estáticas, los objetos que una clase necesita para llevar a cabo sus acciones se proporcionan de algún modo a dicha clase. A menudo, las clases declaran sus dependencias a través de su constructor, lo que les permite seguir el [principio de dependencias explícitas](http://deviq.com/explicit-dependencies-principle/). Este método se conoce como "inserción de constructor".

Cuando las clases se diseñan con DI en mente, se acoplan de manera más flexible porque no tienen dependencias directas codificadas de forma rígida en sus colaboradores. Esto sigue el [principio de inversión de dependencias](http://deviq.com/dependency-inversion-principle/), que afirma que *"los módulos de nivel alto no deben depender de los módulos de nivel bajo; ambos deben depender de abstracciones"*. En lugar de hacer referencia a implementaciones específicas, las clases solicitan abstracciones (normalmente `interfaces`) que se les proporcionan cuando se construye la clase. La extracción de dependencias en interfaces y el abastecimiento de implementaciones de estas interfaces como parámetros son también ejemplos del [modelo de diseño de estrategias](http://deviq.com/strategy-design-pattern/).

Cuando se diseña un sistema para el uso de DI, con muchas clases que solicitan sus dependencias a través de su constructor (o propiedades), resulta útil tener una clase dedicada a la creación de estas clases con sus dependencias asociadas. Estas clases se conocen como *contenedores* o, más concretamente, contenedores de [inversión de control (IoC)](http://deviq.com/inversion-of-control/) o contenedores de inserción de dependencias (DI). Un contenedor es básicamente un generador que se encarga de proporcionar las instancias de tipos que se le solicitan. Si un tipo determinado ha declarado que tiene dependencias y el contenedor se ha configurado para proporcionar tipos de dependencia, creará las dependencias como parte del proceso de creación de la instancia solicitada. De esta manera, se pueden proporcionar gráficos de dependencias complejos a las clases sin necesidad de construir objetos codificados de forma rígida. Además de crear objetos con sus dependencias, los contenedores suelen administrar la duración de los objetos dentro de la aplicación.

ASP.NET Core incluye un simple contenedor integrado (representado por la interfaz `IServiceProvider`) que admite la inserción de constructor de forma predeterminada, y ASP.NET hace que determinados servicios estén disponibles a través de DI. El contenedor de ASP.NET hace referencia a los tipos que administra como *servicios*. De ahora en adelante en este artículo, el término *servicios* hará referencia a los tipos administrados por el contenedor de IoC de ASP.NET Core. Los servicios del contenedor integrado se configuran en el método `ConfigureServices` de la clase `Startup` de la aplicación.

> [!NOTE]
> Martin Fowler ha escrito un amplio artículo sobre [los contenedores de inversión de control y el patrón de inserción de dependencias](https://www.martinfowler.com/articles/injection.html). En los Modelos y prácticas de Microsoft también encontrará una descripción excelente de la [inserción de dependencias](https://msdn.microsoft.com/library/hh323705.aspx).

> [!NOTE]
> Este artículo trata sobre la inserción de dependencias tal como se aplica a todas las aplicaciones ASP.NET. La inserción de dependencias dentro de controladores de MVC se trata en [Inserción de dependencias y controladores](../mvc/controllers/dependency-injection.md).

### <a name="constructor-injection-behavior"></a>Comportamiento de inserción de constructor

La inserción de constructor requiere que el constructor en cuestión sea *público*. En caso contrario, la aplicación producirá una `InvalidOperationException`:

> A suitable constructor for type 'YourType' couldn't be located. Ensure the type is concrete and services are registered for all parameters of a public constructor. (No se pudo encontrar un constructor adecuado para el tipo "SuTipo". Asegúrese de que el tipo sea concreto y de que los servicios estén registrados para todos los parámetros de un constructor público.)

La inserción de constructor requiere que solo exista un constructor aplicable. Se admiten las sobrecargas de constructor, pero solo puede existir una sobrecarga cuyos argumentos pueda cumplir la inserción de dependencias. Si existe más de una, la aplicación producirá una `InvalidOperationException`:

> Multiple constructors accepting all given argument types have been found in type 'YourType'. Multiple constructors accepting all given argument types have been found in type 'YourType'. There should only be one applicable constructor. (Se han encontrado en el tipo "SuTipo" varios constructores que aceptan todos los tipos de argumento especificados. Solo debe haber un constructor aplicable.)

Los constructores pueden aceptar argumentos que no se proporcionan mediante la inserción de dependencias, pero deben admitir valores predeterminados. Por ejemplo:

```csharp
// throws InvalidOperationException: Unable to resolve service for type 'System.String'...
public CharactersController(ICharacterRepository characterRepository, string title)
{
    _characterRepository = characterRepository;
    _title = title;
}

// runs without error
public CharactersController(ICharacterRepository characterRepository, string title = "Characters")
{
    _characterRepository = characterRepository;
    _title = title;
}
```

## <a name="using-framework-provided-services"></a>Uso de servicios proporcionados por el marco de trabajo

El método `ConfigureServices` de la clase `Startup` se encarga de definir los servicios que usará la aplicación, incluidas las características de plataforma como Entity Framework Core y ASP.NET Core MVC. Inicialmente, el valor `IServiceCollection` proporcionado a `ConfigureServices` tiene los siguientes servicios definidos (en función de [cómo se configurara el host](xref:fundamentals/host/index)):

| Tipo de servicio | Período de duración |
| ----- | ------- |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | Singleton |
| [Microsoft.Extensions.Logging.ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | Singleton |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](/dotnet/api/microsoft.extensions.logging.ilogger) | Singleton |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | Transitorio |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | Transitorio |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.ioptions-1) | Singleton |
| [System.Diagnostics.DiagnosticSource](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | Singleton |
| [System.Diagnostics.DiagnosticListener](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | Transitorio |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | Singleton |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | Transitorio |
| [Microsoft.AspNetCore.Hosting.Server.IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartup](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | Singleton |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | Singleton |

A continuación se muestra un ejemplo de cómo agregar servicios adicionales al contenedor mediante una serie de métodos de extensión como `AddDbContext`, `AddIdentity` y `AddMvc`.

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

Las características y el software intermedio proporcionados por ASP.NET, como MVC, siguen la convención de usar un solo método de extensión Add*NombreDelServicio* para registrar todos los servicios requeridos por esa característica.

> [!TIP]
> Puede solicitar determinados servicios proporcionados por el marco de trabajo dentro de métodos `Startup` mediante sus listas de parámetros. Vea [Inicio de la aplicación](startup.md) para obtener más detalles.

## <a name="registering-services"></a>Registrar servicios

Puede registrar sus propios servicios de aplicación de la manera siguiente. El primer tipo genérico representa el tipo (normalmente una interfaz) que se solicitará desde el contenedor. El segundo tipo genérico representa el tipo concreto del cual creará una instancia el contenedor y que se usará para responder a dichas solicitudes.

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> Cada método de extensión `services.Add<ServiceName>` agrega servicios (y potencialmente los configura). Por ejemplo, `services.AddMvc()` agrega los servicios que requiere MVC. Se recomienda que siga esta convención y coloque los métodos de extensión en el espacio de nombres `Microsoft.Extensions.DependencyInjection` para encapsular grupos de registros del servicio.

El método `AddTransient` se usa para asignar tipos abstractos a servicios concretos de los que se crean instancias por separado para cada objeto que lo requiera. Esto se conoce como la *duración* del servicio. A continuación se describen opciones de duración adicionales. Es importante elegir una duración adecuada para cada uno de los servicios que se registren. ¿Debe proporcionarse una nueva instancia del servicio a cada clase que lo solicite? ¿Debe usarse una instancia en una solicitud web determinada? ¿O debe usarse una sola instancia para la duración de la aplicación?

En el ejemplo incluido en este artículo, hay un controlador simple que muestra los nombres de caracteres, denominado `CharactersController`. Su método `Index` muestra la lista actual de caracteres que se han almacenado en la aplicación e inicializa la colección con unos cuantos caracteres, si no existe ninguno. Tenga en cuenta que, aunque esta aplicación usa Entity Framework Core y la clase `ApplicationDbContext` para su persistencia, ninguno es evidente en el controlador. En su lugar, el mecanismo de acceso a datos específico se ha abstraído detrás de una interfaz, `ICharacterRepository`, que sigue el [modelo de repositorio](http://deviq.com/repository-pattern/). Se solicita una instancia de `ICharacterRepository` mediante el constructor y se asigna a un campo privado, que después se usa para tener acceso a caracteres, según sea necesario.

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

`ICharacterRepository` define los dos métodos que el controlador necesita para funcionar con instancias de `Character`.

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

Esta interfaz la implementa a su vez un tipo concreto, `CharacterRepository`, que se usa en tiempo de ejecución.

> [!NOTE]
> La manera en que se usa la inserción de dependencias con la clase `CharacterRepository` es un modelo general que puede seguir para todos los servicios de aplicación, no solo en "repositorios" o clases de acceso a datos.

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

Tenga en cuenta que `CharacterRepository` solicita `ApplicationDbContext` en su constructor. No es raro que la inserción de dependencias se use encadenada de este modo, donde cada dependencia solicitada solicita a su vez sus propias dependencias. El contenedor se encarga de resolver todas las dependencias del gráfico y devolver el servicio totalmente resuelto.

> [!NOTE]
> El proceso de creación del objeto solicitado, de todos los objetos que necesita y de todos los objetos que estos necesitan suele denominarse *gráfico de objetos*. Del mismo modo, el conjunto colectivo de dependencias que deben resolverse suele denominarse *árbol de dependencias* o *gráfico de dependencias*.

En este caso, es necesario registrar `ICharacterRepository` y `ApplicationDbContext` con el contenedor de servicios de `ConfigureServices` en `Startup`. `ApplicationDbContext` se configura con la llamada al método de extensión `AddDbContext<T>`. En el código siguiente se muestra el registro del tipo `CharacterRepository`.

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

Es necesario agregar contextos de Entity Framework al contenedor de servicios mediante la duración `Scoped`. Esto se lleva a cabo automáticamente si usa los métodos auxiliares, tal como se ha indicado anteriormente. Los repositorios que vayan a usar Entity Framework deben usar la misma duración.

> [!WARNING]
> Debe ser especialmente cauteloso al resolver un servicio `Scoped` desde un singleton. Es probable que en este caso el servicio tenga un estado incorrecto al procesar las solicitudes posteriores.

Los servicios que tengan dependencias deben registrarlas en el contenedor. Si el constructor de un servicio requiere un primitivo, como `string`, se puede insertar mediante la [configuración](xref:fundamentals/configuration/index) y el [patrón de opciones](xref:fundamentals/configuration/options).

## <a name="service-lifetimes-and-registration-options"></a>Duración de los servicios y opciones de registro

Los servicios de ASP.NET pueden configurarse con las duraciones siguientes:

**Transitoria**

Los servicios de duración transitoria se crean cada vez que solicitan. Esta duración funciona mejor para servicios sin estado ligeros.

**Con ámbito**

Los servicios de duración con ámbito se crean una vez por solicitud.

> [!WARNING]
> Si usa un servicio con ámbito en un middleware, inserte el servicio en el método `Invoke` o `InvokeAsync`. No lo inserte a través de la inserción de constructores, porque ello hace que el servicio se comporte como un singleton.

**Singleton**

Los servicios de duración de singleton se crean la primera vez que se solicitan (o cuando se ejecuta `ConfigureServices` si se especifica ahí una instancia) y todas las solicitudes posteriores usan la misma instancia. Si la aplicación requiere un comportamiento de singleton, se recomienda permitir que el contenedor de servicios administre la duración del servicio, en lugar de implementar el patrón de diseño de singleton y administrar por sí mismo la duración del objeto en la clase.

Los servicios se pueden registrar con el contenedor de varias maneras. Ya hemos visto cómo registrar una implementación de servicio con un tipo determinado mediante la especificación del tipo concreto que se va a usar. Además, se puede especificar un generador, que se usará para crear la instancia a petición. El tercer método consiste en especificar directamente la instancia del tipo que se va a usar, en cuyo caso el contenedor nunca intentará crear una instancia (ni la eliminará).

Para mostrar la diferencia entre estas duraciones y opciones de registro, imagine una interfaz simple que representa una o varias tareas como una *operación* con un identificador único, `OperationId`. Según cómo se configure la duración de este servicio, el contenedor proporcionará las mismas instancias o instancias diferentes del servicio a la clase que lo solicita. Para dejar claro qué duración se solicita, crearemos un tipo por opción de duración:

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

Estas interfaces se implementan mediante una sola clase, `Operation`, que acepta un `Guid` en su constructor, o usa un `Guid` nuevo si no se proporciona ninguno.

Después, en `ConfigureServices`, se agrega cada tipo al contenedor según su duración con nombre:

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

Tenga en cuenta que el servicio `IOperationSingletonInstance` usa una instancia específica con un identificador conocido de `Guid.Empty` para que quede claro cuándo se usa este tipo (su GUID contendrá únicamente ceros). También hemos registrado un `OperationService` que depende de cada uno de los otros tipos `Operation`, para que quede claro dentro de una solicitud si este servicio obtiene la misma instancia que el controlador o una nueva para cada tipo de operación. Lo único que hace este servicio es exponer sus dependencias como propiedades, para que se puedan mostrar en la vista.

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

Para mostrar a la aplicación las duraciones de los objetos dentro de las solicitudes individuales independientes y entre estas, el ejemplo incluye un `OperationsController` que solicita cada tipo de `IOperation`, así como un `OperationService`. Después, la acción `Index` muestra todos los valores del controlador y del servicio `OperationId`.

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

Ahora se realizan dos solicitudes diferentes a esta acción del controlador:

![Vista de operaciones de la aplicación web del ejemplo de inserción de dependencias en ejecución en Microsoft Edge, donde se muestran los valores de identificación de operación (es decir, los GUID) para las operaciones del controlador y de OperationService de tipo transitorio, con ámbito, singleton e instancia en la primera solicitud.](dependency-injection/_static/lifetimes_request1.png)

![Vista de operaciones en la que se muestran los valores de identificación de operación para una segunda solicitud.](dependency-injection/_static/lifetimes_request2.png)

Observe cuál de los valores `OperationId` varía dentro de una solicitud y entre solicitudes.

* Los objetos *transitorios* son siempre diferentes, ya que se proporciona una nueva instancia a cada controlador y servicio.

* Los objetos *con ámbito* son iguales dentro de una solicitud, pero varían entre solicitudes.

* Los objetos *singleton* son iguales para todos los objetos y solicitudes (independientemente de si se proporciona una instancia en `ConfigureServices`)

## <a name="resolve-a-scoped-service-within-the-application-scope"></a>Resolver un servicio con ámbito dentro del ámbito de la aplicación

Cree un [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) con [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) para resolver un servicio con ámbito dentro del ámbito de la aplicación. Este método resulta útil para tener acceso a un servicio con ámbito durante el inicio para realizar tareas de inicialización. En el siguiente ejemplo se indica cómo obtener un contexto para `MyScopedService` en `Program.Main`:

```csharp
public static void Main(string[] args)
{
    var host = BuildWebHost(args);

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

Cuando la aplicación se ejecuta en el entorno de desarrollo de ASP.NET Core 2.0 o posterior, el proveedor de servicios predeterminado realiza comprobaciones para confirmar lo siguiente:

* Los servicios con ámbito no se resuelven directa o indirectamente desde el proveedor de servicios raíz.
* Los servicios con ámbito no se insertan directa o indirectamente en singletons.

El proveedor de servicios raíz se crea cuando se llama a [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider). La vigencia del proveedor de servicios raíz es la misma que la de la aplicación o el servidor cuando el proveedor se inicia con la aplicación, y se elimina cuando la aplicación se cierra.

De la eliminación de los servicios con ámbito se encarga el contenedor que los creó. Si un servicio con ámbito se crea en el contenedor raíz, su vigencia sube a la del singleton, ya que solo lo puede eliminar el contenedor raíz cuando la aplicación o el servidor se cierran. Al validar los ámbitos de servicio, este tipo de situaciones se detectan cuando se llama a `BuildServiceProvider`.

Para obtener más información, vea [Validación del ámbito en el tema de host web](xref:fundamentals/host/web-host#scope-validation).

## <a name="request-services"></a>Servicios de solicitud

Los servicios disponibles en una solicitud de ASP.NET desde `HttpContext` se exponen mediante la colección `RequestServices`.

![Cuadro de diálogo contextual de Intellisense de los servicios de solicitud HttpContext en el que se muestra que los servicios de solicitud obtienen o establecen el IServiceProvider que proporciona acceso al contenedor de servicios de la solicitud.](dependency-injection/_static/request-services.png)

Los servicios de solicitud representan los servicios que se configuran y se solicitan como parte de la aplicación. Cuando los objetos especifican dependencias, estas se cumplen mediante los tipos que se encuentran en `RequestServices`, no en `ApplicationServices`.

Por lo general, no debe usar estas propiedades directamente. Se recomienda que solicite los tipos que las clases necesitan mediante el constructor de la clase y que deje que el marco de trabajo inserte estas dependencias. Esto da como resultado clases más fáciles de probar (vea [Pruebas y depuración](xref:test/index)) y acopladas de manera más flexible.

> [!NOTE]
> Se recomienda que solicite las dependencias como parámetros del constructor para obtener acceso a la colección `RequestServices`.

## <a name="designing-services-for-dependency-injection"></a>Diseño de servicios para la inserción de dependencias

Debe diseñar los servicios de modo que usen la inserción de dependencias para obtener sus colaboradores. Esto significa que debe evitar el uso de llamadas a métodos estáticos con estado (lo que ocasiona un problema en el código denominado [adhesión estática](http://deviq.com/static-cling/)) y la creación de instancias directa de clases dependientes dentro de los servicios. Puede resultarle útil recordar la frase "[lo nuevo se pega](https://ardalis.com/new-is-glue)" al decidir si va a crear una instancia de un tipo o solicitarlo mediante la inserción de dependencias. Si sigue los [principios SOLID del diseño orientado a objetos](http://deviq.com/solid/), las clases tenderán de forma natural a ser pequeñas, estarán correctamente factorizadas y podrán probarse fácilmente.

¿Qué ocurre si descubre que en sus clases suelen insertarse demasiadas dependencias? Esto suele indicar que la clase intenta realizar demasiadas acciones y que probablemente infringe el [principio de responsabilidad única](http://deviq.com/single-responsibility-principle/) (SRP). Mueva algunas de las responsabilidades de la clase a una nueva para intentar refactorizarla. Tenga en cuenta que las clases `Controller` deben centrarse en aspectos de la interfaz de usuario, por lo que los detalles de implementación de las reglas de negocio y del acceso a datos se deben mantener en las clases pertinentes para [cada uno de estos aspectos](http://deviq.com/separation-of-concerns/).

En lo que respecta al acceso a datos en concreto, puede insertar `DbContext` en los controladores (siempre y cuando haya agregado EF al contenedor de servicios en `ConfigureServices`). Algunos desarrolladores prefieren usar una interfaz de repositorio para la base de datos en lugar de insertar `DbContext` directamente. Si usa una interfaz para encapsular la lógica de acceso a datos en un solo lugar, puede minimizar el número de lugares que tendrá que cambiar cuando cambie la base de datos.

### <a name="disposing-of-services"></a>Eliminación de servicios

El contenedor llamará a `Dispose` para los tipos `IDisposable` que cree. A pesar de todo, si agrega usted mismo una instancia al contenedor, no se eliminará.

Ejemplo:

```csharp
// Services implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}


public void ConfigureServices(IServiceCollection services)
{
    // container will create the instance(s) of these types and will dispose them
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // container didn't create instance so it will NOT dispose it
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

> [!NOTE]
> En la versión 1.0, el contenedor llamaba a Dispose en *todos* los objetos `IDisposable`, incluidos aquellos que no había creado.

## <a name="replacing-the-default-services-container"></a>Reemplazo del contenedor de servicios predeterminado

El contenedor de servicios integrado está pensado para atender las necesidades básicas del marco de trabajo y de la mayoría de las aplicaciones de consumidor compiladas en él. Aun así, los desarrolladores pueden reemplazar el contenedor integrado por su contenedor preferido. El método `ConfigureServices` suele devolver `void`, pero si se cambia su firma para que devuelva `IServiceProvider`, se puede configurar y devolver un contenedor diferente. Hay muchos contenedores de IoC disponibles para .NET. En este ejemplo, se usa el paquete [Autofac](https://autofac.org/).

En primer lugar, instale los paquetes de contenedor adecuados:

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

Después, configure el contenedor en `ConfigureServices` y devuelva un `IServiceProvider`:

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

> [!NOTE]
> Cuando se usa un contenedor de inserción de dependencias de terceros, debe cambiar `ConfigureServices` para que devuelva `IServiceProvider` en lugar de `void`.

Por último, configure Autofac de la forma habitual en `DefaultModule`:

```csharp
public class DefaultModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
    }
}
```

En tiempo de ejecución, se usará Autofac para resolver tipos e insertar dependencias. [Más información sobre el uso de Autofac y ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html).

### <a name="thread-safety"></a>Seguridad para subprocesos

Los servicios de singleton deben ser seguros para subprocesos. Si un servicio de singleton tiene una dependencia en un servicio transitorio, es posible que este también deba ser seguro para subprocesos, según cómo lo use el singleton.

## <a name="recommendations"></a>Recomendaciones

Cuando trabaje con la inserción de dependencias, tenga en cuenta las recomendaciones siguientes:

* La inserción de dependencias está destinada a objetos que tienen dependencias complejas. Los controladores, los servicios, los adaptadores y los repositorios son ejemplos de objetos que podrían agregarse a la inserción de dependencias.

* Evite almacenar datos y configuraciones directamente en la inserción de dependencias. Por ejemplo, el carro de la compra de un usuario no debería agregarse al contenedor de servicios. La configuración debe usar el [patrón de opciones](xref:fundamentals/configuration/options). Del mismo modo, evite los objetos de tipo "contenedor de datos" que solo existen para permitir el acceso a otro objeto. Es mejor solicitar el elemento real que se necesita mediante la inserción de dependencias, si es posible.

* Evite el acceso estático a los servicios.

* Evite la ubicación de servicios en el código de la aplicación.

* Evite el acceso estático a `HttpContext`.

Al igual que sucede con todas las recomendaciones, podría verse en una situación que le obligue a ignorar alguna de ellas. Por lo que sabemos las excepciones son muy poco frecuentes, ya que suelen ser casos muy especiales del propio marco de trabajo.

La inserción de dependencias es una *alternativa* al uso de patrones de acceso a objetos estáticos o globales. No podrá aprovechar las ventajas de la inserción de dependencias si la combina con el acceso a objetos estáticos.

## <a name="additional-resources"></a>Recursos adicionales

* [Inserción de dependencias en vistas](xref:mvc/views/dependency-injection)
* [Inserción de dependencias en controladores](xref:mvc/controllers/dependency-injection)
* [Inserción de dependencias en controladores de requisitos](xref:security/authorization/dependencyinjection)
* [Inicio de aplicaciones](xref:fundamentals/startup)
* [Prueba y depuración](xref:test/index)
* [Factory-based middleware activation](xref:fundamentals/middleware/extensibility) (Activación de middleware basada en Factory)
* [Escritura de código limpio en ASP.NET Core con inserción de dependencias (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [Preludio del diseño de aplicaciones administradas por contenedor: ¿cuál es el lugar del contenedor?](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)
* [Principio de dependencias explícitas](http://deviq.com/explicit-dependencies-principle/)
* [Los contenedores de inversión de control y el patrón de inserción de dependencias](https://www.martinfowler.com/articles/injection.html) (Fowler)
