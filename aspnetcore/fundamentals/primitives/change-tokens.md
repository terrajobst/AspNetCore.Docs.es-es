---
title: Detección de cambios con tokens de cambio en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo usar tokens de cambio para realizar el seguimiento de los cambios.
manager: wpickett
ms.author: riande
ms.date: 11/10/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/primitives/change-tokens
ms.openlocfilehash: 06751e713fbd579a944333cc3c3b2c0c0ad51eba
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2018
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>Detección de cambios con tokens de cambio en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Un *token de cambio* es un bloque de creación de bajo nivel y uso general que se usa para realizar el seguimiento de los cambios.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>Interfaz IChangeToken

[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propaga notificaciones que indican que se ha producido un cambio. `IChangeToken` reside en el espacio de nombres [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives). Para las aplicaciones que no usan el metapaquete [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/), haga referencia al paquete NuGet [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) en el archivo de proyecto.

`IChangeToken` tiene dos propiedades:

* [ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) indica si el token genera devoluciones de llamada de forma proactiva. Si `ActiveChangedCallbacks` se establece en `false`, nunca se llama a una devolución de llamada y la aplicación debe sondear `HasChanged` en busca de cambios. También es posible que un token nunca se cancele si no se producen cambios o si se elimina o deshabilita el agente de escucha de cambios subyacente.
* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) obtiene un valor que indica si se ha producido un cambio.

La interfaz tiene un método, [RegisterChangeCallback(Acción&lt;Objeto&gt;, Objeto)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), que registra una devolución de llamada que se invoca cuando el token ha cambiado. `HasChanged` se debe establecer antes de que se invoque la devolución de llamada.

## <a name="changetoken-class"></a>Clase ChangeToken

`ChangeToken` es una clase estática que se usa para propagar notificaciones que indican que se ha producido un cambio. `ChangeToken` reside en el espacio de nombres [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives). Para las aplicaciones que no usan el metapaquete [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/), haga referencia al paquete NuGet [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) en el archivo de proyecto.

El método [OnChange(Función&lt;IChangeToken&gt;, Acción)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) de `ChangeToken` registra una `Action` que se llama cada vez que cambia el token:
* `Func<IChangeToken>` genera el token.
* Se llama a `Action` cuando cambia el token.

`ChangeToken` tiene una sobrecarga [OnChange&lt;TState&gt;(Función&lt;IChangeToken&gt;, Acción&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) que toma un parámetro `TState` adicional que se pasa a la `Action` de consumidor de token.

`OnChange` devuelve una interfaz [IDisposable](/dotnet/api/system.idisposable). Al llamar a [Dispose](/dotnet/api/system.idisposable.dispose) se detiene la escucha del token de futuras modificaciones y se liberan sus recursos.

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>Ejemplos de uso de tokens de cambio en ASP.NET Core

Los tokens de cambio se usan en áreas principales de ASP.NET Core para la supervisión de cambios en los objetos:

* Para supervisar los cambios en los archivos, el método [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) de [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) crea un `IChangeToken` para los archivos especificados o la carpeta que se va a supervisar.
* Se pueden agregar tokens `IChangeToken` a las entradas de caché para desencadenar expulsiones de caché al producirse un cambio.
* Para los cambios de `TOptions`, la implementación predeterminada [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) de [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) tiene una sobrecarga que acepta una o varias instancias de [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1). Cada instancia devuelve un `IChangeToken` para registrar una devolución de llamada de notificación de cambio a fin de realizar el seguimiento de los cambios en las opciones.

## <a name="monitoring-for-configuration-changes"></a>Supervisión de los cambios de configuración

De forma predeterminada, las plantillas de ASP.NET Core usan [archivos de configuración de JSON](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*, *appsettings.Development.json* y *appsettings.Production.json*) para cargar parámetros de configuración de la aplicación.

Estos archivos se configuran mediante el método de extensión [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) de [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder), que acepta un parámetro `reloadOnChange` (ASP.NET Core 1.1 y versiones posteriores). `reloadOnChange` indica si la configuración se debe recargar en los cambios de archivo. Vea esta configuración en el método de conveniencia [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) de [WebHost](/dotnet/api/microsoft.aspnetcore.webhost):

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

La configuración basada en archivo se representa por medio de [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource). `FileConfigurationSource` usa [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) para supervisar los archivos.

[PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) proporciona de forma predeterminada `IFileMonitor`, que usa [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) para supervisar los cambios del archivo de configuración.

En la aplicación de ejemplo se muestran dos implementaciones para supervisar los cambios de configuración. Si cambia el archivo *appsettings.json* o la versión del entorno del archivo, cada implementación ejecuta código personalizado. La aplicación de ejemplo escribe un mensaje en la consola.

El `FileSystemWatcher` de un archivo de configuración puede desencadenar varias devoluciones de llamada de token para un único cambio del archivo de configuración. La implementación del ejemplo protege contra este problema mediante la comprobación del hash de archivo en los archivos de configuración. La comprobación del hash de archivo garantiza que al menos uno de los archivos de configuración ha cambiado antes de ejecutar el código personalizado. En el ejemplo se usa el hash de archivo SHA1 (*Utilities/Utilities.cs*):

   [!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   Se implementa un reintento con una interrupción exponencial. El reintento aparece porque se puede producir un bloqueo de archivos que impida temporalmente calcular un hash nuevo en uno de los archivos.

### <a name="simple-startup-change-token"></a>Token de cambio de inicio simple

Registre una devolución de llamada de `Action` de consumidor de token para las notificaciones de cambio en el token de recarga de configuración (*Startup.cs*):

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet2)]

`config.GetReloadToken()` proporciona el token. La devolución de llamada es el método `InvokeChanged`:

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet3)]

El `state` de la devolución de llamada se usa para pasar el `IHostingEnvironment`. Esto es útil para determinar el archivo JSON de configuración *appsettings* correcto que se va a supervisar, *appsettings.&lt;Entorno&gt;.json*. Se usa el hash de archivo para evitar que se ejecute varias veces la instrucción `WriteConsole` debido a varias devoluciones de llamada de token cuando el archivo de configuración solo ha cambiado una vez.

Este sistema se ejecuta siempre que la aplicación esté en ejecución y el usuario no lo puede deshabilitar.

### <a name="monitoring-configuration-changes-as-a-service"></a>Supervisión de los cambios de configuración como servicio

En el ejemplo se implementa lo siguiente:

* La supervisión del token de inicio básico.
* La supervisión como servicio.
* Un mecanismo para habilitar y deshabilitar la supervisión.

En el ejemplo se establece una interfaz `IConfigurationMonitor` (*Extensions/ConfigurationMonitor.cs*):

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

El constructor de la clase implementada, `ConfigurationMonitor`, registra una devolución de llamada para las notificaciones de cambio:

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()` proporciona el token. `InvokeChanged` es el método de devolución de llamada. El elemento `state` de esta instancia es una referencia a la instancia de `IConfigurationMonitor` que se usa para tener acceso al estado de supervisión. Se usan dos propiedades:

* `MonitoringEnabled` indica si la devolución de llamada debe ejecutar su código personalizado.
* `CurrentState` describe el estado de supervisión actual para su uso en la interfaz de usuario.

El método `InvokeChanged` es similar al enfoque anterior, excepto en que:

* No ejecuta su código, a menos que `MonitoringEnabled` sea `true`.
* Anota el `state` actual en su salida de `WriteConsole`.

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

Una instancia de `ConfigurationMonitor` se registra como servicio en `ConfigureServices` de *Startup.cs*:

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet1)]

En la página Index se ofrece al usuario el control sobre la supervisión de la configuración. La instancia de `IConfigurationMonitor` se inserta en `IndexModel`:

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

Un botón habilita y deshabilita la supervisión:

[!code-cshtml[](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

Cuando se desencadena `OnPostStartMonitoring`, se habilita la supervisión y se borra el estado actual. Cuando se desencadena `OnPostStopMonitoring`, se deshabilita la supervisión y se establece el estado para reflejar que no se está realizando la supervisión.

## <a name="monitoring-cached-file-changes"></a>Supervisión de los cambios de archivos en caché

El contenido de los archivos se puede almacenar en caché en memoria mediante [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache). El almacenamiento en caché en memoria se describe en el tema [Cache in-memory](xref:performance/caching/memory) (Almacenamiento en caché en memoria). Sin realizar pasos adicionales, como la implementación que se describe a continuación, si los datos de origen cambian, se devuelven datos *obsoletos* (no actualizados) de la caché.

Si no se tiene en cuenta el estado de un archivo de origen en caché cuando se renueva un período de [vencimiento variable](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration), se pueden crear datos en caché obsoletos. En cada solicitud de los datos se renueva el período de vencimiento variable, pero el archivo nunca se vuelve a cargar en la caché. Las características de la aplicación que usen el contenido en caché del archivo están sujetas a la posible recepción de contenido obsoleto.

El uso de tokens de cambio en un escenario de almacenamiento en caché de archivos evita contenido de archivo obsoleto en la caché. En la aplicación de ejemplo se muestra una implementación del enfoque.

En el ejemplo se usa `GetFileContent` para:

* Devolver el contenido del archivo.
* Implementar un algoritmo de reintento con interrupción exponencial para casos en los que un bloqueo de archivo impide temporalmente que se lea un archivo.

*Utilities/Utilities.cs*:

[!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

Se crea un `FileService` para administrar las búsquedas de archivos en caché. La llamada al método `GetFileContent` del servicio intenta obtener el contenido de archivo de la caché en memoria y devolverlo al autor de la llamada (*Services/FileService.cs*).

Si el contenido en caché no se encuentra mediante la clave de caché, se realizan las acciones siguientes:

1. El contenido del archivo se obtiene mediante `GetFileContent`.
1. Se obtiene un token de cambio del proveedor de archivos con [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch). La devolución de llamada del token se desencadena cuando se modifica el archivo.
1. El contenido del archivo se almacena en caché con un período de [vencimiento variable](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration). El token de cambio se adjunta con [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) para expulsar la entrada de caché si el archivo cambia mientras está almacenado en caché.

[!code-csharp[](change-tokens/sample/Services/FileService.cs?name=snippet1)]

El `FileService` se registra en el contenedor de servicios junto con el servicio de almacenamiento en caché (*Startup.cs*):

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet4)]

El modelo de página carga el contenido del archivo mediante el servicio (*Pages/Index.cshtml.cs*):

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>Clase CompositeChangeToken

Para representar una o varias instancias de `IChangeToken` en un solo objeto, use la clase [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken).

```csharp
var firstCancellationTokenSource = new CancellationTokenSource();
var secondCancellationTokenSource = new CancellationTokenSource();

var firstCancellationToken = firstCancellationTokenSource.Token;
var secondCancellationToken = secondCancellationTokenSource.Token;

var firstCancellationChangeToken = new CancellationChangeToken(firstCancellationToken);
var secondCancellationChangeToken = new CancellationChangeToken(secondCancellationToken);

var compositeChangeToken = 
    new CompositeChangeToken(
        new List<IChangeToken> 
        { 
            firstCancellationChangeToken, 
            secondCancellationChangeToken
        });
```

En el token compuesto, `HasChanged` notifica `true` si algún token representado `HasChanged` es `true`. En el token compuesto, `ActiveChangeCallbacks` notifica `true` si algún token representado `ActiveChangeCallbacks` es `true`. Si se producen varios eventos de cambio simultáneos, la devolución de llamada de cambio compuesto se invoca exactamente una vez.

## <a name="see-also"></a>Vea también

* [Almacenamiento en caché en memoria](xref:performance/caching/memory)
* [Trabajar con una memoria caché distribuida](xref:performance/caching/distributed)
* [Almacenamiento en caché de respuestas](xref:performance/caching/response)
* [Middleware de almacenamiento en caché de respuestas](xref:performance/caching/middleware)
* [Aplicación auxiliar de etiquetas de caché](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Aplicación auxiliar de etiquetas de caché distribuida](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
