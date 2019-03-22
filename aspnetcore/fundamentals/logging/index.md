---
title: Registro en ASP.NET Core
author: tdykstra
description: Obtenga información sobre la plataforma de registro de ASP.NET Core. Descubra los proveedores de registro integrados y obtenga más información sobre proveedores de terceros conocidos.
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/02/2019
uid: fundamentals/logging/index
---
# <a name="logging-in-aspnet-core"></a>Registro en ASP.NET Core

Por [Steve Smith](https://ardalis.com/) y [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core es compatible con una API de registro que funciona con una gran variedad de proveedores de registro integrados y de terceros. En este artículo se muestra cómo usar las API de registro con proveedores integrados.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="add-providers"></a>Incorporación de proveedores

Un proveedor de registro muestra o almacena registros. Por ejemplo, el proveedor de consola muestra los registros en la consola y el proveedor de Azure Application Insights los almacena en Azure Application Insights. Los registros se pueden enviar a varios destinos mediante la incorporación de varios proveedores.

::: moniker range=">= aspnetcore-2.0"

Para usar un proveedor, llame al método de extensión `Add{provider name}` del proveedor en *Program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=17-19)]

La plantilla de proyecto predeterminada llama a <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, que agrega los siguientes proveedores de registro:

* Consola
* Depuración
* EventSource (a partir de ASP.NET Core 2.2)

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

Si usa `CreateDefaultBuilder`, puede reemplazar los proveedores predeterminados por sus propios valores. Llame a <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> y agregue los proveedores que desee.

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Para usar un proveedor, instale su paquete NuGet y llame al método de extensión del proveedor en una instancia de <xref:Microsoft.Extensions.Logging.ILoggerFactory>:

[!code-csharp[](index/samples/1.x/TodoApiSample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

La [inserción de dependencias (DI)](xref:fundamentals/dependency-injection) de ASP.NET Core proporciona la instancia de `ILoggerFactory`. Los métodos de extensión `AddConsole` y `AddDebug` se definen en los paquetes [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) y [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/). Cada método de extensión llama al método `ILoggerFactory.AddProvider`, pasando una instancia del proveedor.

> [!NOTE]
> La [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) agrega proveedores de registro en el método `Startup.Configure`. Para obtener la salida de registro de código que se ejecuta antes, agregue los proveedores de registro en el constructor de la clase `Startup`.

::: moniker-end

Obtenga más información sobre los [proveedores de registro integrados](#built-in-logging-providers) y los [proveedores de registro de terceros](#third-party-logging-providers) más adelante en el artículo.

## <a name="create-logs"></a>Creación de registros

Obtenga un objeto <xref:Microsoft.Extensions.Logging.ILogger%601> a partir de la inserción de dependencias.

::: moniker range=">= aspnetcore-2.0"

El siguiente ejemplo de controlador crea los registros `Information` y `Warning`. La *categoría* es `TodoApiSample.Controllers.TodoController` (el nombre de clase completo de `TodoController` en la aplicación de ejemplo):

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=4,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

El siguiente ejemplo de Razor Pages crea registros con `Information` como el *nivel* y `TodoApiSample.Pages.AboutModel` como la *categoría*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3, 7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

El ejemplo anterior crea registros con `Information` y `Warning` como el *nivel* y la clase `TodoController` como la *categoría*. 

::: moniker-end

El *nivel* de registro indica la gravedad del evento registrado. La *categoría* de registro es una cadena que está asociada con cada registro. La `ILogger<T>` instancia crea registros que tienen el nombre completo del tipo `T` como la categoría. Los [niveles](#log-level) y las [categorías](#log-category) se explican detalladamente más adelante en este artículo. 

::: moniker range=">= aspnetcore-2.0"

### <a name="create-logs-in-startup"></a>Creación de registros durante el inicio

Para escribir registros en la clase `Startup`, incluya un parámetro `ILogger` en la signatura de construcción:

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-program"></a>Creación de registros en la clase Program

Para escribir registros la clase `Program`, obtenga una instancia `ILogger` de inserción de dependencias:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a>No hay métodos de registrador asincrónicos

El registro debe ser tan rápido que no merezca la pena el costo de rendimiento del código asincrónico. Si el almacén de datos de registro es lento, no escriba directamente en él. Considere la posibilidad de escribir los mensajes de registro en un almacén rápido inicialmente y luego moverlos a la tienda lenta. Por ejemplo, realice el registro en una cola de mensajes que otro proceso lea y conserve en almacenamiento lento.

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
      "IncludeScopes": true
    }
  }
}
```

La propiedad `Logging` puede tener `LogLevel` y propiedades del proveedor de registro (se muestra la consola).

La propiedad `LogLevel` bajo `Logging` especifica el [nivel](#log-level) mínimo que se va a registrar para las categorías seleccionadas. En el ejemplo, las categorías `System` y `Microsoft` se registran en el nivel `Information` y todas las demás se registran en el nivel `Debug`.

Otras propiedades bajo `Logging` especifican proveedores de registro. El ejemplo es para el proveedor de consola. Si un proveedor admite [ámbitos de registro](#log-scopes), `IncludeScopes` indica si están habilitados. Una propiedad de proveedor (como `Console` en el ejemplo) también puede especificar una propiedad `LogLevel`. `LogLevel` en un proveedor especifica niveles de registro para ese proveedor.

Si los niveles se especifican en `Logging.{providername}.LogLevel`, invalidan todo lo establecido en `Logging.LogLevel`.

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

Con el código de ejemplo que se muestra en la sección anterior, los registros aparecen en la consola cuando la aplicación se ejecuta desde la línea de comandos. Este es un ejemplo de salida de la consola:

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

Los registros anteriores se generaron mediante la realización de una solicitud HTTP GET a la aplicación de ejemplo en `http://localhost:5000/api/todo/0`.

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

Los registros creados por las llamadas a `ILogger` que se muestran en la sección anterior comienzan por "TodoApi.Controllers.TodoController". Los registros que comienzan por categorías de "Microsoft" son del código de marco de ASP.NET Core. ASP.NET Core y el código de la aplicación usan la misma API y los mismos proveedores de registro.

En el resto de este artículo se explican algunos detalles y opciones para el registro.

## <a name="nuget-packages"></a>Paquetes NuGet

Las interfaces `ILogger` e `ILoggerFactory` se encuentran en [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), y sus implementaciones predeterminadas en [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).

## <a name="log-category"></a>Categoría de registro

Cuando se crea un objeto `ILogger`, se ha especificado una *categoría* para él. Esa categoría se incluye con cada mensaje de registro creado por esa instancia de `ILogger`. La categoría puede ser cualquier cadena, pero la convención es usar el nombre de clase, como "TodoApi.Controllers.TodoController".

Use `ILogger<T>` para obtener una instancia `ILogger` que utiliza el nombre de tipo completo de `T` como la categoría:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

Para especificar explícitamente la categoría, llame a `ILoggerFactory.CreateLogger`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

`ILogger<T>` es equivale a llamar a `CreateLogger` con el nombre de tipo completo de `T`.

## <a name="log-level"></a>Nivel de registro

Cara registro especifica un valor <xref:Microsoft.Extensions.Logging.LogLevel>. El nivel de registro indica la gravedad o importancia. Por ejemplo, podría escribir un registro `Information` cuando un método termina con normalidad y un registro `Warning` cuando un método devuelve un código de estado *404 No encontrado*.

El siguiente código crea los registros `Information` y `Warning`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

En el código anterior, el primer parámetro es el [id. de evento del registro](#log-event-id). El segundo parámetro es una plantilla de mensaje con marcadores de posición para los valores de argumento proporcionados por el resto de parámetros de método. Los parámetros de método se explican detalladamente en la [sección de la plantilla de mensaje](#log-message-template) más adelante en este artículo.

Los métodos de registro que incluyen el nivel en el nombre del método (por ejemplo `LogInformation` y `LogWarning`) son [métodos de extensión para ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions). Estos métodos llaman a un método `Log` que toma un parámetro `LogLevel`. Puede llamar directamente al método `Log` en lugar de a uno de estos métodos de extensión, pero la sintaxis es relativamente complicada. Para más información, vea la <xref:Microsoft.Extensions.Logging.ILogger> y el [código fuente de las extensiones de registrador](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).

ASP.NET Core define los niveles de registro siguientes, que aquí se ordenan de menor a mayor gravedad.

* Seguimiento = 0

  Para información que normalmente solo es útil para la depuración. Estos mensajes pueden contener datos confidenciales de la aplicación, por lo que no deben habilitarse en un entorno de producción. *Deshabilitado de forma predeterminada.*

* Depurar = 1

  Para información que puede ser útil para el desarrollo y la depuración. Ejemplo: `Entering method Configure with flag set to true.` Habilite los registros de nivel `Debug` en producción cuando esté solucionando un problema, debido al elevado volumen de registros.

* Información = 2

  Para realizar el seguimiento del flujo general de la aplicación. Estos registros suelen tener algún valor a largo plazo. Ejemplo: `Request received for path /api/todo`

* Advertencia = 3

  Para los eventos anómalos o inesperados en el flujo de la aplicación. Estos pueden incluir errores u otras condiciones que no hacen que la aplicación se detenga, pero que puede que sea necesario investigar. Las excepciones controladas son un lugar común para usar el nivel de registro `Warning`. Ejemplo: `FileNotFoundException for file quotes.txt.`

* Error = 4

  Para los errores y excepciones que no se pueden controlar. Estos mensajes indican un error en la actividad u operación actual (por ejemplo, la solicitud HTTP actual), no un error de toda la aplicación. Mensaje de registro de ejemplo: `Cannot insert record due to duplicate key violation.`

* Crítico = 5

  Para los errores que requieren atención inmediata. Ejemplos: escenarios de pérdida de datos, espacio en disco insuficiente.

Use el nivel de registro para controlar la cantidad de salida del registro que se escribe en un medio de almacenamiento determinado o ventana de presentación. Por ejemplo:

* En producción, envíe `Trace` a través del nivel `Information` a un almacén de datos de volumen. Envíe `Warning` a través de `Critical` a un almacén de datos de valor.
* Durante el desarrollo, envíe `Warning` a través de `Critical` a la consola y agregue `Trace` a través de `Information` cuando solucione problemas.

En la sección [Filtrado del registro](#log-filtering) de este artículo se explica cómo controlar los niveles de registro que controla un proveedor.

ASP.NET Core escribe registros de eventos de marco. En los ejemplos de registro anteriores de este artículo se excluyeron los registros por debajo del nivel `Information`, por lo que no se crearon los registros de nivel `Debug` o `Trace`. Este es un ejemplo de registros de consola generados mediante la ejecución de la aplicación de ejemplo configurada para mostrar registros `Debug`:

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

Cada registro se puede especificar un *id. de evento*. La aplicación de ejemplo lo hace mediante una clase `LoggingEvents` definida de forma local:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

Un id. de evento asocia un conjunto de eventos. Por ejemplo, todos los registros relacionados con la presentación de una lista de elementos en una página podrían ser 1001.

El proveedor de registro puede almacenar el id. de evento en un campo de identificador, en el mensaje de registro o no almacenarlo. El proveedor de depuración no muestra los identificadores de evento. El proveedor de consola muestra los identificadores de evento entre corchetes después de la categoría:

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>Plantilla de mensaje de registro

Cada registro especifica una plantilla de mensaje. La plantilla de mensaje puede contener marcadores de posición para los que se proporcionan argumentos. Use los nombres de los marcadores de posición, no números.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

El orden de los marcadores de posición, no sus nombres, determina qué parámetros se usan para proporcionar sus valores. En el código siguiente, tenga en cuenta que los nombres de parámetro están fuera de la secuencia en la plantilla de mensaje:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

Este código crea un mensaje de registro con los valores de parámetro en secuencia:

```
Parameter values: parm1, parm2
```

La plataforma de registro funciona de esta manera para que los proveedores de registro puedan implementar [el registro semántico, también conocido como registro estructurado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging). Los propios argumentos se pasan al sistema de registro, no solo a la plantilla de mensaje con formato. Esta información permite a los proveedores de registro almacenar los valores de parámetro como campos. Por ejemplo, suponga que las llamadas del método del registrador tiene el aspecto siguiente:

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Si envía los registros a Azure Table Storage, cada entidad de Azure Table puede tener propiedades `ID` y `RequestTime`, lo que simplifica las consultas en los datos de registro. Una consulta puede buscar todos los registros dentro de un intervalo `RequestTime` determinado sin analizar el tiempo de espera del mensaje de texto.

## <a name="logging-exceptions"></a>Excepciones de registro

Los métodos de registrador tienen sobrecargas que le permiten pasar una excepción, como en el ejemplo siguiente:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

Cada proveedor controla la información de la excepción de maneras diferentes. Este es un ejemplo de salida del proveedor de depuración del código mostrado anteriormente.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Filtrado del registro

::: moniker range=">= aspnetcore-2.0"

Puede especificar un nivel de registro mínimo para un proveedor y una categoría específicos, o para todos los proveedores o todas las categorías. Los registros por debajo del nivel mínimo no se pasan a ese proveedor, de modo que no se muestran o almacenan.

Para suprimir todos los registros, especifique `LogLevel.None` como el nivel de registro mínimo. El valor entero de `LogLevel.None` es 6, que es superior a `LogLevel.Critical` (5).

### <a name="create-filter-rules-in-configuration"></a>Creación de reglas de filtro en la configuración

El código de la plantilla de proyecto llama a `CreateDefaultBuilder` para configurar el registro para los proveedores de consola y de depuración. El método `CreateDefaultBuilder` también establece el registro para buscar la configuración en una sección `Logging`, mediante código similar al siguiente:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=16)]

Los datos de configuración especifican niveles de registro mínimo por proveedor y categoría, como en el ejemplo siguiente:

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

Este archivo JSON crea seis reglas de filtro, una para el proveedor de depuración, cuatro para el proveedor de la consola y una para todos los proveedores. Se elige una sola regla para cada proveedor cuando se crea un objeto `ILogger`.

### <a name="filter-rules-in-code"></a>Reglas de filtro en el código

En el siguiente ejemplo se muestra cómo registrar reglas de filtro en el código:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

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

Cuando se crea un objeto `ILogger`, el objeto `ILoggerFactory` selecciona una sola regla por proveedor para aplicar a ese registrador. Todos los mensajes escritos por una instancia `ILogger` se filtran según las reglas seleccionadas. De las reglas disponibles se selecciona la más específica posible para cada par de categoría y proveedor.

Cuando se crea un `ILogger` para una categoría determinada, se usa el algoritmo siguiente para cada proveedor:

* Se seleccionan todas las reglas que coinciden con el proveedor o su alias. Si no se encuentra ninguna coincidencia, se seleccionan todas las reglas con un proveedor vacío.
* Del resultado del paso anterior, se seleccionan las reglas con el prefijo de categoría coincidente más largo. Si no se encuentra ninguna coincidencia, se seleccionan todas las reglas que no especifican una categoría.
* Si se seleccionan varias reglas, se toma la **última**.
* Si no se selecciona ninguna regla, se usa `MinimumLevel`.

Con la lista de reglas anterior, supongamos que crea un objeto `ILogger` para la categoría "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":

* Para el proveedor de depuración, se aplican las reglas 1, 6 y 8. La regla 8 es la más específica, por lo que se selecciona.
* Para el proveedor de la consola, se aplican las reglas 3, 4, 5 y 6. La regla 3 es la más específica.

La instancia `ILogger` resultante envía los registros de nivel `Trace` y superiores al proveedor de depuración. Los registros de nivel `Debug` y superiores se envían al proveedor de consola.

### <a name="provider-aliases"></a>Alias de proveedor

Cada proveedor define un *alias* que se puede utilizar en la configuración en lugar del nombre de tipo completo.  Para los proveedores integrados, use los alias siguientes:

* Consola
* Depuración
* EventLog
* AzureAppServices
* TraceSource
* EventSource

### <a name="default-minimum-level"></a>Nivel mínimo predeterminado

Hay una configuración de nivel mínimo que solo tiene efecto si no se aplica ninguna regla de configuración o código para un proveedor y una categoría determinados. En el ejemplo siguiente se muestra cómo establecer el nivel mínimo:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

Si no establece explícitamente el nivel mínimo, el valor predeterminado es `Information`, lo que significa que los registros `Trace` y `Debug` se omiten.

### <a name="filter-functions"></a>Funciones de filtro

Se invoca una función de filtro para todos los proveedores y categorías que no tienen reglas asignadas mediante configuración o código. El código de la función tiene acceso al tipo de proveedor, la categoría y el nivel de registro. Por ejemplo:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Algunos proveedores de registro permiten especificar cuándo deben escribirse los registros en un medio de almacenamiento o ignorarse en función de la categoría y el nivel de registro.

Los métodos de extensión `AddConsole` y `AddDebug` proporcionan sobrecargas que aceptan criterios de filtrado. El código de ejemplo siguiente hace que el proveedor de la consola ignore los registros por debajo del nivel `Warning`, mientras que el proveedor de depuración omite los registros creados por la plataforma.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

El método `AddEventLog` tiene una sobrecarga que toma una instancia de `EventLogSettings`, que puede contener una función de filtrado en su propiedad `Filter`. El proveedor de TraceSource no proporciona ninguna de estas sobrecargas, dado que su nivel de registro y otros parámetros se basan en el `SourceSwitch` y `TraceListener` que usa.

Para establecer reglas de filtrado para todos los proveedores que están registrados con un instancia de `ILoggerFactory`, use el método de extensión `WithFilter`. En el ejemplo siguiente se limitan los registros de la plataforma (la categoría comienza con "Microsoft" o "System") a las advertencias mientras los registros creados por el código de aplicación se registran en el nivel de depuración.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Para evitar que los registros se escriban, especifique `LogLevel.None` como el nivel de registro mínimo. El valor entero de `LogLevel.None` es 6, que es superior a `LogLevel.Critical` (5).

El paquete NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) proporciona el método de extensión `WithFilter`. El método devuelve una instancia nueva de `ILoggerFactory` que filtrará los mensajes de registro pasados a todos los proveedores de registrador registrados con ella. No afecta a ninguna otra instancia de `ILoggerFactory`, incluida la instancia de `ILoggerFactory` original.

::: moniker-end

## <a name="system-categories-and-levels"></a>Niveles y categorías del sistema

Estas son algunas categorías que ASP.NET Core y Entity Framework Core usan, con notas sobre lo que los registros de espera de ellas:

| Categoría                            | Notas |
| ----------------------------------- | ----- |
| Microsoft.AspNetCore                | Diagnósticos generales de ASP.NET Core. |
| Microsoft.AspNetCore.DataProtection | Qué claves se tuvieron en cuenta, encontraron y usaron. |
| Microsoft.AspNetCore.HostFiltering  | Hosts permitidos. |
| Microsoft.AspNetCore.Hosting        | Cuánto tiempo tardaron en completarse las solicitudes HTTP y a qué hora comenzaron. Qué ensamblados de inicio de hospedaje se cargaron. |
| Microsoft.AspNetCore.Mvc            | Diagnósticos de MVC y Razor. Enlace de modelos, ejecución de filtros, compilación de vistas y selección de acciones. |
| Microsoft.AspNetCore.Routing        | Información de coincidencia de ruta. |
| Microsoft.AspNetCore.Server         | Inicio y detención de conexión y mantener las respuestas activas. Información de certificado HTTPS. |
| Microsoft.AspNetCore.StaticFiles    | Archivos servidos. |
| Microsoft.EntityFrameworkCore       | Diagnósticos generales de Entity Framework Core. Actividad y la configuración de bases de datos, detección de cambios y migraciones. |

## <a name="log-scopes"></a>Ámbitos de registro

 Un *ámbito* puede agrupar un conjunto de operaciones lógicas. Esta agrupación se puede utilizar para adjuntar los mismos datos para cada registro que se crea como parte de un conjunto. Por ejemplo, cada registro creado como parte del procesamiento de una transacción puede incluir el identificador de dicha transacción.

Un ámbito es un tipo `IDisposable` devuelto por el método <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> y se conserva hasta que se elimina. Use un ámbito encapsulando las llamadas de registrador en un bloque `using`:

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

El código siguiente permite ámbitos para el proveedor de la consola:

::: moniker range="> aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Es necesario configurar la opción del registrador de consola `IncludeScopes` para habilitar el registro basado en el ámbito.
>
> Para obtener información sobre la configuración, consulte la sección [Configuración](#configuration).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Es necesario configurar la opción del registrador de consola `IncludeScopes` para habilitar el registro basado en el ámbito.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

*Startup.cs*:

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

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

Las opciones para el [registro en Azure](#logging-in-azure) se tratan más adelante en este artículo.

Para información sobre el registro de stdout, consulte <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log> y <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.

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

[Las sobrecargas de AddConsole](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) permiten pasar un nivel de registro mínimo, una función de filtro y un valor booleano que indica si se admiten los ámbitos. Otra opción consiste en pasar un objeto `IConfiguration`, que puede especificar niveles de registro y si se admiten los ámbitos.

El proveedor de consola tiene un impacto importante en el rendimiento y no suele ser adecuado para su uso en producción.

Cuando se crea un proyecto en Visual Studio, el método `AddConsole` tiene el aspecto siguiente:

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Este código hace referencia a la sección `Logging` del archivo *appSettings.json*:

[!code-json[](index/samples/1.x/TodoApiSample//appsettings.json)]

La configuración que se muestra limita los registros de la plataforma a las advertencias mientras que permite a la aplicación registrar en el nivel de depuración, como se explica en la sección [Filtrado del registro](#log-filtering). Para obtener más información, vea [Configuración](xref:fundamentals/configuration/index).

::: moniker-end

Para ver una salida de registro de la consola, abra un símbolo del sistema en la carpeta del proyecto y ejecute este comando:

```console
dotnet run
```

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

[Las sobrecargas de AddDebug](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) permiten pasar un nivel mínimo de registro o una función de filtro.

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

[Las sobrecargas de AddEventLog](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) permiten pasar `EventLogSettings` o un nivel de registro mínimo.

::: moniker-end

### <a name="tracesource-provider"></a>Proveedor TraceSource

El paquete de proveedor [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) usa las bibliotecas y proveedores de <xref:System.Diagnostics.TraceSource>.

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

[Las sobrecargas de AddTraceSource](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) permiten pasar un modificador de origen y un agente de escucha de seguimiento.

Para usar este proveedor, una aplicación debe ejecutarse en .NET Framework (en lugar de .NET Core). El proveedor puede enrutar mensajes a una variedad de [agentes de escucha](/dotnet/framework/debug-trace-profile/trace-listeners), como <xref:System.Diagnostics.TextWriterTraceListener> que se usa en la aplicación de ejemplo.

::: moniker range="< aspnetcore-2.0"

En el ejemplo siguiente se configura un proveedor `TraceSource` que registra mensajes `Warning` y superiores en la ventana de consola.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

## <a name="logging-in-azure"></a>Registro en Azure

Para información sobre el registro en Azure, consulte las secciones siguientes:

* [Proveedor Azure App Service](#azure-app-service-provider)
* [Secuencias de registro de Azure](#azure-log-streaming)

::: moniker range=">= aspnetcore-1.1"

* [Registro de seguimiento de Azure Application Insights](#azure-application-insights-trace-logging)

::: moniker-end

### <a name="azure-app-service-provider"></a>Proveedor Azure App Service

El paquete de proveedor [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) escribe los registros en archivos de texto en el sistema de archivos de una aplicación de Azure App Service y en [Blob Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) en una cuenta de Azure Storage. El paquete de proveedor está disponible para aplicaciones cuyo destino sea .NET Core 1.1 o una versión posterior.

::: moniker range=">= aspnetcore-2.0"

Si el destino es .NET Core, tenga en cuenta lo siguiente:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* El paquete de proveedor está incluido en el [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage) de ASP.NET Core.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* El paquete de proveedor no está incluido en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Para usar el proveedor, instale el paquete.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

* No llame explícitamente a <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*>. El proveedor está disponible automáticamente para la aplicación cuando esta se implementa en Azure App Service.

Si el destino es .NET Framework o hace referencia al metapaquete `Microsoft.AspNetCore.App`, agregue el paquete de proveedor al proyecto. Invoke `AddAzureWebAppDiagnostics`:

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

Una sobrecarga <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> permite pasar <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>. El objeto de configuración puede invalidar la configuración predeterminada, como la plantilla de salida de registro, el nombre de blob y el límite de tamaño de archivo. (La *plantilla salida* es una plantilla de mensaje que se aplica a todos los registros además de que se proporciona con una llamada de método `ILogger`).

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Para configurar las opciones de proveedor, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> y <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, tal y como se muestra en el ejemplo siguiente:

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

Cuando se implementa en una aplicación de App Service, la aplicación respeta la configuración de la sección [Registros de diagnóstico](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) de la página **App Service** de Azure Portal. Cuando se actualiza esta configuración, los cambios se aplican inmediatamente sin necesidad de reiniciar ni de volver a implementar la aplicación.

![Configuración de los registros de Azure](index/_static/azure-logging-settings.png)

La ubicación predeterminada de los archivos de registro es la carpeta *D:\\home\\LogFiles\\Application* y el nombre de archivo predeterminado es *diagnostics-aaaammdd.txt*. El límite de tamaño de archivo predeterminado es 10 MB, y el número máximo predeterminado de archivos que se conservan es 2. El nombre de blob predeterminado es *{nombre-de-la-aplicación}{marca de tiempo}/aaaa/mm/dd/hh/{guid}-applicationLog.txt*.

El proveedor solo funciona cuando el proyecto se ejecuta en el entorno de Azure. No tiene ningún efecto cuando el proyecto se ejecuta de manera local (no escribe en los archivos locales ni en el almacenamiento de desarrollo local de blobs).

### <a name="azure-log-streaming"></a>Secuencias de registro de Azure

Las secuencias de registro de Azure permiten ver la actividad de registro en tiempo real desde:

* El servidor de aplicaciones
* El servidor web
* Error del seguimiento de solicitudes

Para configurar las secuencias de registro de Azure:

* Navegue hasta la página **Registros de diagnóstico** desde la página de portal de la aplicación.
* Establezca **Registro de la aplicación (sistema de archivos)** en **Activado**.

![Página de registros de diagnóstico de Azure Portal](index/_static/azure-diagnostic-logs.png)

Navegue hasta la página **Secuencias de registro** para ver los mensajes de la aplicación. La aplicación los registra a través de la interfaz `ILogger`.

![Secuencias de registro de aplicación de Azure Portal](index/_static/azure-log-streaming.png)

::: moniker range=">= aspnetcore-1.1"

### <a name="azure-application-insights-trace-logging"></a>Registro de seguimiento de Azure Application Insights

El SDK de Application Insights puede recopilar y notificar los registros que la infraestructura de registro de ASP.NET Core genera. Para obtener más información, vea los siguientes recursos:

* [Información general de Application Insights](/azure/application-insights/app-insights-overview)
* [Application Insights para ASP.NET Core](/azure/application-insights/app-insights-asp-net-core)
* [Adaptadores de registro de Application Insights](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md)
* [Muestras de implementación de Application Insights ILogger](/azure/azure-monitor/app/ilogger)

::: moniker-end

## <a name="third-party-logging-providers"></a>Proveedores de registro de terceros

Plataformas de registro de terceros que funcionan con ASP.NET Core:

* [elmah.io](https://elmah.io/) ([repositorio de GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))
* [Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([repositorio de GitHub](https://github.com/mattwcole/gelf-extensions-logging))
* [JSNLog](http://jsnlog.com/) ([repositorio de GitHub](https://github.com/mperdeck/jsnlog))
* [KissLog.net](https://kisslog.net/) ([repositorio de GitHub](https://github.com/catalingavan/KissLog-net))
* [Loggr](http://loggr.net/) ([repositorio de GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))
* [NLog](http://nlog-project.org/) ([repositorio de GitHub](https://github.com/NLog/NLog.Extensions.Logging))
* [Sentry](https://sentry.io/welcome/) ([repositorio de GitHub](https://github.com/getsentry/sentry-dotnet))
* [Serilog](https://serilog.net/) ([repositorio de GitHub](https://github.com/serilog/serilog-extensions-logging))
* [Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([repositorio de GitHub](https://github.com/googleapis/google-cloud-dotnet))

Algunas plataformas de terceros pueden realizar [registro semántico, también conocido como registro estructurado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

El uso de una plataforma de terceros es similar al uso de uno de los proveedores integrados:

1. Agregue un paquete NuGet al proyecto.
1. Llame a `ILoggerFactory`.

Para más información, vea la documentación de cada proveedor. Microsoft no admite los proveedores de registro de terceros.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/logging/loggermessage>
