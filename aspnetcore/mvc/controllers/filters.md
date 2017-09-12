---
title: Filtros
author: ardalis
description: "Obtenga información acerca de cómo * filtros * trabajo y cómo utilizarlas en MVC de ASP.NET Core."
keywords: "ASP.NET Core, filtros, filtros de mvc, los filtros de acción, los filtros de autorización, filtros de recursos, filtros de resultados, los filtros de excepción"
ms.author: tdykstra
manager: wpickett
ms.date: 12/12/2016
ms.topic: article
ms.assetid: 531bda08-aa5b-4471-8f08-96add22c8683
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/filters
ms.openlocfilehash: b96a70a2446cab7b1af9bd689469584868980595
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="filters"></a>Filtros

Por [Tom Dykstra](https://github.com/tdykstra/) y [Steve Smith](https://ardalis.com/)

*Filtros* en MVC de ASP.NET Core permite ejecutar código antes o después de ciertas fases de la canalización de procesamiento de la solicitud.

 Tareas de controlador de filtros integrados, como autorización (impedir el acceso a los recursos de para que un usuario no está autorizado), asegurarse de que todas las solicitudes utilicen HTTPS y el almacenamiento en caché (evaluación "cortocircuitada" de la canalización de solicitud para devolver una respuesta almacenada en caché) de respuesta. 

Puede crear filtros personalizados para controlar cuestiones de corte del cruce para la aplicación. Siempre que desee evitar la duplicación de código a través de acciones, los filtros son la solución. Por ejemplo, puede consolidar el código en un filtro de excepción de control de errores.

[Ver o descargar el ejemplo desde GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).

## <a name="how-do-filters-work"></a>¿Cómo funcionan los filtros?

Los filtros se ejecutan dentro de la *canalización de invocación de acción de MVC*, a veces denominado el *canalización filtro*.  La canalización de filtro se ejecuta después de MVC selecciona la acción que se va a ejecutar.

![La solicitud se procesa a través de Middleware de otros, enrutamiento Middleware, selección de acción y la canalización de invocación de acción de MVC. El procesamiento de la solicitud continúa hacia atrás por la selección de acción y enrutamiento Middleware, Middleware de otros diversos antes de convertirse en una respuesta que se envía al cliente.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>Tipos de filtro

Cada tipo de filtro se ejecuta en una fase diferente de la canalización de filtro.

* [Los filtros de autorización](#authorization-filters) se ejecutan primero y se usan para determinar si el usuario actual está autorizado para la solicitud actual. La canalización puede cortocircuito si una solicitud no está autorizada. 

* [Filtros de recurso](#resource-filters) son los primeros en atender una solicitud después de la autorización.  Puede ejecutar código antes que el resto de la canalización de filtro y después de que se ha completado el resto de la canalización. Son útiles para implementar el almacenamiento en caché o cortocircuito en caso contrario, la canalización de filtro por motivos de rendimiento. Dado que ejecuta antes del enlace de modelo, resultan útiles para todo lo que necesita para influir en el enlace de modelos.

* [Filtros de acción](#action-filters) puede ejecutar código inmediatamente antes y después de llama a un método de acción individuales. Puede utilizar para manipular los argumentos pasados a una acción y el resultado devuelto de la acción.

* [Los filtros de excepción](#exception-filters) se utilizan para aplicar directivas globales para las excepciones no controladas que se producen antes de nada se ha escrito en el cuerpo de respuesta.

* [Como resultado filtros](#result-filters) puede ejecutar código inmediatamente antes y después de la ejecución de los resultados de acción individual. Ejecutan sólo cuando el método de acción se ha ejecutado correctamente y son útiles para la lógica que debe rodean la ejecución de la vista o el formateador.

El siguiente diagrama muestra cómo interactúan estos tipos de filtro en la canalización de filtro.

![La solicitud se procesa a través de los filtros de autorización, filtros de recursos, enlace de modelos, filtros de acción, ejecución de acciones y conversión del resultado de acción, los filtros de excepción, filtros de resultados y ejecución de resultado. A la salida, solo se procesa la solicitud por filtros de resultados y los filtros de recursos antes de convertirse en una respuesta que se envía al cliente.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>Implementación

Los filtros admiten implementaciones sincrónicas y asincrónicas a través de definiciones de interfaz diferente. Elija la sincronización o async variante según el tipo de tarea que necesite realizar. 

Sincrónicos filtros que se pueden ejecutar código tanto antes y después de definir su fase de canalización en*fase*Executing y en*fase*ejecuta métodos. Por ejemplo, `OnActionExecuting` se llama antes de que se llama al método de acción, y `OnActionExecuted` se llama después de que vuelva el método de acción.

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?highlight=6,8,13)]

Filtros asincrónicos definen una sola en*fase*ExecutionAsync método. Este método toma un *FilterType*ExecutionDelegate delegado que se ejecuta la fase de canalización de filtros. Por ejemplo, `ActionExecutionDelegate` llamadas al método de acción y se pueden ejecutar código antes y después de llamar a él.

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

Pueden implementar interfaces para varias fases de filtro en una sola clase. Por ejemplo, el [ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute) clase abstracta implementa tanto `IActionFilter` y `IResultFilter`, así como sus equivalentes asincrónicos.

> [!NOTE]
> Implemente **cualquier** sincrónico o la versión asincrónica de una interfaz de filtro, no ambos. El marco de trabajo comprueba primero si el filtro implementa la interfaz asincrónica y, si es así, llama. De lo contrario, llama a métodos de la interfaz sincrónica. Si fuera a implementar ambas interfaces en una clase, se denominaría solo el método asincrónico. Al utilizar las clases abstractas como [ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute) invalidaría solo los métodos sincrónicos o el método asincrónico para cada tipo de filtro.

### <a name="ifilterfactory"></a>IFilterFactory

`IFilterFactory` implementa `IFilter`. Por lo tanto, un `IFilterFactory` instancia se puede usar como un `IFilter` instancia en cualquier parte de la canalización de filtro. Cuando el marco de trabajo se prepara para invocar el filtro, intenta convertirlo a un `IFilterFactory`. Si esa conversión se realiza correctamente, el `CreateInstance` método se llama para crear el `IFilter` instancia que se invocará. Esto proporciona un diseño muy flexible, ya que no es necesario que la canalización de filtro preciso establecerse de forma explícita cuando se inicia la aplicación.

Puede implementar `IFilterFactory` en sus propias implementaciones de atributo como otro enfoque para la creación de filtros:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

### <a name="built-in-filter-attributes"></a>Atributos de filtro integrado

El marco incluye filtros integrados basado en atributos que puede crear subclases y personalizar. Por ejemplo, el siguiente filtro de resultado agrega un encabezado a la respuesta.

<a name=add-header-attribute></a>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/AddHeaderAttribute.cs?highlight=5,16)]

Los atributos permiten filtros aceptar argumentos, como se muestra en el ejemplo anterior. ¿Agregar este atributo a un método de acción o controlador y especifique el nombre y el valor del encabezado HTTP:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

El resultado de la `Index` acción se muestra a continuación, se muestran los encabezados de respuesta en la parte inferior derecha.

![Developer Tools de Microsoft Edge que muestra los encabezados de respuesta, incluido el autor Steve Smith@ardalis](filters/_static/add-header.png)

Algunas de las interfaces de filtro tienen atributos correspondientes que pueden usarse como clases base para las implementaciones personalizadas.

Atributos de filtro:

* `ActionFilterAttribute`
* `ExceptionFilterAttribute`
* `ResultFilterAttribute`
* `FormatFilterAttribute`
* `ServiceFilterAttribute`
* `TypeFilterAttribute`

`TypeFilterAttribute`y `ServiceFilterAttribute` se explican [más adelante en este artículo](#dependency-injection).

## <a name="filter-scopes-and-order-of-execution"></a>Ámbitos de filtro y orden de ejecución

Puede agregar un filtro a la canalización en uno de los tres *ámbitos*. Puede agregar un filtro a un método de acción concreta o a una clase de controlador mediante un atributo. O puede registrar un filtro global (para todos los controladores y acciones) agregándolo a la `MvcOptions.Filters` colección en la `ConfigureServices` método en la `Startup` clase:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

### <a name="default-order-of-execution"></a>Orden de ejecución predeterminado

Cuando hay varios filtros para una determinada fase de la canalización, ámbito determina el orden predeterminado de la ejecución del filtro.  Filtros globales especifica los filtros de clase, que a su vez rodean filtros de método. Esto se conoce a veces como anidamiento "Ruso muñeca", tal y como cada aumento en el ámbito se ajusta alrededor del ámbito anterior, como un [muñeca anidamiento](https://wikipedia.org/wiki/Matryoshka_doll). Por lo general, obtendrá el comportamiento deseado de reemplazo sin tener que determinar el orden de forma explícita.

Como resultado de este anidamiento, la *después* código de filtros se ejecuta en el orden inverso de la *antes de* código. La secuencia tiene el siguiente aspecto:

* El *antes de* código de filtros aplicados globalmente
  * El *antes de* código de filtros aplicados a los controladores
    * El *antes de* código de filtros aplicados a métodos de acción
    * El *después* código de filtros aplicados a métodos de acción
  * El *después* código de filtros aplicados a los controladores
* El *después* código de filtros aplicados globalmente
  
Este es un ejemplo que muestra el orden en el filtro que se denominan métodos sincrónicos filtros de acción.

| Secuencia | Ámbito de filtro | Filter (método) |
|:--------:|:------------:|:-------------:|
| 1 | Global | `OnActionExecuting` |
| 2 | Controlador | `OnActionExecuting` |
| 3 | Método | `OnActionExecuting` |
| 4 | Método | `OnActionExecuted` |
| 5 | Controlador | `OnActionExecuted` |
| 6 | Global | `OnActionExecuted` |

Esta secuencia se muestra que el filtro de método se anida en el filtro de controlador y el filtro de controlador se anida dentro de los filtros globales. Lo coloque otra forma, si está dentro de un filtro de async de*fase*ExecutionAsync método, todos los filtros con un ámbito más estricto ejecutan mientras el código está en la pila.

> [!NOTE]
> Todos los controladores que se hereda de la `Controller` incluye la clase base `OnActionExecuting` y `OnActionExecuted` métodos. Estos métodos ajustan los filtros que se ejecutan para una acción determinada: `OnActionExecuting` se llama antes de cualquiera de los filtros, y `OnActionExecuted` se llama después de todos los filtros.

### <a name="overriding-the-default-order"></a>Invalidar el orden predeterminado

Puede invalidar la secuencia predeterminada de ejecución mediante la implementación de `IOrderedFilter`. Esta interfaz expone un `Order` propiedad que tiene prioridad sobre el ámbito para determinar el orden de ejecución. Un filtro con un menor `Order` valor tendrá su *antes de* código ejecutado antes de que un filtro con un valor más alto de `Order`. Un filtro con un menor `Order` valor tendrá su *después* código ejecutado después de que un filtro con una mayor `Order` valor. Puede establecer el `Order` propiedad usando un parámetro de constructor:

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

Si tiene el mismo 3 filtros de acción que se muestra en el ejemplo anterior pero el conjunto la `Order` propiedades globales y del controlador de filtros a 1 y 2 respectivamente, el orden de ejecución estarían invertido.

| Secuencia | Ámbito de filtro | Propiedad `Order` | Filter (método) |
|:--------:|:------------:|:-----------------:|:-------------:|
| 1 | Método | 0 | `OnActionExecuting` |
| 2 | Controlador | 1  | `OnActionExecuting` |
| 3 | Global | 2  | `OnActionExecuting` |
| 4 | Global | 2  | `OnActionExecuted` |
| 5 | Controlador | 1  | `OnActionExecuted` |
| 6 | Método | 0  | `OnActionExecuted` |

El `Order` falsean la propiedad ámbito al determinar el orden en el que se ejecutarán los filtros. Los filtros aparecen ordenados primero por orden, a continuación, ámbito se utiliza para romper los enlaces. Todos los filtros integrados implementan `IOrderedFilter` y establezca el valor predeterminado `Order` valor en 0, por lo que el ámbito determina el orden a menos que establezca `Order` en un valor distinto de cero.

## <a name="cancellation-and-short-circuiting"></a>La cancelación y el cortocircuito

Puede cortocircuita la canalización de filtro en cualquier momento estableciendo la `Result` propiedad en el `context` parámetro proporcionado para el método de filtro. Por ejemplo, el siguiente filtro de recursos impide que el resto de la canalización de ejecución.

<a name=short-circuiting-resource-filter></a>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

En el código siguiente, tanto el `ShortCircuitingResourceFilter` y `AddHeader` destino filtro el `SomeResource` método de acción. Sin embargo, dado que la `ShortCircuitingResourceFilter` se ejecuta primero (porque es un filtro de recurso y `AddHeader` es un filtro de acción) y cortocircuita el resto de la canalización, el `AddHeader` filtro nunca se ejecuta durante la `SomeResource` acción. Este comportamiento podría ser el mismo si ambos filtros aplicados en el nivel de método de acción, proporciona el `ShortCircuitingResourceFilter` ejecutó primer (usar debido a su tipo de filtro, o explícita de `Order` propiedad, por ejemplo).

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a>Inyección de dependencia

Filtros se pueden agregar por tipo o instancia. Si agrega una instancia, esa instancia se utilizará para cada solicitud. Si agrega un tipo, estará activa de tipo, lo que significa que se creará una instancia para cada solicitud y las dependencias de constructor se llenará con [inyección de dependencia](../../fundamentals/dependency-injection.md) (DI). Agregar un filtro por tipo es equivalente a `filters.Add(new TypeFilterAttribute(typeof(MyFilter)))`.

Los filtros que se implementan como atributos y agregan directamente a las clases de controlador o a los métodos de acción no pueden tener dependencias de constructor proporcionadas por [inyección de dependencia](../../fundamentals/dependency-injection.md) (DI). Esto es porque los atributos deben tener sus parámetros de constructor proporcionados que se aplican. Se trata de una limitación de cómo funcionan los atributos.

Si los filtros tienen dependencias que necesite tener acceso desde DI, existen varios enfoques admitidos. Puede aplicar el filtro a un método de acción o clase mediante uno de los siguientes:

* `ServiceFilterAttribute`
* `TypeFilterAttribute`
* `IFilterFactory`implementado en el atributo

> [!NOTE]
> Una dependencia que se desea obtener de DI es un registrador. Sin embargo, evite crear y usar filtros para fines de registro, ya que la [características de registro de marco de trabajo integrado](../../fundamentals/logging.md) ya puede proporcionar lo que necesita. Si se va a agregar un registro a los filtros, debe centrarse en cuestiones de dominio empresariales o el comportamiento específico para el filtro, en lugar de las acciones de MVC u otros eventos de framework.

### <a name="servicefilterattribute"></a>ServiceFilterAttribute

Un `ServiceFilter` recupera una instancia del filtro de DI. Agregue el filtro en el contenedor en `ConfigureServices`y hacer referencia a él en un `ServiceFilter` atributo

[!code-csharp[Main](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=11)]

[!code-csharp[Main](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

Usar `ServiceFilter` sin registrar los resultados del tipo de filtro en una excepción:

```
System.InvalidOperationException: No service for type
'FiltersSample.Filters.AddHeaderFilterWithDI' has been registered.
```

`ServiceFilterAttribute`implementa `IFilterFactory`, que expone un único método para crear un `IFilter` instancia. En el caso de `ServiceFilterAttribute`, `IFilterFactory` la interfaz `CreateInstance` método se implementa para cargar el tipo especificado desde el contenedor de servicios (DI).

### <a name="typefilterattribute"></a>TypeFilterAttribute

`TypeFilterAttribute`es muy similar a `ServiceFilterAttribute` (y también implementa `IFilterFactory`), pero su tipo no se resuelve directamente desde el contenedor DI. En su lugar, crea una instancia del tipo mediante el uso de `Microsoft.Extensions.DependencyInjection.ObjectFactory`.

Debido a esta diferencia, tipos que se hace referencia mediante el `TypeFilterAttribute` no tienen que ser registrados con el contenedor en primer lugar (pero todavía tendrán sus dependencias cumplirlas con el contenedor). Además, `TypeFilterAttribute` , opcionalmente, puede aceptar argumentos de constructor para el tipo en cuestión. En el ejemplo siguiente se muestra cómo pasar argumentos a un tipo mediante `TypeFilterAttribute`:

[!code-csharp[Main](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

Si tiene un filtro que no requiere ningún argumento, pero que tiene dependencias de constructor que deba ser completada por DI, puede usar su propio atributo con nombre en las clases y métodos en lugar de `[TypeFilter(typeof(FilterType))]`). El filtro siguiente muestra cómo se puede implementar esto:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

Este filtro se puede aplicar a clases o métodos que utilizan el `[SampleActionFilter]` sintaxis, en lugar de tener que usar `[TypeFilter]` o `[ServiceFilter]`.

## <a name="authorization-filters"></a>Filtros de autorización

*Los filtros de autorización* controlar el acceso a los métodos de acción y el primer filtros para su ejecución en la canalización de filtro. Solo tienen un antes de método, a diferencia de la mayoría de los filtros que admiten antes y después de métodos. Sólo debe escribir un filtro de autorización personalizada si está escribiendo su propio marco de autorización. Prefiere configurar las directivas de autorización o escribir una directiva de autorización personalizada frente a escribir un filtro personalizado. La implementación de filtro integrado es solo responsable de la llamada del sistema de autorización.

Tenga en cuenta que no deben producir excepciones en los filtros de autorización, ya que no hay nada controlará la excepción (filtros de excepciones no controlen). En su lugar, emita una respuesta de desafío o encontrar otra manera.

Obtenga más información sobre [autorización](../../security/authorization/index.md).

## <a name="resource-filters"></a>Filtros de recursos

*Filtros de recurso* implementan la `IResourceFilter` o `IAsyncResourceFilter` interfaz y su ejecución ajusta la mayor parte de la canalización de filtro. (Solo [filtros de autorización](#authorization-filters) ejecutar antes de ellos.) Filtros de recursos son especialmente útiles si tiene que la mayoría del trabajo que se está realizando una solicitud de cortocircuito. Por ejemplo, un filtro de almacenamiento en caché puede evitar el resto de la canalización si la respuesta está en la memoria caché.

El [filtro de recursos de evaluación "cortocircuitada" short](#short-circuiting-resource-filter) mostrado anteriormente es un ejemplo de un filtro de recurso. Otro ejemplo es [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs), lo que impide que el enlace de modelos tengan acceso a los datos del formulario. Es útil para los casos donde se sabe que va a recibir grandes cargas de archivos y desea impedir que el formulario que se va a leer en la memoria.

## <a name="action-filters"></a>Filtros de acción

*Filtros de acción* implementan la `IActionFilter` o `IAsyncActionFilter` interfaz y su ejecución rodea la ejecución de métodos de acción.

Este es un filtro de acción de ejemplo:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

El [ResultExecutingContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutingcontext) proporciona las siguientes propiedades:

* `ActionArguments`-le permite manipular las entradas a la acción.
* `Controller`-le permite manipular la instancia del controlador. 
* `Result`-Si se establece cortocircuita la ejecución del método de acción y filtros de acción posterior. Producir una excepción también impide la ejecución del método de acción y los filtros siguientes, pero se trata como un error en lugar de un resultado correcto.

El [ActionExecutedContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutedcontext) proporciona `Controller` y `Result` además de las siguientes propiedades:

* `Canceled`-será true si la ejecución de la acción se cortocircuitado por otro filtro.
* `Exception`-será distinto de null si la acción o un filtro de acción posteriores produjo una excepción. Al establecer esta propiedad en null de forma eficaz 'handles' una excepción, y `Result` se ejecutará como si se han devuelto desde el método de acción normalmente.

Para una `IAsyncActionFilter`, una llamada a la `ActionExecutionDelegate` ejecuta cualquier filtro de acción posterior y el método de acción, devolver un `ActionExecutedContext`. De cortocircuito, asignar `ActionExecutingContext.Result` a algunos de los resultados de la instancia y no llame a la `ActionExecutionDelegate`.

El marco de trabajo proporciona un resumen `ActionFilterAttribute` que puede crear subclases. 

Puede utilizar un filtro de acción para validar el estado del modelo y devolver los errores si el estado es válido automáticamente:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

El `OnActionExecuted` método ejecuta después del método de acción y puede ver y manipular los resultados de la acción a través de la `ActionExecutedContext.Result` propiedad. `ActionExecutedContext.Canceled`se establecerá en true si la ejecución de la acción se cortocircuitado por otro filtro. `ActionExecutedContext.Exception`se establecerá en un valor distinto de null si la acción o un filtro de acción posteriores produjo una excepción. Establecer `ActionExecutedContext.Exception` en null de forma eficaz 'handles' una excepción, y `ActionExectedContext.Result` , a continuación, se ejecutará como si se han devuelto desde el método de acción normalmente.

## <a name="exception-filters"></a>Filtros de excepciones

*Los filtros de excepción* implementan la `IExceptionFilter` o `IAsyncExceptionFilter` interfaz. Puede utilizar para implementar directivas para una aplicación de control de errores comunes. 

El filtro de excepción de ejemplo siguiente utiliza una vista de error de programador personalizado para mostrar los detalles sobre las excepciones que se producen cuando la aplicación está en desarrollo:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

Filtros de excepciones no tiene dos eventos (para antes y después de): solo implementan `OnException` (o `OnExceptionAsync`). 

Los filtros de excepción controlan las excepciones no controladas que se producen en la creación del controlador, [enlace de modelo](../models/model-binding.md), filtros de acción o métodos de acción. No detectar las excepciones que se producen en los filtros de recursos, filtros de resultados o ejecución de resultado de MVC.

Para controlar una excepción, establezca la `ExceptionContext.ExceptionHandled` propiedad en true o escribir una respuesta. Esto detiene la propagación de la excepción. Tenga en cuenta que un filtro de excepción no se puede activar una excepción en un estado "correcto". Sólo un filtro de acción puede hacer eso.

> [!NOTE]
> En ASP.NET 1.1, no se envía la respuesta si establece `ExceptionHandled` en true **y** escribir una respuesta. En ese caso, ASP.NET Core 1.0 enviar la respuesta y ASP.NET Core 1.1.2 devolverá al 1.0 comportamiento. Para obtener más información, consulte [emitir #5594](https://github.com/aspnet/Mvc/issues/5594) en el repositorio de GitHub. 

Los filtros de excepción son buenos para la captura de excepciones que se producen dentro de las acciones de MVC, pero no son lo más flexibles middleware de control de errores. Prefiere middleware para el caso general y usar filtros sólo donde es necesario para realizar el control de errores *diferente* en función de qué acción de MVC se ha elegido. Por ejemplo, la aplicación podría tener métodos de acción para ambos extremos de la API y vistas/HTML. Los puntos de conexión de API pudieron devolver información de error como JSON, mientras que las acciones basadas en vistas pudieron devolver una página de error como HTML.

El marco de trabajo proporciona un resumen `ExceptionFilterAttribute` que puede crear subclases. 

## <a name="result-filters"></a>Filtros de resultados

*Como resultado filtros* implementan la `IResultFilter` o `IAsyncResultFilter` interfaz y su ejecución rodea la ejecución de los resultados de la acción. 

Este es un ejemplo de un filtro de resultado que agrega un encabezado HTTP.

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

El tipo de resultado que se está ejecutando depende de la acción en cuestión. Una acción de MVC devuelve una vista incluye todos los razor procesamiento como parte de la `ViewResult` que se está ejecutando. Un método de API puede llevar a cabo algunas serialización como parte de la ejecución del resultado. Obtenga más información sobre [resultados de acción](actions.md)

Filtros de resultados solo se ejecutan para resultados correctos - cuando la acción o filtros de acción no generan un resultado de acción. Filtros de resultados no se ejecutan cuando los filtros de excepción controlen una excepción.

El `OnResultExecuting` método de ejecución de los filtros de resultados siguientes y el resultado de la acción puede cortocircuito estableciendo `ResultExecutingContext.Cancel` en true. Por lo general debe escribir en el objeto de respuesta cuando evaluación "cortocircuitada" para evitar la generación de una respuesta vacía. Producir una excepción también impedirá la ejecución de los filtros subsiguientes y el resultado de la acción, pero se tratará como un error en lugar de un resultado correcto.

Cuando el `OnResultExecuted` método ejecuciones, la respuesta es probable que se ha enviado al cliente y no se puede cambiar más (a menos que se inició una excepción). `ResultExecutedContext.Canceled`se establecerá en true si la ejecución del resultado de acción se cortocircuitado por otro filtro.

`ResultExecutedContext.Exception`se establecerá en un valor distinto de null si el resultado de acción o un filtro de resultados siguientes produjo una excepción. Establecer `Exception` a null eficazmente 'handles' una excepción y evita que la excepción que se va a MVC vuelve a producir más adelante en la canalización. Cuando se está administrando una excepción en un filtro de resultado, no puede escribir datos en la respuesta. Si el resultado de acción mitad produce a través de su ejecución, y los encabezados ya se han vaciado en el cliente, no hay ningún mecanismo confiable para enviar un código de error.

Para una `IAsyncResultFilter` una llamada a `await next()` en el `ResultExecutionDelegate` ejecuta cualquier filtro de resultados siguientes y el resultado de acción. De cortocircuito, establezca `ResultExecutingContext.Cancel` en true y no llame a la `ResultExectionDelegate`.

El marco de trabajo proporciona un resumen `ResultFilterAttribute` que puede crear subclases. El [AddHeaderAttribute](#add-header-attribute) clase mostrado anteriormente es un ejemplo de un atributo de filtro de resultado.

## <a name="using-middleware-in-the-filter-pipeline"></a>Uso de middleware en la canalización de filtro

Filtros de recursos funcionan como [middleware](../../fundamentals/middleware.md) en que les rodea la ejecución de todo lo que se incluye más adelante en la canalización. Pero los filtros se diferencian de middleware en que forman parte de MVC, lo que significa que tienen acceso al contexto MVC y construcciones.

En ASP.NET Core 1.1, puede usar middleware en la canalización de filtro. Puede hacerlo si tiene un componente de middleware que necesita acceso a los datos de ruta MVC, o uno que se debe ejecutar solo para ciertos controladores o acciones.

Para usar el middleware como un filtro, crear un tipo con un `Configure` método que especifica el middleware que desea insertar en la canalización de filtro. Este es un ejemplo que utiliza el middleware de localización para establecer la referencia cultural actual para una solicitud:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

A continuación, puede usar el `MiddlewareFilterAttribute` para ejecutar middleware de una acción o controlador seleccionado o de manera global:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

Filtros de middleware que se ejecuten en la misma fase de la canalización de filtros como filtros de recursos, antes del enlace de modelo y después el resto de la canalización.

## <a name="next-actions"></a>Acciones siguientes

Para experimentar con los filtros, [descargar, probar y modificar el ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).
