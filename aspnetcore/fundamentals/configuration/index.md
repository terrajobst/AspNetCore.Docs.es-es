---
title: "Configuración en ASP.NET Core"
author: rick-anderson
description: "Use la API de configuración para configurar una aplicación ASP.NET Core mediante varios métodos."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/index
ms.openlocfilehash: b662e66ab5b4c46d1a8d10eb7c38bf4064b5b927
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2018
---
# <a name="configure-an-aspnet-core-app"></a>Configurar una aplicación ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)

La API de configuración proporciona una manera de configurar una aplicación web ASP.NET Core según una lista de pares de nombre y valor. La configuración se lee en tiempo de ejecución desde varios orígenes. Puede agrupar estos pares de nombre y valor en una jerarquía multinivel. 

Existen proveedores de configuración para:

* Formatos de archivo (INI, JSON y XML)
* Argumentos de la línea de comandos
* Variables de entorno
* Objetos de .NET en memoria
* Un almacén de usuario cifrado
* [Azure Key Vault](xref:security/key-vault-configuration)
* Proveedores personalizados (instalados o creados)

Cada valor de configuración se asigna a una clave de cadena. Hay compatibilidad de enlace integrada para deserializar la configuración en un objeto [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) personalizado (una clase simple de .NET con propiedades).

El patrón de opciones usa las clases de opciones para representar grupos de configuraciones relacionadas. Para más información sobre cómo usar el patrón de opciones, vea el tema [Opciones](xref:fundamentals/configuration/options).

[Vea o descargue el código de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="json-configuration"></a>Configuración de JSON

En la aplicación de consola siguiente se usa el proveedor de configuración de JSON:

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

La aplicación lee y muestra los valores de configuración siguientes:

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

La configuración consta de una lista jerárquica de pares de nombre y valor en los que los nodos se separan con dos puntos. Para recuperar un valor, obtenga acceso al indizador `Configuration` con la clave del elemento correspondiente:

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

Para trabajar con matrices en orígenes de configuración con formato JSON, use un índice de matriz como parte de la cadena separada por dos puntos. En el ejemplo siguiente se obtiene el nombre del primer elemento de la matriz `wizards` anterior:

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

Los pares de nombre y valor que se escriben en los proveedores de `Configuration` integrados **no** se conservan, pero se puede crear un proveedor personalizado que guarde los valores. Vea [Proveedor de configuración personalizado](xref:fundamentals/configuration/index#custom-config-providers).

En el ejemplo anterior se usa el indizador de configuración para leer los valores. Para obtener acceso a la configuración fuera de `Startup`, use el *patrón de opciones*. Para más información, vea el tema [Opciones](xref:fundamentals/configuration/options).

Es habitual tener distintos valores de configuración para entornos diferentes, por ejemplo para desarrollo, pruebas y producción. El método de extensión `CreateDefaultBuilder` en una aplicación de ASP.NET Core 2.x (o mediante `AddJsonFile` y `AddEnvironmentVariables` directamente en una aplicación de ASP.NET Core 1.x) agrega proveedores de configuración para leer archivos JSON y orígenes de configuración del sistema:

* *appsettings.json*
* *appsettings.\<NombreDelEntorno>.json*
* Variables de entorno

Vea [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) para obtener una explicación de los parámetros. `reloadOnChange` solo se admite en ASP.NET Core 1.1 y versiones posteriores. 

Los orígenes de configuración se leen en el orden en que se especifican. En el código anterior, las variables de entorno se leen en último lugar. Cualquier valor de configuración establecido mediante el entorno reemplaza a los establecidos en los dos proveedores anteriores.

El entorno se establece normalmente en `Development`, `Staging` o `Production`. Para más información, vea [Trabajar con varios entornos](xref:fundamentals/environments).

Consideraciones de configuración:

* `IOptionsSnapshot` puede volver a cargar los datos de configuración cuando cambia. Vea [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) para más información.
* Las claves de configuración no distinguen mayúsculas de minúsculas.
* Especifique las variables de entorno en último lugar para que el entorno local pueda invalidar la configuración en los archivos de configuración implementados.
* **Nunca** almacene contraseñas u otros datos confidenciales en el código del proveedor de configuración o en archivos de configuración de texto sin formato. No use secretos de producción en los entornos de desarrollo o pruebas. En su lugar, especifique los secretos fuera del proyecto para que no se confirmen en el repositorio de manera accidental. Obtenga más información sobre [trabajar con varios entornos](xref:fundamentals/environments) y administrar el [almacenamiento seguro de los secretos de aplicación durante el desarrollo](xref:security/app-secrets).
* Si no se puede usar un signo de dos puntos (`:`) en las variables de entorno del sistema, reemplace los dos puntos (`:`) por un carácter de subrayado doble (`__`).

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>Proveedor en memoria y enlace a una clase POCO

En el ejemplo siguiente se muestra cómo usar el proveedor en memoria y enlazar a una clase:

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

Los valores de configuración se devuelven como cadenas, pero el enlace permite la construcción de objetos. El enlace permite recuperar objetos POCO o incluso gráficos de objetos completos.

### <a name="getvalue"></a>GetValue

En el ejemplo siguiente se muestra el método de extensión [GetValue&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_):

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

El método `GetValue<T>` de ConfigurationBinder permite especificar un valor predeterminado (80 en el ejemplo). `GetValue<T>` es para escenarios sencillos y no se enlaza con secciones completas. `GetValue<T>` obtiene los valores escalares de `GetSection(key).Value` convertidos a un tipo específico.

## <a name="bind-to-an-object-graph"></a>Enlazar a un gráfico de objetos

Puede enlazar de forma recursiva a cada objeto de una clase. Observe la clase `AppSettings` siguiente:

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

El ejemplo siguiente se enlaza a la clase `AppSettings`:

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

En **ASP.NET Core 1.1** y versiones posteriores se puede usar `Get<T>`, que funciona con secciones completas. `Get<T>` puede ser más conveniente que usar `Bind`. En el código siguiente se muestra cómo usar `Get<T>` con el ejemplo anterior:

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

Con el siguiente archivo *appsettings.json*:

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

El programa muestra `Height 11`.

El código siguiente se puede usar para pruebas unitarias de la configuración:

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

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a>Crear un proveedor personalizado de Entity Framework

En esta sección, se crea un proveedor de configuración básico que lee pares de nombre y valor de una base de datos que se crea con EF. 

Defina una entidad `ConfigurationValue` para almacenar los valores de configuración en la base de datos:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

Agregue un `ConfigurationContext` para almacenar y tener acceso a los valores configurados:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

Cree una clase que implemente [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

Cree el proveedor de configuración personalizado heredando de [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).  El proveedor de configuración inicializa la base de datos cuando está vacía:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

Cuando se ejecuta el ejemplo, se muestran los valores resaltados de la base de datos ("value_from_ef_1" y "value_from_ef_2").

Puede agregar un método de extensión `EFConfigSource` para agregar el origen de configuración:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

En el código siguiente se muestra cómo puede usar el `EFConfigProvider` personalizado:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

Tenga en cuenta que el ejemplo agrega el `EFConfigProvider` personalizado después del proveedor de JSON, por lo que cualquier configuración de la base de datos invalidará la configuración del archivo *appsettings.json*.

Con el siguiente archivo *appsettings.json*:

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

Se muestra lo siguiente:

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>Proveedor de configuración CommandLine

El [proveedor de configuración CommandLine](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) recibe pares de clave y valor de argumento de línea de comandos para la configuración en tiempo de ejecución.

[Ver o descargar el ejemplo de configuración CommandLine](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a>Configurar y usar el proveedor de configuración CommandLine

# <a name="basic-configurationtabbasicconfiguration"></a>[Configuración básica](#tab/basicconfiguration)

Para activar la configuración de línea de comandos, llame al método de extensión `AddCommandLine` en una instancia de [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

Al ejecutar el código, se muestra la salida siguiente:

```console
MachineName: MairaPC
Left: 1980
```

Pasar pares de clave y valor de argumento en la línea de comandos cambia los valores de `Profile:MachineName` y `App:MainWindow:Left`:

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

En la ventana de consola se muestra lo siguiente:

```console
MachineName: BartPC
Left: 1979
```

Para invalidar la configuración proporcionada por otros proveedores de configuración con la configuración de línea de comandos, llame a `AddCommandLine` en último lugar en `ConfigurationBuilder`:

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Las aplicaciones típicas de ASP.NET Core 2.x usan el método estático útil `CreateDefaultBuilder` para crear el host:

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

`CreateDefaultBuilder` carga la configuración opcional de *appsettings.json*, *appsettings.{Entorno}.json*, [secretos del usuario](xref:security/app-secrets) (en el entorno `Development`), variables de entorno y argumentos de línea de comandos. El proveedor de configuración CommandLine se llama en último lugar. Llamar al proveedor en último lugar permite que los argumentos de línea de comandos que se pasan en tiempo de ejecución invaliden la configuración establecida por los otros proveedores de configuración que se llamaron anteriormente.

Tenga en cuenta que para los archivos *appsettings* se habilita `reloadOnChange`. Los argumentos de línea de comandos se invalidan si un valor de configuración coincidente en un archivo *appsettings* se cambia después de iniciar la aplicación.

> [!NOTE]
> Como alternativa al uso del método `CreateDefaultBuilder`, en ASP.NET Core 2.x se admite crear un host con [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) y crear manualmente la configuración con [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder). Vea la pestaña ASP.NET Core 1.x para más información.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Cree un [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) y llame al método `AddCommandLine` para usar el proveedor de configuración CommandLine. Llamar al proveedor en último lugar permite que los argumentos de línea de comandos que se pasan en tiempo de ejecución invaliden la configuración establecida por los otros proveedores de configuración que se llamaron anteriormente. Aplique la configuración de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) con el método `UseConfiguration`:

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>Argumentos

Los argumentos que se pasan en la línea de comandos deben ajustarse a uno de los dos formatos que se muestran en la tabla siguiente.

| Formato de argumento                                                     | Ejemplo        |
| ------------------------------------------------------------------- | :------------: |
| Un solo argumento: un par de clave y valor separado por un signo igual (`=`) | `key1=value`   |
| Secuencia de dos argumentos: un par de clave y valor separado por un espacio    | `/key1 value1` |

**Un solo argumento**

El valor debe seguir a un signo igual (`=`). El valor puede ser NULL (por ejemplo, `mykey=`).

La clave puede tener un prefijo.

| Prefijo de la clave               | Ejemplo         |
| ------------------------ | :-------------: |
| Sin prefijo                | `key1=value1`   |
| Un solo guion (`-`)&#8224; | `-key2=value2`  |
| Dos guiones (`--`)        | `--key3=value3` |
| Barra diagonal (`/`)      | `/key4=value4`  |

&#8224;En las [asignaciones de modificador](#switch-mappings) que se describen a continuación debe proporcionarse una clave con un prefijo de un solo guion (`-`).

Comando de ejemplo:

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

Nota: Si `-key1` no está presente en las [asignaciones de modificador](#switch-mappings) que se proporcionan al proveedor de configuración, se produce una excepción `FormatException`.

**Secuencia de dos argumentos**

El valor no puede ser NULL y debe seguir a la clave separado por un espacio.

La clave debe tener un prefijo.

| Prefijo de la clave               | Ejemplo         |
| ------------------------ | :-------------: |
| Un solo guion (`-`)&#8224; | `-key1 value1`  |
| Dos guiones (`--`)        | `--key2 value2` |
| Barra diagonal (`/`)      | `/key3 value3`  |

&#8224;En las [asignaciones de modificador](#switch-mappings) que se describen a continuación debe proporcionarse una clave con un prefijo de un solo guion (`-`).

Comando de ejemplo:

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

Nota: Si `-key1` no está presente en las [asignaciones de modificador](#switch-mappings) que se proporcionan al proveedor de configuración, se produce una excepción `FormatException`.

### <a name="duplicate-keys"></a>Claves duplicadas

Si se proporcionan claves duplicadas, se usa el último par de clave y valor.

### <a name="switch-mappings"></a>Asignaciones de modificador

Cuando la configuración se compila de forma manual con `ConfigurationBuilder`, opcionalmente se puede proporcionar un diccionario de asignaciones de modificador para el método `AddCommandLine`. Las asignaciones de modificador permiten proporcionar lógica de sustitución de nombres de clave.

Cuando se usa el diccionario de asignaciones de modificador, se comprueba en el diccionario si una clave coincide con la clave proporcionada por un argumento de línea de comandos. Si la clave de la línea de comandos se encuentra en el diccionario, se devuelve el valor del diccionario (el reemplazo de la clave) para establecer la configuración. Se requiere una asignación de conmutador para cualquier clave de línea de comandos precedida por un solo guion (`-`).

Reglas de clave del diccionario de asignaciones de modificador:

* Los modificadores deben empezar por un guion (`-`) o guion doble (`--`).
* El diccionario de asignaciones de modificador no debe contener claves duplicadas.

En el ejemplo siguiente, el método `GetSwitchMappings` permite que los argumentos de línea de comandos usen un prefijo de clave de un solo guion (`-`) y evita prefijos de subclave iniciales.

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

Sin proporcionar argumentos de línea de comandos, el diccionario proporcionado para `AddInMemoryCollection` establece los valores de configuración. Ejecute la aplicación con el comando siguiente:

```console
dotnet run
```

En la ventana de consola se muestra lo siguiente:

```console
MachineName: RickPC
Left: 1980
```

Use lo siguiente para pasar valores de configuración:

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

En la ventana de consola se muestra lo siguiente:

```console
MachineName: DahliaPC
Left: 1984
```

Después de crear el diccionario de asignaciones de modificador, contiene los datos que se muestran en la tabla siguiente.

| Key            | Valor                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

Para mostrar la conmutación de claves mediante el diccionario, ejecute el comando siguiente:

```console
dotnet run -MachineName=ChadPC -Left=1988
```

Se intercambian las claves de la línea de comandos. En la ventana de consola se muestran los valores de configuración de `Profile:MachineName` y `App:MainWindow:Left`:

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a>El archivo web.config

Un archivo *web.config* es necesario cuando la aplicación se hospeda en IIS o IIS Express. La configuración de *web.config* habilita el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) para que inicie la aplicación y configure otros módulos y valores de configuración de IIS. Si el archivo *web.config* no está presente y el archivo de proyecto incluye `<Project Sdk="Microsoft.NET.Sdk.Web">`, al publicar el proyecto se crea un archivo *web.config* en la salida publicada (la carpeta de *publicación*). Para más información, vea [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig) (Hospedar ASP.NET Core en Windows con IIS).

## <a name="additional-notes"></a>Notas adicionales

* La inserción de dependencias (DI) no se establece hasta que se invoca `ConfigureServices`.
* El sistema de configuración no es compatible con DI.
* `IConfiguration` tiene dos especializaciones:
  * `IConfigurationRoot` Se usa para el nodo raíz. Puede desencadenar una recarga.
  * `IConfigurationSection` Representa una sección de valores de configuración. Los métodos `GetSection` y `GetChildren` devuelven un elemento `IConfigurationSection`.

## <a name="additional-resources"></a>Recursos adicionales

* [Opciones](xref:fundamentals/configuration/options)
* [Trabajar con varios entornos](xref:fundamentals/environments)
* [Almacenamiento seguro de secretos de aplicación durante el desarrollo](xref:security/app-secrets)
* [Hospedar en ASP.NET Core](xref:fundamentals/hosting)
* [Inserción de dependencias](xref:fundamentals/dependency-injection)
* [Proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration)
