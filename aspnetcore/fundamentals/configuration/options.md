---
title: "Patrón de opciones en ASP.NET Core"
author: guardrex
description: "Descubra cómo usar el patrón de opciones para representar grupos de configuraciones relacionadas en las aplicaciones de ASP.NET Core."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 11/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/options
ms.openlocfilehash: aab96b5313a8632950e51f5586612c1d0d3d176e
ms.sourcegitcommit: 83b5a4715fd25e4eb6f7c8427c0ef03850a7fa07
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2018
---
# <a name="options-pattern-in-aspnet-core"></a>Patrón de opciones en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

El patrón de opciones usa las clases de opciones para representar grupos de configuraciones relacionadas. Cuando valores de configuración están aislados por característica en clases de opciones independientes, la aplicación se ajusta a dos principios de ingeniería de software importantes:

* El [principio de segregación de interfaz (ISP)](http://deviq.com/interface-segregation-principle/): características (clases) que dependen de valores de configuración dependen únicamente de los valores de configuración que utilizan.
* [Separación de intereses](http://deviq.com/separation-of-concerns/): configuración para las distintas partes de la aplicación no es dependiente o acoplamiento entre sí.

[Ver o descargar el código de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample)) en este artículo es más fácil de seguir con la aplicación de ejemplo.

## <a name="basic-options-configuration"></a>Configuración de opciones básicas

Configuración de las opciones básicas se muestra como ejemplo &num;1 en el [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Una clase de opciones debe ser no abstracto con un constructor público sin parámetros. La siguiente clase, `MyOptions`, tiene dos propiedades, `Option1` y `Option2`. Valores predeterminados de configuración es opcional, pero el constructor de clase en el ejemplo siguiente establece el valor predeterminado de `Option1`. `Option2`tiene un valor predeterminado establecido por Inicializando la propiedad directamente (*Models/MyOptions.cs*):

[!code-csharp[Main](options/sample/Models/MyOptions.cs?name=snippet1)]

El `MyOptions` clase se agrega al contenedor de servicios con [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) y enlazado a la configuración:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example1)]

La siguiente página utiliza modelo [inyección de dependencia de constructor](xref:fundamentals/dependency-injection#what-is-dependency-injection) con [IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) para tener acceso a la configuración (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

El ejemplo *appSettings.JSON que se* archivo especifica valores para `option1` y `option2`:

[!code-json[Main](options/sample/appsettings.json)]

Cuando se ejecuta la aplicación, el modelo de páginas `OnGet` método devuelve una cadena que muestra los valores de la clase de opción:

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a>Configurar opciones simples con un delegado

Configuración de las opciones simples con un delegado se muestra como ejemplo &num;2 en el [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Use un delegado para establecer los valores de opciones. La aplicación de ejemplo se usa el `MyOptionsWithDelegateConfig` clase (*Models/MyOptionsWithDelegateConfig.cs*):

[!code-csharp[Main](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

En el código siguiente, un segundo `IConfigureOptions<TOptions>` servicio se agrega al contenedor de servicios. Usa un delegado para configurar el enlace con `MyOptionsWithDelegateConfig`:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*:

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

Puede agregar varios proveedores de configuración. Proveedores de configuración están disponibles en paquetes de NuGet. Se aplican para que se registran.

Cada llamada a [configurar&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) agrega un `IConfigureOptions<TOptions>` servicio al contenedor de servicios. En el ejemplo anterior, los valores de `Option1` y `Option2` se especifican en *appSettings.JSON que se*, pero los valores de `Option1` y `Option2` se reemplazan por el delegado configurado.

Cuando se habilita más de un servicio de configuración, la última fuente de configuración especificado *wins* y establece el valor de configuración. Cuando se ejecuta la aplicación, el modelo de páginas `OnGet` método devuelve una cadena que muestra los valores de la clase de opción:

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>Configuración de subopciones

Configuración de subopciones se muestra como ejemplo &num;3 en el [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Las aplicaciones deben crear clases de opciones que pertenecen a grupos específicos de característica (clases) en la aplicación. Partes de la aplicación que requieren valores de configuración solamente deben tener acceso a los valores de configuración que utilizan.

Al enlazar opciones de configuración, cada propiedad en el tipo de opciones está enlazado a una clave de configuración del formulario `property[:sub-property:]`. Por ejemplo, el `MyOptions.Option1` propiedad se enlaza a la clave `Option1`, que se lee desde el `option1` propiedad en *appSettings.JSON que se*.

En el código siguiente, una tercera `IConfigureOptions<TOptions>` servicio se agrega al contenedor de servicios. Enlaza `MySubOptions` a la sección `subsection` de la *appSettings.JSON que se* archivo:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example3)]

El `GetSection` método de extensión requiere la [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) paquete NuGet. Si la aplicación usa el [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, el paquete se incluye automáticamente.

El ejemplo *appSettings.JSON que se* archivo define un `subsection` miembro con las claves de `suboption1` y `suboption2`:

[!code-json[Main](options/sample/appsettings.json?highlight=4-7)]

El `MySubOptions` clase define propiedades, `SubOption1` y `SubOption2`, para mantener los valores de opción secundario (*Models/MySubOptions.cs*):

[!code-csharp[Main](options/sample/Models/MySubOptions.cs?name=snippet1)]

El modelo de páginas `OnGet` método devuelve una cadena con los valores de opción secundario (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

Cuando se ejecuta la aplicación, el `OnGet` método devuelve una cadena que muestra la opción subcarpetas valores de clase:

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>Opciones proporcionadas por un modelo de vista o de inyección de vista directa

Opciones proporcionadas por un modelo de vista o de inserción directa de vista se muestra como ejemplo &num;4 en el [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Opciones pueden especificarse en un modelo de vista o insertando `IOptions<TOptions>` directamente en una vista (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

Para que la inserción directa, insertar `IOptions<MyOptions>` con un `@inject` directiva:

[!code-cshtml[Main](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

Cuando se ejecuta la aplicación, se muestran los valores de opción en la página presentada:

![Opciones de valores de opción 1: value1_from_json y opción2: -1 se cargan desde el modelo y por inyección de código en la vista.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>Volver a cargar los datos de configuración con IOptionsSnapshot

Volver a cargar los datos de configuración con `IOptionsSnapshot` se muestra en el ejemplo &num;5 en el [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*Requiere ASP.NET Core 1.1 o posterior.*

[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) admite recargar opciones con la mínima sobrecarga de procesamiento. En ASP.NET Core 1.1, `IOptionsSnapshot` es una instantánea de [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) y las actualizaciones automáticamente cada vez que el monitor desencadena cambia de acuerdo con los cambios del origen de datos. En el núcleo de ASP.NET 2.0 y versiones posteriores, opciones se calculan una vez por solicitud cuando se tiene acceso y almacenar en caché para la duración de la solicitud.

En el ejemplo siguiente se muestra cómo un nuevo `IOptionsSnapshot` se crea después *appSettings.JSON que se* cambios (*Pages/Index.cshtml.cs*). Varias solicitudes al servidor devuelven valores constantes proporcionados por el *appSettings.JSON que se* hasta que se modifique el archivo y configuración recarga de archivos.

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

La siguiente imagen muestra la inicial `option1` y `option2` valores cargan desde el *appSettings.JSON que se* archivo:

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

Cambiar los valores de la *appSettings.JSON que se* del archivo a `value1_from_json UPDATED` y `200`. Guardar el *appSettings.JSON que se* archivo. Actualice el explorador para ver que se actualizan los valores de opciones:

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a>Con el nombre de opciones de la compatibilidad con IConfigureNamedOptions

Con el nombre de opciones de la compatibilidad con [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) se muestra como ejemplo &num;6 en el [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*Requiere el núcleo ASP.NET 2.0 o posterior.*

*Denominado opciones* compatibilidad permite a la aplicación distinguir entre las configuraciones de opciones con nombre. En la aplicación de ejemplo, las opciones con nombre se declaran con la [ConfigureNamedOptions&lt;TOptions&gt;. Configurar](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) método:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example6)]

La aplicación de ejemplo tiene acceso a las opciones con nombre con [IOptionsSnapshot&lt;TOptions&gt;. Obtener](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

Ejecuta la aplicación de ejemplo, se devuelven las opciones con nombre:

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

`named_options_1`se proporcionan valores de configuración, que se cargan desde el *appSettings.JSON que se* archivo. `named_options_2`valores proceden de:

* El `named_options_2` delegar en `ConfigureServices` para `Option1`.
* El valor predeterminado de `Option2` proporcionada por el `MyOptions` clase.

Configure todas las instancias con nombre opciones con el [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) método. El siguiente código configura `Option1` para todas las configuración las instancias con nombre con un valor común. Agregue el código siguiente manualmente a la `Configure` método:

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

Ejecuta la aplicación de ejemplo después de agregar el código genera el siguiente resultado:

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> En el núcleo de ASP.NET 2.0 y versiones posteriores, todas las opciones son instancias con nombre. Existente `IConfigureOption` instancias se tratan como destino el `Options.DefaultName` instancia, que es `string.Empty`. `IConfigureNamedOptions`También implementa `IConfigureOptions`. La implementación predeterminada de la [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([origen de referencia](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) tiene una lógica para usar cada una de forma adecuada. El `null` opción con nombre se utiliza para todas las instancias con nombre en lugar de un determinado con el nombre de instancia de destino ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) y [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) usan esta convención).

## <a name="ipostconfigureoptions"></a>IPostConfigureOptions

*Requiere el núcleo ASP.NET 2.0 o posterior.*

Establecer postconfiguration con [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1). Postconfiguración se ejecuta después de que todos [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) se produce la configuración:

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) está disponible para posteriores al configurar las opciones con nombre:

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

Use [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) posteriores al configurar todos configuración las instancias con nombre:

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a>Generador de opciones, la supervisión y la memoria caché

[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) se usa para notificaciones cuando `TOptions` instancias de cambio. `IOptionsMonitor`admite opciones recargable, notificaciones de cambios, y `IPostConfigureOptions`.

[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (núcleo ASP.NET 2.0 o posterior) es responsable de crear nuevas opciones de instancias. Tiene una sola [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) método. La implementación predeterminada tiene todos los `IConfigureOptions` y `IPostConfigureOptions` y ejecuta todos los configura primero, a continuación, la configura posteriores a la. Distingue entre `IConfigureNamedOptions` y `IConfigureOptions` y solo se llama a la interfaz adecuada.

[IOptionsMonitorCache&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (núcleo ASP.NET 2.0 o posterior) se usa por `IOptionsMonitor` en caché `TOptions` instancias. El `IOptionsMonitorCache` invalida instancias de opciones en el monitor de modo que se vuelve a calcular el valor ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)). Los valores pueden ser manualmente presentar también con [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd). El [desactive](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) método se utiliza cuando todas las instancias con nombre se deben volver a crearse a petición.

## <a name="accessing-options-during-startup"></a>Obtener acceso a opciones durante el inicio

`IOptions`puede utilizarse en `Configure`, ya que los servicios están integrados antes de la `Configure` el método se ejecuta. Si se crea un proveedor de servicio `ConfigureServices` para tener acceso a opciones, que no contiene ninguna opciones de las configuraciones que se proporciona después de que se compila el proveedor de servicios. Por lo tanto, puede que exista un estado incoherente opciones debido al orden de los registros de servicio.

Puesto que normalmente se cargan opciones de configuración, se puede usar la configuración de inicio en ambas `Configure` y `ConfigureServices`. Para obtener ejemplos del uso de configuración durante el inicio, consulte el [inicio de la aplicación](xref:fundamentals/startup) tema.

## <a name="see-also"></a>Vea también

* [Configuración](xref:fundamentals/configuration/index)
