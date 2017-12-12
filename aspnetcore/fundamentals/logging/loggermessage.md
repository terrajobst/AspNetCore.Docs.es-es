---
title: Registro de alto rendimiento con LoggerMessage en ASP.NET Core
author: guardrex
description: "Obtenga información acerca de cómo utilizar características de LoggerMessage para crear a delegados almacenable en caché que requieren menos las asignaciones de objetos a los métodos de extensión de registrador para escenarios de registro de alto rendimiento."
ms.author: riande
manager: wpickett
ms.date: 11/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: defba75c6c9ea13d24af4cd8515d82d9e7cf9853
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a>Registro de alto rendimiento con LoggerMessage en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) características crean delegados almacenable en caché que requieren menos las asignaciones de objetos y reducción la sobrecarga computacional que [métodos de extensión de registrador](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), como `LogInformation`, `LogDebug`y `LogError`. Para escenarios de registro de alto rendimiento, use la `LoggerMessage` patrón.

`LoggerMessage`proporciona las siguientes ventajas de rendimiento sobre los métodos de extensión de registrador:

* Métodos de extensión de registrador requieren tipos de valor de "boxing" (conversión), como `int`, en `object`. El `LoggerMessage` patrón evita la conversión boxing utilizando estático `Action` campos y métodos de extensión con parámetros fuertemente tipados.
* Métodos de extensión de registrador deben analizar la plantilla de mensaje (cadena de formato con nombre) cada vez que se escribe un mensaje de registro. `LoggerMessage`solo necesita analizar una plantilla una vez cuando se define el mensaje.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

La aplicación de ejemplo muestra `LoggerMessage` características con una oferta básica de sistema de seguimiento. La aplicación agrega y elimina las comillas con una base de datos en memoria. Cuando se produzcan estas operaciones, se generan mensajes de registro mediante el `LoggerMessage` patrón.

## <a name="loggermessagedefine"></a>LoggerMessage.Define

[Definir (LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) crea un `Action` delegar para registrar un mensaje. `Define`las sobrecargas permiten pasar hasta seis parámetros de tipo a una cadena de formato con nombre (plantilla).

## <a name="loggermessagedefinescope"></a>LoggerMessage.DefineScope

[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) crea un `Func` delegar para definir un [registro ámbito](xref:fundamentals/logging/index#log-scopes). `DefineScope`las sobrecargas permiten pasar hasta tres parámetros de tipo a una cadena de formato con nombre (plantilla).

## <a name="message-template-named-format-string"></a>Plantilla de mensaje (denominada la cadena de formato)

La cadena proporcionada para la `Define` y `DefineScope` métodos es una plantilla y no una cadena interpolada. Los marcadores de posición se rellenan en el orden en que se especifican los tipos. Nombres de marcador de posición en la plantilla deben ser descriptivos y coherente en plantillas. Sirven como nombres de propiedad dentro de los datos estructurados de registro. Se recomienda [Pascal de mayúsculas y minúsculas](/dotnet/standard/design-guidelines/capitalization-conventions) para los nombres de marcador de posición. Por ejemplo, `{Count}`, `{FirstName}`.

## <a name="implementing-loggermessagedefine"></a>Implementar LoggerMessage.Define

Cada mensaje de registro es un `Action` mantenidos en un campo estático creado por `LoggerMessage.Define`. Por ejemplo, la aplicación de ejemplo crea un campo para describir un mensaje de registro para una solicitud GET para la página de índice (*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

Para el `Action`, especifique:

* El nivel de registro.
* Un identificador de evento único ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) con el nombre del método de extensión estática.
* La plantilla de mensaje (denominada la cadena de formato). 

Una solicitud para la página de índice de la aplicación de ejemplo establece el:

* Nivel de registro `Information`.
* Id. de evento a `1` con el nombre de la `IndexPageRequested` método.
* Plantilla de mensaje (denominada la cadena de formato) en una cadena.

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

Almacenes de registro estructurado pueden utilizar el nombre del evento se suministra con el Id. de evento para enriquecer el registro. Por ejemplo, [Serilog](https://github.com/serilog/serilog-extensions-logging) usa el nombre del evento.

El `Action` se invoca a través de un método de extensión fuertemente tipada. El `IndexPageRequested` método registra un mensaje para una solicitud de obtención de la página de índice en la aplicación de ejemplo:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

`IndexPageRequested`se llama en el registrador en el `OnGetAsync` método *Pages/Index.cshtml.cs*:

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

Inspeccionar la salida de la consola de la aplicación:

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

Para pasar parámetros a un mensaje de registro, definir hasta seis tipos al crear el campo estático. La aplicación de ejemplo registra una cadena cuando se agrega un carácter de comilla mediante la definición de un `string` escriba para la `Action` campo:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

Plantilla de mensaje de registro del delegado recibe sus valores de marcador de posición de los tipos proporcionados. La aplicación de ejemplo define un delegado para agregar una oferta donde el parámetro de la oferta es un `string`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

El método de extensión estática para agregar una oferta, `QuoteAdded`, recibe el valor del argumento de oferta y lo pasa a la `Action` delegado:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

En el archivo de código subyacente de la página de índice (*Pages/Index.cshtml.cs*), `QuoteAdded` se llama para registrar el mensaje:

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

Inspeccionar la salida de la consola de la aplicación:

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

La aplicación de ejemplo implementa un `try` &ndash; `catch` patrón para su eliminación de la oferta. Se registra un mensaje informativo para una operación de eliminación correcta. Un mensaje de error se registra para una operación de eliminación cuando se produce una excepción. El mensaje del registro para el error de eliminación de operación incluye el seguimiento de pila de excepción (*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

Observe cómo la excepción se pasa al delegado en `QuoteDeleteFailed`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

En el código de página de índice-subyacente, llama a una operación de eliminación de la oferta correcta el `QuoteDeleted` método en el registrador. Cuando no se encuentra un carácter de comilla para su eliminación, un `ArgumentNullException` se produce. La excepción es capturada por el `try` &ndash; `catch` instrucción y registra mediante una llamada a la `QuoteDeleteFailed` método en el registrador en el `catch` bloque (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

Cuando se elimina correctamente una oferta, estudie la salida de la consola de la aplicación:

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

Cuando se produce un error en la eliminación de la oferta, inspeccionar la salida de la consola de la aplicación. Tenga en cuenta que la excepción se incluye en el mensaje del registro:

```console
fail: LoggerMessageSample.Pages.IndexModel[5]
      => RequestId:0HL90M6E7PHK5:00000010 RequestPath:/ => /Index
      Quote delete failed (Id = 999)
System.ArgumentNullException: Value cannot be null.
Parameter name: entity
   at Microsoft.EntityFrameworkCore.Utilities.Check.NotNull[T](T value, String parameterName)
   at Microsoft.EntityFrameworkCore.DbContext.Remove[TEntity](TEntity entity)
   at Microsoft.EntityFrameworkCore.Internal.InternalDbSet`1.Remove(TEntity entity)
   at LoggerMessageSample.Pages.IndexModel.<OnPostDeleteQuoteAsync>d__14.MoveNext() in 
      <PATH>\sample\Pages\Index.cshtml.cs:line 87
```

## <a name="implementing-loggermessagedefinescope"></a>Implementar LoggerMessage.DefineScope

Definir una [registro ámbito](xref:fundamentals/logging/index#log-scopes) para aplicar a una serie de mensajes de registro mediante el [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) método.

La aplicación de ejemplo tiene un **Borrar todo** botón para eliminar todas las comillas en la base de datos. Las comillas se eliminan al quitar uno a la vez. Cada vez que se elimina una oferta, el `QuoteDeleted` método se llama en el registrador. Un ámbito de registro se agrega a estos mensajes de registro.

Habilitar `IncludeScopes` en las opciones de registrador de consola:

[!code-csharp[Main](loggermessage/sample/Program.cs?name=snippet1&highlight=22)]

Establecer `IncludeScopes` necesario en las aplicaciones de ASP.NET Core 2.0 para habilitar el registro ámbitos. Establecer `IncludeScopes` a través de *appsettings* archivos de configuración es una característica que ha planeado para la versión 2.1 de núcleo de ASP.NET.

La aplicación de ejemplo borra otros proveedores y agrega filtros para reducir la salida del registro. Así resulta más fácil ver los mensajes de registro de ejemplo que muestran `LoggerMessage` características.

Para crear un ámbito de registro, agregue un campo que contenga un `Func` delegado para el ámbito. La aplicación de ejemplo crea un campo denominado `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

Use `DefineScope` para crear el delegado. Hasta tres tipos pueden especificarse para su uso como argumentos de plantilla cuando se invoca el delegado. La aplicación de ejemplo usa una plantilla de mensaje que incluye el número de comillas eliminados (un `int` tipo):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

Proporciona un método de extensión estática para el mensaje del registro. Incluir parámetros de tipo para propiedades con nombre que aparecen en la plantilla de mensaje. La aplicación de ejemplo se toma en un `count` de presupuesto que desea eliminar y devuelve `_allQuotesDeletedScope`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

Los ajustes de ámbito llama a la extensión de registro de un `using` bloque:

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

Inspeccionar los mensajes de registro de salida de la consola de la aplicación. El resultado siguiente muestra tres comillas eliminadas con el mensaje de ámbito del registro incluido:

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 1' Id = 2)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 2' Id = 3)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 3' Id = 4)
```

## <a name="see-also"></a>Vea también

* [Registro](xref:fundamentals/logging/index)
