---
title: Filtros en ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo funcionan los filtros y cómo se pueden usar en ASP.NET Core MVC.
ms.author: riande
ms.custom: mvc
ms.date: 02/08/2019
uid: mvc/controllers/filters
ms.openlocfilehash: 3cd576b389a2a4384c0ba90b5740ac42140533cc
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159319"
---
# <a name="filters-in-aspnet-core"></a>Filtros en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/) y [Steve Smith](https://ardalis.com/)

Los *filtros* en ASP.NET Core MVC permiten ejecutar código antes o después de determinadas fases de la canalización de procesamiento de la solicitud.

 Los filtros integrados se encargan de tareas como las siguientes:

 * Autorización (impedir el acceso a los recursos a un usuario que no está autorizado).
 * Procurar que se use HTTPS en todas las solicitudes.
 * Almacenamiento en caché de respuestas (cortocircuitar la canalización de solicitud para devolver una respuesta almacenada en caché). 

Se pueden crear filtros personalizados que se encarguen de cuestiones transversales. Los filtros pueden evitar la duplicación de código en las acciones. Así, por ejemplo, un filtro de excepción de control de errores puede consolidar el control de errores.

[Vea o descargue el ejemplo de GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).

## <a name="how-filters-work"></a>Funcionamiento de los filtros

Los filtros se ejecutan dentro de la *canalización de invocación de acción de MVC*, a veces denominada *canalización de filtro*.  La canalización de filtro se ejecuta después de que MVC seleccione la acción que se va a ejecutar.

![La solicitud se procesa a través de las fases Otro middleware, Middleware de enrutamiento, Selección de acción y Canalización de invocación de acción de MVC. El procesamiento de la solicitud continúa a la inversa, pasando por Selección de acción, Middleware de enrutamiento y varias fases de Otro middleware, antes de convertirse en una respuesta para enviarla al cliente.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>Tipos de filtro

Cada tipo de filtro se ejecuta en una fase diferente dentro de la canalización de filtro.

* Los [filtros de autorización](#authorization-filters) se ejecutan en primer lugar y sirven para averiguar si el usuario actual está autorizado para realizar la solicitud actual. Esos filtros pueden cortocircuitar la canalización si una solicitud no está autorizada. 

* Los [filtros de recursos](#resource-filters) son los primeros en atender la solicitud después de que se haya autorizado.  Pueden ejecutar código antes de pasar al resto de la canalización de filtro y después de que esta se haya completado. Son útiles para implementar el almacenamiento en caché o para cortocircuitar la canalización de filtro por motivos de rendimiento. Se ejecutan antes del enlace de modelos, de modo que pueden influir en el enlace de modelos.

* Los [filtros de acciones](#action-filters) pueden ejecutar código inmediatamente antes y después de llamar a un método de acción individual. Se pueden usar para manipular los argumentos pasados a una acción y el resultado obtenido de la acción. Los filtros de acción no se admiten en Razor Pages.

* Los [filtros de excepciones](#exception-filters) sirven para aplicar directivas globales a las excepciones no controladas que se producen antes de que se escriba algo en el cuerpo de respuesta.

* Los [filtros de resultados](#result-filters) pueden ejecutar código inmediatamente antes y después de la ejecución de resultados de acción individuales. Se ejecutan solo cuando el método de acción se ha ejecutado correctamente. Son útiles para la lógica que debe regir la ejecución de la vista o el formateador.

En el siguiente diagrama se muestra cómo interactúan estos tipos de filtro en la canalización de filtro.

![La solicitud se procesa a través de las fases Filtros de autorización, Filtros de recursos, Enlace de modelos, Filtros de acciones, Ejecución de acciones/Conversión del resultado de acción, Filtros de excepción, Filtros de resultados y Ejecución del resultado. Como resultado, la solicitud solo se procesa por las fases Filtros de resultados y Filtros de recursos antes de convertirse en una respuesta para enviarla al cliente.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>Implementación

Los filtros admiten implementaciones tanto sincrónicas como asincrónicas a través de diferentes definiciones de interfaz. 

Los filtros sincrónicos que pueden ejecutar código tanto antes como después de su fase de canalización definen los métodos On*Stage*Executing y On*Stage*Executed. Así, por ejemplo, se llama a `OnActionExecuting` antes de llamar al método de acción y a `OnActionExecuted`, después de que el método de acción haya vuelto.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet1)]

Los filtros asincrónicos definen un solo método On*Stage*ExecutionAsync. Este método toma un delegado *FilterType*ExecutionDelegate que ejecuta la fase de canalización del filtro. Por ejemplo, `ActionExecutionDelegate` llama al método de acción o al siguiente filtro de acción y el usuario puede ejecutar código antes y después de esta llamada.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

Se pueden implementar interfaces que abarquen varias fases de filtro en una sola clase. Por ejemplo, la clase <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> implementa `IActionFilter`, `IResultFilter` y sus equivalentes asincrónicos.

> [!NOTE]
> Implemente la versión sincrónica **o** la versión asincrónica de una interfaz de filtro, pero no ambas. El marco comprueba primero si el filtro implementa la interfaz asincrónica y, si es así, es a la interfaz que llama. De lo contrario, llamará a métodos de interfaz sincrónicos. Si se implementaran ambas interfaces en una clase, solamente se llamaría al método asincrónico. Cuando se usan clases abstractas como <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, se invalidan solo los métodos sincrónicos o el método asincrónico de cada tipo de filtro.

### <a name="ifilterfactory"></a>IFilterFactory

[IFilterFactory](/dotnet/api/microsoft.aspnetcore.mvc.filters.ifilterfactory) implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>. Por tanto, una instancia de `IFilterFactory` se puede usar como una instancia de `IFilterMetadata` en cualquier parte de la canalización de filtro. Cuando el marco se prepara para invocar el filtro, intenta convertirlo a un `IFilterFactory`. Si esa conversión se realiza correctamente, se llama al método [CreateInstance](/dotnet/api/microsoft.aspnetcore.mvc.filters.ifilterfactory.createinstance) para crear la instancia de `IFilterMetadata` que se va a invocar. Esto proporciona un diseño flexible, dado que no hay que establecer la canalización de filtro exacta de forma explícita cuando la aplicación se inicia.

Puede implementar `IFilterFactory` en sus propias implementaciones de atributo como método alternativo para crear filtros:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

### <a name="built-in-filter-attributes"></a>Atributos de filtros integrados

El marco incluye filtros integrados basados en atributos que se pueden personalizar y a partir de los cuales crear subclases. Por ejemplo, el siguiente filtro de resultados agrega un encabezado a la respuesta.

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/AddHeaderAttribute.cs?highlight=5,16)]

Los atributos permiten a los filtros aceptar argumentos, como se muestra en el ejemplo anterior. Podríamos agregar este atributo a un método de acción o controlador, y especificar el nombre y el valor del encabezado HTTP:

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

Aquí se muestra el resultado de la acción `Index`; los encabezados de respuesta aparecen en la parte inferior derecha.

![Pantalla de las herramientas de desarrollo de Microsoft Edge que muestra encabezados de respuesta con el autor Steve Smith @ardalis](filters/_static/add-header.png)

Algunas de las interfaces de filtro tienen atributos correspondientes que se pueden usar como clases base en las implementaciones personalizadas.

Atributos de filtro:

* `ActionFilterAttribute`
* `ExceptionFilterAttribute`
* `ResultFilterAttribute`
* `FormatFilterAttribute`
* `ServiceFilterAttribute`
* `TypeFilterAttribute`

`TypeFilterAttribute` y `ServiceFilterAttribute` se explican [más adelante en este artículo](#dependency-injection).

## <a name="filter-scopes-and-order-of-execution"></a>Ámbitos del filtro y orden de ejecución

Un filtro se puede agregar a la canalización en uno de tres *ámbitos* posibles. Un filtro se puede agregar a un método de acción concreto o a una clase de controlador usando un atributo. Un filtro también se puede registrar globalmente para todos los controladores y acciones. Para ello, solo hay que agregarlo a la colección `MvcOptions.Filters` en `ConfigureServices`:

[!code-csharp[](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

### <a name="default-order-of-execution"></a>Orden de ejecución predeterminado

Cuando hay varios filtros en una determinada fase de la canalización, el ámbito determina el orden predeterminado en el que esos filtros se van a ejecutar.  Los filtros globales abarcan a los filtros de clase, que a su vez engloban a los filtros de método. Este proceso se denomina en ocasiones anidamiento tipo "matrioshka", ya que cada aumento en el ámbito está incluido en el ámbito anterior, como las muñecas rusas [matrioshka](https://wikipedia.org/wiki/Matryoshka_doll). Por lo general, se puede lograr el comportamiento de invalidación deseado sin tener que especificar el orden de forma explícita.

Como resultado de este anidamiento, el código de filtros *posterior* se ejecuta en el orden inverso al código *anterior*. La secuencia sería la siguiente:

* Código de filtros *anterior* aplicado globalmente
  * Código de filtros *anterior* aplicado a los controladores
    * Código de filtros *anterior* aplicado a los métodos de acción
    * Código de filtros *posterior* aplicado a los métodos de acción
  * Código de filtros *posterior* aplicado a los controladores
* Código de filtros *posterior* aplicado globalmente
  
Este es un ejemplo que ilustra el orden en el que se llama a los métodos de filtro relativos a filtros de acciones sincrónicos.

| Secuencia | Ámbito del filtro | Método de filtro |
|:--------:|:------------:|:-------------:|
| 1 | Global | `OnActionExecuting` |
| 2 | Controlador | `OnActionExecuting` |
| 3 | Método | `OnActionExecuting` |
| 4 | Método | `OnActionExecuted` |
| 5 | Controlador | `OnActionExecuted` |
| 6 | Global | `OnActionExecuted` |

Esta secuencia pone de manifiesto lo siguiente:

* El filtro de método está anidado en el filtro de controlador.
* El filtro de controlador está anidado en el filtro global. 

Dicho de otro modo, si estamos en el método On*Stage*ExecutionAsync de un filtro asincrónico, todos los filtros que tengan un ámbito más restrictivo se ejecutan mientras el código está en la pila.

> [!NOTE]
> Todos los controladores que heredan de la clase base `Controller` incluyen los métodos `OnActionExecuting` y `OnActionExecuted`. Estos métodos incluyen los filtros que se ejecutan en relación con una acción determinada: se llama a `OnActionExecuting` antes de cualquiera de los filtros y a `OnActionExecuted`, después de todos los filtros.

### <a name="overriding-the-default-order"></a>Invalidación del orden predeterminado

La secuencia de ejecución predeterminada se puede invalidar implementando `IOrderedFilter`. Esta interfaz expone una propiedad `Order` que tiene prioridad sobre el ámbito a la hora de determinar el orden de ejecución. Así, el código *anterior* de un filtro con un valor de `Order` más bajo se ejecutará antes que el de un filtro con un valor de `Order` más alto, de igual modo que el código *posterior* de un filtro con un valor de `Order` más bajo se ejecutará después que el de un filtro con un valor de `Order` más alto. La propiedad `Order` se puede establecer por medio de un parámetro de constructor:

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

Si tuviéramos estos tres mismos filtros de acciones del ejemplo anterior, pero estableciéramos la propiedad `Order` de los filtros global y del controlador en 1 y 2 respectivamente, el orden de ejecución se invertiría.

| Secuencia | Ámbito del filtro | Propiedad`Order`  | Método de filtro |
|:--------:|:------------:|:-----------------:|:-------------:|
| 1 | Método | 0 | `OnActionExecuting` |
| 2 | Controlador | 1  | `OnActionExecuting` |
| 3 | Global | 2  | `OnActionExecuting` |
| 4 | Global | 2  | `OnActionExecuted` |
| 5 | Controlador | 1  | `OnActionExecuted` |
| 6 | Método | 0  | `OnActionExecuted` |

La propiedad `Order` altera el ámbito al determinar el orden en el que se ejecutarán los filtros. Los filtros se clasifican por orden en primer lugar y, después, se usa el ámbito para priorizar en caso de igualdad. Todos los filtros integrados implementan `IOrderedFilter` y establecen el valor predeterminado de `Order` en 0. En los filtros integrados, el ámbito determinará el orden, a menos que `Order` se establezca en un valor distinto de cero.

## <a name="cancellation-and-short-circuiting"></a>Cancelación y cortocircuito

La canalización de filtro se puede cortocircuitar en cualquier momento estableciendo la propiedad `Result` en el parámetro `context` que se ha proporcionado al método de filtro. Por ejemplo, el siguiente filtro de recursos impide que el resto de la canalización se ejecute.

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

En el siguiente código, tanto el filtro `ShortCircuitingResourceFilter` como el filtro `AddHeader` tienen como destino el método de acción `SomeResource`. El `ShortCircuitingResourceFilter`:

* Se ejecuta en primer lugar, porque es un filtro de recursos y `AddHeader` es un filtro de acciones.
* Cortocircuita el resto de la canalización.

Por tanto, el filtro `AddHeader` nunca se ejecuta en relación con la acción `SomeResource`. Este comportamiento sería el mismo si ambos filtros se aplicaran en el nivel de método de acción, siempre y cuando `ShortCircuitingResourceFilter` se haya ejecutado primero. `ShortCircuitingResourceFilter` se ejecuta primero debido a su tipo de filtro o al uso explícito de la propiedad `Order`.

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a>Inserción de dependencias

Los filtros se pueden agregar por tipo o por instancia. Si agrega una instancia, dicha instancia se usará en cada solicitud. Si se agrega un tipo, se activará por tipo, lo que significa que se creará una instancia por cada solicitud y las dependencias de constructor que haya se rellenarán por medio de la [inserción de dependencias](../../fundamentals/dependency-injection.md). Agregar un filtro por tipo equivale a usar `filters.Add(new TypeFilterAttribute(typeof(MyFilter)))`.

Los filtros que se implementan como atributos y se agregan directamente a las clases de controlador o a los métodos de acción no pueden tener dependencias de constructor proporcionadas por la [inserción de dependencias](../../fundamentals/dependency-injection.md). El motivo es que los atributos deben tener los parámetros de constructor proporcionados allá donde se apliquen. Se trata de una limitación de cómo funcionan los atributos.

Si los filtros tienen dependencias a las que hay que tener acceso desde la inserción de dependencias, existen varios métodos posibles para ello. Puede aplicar el filtro a una clase o a un método de acción usando cualquiera de los siguientes elementos:

* `ServiceFilterAttribute`
* `TypeFilterAttribute`
* `IFilterFactory` implementado en el atributo

> [!NOTE]
> Una dependencia que probablemente convenga obtener de la inserción de dependencias es un registrador. Pese a ello, no cree ni use filtros con fines exclusivamente de registro, ya que seguro que las [características de registro del marco integradas](xref:fundamentals/logging/index) ya proporcionan lo que necesita. Si va a agregar funciones de registro a los filtros, estas deberán ir dirigidas a cuestiones de dominio empresarial o a un comportamiento específico del filtro, y no a acciones de MVC o a otros eventos del marco.

### <a name="servicefilterattribute"></a>ServiceFilterAttribute

Los tipos de implementación de filtro de servicio se registran en la inserción de dependencias. `ServiceFilterAttribute` recupera una instancia del filtro de la inserción de dependencias. Agregue `ServiceFilterAttribute` al contenedor en `Startup.ConfigureServices` y haga referencia a él en un atributo `[ServiceFilter]`:

[!code-csharp[](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=11)]

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

Al usar `ServiceFilterAttribute`, el valor `IsReusable` es una sugerencia de que la instancia de filtro *podría* reutilizarse fuera del ámbito de la solicitud en la que se creó. El marco no ofrece ninguna garantía de que se vaya a crear una única instancia del filtro o de que el filtro no vuelva a solicitarse desde el contenedor de DI en algún momento posterior. Evite el uso de `IsReusable` cuando use un filtro que dependa de servicios con una duración distinta de singleton.

Si `ServiceFilterAttribute` se usa sin registrar el tipo de filtro, se producirá una excepción:

```
System.InvalidOperationException: No service for type
'FiltersSample.Filters.AddHeaderFilterWithDI' has been registered.
```

`ServiceFilterAttribute` implementa `IFilterFactory`. `IFilterFactory` expone el método `CreateInstance` para crear una instancia de `IFilterMetadata`. El método `CreateInstance` carga el tipo especificado desde el contenedor de servicios (inserción de dependencias).

### <a name="typefilterattribute"></a>TypeFilterAttribute

`TypeFilterAttribute` es similar a `ServiceFilterAttribute`, pero su tipo no se resuelve directamente desde el contenedor de inserción de dependencias, sino que crea una instancia del tipo usando el elemento `Microsoft.Extensions.DependencyInjection.ObjectFactory`.

Debido a esta diferencia:

* Los tipos a los que se hace referencia con `TypeFilterAttribute` no tienen que estar ya registrados con el contenedor.  Sus dependencias se completan a través del contenedor. 
* `TypeFilterAttribute` puede aceptar opcionalmente argumentos de constructor del tipo en cuestión.

Al usar `TypeFilterAttribute`, el valor `IsReusable` es una sugerencia de que la instancia de filtro *podría* reutilizarse fuera del ámbito de la solicitud en la que se creó. El marco no ofrece ninguna garantía de que se vaya a crear una única instancia del filtro. Evite el uso de `IsReusable` cuando use un filtro que dependa de servicios con una duración distinta de singleton.

En el siguiente ejemplo se muestra cómo pasar argumentos a un tipo usando `TypeFilterAttribute`:

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

### <a name="ifilterfactory-implemented-on-your-attribute"></a>IFilterFactory implementado en el atributo

Si tiene un filtro con las siguientes características:

* No necesita argumentos.
* Tiene dependencias de constructor que deben completarse por medio de la inserción de dependencias.

Puede usar su propio atributo con nombre en las clases y métodos, en lugar de `[TypeFilter(typeof(FilterType))]`). En el siguiente filtro se muestra cómo se puede implementar esto:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

Este filtro se puede aplicar a clases o a métodos usando la sintaxis de `[SampleActionFilter]`, en lugar de tener que recurrir a `[TypeFilter]` o a `[ServiceFilter]`.

## <a name="authorization-filters"></a>Filtros de autorización

*Filtros de autorización*:

* Controlan el acceso a los métodos de acción.
* Son los primeros filtros que se ejecutan en la canalización de filtro. 
* Tienen un método anterior, pero no uno posterior. 

Solo se deben escribir filtros de autorización personalizados si se está escribiendo un marco de autorización propio. Es preferible configurar directivas de autorización o escribir una directiva de autorización personalizada a escribir un filtro personalizado. La implementación de filtro integrada se encarga únicamente de llamar al sistema de autorización.

No se deberían producir excepciones dentro de los filtros de autorización, ya que no habrá nada que controle esas excepciones (los filtros de excepciones no lo harán). Considere la posibilidad de emitir un desafío cuando se produzca una excepción.

[Aquí](xref:security/authorization/introduction) encontrará más información sobre la autorización.

## <a name="resource-filters"></a>Filtros de recursos

* Implementan la interfaz `IResourceFilter` o `IAsyncResourceFilter`.
* Su ejecución abarca la mayor parte de la canalización de filtro. 
* Los [filtros de autorización](#authorization-filters) son los únicos que se ejecutan antes que los filtros de recursos.

Los filtros de recursos son útiles para cortocircuitar la mayor parte del trabajo que está realizando una solicitud. Por ejemplo, un filtro de almacenamiento en caché puede evitar que se ejecute el resto de la canalización si la respuesta está en la memoria caché.

El [filtro de recursos de cortocircuito](#short-circuiting-resource-filter) mostrado anteriormente es un ejemplo de filtro de recursos. Otro ejemplo es [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):

* Evita que el enlace de modelos tenga acceso a los datos del formulario. 
* Resulta práctico cuando hay cargas de archivos muy voluminosos y se quiere impedir que el formulario se lea en la memoria.

## <a name="action-filters"></a>Filtros de acciones

> [!IMPORTANT]
> Los filtros de acción **no** se aplican a Razor Pages. Razor Pages admite <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> y <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>. Para más información, vea [Filter methods for Razor Pages](xref:razor-pages/filter) (Métodos de filtrado para páginas de Razor).

Los *filtros de acciones*:

* Implementan la interfaz `IActionFilter` o `IAsyncActionFilter`.
* Su ejecución rodea la ejecución de los métodos de acción.

Este es un filtro de acciones de ejemplo:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

<xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> ofrece las siguientes propiedades:

* `ActionArguments`: permite manipular las entradas en la acción.
* `Controller`: permite manipular la instancia del controlador. 
* `Result`: si se establece, cortocircuita la ejecución del método de acción y de los filtros de acciones posteriores. Producir una excepción también impide que el método de acción y los filtros posteriores se ejecuten, pero se considera un error en vez de un resultado correcto.

<xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> proporciona `Controller` y `Result`, además de las siguientes propiedades:

* `Canceled`: será true si otro filtro ha cortocircuitado la ejecución de la acción.
* `Exception`: será distinto de null si la acción o un filtro de acción posterior han producido una excepción. Si esta propiedad se establece en null, se "controlará" una excepción de forma eficaz y `Result` se ejecutará como si se hubiera devuelto desde el método de acción con normalidad.

En un `IAsyncActionFilter`, una llamada a `ActionExecutionDelegate`:

* Ejecuta cualquier filtro de acciones posterior y el método de acción.
* Devuelve `ActionExecutedContext`. 

Para cortocircuitar esto, asigne `ActionExecutingContext.Result` a alguna instancia de resultado y no llame a `ActionExecutionDelegate`.

El marco proporciona una clase abstracta `ActionFilterAttribute` de la que se pueden crear subclases. 

Un filtro de acciones puede servir para validar el estado del modelo y devolver cualquier error que surja si el estado no es válido:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

El método `OnActionExecuted` se ejecuta después del método de acción y puede ver y manipular los resultados de la acción a través de la propiedad `ActionExecutedContext.Result`. `ActionExecutedContext.Canceled` se establecerá en true si otro filtro ha cortocircuitado la ejecución de la acción. `ActionExecutedContext.Exception` se establecerá en un valor distinto de null si la acción o un filtro de acción posterior han producido una excepción. Si `ActionExecutedContext.Exception` se establece como nulo:

* Controla una excepción eficazmente.
* `ActionExectedContext.Result` se ejecuta como si se devolviera con normalidad desde el método de acción.

## <a name="exception-filters"></a>Filtros de excepciones

Los *filtros de excepciones* implementan la interfaz `IExceptionFilter` o `IAsyncExceptionFilter`. Se pueden usar para implementar directivas de control de errores comunes de una aplicación. 

En el siguiente filtro de excepciones de ejemplo se usa una vista de error de desarrollador personalizada para mostrar los detalles sobre las excepciones que se producen cuando la aplicación está en fase de desarrollo:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

Los filtros de excepciones:

* No tienen eventos anteriores ni posteriores. 
* Implementan `OnException` o `OnExceptionAsync`. 
* Controlan las excepciones sin controlar que se producen al crear controladores, en el [enlace de modelos](../models/model-binding.md), en los filtros de acciones o en los métodos de acción. 
* No detectan aquellas excepciones que se produzcan en los filtros de recursos, en los filtros de resultados o en la ejecución de resultados de MVC.

Para controlar una excepción, establezca la propiedad `ExceptionContext.ExceptionHandled` en true o escriba una respuesta. Esto detiene la propagación de la excepción. Un filtro de excepciones no tiene capacidad para convertir una excepción en un proceso "correcto". Eso solo lo pueden hacer los filtros de acciones.

> [!NOTE]
> En ASP.NET Core 1.1, la respuesta no se envía si `ExceptionHandled` está establecido en true **y** escribe una respuesta. En este caso, ASP.NET Core 1.0 envía la respuesta, mientras que ASP.NET Core 1.1.2 regresará al comportamiento de la versión 1.0. Para más información, vea el [problema n.º 5594](https://github.com/aspnet/Mvc/issues/5594) del repositorio de GitHub. 

Los filtros de excepciones:

* Son adecuados para interceptar las excepciones que se producen en las acciones de MVC.
* No son tan flexibles como el middleware de control de errores. 

Es preferible usar middleware de control de excepciones. Use filtros de excepción solo cuando deba realizar el control de errores *de manera diferente* según la acción de MVC elegida. Por ejemplo, puede que su aplicación tenga métodos de acción tanto para los puntos de conexión de API como para las vistas/HTML. Los puntos de conexión de API podrían devolver información sobre errores como JSON, mientras que las acciones basadas en vistas podrían devolver una página de error como HTML.

`ExceptionFilterAttribute` puede tener subclases. 

## <a name="result-filters"></a>Filtros de resultados

* Implementar una interfaz:
  * `IResultFilter` o `IAsyncResultFilter`.
  * `IAlwaysRunResultFilter` o `IAsyncAlwaysRunResultFilter`
* Su ejecución rodea la ejecución de los resultados de acción. 

### <a name="iresultfilter-and-iasyncresultfilter"></a>IResultFilter e IAsyncResultFilter

Este es un ejemplo de un filtro de resultados que agrega un encabezado HTTP.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

El tipo de resultado que se ejecute dependerá de la acción en cuestión. Una acción de MVC que devuelve una vista incluye todo el procesamiento de Razor como parte del elemento `ViewResult` que se está ejecutando. Un método API puede llevar a cabo algunas funciones de serialización como parte de la ejecución del resultado. [Aquí](actions.md) encontrará más información sobre los resultados de acciones.

Los filtros de resultados solo se ejecutan cuando los resultados son correctos; es decir, cuando la acción o los filtros de acciones generan un resultado de acción. Los filtros de resultados no se ejecutan si hay filtros de excepciones que controlan una excepción.

El método `OnResultExecuting` puede cortocircuitar la ejecución del resultado de la acción y de los filtros de resultados posteriores estableciendo `ResultExecutingContext.Cancel` en true. Por lo general, conviene escribir en el objeto de respuesta cuando el proceso se cortocircuite, ya que así evitará que se genere una respuesta vacía. Si se produce una excepción, sucederá lo siguiente:

* Se impedirá la ejecución del resultado de la acción y de los filtros subsiguientes.
* Se tratará como un error en lugar de como un resultado correcto.

Cuando el método `OnResultExecuted` se ejecuta, probablemente la respuesta se haya enviado al cliente y ya no se pueda cambiar (a menos que se produzca una excepción). `ResultExecutedContext.Canceled` se establecerá en true si otro filtro ha cortocircuitado la ejecución del resultado de la acción.

`ResultExecutedContext.Exception` se establecerá en un valor distinto de null si el resultado de la acción o un filtro de resultado posterior ha producido una excepción. Establecer `Exception` en un valor null hace que una excepción se "controle" de forma eficaz y evita que MVC vuelva a producir dicha excepción más adelante en la canalización. Cuando se controla una excepción en un filtro de resultados, no se pueden escribir datos en la respuesta. Si el resultado de la acción produce una excepción a mitad de su ejecución y los encabezados ya se han vaciado en el cliente, no hay ningún mecanismo confiable que permita enviar un código de error.

En un elemento `IAsyncResultFilter`, una llamada a `await next` en `ResultExecutionDelegate` ejecuta cualquier filtro de resultados posterior y el resultado de la acción. Para cortocircuitar esto, establezca `ResultExecutingContext.Cancel` en true y no llame a `ResultExectionDelegate`.

El marco proporciona una clase abstracta `ResultFilterAttribute` de la que se pueden crear subclases. La clase [AddHeaderAttribute](#add-header-attribute) mostrada anteriormente es un ejemplo de un atributo de filtro de resultados.

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a>IAlwaysRunResultFilter e IAsyncAlwaysRunResultFilter

Las interfaces <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> e <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> declaran una implementación <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> que se ejecuta para obtener resultados de la acción. El filtro se aplicará a un resultado de la acción a menos que un filtro <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAuthorizationFilter> aplique y cortocircuite la respuesta.

En otras palabras, estos filtros de "ejecución permanente" se ejecutan siempre, excepto si un filtro de autorización o excepción los cortocircuita. Los filtros distintos de `IExceptionFilter` e `IAuthorizationFilter` no los cortocircuitan.

Por ejemplo, el siguiente filtro siempre ejecuta y establece un resultado de la acción (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) con un código de estado *422 - Entidad no procesable* cuando se produce un error en la negociación de contenido:

```csharp
public class UnprocessableResultFilter : Attribute, IAlwaysRunResultFilter
{
    public void OnResultExecuting(ResultExecutingContext context)
    {
        if (context.Result is StatusCodeResult statusCodeResult &&
            statusCodeResult.StatusCode == 415)
        {
            context.Result = new ObjectResult("Can't process this!")
            {
                StatusCode = 422,
            };
        }
    }

    public void OnResultExecuted(ResultExecutedContext context)
    {
    }
}
```

## <a name="using-middleware-in-the-filter-pipeline"></a>Uso de middleware en la canalización de filtro

Los filtros de recursos funcionan como el [middleware](xref:fundamentals/middleware/index), en el sentido de que se encargan de la ejecución de todo lo que viene después en la canalización. Pero los filtros se diferencian del middleware en que forman parte de MVC, lo que significa que tienen acceso al contexto y las construcciones de MVC.

En ASP.NET Core 1.1 se puede usar middleware en la canalización de filtro. Conviene hacerlo si tiene un componente de middleware que necesita tener acceso a los datos de ruta de MVC, u otro que deba ejecutarse solo con ciertos controladores o acciones.

Para usar middleware como un filtro, cree un tipo con un método `Configure` en el que se especifique el middleware que quiera insertar en la canalización de filtro. Este es un ejemplo en el que se usa middleware de localización para establecer la referencia cultural actual de una solicitud:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

Después, puede usar `MiddlewareFilterAttribute` para ejecutar el middleware en relación con una acción o controlador concretos, o bien de manera global:

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

Los filtros de middleware se ejecutan en la misma fase de la canalización de filtro que los filtros de recursos, antes del enlace de modelos y después del resto de la canalización.

## <a name="next-actions"></a>Siguientes acciones

* Consulte [Métodos de filtrado para páginas de Razor](xref:razor-pages/filter).
* Para experimentar con los filtros, [descargue, pruebe y modifique el ejemplo de GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).
