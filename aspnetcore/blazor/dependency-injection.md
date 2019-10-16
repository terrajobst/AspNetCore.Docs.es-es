---
title: Inyección de dependencia de ASP.NET Core extraordinaria
author: guardrex
description: Vea cómo las aplicaciones más extraordinarias pueden insertar servicios en los componentes.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: blazor/dependency-injection
ms.openlocfilehash: b548f0e50e1a60b74969e5bbee43860be9ba5a7f
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391136"
---
# <a name="aspnet-core-blazor-dependency-injection"></a>Inyección de dependencia de ASP.NET Core extraordinaria

Por [Rainer Stropek](https://www.timecockpit.com)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

El increíble es compatible con la [inserción de dependencias (di)](xref:fundamentals/dependency-injection). Las aplicaciones pueden usar servicios integrados mediante su inserción en componentes. Las aplicaciones también pueden definir y registrar servicios personalizados y hacer que estén disponibles en toda la aplicación a través de DI.

DI es una técnica para tener acceso a los servicios configurados en una ubicación central. Esto puede ser útil en aplicaciones increíbles para:

* Compartir una única instancia de una clase de servicio entre varios componentes, conocido como servicio *Singleton* .
* Desacoplar componentes de clases de servicio concretas mediante el uso de abstracciones de referencia. Por ejemplo, considere una interfaz `IDataAccess` para tener acceso a los datos de la aplicación. La interfaz se implementa mediante una clase `DataAccess` concreta y se registra como un servicio en el contenedor de servicios de la aplicación. Cuando un componente usa DI para recibir una implementación `IDataAccess`, el componente no se acopla al tipo concreto. La implementación se puede intercambiar, quizás para una implementación ficticia en pruebas unitarias.

## <a name="default-services"></a>Servicios predeterminados

Los servicios predeterminados se agregan automáticamente a la colección de servicios de la aplicación.

| web de Office | Período de duración | Descripción |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | Singleton | Proporciona métodos para enviar solicitudes HTTP y recibir respuestas HTTP de un recurso identificado por un URI. Tenga en cuenta que esta instancia de `HttpClient` usa el explorador para controlar el tráfico HTTP en segundo plano. [HttpClient. BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) se establece automáticamente en el prefijo de URI base de la aplicación. Para obtener más información, vea <xref:blazor/call-web-api>. |
| `IJSRuntime` | Singleton | Representa una instancia de un Runtime de JavaScript en la que se envían las llamadas de JavaScript. Para obtener más información, vea <xref:blazor/javascript-interop>. |
| `NavigationManager` | Singleton | Contiene aplicaciones auxiliares para trabajar con URI y el estado de navegación. Para obtener más información, vea [aplicaciones auxiliares de URI y de estado de navegación](xref:blazor/routing#uri-and-navigation-state-helpers). |

Un proveedor de servicios personalizado no proporciona automáticamente los servicios predeterminados que aparecen en la tabla. Si utiliza un proveedor de servicios personalizado y requiere cualquiera de los servicios que se muestran en la tabla, agregue los servicios necesarios al nuevo proveedor de servicios.

## <a name="add-services-to-an-app"></a>Agregar servicios a una aplicación

Después de crear una nueva aplicación, examine el método `Startup.ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

Al método `ConfigureServices` se le pasa una <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, que es una lista de objetos de descriptor de servicio (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>). Para agregar los servicios, se proporcionan descriptores de servicio a la colección de servicios. En el ejemplo siguiente se muestra el concepto con la interfaz `IDataAccess` y su implementación concreta `DataAccess`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

Los servicios se pueden configurar con las duraciones que se muestran en la tabla siguiente.

| Período de duración | Descripción |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Las aplicaciones de webassembly increíbles no tienen actualmente un concepto de ámbito de DI. @no__t servicios registrados de -0 se comportan como los servicios `Singleton`. Sin embargo, el modelo de hospedaje del servidor más rápido admite la duración `Scoped`. En las aplicaciones de servidor increíbles, el ámbito de un registro de servicio de ámbito es la *conexión*. Por esta razón, se prefiere el uso de servicios con ámbito para los servicios que deben tener el ámbito del usuario actual, aunque la intención actual sea ejecutar el lado cliente en el explorador. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | DI crea una *única instancia* del servicio. Todos los componentes que requieren un servicio `Singleton` reciben una instancia del mismo servicio. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | Cada vez que un componente obtiene una instancia de un servicio `Transient` del contenedor de servicios, recibe una *nueva instancia* del servicio. |

El sistema DI se basa en el sistema DI en ASP.NET Core. Para obtener más información, vea <xref:fundamentals/dependency-injection>.

## <a name="request-a-service-in-a-component"></a>Solicitar un servicio en un componente

Una vez agregados los servicios a la colección de servicios, inserte los servicios en los componentes mediante la Directiva Razor [\@inject](xref:mvc/views/razor#inject) . `@inject` tiene dos parámetros:

* Escriba &ndash; tipo del servicio que se va a insertar.
* Propiedad &ndash; nombre de la propiedad que recibe la aplicación insertada. La propiedad no requiere la creación manual. El compilador crea la propiedad.

Para obtener más información, vea <xref:mvc/views/dependency-injection>.

Use varias instrucciones `@inject` para insertar distintos servicios.

En el ejemplo siguiente se muestra cómo utilizar `@inject`. El servicio que implementa `Services.IDataAccess` se inserta en la propiedad del componente `DataRepository`. Observe cómo el código solo usa la abstracción `IDataAccess`:

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

Internamente, la propiedad generada (`DataRepository`) se decora con el atributo `InjectAttribute`. Normalmente, este atributo no se usa directamente. Si se requiere una clase base para los componentes y las propiedades insertadas también son necesarias para la clase base, agregue manualmente el `InjectAttribute`:

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

En los componentes derivados de la clase base, no se requiere la Directiva `@inject`. El `InjectAttribute` de la clase base es suficiente:

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a>Usar DI en servicios

Los servicios complejos pueden requerir servicios adicionales. En el ejemplo anterior, `DataAccess` podría requerir el servicio predeterminado `HttpClient`. `@inject` (o el `InjectAttribute`) no está disponible para su uso en los servicios. En su lugar, se debe usar la *inserción de constructores* . Los servicios necesarios se agregan agregando parámetros al constructor del servicio. Cuando DI crea el servicio, reconoce los servicios que requiere en el constructor y los proporciona en consecuencia.

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

Requisitos previos para la inserción de constructores:

* Debe existir un constructor cuyos argumentos se puedan cumplir con DI. Los parámetros adicionales que no están incluidos en DI se permiten si especifican valores predeterminados.
* El constructor aplicable debe ser *público*.
* Debe existir un constructor aplicable. En caso de ambigüedad, DI produce una excepción.

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a>Clases de componentes base de la utilidad para administrar un ámbito de DI

En ASP.NET Core aplicaciones, el ámbito de los servicios de ámbito suele ser la solicitud actual. Una vez completada la solicitud, el sistema DI elimina todos los servicios de ámbito o transitorios. En las aplicaciones de servidor increíbles, el ámbito de la solicitud se mantiene durante la conexión del cliente, lo que puede dar lugar a que los servicios transitorios y de ámbito duren mucho más tiempo del esperado.

Para limitar los servicios a la duración de un componente, puede usar las clases base `OwningComponentBase` y `OwningComponentBase<TService>`. Estas clases base exponen una propiedad `ScopedServices` de tipo `IServiceProvider` que resuelve los servicios cuyo ámbito es la duración del componente. Para crear un componente que herede de una clase base en Razor, use la Directiva `@inherits`.

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
> Los servicios insertados en el componente mediante `@inject` o el `InjectAttribute` no se crean en el ámbito del componente y están vinculados al ámbito de la solicitud.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
