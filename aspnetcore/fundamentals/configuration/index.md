---
title: Configuración en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo usar la API de configuración para una aplicación ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/11/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 52450460af59a8da4fc6b24e3aa83d55385e8c77
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750957"
---
# <a name="configuration-in-aspnet-core"></a>Configuración en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

La configuración de la aplicación en ASP.NET Core se basa en pares clave-valor establecidos por *proveedores de configuración*. Los proveedores de configuración leen los datos de configuración en los pares clave-valor de distintos orígenes de configuración:

* Azure Key Vault
* Argumentos de la línea de comandos
* Proveedores personalizados (instalados o creados)
* Archivos de directorio
* Variables de entorno
* Objetos de .NET en memoria
* Archivos de configuración

El *patrón de opciones* es una extensión de los conceptos de configuración que se describen en este tema. Las opciones usan clases para representar grupos de configuraciones relacionadas. Para más información sobre cómo usar el patrón de opciones, consulte <xref:fundamentals/configuration/options>.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

Estos tres paquetes están incluidos en el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

## <a name="host-vs-app-configuration"></a>Configuración de host frente a configuración de aplicación

Antes de configurar e iniciar la aplicación, se configura e inicia un *host*. El host es responsable de la administración del inicio y la duración de la aplicación. Tanto la aplicación como el host se configuran mediante los proveedores de configuración que se describen en este tema. Los pares clave-valor de la configuración de host se vuelven parte de la configuración global de la aplicación. Para obtener más información sobre cómo se usan los proveedores de configuración cuando se compila el host y cómo afectan los orígenes de configuración a la configuración del host, consulte [El host](xref:fundamentals/index#host).

## <a name="default-configuration"></a>Configuración predeterminada

Las aplicaciones web basadas en las plantillas de [dotnet new](/dotnet/core/tools/dotnet-new) de ASP.NET Core llaman a <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> al crear un host. <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> proporciona la configuración predeterminada para la aplicación en el orden siguiente:

* La configuración del host la proporcionan los siguientes elementos:
  * Variables de entorno prefijadas con `ASPNETCORE_` (por ejemplo, `ASPNETCORE_ENVIRONMENT`) con el [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider). El prefijo (`ASPNETCORE_`) se quita cuando se cargan los pares clave-valor de configuración.
  * Argumentos de la línea de comandos con el [Proveedor de configuración de línea de comandos](#command-line-configuration-provider).
* La configuración de la aplicación la proporcionan los siguientes elementos:
  * *appsettings.json* con el [Proveedor de configuración de archivo](#file-configuration-provider).
  * *appsettings.{Entorno}.json* con el [Proveedor de configuración de archivo](#file-configuration-provider).
  * [Administrador de secretos](xref:security/app-secrets) cuando la aplicación se ejecuta en el entorno `Development` por medio del ensamblado de entrada.
  * Variables de entorno con el [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider). Si se usa un prefijo personalizado (por ejemplo, `PREFIX_` con `.AddEnvironmentVariables(prefix: "PREFIX_")`), el prefijo se quita cuando se cargan los pares clave y valor de configuración.
  * Argumentos de la línea de comandos con el [Proveedor de configuración de línea de comandos](#command-line-configuration-provider).

Los proveedores de configuración se explican posteriormente en este tema. Para obtener más información sobre el host y <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, vea <xref:fundamentals/host/web-host#set-up-a-host>.

## <a name="security"></a>Seguridad

Adopte estos procedimientos recomendados:

* Nunca almacene contraseñas u otros datos confidenciales en el código del proveedor de configuración o en archivos de configuración de texto sin formato.
* No use secretos de producción en los entornos de desarrollo o pruebas.
* Especifique los secretos fuera del proyecto para que no se confirmen en un repositorio de código fuente de manera accidental.

Obtenga más información sobre [cómo usar varios entornos](xref:fundamentals/environments) y cómo administrar el [almacenamiento seguro de secretos de aplicación en el desarrollo con el Administrador de secretos](xref:security/app-secrets) (incluye consejos sobre el uso de variables de entorno para almacenar información confidencial). El Administrador de secretos usa el proveedor de configuración de archivo para almacenar secretos de usuario en un archivo JSON del sistema local. El proveedor de configuración de archivo se describe más adelante en este tema.

[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) es una opción para el almacenamiento seguro de los secretos de aplicación. Para obtener más información, vea <xref:security/key-vault-configuration>.

## <a name="hierarchical-configuration-data"></a>Datos de configuración jerárquica

La API de configuración es capaz de mantener los datos de configuración jerárquica al disminuir los datos jerárquicos mediante el uso de un delimitador en las claves de configuración.

En el siguiente archivo JSON, hay cuatro claves en una estructura jerárquica de dos secciones:

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

Cuando el archivo se lee en la configuración, se crean claves únicas para mantener la estructura de datos jerárquicos original del origen de configuración. Las secciones y claves se disminuyen con el uso de dos puntos (`:`) para mantener la estructura original:

* section0:key0
* section0:key1
* section1:key0
* section1:key1

Los métodos <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> y <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> están disponibles para aislar las secciones y los elementos secundarios de una sección en los datos de configuración. Estos métodos se describen más adelante en la sección [GetSection, GetChildren y Exists](#getsection-getchildren-and-exists). `GetSection` está en el paquete [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

## <a name="conventions"></a>Convenciones

Al iniciar la aplicación, los orígenes de configuración se leen en el orden en que se especifican sus proveedores de configuración.

Los proveedores de configuración de archivo tienen la capacidad de recargar la configuración cuando se modifica un archivo de configuración subyacente después del inicio de la aplicación. El proveedor de configuración de archivo se describe más adelante en este tema.

<xref:Microsoft.Extensions.Configuration.IConfiguration> está disponible en el contenedor de [inserción de dependencias (DI)](xref:fundamentals/dependency-injection) de la aplicación. <xref:Microsoft.Extensions.Configuration.IConfiguration> se puede insertar en <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> de Razor Pages para obtener la configuración de la clase:

```csharp
// using Microsoft.Extensions.Configuration;

public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    // The _config local variable is used to obtain configuration 
    // throughout the class.
}
```

Los proveedores de configuración no pueden usar la inserción de dependencias, porque no está disponible cuando el host los configura.

Las claves de configuración adoptan las convenciones siguientes:

* Las claves no distinguen mayúsculas de minúsculas. Por ejemplo, `ConnectionString` y `connectionstring` se consideran claves equivalente.
* Si los mismos o distintos proveedores de configuración establecen un valor para la misma clave, el último valor establecido en la clave es el valor que se usa.
* Claves jerárquicas
  * Dentro de la API de configuración, un separador de dos puntos (`:`) funciona en todas las plataformas.
  * En las variables de entorno, puede que un separador de dos puntos no funcione en todas las plataformas. Un guion bajo doble (`__`) es compatible con todas las plataformas y se convierte en un signo de dos puntos.
  * En Azure Key Vault, las claves jerárquicas usan `--` (dos guiones) como separador. Debe proporcionar código para reemplazar los guiones por dos puntos cuando los secretos se cargan en la configuración de la aplicación.
* <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> admite enlazar matrices a objetos con los índices de matriz en las claves de configuración. El enlace de matriz se describe en la sección [Enlace de una matriz a una clase](#bind-an-array-to-a-class).

Los valores de configuración adoptan las convenciones siguientes:

* Los valores son cadenas.
* Los valores NULL no se pueden almacenar en la configuración ni enlazar a los objetos.

## <a name="providers"></a>Proveedores

La siguiente tabla muestra los proveedores de configuración disponibles para las aplicaciones de ASP.NET Core.

| Proveedor | Proporciona la configuración de&hellip; |
| -------- | ----------------------------------- |
| [Proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration) (temas de *Seguridad*) | Azure Key Vault |
| [Proveedor de configuración de línea de comandos](#command-line-configuration-provider) | Parámetros de la línea de comandos |
| [Proveedor de configuración personalizada](#custom-configuration-provider) | Origen personalizado |
| [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider) | Variables de entorno |
| [Proveedor de configuración de archivo](#file-configuration-provider) | Archivos (INI, JSON, XML) |
| [Proveedor de configuración de clave por archivo](#key-per-file-configuration-provider) | Archivos de directorio |
| [Proveedor de configuración de memoria](#memory-configuration-provider) | Colecciones en memoria |
| [Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (temas de *Seguridad*) | Archivo en el directorio del perfil de usuario |

Los orígenes de configuración se leen en el orden en que se especifican sus proveedores de configuración en el inicio. En este tema, los proveedores de configuración se describen en orden alfabético y no en el orden en que el código podría organizarlos. Ordene los proveedores de configuración en el código para cumplir con sus prioridades relacionadas con los orígenes de configuración subyacentes.

Esta es una secuencia típica de proveedores de configuración:

1. Archivos (*appsettings.json*, *appsettings.{Environment}.json*, donde `{Environment}` es el entorno de hospedaje actual de la aplicación)
1. [Azure Key Vault](xref:security/key-vault-configuration)
1. [Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (solo en el entorno de desarrollo)
1. Variables de entorno
1. Argumentos de la línea de comandos

Una práctica común es colocar el proveedor de configuración de línea de comandos al final de en una serie de proveedores para permitir que los argumentos de la línea de comandos invaliden la configuración que establecieron los demás proveedores.

Esta secuencia de los proveedores se aplica al inicializar un nuevo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>. Para más información, consulte [Host web: Configuración de un host](xref:fundamentals/host/web-host#set-up-a-host).

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar los proveedores de configuración de la aplicación además de aquellos que <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> agrega automáticamente:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

La configuración proporcionada a la aplicación en <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> está disponible durante el inicio de la aplicación, incluido `Startup.ConfigureServices`. Para obtener más información, vea la sección [Acceso a la configuración durante el inicio](#access-configuration-during-startup).

## <a name="command-line-configuration-provider"></a>Proveedor de configuración de línea de comandos

<xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> carga la configuración de pares clave-valor de argumento de la línea de comandos en tiempo de ejecución.

Para activar la configuración de línea de comandos, se llama al método de extensión <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

`AddCommandLine` se llama automáticamente cuando se inicializa un nuevo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>. Para más información, consulte [Host web: Configuración de un host](xref:fundamentals/host/web-host#set-up-a-host).

`CreateDefaultBuilder` también carga:

* Configuración opcional de *appsettings.json* y *appsettings.{Environment}.json*.
* [Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (en el entorno de desarrollo).
* Variables de entorno.

`CreateDefaultBuilder` agrega el proveedor de configuración de línea de comandos al final. Los argumentos de la línea de comandos que se pasan en tiempo de ejecución invalidan la configuración establecida por los otros proveedores.

`CreateDefaultBuilder` actúa cuando se construye el host. Por tanto, la configuración de línea de comandos activada por `CreateDefaultBuilder` puede afectar la manera en que se configura el host.

Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar la configuración de la aplicación.

Ya se ha llamado a `AddCommandLine` por parte de `CreateDefaultBuilder`. Si tiene que proporcionar la configuración de la aplicación y mantener la posibilidad de invalidar dicha configuración con argumentos de línea de comandos, llame a los proveedores adicionales de la aplicación en <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> y llame por último a `AddCommandLine`.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call other providers here and call AddCommandLine last.
                config.AddCommandLine(args);
            })
            .UseStartup<Startup>();
}
```

Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:

```csharp
var config = new ConfigurationBuilder()
    // Call additional providers here as needed.
    // Call AddCommandLine last to allow arguments to override other configuration.
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

**Ejemplo**

La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.

1. Abra un símbolo del sistema en el directorio del proyecto.
1. Proporcione un argumento de la línea de comandos para el comando `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.
1. Una vez que se ejecuta la aplicación, abra un explorador a la aplicación en `http://localhost:5000`.
1. Observe que la salida contiene el par clave-valor para el argumento de línea de comandos de configuración proporcionado a `dotnet run`.

### <a name="arguments"></a>Argumentos

El valor debe seguir a un signo igual (`=`) o la clave debe tener un prefijo (`--` o `/`) cuando el valor siga a un espacio. El valor puede ser NULL si se usa un signo igual (por ejemplo, `CommandLineKey=`).

| Prefijo de la clave               | Ejemplo                                                |
| ------------------------ | ------------------------------------------------------ |
| Sin prefijo                | `CommandLineKey1=value1`                               |
| Dos guiones (`--`)        | `--CommandLineKey2=value2`, `--CommandLineKey2 value2` |
| Barra diagonal (`/`)      | `/CommandLineKey3=value3`, `/CommandLineKey3 value3`   |

Dentro del mismo comando, no mezcle pares clave-valor de argumento de la línea de comandos que usan un signo igual con pares de clave-valor que usan un espacio.

Comandos de ejemplo:

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a>Asignaciones de modificador

Las asignaciones de modificador admiten la lógica de sustitución de nombres de clave. Cuando crea manualmente la configuración con <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, puede proporcionar un diccionario de reemplazos de modificador al método <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.

Cuando se usa el diccionario de asignaciones de modificador, se comprueba en el diccionario si una clave coincide con la clave proporcionada por un argumento de línea de comandos. Si la clave de la línea de comandos se encuentra en el diccionario, se devuelve el valor del diccionario (el reemplazo de la clave) para establecer el par clave-valor en la configuración de la aplicación. Se requiere una asignación de conmutador para cualquier clave de línea de comandos precedida por un solo guion (`-`).

Reglas de clave del diccionario de asignaciones de modificador:

* Los modificadores deben empezar por un guion (`-`) o guion doble (`--`).
* El diccionario de asignaciones de modificador no debe contener claves duplicadas.

Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar la configuración de la aplicación:

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _switchMappings = 
        new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    // Do not pass the args to CreateDefaultBuilder
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder()
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddCommandLine(args, _switchMappings);
            })
            .UseStartup<Startup>();
}
```

Como se muestra en el ejemplo anterior, la llamada a `CreateDefaultBuilder` no debería pasar argumentos cuando se usan las asignaciones de modificador. La llamada de `AddCommandLine` del método `CreateDefaultBuilder` no incluye modificadores asignados y no hay forma de pasar el diccionario de asignación de modificador a `CreateDefaultBuilder`. Si los argumentos incluyen un conmutador asignado y se pasan a `CreateDefaultBuilder`, el proveedor de `AddCommandLine` no se puede inicializar con una <xref:System.FormatException>. La solución no es pasar los argumentos de `CreateDefaultBuilder`, sino que permitir que el método `AddCommandLine` del método `ConfigurationBuilder` procese tanto los argumentos como el diccionario de asignación de modificador.

Después de crear el diccionario de asignaciones de modificador, contiene los datos que se muestran en la tabla siguiente.

| Key       | Valor             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

Si las claves de asignación de conmutador se usan al iniciar la aplicación, la configuración recibe el valor de configuración en la clave que el diccionario suministra:

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

Una vez que se ejecuta el comando anterior, la configuración contiene los valores que se muestran en la tabla siguiente.

| Key               | Valor    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a>Proveedor de configuración de variables de entorno

<xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> carga la configuración de pares clave-valor de variables de entorno en tiempo de ejecución.

Para activar la configuración de variables de entorno, llame al método de extensión <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

[Azure App Service](https://azure.microsoft.com/services/app-service/) permite establecer las variables de entorno en Azure Portal que pueden invalidar la configuración de la aplicación mediante el proveedor de configuración de variables de entorno. Para más información, consulte [Aplicaciones de Azure: Invalidación de la configuración de la aplicación mediante Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).

Se llama automáticamente a `AddEnvironmentVariables` para las variables de entorno con el prefijo `ASPNETCORE_` al inicializar un nuevo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>. Para más información, consulte [Host web: Configuración de un host](xref:fundamentals/host/web-host#set-up-a-host).

`CreateDefaultBuilder` también carga:

* Configuración de la aplicación desde variables de entorno sin prefijo mediante la llamada a `AddEnvironmentVariables` sin prefijo.
* Configuración opcional de *appsettings.json* y *appsettings.{Environment}.json*.
* [Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (en el entorno de desarrollo).
* Argumentos de la línea de comandos.

El proveedor de configuración de variables de entorno se llama una vez establecida la configuración desde los secretos de usuario y los archivos *appsettings*. Llamar al proveedor en esta posición permite que la lectura de las variables de entorno en tiempo de ejecución invaliden la configuración establecida por los secretos de usuario y los archivos *appsettings*.

Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar la configuración de la aplicación.

Si tiene que proporcionar la configuración de la aplicación desde variables de entorno adicionales, llame a los proveedores adicionales de la aplicación en <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> y llame a `AddEnvironmentVariables` con el prefijo.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call additional providers here as needed.
                // Call AddEnvironmentVariables last if you need to allow environment
                // variables to override values from other providers.
                config.AddEnvironmentVariables(prefix: "PREFIX_");
            })
            .UseStartup<Startup>();
}
```

Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

**Ejemplo**

La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a `AddEnvironmentVariables`.

1. Ejecute la aplicación de ejemplo. Abra un explorador a la aplicación en `http://localhost:5000`.
1. Observe que el resultado contiene el par clave y valor para la variable de entorno `ENVIRONMENT`. El valor refleja el entorno en el que se ejecuta la aplicación, habitualmente `Development` cuando se ejecuta de manera local.

Para que la lista de variables de entorno que muestra la aplicación sea breve, la aplicación filtra las variables de entorno que comienzan con lo siguiente:

* ASPNETCORE_
* urls
* Registro
* ENVIRONMENT
* contentRoot
* AllowedHosts
* applicationName
* CommandLine

Si quiere exponer todas las variables de entorno disponibles para la aplicación, cambie `FilteredConfiguration` en *Pages/Index.cshtml.cs* a lo siguiente:

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a>Prefijos

Las variables de entorno que se cargan en la configuración de la aplicación se filtran cuando se proporciona un prefijo para el método `AddEnvironmentVariables`. Por ejemplo, para filtrar las variables de entorno en el prefijo `CUSTOM_`, suministre el prefijo para el proveedor de configuración:

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

El prefijo se quita cuando se crean los pares clave-valor de configuración.

El método de conveniencia estático `CreateDefaultBuilder` crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> para establecer el host de la aplicación. Cuando se crea `WebHostBuilder`, encuentra su configuración de host en variables de entorno con el prefijo `ASPNETCORE_`.

**Prefijos de cadena de conexión**

La API de configuración tiene reglas de procesamiento especial para cuatro variables de entorno de cadena de conexión relacionadas con la configuración de las cadenas de conexión de Azure para el entorno de la aplicación. Las variables de entorno con los prefijos que se muestran en la tabla se cargan en la aplicación si no se proporciona ningún prefijo a `AddEnvironmentVariables`.

| Prefijo de cadena de conexión | Proveedor |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | Proveedor personalizado |
| `MYSQLCONNSTR_` | [MySQL](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [SQL Server](https://www.microsoft.com/sql-server/) |

Cuando una variable de entorno se detecta y carga en la configuración con cualquiera de los cuatro prefijos que se muestran en la tabla:

* La clave de configuración se crea al quitar el prefijo de la variable de entorno y al agregar una sección de clave de configuración (`ConnectionStrings`).
* Se crea un nuevo par clave-valor de configuración que representa el proveedor de conexión de base de datos (excepto para `CUSTOMCONNSTR_`, que no tiene ningún proveedor indicado).

| Clave de variable de entorno | Clave de configuración convertida | Entrada de configuración del proveedor                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | Entrada de configuración no creada.                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | Clave: `ConnectionStrings:<KEY>_ProviderName`:<br>Valor: `MySql.Data.MySqlClient` |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | Clave: `ConnectionStrings:<KEY>_ProviderName`:<br>Valor: `System.Data.SqlClient`  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | Clave: `ConnectionStrings:<KEY>_ProviderName`:<br>Valor: `System.Data.SqlClient`  |

## <a name="file-configuration-provider"></a>Proveedor de configuración de archivo

<xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> es la clase base para cargar la configuración del sistema de archivos. Los proveedores de configuración siguientes están dedicados a determinados tipos de archivo:

* [Proveedor de configuración INI](#ini-configuration-provider)
* [Proveedor de configuración JSON](#json-configuration-provider)
* [Proveedor de configuración XML](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a>Proveedor de configuración de INI

<xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> carga la configuración desde pares clave-valor de archivo INI en tiempo de ejecución.

Para activar la configuración de archivo INI, llame al método de extensión <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Los dos puntos se pueden usar como un delimitador de sección en la configuración de archivo INI.

Las sobrecargas permiten especificar:

* Si el archivo es opcional.
* Si la configuración se recarga si el archivo cambia.
* <xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.

Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar la configuración de la aplicación:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddIniFile("config.ini", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>. `SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddIniFile("config.ini", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>. `SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Un ejemplo genérico de un archivo de configuración INI:

```ini
[section0]
key0=value
key1=value

[section1]
subsection:key=value

[section2:subsection0]
key=value

[section2:subsection1]
key=value
```

El archivo de configuración anterior carga las siguientes claves con `value`:

* section0:key0
* section0:key1
* section1:subsection:key
* section2:subsection0:key
* section2:subsection1:key

### <a name="json-configuration-provider"></a>Proveedor de configuración JSON

<xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> carga la configuración desde pares clave-valor de archivos JSON en tiempo de ejecución.

Para activar la configuración de archivo JSON, llame al método de extensión <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Las sobrecargas permiten especificar:

* Si el archivo es opcional.
* Si la configuración se recarga si el archivo cambia.
* <xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.

`AddJsonFile` se llama automáticamente dos veces al inicializar un nuevo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>. Se llama al método para cargar la configuración desde:

* *appsettings.json* &ndash; Este archivo se lee primero. La versión del entorno del archivo puede invalidar los valores que proporciona el archivo *appsettings.json*.
* *appsettings.{Environment}.json* &ndash; La versión del entorno del archivo se carga en función de [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).

Para más información, consulte [Host web: Configuración de un host](xref:fundamentals/host/web-host#set-up-a-host).

`CreateDefaultBuilder` también carga:

* Variables de entorno.
* [Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (en el entorno de desarrollo).
* Argumentos de la línea de comandos.

El proveedor de configuración de JSON se establece en primer lugar. Por tanto, los secretos de usuario, las variables de entorno y los argumentos de la línea de comandos invalidan la configuración establecida por los archivos *appsettings*.

Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> al crear el host para especificar la configuración de la aplicación para archivos distintos de *appsettings.json* y *appsettings.{Environment}.json*:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddJsonFile("config.json", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>. `SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("config.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>. `SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

**Ejemplo**

La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a `AddJsonFile`. La configuración se carga desde *appsettings.json* y *appsettings.{Environment}.json*.

1. Ejecute la aplicación de ejemplo. Abra un explorador a la aplicación en `http://localhost:5000`.
1. Observe que la salida contiene los pares clave-valor para la configuración que se muestra en la tabla según el entorno. Las claves de configuración de registro usan dos puntos (`:`) como separador jerárquico.

| Key                        | Valor de desarrollo | Valor de producción |
| -------------------------- | :---------------: | :--------------: |
| Logging:LogLevel:System    | Información       | Información      |
| Logging:LogLevel:Microsoft | Información       | Información      |
| Logging:LogLevel:Default   | Depuración             | Error            |
| AllowedHosts               | *                 | *                |

### <a name="xml-configuration-provider"></a>Proveedor de configuración XML

<xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> carga la configuración desde pares clave-valor de archivo XML en tiempo de ejecución.

Para activar la configuración de archivo XML, llame al método de extensión <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Las sobrecargas permiten especificar:

* Si el archivo es opcional.
* Si la configuración se recarga si el archivo cambia.
* <xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.

El nodo raíz del archivo de configuración se omite cuando se crean los pares clave-valor de configuración. No especifique una definición de tipo de documento (DTD) ni el espacio de nombres en el archivo.

Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar la configuración de la aplicación:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddXmlFile("config.xml", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>. `SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>. `SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Los archivos de configuración XML pueden usar distintos nombres de elemento para las secciones repetidas:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section0>
    <key0>value</key0>
    <key1>value</key1>
  </section0>
  <section1>
    <key0>value</key0>
    <key1>value</key1>
  </section1>
</configuration>
```

El archivo de configuración anterior carga las siguientes claves con `value`:

* section0:key0
* section0:key1
* section1:key0
* section1:key1

Los elementos de repetición que usan el mismo nombre de elemento funcionan si el atributo `name` se usa para distinguir los elementos:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section name="section0">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
  <section name="section1">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
</configuration>
```

El archivo de configuración anterior carga las siguientes claves con `value`:

* section:section0:key:key0
* section:section0:key:key1
* section:section1:key:key0
* section:section1:key:key1

Los atributos se pueden usar para suministrar valores:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

El archivo de configuración anterior carga las siguientes claves con `value`:

* key:attribute
* section:key:attribute

## <a name="key-per-file-configuration-provider"></a>Proveedor de configuración de clave por archivo

<xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> usa los archivos de un directorio como pares clave-valor de configuración. La clave es el nombre de archivo. El valor contiene el contenido del archivo. El proveedor de configuración de clave por archivo se usa en escenarios de hospedaje de Docker.

Para activar la configuración de clave por archivo, llame al método de extensión <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>. La ruta de acceso `directoryPath` a los archivos debe ser una ruta de acceso absoluta.

Las sobrecargas permiten especificar:

* Un delegado `Action<KeyPerFileConfigurationSource>` que configura el origen.
* Si el directorio es opcional y la ruta de acceso al directorio.

El carácter de subrayado doble (`__`) se usa como delimitador de claves de configuración en los nombres de archivo. Por ejemplo, el nombre de archivo `Logging__LogLevel__System` genera la clave de configuración `Logging:LogLevel:System`.

Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar la configuración de la aplicación:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
                config.AddKeyPerFile(directoryPath: path, optional: true);
            })
            .UseStartup<Startup>();
}
```

Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>. `SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:

```csharp
var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
var config = new ConfigurationBuilder()
    .AddKeyPerFile(directoryPath: path, optional: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

## <a name="memory-configuration-provider"></a>Proveedor de configuración de memoria

<xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> usa una colección en memoria como pares clave-valor de configuración.

Para activar la configuración de colección en memoria, llame al método de extensión <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

El proveedor de configuración se puede inicializar con un `IEnumerable<KeyValuePair<String,String>>`.

Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar la configuración de la aplicación:

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _dict = 
        new Dictionary<string, string>
        {
            {"MemoryCollectionKey1", "value1"},
            {"MemoryCollectionKey2", "value2"}
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddInMemoryCollection(_dict);
            })
            .UseStartup<Startup>();
}
```

Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:

```csharp
var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

var config = new ConfigurationBuilder()
    .AddInMemoryCollection(dict)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

## <a name="getvalue"></a>GetValue

[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrae un valor de la configuración con una clave especificada y lo convierte al tipo especificado. Una sobrecarga permite proporcionar un valor predeterminado si no se encuentra la clave.

En el ejemplo siguiente:

* Se extrae el valor de cadena de la configuración con la clave `NumberKey`. Si `NumberKey` no se encuentra en las claves de configuración, se usa el valor predeterminado de `99`.
* Se escribe el valor como `int`.
* Se almacena el valor en la propiedad `NumberConfig` para usarlo en la página.

```csharp
// using Microsoft.Extensions.Configuration;

public class IndexModel : PageModel
{
    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    public int NumberConfig { get; private set; }

    public void OnGet()
    {
        NumberConfig = _config.GetValue<int>("NumberKey", 99);
    }
}
```

## <a name="getsection-getchildren-and-exists"></a>GetSection, GetChildren y Exists

Para los ejemplos siguientes, considere el siguiente archivo JSON. Se encuentran cuatro claves en dos secciones, una de las cuales incluye un par de subsecciones:

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  },
  "section2": {
    "subsection0" : {
      "key0": "value",
      "key1": "value"
    },
    "subsection1" : {
      "key0": "value",
      "key1": "value"
    }
  }
}
```

Cuando el archivo se lee en la configuración, se crean las siguientes claves jerárquicas únicas para contener los valores de configuración:

* section0:key0
* section0:key1
* section1:key0
* section1:key1
* section2:subsection0:key0
* section2:subsection0:key1
* section2:subsection1:key0
* section2:subsection1:key1

### <a name="getsection"></a>GetSection

[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrae una subsección de la configuración con la clave de subsección especificada. `GetSection` está en el paquete [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Para devolver un <xref:Microsoft.Extensions.Configuration.IConfigurationSection> que contiene los pares clave-valor en `section1`, llame a `GetSection` y suministre el nombre de la sección:

```csharp
var configSection = _config.GetSection("section1");
```

`configSection` no tiene ningún valor, solo una clave y una ruta de acceso.

De forma similar, para obtener los valores de las claves de `section2:subsection0`, llame a `GetSection` y suministre la ruta de acceso a la sección:

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

`GetSection` nunca devuelve `null`. Si no se encuentra una sección que coincida, se devuelve una `IConfigurationSection` vacía.

Cuando `GetSection` devuelve una sección coincidente, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> no se rellena. Se devuelven <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> y <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> si la sección existe.

### <a name="getchildren"></a>GetChildren

Una llamada a [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) en `section2` obtiene una `IEnumerable<IConfigurationSection>` que incluye:

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a>Existe

Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) para determinar si existe una sección de configuración:

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

Dados los datos de ejemplo, `sectionExists` es `false` porque no hay una sección `section2:subsection2` en los datos de configuración.

## <a name="bind-to-a-class"></a>Enlace a una clase

La configuración se puede enlazar a clases que representan grupos de configuraciones relacionadas a través del *patrón de opciones*. Para obtener más información, vea <xref:fundamentals/configuration/options>.

Los valores de configuración se devuelven como cadenas, pero llamar a <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> permite la construcción de objetos [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object). `Bind` está en el paquete [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

La aplicación de ejemplo contiene un modelo `Starship` (*Models/Starship.cs*):

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

La sección `starship` del archivo *starship.json* crea la configuración cuando la aplicación de ejemplo usa el proveedor de configuración JSON para cargar la configuración:

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

Se crean los siguientes pares clave-valor de configuración:

| Key                   | Valor                                             |
| --------------------- | ------------------------------------------------- |
| starship:name         | USS Enterprise                                    |
| starship:registry     | NCC-1701                                          |
| starship:class        | Constitution                                      |
| starship:length       | 304.8                                             |
| starship:commissioned | False                                             |
| trademark             | Paramount Pictures Corp. http://www.paramount.com |

La aplicación de ejemplo llama a `GetSection` con la clave `starship`. Los pares clave-valor `starship` están aislados. El método `Bind` se llama en la subsección que pasa una instancia de la clase `Starship`. Después de enlazar los valores de instancia, la instancia está asignada a una propiedad para la representación:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

`GetSection` está en el paquete [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

## <a name="bind-to-an-object-graph"></a>Enlazar a un gráfico de objetos

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> es capaz de enlazar todo un gráfico de objetos POCO. `Bind` está en el paquete [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

El ejemplo contiene un modelo `TvShow` cuyo gráfico de objetos incluye las clases `Metadata` y `Actors` (*Models/TvShow.cs*):

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

La aplicación de ejemplo tiene un archivo *tvshow.xml* que contiene los datos de configuración:

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

Configuración está enlazada a todo el gráfico de objetos `TvShow`con el método `Bind`. La instancia enlazada se asigna a una propiedad para la representación:

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) enlaza y devuelve el tipo especificado. `Get<T>` es más conveniente que usar `Bind`. El código siguiente muestra cómo usar `Get<T>` con el ejemplo anterior, lo que permite que la instancia enlazada se asigne directamente a la propiedad que se usa para la representación:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> está en el paquete [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). `Get<T>` está disponible en ASP.NET Core 1.1 o versiones posteriores. `GetSection` está en el paquete [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

## <a name="bind-an-array-to-a-class"></a>Enlace de una matriz a una clase

*La aplicación de ejemplo muestra los conceptos que se explican en esta sección.*

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> admite enlazar matrices a objetos con los índices de matriz en las claves de configuración. Cualquier formato de matriz que expone un segmento de clave numérica (`:0:`, `:1:`, &hellip; `:{n}:`) es capaz de enlazar una matriz a una matriz de clase POCO. "Bind" está en el paquete [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

> [!NOTE]
> El enlace se proporciona por convención. Los proveedores de configuración personalizados no son necesarios para implementar el enlace de matriz.

**Procesamiento de matriz en memoria**

Considere los valores y las claves de configuración que se muestran en esta tabla.

| Key             | Valor  |
| :-------------: | :----: |
| array:entries:0 | value0 |
| array:entries:1 | value1 |
| array:entries:2 | value2 |
| array:entries:4 | value4 |
| array:entries:5 | value5 |

Estas claves y valores se cargan en la aplicación de ejemplo a través del proveedor de configuración de memoria:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

La matriz omite un valor de índice &num;3. El enlazador de configuración no es capaz de enlazar los valores NULL ni de crear entradas NULL en los objetos enlazados, lo que queda claro cuando se muestra el resultado de enlazar esta matriz a un objeto.

En la aplicación de ejemplo, hay disponible una clase POCO para almacenar los datos de configuración enlazados:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

Los datos de configuración están enlazados al objeto:

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

`GetSection` está en el paquete [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

También se puede usar la sintaxis de [ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*), lo que genera un código más compacto:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

El objeto enlazado, una instancia de `ArrayExample`, recibe los datos de la matriz desde la configuración.

| Índice de `ArrayExample.Entries` | Valor de `ArrayExample.Entries` |
| :--------------------------: | :--------------------------: |
| 0                            | value0                       |
| 1                            | value1                       |
| 2                            | value2                       |
| 3                            | value4                       |
| 4                            | value5                       |

El índice &num;3 en el objeto enlazado contiene los datos de configuración para la clave de configuración `array:4` y su valor de `value4`. Cuando se enlazan datos de configuración que contienen una matriz, los índices de la matriz en las claves de configuración solo se usan para iterar los datos de configuración al crear el objeto. No se puede conservar un valor NULL en los datos de configuración y no se crea una entrada con valores NULL en un objeto enlazado cuando una matriz en las claves de configuración omite uno o más índices.

El elemento de configuración omitido para el índice &num;3 se puede proporcionar antes del enlace a la instancia `ArrayExample` por cualquier proveedor de configuración que genera el par clave-valor correcto en la configuración. Si el ejemplo incluía un proveedor de configuración JSON adicional con el par clave-valor omitido, `ArrayExample.Entries` coincide con la matriz de configuración completa:

*missing_value.json*:

```json
{
  "array:entries:3": "value3"
}
```

En <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

El par clave-valor que se muestra en la tabla se carga en la configuración.

| Key             | Valor  |
| :-------------: | :----: |
| array:entries:3 | value3 |

Si la instancia de clase `ArrayExample` se enlaza después de que el proveedor de configuración JSON incluye la entrada para el índice &num;3, la matriz `ArrayExample.Entries` incluye el valor.

| Índice de `ArrayExample.Entries` | Valor de `ArrayExample.Entries` |
| :--------------------------: | :--------------------------: |
| 0                            | value0                       |
| 1                            | value1                       |
| 2                            | value2                       |
| 3                            | value3                       |
| 4                            | value4                       |
| 5                            | value5                       |

**Procesamiento de matriz JSON**

Si un archivo JSON contiene una matriz, se crean claves de configuración para los elementos de la matriz con un índice de sección basado en cero. En el siguiente archivo de configuración, `subsection` es una matriz:

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

El proveedor de configuración JSON lee los datos de configuración en los siguientes pares clave-valor:

| Key                     | Valor  |
| ----------------------- | :----: |
| json_array:key          | valueA |
| json_array:subsection:0 | valueB |
| json_array:subsection:1 | valueC |
| json_array:subsection:2 | valueD |

En la aplicación de ejemplo, la clase POCO siguiente está disponible para enlazar los pares clave-valor de configuración:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

Después del enlace, `JsonArrayExample.Key` contiene el valor `valueA`. Los valores de la subsección se almacenan en la propiedad de la matriz POCO, `Subsection`.

| Índice de `JsonArrayExample.Subsection` | Valor de `JsonArrayExample.Subsection` |
| :---------------------------------: | :---------------------------------: |
| 0                                   | valueB                              |
| 1                                   | valueC                              |
| 2                                   | valueD                              |

## <a name="custom-configuration-provider"></a>Proveedores de configuración personalizada

La aplicación de ejemplo muestra cómo crear un proveedor de configuración básica que lee los pares clave-valor de configuración desde una base de datos mediante [Entity Framework (EF)](/ef/core/).

El proveedor tiene las siguientes características:

* La base de datos en memoria de EF se usa para fines de demostración. Para usar una base de datos que requiere una cadena de conexión, implemente un `ConfigurationBuilder` secundario para suministrar la cadena de conexión desde otro proveedor de configuración.
* El proveedor lee una tabla de base de datos en la configuración en el inicio. El proveedor no consulta la base de datos por clave.
* La función de recarga en cambio no se implementa, por lo que actualizar la base de datos después del inicio de la aplicación no afecta a la configuración de la aplicación.

Defina una entidad `EFConfigurationValue` para almacenar los valores de configuración en la base de datos.

*Models/EFConfigurationValue.cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

Agregue un `EFConfigurationContext` para almacenar y tener acceso a los valores configurados.

*EFConfigurationProvider/EFConfigurationContext.cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

Cree una clase que implemente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.

*EFConfigurationProvider/EFConfigurationSource.cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

Cree el proveedor de configuración personalizado heredando de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>. El proveedor de configuración inicializa la base de datos cuando está vacía.

*EFConfigurationProvider/EFConfigurationProvider.cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

Un método de extensión `AddEFConfiguration` permite agregar el origen de configuración a un `ConfigurationBuilder`.

*Extensions/EntityFrameworkExtensions.cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

En el código siguiente se muestra cómo puede usar el `EFConfigurationProvider` personalizado en *Program.cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

## <a name="access-configuration-during-startup"></a>Acceso a la configuración durante el inicio

Inserte `IConfiguration` en el constructor `Startup` para acceder a los valores de configuración en `Startup.ConfigureServices`. Para acceder a la configuración en `Startup.Configure`, inserte `IConfiguration` directamente en el método o use la instancia desde el constructor:

```csharp
public class Startup
{
    private readonly IConfiguration _config;

    public Startup(IConfiguration config)
    {
        _config = config;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        var value = _config["key"];
    }

    public void Configure(IApplicationBuilder app, IConfiguration config)
    {
        var value = config["key"];
    }
}
```

Para ver un ejemplo de cómo acceder a la configuración mediante métodos de conveniencia de inicio, consulte [Inicio de la aplicación: Métodos de conveniencia](xref:fundamentals/startup#convenience-methods)

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a>Acceso a la configuración en una página de Razor Pages o en una vista de MVC.

Para obtener acceso a los valores de configuración en una página de Razor Pages o una vista de MVC, agregue una [directiva using](xref:mvc/views/razor#using) ([referencia de C#: directiva using](/dotnet/csharp/language-reference/keywords/using-directive)) para el [espacio de nombres Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) e inyecte <xref:Microsoft.Extensions.Configuration.IConfiguration> en la página o en la vista.

En una página de las páginas de Razor:

```cshtml
@page
@model IndexModel
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

En una vista de MVC:

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

## <a name="add-configuration-from-an-external-assembly"></a>Agregar configuración a partir de un ensamblado externo

Una implementación de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permite agregar mejoras a una aplicación al iniciarla a partir de un ensamblado externo fuera de la clase `Startup` de esta. Para obtener más información, vea <xref:fundamentals/configuration/platform-specific-configuration>.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/configuration/options>
* [Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/) (Profundización en la configuración de Microsoft)
