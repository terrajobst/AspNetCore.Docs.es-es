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
# <a name="razor-components-dependency-injection"></a>Inserción de dependencias de los componentes de Razor

Por [Rainer Stropek](https://www.timecockpit.com)

Es compatible con los componentes de Razor [inserción de dependencias (DI)](xref:fundamentals/dependency-injection). Las aplicaciones pueden usar los servicios integrados insertando en componentes. Las aplicaciones también pueden definir y registrar los servicios personalizados y que estén disponibles en toda la aplicación a través de DI.

## <a name="dependency-injection"></a>Inserción de dependencias

Inserción de dependencias es una técnica para tener acceso a los servicios configurados en una ubicación central. Esto puede ser útil en aplicaciones de componentes de Razor para:

* Compartir una única instancia de una clase de servicio a través de muchos componentes, conocidos como un *singleton* service.
* Desacoplar componentes de las clases de servicio concreta mediante el uso de las abstracciones de referencia. Por ejemplo, considere la posibilidad de una interfaz `IDataAccess` para tener acceso a datos de la aplicación. La interfaz se implementa mediante un hormigón `DataAccess` clase y registrado como un servicio en contenedor de servicios de la aplicación. Cuando un componente usa DI para recibir un `IDataAccess` implementación, el componente no es estrictamente con el tipo concreto. La implementación se puede intercambiar, quizás a una implementación ficticia en pruebas unitarias.

Para obtener más información, consulta <xref:fundamentals/dependency-injection>.

## <a name="add-services-to-di"></a>Agregar servicios a la inserción de dependencias

Después de crear una nueva aplicación, examine el `Startup.ConfigureServices` método:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

El `ConfigureServices` se pasa al método un <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, que es una lista de objetos de descriptor de servicio (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>). Los servicios se agregan al proporcionar descriptores del servicio a la colección de servicios. En el ejemplo siguiente se muestra el concepto con la `IDataAccess` interfaz y su implementación concreta `DataAccess`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

Servicios pueden configurarse con las duraciones que se muestra en la tabla siguiente.

| Período de duración | Descripción |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | Inserción de dependencias se crea un *única instancia* del servicio. Todos los componentes que requieren un `Singleton` servicio recibe una instancia del mismo servicio. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | Cada vez que un componente obtiene una instancia de un `Transient` servicio desde el contenedor de servicios, recibe un *nueva instancia* del servicio. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Del lado cliente Blazor actualmente no tiene el concepto de ámbitos de DI. `Scoped` se comporta como `Singleton`. Sin embargo, los componentes de Razor de ASP.NET Core admiten la `Scoped` duración. En un componente de Razor, un registro de servicio con ámbito se limita a la conexión. Por este motivo, utilizando los servicios de ámbito es preferible para los servicios que se deben acotar al usuario actual, incluso si la intención actual es ejecutar el cliente en el explorador. |

El sistema de DI se basa en el sistema de DI en ASP.NET Core. Para obtener más información, consulta <xref:fundamentals/dependency-injection>.

## <a name="default-services"></a>Servicios predeterminados

Servicios predeterminados se agregan automáticamente a la colección de servicios de la aplicación.

| web de Office | Descripción |
| ------- | ----------- |
| <xref:System.Net.Http.HttpClient> | Proporciona métodos para enviar solicitudes HTTP y recibir respuestas HTTP de un recurso identificado por un URI (singleton). Tenga en cuenta que esta instancia de `HttpClient` utiliza el explorador para controlar el tráfico HTTP en segundo plano. [HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) se establece automáticamente en el prefijo URI base de la aplicación. `HttpClient` solo se proporciona a las aplicaciones de Blazor del lado cliente. |
| `IJSRuntime` | Representa una instancia de un tiempo de ejecución de JavaScript a la que se pueden enviar las llamadas. Para obtener más información, consulta <xref:razor-components/javascript-interop>. |
| `IUriHelper` | Contiene aplicaciones auxiliares para trabajar con el estado de los URI y la navegación (singleton). `IUriHelper` se proporciona a las aplicaciones Blazor y componentes de Razor. |

Es posible utilizar un proveedor de servicio personalizado en lugar del proveedor de servicio predeterminada agregado por la plantilla predeterminada. Un proveedor de servicios personalizada no proporciona automáticamente los servicios predeterminados que se muestran en la tabla. Si usa un proveedor de servicios personalizados y requieren cualquiera de los servicios que se muestra en la tabla, agregue los servicios necesarios para el nuevo proveedor de servicios.

## <a name="request-a-service-in-a-component"></a>Solicitar un servicio en un componente

Después de que los servicios se agregan a la colección de servicios, insertar los servicios en plantillas de Razor de los componentes mediante la [ @inject ](xref:mvc/views/razor#section-4) directiva Razor. `@inject` tiene dos parámetros:

* Nombre del tipo: El tipo del servicio que se va a insertar.
* Nombre de propiedad: El nombre de la propiedad recibe el servicio de aplicación insertado. Tenga en cuenta que la propiedad no requiere la creación manual. El compilador crea la propiedad.

Para obtener más información, consulta <xref:mvc/views/dependency-injection>.

Usar varios `@inject` instrucciones para insertar los diferentes servicios.

En el ejemplo siguiente se muestra cómo utilizar `@inject`. El servicio que implementa `Services.IDataAccess` se inserta en la propiedad del componente `DataRepository`. Tenga en cuenta cómo el código solo usa el `IDataAccess` abstracción:

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.cshtml?highlight=2-3,23)]

Internamente, la propiedad generada (`DataRepository`) está decorada con el `InjectAttribute` atributo. Normalmente, este atributo no se usa directamente. Si es necesaria para los componentes de una clase base e insertadas propiedades también son necesarias para la clase base, `InjectAttribute` pueden agregarse manualmente:

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

En los componentes derivados de la clase base, el `@inject` directiva no es necesaria. El `InjectAttribute` de la clase base es suficiente:

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="dependency-injection-in-services"></a>Inserción de dependencias en servicios

Servicios complejos pueden requerir servicios adicionales. En el ejemplo anterior, `DataAccess` podría requerir el `HttpClient` servicio predeterminado. `@inject` (o `InjectAttribute`) no está disponible para su uso en los servicios. *Inserción de constructor* debe usarse en su lugar. Los servicios necesarios se agregan mediante la adición de parámetros al constructor del servicio. Cuando la inserción de dependencias, crea el servicio, reconoce los servicios que requiere en el constructor y las proporciona según corresponda.

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

Requisitos previos para la inyección de constructor:

* Debe haber un constructor cuyos argumentos pueden cumplir la inserción de dependencias. Tenga en cuenta que se permiten parámetros adicionales no cubiertos por la inserción de dependencias si especifican los valores predeterminados.
* El constructor correspondiente debe ser *pública*.
* Solo debe haber un constructor aplicable. En el caso de ambigüedad, inserción de dependencias produce una excepción.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
