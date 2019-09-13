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
# <a name="aspnet-core-blazor-dependency-injection"></a>Inyección de dependencia de ASP.NET Core extraordinaria

Por [Rainer Stropek](https://www.timecockpit.com)

El increíble es compatible con la [inserción de dependencias (di)](xref:fundamentals/dependency-injection). Las aplicaciones pueden usar servicios integrados mediante su inserción en componentes. Las aplicaciones también pueden definir y registrar servicios personalizados y hacer que estén disponibles en toda la aplicación a través de DI.

DI es una técnica para tener acceso a los servicios configurados en una ubicación central. Esto puede ser útil en aplicaciones increíbles para:

* Compartir una única instancia de una clase de servicio entre varios componentes, conocido como servicio *Singleton* .
* Desacoplar componentes de clases de servicio concretas mediante el uso de abstracciones de referencia. Por ejemplo, considere una interfaz `IDataAccess` para tener acceso a los datos de la aplicación. La interfaz se implementa mediante una clase `DataAccess` concreta y se registra como un servicio en el contenedor de servicios de la aplicación. Cuando un componente usa di para recibir una `IDataAccess` implementación, el componente no se acopla al tipo concreto. La implementación se puede intercambiar, quizás para una implementación ficticia en pruebas unitarias.

## <a name="default-services"></a>Servicios predeterminados

Los servicios predeterminados se agregan automáticamente a la colección de servicios de la aplicación.

| Servicio | Período de duración | DESCRIPCIÓN |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | Singleton | Proporciona métodos para enviar solicitudes HTTP y recibir respuestas HTTP de un recurso identificado por un URI. Tenga en cuenta que esta `HttpClient` instancia de usa el explorador para controlar el tráfico http en segundo plano. [HttpClient. BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) se establece automáticamente en el prefijo de URI base de la aplicación. Para obtener más información, consulta <xref:blazor/call-web-api>. |
| `IJSRuntime` | Singleton | Representa una instancia de un Runtime de JavaScript en la que se envían las llamadas de JavaScript. Para obtener más información, consulta <xref:blazor/javascript-interop>. |
| `NavigationManager` | Singleton | Contiene aplicaciones auxiliares para trabajar con URI y el estado de navegación. Para obtener más información, vea [aplicaciones auxiliares de URI y de estado de navegación](xref:blazor/routing#uri-and-navigation-state-helpers). |

Un proveedor de servicios personalizado no proporciona automáticamente los servicios predeterminados que aparecen en la tabla. Si utiliza un proveedor de servicios personalizado y requiere cualquiera de los servicios que se muestran en la tabla, agregue los servicios necesarios al nuevo proveedor de servicios.

## <a name="add-services-to-an-app"></a>Agregar servicios a una aplicación

Después de crear una nueva aplicación, examine `Startup.ConfigureServices` el método:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

Al `ConfigureServices` método se le pasa <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>una, que es una lista de objetos de descriptor de servicio (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>). Para agregar los servicios, se proporcionan descriptores de servicio a la colección de servicios. En el ejemplo siguiente se muestra el concepto `IDataAccess` con la interfaz y su `DataAccess`implementación concreta:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

Los servicios se pueden configurar con las duraciones que se muestran en la tabla siguiente.

| Período de duración | DESCRIPCIÓN |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Las aplicaciones de webassembly increíbles no tienen actualmente un concepto de ámbito de DI. `Scoped`: los servicios registrados se `Singleton` comportan como servicios. Sin embargo, el modelo de hospedaje del servidor más `Scoped` rápido admite la duración. En las aplicaciones de servidor increíbles, el ámbito de un registro de servicio de ámbito es la *conexión*. Por esta razón, se prefiere el uso de servicios con ámbito para los servicios que deben tener el ámbito del usuario actual, aunque la intención actual sea ejecutar el lado cliente en el explorador. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | DI crea una *única instancia* del servicio. Todos los componentes que requieren `Singleton` un servicio reciben una instancia del mismo servicio. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | Cada vez que un componente obtiene una instancia de `Transient` un servicio del contenedor de servicios, recibe una *nueva instancia* del servicio. |

El sistema DI se basa en el sistema DI en ASP.NET Core. Para obtener más información, consulta <xref:fundamentals/dependency-injection>.

## <a name="request-a-service-in-a-component"></a>Solicitar un servicio en un componente

Una vez agregados los servicios a la colección de servicios, inserte los servicios en los componentes mediante la Directiva de [ \@inserción](xref:mvc/views/razor#inject) de Razor. `@inject`tiene dos parámetros:

* Escriba &ndash; el tipo de servicio que se va a insertar.
* Propiedad &ndash; nombre de la propiedad que recibe la aplicación insertada. La propiedad no requiere la creación manual. El compilador crea la propiedad.

Para obtener más información, consulta <xref:mvc/views/dependency-injection>.

Use varias `@inject` instrucciones para insertar distintos servicios.

En el ejemplo siguiente se muestra cómo utilizar `@inject`. El servicio que `Services.IDataAccess` implementa se inserta en la propiedad `DataRepository`del componente. Observe cómo el código solo usa la `IDataAccess` abstracción:

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

Internamente, la propiedad generada`DataRepository`() se decora con `InjectAttribute` el atributo. Normalmente, este atributo no se usa directamente. Si se requiere una clase base para los componentes y las propiedades insertadas también son necesarias para la clase base, agregue `InjectAttribute`manualmente el:

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

En los componentes derivados de la clase base, `@inject` no se requiere la Directiva. El `InjectAttribute` de la clase base es suficiente:

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a>Usar DI en servicios

Los servicios complejos pueden requerir servicios adicionales. En el ejemplo anterior, `DataAccess` podría requerir el `HttpClient` servicio predeterminado. `@inject`(o) `InjectAttribute`no está disponible para su uso en los servicios de. En su lugar, se debe usar la *inserción de constructores* . Los servicios necesarios se agregan agregando parámetros al constructor del servicio. Cuando DI crea el servicio, reconoce los servicios que requiere en el constructor y los proporciona en consecuencia.

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

Para limitar los servicios a la duración de un componente, puede usar `OwningComponentBase` las `OwningComponentBase<TService>` clases base y. Estas clases base exponen una `ScopedServices` propiedad de tipo `IServiceProvider` que resuelve los servicios cuyo ámbito es la duración del componente. Para crear un componente que herede de una clase base en Razor, use la `@inherits` Directiva.

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
> Los servicios insertados en el componente `@inject` con `InjectAttribute` o no se crean en el ámbito del componente y están vinculados al ámbito de la solicitud.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
