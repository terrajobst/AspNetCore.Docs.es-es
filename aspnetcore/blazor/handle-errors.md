---
title: Control de errores en las aplicaciones de Blazor de ASP.NET Core
author: guardrex
description: Descubra cómo ASP.NET Core Blazor cómo Blazor administra las excepciones no controladas y cómo desarrollar aplicaciones que detecten y controlen los errores.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
no-loc:
- Blazor
- SignalR
uid: blazor/handle-errors
ms.openlocfilehash: b987513e5410e95ab632b9935d858b648838d94f
ms.sourcegitcommit: 0b0e485a8a6dfcc65a7a58b365622b3839f4d624
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2020
ms.locfileid: "76928266"
---
# <a name="handle-errors-in-aspnet-core-opno-locblazor-apps"></a>Control de errores en las aplicaciones de Blazor de ASP.NET Core

Por [Steve Sanderson](https://github.com/SteveSandersonMS)

En este artículo se describe cómo Blazor administra las excepciones no controladas y cómo desarrollar las aplicaciones que detectan y controlan los errores.

## <a name="detailed-errors-during-development"></a>Errores detallados durante el desarrollo

Cuando una aplicación Blazor no funciona correctamente durante el desarrollo, recibir información detallada del error de la aplicación ayuda a solucionar el problema. Cuando se produce un error, en las aplicaciones Blazor se muestra una barra dorada en la parte inferior de la pantalla:

* Durante el desarrollo, la barra dorada le dirige a la consola del explorador, donde puede ver la excepción.
* En producción, la barra dorada informa al usuario de que se ha producido un error y recomienda actualizar el explorador.

La interfaz de usuario para esta experiencia de control de errores forma parte de las plantillas de proyecto de Blazor.

En una aplicación Blazor webassembly, personalice la experiencia en el archivo *wwwroot/index.html* :

```html
<div id="blazor-error-ui">
    An unhandled error has occurred.
    <a href="" class="reload">Reload</a>
    <a class="dismiss">🗙</a>
</div>
```

En una aplicación de Blazor Server, personalice la experiencia en el archivo *pages/_Host. cshtml* :

```cshtml
<div id="blazor-error-ui">
    <environment include="Staging,Production">
        An error has occurred. This application may no longer respond until reloaded.
    </environment>
    <environment include="Development">
        An unhandled exception has occurred. See browser dev tools for details.
    </environment>
    <a href="" class="reload">Reload</a>
    <a class="dismiss">🗙</a>
</div>
```

Los estilos incluidos en las plantillas de Blazor ocultan el elemento `blazor-error-ui` y, a continuación, se muestra cuando se produce un error.

## <a name="how-a-opno-locblazor-server-app-reacts-to-unhandled-exceptions"></a>Cómo reacciona una aplicación de Blazor Server a las excepciones no controladas

Blazor Server es un marco con estado. Mientras los usuarios interactúan con una aplicación, mantienen una conexión con el servidor conocido como *circuito*. El circuito contiene instancias de componentes activas, además de muchos otros aspectos del estado, como:

* La salida representada más reciente de los componentes.
* Conjunto actual de delegados de control de eventos que pueden ser desencadenados por eventos del cliente.

Si un usuario abre la aplicación en varias pestañas del explorador, tiene varios circuitos independientes.

Blazor trata la mayoría de las excepciones no controladas como graves para el circuito en el que se producen. Si se termina un circuito debido a una excepción no controlada, el usuario solo puede seguir interactuando con la aplicación recargando la página para crear un nuevo circuito. Los circuitos fuera de la terminación, que son circuitos para otros usuarios u otras pestañas del explorador, no se ven afectados. Este escenario es similar a una aplicación de escritorio que se bloquea&mdash;se debe reiniciar la aplicación bloqueada, pero no se ven afectadas otras aplicaciones.

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

Si se produce una excepción no controlada, la excepción se registra en <xref:Microsoft.Extensions.Logging.ILogger> las instancias configuradas en el contenedor de servicios. De forma predeterminada, Blazor aplicaciones registran la salida de la consola con el proveedor de registro de la consola. Considere la posibilidad de iniciar sesión en una ubicación más permanente con un proveedor que administre el tamaño y la rotación del registro. Para obtener más información, vea <xref:fundamentals/logging/index>.

Durante el desarrollo, Blazor envía normalmente los detalles completos de las excepciones a la consola del explorador para ayudar en la depuración. En producción, los errores detallados en la consola del explorador están deshabilitados de forma predeterminada, lo que significa que los errores no se envían a los clientes, pero los detalles completos de la excepción siguen registrándose en el lado servidor. Para obtener más información, vea <xref:fundamentals/error-handling>.

Debe decidir qué incidentes registrar y el nivel de gravedad de los incidentes registrados. Es posible que los usuarios hostiles puedan desencadenar errores deliberadamente. Por ejemplo, no registre un incidente de un error en el que se proporcione un `ProductId` desconocido en la dirección URL de un componente que muestra los detalles del producto. No todos los errores se deben tratar como incidentes de alta gravedad para el registro.

## <a name="places-where-errors-may-occur"></a>Lugares donde se pueden producir errores

El código de marco de trabajo y la aplicación pueden desencadenar excepciones no controladas en cualquiera de las siguientes ubicaciones:

* [Creación de instancias de componentes](#component-instantiation)
* [Métodos de ciclo de vida](#lifecycle-methods)
* [Lógica de representación](#rendering-logic)
* [Controladores de eventos](#event-handlers)
* [Eliminación de componentes](#component-disposal)
* [Interoperabilidad de JavaScript](#javascript-interop)
* [Controladores de circuitos de Blazor Server](#blazor-server-circuit-handlers)
* [eliminación de circuitos de Blazor Server](#blazor-server-circuit-disposal)
* [rerepresentación de Blazor Server](#blazor-server-prerendering)

Las excepciones no controladas anteriores se describen en las siguientes secciones de este artículo.

### <a name="component-instantiation"></a>Creación de instancias de componentes

Cuando Blazor crea una instancia de un componente:

* Se invoca el constructor del componente.
* Se invocan los constructores de cualquier servicio de DI no singleton proporcionado al constructor del componente mediante la directiva [`@inject`](xref:blazor/dependency-injection#request-a-service-in-a-component) o el atributo [`[Inject]`](xref:blazor/dependency-injection#request-a-service-in-a-component) .

Se produce un error en un circuito de Blazor Server cuando cualquier constructor ejecutado o un establecedor para cualquier propiedad `[Inject]` produce una excepción no controlada. La excepción es grave porque el marco no puede crear una instancia del componente. Si la lógica del constructor puede producir excepciones, la aplicación debe interceptar las excepciones mediante una instrucción [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con control de errores y registro.

### <a name="lifecycle-methods"></a>Métodos de ciclo de vida

Durante la vigencia de un componente, Blazor invoca los siguientes [métodos de ciclo de vida](xref:blazor/lifecycle):

* `OnInitialized` / `OnInitializedAsync`
* `OnParametersSet` / `OnParametersSetAsync`
* `ShouldRender` / `ShouldRenderAsync`
* `OnAfterRender` / `OnAfterRenderAsync`

Si cualquier método de ciclo de vida produce una excepción, de forma sincrónica o asincrónica, la excepción es grave para un circuito de Blazor Server. Para que los componentes traten los errores de los métodos de ciclo de vida, agregue la lógica de control de errores.

En el ejemplo siguiente, donde `OnParametersSetAsync` llama a un método para obtener un producto:

* Una instrucción `try-catch` controla una excepción que se produce en el método `ProductRepository.GetProductByIdAsync`.
* Cuando se ejecuta el bloque de `catch`:
  * `loadFailed` se establece en `true`, que se usa para mostrar un mensaje de error al usuario.
  * El error se registra.

[!code-razor[](handle-errors/samples_snapshot/3.x/product-details.razor?highlight=11,27-39)]

### <a name="rendering-logic"></a>Lógica de representación

El marcado declarativo de un archivo de componente de `.razor` C# se compila en un método denominado `BuildRenderTree`. Cuando se representa un componente, `BuildRenderTree` ejecuta y genera una estructura de datos que describe los elementos, el texto y los componentes secundarios del componente representado.

La lógica de representación puede producir una excepción. Un ejemplo de este escenario se produce cuando se evalúa `@someObject.PropertyName` pero `@someObject` es `null`. Una excepción no controlada producida por la lógica de representación es grave para un circuito de Blazor Server.

Para evitar una excepción de referencia nula en la lógica de representación, busque un objeto `null` antes de tener acceso a sus miembros. En el ejemplo siguiente, no se tiene acceso a las propiedades de `person.Address` si `person.Address` se `null`:

[!code-razor[](handle-errors/samples_snapshot/3.x/person-example.razor?highlight=1)]

En el código anterior se supone que `person` no está `null`. A menudo, la estructura del código garantiza que un objeto existe en el momento en que se representa el componente. En esos casos, no es necesario comprobar `null` en la lógica de representación. En el ejemplo anterior, es posible que se garantice la existencia de `person` porque `person` se crea cuando se crea una instancia del componente.

### <a name="event-handlers"></a>Controladores de eventos

El código del lado cliente desencadena invocaciones de código C# cuando se crean controladores de eventos mediante:

* `@onclick`
* `@onchange`
* Otros atributos de `@on...`
* `@bind`

El código del controlador de eventos podría producir una excepción no controlada en estos escenarios.

Si un controlador de eventos produce una excepción no controlada (por ejemplo, se produce un error en una consulta de base de datos), la excepción es grave para un circuito de Blazor Server. Si la aplicación llama a código que podría dar error por motivos externos, Capture las excepciones mediante una instrucción [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con control de errores y registro.

Si el código de usuario no intercepta y controla la excepción, el marco de trabajo registra la excepción y finaliza el circuito.

### <a name="component-disposal"></a>Eliminación de componentes

Un componente se puede quitar de la interfaz de usuario, por ejemplo, porque el usuario ha navegado a otra página. Cuando un componente que implementa <xref:System.IDisposable?displayProperty=fullName> se quita de la interfaz de usuario, el marco de trabajo llama al método de <xref:System.IDisposable.Dispose*> del componente.

Si el método `Dispose` del componente produce una excepción no controlada, la excepción es grave para un circuito de Blazor Server. Si la lógica de eliminación puede producir excepciones, la aplicación debe interceptar las excepciones mediante una instrucción [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con control de errores y registro.

Para obtener más información sobre la eliminación de componentes, consulte <xref:blazor/lifecycle#component-disposal-with-idisposable>.

### <a name="javascript-interop"></a>Interoperabilidad de JavaScript

`IJSRuntime.InvokeAsync<T>` permite que el código .NET realice llamadas asincrónicas al tiempo de ejecución de JavaScript en el explorador del usuario.

Las condiciones siguientes se aplican al control de errores con `InvokeAsync<T>`:

* Si una llamada a `InvokeAsync<T>` produce un error sincrónicamente, se produce una excepción de .NET. Se puede producir un error en una llamada a `InvokeAsync<T>`, por ejemplo, porque no se pueden serializar los argumentos proporcionados. El código del desarrollador debe detectar la excepción. Si el código de la aplicación en un controlador de eventos o en un método de ciclo de vida de componente no controla una excepción, la excepción resultante es grave para un circuito de Blazor Server.
* Si se produce un error en una llamada a `InvokeAsync<T>` de forma asincrónica, se produce un error en .NET <xref:System.Threading.Tasks.Task>. Una llamada a `InvokeAsync<T>` puede producir un error, por ejemplo, porque el código de JavaScript produce una excepción o devuelve un `Promise` que se ha completado como `rejected`. El código del desarrollador debe detectar la excepción. Si usa el operador [Await](/dotnet/csharp/language-reference/keywords/await) , considere la posibilidad de encapsular la llamada al método en una instrucción [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con control de errores y registro. De lo contrario, el código que genera el error provoca una excepción no controlada que es grave para un circuito de Blazor Server.
* De forma predeterminada, las llamadas a `InvokeAsync<T>` deben completarse en un período determinado o, de lo contrario, se agota el tiempo de espera de la llamada. El período de tiempo de espera predeterminado es de un minuto. El tiempo de espera protege el código contra una pérdida en la conectividad de red o código JavaScript que nunca devuelve un mensaje de finalización. Si se agota el tiempo de espera de la llamada, se produce un error en el `Task` resultante con un <xref:System.OperationCanceledException>. Capture y procese la excepción con el registro.

Del mismo modo, el código de JavaScript puede iniciar llamadas a métodos .NET indicados por el atributo [`[JSInvokable]`](xref:blazor/javascript-interop#invoke-net-methods-from-javascript-functions) . Si estos métodos .NET producen una excepción no controlada:

* La excepción no se trata como grave para un circuito de Blazor Server.
* Se rechaza el `Promise` del lado de JavaScript.

Tiene la opción de usar el código de control de errores en el lado de .NET o en el lado de JavaScript de la llamada al método.

Para obtener más información, vea <xref:blazor/javascript-interop>.

### <a name="opno-locblazor-server-circuit-handlers"></a>Controladores de circuitos de Blazor Server

Blazor Server permite al código definir un *controlador de circuito*, lo que permite ejecutar el código en los cambios realizados en el estado del circuito de un usuario. Un controlador de circuito se implementa derivando de `CircuitHandler` y registrando la clase en el contenedor de servicios de la aplicación. En el siguiente ejemplo de un controlador de circuito se realiza un seguimiento de las conexiones SignalR abiertas:

```csharp
using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.Server.Circuits;

public class TrackingCircuitHandler : CircuitHandler
{
    private HashSet<Circuit> _circuits = new HashSet<Circuit>();

    public override Task OnConnectionUpAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        _circuits.Add(circuit);

        return Task.CompletedTask;
    }

    public override Task OnConnectionDownAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        _circuits.Remove(circuit);

        return Task.CompletedTask;
    }

    public int ConnectedCircuits => _circuits.Count;
}
```

Los controladores de circuito se registran mediante DI. Las instancias con ámbito se crean por instancia de un circuito. Con el `TrackingCircuitHandler` en el ejemplo anterior, se crea un servicio singleton porque se debe realizar un seguimiento del estado de todos los circuitos:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...
    services.AddSingleton<CircuitHandler, TrackingCircuitHandler>();
}
```

Si los métodos de un controlador de circuito personalizado producen una excepción no controlada, la excepción es grave para el circuito de Blazor Server. Para tolerar excepciones en el código de un controlador o en métodos denominados, ajuste el código en una o varias instrucciones [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con control de errores y registro.

### <a name="opno-locblazor-server-circuit-disposal"></a>eliminación de circuitos de Blazor Server

Cuando un circuito finaliza porque un usuario se ha desconectado y el marco está limpiando el estado del circuito, el marco de trabajo desecha el ámbito de DI del circuito. Al desechar el ámbito, se eliminan todos los servicios DI de ámbito de circuito que implementan <xref:System.IDisposable?displayProperty=fullName>. Si un servicio DI produce una excepción no controlada durante la eliminación, el marco de trabajo registra la excepción.

### <a name="opno-locblazor-server-prerendering"></a>preprocesamiento de Blazor Server

Blazor componentes se pueden representarse mediante la aplicación auxiliar de etiquetas `Component` de modo que su marca HTML representada se devuelva como parte de la solicitud HTTP inicial del usuario. Esto funciona de la siguiente manera:

* Crear un circuito nuevo para todos los componentes prerepresentados que forman parte de la misma página.
* Generar el código HTML inicial.
* Tratamiento del circuito como `disconnected` hasta que el explorador del usuario establece una conexión de SignalR al mismo servidor. Cuando se establece la conexión, se reanuda la interactividad en el circuito y se actualiza el marcado HTML de los componentes.

Si algún componente produce una excepción no controlada durante la representación previa, por ejemplo, durante un método de ciclo de vida o en la lógica de representación:

* La excepción es grave para el circuito.
* La excepción se inicia en la pila de llamadas de la aplicación auxiliar de etiquetas `Component`. Por lo tanto, se produce un error en toda la solicitud HTTP a menos que el código del desarrollador detecte la excepción explícitamente.

En circunstancias normales, cuando se produce un error en la representación previa, la generación y representación del componente no tiene sentido, ya que no se puede representar un componente de trabajo.

Para tolerar los errores que pueden producirse durante la representación previa, la lógica de control de errores debe colocarse dentro de un componente que pueda producir excepciones. Use instrucciones [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) con control de errores y registro. En lugar de ajustar la aplicación auxiliar de etiquetas `Component` en una instrucción `try-catch`, coloque la lógica de control de errores en el componente representado por la aplicación auxiliar de etiquetas `Component`.

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

En estos escenarios, se produce un error en un circuito de Blazor Server afectado y el subproceso normalmente intenta:

* Consume el tiempo de CPU permitido por el sistema operativo, indefinidamente.
* Consume una cantidad ilimitada de memoria del servidor. Consumir memoria ilimitada es equivalente al escenario en el que un bucle sin terminar agrega entradas a una colección en cada iteración.

Para evitar patrones infinitos de recursividad, asegúrese de que el código de representación recursivo contiene condiciones de detención adecuadas.

### <a name="custom-render-tree-logic"></a>Lógica de árbol de representación personalizada

La mayoría de los componentes de Blazor se implementan como archivos *. Razor* y se compilan para generar lógica que opere en un `RenderTreeBuilder` para representar su salida. Un desarrollador puede implementar manualmente `RenderTreeBuilder` lógica mediante código C# de procedimiento. Para obtener más información, vea <xref:blazor/components#manual-rendertreebuilder-logic>.

> [!WARNING]
> El uso de la lógica del generador de árboles de representación manual se considera un escenario avanzado y no seguro, no recomendado para el desarrollo de componentes generales.

Si se escribe `RenderTreeBuilder` código, el desarrollador debe garantizar la corrección del código. Por ejemplo, el desarrollador debe asegurarse de que:

* Las llamadas a `OpenElement` y `CloseElement` están equilibradas correctamente.
* Los atributos solo se agregan en los lugares correctos.

Una lógica incorrecta del generador de árboles de representación manual puede producir un comportamiento indefinido arbitrario, incluidos los bloqueos, los bloqueos del servidor y las vulnerabilidades de seguridad.

Considere la posibilidad de representar la lógica del generador de árbol de representación manual en el mismo nivel de complejidad y con el mismo nivel de *riesgo* que escribir manualmente código de ensamblado o instrucciones MSIL.
