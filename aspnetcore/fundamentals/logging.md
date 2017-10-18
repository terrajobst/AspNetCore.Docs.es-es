---
title: "Inicio de sesión principal de ASP.NET"
author: ardalis
description: "Obtenga información sobre el marco de trabajo de registro de ASP.NET Core. Detectar los proveedores de registro integrados y obtener más información sobre los proveedores de terceros populares."
keywords: "Núcleo de ASP.NET, el registro, el registro providers,Microsoft.Extensions.Logging,ILogger,ILoggerFactory,LogLevel,WithFilter,TraceSource,EventLog,EventSource,scopes"
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ac27ac68-d76a-4f8e-b8ab-ea045803e5f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b2e991ea37b1b726e472d78d839143546ebd559f
ms.sourcegitcommit: 29da58de11e20c9c60448e36e7075c6b13622624
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2017
---
# <a name="introduction-to-logging-in-aspnet-core"></a>Introducción al inicio de sesión principal de ASP.NET

Por [Steve Smith](https://ardalis.com/) y [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core es compatible con una API de registro que funciona con una variedad de proveedores de registro. Proveedores integrados permiten enviar registros a uno o varios destinos, y puede conectar un marco de trabajo de registro de aplicaciones de terceros. Este artículo muestra cómo usar las API de registro integrados y los proveedores en el código.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample2) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

---

## <a name="how-to-create-logs"></a>Cómo crear registros

Para crear registros, obtener una `ILogger` objeto desde el [inyección de dependencia](dependency-injection.md) contenedor:

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

A continuación, llamar a métodos de registro de ese objeto de registrador:

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Este ejemplo crea registros con la `TodoController` de clase como el *categoría*.  Categorías se explican [más adelante en este artículo](#log-category).

ASP.NET Core no proporciona async registrador métodos porque el registro debe ser tan rápido que no vale la pena el costo de usar async. Si se encuentra en una situación en los que no es así, considere la posibilidad de cambiar el modo de que iniciar sesión.  Si el almacén de datos es lento, escribir los mensajes de registro a un almacén rápido en primer lugar, a continuación, moverlos a un almacén de baja velocidad. Por ejemplo, inicie sesión en una cola de mensajes que se lee y se conservan en almacenamiento lento por otro proceso.

## <a name="how-to-add-providers"></a>Cómo agregar proveedores

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Un proveedor de registro toma los mensajes que se crean con un `ILogger` de objeto y muestra o almacenarlos. Por ejemplo, el proveedor de la consola muestra mensajes en la consola y el proveedor de servicios de aplicación de Azure puede almacenar en el almacenamiento de blobs de Azure.

Para usar un proveedor, póngase en contacto el proveedor `Add<ProviderName>` método de extensión en *Program.cs*:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

La plantilla de proyecto predeterminada establece la creación de registros de la manera en que puede ver en el código anterior, pero la `ConfigureLogging` llamada se realiza mediante el `CreateDefaultBuilder` método. Este es el código *Program.cs* que se crean mediante plantillas de proyecto:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Un proveedor de registro toma los mensajes que se crean con un `ILogger` de objeto y muestra o almacenarlos. Por ejemplo, el proveedor de la consola muestra mensajes en la consola y el proveedor de servicios de aplicación de Azure puede almacenar en el almacenamiento de blobs de Azure.

Para usar un proveedor, instale el paquete de NuGet y llame al método de extensión del proveedor en una instancia de `ILoggerFactory`, como se muestra en el ejemplo siguiente.

[!code-csharp[](logging/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

ASP.NET Core [inyección de dependencia](dependency-injection.md) (DI) proporciona el `ILoggerFactory` instancia. El `AddConsole` y `AddDebug` métodos de extensión se definen en el [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) y [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) paquetes. Llama a cada método de extensión el `ILoggerFactory.AddProvider` método, pasando una instancia del proveedor. 

> [!NOTE]
> La aplicación de ejemplo de este artículo agrega proveedores de registro en el `Configure` método de la `Startup` clase. Si desea obtener la salida de registro del código que se ejecuta en versiones anteriores, agregar proveedores de registro en el `Startup` constructor de la clase en su lugar. 

---

Puede encontrar información sobre cada [proveedor de registro integrados](#built-in-logging-providers) y vínculos a [proveedores de registro de aplicaciones de terceros](#third-party-logging-providers) más adelante en el artículo.

## <a name="sample-logging-output"></a>Resultados del registro de ejemplo

Con el código de ejemplo que se muestra en la sección anterior, verá los registros en la consola cuando se ejecuta desde la línea de comandos. Este es un ejemplo de salida de la consola:

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
 
Estos registros se crearon yendo a `http://localhost:5000/api/todo/0`, lo que desencadena la ejecución de ambos `ILogger` llamadas se muestra en la sección anterior.

Este es un ejemplo de los mismos registros que aparecen en la ventana de depuración cuando se ejecute la aplicación de ejemplo en Visual Studio:

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

Los registros creados por el `ILogger` llamadas se muestra en la sección anterior comienzan por "TodoApi.Controllers.TodoController". Los registros que comienzan por categorías de "Microsoft" son de ASP.NET Core. ASP.NET Core propio y el código de aplicación están utilizando las mismas API de registro y los mismos proveedores de registro.

El resto de este artículo explica algunos detalles y las opciones para el registro.

## <a name="nuget-packages"></a>Paquetes NuGet

El `ILogger` y `ILoggerFactory` interfaces se encuentran en [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), y las implementaciones predeterminadas para ellos en [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).

## <a name="log-category"></a>Categoría de registro

A *categoría* se incluye con cada registro creados por usted.  Especifica la categoría cuando se crea un `ILogger` objeto. La categoría puede ser cualquier cadena, pero una convención consiste en utilizar el nombre completo de la clase desde el que se escriben los registros.  Por ejemplo: "TodoApi.Controllers.TodoController".

Puede especificar la categoría como una cadena o utilice un método de extensión que se deriva la categoría del tipo. Para especificar la categoría como una cadena, llame a `CreateLogger` en un `ILoggerFactory` de instancia, tal y como se muestra a continuación.

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

La mayoría de los casos, le resultará más fácil usar `ILogger<T>`, como en el ejemplo siguiente.

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Esto equivale a llamar a `CreateLogger` con el nombre de tipo completo de `T`.

## <a name="log-level"></a>Nivel de registro

Cada vez que se escribe un registro, especifique su [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel). El nivel de registro indica el grado de gravedad o importancia.  Por ejemplo, puede escribir una `Information` iniciar sesión cuando un método finaliza normalmente, un `Warning` cuando un método devuelve un código de retorno de un error 404 y un registro `Error` registrar cuando se detecta una excepción inesperada.

En el ejemplo de código siguiente, los nombres de los métodos (por ejemplo, `LogWarning`) especificar el nivel de registro.  El primer parámetro es el [Id. de evento de registro](#log-event-id) (se explica más adelante en este artículo).  Los parámetros restantes construcción una cadena de mensaje de registro.

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Los métodos de registro que incluyen el nivel en el nombre del método están [métodos de extensión para ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions). En segundo plano, llame estos métodos una `Log` método que toma un `LogLevel` parámetro. Puede llamar a la `Log` (método) directamente en lugar de uno de estos métodos de extensión, pero la sintaxis es relativamente complicado. Para obtener más información, consulte el [interfaz ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) y [código fuente de las extensiones de registrador](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).

ASP.NET Core define las siguientes [niveles de registro](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), aquí ordenados de menor a mayor grado de gravedad.

* Seguimiento = 0

  Para obtener información que es útil sólo para un desarrollador depurar un problema. Estos mensajes pueden contener datos confidenciales de la aplicación y por lo que no deben habilitarse en un entorno de producción. *Deshabilitada de forma predeterminada.* Ejemplo: `Credentials: {"User":"someuser", "Password":"P@ssword"}`

* Depurar = 1

  Para obtener información que tiene la utilidad a corto plazo durante el desarrollo y depuración. Ejemplo: `Entering method Configure with flag set to true.` normalmente no se debería habilitar `Debug` nivel se registra en producción, a menos que esté solucionando, debido al elevado volumen de registros.

* Información = 2

  Para supervisar el flujo general de la aplicación. Estos registros suelen tengan algún valor a largo plazo. Ejemplo: `Request received for path /api/todo`

* Advertencia = 3

  Para los eventos anómalos o inesperados en el flujo de la aplicación. Estos pueden incluir errores u otras condiciones que hacen que la aplicación detener, pero puede que deba investigar. Las excepciones administradas son un lugar común para utilizar el `Warning` nivel de registro. Ejemplo: `FileNotFoundException for file quotes.txt.`

* Error = 4

  Para los errores y excepciones que no se puede controlar. Estos mensajes indican un error en la actividad actual o la operación (por ejemplo, la solicitud HTTP actual), no un error de toda la aplicación. Mensaje de registro de ejemplo:`Cannot insert record due to duplicate key violation.`

* Crítico = 5

  Para los errores que requieren atención inmediata. Ejemplos: escenarios de pérdida de datos de espacio en disco insuficiente.

Puede utilizar el nivel de registro para controlar cuántos resultados del registro se escriben en un medio de almacenamiento determinado o mostrar la ventana. Por ejemplo, en producción podría interesar todos los registros de `Information` nivel y reducir para ir a un almacén de datos de volumen y todos los registros de `Warning` nivel y versiones posteriores para ir a un almacén de datos de valor. Durante el desarrollo, normalmente puede enviar registros de `Warning` o en la consola de mayor gravedad. A continuación si necesita solucionar problemas, puede agregar `Debug` nivel. El [filtrado del registro](#log-filtering) sección más adelante en este artículo explica cómo controlar qué niveles de registro que se trata de un proveedor.

Escribe el marco de trabajo de ASP.NET Core `Debug` nivel registros de eventos de framework. Los ejemplos de registro anteriormente en este artículo excluyen registros siguiente `Information` nivel, por lo que no `Debug` se mostraron registros de nivel. Este es un ejemplo de registros de la consola si ejecuta la aplicación de ejemplo configurada para mostrar `Debug` y registros superior para el proveedor de la consola.

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

## <a name="log-event-id"></a>Id. de evento de registro

Cada vez que se escribe un registro, puede especificar un *Id. de evento*. La aplicación de ejemplo lo hace al utilizar un definido localmente `LoggingEvents` clase:

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](logging/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

Un identificador de evento es un valor entero que puede utilizar para asociar un conjunto de eventos registrados entre sí. Por ejemplo, un registro para agregar un elemento a un carro de la compra podría ser el identificador de evento 1000 y un registro para completar una compra podría ser 1001 de Id. de evento.

En los resultados del registro, el identificador de evento podría estar almacenado en un campo o incluido en el mensaje de texto, en función del proveedor.  El proveedor de depuración no muestra el Id. de evento, pero el proveedor de la consola muestra entre paréntesis después de la categoría:

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-format-string"></a>Cadena de formato de mensaje de registro

Cada vez que escriba un registro, proporcionar un mensaje de texto. La cadena de mensaje puede contener marcadores de posición con nombre en el argumento que se colocan valores, como en el ejemplo siguiente:

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

El orden de los marcadores de posición, no por sus nombres, determina qué parámetros se utilizan para ellos. Por ejemplo, si tiene el siguiente código:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

El mensaje del registro resultante sería similar al siguiente:

```
Parameter values: parm1, parm2
```

El marco de trabajo de registro de mensajes de formato de esta manera para que sea posible para los proveedores de registro implementar [registro semántica, también conocido como registro estructurado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging). Dado que los argumentos se pasan al sistema de registro, no solo la cadena de mensaje con formato, los proveedores de registro pueden almacenar los valores de parámetro como campos además de la cadena de mensaje. Por ejemplo, si dirigen el registro de salida en el almacenamiento de tabla de Azure y la llamada al método de registrador tiene el siguiente aspecto:

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Cada entidad de tabla de Azure podría tener `ID` y `RequestTime` propiedades, que se simplifican las consultas en los datos de registro. Puede buscar todos los registros dentro de un determinado `RequestTime` intervalo, sin tener que analizar el tiempo de espera del mensaje de texto.

## <a name="logging-exceptions"></a>Registro de excepciones

Los métodos de registrador tienen sobrecargas que le permiten pasar una excepción, como en el ejemplo siguiente:

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

Diferentes proveedores de controlan la información de excepción de maneras diferentes. Este es un ejemplo de salida de proveedor de depuración desde el código mostrado anteriormente.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Filtrado del registro

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Puede especificar un nivel de registro mínimo de un proveedor específico y una categoría o para todos los proveedores o todas las categorías.  Los registros por debajo del nivel mínimo no se pasan a ese proveedor, de modo que no obtener mostrados o almacenados. 

Si desea suprimir todos los registros, puede especificar `LogLevel.None` como el nivel de registro mínimo. El valor del entero de `LogLevel.None` es 6, que son superiores a `LogLevel.Critical` (5).

**Crear reglas de filtro en la configuración**

Las plantillas de proyecto crean código que llama a `CreateDefaultBuilder` para configurar el registro para los proveedores de consola y de depuración. El `CreateDefaultBuilder` método también establece el registro para buscar la configuración en un `Logging` sección, mediante código similar al siguiente:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

Los datos de configuración especifican niveles de registro mínimo al proveedor y categoría, como en el ejemplo siguiente:

[!code-json[](logging/sample2/appsettings.json)]

Este archivo JSON crea seis reglas de filtro, uno para el proveedor de depuración, 4 para el proveedor de la consola y que se aplica a todos los proveedores. Podrá ver más adelante cómo solamente una de estas reglas se elige para cada proveedor cuando un `ILogger` se crea el objeto.

**Reglas de filtro en el código**

Puede registrar las reglas de filtro en el código, tal como se muestra en el ejemplo siguiente:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

El segundo `AddFilter` especifica el proveedor de depuración mediante su nombre de tipo. La primera `AddFilter` se aplica a todos los proveedores, dado que no especifica un tipo de proveedor.

**Cómo filtrar las reglas se aplican**

Los datos de configuración y la `AddFilter` código que se muestra en los ejemplos anteriores crear las reglas que se muestran en la tabla siguiente. Los seis primeros proceden del ejemplo de configuración y las dos últimas proceden del ejemplo de código.

Número|Proveedor|Categorías que comienzan por|Nivel de registro mínimo|
------|--------|--------------------------|-----------------|
1|Depuración|Todas las categorías|Información|
2|Consola|Microsoft.AspNetCore.Mvc.Razor.Internal|Advertencia|
3|Consola|Microsoft.AspNetCore.Mvc.Razor.Razor|Depuración|
4|Consola|Microsoft.AspNetCore.Mvc.Razor|Error|
5|Consola|Todas las categorías|Información|
6|Todos los proveedores|Todas las categorías|Depuración
7|Todos los proveedores|Sistema|Depuración
8|Depuración|Microsoft|Seguimiento

Cuando se crea un `ILogger` objeto para escribir los registros, la `ILoggerFactory` objeto selecciona una sola regla por proveedor para aplicar a dicho registrador. Todos los mensajes escritos por esa `ILogger` objeto se filtra según las reglas seleccionadas. Se selecciona la regla más específica posible para cada par de categoría y el proveedor de las reglas disponibles.

Se utiliza el algoritmo siguiente para cada proveedor cuando un `ILogger` se crea para una categoría determinada:

* Seleccionar todas las reglas que coinciden con el proveedor o su alias.  Si no se encuentra ninguno, seleccione todas las reglas con un proveedor vacío.
* Desde el resultado del paso anterior, seleccione las reglas con mayor tiempo coincidencia de prefijo de categoría. Si no se encuentra ninguno, seleccione todas las reglas que no se especifican una categoría.
* Si se seleccionan varias reglas de tomar la **última** uno.
* Si no se selecciona ninguna regla, use `MinimumLevel`.
 
Por ejemplo, supongamos que tiene la lista de reglas anterior y se crea un `ILogger` objeto de categoría "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":

* Para el proveedor de depuración, se aplican las reglas 1, 6 y 8. Regla 8 es más específico, por lo que está seleccionado.
* Para el proveedor de la consola, se aplican las reglas de 3, 4, 5 y 6. La regla 3 es más específica.

Cuando crea registros con un `ILogger` para la categoría "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", los registros de `Trace` nivel y versiones posteriores será dirigido a los proveedores de depuración y registros de `Debug` nivel y versiones posteriores pasa al proveedor de consola.

**Alias de proveedor**

Puede usar el nombre de tipo para especificar un proveedor de configuración, pero cada proveedor define una menor *alias* que es más fácil de usar. Para los proveedores integrados, use los siguientes alias:

- Consola
- Depuración
- Registro de eventos
- AzureAppServices
- TraceSource
- EventSource

**Nivel mínimo de manera predeterminada**

No hay una configuración de nivel mínima que solo tiene efecto si se aplica ninguna regla de configuración o código de un proveedor determinado y una categoría. En el ejemplo siguiente se muestra cómo establecer el nivel mínimo:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

Si no establece explícitamente el nivel mínimo, el valor predeterminado es `Information`, lo que significa que `Trace` y `Debug` se omiten los registros.

**Funciones de filtro**

Puede escribir código en una función de filtro para aplicar las reglas de filtrado. Se invoca una función de filtro para todos los proveedores y las categorías que no tiene reglas asignadas mediante configuración o código. Código de la función tiene acceso al tipo de proveedor, categoría y nivel de registro para decidir si se debe registrar un mensaje. Por ejemplo:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Algunos proveedores de registro permiten especificar cuando los registros se deben escritos en un medio de almacenamiento o pasa por alto en función de categoría y nivel de registro.

El `AddConsole` y `AddDebug` métodos de extensión proporcionan sobrecargas que le permiten pasar en criterios de filtrado. El código de ejemplo siguiente hace que el proveedor de la consola pasar por alto los registros siguientes `Warning` nivel, mientras que el proveedor de depuración omite registros creados por el marco de trabajo.

[!code-csharp[](logging/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

El `AddEventLog` método tiene una sobrecarga que toma un `EventLogSettings` instancia, lo que puede contener una función de filtrado en su `Filter` propiedad. El proveedor de TraceSource no proporciona ninguna de dichas sobrecargas, puesto que su nivel de registro y otros parámetros se basan en el `SourceSwitch` y `TraceListener` utiliza.

Puede establecer reglas de filtrado para todos los proveedores que están registrados con un `ILoggerFactory` instancia mediante el uso de la `WithFilter` método de extensión. En el ejemplo siguiente limita los registros de framework (categoría comienza con "Microsoft" o "System") para las advertencias al permitir que el registro de aplicación en el nivel de depuración.

[!code-csharp[](logging/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Si desea usar el filtrado para impedir que todos los registros que se escriben para una categoría concreta, puede especificar `LogLevel.None` como el nivel de registro mínimo de esa categoría. El valor del entero de `LogLevel.None` es 6, que son superiores a `LogLevel.Critical` (5).

El `WithFilter` se proporciona el método de extensión mediante la [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) paquete NuGet. El método devuelve un nuevo `ILoggerFactory` instancia que se filtrará los mensajes de registro pasados a todos los proveedores de registrador registrados con él. No no afecta a ningún otro `ILoggerFactory` instancias, incluido el original `ILoggerFactory` instancia.

---

## <a name="log-scopes"></a>Ámbitos de registro

Puede agrupar un conjunto de operaciones lógicas dentro de un *ámbito* para adjuntar los mismos datos para cada registro que se crea como parte de dicho conjunto.  Por ejemplo, podría desea que cada registro creado como parte del procesamiento de transacciones para incluir el identificador de transacción.

Un ámbito es un `IDisposable` tipo devuelto por la `ILogger.BeginScope<TState>` método y permanecen activas hasta que se elimina. Para utilizar un ámbito, ajuste su registrador llama en un `using` bloquear, como se muestra aquí:

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

El código siguiente permite ámbitos para el proveedor de la consola:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

En *Program.cs*:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

En *Startup.cs*:

[!code-csharp[](logging/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

Cada mensaje del registro incluye la información de ámbito:

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

* [Consola](#console)
* [Depurar](#debug)
* [EventSource](#eventsource)
* [EventLog](#eventlog)
* [TraceSource](#tracesource)
* [Azure App Service](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a>El proveedor de la consola

El [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) el paquete de proveedor envía los resultados de registro en la consola. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

[Las sobrecargas de AddConsole](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) permiten pasar en un un nivel de registro mínimo, una función de filtro y un valor booleano que indica si se admiten los ámbitos.  Otra opción consiste en pasar un `IConfiguration` objeto, que puede especificar niveles de registro y de soporte técnico de ámbitos. 

Si está pensando en la consola de proveedor para su uso en producción, tenga en cuenta que tiene un impacto significativo en el rendimiento.

Cuando crea un nuevo proyecto en Visual Studio, el `AddConsole` método tiene el siguiente aspecto:

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Este código hace referencia a la `Logging` sección de la *appSettings.JSON que se* archivo:

[!code-json[](logging/sample//appsettings.json)]

La configuración que muestra registros de framework de límite para las advertencias mientras que permite a la aplicación para iniciar sesión en el nivel de depuración, como se explica en la [filtrado del registro](#log-filtering) sección. Para obtener más información, vea [Configuración](configuration.md).

---

<a id="debug"></a>
### <a name="the-debug-provider"></a>El proveedor de depuración

El [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) el paquete de proveedor escribe la salida del registro mediante el [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) clase (`Debug.WriteLine` llamadas a métodos).

En Linux, este proveedor escribe registros en */var/log/message*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

[Las sobrecargas de AddDebug](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) permiten pasar en un nivel mínimo de registro o una función de filtro.

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a>El proveedor EventSource

Para las aplicaciones que tienen como destino ASP.NET Core 1.1.0 o superior, el [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) el paquete de proveedor puede implementar el seguimiento de eventos. En Windows, usa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803). El proveedor es entre plataformas, pero no hay ningún evento herramientas de recopilación y visualización aún para Linux o Mac OS. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

Una buena manera de recopilar y ver los registros es usar la [PerfView utilidad](https://www.microsoft.com/download/details.aspx?id=28567). Hay otras herramientas para ver los registros ETW, pero PerfView proporciona la mejor experiencia para trabajar con los eventos ETW emitidos por ASP.NET. 

Para configurar PerfView para recopilar eventos registrados por este proveedor, agregue la cadena `*Microsoft-Extensions-Logging` a la **proveedores adicionales** lista. (No se pierda el asterisco al principio de la cadena.)

![Proveedores de Perfview adicionales](logging/_static/perfview-additional-providers.png)

Captura de eventos en Nano Server, requiere alguna configuración adicional:

* Conectar la comunicación remota de PowerShell con Nano Server:

  ```powershell
  Enter-PSSession [name]
  ```

* Crear una sesión de ETW:

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* Agregar proveedores ETW para [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers), ASP.NET Core y otros elementos según sea necesario. El proveedor de ASP.NET Core GUID es `3ac73b97-af73-50e9-0822-5da4367920d0`. 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* Ejecute el sitio Web y realice las acciones que desee obtener información de seguimiento para.

* Detener la sesión de seguimiento cuando haya terminado:

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

Resultante *C:\trace.etl* archivo se pueden analizar con PerfView como en otras ediciones de Windows.

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a>El proveedor de registro de eventos de Windows

El [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) el paquete de proveedor envía la salida de registro en el registro de eventos de Windows.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

[Las sobrecargas de método AddEventLog](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) permiten pasar en `EventLogSettings` o un nivel de registro mínimo.

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a>El proveedor de TraceSource

El [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) proveedor paquete utiliza la [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource) bibliotecas y los proveedores.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

[Las sobrecargas de AddTraceSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) permiten pasar en un modificador de origen y un agente de escucha de seguimiento.

Para utilizar este proveedor, una aplicación debe ejecutarse en el .NET Framework (en lugar de .NET Core). El proveedor permite enrutar mensajes a una variedad de [los agentes de escucha](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners), como el [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr) usado en la aplicación de ejemplo.

En el ejemplo siguiente se configura un `TraceSource` proveedor que registra `Warning` y mensajes superior en la ventana de consola.

[!code-csharp[](logging/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a>El proveedor de servicios de aplicación de Azure

El [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) el paquete de proveedor escribe registros en archivos de texto en el sistema de archivos de una aplicación de servicio de aplicaciones de Azure y al [almacenamiento de blobs](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) en una cuenta de almacenamiento de Azure. El proveedor está disponible únicamente para las aplicaciones que tienen como destino ASP.NET Core 1.1.0 o superior. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

No tienes que instalar el paquete de proveedor o llame a la `AddAzureWebAppDiagnostics` método de extensión.  El proveedor está disponible automáticamente para la aplicación al implementar la aplicación en el servicio de aplicaciones de Azure.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

Un `AddAzureWebAppDiagnostics` sobrecarga permite pasar en [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs), con el que puede invalidar la configuración predeterminada, como la plantilla de registro de salida, el nombre de blob y el límite de tamaño de archivo. (*Plantilla salida* es una cadena de formato de mensaje que se aplica a todos los registros, en la parte superior que proporciona cuando se llama a un `ILogger` método.)  

---

Cuando se implementa en una aplicación de servicio de aplicaciones, la aplicación respeta la configuración de la [registros de diagnóstico](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) sección de la **servicio de aplicaciones** página del portal de Azure. Cuando cambia esta configuración, los cambios surten efecto inmediatamente sin necesidad de reiniciar la aplicación ni de volver a implementar código en él. 

![Configuración de registro de Azure](logging/_static/azure-logging-settings.png)

La ubicación predeterminada de los archivos de registro está en la *D:\\principal\\LogFiles\\aplicación* carpeta y el nombre de archivo predeterminado es *yyyymmdd.txt diagnósticos*. El límite de tamaño de archivo predeterminado es 10 MB, y el número máximo predeterminado de archivos conserva es 2. El nombre de blob predeterminado es *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*. Para obtener más información sobre el comportamiento predeterminado, consulte [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).

El proveedor solo funciona cuando se ejecuta el proyecto en el entorno de Azure.  No tiene ningún efecto cuando se ejecuta localmente &mdash; no los escribe en archivos locales o almacenamiento de desarrollo local para los blobs.

## <a name="third-party-logging-providers"></a>Proveedores de registro de aplicaciones de terceros

Estos son algunos marcos de registro de aplicaciones de terceros que funcionan con ASP.NET Core:

* [ELMAH.IO](https://github.com/elmahio/Elmah.Io.Extensions.Logging) -proveedor para el servicio Elmah.Io

* [JSNLog](http://jsnlog.com) -registra las excepciones de JavaScript y otros eventos de cliente en el registro de servidor.

* [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) -proveedor para el servicio Loggr

* [NLog](https://github.com/NLog/NLog.Extensions.Logging) -proveedor para la biblioteca de NLog

* [Serilog](https://github.com/serilog/serilog-extensions-logging) -proveedor para la biblioteca de Serilog

Algunos marcos de terceros pueden hacer [registro semántica, también conocido como registro estructurado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Con el marco de terceros es similar al uso de uno de los proveedores integrados: agregar un paquete de NuGet al proyecto y llame a un método de extensión en `ILoggerFactory`. Para obtener más información, consulte la documentación de cada marco de trabajo.

Puede crear sus propios proveedores personalizados, para admitir otros marcos de registro o sus propios requisitos de registro.
