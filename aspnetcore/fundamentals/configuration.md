---
title: "Configuración en ASP.NET Core"
author: rick-anderson
description: "Obtenga información acerca de cómo usar la API de configuración para configurar una aplicación ASP.NET Core desde varios orígenes."
keywords: "Núcleo de ASP.NET, configuración, JSON, configuración"
ms.author: riande
manager: wpickett
ms.date: 6/24/2017
ms.topic: article
ms.assetid: b3a5984d-e172-42eb-8a48-547e4acb6806
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration
ms.openlocfilehash: 379030df4ca91a38fce251aeaab9c5dfaf11e915
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2017
---
# <a name="configuration-in-aspnet-core"></a>Configuración en ASP.NET Core

[Rick Anderson](https://twitter.com/RickAndMSFT), [marca Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), y [Luke Latham](https://github.com/guardrex)

La API de configuración proporciona una manera de configurar una aplicación basada en una lista de pares nombre / valor. Se lee la configuración en tiempo de ejecución de varios orígenes. Los pares de nombre-valor se pueden agrupar en una jerarquía multinivel. Existen proveedores de configuración para:

* Formatos de archivo (INI, JSON y XML)
* Argumentos de línea de comandos
* Variables de entorno
* Objetos de .NET en memoria
* Un almacén de usuario cifrado
* [Almacén de claves de Azure](xref:security/key-vault-configuration)
* Proveedores personalizados, que se instale o cree

Cada valor de configuración se asigna a una clave de cadena. No hay compatibilidad de enlace integrado para deserializar la configuración en un personalizado [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objeto (una clase .NET simple con propiedades).

[Ver o descargar el código de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample)

## <a name="simple-configuration"></a>Muestra una configuración sencilla

La siguiente aplicación de consola utiliza el proveedor de configuración de JSON:

[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]

La aplicación lee y muestra los valores de configuración siguientes:

[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]

Configuración consta de una lista jerárquica de los pares de nombre / valor en el que los nodos están separados por un coma. Para recuperar un valor, tener acceso a la `Configuration` indizador con la correspondiente clave del elemento:

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

Para trabajar con matrices en orígenes de configuración con formato JSON, usar un índice de matriz como parte de la cadena separada por dos puntos. En el ejemplo siguiente se obtiene el nombre del primer elemento en el anterior `wizards` matriz:

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

Pares de nombre / valor que se escriben en integrado en `Configuration` proveedores son **no** es persistente, sin embargo, puede crear un proveedor personalizado que guarda los valores. Vea [proveedor de configuración personalizada](xref:fundamentals/configuration#custom-config-providers).

En el ejemplo anterior utiliza el indizador de configuración para leer los valores. Acceso a la configuración fuera de `Startup`, use la [patrón opciones](xref:fundamentals/configuration#options-config-objects). El *patrón opciones* se muestra más adelante en este artículo.

Es habitual tener distintos valores de configuración para los entornos diferentes, por ejemplo, desarrollo, prueba y producción. El `CreateDefaultBuilder` método de extensión en una aplicación de ASP.NET Core 2.x (o mediante `AddJsonFile` y `AddEnvironmentVariables` directamente en una aplicación de ASP.NET Core 1.x) agrega proveedores de configuración para leer archivos JSON y sistema de orígenes de configuración:

* *appsettings.json*
* *appSettings. \<EnvironmentName > .json*
* variables de entorno

Vea [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) para obtener una explicación de los parámetros. `reloadOnChange`solo se admite en ASP.NET Core 1.1 y versiones posteriores. 

Orígenes de configuración se leen en el orden en que se especifican. En el código anterior, las variables de entorno se leen de la última. Los valores de configuración establecido a través del entorno podría reemplazarlas que se establecen en los dos proveedores anteriores.

El entorno se establece normalmente en uno de `Development`, `Staging`, o `Production`. Vea [trabajar con varios entornos](environments.md) para obtener más información.

Consideraciones sobre la configuración:

* `IOptionsSnapshot`puede volver a cargar los datos de configuración, cuando el estado cambia. Use `IOptionsSnapshot` si tiene que volver a cargar los datos de configuración.  Vea [IOptionsSnapshot](#ioptionssnapshot) para obtener más información.
* Claves de configuración que distinguen mayúsculas de minúsculas.
* Una práctica recomendada es especificar variables de entorno por último, para que el entorno local puedan reemplazar todo lo establecido en los archivos de configuración implementada.
* **Nunca** almacenar contraseñas u otros datos confidenciales en el código de proveedor de configuración o en archivos de configuración de texto sin formato. No utilice secretos de producción en el desarrollo o entornos de prueba. En su lugar, especifique secretos fuera del árbol del proyecto, por lo que no se pueden confirmar accidentalmente en el repositorio. Obtenga más información sobre [trabajar con varios entornos](environments.md) y administrar [almacenamiento seguro para la ejecución de secretos de aplicación durante el desarrollo](../security/app-secrets.md).
* Si `:` no se puede utilizar en variables de entorno en el sistema, reemplace `:` con `__` (subrayado doble).

<a name=options-config-objects></a>

## <a name="using-options-and-configuration-objects"></a>Uso de opciones y objetos de configuración

El patrón de opciones usa las clases de opciones personalizadas para representar un grupo de valores relacionados. Se recomienda crear clases desacopladas para cada característica dentro de la aplicación. Clases desacopladas siguen:

* El [principio de segregación de interfaz (ISP)](http://deviq.com/interface-segregation-principle/) : clases dependen únicamente de los valores de configuración que se usan.
* [Separación de intereses](http://deviq.com/separation-of-concerns/) : configuración para las distintas partes de la aplicación no es dependientes o acoplamiento entre sí.

La clase de opciones debe ser no abstracto con un constructor público sin parámetros. Por ejemplo:

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]

<a name=options-example></a>

En el código siguiente, se habilita el proveedor de configuración de JSON. La `MyOptions` clase se agrega al contenedor de servicios y se enlaza a la configuración.

[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]

El siguiente [controlador](../mvc/controllers/index.md) utiliza [constructor inyección de dependencia](xref:fundamentals/dependency-injection#what-is-dependency-injection) en [ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) acceso a la configuración:

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]

Con el siguiente *appSettings.JSON que se* archivo:

[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]

El `HomeController.Index` método `option1 = value1_from_json, option2 = 2`.

Las aplicaciones típicas no enlazar toda la configuración a un archivo de opciones de inicio único. Más adelante voy a explicar cómo usar `GetSection` para enlazar a una sección.

En el código siguiente, un segundo `IConfigureOptions<TOptions>` servicio se agrega al contenedor de servicios. Usa un delegado para configurar el enlace con `MyOptions`.

[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]

Puede agregar varios proveedores de configuración. Proveedores de configuración están disponibles en paquetes de NuGet. Se aplican en orden que están registrados.

Cada llamada a `Configure<TOptions>` agrega un `IConfigureOptions<TOptions>` servicio al contenedor de servicios. En el ejemplo anterior, los valores de `Option1` y `Option2` se especifican en *appSettings.JSON que se* --pero el valor de `Option1` se reemplaza por el delegado configurado. 

Cuando se habilita más de un servicio de configuración, la última fuente de configuración especificado "gana" (que establece el valor de configuración). En el código anterior, el `HomeController.Index` método `option1 = value1_from_action, option2 = 2`.

Al enlazar opciones de configuración, cada propiedad en el tipo de opciones está enlazado a una clave de configuración del formulario `property[:sub-property:]`. Por ejemplo, el `MyOptions.Option1` propiedad se enlaza a la clave `Option1`, que se lee desde el `option1` propiedad en *appSettings.JSON que se*. Más adelante en este artículo, se muestra un ejemplo de la subpropiedad.

En el código siguiente, una tercera `IConfigureOptions<TOptions>` servicio se agrega al contenedor de servicios. Enlaza `MySubOptions` a la sección `subsection` de la *appSettings.JSON que se* archivo:

[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]

Nota: Este método de extensión requiere el `Microsoft.Extensions.Options.ConfigurationExtensions` paquete NuGet.

Con los siguientes *appSettings.JSON que se* archivo:

[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]

La `MySubOptions` clase:

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]

Con los siguientes `Controller`:

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]

`subOption1 = subvalue1_from_json, subOption2 = 200`se devuelve.

También puede proporcionar opciones en un modelo de vista o insertar `IOptions<TOptions>` directamente en una vista:

[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]

<a name=in-memory-provider></a>

## <a name="ioptionssnapshot"></a>IOptionsSnapshot

*Requiere ASP.NET Core 1.1 o posterior.*

`IOptionsSnapshot`permite volver a cargar los datos de configuración cuando ha cambiado el archivo de configuración. Tiene una sobrecarga mínima. Usar `IOptionsSnapshot` con `reloadOnChange: true`, las opciones están enlazadas a `IConfiguration` y volver a cargar cuando cambia.

El siguiente ejemplo se muestra cómo un nuevo `IOptionsSnapshot` se crea después *config.json* cambios. Las solicitudes realizadas al servidor devolverán la misma hora *config.json* tiene **no** cambiado. La primera solicitud después de *config.json* cambios mostrará una nueva hora.

[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]

La siguiente imagen muestra la salida de servidor:

![que muestra de imagen de explorador "última actualización: 11/22/2016 4:43 PM"](configuration/_static/first.png)

Actualizar el explorador no cambia el valor de mensaje o el tiempo que se muestra (cuando *config.json* no ha cambiado).

Cambiar y guardar la *config.json* y, a continuación, actualice el explorador:

![que muestra de imagen de explorador "se actualizó por última vez a e: 11/22/2016 4:53 P.M."](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>Proveedor en memoria y el enlace a una clase POCO

El ejemplo siguiente muestra cómo usar el proveedor en memoria y enlazar a una clase:

[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]

Valores de configuración se devuelven como cadenas, pero el enlace permite la construcción de objetos. Enlace le permite recuperar objetos POCO o gráficos de objetos incluso todo. El ejemplo siguiente muestra cómo enlazar a `MyWindow` y utiliza el patrón de opciones con una aplicación de MVC de ASP.NET Core:

[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]

[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]

Enlazar la clase personalizada de `ConfigureServices` al crear el host:

[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]

Mostrar la configuración de la `HomeController`:

[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]

### <a name="getvalue"></a>GetValue

El ejemplo siguiente se muestra la [GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) método de extensión:

[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

El ConfigurationBinder `GetValue<T>` método le permite especificar un valor predeterminado (80 en el ejemplo). `GetValue<T>`es para escenarios sencillos y no se enlaza con secciones completas. `GetValue<T>`Obtiene los valores escalares de `GetSection(key).Value` convertir a un tipo específico.

## <a name="binding-to-an-object-graph"></a>Enlazar a un gráfico de objetos

Puede enlazar de forma recursiva para cada objeto en una clase. Tenga en cuenta la siguiente `AppOptions` clase:

[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]

El ejemplo siguiente se enlaza a la `AppOptions` clase:

[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]

**Núcleo de ASP.NET 1.1** y versiones posteriores pueden usar `Get<T>`, que funciona con secciones completas. `Get<T>`puede ser más convienent que el uso de `Bind`. El código siguiente muestra cómo utilizar `Get<T>` con el ejemplo anterior:

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

Con los siguientes *appSettings.JSON que se* archivo:

[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]

El programa mostrará `Height 11`.

El código siguiente puede utilizarse para la unidad de la configuración de pruebas:

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var options = new AppOptions();
    config.GetSection("App").Bind(options);

    Assert.Equal("Rick", options.Profile.Machine);
    Assert.Equal(11, options.Window.Height);
    Assert.Equal(11, options.Window.Width);
    Assert.Equal("connectionstring", options.Connection.Value);
}
```

<a name=custom-config-providers></a>

## <a name="basic-sample-of-entity-framework-custom-provider"></a>Ejemplo básico de proveedor personalizado de Entity Framework

En esta sección, se crea un proveedor de configuración básica que lee los pares nombre / valor de una base de datos usan EF. 

Definir una `ConfigurationValue` entidad para almacenar los valores de configuración en la base de datos:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

Agregar un `ConfigurationContext` para almacenar y tener acceso a los valores configurados:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

Cree una clase que implemente [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

Crear el proveedor de configuración personalizados heredando de [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).  El proveedor de configuración inicializa la base de datos cuando está vacía:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

Cuando se ejecuta el ejemplo, se muestran los valores de resaltado de la base de datos ("value_from_ef_1" y "value_from_ef_2").

Puede agregar un `EFConfigSource` método de extensión para agregar el origen de configuración:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

El código siguiente muestra cómo utilizar la opción de instalación `EFConfigProvider`:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

Tenga en cuenta el ejemplo agrega personalizado `EFConfigProvider` una vez que el proveedor JSON, por lo que cualquier configuración de la base de datos invalidará la configuración de la *appSettings.JSON que se* archivo.

Con los siguientes *appSettings.JSON que se* archivo:

[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]

Se muestra lo siguiente:

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>Proveedor de configuración de la línea de comandos

El [proveedor de configuración de la línea de comandos](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) recibe pares de clave y valor de argumento de línea de comandos para la configuración en tiempo de ejecución.

[Ver o descargar el ejemplo de configuración de la línea de comandos](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample/CommandLine)

### <a name="setting-up-the-provider"></a>Configurar el proveedor

# <a name="basic-configurationtabbasicconfiguration"></a>[Configuración básica](#tab/basicconfiguration)

Para activar la configuración de línea de comandos, llame a la `AddCommandLine` método de extensión en una instancia de [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program.cs?highlight=18,21)]

Ejecutar el código, se muestra el siguiente resultado:

```console
MachineName: MairaPC
Left: 1980
```

Pasar pares de clave y valor de argumento en la línea de comandos cambia los valores de `Profile:MachineName` y `App:MainWindow:Left`:

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

Muestra la ventana de consola:

```console
MachineName: BartPC
Left: 1979
```

Para invalidar la configuración proporcionada por otros proveedores de configuración con la configuración de línea de comandos, llame a `AddCommandLine` último en `ConfigurationBuilder`:

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Las aplicaciones de 2.x típicas de ASP.NET Core usan el método estáticos `CreateDefaultBuilder` para crear el host:

[!code-csharp[Main](configuration/sample_snapshot/Program.cs?highlight=12)]

`CreateDefaultBuilder`carga la configuración opcional de *appSettings.JSON que se*, *appsettings. {} Entorno} .json*, [secretos del usuario](xref:security/app-secrets) (en el `Development` entorno), variables de entorno y los argumentos de línea de comandos. El proveedor de configuración de la línea de comandos es llamado en último lugar. Llamando al proveedor de última permite que los argumentos de línea de comandos que se pasan en tiempo de ejecución para invalidar la configuración establecida por los otros proveedores de configuración llamado anteriormente.

Tenga en cuenta que para *appsettings* archivos que `reloadOnChange` está habilitado. Argumentos de línea de comandos se invalidan si un valor de configuración coincidente en un *appsettings* archivo se cambia después de la aplicación se inicia.

> [!NOTE]
> Como alternativa al uso de la `CreateDefaultBuilder` (método), crear un host con [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) y crear manualmente la configuración con [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) es compatible con ASP.NET Core 2.x. Consulte la ficha de 1.x ASP.NET Core para obtener más información.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Crear un [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) y llamar a la `AddCommandLine` método se debe utilizar el proveedor de configuración de la línea de comandos. Llamando al proveedor de última permite que los argumentos de línea de comandos que se pasan en tiempo de ejecución para invalidar la configuración establecida por los otros proveedores de configuración llamado anteriormente. Aplicar la configuración de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) con el `UseConfiguration` método:

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>Argumentos

Argumentos que se pasan en la línea de comandos deben ajustarse a uno de los dos formatos que se muestra en la tabla siguiente.

| Formato de argumento                                                     | Ejemplo        |
| ------------------------------------------------------------------- | :------------: |
| Único argumento: un par de clave y valor separados por un signo igual (`=`) | `key1=value`   |
| Secuencia de dos argumentos: un par de clave y valor separados por un espacio    | `/key1 value1` |

**Argumento único**

El valor debe seguir un signo igual (`=`). El valor puede ser null (por ejemplo, `mykey=`).

La clave puede tener un prefijo.

| Prefijo de la clave               | Ejemplo         |
| ------------------------ | :-------------: |
| Ningún prefijo                | `key1=value1`   |
| Único guión (`-`) &#8224; | `-key2=value2`  |
| Dos guiones (`--`)        | `--key3=value3` |
| Barra diagonal (`/`)      | `/key4=value4`  |

&#8224; Una clave con un prefijo único guión (`-`) deben indicarse en [cambiar las asignaciones de](#switch-mappings), que se describen a continuación.

Comando de ejemplo:

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

Nota: Si `-key1` no está presente en el [cambiar las asignaciones de](#switch-mappings) dado al proveedor de configuración, un `FormatException` se produce.

**Secuencia de dos argumentos**

El valor no puede ser nulo y debe seguir la clave separada por un espacio.

La clave debe tener un prefijo.

| Prefijo de la clave               | Ejemplo         |
| ------------------------ | :-------------: |
| Único guión (`-`) &#8224; | `-key1 value1`  |
| Dos guiones (`--`)        | `--key2 value2` |
| Barra diagonal (`/`)      | `/key3 value3`  |

&#8224; Una clave con un prefijo único guión (`-`) deben indicarse en [cambiar las asignaciones de](#switch-mappings), que se describen a continuación.

Comando de ejemplo:

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

Nota: Si `-key1` no está presente en el [cambiar las asignaciones de](#switch-mappings) dado al proveedor de configuración, un `FormatException` se produce.

### <a name="duplicate-keys"></a>Claves duplicadas

Si se proporcionan las claves duplicadas, se utiliza el último par de clave-valor.

### <a name="switch-mappings"></a>Cambiar las asignaciones

Cuando se compila manualmente la configuración con `ConfigurationBuilder`, también puede proporcionar un diccionario de asignaciones de conmutador para la `AddCommandLine` método. Asignaciones de conmutador permiten proporcionar lógica de sustitución de nombre de clave.

Cuando se utiliza el diccionario de asignaciones de conmutador, se comprueba el diccionario para una clave que coincida con la clave proporcionada por un argumento de línea de comandos. Si la clave de la línea de comandos se encuentra en el diccionario, se pasa el valor del diccionario (sustitución de claves) atrás para establecer la configuración. Se requiere para cualquier clave de línea de comandos como precedida un guion solo una asignación de conmutador (`-`).

Cambiar las reglas de clave de diccionario de asignaciones:

* Conmutadores deben empezar por un guión (`-`) o guión doble (`--`).
* El diccionario de asignaciones del conmutador no debe contener claves duplicadas.

En el ejemplo siguiente, la `GetSwitchMappings` método permite a los argumentos de línea de comandos usar un solo guión (`-`) la clave de prefijo y evitar prefijos subclaves iniciales.

[!code-csharp[Main](configuration/sample/CommandLine/Program.cs?highlight=10-19,32)]

Sin proporcionar argumentos de línea de comandos, el diccionario proporcionadas para `AddInMemoryCollection` establece los valores de configuración. Ejecute la aplicación con el siguiente comando:

```console
dotnet run
```

Muestra la ventana de consola:

```console
MachineName: RickPC
Left: 1980
```

Para pasar valores de configuración, utilice lo siguiente:

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

Muestra la ventana de consola:

```console
MachineName: DahliaPC
Left: 1984
```

Después de crea el diccionario de asignaciones de conmutador, que contiene los datos que se muestra en la tabla siguiente.

| Key            | Valor                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

Para mostrar el cambio de clave utilizando el diccionario, ejecute el siguiente comando:

```console
dotnet run -MachineName=ChadPC -Left=1988
```

Se intercambian las claves de la línea de comandos. La ventana de consola muestra los valores de configuración de `Profile:MachineName` y `App:MainWindow:Left`:

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a>El archivo web.config

A *web.config* archivo es necesario cuando se hospeda la aplicación en IIS o IIS Express. *Web.config* activa el AspNetCoreModule en IIS para iniciar la aplicación. Configuración de *web.config* habilitar la AspNetCoreModule en IIS para que inicie la aplicación y configurar otros módulos y la configuración de IIS. Si usa Visual Studio y eliminar *web.config*, Visual Studio creará uno nuevo.

## <a name="additional-notes"></a>Notas adicionales

* Inyección de dependencia (DI) no se establece hasta después de `ConfigureServices` se invoca.
* El sistema de configuración no es compatible con DI.
* `IConfiguration`tiene dos especializaciones:
  * `IConfigurationRoot`Se usa para el nodo raíz. Puede desencadenar una recarga.
  * `IConfigurationSection`Representa una sección de valores de configuración. El `GetSection` y `GetChildren` métodos devuelven un `IConfigurationSection`.

## <a name="additional-resources"></a>Recursos adicionales

* [Trabajar con varios entornos](environments.md)
* [Almacenamiento seguro de secretos de aplicación durante el desarrollo](../security/app-secrets.md)
* [Hospedar en ASP.NET Core](xref:fundamentals/hosting)
* [Inserción de dependencias](dependency-injection.md)
* [Proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration)
