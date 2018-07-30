---
title: Registro en ASP.NET Core
author: ardalis
description: Obtenga información sobre la plataforma de registro de ASP.NET Core. Descubra los proveedores de registro integrados y obtenga más información sobre proveedores de terceros conocidos.
ms.author: tdykstra
ms.date: 07/24/2018
uid: fundamentals/logging/index
ms.openlocfilehash: f629b062afb5c17cd05040a9ef0281aa7121aabc
ms.sourcegitcommit: 516d0645c35ea784a3ae807be087ae70446a46ee
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2018
ms.locfileid: "39320757"
---
# <a name="logging-in-aspnet-core"></a>Registro en ASP.NET Core

Por [Steve Smith](https://ardalis.com/) y [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core es compatible con una API de registro que funciona con una variedad de proveedores de registro. Los proveedores integrados permiten enviar registros a uno o varios destinos, y se puede conectar una plataforma de registro de terceros. En este artículo se muestra cómo usar las API y los proveedores de registro integrados en el código.

::: moniker range=">= aspnetcore-2.0"

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

::: moniker-end

Para obtener información sobre el registro de stdout al hospedar con IIS, consulte <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>. Para obtener información sobre el registro de stdout con Azure App Service, consulte <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.

## <a name="how-to-create-logs"></a>Cómo crear registros

Para crear registros, implemente un objeto [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) desde el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection):

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Después, llame a los métodos de registro de ese objeto de registrador:

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

En este ejemplo se crean registros con la clase `TodoController` como *categoría*. Las categorías se explican [más adelante en este artículo](#log-category).

ASP.NET Core no proporciona métodos de registrador asincrónicos porque el registro debe ser tan rápido que el costo de usarlos no vale la pena. Si se encuentra en una situación en la que no sea así, considere la posibilidad de cambiar el modo de registro. Si el almacén de datos es lento, escriba primero los mensajes de registro en un almacén rápido y, después, muévalos a un almacén de baja velocidad. Por ejemplo, realice el registro en una cola de mensajes que otro proceso lea y conserve en almacenamiento lento.

## <a name="how-to-add-providers"></a>Cómo agregar proveedores

::: moniker range=">= aspnetcore-2.0"

Un proveedor de registro toma los mensajes que se crean con un objeto `ILogger` y los muestra o almacena. Por ejemplo, el proveedor de la consola muestra mensajes en la consola y el proveedor de Azure App Service puede almacenarlos en Azure Blob Storage.

Para usar un proveedor, llame al método de extensión `Add<ProviderName>` del proveedor en *Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

La plantilla de proyecto predeterminada habilita los proveedores de registro de la consola y de depuración con una llamada al método de extensión [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) en *Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Un proveedor de registro toma los mensajes que se crean con un objeto `ILogger` y los muestra o almacena. Por ejemplo, el proveedor de la consola muestra mensajes en la consola y el proveedor de Azure App Service puede almacenarlos en Azure Blob Storage.

Para usar un proveedor, instale su paquete NuGet y llame al método de extensión del proveedor en una instancia de [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), como se muestra en el ejemplo siguiente:

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

La [inserción de dependencias](xref:fundamentals/dependency-injection) (DI) de ASP.NET Core proporciona la instancia de `ILoggerFactory`. Los métodos de extensión `AddConsole` y `AddDebug` se definen en los paquetes [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) y [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/). Cada método de extensión llama al método `ILoggerFactory.AddProvider`, pasando una instancia del proveedor.

> [!NOTE]
> La [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) agrega proveedores de registro en el método `Startup.Configure`. Si quiere obtener la salida de registro de código que se ejecuta antes, agregue los proveedores de registro en el constructor de la clase `Startup`.

::: moniker-end

Obtenga más información sobre los [proveedores de registro integrados](#built-in-logging-providers) y encuentre [proveedores de registro de terceros](#third-party-logging-providers) más adelante en el artículo.

## <a name="configuration"></a>Configuración

Uno o varios proveedores de configuración proporcionan la configuración del proveedor de registro:

* Formatos de archivo (INI, JSON y XML).
* Argumentos de la línea de comandos.
* Variables de entorno.
* Objetos de .NET en memoria.
* El almacenamiento de [administrador secreto](xref:security/app-secrets) sin cifrar.
* Un almacén de usuario cifrado, como [Azure Key Vault](xref:security/key-vault-configuration).
* Proveedores personalizados (instalados o creados).

Por ejemplo, la sección `Logging` de archivos de configuración de aplicación suele proporcionar la configuración de registro. En el ejemplo siguiente se muestra el contenido de un archivo *appsettings.Development.json* típico:

::: moniker range=">= aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    },
    "Console":
    {
      "IncludeScopes": "true"
    }
  }
}
```

Las claves `LogLevel` representan los nombres de registro. La clave `Default` se aplica a los registros que no se enumeran de forma explícita. El valor representa el [nivel de registro](#log-level) aplicado al registro determinado. Las claves de registro que establecen `IncludeScopes` (`Console` en el ejemplo), especifican si los [ámbitos de registro](#log-scopes) están habilitados para el registro indicado.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    }
  }
}
```

Las claves `LogLevel` representan los nombres de registro. La clave `Default` se aplica a los registros que no se enumeran de forma explícita. El valor representa el [nivel de registro](#log-level) aplicado al registro determinado.

::: moniker-end

Para obtener información sobre cómo implementar proveedores de configuración, consulte <xref:fundamentals/configuration/index>.

## <a name="sample-logging-output"></a>Salida de registro de ejemplo

Con el código de ejemplo que se muestra en la sección anterior, verá los registros en la consola cuando se ejecute desde la línea de comandos. Este es un ejemplo de salida de la consola:

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```

Estos registros se crearon yendo a `http://localhost:5000/api/todo/0`, lo que desencadena la ejecución de las dos llamadas a `ILogger` que se muestran en la sección anterior.

Este es un ejemplo de los mismos registros tal y como aparecen en la ventana de depuración cuando se ejecuta la aplicación de ejemplo en Visual Studio:

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

Los registros creados por las llamadas a `ILogger` que se muestran en la sección anterior comienzan por "TodoApi.Controllers.TodoController". Los registros que comienzan por categorías de "Microsoft" son de ASP.NET Core. El propio ASP.NET Core y el código de la aplicación usan la misma API y los mismos proveedores de registro.

En el resto de este artículo se explican algunos detalles y opciones para el registro.

## <a name="nuget-packages"></a>Paquetes NuGet

Las interfaces `ILogger` e `ILoggerFactory` se encuentran en [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), y sus implementaciones predeterminadas en [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).

## <a name="log-category"></a>Categoría de registro

Con cada registro que cree se incluye una *categoría*. La categoría se especifica cuando se crea un objeto `ILogger`. La categoría puede ser cualquier cadena, pero una convención consiste en usar el nombre completo de la clase desde la que se escriben los registros. Por ejemplo: "TodoApi.Controllers.TodoController".

Puede especificar la categoría como una cadena o usar un método de extensión que derive la categoría del tipo. Para especificar la categoría como una cadena, llame a `CreateLogger` en una instancia de `ILoggerFactory`, como se muestra a continuación.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

En la mayoría de los casos, será más fácil usar `ILogger<T>`, como en el ejemplo siguiente.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Esto equivale a llamar a `CreateLogger` con el nombre de tipo completo de `T`.

## <a name="log-level"></a>Nivel de registro

Cada vez que se escribe un registro, se especifica su [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel). El nivel de registro indica el grado de gravedad o importancia. Por ejemplo, es posible que escriba un registro `Information` cuando un método finaliza normalmente, un registro `Warning` cuando un método devuelve un código de retorno 404 y un registro `Error` cuando capture una excepción inesperada.

En el ejemplo de código siguiente, los nombres de los métodos (por ejemplo, `LogWarning`) especifican el nivel de registro. El primer parámetro es el [Id. del evento del Registro](#log-event-id). El segundo parámetro es una [plantilla de mensaje](#log-message-template) con marcadores de posición para los valores de argumento proporcionados por el resto de parámetros de método. Los parámetros de método se explican detalladamente más adelante en este artículo.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Los métodos de registro que incluyen el nivel en el nombre del método son [métodos de extensión para ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions). En segundo plano, estos métodos llaman a un método `Log` que toma un parámetro `LogLevel`. Puede llamar directamente al método `Log` en lugar de a uno de estos métodos de extensión, pero la sintaxis es relativamente complicada. Para más información, vea la [interfaz ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) y el [código fuente de las extensiones de registrador](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).

ASP.NET Core define los [niveles de registro](/dotnet/api/microsoft.extensions.logging.loglevel) siguientes, que aquí se ordenan de menor a mayor gravedad.

* Seguimiento = 0

  Para la información que solo es útil para un desarrollador que depura un problema. Estos mensajes pueden contener datos confidenciales de la aplicación, por lo que no deben habilitarse en un entorno de producción. *Deshabilitado de forma predeterminada.* Ejemplo: `Credentials: {"User":"someuser", "Password":"P@ssword"}`

* Depurar = 1

  Para la información que tiene utilidad a corto plazo durante el desarrollo y la depuración. Ejemplo: `Entering method Configure with flag set to true.` normalmente los registros de nivel `Debug` no se habilitarían en producción, a menos que se esté solucionando un problema, debido al elevado volumen de registros.

* Información = 2

  Para realizar el seguimiento del flujo general de la aplicación. Estos registros suelen tener algún valor a largo plazo. Ejemplo: `Request received for path /api/todo`

* Advertencia = 3

  Para los eventos anómalos o inesperados en el flujo de la aplicación. Estos pueden incluir errores u otras condiciones que no hacen que la aplicación se detenga, pero que puede que sea necesario investigar. Las excepciones controladas son un lugar común para usar el nivel de registro `Warning`. Ejemplo: `FileNotFoundException for file quotes.txt.`

* Error = 4

  Para los errores y excepciones que no se pueden controlar. Estos mensajes indican un error en la actividad u operación actual (por ejemplo, la solicitud HTTP actual), no un error de toda la aplicación. Mensaje de registro de ejemplo: `Cannot insert record due to duplicate key violation.`

* Crítico = 5

  Para los errores que requieren atención inmediata. Ejemplos: escenarios de pérdida de datos, espacio en disco insuficiente.

Puede usar el nivel de registro para controlar la cantidad de salida del registro que se escribe en un medio de almacenamiento determinado o ventana de presentación. Por ejemplo, en producción es posible que quiera que todos los registros de nivel `Information` e inferiores vayan a un almacén de datos de volumen y que todos los registros de nivel `Warning` y superiores vayan un almacén de datos de valor. Durante el desarrollo, es posible que normalmente envíe los registros de gravedad `Warning` o superior a la consola. Después, si tiene que solucionar problemas, puede agregar el nivel `Debug`. En la sección [Filtrado del registro](#log-filtering) de este artículo se explica cómo controlar los niveles de registro que controla un proveedor.

La plataforma ASP.NET Core escribe registros de nivel `Debug` para los eventos de la plataforma. En los ejemplos de registro anteriores de este artículo se excluyeron los registros por debajo del nivel `Information`, por lo que no se mostraron los registros de nivel `Debug`. Este es un ejemplo de registros de consola si ejecuta la aplicación de ejemplo configurada para mostrar `Debug` y registros superiores para el proveedor de la consola.

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a>Id. de evento del registro

Cada vez que se escribe un registro, puede especificar un *Id. de evento*. La aplicación de ejemplo lo hace mediante una clase `LoggingEvents` definida de forma local:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

Un identificador de evento es un valor entero que se puede usar para asociar un conjunto de eventos registrados entre sí. Por ejemplo, un registro para agregar un elemento a un carro de la compra podría tener el identificador de evento 1000 y un registro para completar una compra podría tener el identificador de evento 1001.

En la salida del registro, el identificador de evento podría estar almacenado en un campo o incluido en el mensaje de texto, en función del proveedor. El proveedor de depuración no muestra los identificadores de evento, pero el proveedor de la consola los muestra entre paréntesis después de la categoría:

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>Plantilla de mensaje de registro

Cada vez que se escribe un mensaje de registro, se proporciona una plantilla de mensaje. La plantilla de mensaje puede ser una cadena o puede contener marcadores de posición con nombre en los que se colocan los valores de argumento. La plantilla no es una cadena de formato y los marcadores de posición deben tener un nombre, no un número.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

El orden de los marcadores de posición, no sus nombres, determina qué parámetros se usan para proporcionar sus valores. Si tiene el siguiente código:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

El mensaje del registro resultante tiene el aspecto siguiente:

```
Parameter values: parm1, parm2
```

La plataforma de registro aplica este formato de mensajes para que los proveedores de registro puedan implementar el [registro semántico, también conocido como registro estructurado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging). Dado que los propios argumentos se pasan al sistema de registro, no solo la plantilla de mensaje con formato, los proveedores de registro pueden almacenar los valores de parámetros como campos además de la plantilla de mensaje. Si va a dirigir la salida del registro a Azure Table Storage y la llamada al método de registrador tiene el aspecto siguiente:

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Cada entidad de la Tabla de Azure puede tener propiedades `ID` y `RequestTime`, lo que simplifica las consultas en los datos de registro. Puede buscar todos los registros dentro de un intervalo `RequestTime` determinado sin necesidad de analizar el tiempo de espera del mensaje de texto.

## <a name="logging-exceptions"></a>Excepciones de registro

Los métodos de registrador tienen sobrecargas que le permiten pasar una excepción, como en el ejemplo siguiente:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

Cada proveedor controla la información de la excepción de maneras diferentes. Este es un ejemplo de salida del proveedor de depuración del código mostrado anteriormente.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Filtrado del registro

::: moniker range=">= aspnetcore-2.0"

Puede especificar un nivel de registro mínimo para un proveedor y una categoría específicos, o para todos los proveedores o todas las categorías. Los registros por debajo del nivel mínimo no se pasan a ese proveedor, de modo que no se muestran o almacenan.

Si quiere suprimir todos los registros, puede especificar `LogLevel.None` como el nivel de registro mínimo. El valor entero de `LogLevel.None` es 6, que es superior a `LogLevel.Critical` (5).

### <a name="create-filter-rules-in-configuration"></a>Creación de reglas de filtro en la configuración

Las plantillas de proyecto crean código que llama a `CreateDefaultBuilder` para configurar el registro para los proveedores de consola y de depuración. El método `CreateDefaultBuilder` también establece el registro para buscar la configuración en una sección `Logging`, mediante código similar al siguiente:

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

Los datos de configuración especifican niveles de registro mínimo por proveedor y categoría, como en el ejemplo siguiente:

[!code-json[](index/sample2/appsettings.json)]

Este archivo JSON crea seis reglas de filtro, una para el proveedor de depuración, cuatro para el proveedor de la consola y una que se aplica a todos los proveedores. Más adelante verá que solo una de estas reglas se elige para cada proveedor cuando se crea un objeto `ILogger`.

### <a name="filter-rules-in-code"></a>Reglas de filtro en el código

Puede registrar las reglas de filtro en el código, como se muestra en el ejemplo siguiente:

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

El segundo `AddFilter` especifica el proveedor de depuración mediante su nombre de tipo. El primer `AddFilter` se aplica a todos los proveedores, dado que no especifica un tipo de proveedor.

### <a name="how-filtering-rules-are-applied"></a>Cómo se aplican las reglas de filtro

Los datos de configuración y el código de `AddFilter` que se muestran en los ejemplos anteriores crean las reglas que se muestran en la tabla siguiente. Las seis primeras proceden del ejemplo de configuración y las dos últimas del ejemplo de código.

| número | Proveedor      | Categorías que comienzan por...          | Nivel de registro mínimo |
| :----: | ------------- | --------------------------------------- | ----------------- |
| 1      | Depuración         | Todas las categorías                          | Información       |
| 2      | Consola       | Microsoft.AspNetCore.Mvc.Razor.Internal | Advertencia           |
| 3      | Consola       | Microsoft.AspNetCore.Mvc.Razor.Razor    | Depuración             |
| 4      | Consola       | Microsoft.AspNetCore.Mvc.Razor          | Error             |
| 5      | Consola       | Todas las categorías                          | Información       |
| 6      | Todos los proveedores | Todas las categorías                          | Depuración             |
| 7      | Todos los proveedores | Sistema                                  | Depuración             |
| 8      | Depuración         | Microsoft                               | Seguimiento             |

Cuando se crea un objeto `ILogger` con el que escribir los registros, el objeto `ILoggerFactory` selecciona una sola regla por proveedor para aplicar a ese registrador. Todos los mensajes escritos por ese objeto `ILogger` se filtran según las reglas seleccionadas. De las reglas disponibles se selecciona la más específica posible para cada par de categoría y proveedor.

Cuando se crea un `ILogger` para una categoría determinada, se usa el algoritmo siguiente para cada proveedor:

* Se seleccionan todas las reglas que coinciden con el proveedor o su alias. Si no se encuentra ninguna, se seleccionan todas las reglas con un proveedor vacío.
* Del resultado del paso anterior, se seleccionan las reglas con el prefijo de categoría coincidente más largo. Si no se encuentra ninguna, se seleccionan todas las reglas que no especifican una categoría.
* Si se seleccionan varias reglas, se toma la **última**.
* Si no se selecciona ninguna regla, se usa `MinimumLevel`.

Por ejemplo, supongamos que tiene la lista de reglas anterior y crea un objeto `ILogger` para la categoría "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":

* Para el proveedor de depuración, se aplican las reglas 1, 6 y 8. La regla 8 es la más específica, por lo que se selecciona.
* Para el proveedor de la consola, se aplican las reglas 3, 4, 5 y 6. La regla 3 es la más específica.

Cuando crea registros con un `ILogger` para la categoría "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", los registros de nivel `Trace` y superiores se dirigirán al proveedor de depuración y los registros de nivel `Debug` y superiores se dirigirán al proveedor de consola.

### <a name="provider-aliases"></a>Alias de proveedor

Puede usar el nombre de tipo para especificar un proveedor en la configuración, pero cada proveedor define un *alias* más breve que es más fácil de usar. Para los proveedores integrados, use los alias siguientes:

* Consola
* Depuración
* EventLog
* AzureAppServices
* TraceSource
* EventSource

### <a name="default-minimum-level"></a>Nivel mínimo predeterminado

Hay una configuración de nivel mínimo que solo tiene efecto si no se aplica ninguna regla de configuración o código para un proveedor y una categoría determinados. En el ejemplo siguiente se muestra cómo establecer el nivel mínimo:

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

Si no establece explícitamente el nivel mínimo, el valor predeterminado es `Information`, lo que significa que los registros `Trace` y `Debug` se omiten.

### <a name="filter-functions"></a>Funciones de filtro

Puede escribir código en una función de filtro para aplicar las reglas de filtrado. Se invoca una función de filtro para todos los proveedores y categorías que no tienen reglas asignadas mediante configuración o código. El código de la función tiene acceso al tipo de proveedor, categoría y nivel de registro para decidir si se debe registrar un mensaje. Por ejemplo:

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Algunos proveedores de registro permiten especificar cuándo deben escribirse los registros en un medio de almacenamiento o ignorarse en función de la categoría y el nivel de registro.

Los métodos de extensión `AddConsole` y `AddDebug` proporcionan sobrecargas que le permiten pasar criterios de filtrado. El código de ejemplo siguiente hace que el proveedor de la consola ignore los registros por debajo del nivel `Warning`, mientras que el proveedor de depuración omite los registros creados por la plataforma.

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

El método `AddEventLog` tiene una sobrecarga que toma una instancia de `EventLogSettings`, que puede contener una función de filtrado en su propiedad `Filter`. El proveedor de TraceSource no proporciona ninguna de estas sobrecargas, dado que su nivel de registro y otros parámetros se basan en el `SourceSwitch` y `TraceListener` que usa.

Puede establecer reglas de filtrado para todos los proveedores que están registrados con un instancia de `ILoggerFactory` mediante el método de extensión `WithFilter`. En el ejemplo siguiente se limitan los registros de la plataforma (la categoría comienza con "Microsoft" o "System") a las advertencias, mientras se permite el registro de la aplicación en el nivel de depuración.

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Si quiere usar el filtrado para impedir que se escriban todos los registros para una categoría concreta, puede especificar `LogLevel.None` como el nivel de registro mínimo de esa categoría. El valor entero de `LogLevel.None` es 6, que es superior a `LogLevel.Critical` (5).

El paquete NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) proporciona el método de extensión `WithFilter`. El método devuelve una instancia nueva de `ILoggerFactory` que filtrará los mensajes de registro pasados a todos los proveedores de registrador registrados con ella. No afecta a ninguna otra instancia de `ILoggerFactory`, incluida la instancia de `ILoggerFactory` original.

::: moniker-end

## <a name="log-scopes"></a>Ámbitos de registro

Puede agrupar un conjunto de operaciones lógicas dentro de un *ámbito* para adjuntar los mismos datos para cada registro que se crea como parte de ese conjunto. Por ejemplo, es posible que quiera que todos los registros creados como parte del procesamiento de una transacción incluyan el identificador de la transacción.

Un ámbito es un tipo `IDisposable` devuelto por el método [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) que se conserva hasta que se elimina. Para usar un ámbito, las llamadas de registrador se encapsulan en un bloque `using`, como se muestra aquí:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

El código siguiente permite ámbitos para el proveedor de la consola:

::: moniker range="> aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Es necesario configurar la opción del registrador de consola `IncludeScopes` para habilitar el registro basado en el ámbito.
>
> Para obtener información sobre la configuración, consulte la sección [Configuración](#Configuration).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Es necesario configurar la opción del registrador de consola `IncludeScopes` para habilitar el registro basado en el ámbito.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

*Startup.cs*:

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

Cada mensaje de registro incluye la información de ámbito:

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>Proveedores de registro integrados

ASP.NET Core incluye los proveedores siguientes:

* [Consola](#console-provider)
* [Depurar](#debug-provider)
* [EventSource](#eventsource-provider)
* [EventLog](#windows-eventlog-provider)
* [TraceSource](#tracesource-provider)
* [Azure App Service](#azure-app-service-provider)

### <a name="console-provider"></a>Proveedor de la consola

El paquete de proveedor [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) envía la salida del registro a la consola.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

[Las sobrecargas de AddConsole](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) permiten pasar un nivel de registro mínimo, una función de filtro y un valor booleano que indica si se admiten los ámbitos. Otra opción consiste en pasar un objeto `IConfiguration`, que puede especificar niveles de registro y si se admiten los ámbitos.

Si está pensando en la posibilidad de usar la consola de proveedor en producción, tenga en cuenta que tiene un impacto significativo en el rendimiento.

Cuando se crea un proyecto en Visual Studio, el método `AddConsole` tiene el aspecto siguiente:

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Este código hace referencia a la sección `Logging` del archivo *appSettings.json*:

[!code-json[](index/sample//appsettings.json)]

La configuración que se muestra limita los registros de la plataforma a las advertencias mientras que permite a la aplicación registrar en el nivel de depuración, como se explica en la sección [Filtrado del registro](#log-filtering). Para obtener más información, vea [Configuración](xref:fundamentals/configuration/index).

::: moniker-end

### <a name="debug-provider"></a>Proveedor de depuración

El paquete de proveedor [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) escribe la salida del registro mediante la clase [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (llamadas a métodos `Debug.WriteLine`).

En Linux, este proveedor escribe registros en */var/log/message*.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

[Las sobrecargas de AddDebug](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) permiten pasar un nivel mínimo de registro o una función de filtro.

::: moniker-end

### <a name="eventsource-provider"></a>Proveedor EventSource

Para las aplicaciones que tengan como destino ASP.NET Core 1.1.0 o una versión posterior, el paquete de proveedor [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) puede implementar el seguimiento de eventos. En Windows, usa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803). Es un proveedor multiplataforma, pero todavía no hay herramientas de recopilación y visualización de eventos para Linux o macOS.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger();
```

::: moniker-end

Una buena manera de recopilar y ver los registros es usar la [utilidad PerfView](https://github.com/Microsoft/perfview). Hay otras herramientas para ver los registros ETW, pero PerfView proporciona la mejor experiencia para trabajar con los eventos ETW emitidos por ASP.NET.

Para configurar PerfView para la recopilación de eventos registrados por este proveedor, agregue la cadena `*Microsoft-Extensions-Logging` a la lista **Proveedores adicionales**. (No olvide el asterisco al principio de la cadena).

![Proveedores adicionales de Perfview](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a>Proveedor EventLog de Windows

El paquete de proveedor [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) envía la salida del registro al Registro de eventos de Windows.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

[Las sobrecargas de AddEventLog](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) permiten pasar `EventLogSettings` o un nivel de registro mínimo.

::: moniker-end

### <a name="tracesource-provider"></a>Proveedor TraceSource

El paquete de proveedor [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) usa las bibliotecas y proveedores de [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource).

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddTraceSource(sourceSwitchName);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

::: moniker-end

[Las sobrecargas de AddTraceSource](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) permiten pasar un modificador de origen y un agente de escucha de seguimiento.

Para usar este proveedor, una aplicación debe ejecutarse en .NET Framework (en lugar de .NET Core). El proveedor permite enrutar mensajes a una variedad de [agentes de escucha](/dotnet/framework/debug-trace-profile/trace-listeners), como [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) que se usa en la aplicación de ejemplo.

En el ejemplo siguiente se configura un proveedor `TraceSource` que registra mensajes `Warning` y superiores en la ventana de consola.

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

### <a name="azure-app-service-provider"></a>Proveedor Azure App Service

El paquete de proveedor [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) escribe los registros en archivos de texto en el sistema de archivos de una aplicación de Azure App Service y en [Blob Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) en una cuenta de Azure Storage. El paquete de proveedor está disponible para aplicaciones cuyo destino sea .NET Core 1.1 o una versión posterior.

::: moniker range=">= aspnetcore-2.0"

Si el destino es .NET Core, tenga en cuenta lo siguiente:

* El paquete de proveedor está incluido en el [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage) de ASP.NET Core, pero no en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).
* No realice llamadas explícitas a [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics). El proveedor está disponible automáticamente para la aplicación cuando esta se implementa en Azure App Service.

Si el destino es .NET Framework o hace referencia al metapaquete `Microsoft.AspNetCore.App`, agregue el paquete de proveedor al proyecto. Invoque `AddAzureWebAppDiagnostics` en una instancia [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory):

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

Una sobrecarga de [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) permite pasar [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings), con lo que se puede invalidar la configuración predeterminada, como la plantilla de salida del registro, el nombre de blob y el límite de tamaño de archivo. (La *plantilla de salida* es una plantilla de mensaje que se aplica a todos los registros además de la que se proporciona cuando se llama a un método `ILogger`).

Cuando se realiza una implementación en una aplicación de App Service, la aplicación respeta la configuración de la sección [Registros de diagnóstico](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) situada en la página **App Service** de Azure Portal. Cuando se actualiza esta configuración, los cambios se aplican inmediatamente sin necesidad de reiniciar ni de volver a implementar la aplicación.

![Configuración de los registros de Azure](index/_static/azure-logging-settings.png)

La ubicación predeterminada de los archivos de registro es la carpeta *D:\\home\\LogFiles\\Application* y el nombre de archivo predeterminado es *diagnostics-aaaammdd.txt*. El límite de tamaño de archivo predeterminado es 10 MB, y el número máximo predeterminado de archivos que se conservan es 2. El nombre de blob predeterminado es *{nombre-de-la-aplicación}{marca de tiempo}/aaaa/mm/dd/hh/{guid}-applicationLog.txt*. Para más información sobre el comportamiento predeterminado, vea [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).

El proveedor solo funciona cuando el proyecto se ejecuta en el entorno de Azure. No tiene ningún efecto cuando el proyecto se ejecuta de manera local (no escribe en los archivos locales ni en el almacenamiento de desarrollo local de blobs).

## <a name="third-party-logging-providers"></a>Proveedores de registro de terceros

Plataformas de registro de terceros que funcionan con ASP.NET Core:

* [elmah.io](https://elmah.io/) ([repositorio de GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))
* [Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([repositorio de GitHub](https://github.com/mattwcole/gelf-extensions-logging))
* [JSNLog](http://jsnlog.com/) ([repositorio de GitHub](https://github.com/mperdeck/jsnlog))
* [Loggr](http://loggr.net/) ([repositorio de GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))
* [NLog](http://nlog-project.org/) ([repositorio de GitHub](https://github.com/NLog/NLog.Extensions.Logging))
* [Serilog](https://serilog.net/) ([repositorio de GitHub](https://github.com/serilog/serilog-extensions-logging))

Algunas plataformas de terceros pueden realizar [registro semántico, también conocido como registro estructurado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

El uso de una plataforma de terceros es similar al uso de uno de los proveedores integrados:

1. Agregue un paquete NuGet al proyecto.
1. Llame a un método de extensión en `ILoggerFactory`.

Para más información, vea la documentación de cada plataforma.

## <a name="azure-log-streaming"></a>Secuencias de registro de Azure

Las secuencias de registro de Azure permiten ver la actividad de registro en tiempo real desde:

* El servidor de aplicaciones
* El servidor web
* Error del seguimiento de solicitudes

Para configurar las secuencias de registro de Azure:

* Navegue hasta la página **Registros de diagnóstico** desde la página de portal de la aplicación
* Establezca **Registro de la aplicación (sistema de archivos)** en Activado.

![Página de registros de diagnóstico de Azure Portal](index/_static/azure-diagnostic-logs.png)

Navegue hasta la página **Secuencias de registro** para ver los mensajes de la aplicación. Se registran por la aplicación a través de la interfaz `ILogger`.

![Secuencias de registro de aplicación de Azure Portal](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a>Registro de seguimiento de Azure Application Insights

El SDK de [Application Insights](https://azure.microsoft.com/services/application-insights/) es capaz de recopilar información de telemetría de seguimiento de registros generados mediante la infraestructura de registro de ASP.NET Core. Para obtener más información, vea la [wiki de Microsoft/ApplicationInsights-aspnetcore sobre el registro](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/logging/loggermessage>
