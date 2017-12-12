---
title: Detectar cambios con tokens de cambio en ASP.NET Core
author: guardrex
description: "Obtenga información acerca de cómo usar tokens de cambio para realizar un seguimiento de cambios."
manager: wpickett
ms.author: riande
ms.date: 11/10/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/primitives/change-tokens
ms.openlocfilehash: a9479e3d676ed4dc880996a4a77de30d82b84cd5
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2017
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>Detectar cambios con tokens de cambio en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

A *cambiar símbolo (token)* es un bloque de creación de uso general, bajo nivel utilizado para realizar el seguimiento de cambios.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>Interfaz IChangeToken

[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propaga las notificaciones que se ha producido un cambio. `IChangeToken`reside en el [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) espacio de nombres. Para las aplicaciones que no usan el [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, referencia de la [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) paquete de NuGet en el archivo de proyecto.

`IChangeToken`tiene dos propiedades:

* [ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) indicar si el token de forma proactiva genera las devoluciones de llamada. Si `ActiveChangedCallbacks` está establecido en `false`, nunca se llama a una devolución de llamada y la aplicación debe sondear `HasChanged` cambios. También es posible para un símbolo (token) nunca Cancelar si no se producen cambios o se elimina o deshabilita el agente de escucha de cambio subyacente.
* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) Obtiene un valor que indica si se ha producido un cambio.

La interfaz tiene un método, [RegisterChangeCallback (acción&lt;objeto&gt;, objeto)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), que registra una devolución de llamada que se invoca cuando ha cambiado el token. `HasChanged`se debe establecer antes de que se invoca la devolución de llamada.

## <a name="changetoken-class"></a>Clase ChangeToken

`ChangeToken`es una clase estática que se utiliza para propagar las notificaciones que se ha producido un cambio. `ChangeToken`reside en el [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) espacio de nombres. Para las aplicaciones que no usan el [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, referencia de la [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) paquete de NuGet en el archivo de proyecto.

El `ChangeToken` [OnChange (Func&lt;IChangeToken&gt;, acción)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) método registra una `Action` para llamar a cada vez que cambie el símbolo (token):
* `Func<IChangeToken>`genera el token.
* `Action`se llama cuando cambia el token.

`ChangeToken`tiene una [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, acción&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) sobrecarga que toma un adicionales`TState`parámetro que se pasa en el consumidor token `Action`.

`OnChange`Devuelve un [IDisposable](/dotnet/api/system.idisposable). Al llamar a [Dispose](/dotnet/api/system.idisposable.dispose) detiene el token de escucha para futuras modificaciones y libera los recursos del token.

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>Ejemplos de uso de tokens de cambio en ASP.NET Core

Cambiar símbolos (tokens) se utiliza en las áreas principales de ASP.NET Core cambios en los objetos de supervisión:

* Para supervisar los cambios a los archivos, [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)del [inspección](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) método crea un `IChangeToken` para los archivos especificados o una carpeta para supervisarla.
* `IChangeToken`símbolos (tokens) se puede agregar a las entradas de caché para desencadenar expulsiones de la caché en el cambio.
* Para `TOptions` cambia, el valor predeterminado [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) implementación de [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) tiene una sobrecarga que acepta uno o varios [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1)instancias. Cada instancia devuelve un `IChangeToken` para registrar una devolución de llamada de notificación de cambio para opciones de seguimiento de cambios.

## <a name="monitoring-for-configuration-changes"></a>Supervisión de los cambios de configuración

De forma predeterminada, las plantillas de ASP.NET Core usar [archivos de configuración de JSON](xref:fundamentals/configuration/index#json-configuration) (*appSettings.JSON que se*, *appsettings. Development.JSON*, y *appsettings. Production.JSON*) para cargar valores de configuración de aplicación.

Estos archivos se configuran utilizando la [AddJsonFile (IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) método de extensión [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) que acepta un `reloadOnChange` parámetro (ASP.NET Core 1.1 y versiones posterior). `reloadOnChange`indica si se debe recargar la configuración en los cambios de archivo. Vea esta configuración en el [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) método conveniencia [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ([origen de referencia](https://github.com/aspnet/MetaPackages/blob/rel/2.0.3/src/Microsoft.AspNetCore/WebHost.cs#L152-L193)):

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

Configuración basada en archivo representado por [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource). `FileConfigurationSource`usa [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) ([origen de referencia](https://github.com/aspnet/FileSystem/blob/patch/2.0.1/src/Microsoft.Extensions.FileProviders.Abstractions/IFileProvider.cs)) para supervisar los archivos.

De forma predeterminada, el `IFileMonitor` proporcionada por un [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) ([origen de referencia](https://github.com/aspnet/Configuration/blob/patch/2.0.1/src/Microsoft.Extensions.Configuration.FileExtensions/FileConfigurationSource.cs#L82)), que usa [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) para supervisar el archivo de configuración cambios.

La aplicación de ejemplo muestra dos implementaciones para supervisar los cambios de configuración. Si el *appSettings.JSON que se* cambios en los archivos o el entorno de la versión del archivo cambia, cada implementación ejecuta código personalizado. La aplicación de ejemplo escribe un mensaje en la consola.

Un archivo de configuración `FileSystemWatcher` puede desencadenar varias devoluciones de llamada de símbolo (token) de un cambio de archivo de configuración único. Implementación del ejemplo protege contra este problema mediante la comprobación de hash de archivo en los archivos de configuración. La comprobación de hash de archivo, se garantiza que al menos uno de los archivos de configuración ha cambiado antes de ejecutar el código personalizado. El ejemplo usa hash de archivo SHA1 (*Utilities/Utilities.cs*):

   [!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   Un reintento se implementa con un retroceso exponencial. Intente volver a está presente porque el bloqueo de archivos puede producir que impide temporalmente que calcular un hash nuevo en uno de los archivos.

### <a name="simple-startup-change-token"></a>Token de cambio de inicio simple

Registrar un consumidor token `Action` devolución de llamada para las notificaciones de cambio para el token de recarga de configuración (*Startup.cs*):

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet2)]

`config.GetReloadToken()`proporciona el token. La devolución de llamada es la `InvokeChanged` método:

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet3)]

El `state` de la devolución de llamada se usa para pasar el `IHostingEnvironment`. Esto es útil para determinar el valor correcto *appsettings* archivo de configuración JSON para supervisar, *appsettings.&lt; Entorno&gt;.json*. Hash de archivo se usan para evitar la `WriteConsole` instrucción ejecute varias veces debido a varias devoluciones de llamada de símbolo (token) cuando el archivo de configuración solo cambia una vez.

Este sistema se ejecuta siempre y cuando la aplicación se está ejecutando y no se puede deshabilitar el usuario.

### <a name="monitoring-configuration-changes-as-a-service"></a>Supervisión de los cambios de configuración como un servicio

El ejemplo implementa:

* Supervisión del inicio básica del token.
* Supervisión como un servicio.
* Un mecanismo para habilitar y deshabilitar la supervisión.

El ejemplo establece un `IConfigurationMonitor` interfaz (*Extensions/ConfigurationMonitor.cs*):

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

El constructor de la clase implementada, `ConfigurationMonitor`, registra una devolución de llamada para las notificaciones de cambio:

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()`proporciona el token. `InvokeChanged`es el método de devolución de llamada. El `state` en este caso es una cadena que describe el estado de supervisión. Se utilizan dos propiedades:

* `MonitoringEnabled`indica si la devolución de llamada debe ejecutar su código personalizado.
* `CurrentState`Describe el estado de supervisión actual para su uso en la interfaz de usuario.

El `InvokeChanged` método es similar al enfoque anterior, excepto en que:

* No se ejecuta el código, a menos que `MonitoringEnabled` es `true`.
* Establece el `CurrentState` cadena de propiedad para un mensaje descriptivo que registra el tiempo que el código se ejecuta.
* Notas de la actual `state` en su `WriteConsole` salida.

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

Una instancia `ConfigurationMonitor` está registrado como un servicio en `ConfigureServices` de *Startup.cs*:

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet1)]

La página de índice ofrece el control de usuario sobre la supervisión de la configuración. La instancia de `IConfigurationMonitor` insertado en el `IndexModel`:

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

Un botón se habilita y deshabilita la supervisión:

[!code-cshtml[Main](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

Cuando `OnPostStartMonitoring` está activa, la supervisión está habilitada y se borra el estado actual. Cuando `OnPostStopMonitoring` está activa, la supervisión está deshabilitada y el estado se establece para reflejar que la supervisión no se produce.

## <a name="monitoring-cached-file-changes"></a>Supervisión de los cambios de archivo almacenado en caché

Contenido del archivo puede estar almacenado en caché en memoria con [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache). Almacenamiento en caché en memoria se describe en el [almacenamiento en caché en memoria](xref:performance/caching/memory) tema. Sin realizar pasos adicionales, como la implementación se describe a continuación, *obsoletos* (no actualizadas) de datos se devuelven desde una memoria caché si cambian los datos de origen.

No teniendo en cuenta el estado de un archivo de origen almacenados en memoria caché cuando se renueva un [la expiración variable](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) período conduce a caché datos obsoletos. El período de expiración deslizante renueva cada solicitud para los datos, pero nunca se vuelve a cargar el archivo en la memoria caché. Las características de aplicaciones que usan el contenido del archivo en caché están sujetos a posiblemente recibir contenido obsoleto.

Uso de tokens de cambio en un archivo de almacenamiento en caché escenario evita contenido del archivo obsoletos en la memoria caché. La aplicación de ejemplo muestra una implementación del enfoque.

El ejemplo usa `GetFileContent` para:

* Devuelve el contenido del archivo.
* Implementar un algoritmo de reintento con retroceso exponencial para casos de portada donde un bloqueo de archivo temporalmente impide que un archivo que se va a leer.

*Utilities/Utilities.cs*:

[!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

Un `FileService` se crea para administrar las búsquedas de archivos almacenados en caché. El `GetFileContent` llamada al método del servicio intenta obtener el contenido del archivo de la memoria caché en memoria y devolver al llamador (*Services/FileService.cs*).

Si el contenido almacenado en caché no se encuentra mediante la clave de caché, se realizan las acciones siguientes:

1. El contenido del archivo se obtiene mediante `GetFileContent`.
1. Se obtiene un token de cambio desde el proveedor de archivos con [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch). Devolución de llamada del token se desencadena cuando se modifica el archivo.
1. Se almacena en caché el contenido del archivo con un [la expiración variable](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) período. El token de cambio está asociado con [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) para expulsar la entrada de caché si el archivo cambia mientras se almacena en caché.

[!code-csharp[Main](change-tokens/sample/Services/FileService.cs?name=snippet1)]

El `FileService` está registrado en el contenedor de servicio junto con el servicio de caché de memoria (*Startup.cs*):

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet4)]

El modelo de página carga el contenido del archivo con el servicio (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>Clase CompositeChangeToken

Para representar uno o varios `IChangeToken` instancias en un solo objeto, use la [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) clase ([origen de referencia](https://github.com/aspnet/Common/blob/patch/2.0.1/src/Microsoft.Extensions.Primitives/CompositeChangeToken.cs)).

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

`HasChanged`en los informes de token compuestos `true` si cualquier token que representa `HasChanged` es `true`. `ActiveChangeCallbacks`en los informes de token compuestos `true` si cualquier token que representa `ActiveChangeCallbacks` es `true`. Si se producen varios eventos de cambio simultáneas, la devolución de llamada de cambio compuesto se invoca exactamente una vez.

## <a name="see-also"></a>Vea también

* [Almacenamiento en caché en memoria](xref:performance/caching/memory)
* [Trabajar con una memoria caché distribuida](xref:performance/caching/distributed)
* [Detectar cambios con tokens de cambio](xref:fundamentals/primitives/change-tokens)
* [Almacenamiento en caché de respuestas](xref:performance/caching/response)
* [Middleware de almacenamiento en caché de respuestas](xref:performance/caching/middleware)
* [Aplicación auxiliar de etiqueta de caché](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Aplicación auxiliar de etiqueta de caché distribuida](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
