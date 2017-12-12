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
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a><span data-ttu-id="eff73-103">Registro de alto rendimiento con LoggerMessage en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eff73-103">High-performance logging with LoggerMessage in ASP.NET Core</span></span>

<span data-ttu-id="eff73-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="eff73-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="eff73-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) características crean delegados almacenable en caché que requieren menos las asignaciones de objetos y reducción la sobrecarga computacional que [métodos de extensión de registrador](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), como `LogInformation`, `LogDebug`y `LogError`.</span><span class="sxs-lookup"><span data-stu-id="eff73-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) features create cacheable delegates that require fewer object allocations and reduced computational overhead than [logger extension methods](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), such as `LogInformation`, `LogDebug`, and `LogError`.</span></span> <span data-ttu-id="eff73-106">Para escenarios de registro de alto rendimiento, use la `LoggerMessage` patrón.</span><span class="sxs-lookup"><span data-stu-id="eff73-106">For high-performance logging scenarios, use the `LoggerMessage` pattern.</span></span>

<span data-ttu-id="eff73-107">`LoggerMessage`proporciona las siguientes ventajas de rendimiento sobre los métodos de extensión de registrador:</span><span class="sxs-lookup"><span data-stu-id="eff73-107">`LoggerMessage` provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="eff73-108">Métodos de extensión de registrador requieren tipos de valor de "boxing" (conversión), como `int`, en `object`.</span><span class="sxs-lookup"><span data-stu-id="eff73-108">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="eff73-109">El `LoggerMessage` patrón evita la conversión boxing utilizando estático `Action` campos y métodos de extensión con parámetros fuertemente tipados.</span><span class="sxs-lookup"><span data-stu-id="eff73-109">The `LoggerMessage` pattern avoids boxing by using static `Action` fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="eff73-110">Métodos de extensión de registrador deben analizar la plantilla de mensaje (cadena de formato con nombre) cada vez que se escribe un mensaje de registro.</span><span class="sxs-lookup"><span data-stu-id="eff73-110">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="eff73-111">`LoggerMessage`solo necesita analizar una plantilla una vez cuando se define el mensaje.</span><span class="sxs-lookup"><span data-stu-id="eff73-111">`LoggerMessage` only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="eff73-112">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="eff73-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="eff73-113">La aplicación de ejemplo muestra `LoggerMessage` características con una oferta básica de sistema de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="eff73-113">The sample app demonstrates `LoggerMessage` features with a basic quote tracking system.</span></span> <span data-ttu-id="eff73-114">La aplicación agrega y elimina las comillas con una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="eff73-114">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="eff73-115">Cuando se produzcan estas operaciones, se generan mensajes de registro mediante el `LoggerMessage` patrón.</span><span class="sxs-lookup"><span data-stu-id="eff73-115">As these operations occur, log messages are generated using the `LoggerMessage` pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="eff73-116">LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="eff73-116">LoggerMessage.Define</span></span>

<span data-ttu-id="eff73-117">[Definir (LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) crea un `Action` delegar para registrar un mensaje.</span><span class="sxs-lookup"><span data-stu-id="eff73-117">[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) creates an `Action` delegate for logging a message.</span></span> <span data-ttu-id="eff73-118">`Define`las sobrecargas permiten pasar hasta seis parámetros de tipo a una cadena de formato con nombre (plantilla).</span><span class="sxs-lookup"><span data-stu-id="eff73-118">`Define` overloads permit passing up to six type parameters to a named format string (template).</span></span>

## <a name="loggermessagedefinescope"></a><span data-ttu-id="eff73-119">LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="eff73-119">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="eff73-120">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) crea un `Func` delegar para definir un [registro ámbito](xref:fundamentals/logging/index#log-scopes).</span><span class="sxs-lookup"><span data-stu-id="eff73-120">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) creates a `Func` delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="eff73-121">`DefineScope`las sobrecargas permiten pasar hasta tres parámetros de tipo a una cadena de formato con nombre (plantilla).</span><span class="sxs-lookup"><span data-stu-id="eff73-121">`DefineScope` overloads permit passing up to three type parameters to a named format string (template).</span></span>

## <a name="message-template-named-format-string"></a><span data-ttu-id="eff73-122">Plantilla de mensaje (denominada la cadena de formato)</span><span class="sxs-lookup"><span data-stu-id="eff73-122">Message template (named format string)</span></span>

<span data-ttu-id="eff73-123">La cadena proporcionada para la `Define` y `DefineScope` métodos es una plantilla y no una cadena interpolada.</span><span class="sxs-lookup"><span data-stu-id="eff73-123">The string provided to the `Define` and `DefineScope` methods is a template and not an interpolated string.</span></span> <span data-ttu-id="eff73-124">Los marcadores de posición se rellenan en el orden en que se especifican los tipos.</span><span class="sxs-lookup"><span data-stu-id="eff73-124">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="eff73-125">Nombres de marcador de posición en la plantilla deben ser descriptivos y coherente en plantillas.</span><span class="sxs-lookup"><span data-stu-id="eff73-125">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="eff73-126">Sirven como nombres de propiedad dentro de los datos estructurados de registro.</span><span class="sxs-lookup"><span data-stu-id="eff73-126">They serve as property names within structured log data.</span></span> <span data-ttu-id="eff73-127">Se recomienda [Pascal de mayúsculas y minúsculas](/dotnet/standard/design-guidelines/capitalization-conventions) para los nombres de marcador de posición.</span><span class="sxs-lookup"><span data-stu-id="eff73-127">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="eff73-128">Por ejemplo, `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="eff73-128">For example, `{Count}`, `{FirstName}`.</span></span>

## <a name="implementing-loggermessagedefine"></a><span data-ttu-id="eff73-129">Implementar LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="eff73-129">Implementing LoggerMessage.Define</span></span>

<span data-ttu-id="eff73-130">Cada mensaje de registro es un `Action` mantenidos en un campo estático creado por `LoggerMessage.Define`.</span><span class="sxs-lookup"><span data-stu-id="eff73-130">Each log message is an `Action` held in a static field created by `LoggerMessage.Define`.</span></span> <span data-ttu-id="eff73-131">Por ejemplo, la aplicación de ejemplo crea un campo para describir un mensaje de registro para una solicitud GET para la página de índice (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="eff73-131">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="eff73-132">Para el `Action`, especifique:</span><span class="sxs-lookup"><span data-stu-id="eff73-132">For the `Action`, specify:</span></span>

* <span data-ttu-id="eff73-133">El nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="eff73-133">The log level.</span></span>
* <span data-ttu-id="eff73-134">Un identificador de evento único ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) con el nombre del método de extensión estática.</span><span class="sxs-lookup"><span data-stu-id="eff73-134">A unique event identifier ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) with the name of the static extension method.</span></span>
* <span data-ttu-id="eff73-135">La plantilla de mensaje (denominada la cadena de formato).</span><span class="sxs-lookup"><span data-stu-id="eff73-135">The message template (named format string).</span></span> 

<span data-ttu-id="eff73-136">Una solicitud para la página de índice de la aplicación de ejemplo establece el:</span><span class="sxs-lookup"><span data-stu-id="eff73-136">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="eff73-137">Nivel de registro `Information`.</span><span class="sxs-lookup"><span data-stu-id="eff73-137">Log level to `Information`.</span></span>
* <span data-ttu-id="eff73-138">Id. de evento a `1` con el nombre de la `IndexPageRequested` método.</span><span class="sxs-lookup"><span data-stu-id="eff73-138">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="eff73-139">Plantilla de mensaje (denominada la cadena de formato) en una cadena.</span><span class="sxs-lookup"><span data-stu-id="eff73-139">Message template (named format string) to a string.</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="eff73-140">Almacenes de registro estructurado pueden utilizar el nombre del evento se suministra con el Id. de evento para enriquecer el registro.</span><span class="sxs-lookup"><span data-stu-id="eff73-140">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="eff73-141">Por ejemplo, [Serilog](https://github.com/serilog/serilog-extensions-logging) usa el nombre del evento.</span><span class="sxs-lookup"><span data-stu-id="eff73-141">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="eff73-142">El `Action` se invoca a través de un método de extensión fuertemente tipada.</span><span class="sxs-lookup"><span data-stu-id="eff73-142">The `Action` is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="eff73-143">El `IndexPageRequested` método registra un mensaje para una solicitud de obtención de la página de índice en la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="eff73-143">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="eff73-144">`IndexPageRequested`se llama en el registrador en el `OnGetAsync` método *Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="eff73-144">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="eff73-145">Inspeccionar la salida de la consola de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="eff73-145">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="eff73-146">Para pasar parámetros a un mensaje de registro, definir hasta seis tipos al crear el campo estático.</span><span class="sxs-lookup"><span data-stu-id="eff73-146">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="eff73-147">La aplicación de ejemplo registra una cadena cuando se agrega un carácter de comilla mediante la definición de un `string` escriba para la `Action` campo:</span><span class="sxs-lookup"><span data-stu-id="eff73-147">The sample app logs a string when adding a quote by defining a `string` type for the `Action` field:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="eff73-148">Plantilla de mensaje de registro del delegado recibe sus valores de marcador de posición de los tipos proporcionados.</span><span class="sxs-lookup"><span data-stu-id="eff73-148">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="eff73-149">La aplicación de ejemplo define un delegado para agregar una oferta donde el parámetro de la oferta es un `string`:</span><span class="sxs-lookup"><span data-stu-id="eff73-149">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="eff73-150">El método de extensión estática para agregar una oferta, `QuoteAdded`, recibe el valor del argumento de oferta y lo pasa a la `Action` delegado:</span><span class="sxs-lookup"><span data-stu-id="eff73-150">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the `Action` delegate:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="eff73-151">En el archivo de código subyacente de la página de índice (*Pages/Index.cshtml.cs*), `QuoteAdded` se llama para registrar el mensaje:</span><span class="sxs-lookup"><span data-stu-id="eff73-151">In the Index page's code-behind file (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="eff73-152">Inspeccionar la salida de la consola de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="eff73-152">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="eff73-153">La aplicación de ejemplo implementa un `try` &ndash; `catch` patrón para su eliminación de la oferta.</span><span class="sxs-lookup"><span data-stu-id="eff73-153">The sample app implements a `try`&ndash;`catch` pattern for quote deletion.</span></span> <span data-ttu-id="eff73-154">Se registra un mensaje informativo para una operación de eliminación correcta.</span><span class="sxs-lookup"><span data-stu-id="eff73-154">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="eff73-155">Un mensaje de error se registra para una operación de eliminación cuando se produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="eff73-155">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="eff73-156">El mensaje del registro para el error de eliminación de operación incluye el seguimiento de pila de excepción (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="eff73-156">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="eff73-157">Observe cómo la excepción se pasa al delegado en `QuoteDeleteFailed`:</span><span class="sxs-lookup"><span data-stu-id="eff73-157">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="eff73-158">En el código de página de índice-subyacente, llama a una operación de eliminación de la oferta correcta el `QuoteDeleted` método en el registrador.</span><span class="sxs-lookup"><span data-stu-id="eff73-158">In the Index page code-behind, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="eff73-159">Cuando no se encuentra un carácter de comilla para su eliminación, un `ArgumentNullException` se produce.</span><span class="sxs-lookup"><span data-stu-id="eff73-159">When a quote isn't found for deletion, an `ArgumentNullException` is thrown.</span></span> <span data-ttu-id="eff73-160">La excepción es capturada por el `try` &ndash; `catch` instrucción y registra mediante una llamada a la `QuoteDeleteFailed` método en el registrador en el `catch` bloque (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="eff73-160">The exception is trapped by the `try`&ndash;`catch` statement and logged by calling the `QuoteDeleteFailed` method on the logger in the `catch` block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

<span data-ttu-id="eff73-161">Cuando se elimina correctamente una oferta, estudie la salida de la consola de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="eff73-161">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="eff73-162">Cuando se produce un error en la eliminación de la oferta, inspeccionar la salida de la consola de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eff73-162">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="eff73-163">Tenga en cuenta que la excepción se incluye en el mensaje del registro:</span><span class="sxs-lookup"><span data-stu-id="eff73-163">Note that the exception is included in the log message:</span></span>

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

## <a name="implementing-loggermessagedefinescope"></a><span data-ttu-id="eff73-164">Implementar LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="eff73-164">Implementing LoggerMessage.DefineScope</span></span>

<span data-ttu-id="eff73-165">Definir una [registro ámbito](xref:fundamentals/logging/index#log-scopes) para aplicar a una serie de mensajes de registro mediante el [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) método.</span><span class="sxs-lookup"><span data-stu-id="eff73-165">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) method.</span></span>

<span data-ttu-id="eff73-166">La aplicación de ejemplo tiene un **Borrar todo** botón para eliminar todas las comillas en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="eff73-166">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="eff73-167">Las comillas se eliminan al quitar uno a la vez.</span><span class="sxs-lookup"><span data-stu-id="eff73-167">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="eff73-168">Cada vez que se elimina una oferta, el `QuoteDeleted` método se llama en el registrador.</span><span class="sxs-lookup"><span data-stu-id="eff73-168">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="eff73-169">Un ámbito de registro se agrega a estos mensajes de registro.</span><span class="sxs-lookup"><span data-stu-id="eff73-169">A log scope is added to these log messages.</span></span>

<span data-ttu-id="eff73-170">Habilitar `IncludeScopes` en las opciones de registrador de consola:</span><span class="sxs-lookup"><span data-stu-id="eff73-170">Enable `IncludeScopes` in the console logger options:</span></span>

[!code-csharp[Main](loggermessage/sample/Program.cs?name=snippet1&highlight=22)]

<span data-ttu-id="eff73-171">Establecer `IncludeScopes` necesario en las aplicaciones de ASP.NET Core 2.0 para habilitar el registro ámbitos.</span><span class="sxs-lookup"><span data-stu-id="eff73-171">Setting `IncludeScopes` is required in ASP.NET Core 2.0 apps to enable log scopes.</span></span> <span data-ttu-id="eff73-172">Establecer `IncludeScopes` a través de *appsettings* archivos de configuración es una característica que ha planeado para la versión 2.1 de núcleo de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="eff73-172">Setting `IncludeScopes` via *appsettings* configuration files is a feature that's planned for the ASP.NET Core 2.1 release.</span></span>

<span data-ttu-id="eff73-173">La aplicación de ejemplo borra otros proveedores y agrega filtros para reducir la salida del registro.</span><span class="sxs-lookup"><span data-stu-id="eff73-173">The sample app clears other providers and adds filters to reduce the logging output.</span></span> <span data-ttu-id="eff73-174">Así resulta más fácil ver los mensajes de registro de ejemplo que muestran `LoggerMessage` características.</span><span class="sxs-lookup"><span data-stu-id="eff73-174">This makes it easier to see the sample's log messages that demonstrate `LoggerMessage` features.</span></span>

<span data-ttu-id="eff73-175">Para crear un ámbito de registro, agregue un campo que contenga un `Func` delegado para el ámbito.</span><span class="sxs-lookup"><span data-stu-id="eff73-175">To create a log scope, add a field to hold a `Func` delegate for the scope.</span></span> <span data-ttu-id="eff73-176">La aplicación de ejemplo crea un campo denominado `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="eff73-176">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="eff73-177">Use `DefineScope` para crear el delegado.</span><span class="sxs-lookup"><span data-stu-id="eff73-177">Use `DefineScope` to create the delegate.</span></span> <span data-ttu-id="eff73-178">Hasta tres tipos pueden especificarse para su uso como argumentos de plantilla cuando se invoca el delegado.</span><span class="sxs-lookup"><span data-stu-id="eff73-178">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="eff73-179">La aplicación de ejemplo usa una plantilla de mensaje que incluye el número de comillas eliminados (un `int` tipo):</span><span class="sxs-lookup"><span data-stu-id="eff73-179">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="eff73-180">Proporciona un método de extensión estática para el mensaje del registro.</span><span class="sxs-lookup"><span data-stu-id="eff73-180">Provide a static extension method for the log message.</span></span> <span data-ttu-id="eff73-181">Incluir parámetros de tipo para propiedades con nombre que aparecen en la plantilla de mensaje.</span><span class="sxs-lookup"><span data-stu-id="eff73-181">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="eff73-182">La aplicación de ejemplo se toma en un `count` de presupuesto que desea eliminar y devuelve `_allQuotesDeletedScope`:</span><span class="sxs-lookup"><span data-stu-id="eff73-182">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="eff73-183">Los ajustes de ámbito llama a la extensión de registro de un `using` bloque:</span><span class="sxs-lookup"><span data-stu-id="eff73-183">The scope wraps the logging extension calls in a `using` block:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="eff73-184">Inspeccionar los mensajes de registro de salida de la consola de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eff73-184">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="eff73-185">El resultado siguiente muestra tres comillas eliminadas con el mensaje de ámbito del registro incluido:</span><span class="sxs-lookup"><span data-stu-id="eff73-185">The following result shows three quotes deleted with the log scope message included:</span></span>

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

## <a name="see-also"></a><span data-ttu-id="eff73-186">Vea también</span><span class="sxs-lookup"><span data-stu-id="eff73-186">See also</span></span>

* [<span data-ttu-id="eff73-187">Registro</span><span class="sxs-lookup"><span data-stu-id="eff73-187">Logging</span></span>](xref:fundamentals/logging/index)
