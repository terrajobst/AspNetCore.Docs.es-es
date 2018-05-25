---
title: Patrón de opciones en ASP.NET Core
author: guardrex
description: Descubra cómo usar el patrón de opciones para representar grupos de valores de configuración relacionados en aplicaciones ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/options
ms.openlocfilehash: 800ff2039e7cc1fa37315ed55a77711dc9f47504
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2018
---
# <a name="options-pattern-in-aspnet-core"></a>Patrón de opciones en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

El patrón de opciones usa clases para representar grupos de configuraciones relacionadas. Cuando los valores de configuración están aislados por característica en clases independientes, la aplicación se ajusta a dos principios de ingeniería de software importantes:

* El [principio de segregación de interfaz (ISP)](http://deviq.com/interface-segregation-principle/): las características (clases) que dependen de valores de configuración dependerán únicamente de los valores de configuración que usen.
* [Separación de intereses](http://deviq.com/separation-of-concerns/): los valores de configuración para distintos elementos de la aplicación no son dependientes entre sí ni están emparejados.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample)). Este artículo es más fácil de seguir con la aplicación de ejemplo.

## <a name="basic-options-configuration"></a>Configuración de opciones básicas

La configuración de opciones básicas se muestra en el ejemplo &num;1 en la [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Una clase de opciones debe ser no abstracta con un constructor público sin parámetros. La siguiente clase, `MyOptions`, tiene dos propiedades: `Option1` y `Option2`. Configurar los valores predeterminados es opcional, pero el constructor de clases en el ejemplo siguiente establece el valor predeterminado de `Option1`. `Option2` tiene un valor predeterminado que se establece al inicializar la propiedad directamente (*Models/MyOptions.cs*):

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

La clase `MyOptions` se agrega al contenedor de servicios con [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) y enlaza a la configuración:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

El siguiente modelo de página usa la [inserción de dependencias de constructor](xref:fundamentals/dependency-injection#what-is-dependency-injection) con [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) para acceder a la configuración (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

El archivo *appSettings.json* del ejemplo especifica valores para `option1` y `option2`:

[!code-json[](options/sample/appsettings.json)]

Cuando se ejecuta la aplicación, el método `OnGet` del modelo de página devuelve una cadena que muestra los valores de la clase de opción:

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a>Configurar opciones simples con un delegado

La configuración de opciones simples con un delegado se muestra como ejemplo &num;2 en la [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Use un delegado para establecer los valores de opciones. La aplicación de ejemplo usa la clase `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

En el código siguiente, un segundo servicio `IConfigureOptions<TOptions>` se agrega al contenedor de servicios. Usa un delegado para configurar el enlace con `MyOptionsWithDelegateConfig`:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*:

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

Puede agregar varios proveedores de configuración. Los proveedores de configuración están disponibles en paquetes de NuGet. Se aplican en el orden en que están registrados.

Cada llamada a [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) agrega un servicio `IConfigureOptions<TOptions>` al contenedor de servicios. En el ejemplo anterior, los valores de `Option1` y `Option2` se especifican en *appSettings.json*, pero los valores de `Option1` y `Option2` se reemplazan por el delegado configurado.

Cuando se habilita más de un servicio de configuración, la última fuente de configuración especificada *gana* y establece el valor de configuración. Cuando se ejecuta la aplicación, el método `OnGet` del modelo de página devuelve una cadena que muestra los valores de la clase de opción:

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>Configuración de subopciones

La configuración de subopciones se muestra en el ejemplo &num;3 en la [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Las aplicaciones deben crear clases de opciones que pertenezcan a grupos específicos de características (clases) en la aplicación. Los elementos de la aplicación que requieran valores de configuración deben acceder solamente a los valores de configuración que usen.

Al enlazar opciones para la configuración, cada propiedad en el tipo de opciones se enlaza a una clave de configuración del formulario `property[:sub-property:]`. Por ejemplo, la propiedad `MyOptions.Option1` se enlaza a la clave `Option1`, que se lee desde la propiedad `option1` en *appSettings.json*.

En el código siguiente, se agrega un tercer servicio `IConfigureOptions<TOptions>` al contenedor de servicios. Enlaza `MySubOptions` a la sección `subsection` del archivo *appsettings.json*:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

El método de extensión `GetSection` requiere el paquete NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/). Si la aplicación usa el metapaquete [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/), el paquete se incluye automáticamente.

El archivo *appSettings.json* del ejemplo define un miembro `subsection` con las claves para `suboption1` y `suboption2`:

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

La clase `MySubOptions` define propiedades, `SubOption1` y `SubOption2`, para mantener los valores de subopciones (*Models/MySubOptions.cs*):

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

El método `OnGet` del modelo de página devuelve una cadena con los valores de subopciones (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

Cuando se ejecuta la aplicación, el método `OnGet` devuelve una cadena que muestra los valores de clase de subopciones:

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>Opciones proporcionadas por un modelo de vista o con inserción de vista directa

Las opciones proporcionadas por un modelo de vista o de inserción de vista directa se muestran en el ejemplo &num;4 en la [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Las opciones se pueden suministrar en un modelo de vista o insertando `IOptions<TOptions>` directamente en una vista (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

Para la inserción directa, inserte `IOptions<MyOptions>` con una directiva `@inject`:

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

Cuando se ejecuta la aplicación, se muestran los valores de opciones en la página representada:

![Los valores de opciones de Option1: value1_from_json y Option2: -1 se cargan desde el modelo y por inserción en la vista.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>Volver a cargar los datos de configuración con IOptionsSnapshot

El procedimiento de volver a cargar los datos de configuración con `IOptionsSnapshot` se muestra en el ejemplo &num;5 en la [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*Requiere ASP.NET Core 1.1 o versiones posteriores.*

[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) admite volver a cargar opciones con la mínima sobrecarga de procesamiento. En ASP.NET Core 1.1, `IOptionsSnapshot` es una instantánea de [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) y se actualiza automáticamente cada vez que el monitor desencadena cambios de acuerdo con los cambios del origen de datos. En ASP.NET Core 2.0 y versiones posteriores, las opciones se calculan una vez por solicitud cuando se accede a ellas y se almacenan en caché en el tiempo que dura la solicitud.

En el ejemplo siguiente se muestra cómo se crea un nuevo `IOptionsSnapshot` después de cambiar el archivo *appSettings.json* (*Pages/Index.cshtml.cs*). Varias solicitudes al servidor devuelven valores constantes proporcionados por el archivo *appSettings.json* hasta que se modifique el archivo y vuelva a cargarse la configuración.

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

En la siguiente imagen se muestran los valores `option1` y `option2` iniciales cargados desde el archivo *appSettings.json*:

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

Cambie los valores del archivo *appSettings.json* a `value1_from_json UPDATED` y `200`. Guarde el archivo *appSettings.json*. Actualice el explorador para ver qué valores de opciones se han actualizado:

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a>Compatibilidad de opciones con nombre con IConfigureNamedOptions

La compatibilidad de opciones con nombre con [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) se muestra en el ejemplo &num;6 de la [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*Requiere ASP.NET Core 2.0 o versiones posteriores.*

La compatibilidad con las *opciones con nombre* permite a la aplicación distinguir entre las configuraciones de opciones con nombre. En la aplicación de ejemplo, las opciones con nombre se declaran con [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, cadena, acción&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) que, a su vez, llama al método de extensión [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure):

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

La aplicación de ejemplo accede a las opciones con nombre con [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

Al ejecutar la aplicación de ejemplo, se devuelven las opciones con nombre:

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

Se proporcionan valores de `named_options_1` a partir de la configuración, que se cargan desde el archivo *appSettings.json*. Los valores de `named_options_2` los proporciona:

* El delegado `named_options_2` en `ConfigureServices` para `Option1`.
* El valor predeterminado para `Option2` proporcionado por la clase `MyOptions`.

Configure todas las instancias de opciones con nombre con el método [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall). El siguiente código configura `Option1` para todas las instancias de configuración con nombre con un valor común. Agregue manualmente este código al método `Configure`:

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

Al ejecutar la aplicación de ejemplo después de agregar el código se produce el siguiente resultado:

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> En ASP.NET 2.0 Core y versiones posteriores, todas las opciones son instancias con nombre. Las instancias de `IConfigureOption` existentes se usan para seleccionar como destino la instancia de `Options.DefaultName`, que es `string.Empty`. `IConfigureNamedOptions` también implementa `IConfigureOptions`. La implementación predeterminada de [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([origen de referencia](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) tiene lógica para usar cada una de forma adecuada. La opción con nombre `null` se usa para seleccionar como destino todas las instancias con nombre, en lugar de una instancia con nombre determinada ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) y [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) usan esta convención).

## <a name="ipostconfigureoptions"></a>IPostConfigureOptions

*Requiere ASP.NET Core 2.0 o versiones posteriores.*

Establezca la postconfiguración con [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1). La postconfiguración se ejecuta después de que tenga lugar toda la configuración de [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1):

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) está disponible para postconfigurar las opciones con nombre:

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) para postconfigurar todas las instancias de configuración con nombre:

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a>Generador de opciones, supervisión y memoria caché

[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) se usa para crear notificaciones cuando cambian las instancias de `TOptions`. `IOptionsMonitor` admite opciones recargables, notificaciones de cambios y `IPostConfigureOptions`.

[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 o versiones posteriores) es responsable de crear nuevas instancias de opciones. Tiene un solo método [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create). La implementación predeterminada toma todas las instancias registradas de `IConfigureOptions` y `IPostConfigureOptions`, y configura todas las configuraciones primero, seguidas de las postconfiguraciones. Distingue entre `IConfigureNamedOptions` y `IConfigureOptions`, y solo llama a la interfaz adecuada.

[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 o versiones posteriores) lo usa `IOptionsMonitor` para almacenar en caché instancias de `TOptions`. `IOptionsMonitorCache` invalida instancias de opciones en la supervisión para que se pueda volver a calcular el valor ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)). Los valores se pueden introducir manualmente y mediante [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd). Se usa el método [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) cuando todas las instancias con nombre se deben volver a crear a petición.

## <a name="accessing-options-during-startup"></a>Acceso a opciones durante el inicio

`IOptions` puede usarse en `Configure`, ya que los servicios se compilan antes de que se ejecute el método `Configure`. Si se compila un proveedor de servicios en `ConfigureServices` para acceder a opciones, no incluirá ninguna configuración de opción proporcionada después de que se compile el proveedor de servicios. Por tanto, puede que exista un estado incoherente de opciones debido al orden de los registros de servicio.

Puesto que las opciones suelen cargarse a partir de la configuración, puede usar la configuración al inicio en `Configure` y `ConfigureServices`. Para obtener ejemplos de uso de la configuración durante el inicio, vea el tema [Inicio de la aplicación ASP.NET Core](xref:fundamentals/startup).

## <a name="see-also"></a>Vea también

* [Configuración](xref:fundamentals/configuration/index)
