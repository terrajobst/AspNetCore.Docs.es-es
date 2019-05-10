---
title: Realización de solicitudes HTTP mediante IHttpClientFactory en ASP.NET Core
author: stevejgordon
description: Obtenga información sobre cómo usar la interfaz IHttpClientFactory para administrar instancias de HttpClient lógicas en ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 03/30/2019
uid: fundamentals/http-requests
ms.openlocfilehash: 84cdbca20e7c7aaa1941c78483cc36a9f0b24505
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64888950"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a>Realización de solicitudes HTTP mediante IHttpClientFactory en ASP.NET Core

Por [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak) y [Steve Gordon](https://github.com/stevejgordon)

Se puede registrar y usar una interfaz <xref:System.Net.Http.IHttpClientFactory> para crear y configurar instancias de <xref:System.Net.Http.HttpClient> en una aplicación. Esto reporta las siguientes ventajas:

* Proporciona una ubicación central para denominar y configurar instancias de `HttpClient` lógicas. Así, por ejemplo, se puede registrar y configurar un cliente *github* para tener acceso a GitHub. y, de igual modo, registrar otro cliente predeterminado para otros fines.
* Codifica el concepto de middleware saliente a través de controladores de delegación en `HttpClient` y proporciona extensiones para middleware basado en Polly para poder sacar partido de este.
* Administra la agrupación y duración de las instancias de `HttpClientMessageHandler` subyacentes para evitar los problemas de DNS que suelen producirse al administrar las duraciones de `HttpClient` manualmente.
* Agrega una experiencia de registro configurable (a través de `ILogger`) en todas las solicitudes enviadas a través de los clientes creados por Factory.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Requisitos previos

Los proyectos para .NET Framework requieren instalar el paquete NuGet [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/). Los proyectos para .NET Core y que hagan referencia al [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ya incluyen el paquete `Microsoft.Extensions.Http`.

## <a name="consumption-patterns"></a>Patrones de consumo

`IHttpClientFactory` se puede usar de varias formas en una aplicación:

* [Uso básico](#basic-usage)
* [Clientes con nombre](#named-clients)
* [Clientes con tipo](#typed-clients)
* [Clientes generados](#generated-clients)

Ninguno de ellos es rigurosamente superior a otro, sino que el mejor método dependerá de las restricciones particulares de la aplicación.

### <a name="basic-usage"></a>Uso básico

Se puede registrar un `IHttpClientFactory` llamando al método de extensión `AddHttpClient` en `IServiceCollection`, dentro del método `Startup.ConfigureServices`.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

Una vez registrado, el código puede aceptar una interfaz `IHttpClientFactory` en cualquier parte donde se puedan insertar servicios por medio de la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection). El `IHttpClientFactory` se puede usar para crear una instancia de `HttpClient`:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

Cuando `IHttpClientFactory` se usa de este modo, constituye una excelente manera de refactorizar una aplicación existente. No tiene efecto alguno en la forma en que `HttpClient` se usa. En aquellos sitios en los que ya se hayan creado instancias de `HttpClient`, reemplace esas apariciones por una llamada a <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.

### <a name="named-clients"></a>Clientes con nombre

Si una aplicación necesita usar `HttpClient` de diversas maneras, cada una con una configuración diferente, una opción consiste en usar **clientes con nombre**. La configuración de un `HttpClient` con nombre se puede realizar durante la fase de registro en `Startup.ConfigureServices`.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

En el código anterior, se llama a `AddHttpClient` usando el nombre *github*. Este cliente tiene aplicadas algunas configuraciones predeterminadas, a saber, la dirección base y dos encabezados necesarios para trabajar con la API de GitHub.

Cada vez que se llama a `CreateClient`, se crea otra instancia de `HttpClient` y se llama a la acción de configuración.

Para consumir un cliente con nombre, se puede pasar un parámetro de cadena a `CreateClient`. Especifique el nombre del cliente que se va a crear:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

En el código anterior, no es necesario especificar un nombre de host en la solicitud. Basta con pasar solo la ruta de acceso, ya que se usa la dirección base configurada del cliente.

### <a name="typed-clients"></a>Clientes con tipo

Los clientes con tipo proporcionan las mismas funciones que los clientes con nombre sin la necesidad de usar cadenas como claves. El método del cliente con tipo proporciona ayuda de compilador e IntelliSense al consumir clientes. Ofrecen una sola ubicación para configurar un determinado `HttpClient` e interactuar con él. Por ejemplo, el mismo cliente con tipo se puede usar para un punto de conexión back-end único y encapsular toda la lógica que se ocupa de ese punto de conexión. Otra ventaja es que funcionan con la inserción de dependencias, de modo que se pueden insertar cuando sea necesario en la aplicación.

Un cliente con tipo acepta un parámetro `HttpClient` en su constructor:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

En el código anterior, la configuración se mueve al cliente con tipo. El objeto `HttpClient` se expone como una propiedad pública. Se pueden definir métodos específicos de API que exponen la funcionalidad `HttpClient`. El método `GetAspNetDocsIssues` encapsula el código necesario para consultar y analizar los últimos problemas abiertos de un repositorio de GitHub.

Para registrar un cliente con tipo, se puede usar el método de extensión genérico <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> dentro de `Startup.ConfigureServices`, especificando la clase del cliente con tipo:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

El cliente con tipo se registra como transitorio con inserción con dependencias, y se puede insertar y consumir directamente:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

Si lo prefiere, la configuración de un cliente con nombre se puede especificar durante su registro en `Startup.ConfigureServices`, en lugar de en su constructor:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

El `HttpClient` se puede encapsular completamente dentro de un cliente con nombre. En lugar de exponerlo como una propiedad, se pueden proporcionar métodos públicos que llamen a la instancia de `HttpClient` internamente.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

En el código anterior, el `HttpClient` se almacena como un campo privado. Todo el acceso para realizar llamadas externas pasa por el método `GetRepos`.

### <a name="generated-clients"></a>Clientes generados

`IHttpClientFactory` se puede usar en combinación con otras bibliotecas de terceros, como [Refit](https://github.com/paulcbetts/refit). Refit es una biblioteca de REST para .NET que convierte las API de REST en interfaces en vivo. Se genera una implementación de la interfaz dinámicamente por medio de `RestService`, usando `HttpClient` para realizar las llamadas HTTP externas.

Se define una interfaz y una respuesta para representar la API externa y su correspondiente respuesta:

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

Un cliente con tipo se puede agregar usando Refit para generar la implementación:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

La interfaz definida se puede usar cuando sea preciso con la implementación proporcionada por la inserción de dependencias y Refit:

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a>Middleware de solicitud saliente

`HttpClient` ya posee el concepto de controladores de delegación, que se pueden vincular entre sí para las solicitudes HTTP salientes. `IHttpClientFactory` permite definir fácilmente los controladores que se usarán en cada cliente con nombre. Admite el registro y encadenamiento de varios controladores para crear una canalización de middleware de solicitud saliente. Cada uno de estos controladores es capaz de realizar la tarea antes y después de la solicitud de salida. Este patrón es similar a la canalización de middleware de entrada de ASP.NET Core. Dicho patrón proporciona un mecanismo para administrar cuestiones transversales relativas a las solicitudes HTTP, como el almacenamiento en caché, el control de errores, la serialización y el registro.

Para crear un controlador, defina una clase que se derive de <xref:System.Net.Http.DelegatingHandler>. Invalide el método `SendAsync` para ejecutar el código antes de pasar la solicitud al siguiente controlador de la canalización:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

El código anterior define un controlador básico. Comprueba si se ha incluido un encabezado `X-API-KEY` en la solicitud. Si no está presente, puede evitar la llamada HTTP y devolver una respuesta adecuada.

Durante el registro, se pueden agregar uno o varios controladores a la configuración de un `HttpClient`. Esta tarea se realiza a través de métodos de extensión en <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

::: moniker range=">= aspnetcore-2.2"

En el código anterior, `ValidateHeaderHandler` se ha registrado con inserción de dependencias. `IHttpClientFactory` crea un ámbito de inserción de dependencias independiente para cada controlador. Los controladores pueden depender de servicios de cualquier ámbito. Los servicios de los que dependen los controladores se eliminan cuando se elimina el controlador.

Una vez registrado, se puede llamar a <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, pasando el tipo del controlador.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

En el código anterior, `ValidateHeaderHandler` se ha registrado con inserción de dependencias. El controlador **debe** estar registrado en la inserción de dependencias como servicio transitorio, nunca como servicio con ámbito. Si el controlador está registrado como un servicio con ámbito y los servicios de los que depende el controlador son descartables, los servicios del controlador podrían eliminarse antes de que el controlador salga del ámbito, lo que haría que se produjeran errores en el controlador.

Una vez registrado, se puede llamar a <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, con lo que se pasa el tipo de controlador.

::: moniker-end

Se pueden registrar varios controladores en el orden en que deben ejecutarse. Cada controlador contiene el siguiente controlador hasta que el último `HttpClientHandler` ejecuta la solicitud:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

Use uno de los siguientes enfoques para compartir el estado por solicitud con controladores de mensajes:

* Pase datos al controlador usando `HttpRequestMessage.Properties`.
* Use `IHttpContextAccessor` para acceder a la solicitud actual.
* Cree un objeto de almacenamiento `AsyncLocal` personalizado para pasar los datos.

## <a name="use-polly-based-handlers"></a>Usar controladores basados en Polly

`IHttpClientFactory` se integra con una biblioteca de terceros muy conocida denominada [Polly](https://github.com/App-vNext/Polly). Polly es una biblioteca con capacidades de resistencia y control de errores transitorios para .NET. Permite a los desarrolladores expresar directivas como, por ejemplo, de reintento, interruptor, tiempo de espera, aislamiento compartimentado y reserva de forma fluida y segura para los subprocesos.

Se proporcionan métodos de extensión para hacer posible el uso de directivas de Polly con instancias de `HttpClient` configuradas. Encontrará extensiones de Polly disponibles en el paquete NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/). Este paquete no está incluido en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Para usar las extensiones, se debe incluir un `<PackageReference />` explícito en el proyecto.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/HttpClientFactorySample.csproj?highlight=10)]

Tras restaurar este paquete, hay métodos de extensión disponibles para admitir la adición de controladores basados en Polly en clientes.

### <a name="handle-transient-faults"></a>Control de errores transitorios

Los errores más comunes se producen cuando las llamadas HTTP externas son transitorias. Por ello, se incluye un método de extensión muy práctico denominado `AddTransientHttpErrorPolicy`, que permite definir una directiva para controlar los errores transitorios. Las directivas que se configuran con este método de extensión controlan `HttpRequestException`, las respuestas HTTP 5xx y las respuestas HTTP 408.

La extensión `AddTransientHttpErrorPolicy` se puede usar en `Startup.ConfigureServices`. La extensión da acceso a un objeto `PolicyBuilder`, configurado para controlar los errores que pueden constituir un posible error transitorio:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

En el código anterior, se define una directiva `WaitAndRetryAsync`. Las solicitudes erróneas se reintentan hasta tres veces con un retardo de 600 ms entre intentos.

### <a name="dynamically-select-policies"></a>Seleccionar directivas dinámicamente

Existen más métodos de extensión que pueden servir para agregar controladores basados en Polly. Una de esas extensiones es `AddPolicyHandler`, que tiene varias sobrecargas. Una de esas sobrecargas permite inspeccionar la solicitud al dilucidar qué directiva aplicar:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

En el código anterior, si la solicitud de salida es GET, se aplica un tiempo de espera de 10 segundos. En cualquier otro método HTTP, se usa un tiempo de espera de 30 segundos.

### <a name="add-multiple-polly-handlers"></a>Agregar varios controladores de Polly

Es habitual anidar directivas de Polly para proporcionar una mejor funcionalidad:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

En el ejemplo anterior, se agregan dos controladores. En el primer ejemplo se usa la extensión `AddTransientHttpErrorPolicy` para agregar una directiva de reintento. Las solicitudes con error se reintentan hasta tres veces. La segunda llamada a `AddTransientHttpErrorPolicy` agrega una directiva de interruptor. Las solicitudes externas subsiguientes se bloquean durante 30 segundos si se producen cinco intentos infructuosos seguidos. Las directivas de interruptor tienen estado. Así, todas las llamadas realizadas a través de este cliente comparten el mismo estado de circuito.

### <a name="add-policies-from-the-polly-registry"></a>Agregar directivas desde el Registro de Polly

Una forma de administrar las directivas usadas habitualmente consiste en definirlas una vez y registrarlas con `PolicyRegistry`. Se proporciona un método de extensión que permite agregar un controlador por medio de una directiva del Registro:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

En el código anterior, se registran dos directivas cuando se agrega `PolicyRegistry` a `ServiceCollection`. Para usar una directiva del Registro, se usa el método `AddPolicyHandlerFromRegistry` pasando el nombre de la directiva que se va a aplicar.

Encontrará más información sobre `IHttpClientFactory` y las integraciones de Polly en la [wiki de Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).

## <a name="httpclient-and-lifetime-management"></a>HttpClient y administración de la duración

Cada vez que se llama a `CreateClient` en `IHttpClientFactory`, se devuelve una nueva instancia de `HttpClient`. Hay un controlador <xref:System.Net.Http.HttpMessageHandler> por cliente con nombre. La fábrica administra la duración de las instancias de `HttpMessageHandler`.

`IHttpClientFactory` agrupa las instancias de `HttpMessageHandler` creadas por Factory para reducir el consumo de recursos. Se puede reutilizar una instancia de `HttpMessageHandler` del grupo al crear una instancia de `HttpClient` si su duración aún no ha expirado.

Se recomienda agrupar controladores porque cada uno de ellos normalmente administra sus propias conexiones HTTP subyacentes. Crear más controladores de los necesarios puede provocar retrasos en la conexión. Además, algunos controladores dejan las conexiones abiertas de forma indefinida, lo que puede ser un obstáculo a la hora de reaccionar ante los cambios de DNS.

La duración de controlador predeterminada es dos minutos. El valor predeterminado se puede reemplazar individualmente en cada cliente con nombre. Para ello, llame a <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> en el `IHttpClientBuilder` que se devuelve cuando se crea el cliente:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

No hace falta eliminar el cliente, ya que se cancelan las solicitudes salientes y la instancia de `HttpClient` determinada no se puede usar después de llamar a <xref:System.IDisposable.Dispose*>. `IHttpClientFactory` realiza un seguimiento y elimina los recursos que usan las instancias de `HttpClient`. Normalmente, las instancias de `HttpClient` pueden tratarse como objetos de .NET que no requieren eliminación.

Mantener una sola instancia de `HttpClient` activa durante un período prolongado es un patrón común que se utiliza antes de la concepción de `IHttpClientFactory`. Este patrón se convierte en innecesario tras la migración a `IHttpClientFactory`.

## <a name="logging"></a>Registro

Los clientes que se han creado a través de `IHttpClientFactory` registran mensajes de registro de todas las solicitudes. Habilite el nivel de información adecuado en la configuración del registro para ver los mensajes de registro predeterminados. El registro de más información, como el registro de encabezados de solicitud, solo se incluye en el nivel de seguimiento.

La categoría de registro usada en cada cliente incluye el nombre del cliente. Así, por ejemplo, un cliente llamado *MyNamedClient* registra mensajes con una categoría `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`. Los mensajes con el sufijo *LogicalHandler* se producen fuera de la canalización de controlador de la solicitud. En la solicitud, los mensajes se registran antes de que cualquier otro controlador de la canalización haya procesado la solicitud. En la respuesta, los mensajes se registran después de que cualquier otro controlador de la canalización haya recibido la respuesta.

El registro también se produce dentro de la canalización de controlador de la solicitud. En nuestro caso de ejemplo de *MyNamedClient*, esos mensajes se registran en la categoría de registro `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`. En la solicitud, esto tiene lugar después de que todos los demás controladores se hayan ejecutado y justo antes de que la solicitud se envíe por la red. En la respuesta, este registro incluye el estado de la respuesta antes de que vuelva a pasar por la canalización del controlador.

Al habilitar el registro tanto dentro como fuera de la canalización, se podrán inspeccionar los cambios realizados por otros controladores de la canalización. Esto puede englobar cambios, por ejemplo, en los encabezados de solicitud o en el código de estado de la respuesta.

Si el nombre del cliente se incluye en la categoría de registro, dicho registro se podrá filtrar para encontrar clientes con nombre específicos cuando sea necesario.

## <a name="configure-the-httpmessagehandler"></a>Configurar HttpMessageHandler

Puede que sea necesario controlar la configuración del elemento `HttpMessageHandler` interno usado por un cliente.

Se devuelve un `IHttpClientBuilder` cuando se agregan clientes con nombre o con tipo. Se puede usar el método de extensión <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> para definir un delegado. Este delegado servirá para crear y configurar el elemento principal `HttpMessageHandler` que ese cliente usa:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="additional-resources"></a>Recursos adicionales

* [Uso de HttpClientFactory para implementar solicitudes HTTP resistentes](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [Implementación de reintentos de llamada HTTP con retroceso exponencial con HttpClientFactory y las directivas de Polly](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [Implementación del patrón de interruptor](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)