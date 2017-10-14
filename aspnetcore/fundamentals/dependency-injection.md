---
title: "Inyección de dependencia en el núcleo de ASP.NET"
author: ardalis
description: "Obtenga información acerca de cómo ASP.NET Core implementa inyección de dependencia y cómo utilizarlo."
keywords: "Núcleo de ASP.NET, la inserción de dependencias, di"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fccd69be-7ad1-47fb-b203-b3633b6b9a9b
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/dependency-injection
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a3a6f755d8825d4bd31cad4e459750c549709c6d
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2017
---
# <a name="introduction-to-dependency-injection-in-aspnet-core"></a>Introducción a la inyección de dependencia en el núcleo de ASP.NET

<a name="fundamentals-dependency-injection"></a>

Por [Steve Smith](https://ardalis.com/) y [Scott Addie](https://scottaddie.com)

ASP.NET Core está diseñado desde el principio para admitir y aprovechar la inserción de dependencias. Las aplicaciones principales de ASP.NET pueden aprovechar los servicios de marco integrado por indica que existen insertado en métodos de la clase de inicio y los servicios de aplicaciones pueden configurarse para la inyección de así. El contenedor de servicios predeterminado proporcionado por ASP.NET Core proporciona una característica mínimo establecido y no está diseñada para reemplazar otros contenedores.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-dependency-injection"></a>¿Qué es la inyección de dependencia?

Inserción de dependencias (DI) es una técnica para lograr el acoplamiento flexible entre los objetos y sus colaboradores o dependencias. En lugar de crear directamente instancias de colaboradores, o mediante las referencias estáticas, se proporcionan los objetos de una clase que se necesita para llevar a cabo sus acciones a la clase de algún modo. A menudo, las clases declarará sus dependencias a través de su constructor, lo que les permite seguir el [principio de dependencias explícitas](http://deviq.com/explicit-dependencies-principle/). Este enfoque se conoce como "inyección de constructor".

Cuando las clases se diseñan con DI en mente, se acopla más flexible porque no tienen dependencias directas, codificado de forma rígida en sus colaboradores. Esto se sigue la [principio de inversión de dependencia](http://deviq.com/dependency-inversion-principle/), que indica que *"módulos de nivel alto no deben depender de los módulos de nivel inferiores; ambos deben depender de abstracciones."* En lugar de hacer referencia a las implementaciones específicas, las clases de solicitud abstracciones (normalmente `interfaces`) que se proporcionan para ellos cuando se construye la clase. Extracción de dependencias en interfaces y proporciona las implementaciones de estas interfaces como parámetros también son un ejemplo de la [modelo de diseño de la estrategia](http://deviq.com/strategy-design-pattern/).

Cuando un sistema está diseñado para utilizar DI, con muchas clases solicitar sus dependencias a través de su constructor (o propiedades), resulta útil tener una clase dedicada a la creación de estas clases con sus dependencias asociadas. Estas clases se conocen como *contenedores*, o más específicamente, [inversión de Control (IoC)](http://deviq.com/inversion-of-control/) contenedores o contenedores de inyección de dependencia (DI). Un contenedor es básicamente un generador que se encarga de proporcionar instancias de tipos que se solicitan desde él. Si ha declarado un tipo determinado que tiene dependencias, y el contenedor se ha configurado para proporcionar los tipos de dependencia, creará las dependencias como parte de la creación de la instancia solicitada. De esta manera, gráficos de dependencias complejos pueden proporcionarse a clases sin necesidad de ninguna construcción de objetos codificados de forma rígida. Además de crear objetos con sus dependencias, contenedores suelen administran la duración de los objetos dentro de la aplicación.

ASP.NET Core incluye un simple contenedor integrado (representado por la `IServiceProvider` interfaz) que admite la inserción del constructor de forma predeterminada y ASP.NET pone a disposición a través de DI determinados servicios. ASP. Contenedor de red hace referencia a los tipos que administra como *services*. En el resto de este artículo, *servicios* hará referencia a tipos que están administrados por el contenedor de IoC del núcleo de ASP.NET. Configurar los servicios del contenedor integrados en el `ConfigureServices` método de la aplicación `Startup` clase.

> [!NOTE]
> Martin Fowler ha escrito un artículo de una amplia en [contenedores de inversión de Control y el patrón de inyección de dependencia](https://www.martinfowler.com/articles/injection.html). Microsoft Patterns and Practices también tiene una descripción excelente de [inyección de dependencia](https://msdn.microsoft.com/library/hh323705.aspx).

> [!NOTE]
> Este artículo tratan inyección de dependencia tal como se aplica a todas las aplicaciones de ASP.NET. Inyección de dependencia dentro de los controladores de MVC se trata en [controladores e inserción de dependencias](../mvc/controllers/dependency-injection.md).

### <a name="constructor-injection-behavior"></a>Comportamiento de inyección de constructor

Inyección de constructor requiere que el constructor en cuestión sea *público*. En caso contrario, la aplicación producirá una `InvalidOperationException`:

> No se pudo encontrar un constructor adecuado para el tipo 'YourType'. Asegúrese del tipo es concreto y los servicios se registran para todos los parámetros de un constructor público.


Inyección de constructor requiere existe este solo constructor es aplicable. Se admiten las sobrecargas de constructor, pero solo una sobrecarga puede existir cuyos argumentos pueden cumplirse por inyección de dependencia. Si existe más de uno, la aplicación producirá una `InvalidOperationException`:

> Se han encontrado varios constructores que acepta todos los tipos de argumento especificado en el tipo 'YourType'. Solo debe haber un constructor es aplicable.

Los constructores pueden aceptar argumentos que no proceden de inserción de dependencias, pero éstas deben admitir valores predeterminados. Por ejemplo:

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

## <a name="using-framework-provided-services"></a>Uso de los servicios proporcionados por el marco de trabajo

El `ConfigureServices` método en la `Startup` clase es responsable de definir los servicios que se va a utilizar la aplicación, incluidas las características de plataforma como Entity Framework Core y ASP.NET Core MVC. Inicialmente, el `IServiceCollection` proporcionadas para `ConfigureServices` tiene los siguientes servicios definidos (en función de [cómo se configurara el host](xref:fundamentals/hosting)):

| Tipo de servicio | Período de duración |
| ----- | ------- |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) | Singleton |
| [Microsoft.Extensions.Logging.ILoggerFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.iloggerfactory) | Singleton |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) | Singleton |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | Transitorio |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.ihttpcontextfactory) | Transitorio |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) | Singleton |
| [System.Diagnostics.DiagnosticSource](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | Singleton |
| [System.Diagnostics.DiagnosticListener](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartupfilter) | Transitorio |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.objectpool.objectpoolprovider) | Singleton |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.iconfigureoptions-1) | Transitorio |
| [Microsoft.AspNetCore.Hosting.Server.IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartup](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartup) | Singleton |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | Singleton |

A continuación se muestra un ejemplo de cómo agregar servicios adicionales para el contenedor mediante una serie de métodos de extensión como `AddDbContext`, `AddIdentity`, y `AddMvc`.

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

Las características y middleware proporcionadas por ASP.NET, como MVC, siguen una convención de usar un único complemento*ServiceName* método de extensión para registrar todos los servicios necesarios para esa característica.

>[!TIP]
> Puede solicitar determinados servicios proporcionados por el marco de trabajo dentro de `Startup` Vea métodos a través de sus listas de parámetros - [inicio de la aplicación](startup.md) para obtener más detalles.

## <a name="registering-your-own-services"></a>Registrar sus propios servicios

Puede registrar sus propios servicios de aplicación de la siguiente manera. El primer tipo genérico representa el tipo (normalmente una interfaz) que se le pedirá desde el contenedor. El segundo tipo genérico representa el tipo concreto que se crea una instancia del contenedor y se usará para responder a dichas solicitudes.

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> Cada `services.Add<ServiceName>` método de extensión agrega (y potencialmente configura) en servicios. Por ejemplo, `services.AddMvc()` agrega los servicios que requiere de MVC. Se recomienda que siga esta convención, colocar métodos de extensión en la `Microsoft.Extensions.DependencyInjection` espacio de nombres a encapsular grupos de registros de servicio.

El `AddTransient` método se utiliza para asignar tipos abstractos a servicios concretos que se crean instancias por separado para cada objeto que lo requiera. Esto se conoce como el servicio *duración*, y a continuación se describen las opciones de duración adicional. Es importante elegir un período adecuado para cada uno de los servicios que registra. ¿Una nueva instancia del servicio se debe proporcionar para cada clase que lo solicita? ¿Debe usarse una instancia a lo largo de una solicitud web determinado? ¿O bien, debe usarse una sola instancia para la duración de la aplicación?

En el ejemplo de este artículo, hay un controlador simple que muestra los nombres de carácter, denominados `CharactersController`. Su `Index` método muestra la lista actual de caracteres que se han almacenado en la aplicación e inicializa la colección con una serie de caracteres si no existe ninguna. Tenga en cuenta que, aunque esta aplicación usa Entity Framework Core y `ApplicationDbContext` clase para su persistencia, ninguno de los que es evidente en el controlador. En su lugar, se ha abstrae el mecanismo de acceso de datos específico detrás de una interfaz, `ICharacterRepository`, que sigue la [modelo de repositorio](http://deviq.com/repository-pattern/). Una instancia de `ICharacterRepository` se solicita mediante el constructor y se asigna a un campo privado, que se usa para tener acceso a caracteres según sea necesario.

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

El `ICharacterRepository` define los dos métodos es necesario trabajar con el controlador `Character` instancias.

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

Esta interfaz se implementa a su vez por un tipo concreto, `CharacterRepository`, que se utiliza en tiempo de ejecución.

> [!NOTE]
> La forma DI se utiliza con la `CharacterRepository` clase es un modelo general que puede seguir para todos los servicios de aplicación, no solo en "repositorios" o las clases de acceso a datos.

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

Tenga en cuenta que `CharacterRepository` solicitudes un `ApplicationDbContext` en su constructor. No es inusual para la inyección de dependencia que se usará de forma encadenada como este, con cada dependencia solicitados a su vez solicitar sus propias dependencias. El contenedor es responsable de resolver todas las dependencias en el gráfico y devolver el servicio totalmente resuelto.

> [!NOTE]
> Crear el objeto solicitado y todos los objetos que necesita y todos los objetos de los que requieren, se denomina a veces un *gráfico de objetos*. Del mismo modo, el conjunto colectivo de dependencias que deben resolverse se suele hacer referencia a como un *árbol de dependencias* o *gráfico de dependencias*.

En este caso, ambos `ICharacterRepository` y a su vez `ApplicationDbContext` debe estar registrado en el contenedor de servicios de `ConfigureServices` en `Startup`. `ApplicationDbContext`se configura con la llamada al método de extensión `AddDbContext<T>`. El código siguiente muestra el registro de la `CharacterRepository` tipo.

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

Contextos de Entity Framework deben agregarse en el contenedor de servicios mediante la `Scoped` duración. Esto se encarga del automáticamente si utiliza los métodos auxiliares como se indicó anteriormente. Repositorios que harán que el uso de Entity Framework deben usar la misma duración.

>[!WARNING]
> Está resolviendo el peligro principal sea cuidadoso con la representación de un `Scoped` servicio de un singleton. Es probable que en este caso el servicio tendrá un estado incorrecto al procesar las solicitudes posteriores.

Servicios que tengan dependencias deben registrarlos en el contenedor. Si el constructor de un servicio requiere un tipo primitivo, como un `string`, se pueden insertar utilizando el [opciones patrón y la configuración](configuration.md).

## <a name="service-lifetimes-and-registration-options"></a>Duración del servicio y las opciones de registro

Los servicios de ASP.NET pueden configurarse con la duración de la siguiente:

**Transitorio**

Servicios de duración transitorios se crean cada vez que se solicitan. Este período de vigencia funciona mejor para servicios sin estado ligeros.

**El ámbito**

Servicios de duración de ámbito se crean una vez por solicitud.

**Singleton**

Servicios de duración de singleton se crean la primera vez que se solicitan (o cuando `ConfigureServices` se ejecuta si se especifica una instancia no existe) y, a continuación, todas las solicitudes subsiguientes usará la misma instancia. Si la aplicación requiere un comportamiento singleton, lo que permite el contenedor de servicios administrar la duración del servicio se recomienda en lugar de implementar el patrón de diseño singleton y administrar la duración del objeto en la clase por sí mismo.

Los servicios se pueden registrar con el contenedor de varias maneras. Ya hemos visto cómo registrar una implementación de servicio con un tipo determinado mediante la especificación de tipo concreto para usar. Además, un generador puede especificarse, que se utilizará para crear la instancia a petición. El tercer enfoque consiste en especificar directamente la instancia del tipo que se utiliza en el que caso el contenedor nunca intentará crear una instancia (ni eliminará de la instancia).

Para mostrar la diferencia entre estas opciones de duración y el registro, considere la posibilidad de una interfaz simple que representa una o varias tareas como un *operación* con un identificador único, `OperationId`. Según cómo se configura la duración de este servicio, el contenedor proporciona las mismas o diferentes instancias del servicio a la clase que lo solicita. Para dejar claro qué duración se solicita, crearemos un tipo por la opción de duración:

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

Se implementan estas interfaces mediante una sola clase, `Operation`, que acepta un `Guid` en su constructor, o usa un nuevo `Guid` si no se proporciona ninguno.

Después, en `ConfigureServices`, cada tipo se agrega al contenedor de acuerdo con su duración con nombre:

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

Tenga en cuenta que la `IOperationSingletonInstance` servicio está usando una instancia específica con un identificador conocido de `Guid.Empty` para poder borrar cuando este tipo está en uso (su Guid será todo ceros). También hemos registrado un `OperationService` que depende de cada uno de los otros `Operation` tipos, por lo que esté claro dentro de una solicitud si este servicio obtiene la misma instancia que el controlador o uno nuevo para cada tipo de operación. Todo lo que hace este servicio es exponer sus dependencias como propiedades, por lo que se pueden mostrar en la vista.

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

Para mostrar la duración de los objetos dentro y entre las solicitudes individuales independientes de la aplicación, el ejemplo incluye una `OperationsController` que cada tipo de solicitudes `IOperation` tipo así como un `OperationService`. El `Index` acción, a continuación, muestra todas del controlador y del servicio `OperationId` valores.

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

Ahora se realizan dos solicitudes diferentes para esta acción de controlador:

![La vista de operaciones de la aplicación web de ejemplo de inyección de dependencia ejecutando con Microsoft Edge con valores de Id. de operación (GUID) para transitorio, en el ámbito, Singleton y el controlador de instancia y las operaciones de servicio de operación en la primera solicitud.](dependency-injection/_static/lifetimes_request1.png)

![Las operaciones de vista que muestra los valores de Id. de operación para una segunda solicitud.](dependency-injection/_static/lifetimes_request2.png)

Observe cuál de los `OperationId` valores variar dentro de una solicitud y entre las solicitudes.

* *Transitorio* objetos siempre son distintos; se proporciona una nueva instancia para cada controlador y todos los servicios.

* *Ámbito* objetos son iguales dentro de una solicitud, pero es diferente entre distintas solicitudes

* *Singleton* objetos son los mismos para todos los objetos y todas las solicitudes (independientemente de si se proporciona una instancia en `ConfigureServices`)

## <a name="request-services"></a>Servicios de solicitud

Los servicios disponibles en un ASP.NET solicitar de `HttpContext` se exponen a través de la `RequestServices` colección.

![Intellisense de servicios de solicitud de HttpContext contextual cuadro de diálogo que indica que los servicios de solicitud Obtiene o establece el IServiceProvider que proporciona acceso al contenedor de servicios de la solicitud.](dependency-injection/_static/request-services.png)

Servicios de solicitud representan los servicios que configura y solicitar como parte de la aplicación. Cuando los objetos de especifican las dependencias, los tipos encontrados en satisface estos `RequestServices`, no `ApplicationServices`.

Por lo general, no utilice estas propiedades directamente, por lo que prefieren en su lugar solicitar a los tipos de las clases que necesita a través de su constructor de clase y dejar que el marco de trabajo insertar estas dependencias. Esto da como resultado las clases que son más fáciles de probar (vea [pruebas](../testing/index.md)) y más están acopladas.

> [!NOTE]
> Prefiere solicitar dependencias como parámetros del constructor al tener acceso a la `RequestServices` colección.

## <a name="designing-your-services-for-dependency-injection"></a>Diseñar los servicios para la inyección de dependencia

Debe diseñar los servicios para usar la inserción de dependencias para obtener sus colaboradores. Esto significa que evitar el uso de llamadas a métodos estáticos con estado (lo que ocasionará un olor de código que se conoce como [estáticos adhesivos para sus](http://deviq.com/static-cling/)) y la creación de instancias directa de las clases dependientes dentro de los servicios. Puede ser útil para recordar la frase [New es adherencia](https://ardalis.com/new-is-glue), al elegir si desea crear una instancia de un tipo o para solicitar a través de la inserción de dependencias. Siguiendo la [sólido principios de objeto orientada a servicios diseño](http://deviq.com/solid/), las clases naturalmente tienden a ser pequeña, correctamente factorizada y probados fácilmente.

¿Qué ocurre si piensa que las clases suelen tener demasiadas dependencias que se va a insertar de forma? Esto suele ser un inicio de sesión que está intentando hacer demasiado la clase y, probablemente es infringiendo SRP - la [principio de responsabilidad única](http://deviq.com/single-responsibility-principle/). Vea si puede refactorizar la clase moviendo algunos de sus responsabilidades en una nueva clase. Tenga en cuenta que la `Controller` clases deberían centrarse en aspectos de la interfaz de usuario, por lo que se deben mantener detalles de implementación de acceso de reglas y datos empresariales en clases adecuadas a estos [separar los problemas](http://deviq.com/separation-of-concerns/).

Con respecto a acceso a datos en concreto, puede insertar el `DbContext` en los controladores (suponiendo que ha agregado EF al contenedor de servicios de `ConfigureServices`). Algunos desarrolladores prefieren usar una interfaz de repositorio para la base de datos en lugar de insertar el `DbContext` directamente. Mediante una interfaz para encapsular los datos lógica de acceso en un solo lugar puede minimizar el número de posiciones tendrá que cambiar cuando cambia la base de datos.

### <a name="disposing-of-services"></a>Eliminación de servicios

El contenedor llamará `Dispose` para `IDisposable` tipos lo crea. Sin embargo, si agrega una instancia al contenedor usted mismo, lo no se eliminará.

Ejemplo:

```csharp
// Services implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    // container will create the instance(s) of these types and will dispose them
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();

    // container did not create instance so it will NOT dispose it
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

> [!NOTE]
> En la versión 1.0, el contenedor llama a dispose en *todos los* `IDisposable` objetos, incluso aquellos que no ha creado.

## <a name="replacing-the-default-services-container"></a>Reemplazar el contenedor de servicios predeterminado

El contenedor de servicios integrados está pensado para atender las necesidades básicas de la plataforma y la mayoría de las aplicaciones de consumidor creada en él. Sin embargo, los desarrolladores pueden reemplazar el contenedor integrado con el contenedor preferido. El `ConfigureServices` método normalmente devuelve `void`, sin embargo, si se cambia la firma para devolver `IServiceProvider`, puede configurar y devolver un contenedor diferente. Hay muchos contenedores de IOC disponibles para. NET. En este ejemplo, el [Autofac](https://autofac.org/) se usa el paquete.

En primer lugar, instale los paquetes de contenedor adecuada:

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

A continuación, configure el contenedor en `ConfigureServices` y devolver un `IServiceProvider`:

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
> Cuando se utiliza un contenedor de DI de terceros, debe cambiar `ConfigureServices` para que devuelva `IServiceProvider` en lugar de `void`.

Por último, configure Autofac como es habitual en `DefaultModule`:

```csharp
public class DefaultModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
    }
}
```

En tiempo de ejecución, se utilizará Autofac para resolver tipos e inyectar dependencias. [Más información sobre el uso de Autofac y ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html).

### <a name="thread-safety"></a>Seguridad para subprocesos

Servicios de singleton deben ser seguros para subprocesos. Si un servicio de singleton tiene una dependencia en un servicio transitorio, el servicio transitorio también puede necesitar para subprocesos según su uso por el singleton.

## <a name="recommendations"></a>Recomendaciones

Cuando se trabaja con la inserción de dependencias, tenga las siguientes recomendaciones en cuenta:

* DI es para los objetos que tienen dependencias complejas. Controladores, servicios, adaptadores y repositorios son ejemplos de objetos que podrían agregarse al DI.

* Evitar el almacenamiento de datos y la configuración directamente en DI. Por ejemplo, normalmente no debería agregarse carro de la compra de un usuario para el contenedor de servicios. Debe usar la configuración de la [opciones modelo](configuration.md#options-config-objects). Del mismo modo, evitar objetos "marcador de datos" que solo existen para permitir el acceso a otro objeto. Es mejor solicitar el elemento real que sea necesario mediante la inserción de dependencias, si es posible.

* Evitar el acceso estático a los servicios.

* Evite la ubicación del servicio en el código de aplicación.

* Evitar el acceso estático a `HttpContext`.

> [!NOTE]
> Al igual que todos los conjuntos de recomendaciones, puede encontrar situaciones donde se requiere omite uno. Hemos encontrado excepciones deben ser infrecuentes--casos principalmente muy especiales en el marco de trabajo.

Recuerde que la inyección de dependencia es un *alternativo* con los patrones de acceso de objeto estático o de forma global. No podrá aprovechar las ventajas de DI Si mezcla con el acceso a objetos estáticos.

## <a name="additional-resources"></a>Recursos adicionales

* [Inicio de aplicaciones](startup.md)

* [Pruebas](../testing/index.md)

* [Escribir código correcto en el núcleo de ASP.NET con la inserción de dependencias (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)

* [Administrado por el contenedor de diseño de la aplicación, preludio: Que lleva el contenedor pertenecen?](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)

* [Principio de dependencias explícitas](http://deviq.com/explicit-dependencies-principle/)

* [Inversión de contenedores de controles y el patrón de inyección de dependencia](https://www.martinfowler.com/articles/injection.html) (Fowler)
