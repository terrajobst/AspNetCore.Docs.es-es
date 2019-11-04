---
title: Control de errores en ASP.NET Core aplicaciones increíbles
author: guardrex
description: Descubra cómo ASP.NET Core increíble el grado de administración de las excepciones no controladas y cómo desarrollar aplicaciones que detecten y controlen los errores.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/31/2019
uid: blazor/handle-errors
ms.openlocfilehash: afcaa4d926c3e5f0a018897ce4b67b54574dae77
ms.sourcegitcommit: 77c8be22d5e88dd710f42c739748869f198865dd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2019
ms.locfileid: "73426983"
---
# <a name="handle-errors-in-aspnet-core-blazor-apps"></a>Control de errores en ASP.NET Core aplicaciones increíbles

Por [Steve Sanderson](https://github.com/SteveSandersonMS)

En este artículo se describe el modo en que el increíble grado de administración de las excepciones no controladas y el modo de desarrollar aplicaciones que detectan y controlan los errores.

## <a name="how-the-blazor-framework-reacts-to-unhandled-exceptions"></a>Cómo reacciona el marco de increíbles a las excepciones no controladas

El servidor es un marco con estado. Mientras los usuarios interactúan con una aplicación, mantienen una conexión con el servidor conocido como *circuito*. El circuito contiene instancias de componentes activas, además de muchos otros aspectos del estado, como:

* La salida representada más reciente de los componentes.
* Conjunto actual de delegados de control de eventos que pueden ser desencadenados por eventos del cliente.

Si un usuario abre la aplicación en varias pestañas del explorador, tiene varios circuitos independientes.

El increíbledor trata las excepciones no controladas como graves para el circuito en el que se producen. Si se termina un circuito debido a una excepción no controlada, el usuario solo puede seguir interactuando con la aplicación recargando la página para crear un nuevo circuito. Los circuitos fuera de la terminación, que son circuitos para otros usuarios u otras pestañas del explorador, no se ven afectados. Este escenario es similar a una aplicación de escritorio que se bloquea&mdash;se debe reiniciar la aplicación bloqueada, pero no se ven afectadas otras aplicaciones.

Un circuito finaliza cuando se produce una excepción no controlada por los siguientes motivos:

* Una excepción no controlada a menudo deja el circuito en un estado indefinido.
* No se puede garantizar la operación normal de la aplicación después de una excepción no controlada.
* Es posible que aparezcan vulnerabilidades de seguridad en la aplicación si el circuito continúa.

## <a name="manage-unhandled-exceptions-in-developer-code"></a>Administrar excepciones no controladas en el código de desarrollador

Para que una aplicación continúe después de un error, la aplicación debe tener una lógica de control de errores. En secciones posteriores de este artículo se describen posibles orígenes de excepciones no controladas.

En producción, no se representan mensajes de excepción de marco o seguimientos de pila en la interfaz de usuario. La representación de mensajes de excepción o seguimientos de la pila podría:

* Divulgar información confidencial a los usuarios finales.
* Ayudar a un usuario malintencionado a detectar debilidades en una aplicación que puede poner en peligro la seguridad de la aplicación, el servidor o la red.

## <a name="log-errors-with-a-persistent-provider"></a>Registrar errores con un proveedor persistente

Si se produce una excepción no controlada, la excepción se registra en <xref:Microsoft.Extensions.Logging.ILogger> las instancias configuradas en el contenedor de servicios. De forma predeterminada, las aplicaciones increíbles se registran en la salida de la consola con el proveedor de registro de la consola. Considere la posibilidad de iniciar sesión en una ubicación más permanente con un proveedor que administre el tamaño y la rotación del registro. Para obtener más información, vea <xref:fundamentals/logging/index>.

Durante el desarrollo, el increíble normalmente envía los detalles completos de las excepciones a la consola del explorador para ayudar en la depuración. En producción, los errores detallados en la consola del explorador están deshabilitados de forma predeterminada, lo que significa que los errores no se envían a los clientes, pero los detalles completos de la excepción siguen registrándose en el lado servidor. Para obtener más información, vea <xref:fundamentals/error-handling>.

Debe decidir qué incidentes registrar y el nivel de gravedad de los incidentes registrados. Es posible que los usuarios hostiles puedan desencadenar errores deliberadamente. Por ejemplo, no registre un incidente de un error en el que se proporcione un `ProductId` desconocido en la dirección URL de un componente que muestra los detalles del producto. No todos los errores se deben tratar como incidentes de alta gravedad para el registro.

## <a name="places-where-errors-may-occur"></a>Lugares donde se pueden producir errores

El código de marco de trabajo y la aplicación pueden desencadenar excepciones no controladas en cualquiera de las siguientes ubicaciones:

* [Creación de instancias de componentes](#component-instantiation)
* [Métodos de ciclo de vida](#lifecycle-methods)
* [Lógica de representación](#rendering-logic)
* [Controladores de eventos](#event-handlers)
* [Eliminación de componentes](#component-disposal)
* [Interoperabilidad de JavaScript](#javascript-interop)
* [Controladores de circuito](#circuit-handlers)
* [Eliminación de circuitos](#circuit-disposal)
* [Representación previa](#prerendering)

Las excepciones no controladas anteriores se describen en las siguientes secciones de este artículo.

### <a name="component-instantiation"></a>Creación de instancias de componentes

Cuando el increíbles crea una instancia de un componente:

* Se invoca el constructor del componente.
* Se invocan los constructores de cualquier servicio de DI no singleton proporcionado al constructor del componente a través de la directiva [@inject](xref:blazor/dependency-injection#request-a-service-in-a-component) o el atributo [[Insert]](xref:blazor/dependency-injection#request-a-service-in-a-component) . 

Se produce un error en un circuito cuando cualquier constructor ejecutado o un establecedor para cualquier propiedad `[Inject]` produce una excepción no controlada. La excepción es grave porque el marco no puede crear una instancia del componente. Si la lógica del constructor puede producir excepciones, la aplicación debe interceptar las excepciones mediante una instrucción [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con control de errores y registro.

### <a name="lifecycle-methods"></a>Métodos de ciclo de vida

Durante el ciclo de vida de un componente, el increíblemente llama a los métodos de ciclo de vida:

* `OnInitialized` / `OnInitializedAsync`
* `OnParametersSet` / `OnParametersSetAsync`
* `ShouldRender` / `ShouldRenderAsync`
* `OnAfterRender` / `OnAfterRenderAsync`

Si cualquier método de ciclo de vida produce una excepción, de forma sincrónica o asincrónica, la excepción es grave para el circuito. Para que los componentes traten los errores de los métodos de ciclo de vida, agregue la lógica de control de errores.

En el ejemplo siguiente, donde `OnParametersSetAsync` llama a un método para obtener un producto:

* Una instrucción `try-catch` controla una excepción que se produce en el método `ProductRepository.GetProductByIdAsync`.
* Cuando se ejecuta el bloque de `catch`:
  * `loadFailed` se establece en `true`, que se usa para mostrar un mensaje de error al usuario.
  * El error se registra.

[!code-cshtml[](handle-errors/samples_snapshot/3.x/product-details.razor?highlight=11,27-39)]

### <a name="rendering-logic"></a>Lógica de representación

El marcado declarativo de un archivo de componente de `.razor` C# se compila en un método denominado `BuildRenderTree`. Cuando se representa un componente, `BuildRenderTree` ejecuta y genera una estructura de datos que describe los elementos, el texto y los componentes secundarios del componente representado.

La lógica de representación puede producir una excepción. Un ejemplo de este escenario se produce cuando se evalúa `@someObject.PropertyName` pero `@someObject` es `null`. Una excepción no controlada producida por la lógica de representación es grave para el circuito.

Para evitar una excepción de referencia nula en la lógica de representación, busque un objeto `null` antes de tener acceso a sus miembros. En el ejemplo siguiente, no se tiene acceso a las propiedades de `person.Address` si `person.Address` se `null`:

[!code-cshtml[](handle-errors/samples_snapshot/3.x/person-example.razor?highlight=1)]

En el código anterior se supone que `person` no está `null`. A menudo, la estructura del código garantiza que un objeto existe en el momento en que se representa el componente. En esos casos, no es necesario comprobar `null` en la lógica de representación. En el ejemplo anterior, es posible que se garantice la existencia de `person` porque `person` se crea cuando se crea una instancia del componente.

### <a name="event-handlers"></a>Controladores de eventos

El código del lado cliente desencadena invocaciones de código C# cuando se crean controladores de eventos mediante:

* `@onclick`
* `@onchange`
* Otros atributos de `@on...`
* `@bind`

El código del controlador de eventos podría producir una excepción no controlada en estos escenarios.

Si un controlador de eventos produce una excepción no controlada (por ejemplo, se produce un error en una consulta de base de datos), la excepción es grave para el circuito. Si la aplicación llama a código que podría dar error por motivos externos, Capture las excepciones mediante una instrucción [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con control de errores y registro.

Si el código de usuario no intercepta y controla la excepción, el marco de trabajo registra la excepción y finaliza el circuito.

### <a name="component-disposal"></a>Eliminación de componentes

Un componente se puede quitar de la interfaz de usuario, por ejemplo, porque el usuario ha navegado a otra página. Cuando un componente que implementa <xref:System.IDisposable?displayProperty=fullName> se quita de la interfaz de usuario, el marco de trabajo llama al método de <xref:System.IDisposable.Dispose*> del componente. 

Si el método `Dispose` del componente produce una excepción no controlada, la excepción es grave para el circuito. Si la lógica de eliminación puede producir excepciones, la aplicación debe interceptar las excepciones mediante una instrucción [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con control de errores y registro.

Para obtener más información sobre la eliminación de componentes, consulte <xref:blazor/components#component-disposal-with-idisposable>.

### <a name="javascript-interop"></a>Interoperabilidad de JavaScript

`IJSRuntime.InvokeAsync<T>` permite que el código .NET realice llamadas asincrónicas al tiempo de ejecución de JavaScript en el explorador del usuario.

Las condiciones siguientes se aplican al control de errores con `InvokeAsync<T>`:

* Si una llamada a `InvokeAsync<T>` produce un error sincrónicamente, se produce una excepción de .NET. Se puede producir un error en una llamada a `InvokeAsync<T>`, por ejemplo, porque no se pueden serializar los argumentos proporcionados. El código del desarrollador debe detectar la excepción. Si el código de la aplicación en un controlador de eventos o un método de ciclo de vida de componente no controla una excepción, la excepción resultante es grave para el circuito.
* Si se produce un error en una llamada a `InvokeAsync<T>` de forma asincrónica, se produce un error en .NET <xref:System.Threading.Tasks.Task>. Una llamada a `InvokeAsync<T>` puede producir un error, por ejemplo, porque el código de JavaScript produce una excepción o devuelve un `Promise` que se ha completado como `rejected`. El código del desarrollador debe detectar la excepción. Si usa el operador [Await](/dotnet/csharp/language-reference/keywords/await) , considere la posibilidad de encapsular la llamada al método en una instrucción [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con control de errores y registro. De lo contrario, el código que genera el error provoca una excepción no controlada que es grave para el circuito.
* De forma predeterminada, las llamadas a `InvokeAsync<T>` deben completarse en un período determinado o, de lo contrario, se agota el tiempo de espera de la llamada. El período de tiempo de espera predeterminado es de un minuto. El tiempo de espera protege el código contra una pérdida en la conectividad de red o código JavaScript que nunca devuelve un mensaje de finalización. Si se agota el tiempo de espera de la llamada, se produce un error en el `Task` resultante con un <xref:System.OperationCanceledException>. Capture y procese la excepción con el registro.

Del mismo modo, el código de JavaScript puede iniciar llamadas a métodos .NET indicados por el [atributo [JSInvokable]](xref:blazor/javascript-interop#invoke-net-methods-from-javascript-functions). Si estos métodos .NET producen una excepción no controlada:

* La excepción no se trata como grave para el circuito.
* Se rechaza el `Promise` del lado de JavaScript.

Tiene la opción de usar el código de control de errores en el lado de .NET o en el lado de JavaScript de la llamada al método.

Para obtener más información, vea <xref:blazor/javascript-interop>.

### <a name="circuit-handlers"></a>Controladores de circuito

Increíbles permite que el código defina un *controlador de circuito*, que recibe notificaciones cuando cambia el estado del circuito de un usuario. Se utilizan los siguientes Estados:

* `initialized`
* `connected`
* `disconnected`
* `disposed`

Las notificaciones se administran registrando un servicio DI que hereda de la `CircuitHandler` clase base abstracta.

Si los métodos de un controlador de circuito personalizado producen una excepción no controlada, la excepción es grave para el circuito. Para tolerar excepciones en el código de un controlador o en métodos denominados, ajuste el código en una o varias instrucciones [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con control de errores y registro.

### <a name="circuit-disposal"></a>Eliminación de circuitos

Cuando un circuito finaliza porque un usuario se ha desconectado y el marco está limpiando el estado del circuito, el marco de trabajo desecha el ámbito de DI del circuito. Al desechar el ámbito, se eliminan todos los servicios DI de ámbito de circuito que implementan <xref:System.IDisposable?displayProperty=fullName>. Si un servicio DI produce una excepción no controlada durante la eliminación, el marco de trabajo registra la excepción.

### <a name="prerendering"></a>Representación previa

Los componentes increíbles se pueden representar con `Html.RenderComponentAsync` de modo que su marcado HTML representado se devuelva como parte de la solicitud HTTP inicial del usuario. Esto funciona de la siguiente manera:

* Crear un circuito nuevo para todos los componentes prerepresentados que forman parte de la misma página.
* Generar el código HTML inicial.
* Tratamiento del circuito como `disconnected` hasta que el explorador del usuario establece una conexión Signalr en el mismo servidor. Cuando se establece la conexión, se reanuda la interactividad en el circuito y se actualiza el marcado HTML de los componentes.

Si algún componente produce una excepción no controlada durante la representación previa, por ejemplo, durante un método de ciclo de vida o en la lógica de representación:

* La excepción es grave para el circuito.
* La excepción se inicia en la pila de llamadas de la llamada `Html.RenderComponentAsync`. Por lo tanto, se produce un error en toda la solicitud HTTP a menos que el código del desarrollador detecte la excepción explícitamente.

En circunstancias normales, cuando se produce un error en la representación previa, la generación y representación del componente no tiene sentido, ya que no se puede representar un componente de trabajo.

Para tolerar los errores que pueden producirse durante la representación previa, la lógica de control de errores debe colocarse dentro de un componente que pueda producir excepciones. Use instrucciones [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con control de errores y registro. En lugar de encapsular la llamada a `RenderComponentAsync` en una instrucción de `try-catch`, coloque la lógica de control de errores en el componente representado por `RenderComponentAsync`.

## <a name="advanced-scenarios"></a>Escenarios avanzados

### <a name="recursive-rendering"></a>Representación recursiva

Los componentes se pueden anidar de forma recursiva. Esto resulta útil para representar estructuras de datos recursivas. Por ejemplo, un componente de `TreeNode` puede representar más componentes `TreeNode` para cada uno de los elementos secundarios del nodo.

Cuando se represente de forma recursiva, evite patrones de codificación que produzcan una recursividad infinita:

* No represente de forma recursiva una estructura de datos que contenga un ciclo. Por ejemplo, no represente un nodo de árbol cuyos elementos secundarios se incluyan a sí mismos.
* No cree una cadena de diseños que contengan un ciclo. Por ejemplo, no cree un diseño cuyo diseño sea el mismo.
* No permita que un usuario final viole invariantes de recursividad (reglas) a través de entradas de datos malintencionadas o llamadas de interoperabilidad de JavaScript.

Bucles infinitos durante la representación:

* Hace que el proceso de representación continúe indefinidamente.
* Es equivalente a crear un bucle sin terminar.

En estos casos, el circuito afectado se bloquea y el subproceso normalmente intenta:

* Consume el tiempo de CPU permitido por el sistema operativo, indefinidamente.
* Consume una cantidad ilimitada de memoria del servidor. Consumir memoria ilimitada es equivalente al escenario en el que un bucle sin terminar agrega entradas a una colección en cada iteración.

Para evitar patrones infinitos de recursividad, asegúrese de que el código de representación recursivo contiene condiciones de detención adecuadas.

### <a name="custom-render-tree-logic"></a>Lógica de árbol de representación personalizada

La mayoría de los componentes más increíbles se implementan como archivos *. Razor* y se compilan para generar lógica que opere en un `RenderTreeBuilder` para representar la salida. Un desarrollador puede implementar manualmente `RenderTreeBuilder` lógica mediante código C# de procedimiento. Para obtener más información, vea <xref:blazor/components#manual-rendertreebuilder-logic>.

> [!WARNING]
> El uso de la lógica del generador de árboles de representación manual se considera un escenario avanzado y no seguro, no recomendado para el desarrollo de componentes generales.

Si se escribe `RenderTreeBuilder` código, el desarrollador debe garantizar la corrección del código. Por ejemplo, el desarrollador debe asegurarse de que:

* Las llamadas a `OpenElement` y `CloseElement` están equilibradas correctamente.
* Los atributos solo se agregan en los lugares correctos.

Una lógica incorrecta del generador de árboles de representación manual puede producir un comportamiento indefinido arbitrario, incluidos los bloqueos, los bloqueos del servidor y las vulnerabilidades de seguridad.

Considere la posibilidad de representar la lógica del generador de árbol de representación manual en el mismo nivel de complejidad y con el mismo nivel de *riesgo* que escribir manualmente código de ensamblado o instrucciones MSIL.
