---
title: Configuración en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo usar la API de configuración para una aplicación ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/10/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: d0ef670aa0ac4960318f86ea7888b9eab71f17fd
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77171896"
---
# <a name="configuration-in-aspnet-core"></a>Configuración en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

La configuración de la aplicación en ASP.NET Core se basa en pares clave-valor establecidos por *proveedores de configuración*. Los proveedores de configuración leen los datos de configuración en los pares clave-valor de distintos orígenes de configuración:

* Azure Key Vault
* Configuración de aplicaciones de Azure
* Argumentos de la línea de comandos
* Proveedores personalizados (instalados o creados)
* Archivos de directorio
* Variables de entorno
* Objetos de .NET en memoria
* Archivos de configuración

Los paquetes de configuración para escenarios de proveedor de configuración común ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) se incluyen implícitamente en el marco.

Los ejemplos de código que siguen y en la aplicación de ejemplo utilizan el espacio de nombres <xref:Microsoft.Extensions.Configuration>:

```csharp
using Microsoft.Extensions.Configuration;
```

El *patrón de opciones* es una extensión de los conceptos de configuración que se describen en este tema. Las opciones usan clases para representar grupos de configuraciones relacionadas. Para obtener más información, vea <xref:fundamentals/configuration/options>.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="host-versus-app-configuration"></a>Configuración de host y de aplicación

Antes de configurar e iniciar la aplicación, se configura e inicia un *host*. El host es responsable de la administración del inicio y la duración de la aplicación. Tanto la aplicación como el host se configuran mediante los proveedores de configuración que se describen en este tema. Los pares clave-valor de configuración del host también se incluyen en la configuración de la aplicación. Para más información sobre cómo se usan los proveedores de configuración cuando se compila el host y cómo afectan los orígenes de configuración a la configuración del host, consulte <xref:fundamentals/index#host>.

## <a name="other-configuration"></a>Otra configuración

Este tema solo concierne a la *configuración de aplicaciones*. Otros aspectos de la ejecución y el hospedaje de aplicaciones ASP.NET Core se configuran mediante archivos de configuración que no se describen en este tema:

* *launch.json*/*launchSettings.json* son archivos de configuración de herramientas para el entorno de desarrollo, que se describe a continuación:
  * En <xref:fundamentals/environments#development>.
  * En el conjunto de documentación en el que se usan los archivos para configurar aplicaciones ASP.NET Core para escenarios de desarrollo.
* *web.config* es un archivo de configuración del servidor, que se describe en los temas siguientes:
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

Para más información sobre cómo migrar la configuración de aplicaciones de versiones anteriores de ASP.NET, consulte <xref:migration/proper-to-2x/index#store-configurations>.

## <a name="default-configuration"></a>Configuración predeterminada

Las aplicaciones web basadas en las plantillas de [dotnet new](/dotnet/core/tools/dotnet-new) de ASP.NET Core llaman a <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> al crear un host. `CreateDefaultBuilder` proporciona la configuración predeterminada para la aplicación en el orden siguiente:

El contenido siguiente es válido para las aplicaciones que usen el [host genérico](xref:fundamentals/host/generic-host). Para obtener más información sobre la configuración predeterminada al usar el [host de web](xref:fundamentals/host/web-host), vea la [versión de este tema para ASP.NET Core 2.2](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).

* La configuración del host la proporcionan los siguientes elementos:
  * Variables de entorno prefijadas con `DOTNET_` (por ejemplo, `DOTNET_ENVIRONMENT`) con el [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider). El prefijo (`DOTNET_`) se quita cuando se cargan los pares clave-valor de configuración.
  * Argumentos de la línea de comandos con el [Proveedor de configuración de línea de comandos](#command-line-configuration-provider).
* La configuración predeterminada del host de web se ha establecido (`ConfigureWebHostDefaults`):
  * Kestrel se usa como el servidor web y se ha configurado mediante los proveedores de configuración de la aplicación.
  * Agregar el middleware de filtrado de host
  * Agregue el middleware de encabezados reenviados si la variable de entorno `ASPNETCORE_FORWARDEDHEADERS_ENABLED` se establece en `true`.
  * Habilite la integración con IIS.
* La configuración de la aplicación la proporcionan los siguientes elementos:
  * *appsettings.json* con el [Proveedor de configuración de archivo](#file-configuration-provider).
  * *appsettings.{Entorno}.json* con el [Proveedor de configuración de archivo](#file-configuration-provider).
  * [Administrador de secretos](xref:security/app-secrets) cuando la aplicación se ejecuta en el entorno `Development` por medio del ensamblado de entrada.
  * Variables de entorno con el [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider).
  * Argumentos de la línea de comandos con el [Proveedor de configuración de línea de comandos](#command-line-configuration-provider).

## <a name="security"></a>Seguridad

Adopte los procedimientos siguientes para proteger los datos de configuración confidenciales:

* Nunca almacene contraseñas u otros datos confidenciales en el código del proveedor de configuración o en archivos de configuración de texto sin formato.
* No use secretos de producción en los entornos de desarrollo o pruebas.
* Especifique los secretos fuera del proyecto para que no se confirmen en un repositorio de código fuente de manera accidental.

Para obtener más información, vea los temas siguientes:

* <xref:fundamentals/environments>
* <xref:security/app-secrets> &ndash; Se incluyen recomendaciones sobre el uso de variables de entorno para almacenar información confidencial. El Administrador de secretos usa el proveedor de configuración de archivo para almacenar secretos de usuario en un archivo JSON del sistema local. El proveedor de configuración de archivo se describe más adelante en este tema.

[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) almacena de forma segura secretos de aplicación para aplicaciones de ASP.NET Core. Para obtener más información, vea <xref:security/key-vault-configuration>.

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

Los métodos <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> y <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> están disponibles para aislar las secciones y los elementos secundarios de una sección en los datos de configuración. Estos métodos se describen más adelante en la sección [GetSection, GetChildren y Exists](#getsection-getchildren-and-exists).

## <a name="conventions"></a>Convenciones

### <a name="sources-and-providers"></a>Orígenes y proveedores

Al iniciar la aplicación, los orígenes de configuración se leen en el orden en que se especifican sus proveedores de configuración.

Los proveedores de configuración que implementan la detección de cambios tienen la capacidad de volver a cargar la configuración cuando se cambia una configuración subyacente. Por ejemplo, el proveedor de configuración de archivos (descrito más adelante en este tema) y el [proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration) implementan la detección de cambios.

<xref:Microsoft.Extensions.Configuration.IConfiguration> está disponible en el contenedor de [inserción de dependencias (DI)](xref:fundamentals/dependency-injection) de la aplicación. <xref:Microsoft.Extensions.Configuration.IConfiguration> se puede insertar en <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> de Razor Pages o <xref:Microsoft.AspNetCore.Mvc.Controller> de MVC para obtener la configuración de la clase.

En los ejemplos siguientes, se usa el campo `_config` para tener acceso a los valores de configuración:

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }
}
```

```csharp
public class HomeController : Controller
{
    private readonly IConfiguration _config;

    public HomeController(IConfiguration config)
    {
        _config = config;
    }
}
```

Los proveedores de configuración no pueden usar la inserción de dependencias, porque no está disponible cuando el host los configura.

### <a name="keys"></a>Teclas

Las claves de configuración adoptan las convenciones siguientes:

* Las claves no distinguen mayúsculas de minúsculas. Por ejemplo, `ConnectionString` y `connectionstring` se consideran claves equivalente.
* Si los mismos o distintos proveedores de configuración establecen un valor para la misma clave, el último valor establecido en la clave es el valor que se usa.
* Claves jerárquicas
  * Dentro de la API de configuración, un separador de dos puntos (`:`) funciona en todas las plataformas.
  * En las variables de entorno, puede que un separador de dos puntos no funcione en todas las plataformas. Todas las plataformas admiten un carácter de subrayado doble (`__`), que se convierte automáticamente en un signo de dos puntos.
  * En Azure Key Vault, las claves jerárquicas usan `--` (dos guiones) como separador. Escriba código para reemplazar los guiones por dos puntos cuando los secretos se carguen en la configuración de la aplicación.
* <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> admite enlazar matrices a objetos con los índices de matriz en las claves de configuración. El enlace de matriz se describe en la sección [Enlace de una matriz a una clase](#bind-an-array-to-a-class).

### <a name="values"></a>Valores

Los valores de configuración adoptan las convenciones siguientes:

* Los valores son cadenas.
* Los valores NULL no se pueden almacenar en la configuración ni enlazar a los objetos.

## <a name="providers"></a>Proveedores

La siguiente tabla muestra los proveedores de configuración disponibles para las aplicaciones de ASP.NET Core.

| Proveedor | Proporciona la configuración de&hellip; |
| -------- | ----------------------------------- |
| [Proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration) (temas de *Seguridad*) | Azure Key Vault |
| [Proveedor de Azure App Configuration](/azure/azure-app-configuration/quickstart-aspnet-core-app) (documentación de Azure) | Configuración de aplicaciones de Azure |
| [Proveedor de configuración de línea de comandos](#command-line-configuration-provider) | Parámetros de la línea de comandos |
| [Proveedor de configuración personalizada](#custom-configuration-provider) | Origen personalizado |
| [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider) | Variables de entorno |
| [Proveedor de configuración de archivo](#file-configuration-provider) | Archivos (INI, JSON, XML) |
| [Proveedor de configuración de clave por archivo](#key-per-file-configuration-provider) | Archivos de directorio |
| [Proveedor de configuración de memoria](#memory-configuration-provider) | Colecciones en memoria |
| [Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (temas de *Seguridad*) | Archivo en el directorio del perfil de usuario |

Los orígenes de configuración se leen en el orden en que se especifican sus proveedores de configuración en el inicio. En este tema, los proveedores de configuración se describen en orden alfabético y no en el orden en el que el código los organiza. Ordene los proveedores de configuración en el código para cumplir con las prioridades relacionadas con los orígenes de configuración subyacentes que la aplicación necesita.

Esta es una secuencia típica de proveedores de configuración:

1. Archivos (*appsettings.json*, *appsettings.{Environment}.json*, donde `{Environment}` es el entorno de hospedaje actual de la aplicación)
1. [Azure Key Vault](xref:security/key-vault-configuration)
1. [Secretos de usuario (administrador de secretos)](xref:security/app-secrets) (solo para entornos de desarrollo)
1. Variables de entorno
1. Argumentos de la línea de comandos

Una práctica común es colocar el proveedor de configuración de línea de comandos al final en una serie de proveedores para permitir que los argumentos de la línea de comandos reemplacen la configuración establecida por el resto de los proveedores.

La secuencia de proveedores anterior se usa cuando se inicializa un nuevo generador de host con `CreateDefaultBuilder`. Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a>Configurar el generador de host con ConfigureHostConfiguration

Para configurar el generador de host, realice una llamada a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> y proporcione la configuración. `ConfigureHostConfiguration` se usa para inicializar <xref:Microsoft.Extensions.Hosting.IHostEnvironment> para su uso posterior en el proceso de compilación. Se puede llamar varias veces a `ConfigureHostConfiguration` con resultados de suma.

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureHostConfiguration(config =>
        {
            var dict = new Dictionary<string, string>
            {
                {"MemoryCollectionKey1", "value1"},
                {"MemoryCollectionKey2", "value2"}
            };

            config.AddInMemoryCollection(dict);
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Llame a `ConfigureAppConfiguration` cuando cree el host para especificar los proveedores de configuración de la aplicación además de aquellos que `CreateDefaultBuilder` agrega automáticamente:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a>Reemplazar la configuración anterior con argumentos de la línea de comandos

Para proporcionar configuración de aplicaciones que se pueda reemplazar por argumentos de la línea de comandos, llame a `AddCommandLine` en último lugar:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a>Quitar proveedores agregados por CreateDefaultBuilder

Para quitar los proveedores agregados por `CreateDefaultBuilder`, llame primero a [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) en [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources):

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a>Usar la configuración durante el inicio de la aplicación

La configuración proporcionada a la aplicación en `ConfigureAppConfiguration` está disponible durante el inicio de la aplicación, incluido `Startup.ConfigureServices`. Para obtener más información, vea la sección [Acceso a la configuración durante el inicio](#access-configuration-during-startup).

## <a name="command-line-configuration-provider"></a>Proveedor de configuración de línea de comandos

<xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> carga la configuración de pares clave-valor de argumento de la línea de comandos en tiempo de ejecución.

Para activar la configuración de línea de comandos, se llama al método de extensión <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

`AddCommandLine` se llama automáticamente al llamar a `CreateDefaultBuilder(string [])`. Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).

`CreateDefaultBuilder` también carga:

* Configuración opcional de los archivos *appsettings.json* y *appsettings.{Environment}.json*.
* [Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) en el entorno de desarrollo.
* Variables de entorno.

`CreateDefaultBuilder` agrega el proveedor de configuración de línea de comandos al final. Los argumentos de la línea de comandos que se pasan en tiempo de ejecución invalidan la configuración establecida por los otros proveedores.

`CreateDefaultBuilder` actúa cuando se construye el host. Por tanto, la configuración de línea de comandos activada por `CreateDefaultBuilder` puede afectar la manera en que se configura el host.

Para las aplicaciones basadas en plantillas de ASP.NET Core, `CreateDefaultBuilder` ya realiza una llamada a `AddCommandLine`. Para agregar proveedores de configuración adicionales y mantener la capacidad de reemplazar la configuración de sus proveedores con argumentos de la línea de comandos, llame a los proveedores adicionales de la aplicación en `ConfigureAppConfiguration` y llame a `AddCommandLine` en último lugar.

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

**Ejemplo**

La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.

1. Abra un símbolo del sistema en el directorio del proyecto.
1. Proporcione un argumento de la línea de comandos para el comando `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.
1. Una vez que se ejecuta la aplicación, abra un explorador a la aplicación en `http://localhost:5000`.
1. Observe que la salida contiene el par clave-valor para el argumento de línea de comandos de configuración proporcionado a `dotnet run`.

### <a name="arguments"></a>Argumentos

El valor debe seguir a un signo igual (`=`) o la clave debe tener un prefijo (`--` o `/`) cuando el valor siga a un espacio. El valor no es obligatorio si se usa un signo igual (por ejemplo, `CommandLineKey=`).

| Prefijo de la clave               | Ejemplo                                                |
| ------------------------ | ------------------------------------------------------ |
| Sin prefijo                | `CommandLineKey1=value1`                               |
| Dos guiones (`--`)        | `--CommandLineKey2=value2`, `--CommandLineKey2 value2` |
| Barra diagonal (`/`)      | `/CommandLineKey3=value3`, `/CommandLineKey3 value3`   |

Dentro del mismo comando, no mezcle pares clave-valor de argumento de la línea de comandos que usan un signo igual con pares de clave-valor que usan un espacio.

Comandos de ejemplo:

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a>Asignaciones de modificador

Las asignaciones de modificador admiten la lógica de sustitución de nombres de clave. Al crear manualmente la configuración con <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, proporcione un diccionario de reemplazos de modificador al método <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.

Cuando se usa el diccionario de asignaciones de modificador, se comprueba en el diccionario si una clave coincide con la clave proporcionada por un argumento de línea de comandos. Si la clave de la línea de comandos se encuentra en el diccionario, se devuelve el valor del diccionario (el reemplazo de la clave) para establecer el par clave-valor en la configuración de la aplicación. Se requiere una asignación de conmutador para cualquier clave de línea de comandos precedida por un solo guion (`-`).

Reglas de clave del diccionario de asignaciones de modificador:

* Los modificadores deben empezar por un guion (`-`) o guion doble (`--`).
* El diccionario de asignaciones de modificador no debe contener claves duplicadas.

Cree un diccionario de asignaciones de modificador. En el ejemplo siguiente, se crean dos asignaciones de modificador:

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

Al compilar el host, llame a `AddCommandLine` con el diccionario de asignaciones de modificador:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

En el caso de las aplicaciones que usen las asignaciones de modificador, la llamada a `CreateDefaultBuilder` no tiene que pasar argumentos. En la llamada de `AddCommandLine` del método `CreateDefaultBuilder`, no se incluyen modificadores asignados y no hay forma de pasar el diccionario de asignación de modificador a `CreateDefaultBuilder`. La solución no es pasar los argumentos de `CreateDefaultBuilder`, sino que permitir que el método `AddCommandLine` del método `ConfigurationBuilder` procese tanto los argumentos como el diccionario de asignación de modificador.

Después de crear el diccionario de asignaciones de modificador, contiene los datos que se muestran en la tabla siguiente.

| Key       | Valor             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

Si las claves de asignación de conmutador se usan al iniciar la aplicación, la configuración recibe el valor de configuración en la clave que el diccionario suministra:

```dotnetcli
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

[Azure App Service](https://azure.microsoft.com/services/app-service/) permite establecer variables de entorno en Azure Portal que pueden invalidar la configuración de la aplicación mediante el proveedor de configuración de variables de entorno. Para más información, consulte [Aplicaciones de Azure: Invalidación de la configuración de la aplicación mediante Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).

`AddEnvironmentVariables` se usa para cargar variables de entorno con el prefijo `DOTNET_` para la [configuración de host](#host-versus-app-configuration) al inicializar un nuevo generador de host con el [host genérico](xref:fundamentals/host/generic-host) y al llamar a `CreateDefaultBuilder`. Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).

`CreateDefaultBuilder` también carga:

* Configuración de la aplicación desde variables de entorno sin prefijo mediante la llamada a `AddEnvironmentVariables` sin prefijo.
* Configuración opcional de los archivos *appsettings.json* y *appsettings.{Environment}.json*.
* [Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) en el entorno de desarrollo.
* Argumentos de la línea de comandos.

El proveedor de configuración de variables de entorno se llama una vez establecida la configuración desde los secretos de usuario y los archivos *appsettings*. Llamar al proveedor en esta posición permite que la lectura de las variables de entorno en tiempo de ejecución invaliden la configuración establecida por los secretos de usuario y los archivos *appsettings*.

Para proporcionar la configuración de la aplicación desde variables de entorno adicionales, llame a los proveedores adicionales de la aplicación en `ConfigureAppConfiguration` y llame a `AddEnvironmentVariables` con el prefijo:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

Llame a `AddEnvironmentVariables` en último lugar para permitir que las variables de entorno con el prefijo especificado invaliden los valores de otros proveedores.

**Ejemplo**

La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a `AddEnvironmentVariables`.

1. Ejecute la aplicación de ejemplo. Abra un explorador a la aplicación en `http://localhost:5000`.
1. Observe que el resultado contiene el par clave y valor para la variable de entorno `ENVIRONMENT`. El valor refleja el entorno en el que se ejecuta la aplicación, habitualmente `Development` cuando se ejecuta de manera local.

Para mantener un número adecuado de variables de entorno en la lista representada por la aplicación, la aplicación filtrará las variables de entorno. Vea el archivo *Pages/Index.cshtml.cs* de la aplicación de ejemplo.

Para exponer todas las variables de entorno disponibles para la aplicación, cambie `FilteredConfiguration` en *Pages/Index.cshtml.cs* por lo siguiente:

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

Al crear el generador de host, las variables de entorno proporcionan la configuración del host. Para obtener más información sobre el prefijo usado para estas variables de entorno, vea la sección [Configuración predeterminada](#default-configuration).

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
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | Entrada de configuración no creada.                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | Clave: `ConnectionStrings:{KEY}_ProviderName`:<br>Valor: `MySql.Data.MySqlClient` |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | Clave: `ConnectionStrings:{KEY}_ProviderName`:<br>Valor: `System.Data.SqlClient`  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | Clave: `ConnectionStrings:{KEY}_ProviderName`:<br>Valor: `System.Data.SqlClient`  |

**Ejemplo**

Se crea una variable de entorno de una cadena de conexión personalizada en el servidor:

* Nombre: `CUSTOMCONNSTR_ReleaseDB`
* Valor: `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`

Si `IConfiguration` se inserta y se asigna a un campo denominado `_config`, se lee el valor siguiente:

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

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

Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

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

Se llama automáticamente a `AddJsonFile` dos veces cuando un nuevo generador de host se inicializa con `CreateDefaultBuilder`. Se llama al método para cargar la configuración desde:

* *appsettings.json*: este archivo se lee primero. La versión del entorno del archivo puede invalidar los valores que proporciona el archivo *appsettings.json*.
* *appsettings.{Environment}.json*: la versión del entorno del archivo se carga en función de [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).

Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).

`CreateDefaultBuilder` también carga:

* Variables de entorno.
* [Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) en el entorno de desarrollo.
* Argumentos de la línea de comandos.

El proveedor de configuración de JSON se establece en primer lugar. Por tanto, los secretos de usuario, las variables de entorno y los argumentos de la línea de comandos invalidan la configuración establecida por los archivos *appsettings*.

Llame a `ConfigureAppConfiguration` al crear el host para especificar la configuración de la aplicación para archivos distintos de *appsettings.json* y *appsettings.{Environment}.json*:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

**Ejemplo**

La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a `AddJsonFile`:

* La primera llamada a `AddJsonFile` carga la configuración desde *appsettings.json*:

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.json)]

* La segunda llamada a `AddJsonFile` carga la configuración desde *appsettings.{Environment}.json*. Para *appsettings.Development.json* en la aplicación de ejemplo, se carga el siguiente archivo:

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.Development.json)]

1. Ejecute la aplicación de ejemplo. Abra un explorador a la aplicación en `http://localhost:5000`.
1. La salida contiene pares clave-valor para la configuración basada en el entorno de la aplicación. El nivel de registro de la clave `Logging:LogLevel:Default` es `Debug` al ejecutar la aplicación en el entorno de desarrollo.
1. Vuelva a ejecutar la aplicación de ejemplo en el entorno de producción:
   1. Abra el archivo *Properties/launchSettings.json*.
   1. En el perfil de `ConfigurationSample`, cambie el valor de la variable de entorno de `ASPNETCORE_ENVIRONMENT` por `Production`.
   1. Guarde el archivo y ejecute la aplicación con `dotnet run` en un shell de comandos.
1. La configuración de *appsettings.Development.json* ya no invalida la configuración de *appsettings.json*. El nivel de registro de la clave `Logging:LogLevel:Default` es `Information`.

### <a name="xml-configuration-provider"></a>Proveedor de configuración XML

<xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> carga la configuración desde pares clave-valor de archivo XML en tiempo de ejecución.

Para activar la configuración de archivo XML, llame al método de extensión <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Las sobrecargas permiten especificar:

* Si el archivo es opcional.
* Si la configuración se recarga si el archivo cambia.
* <xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.

El nodo raíz del archivo de configuración se omite cuando se crean los pares clave-valor de configuración. No especifique una definición de tipo de documento (DTD) ni el espacio de nombres en el archivo.

Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

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

Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a>Proveedor de configuración de memoria

<xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> usa una colección en memoria como pares clave-valor de configuración.

Para activar la configuración de colección en memoria, llame al método de extensión <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

El proveedor de configuración se puede inicializar con un `IEnumerable<KeyValuePair<String,String>>`.

Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación.

En el ejemplo siguiente, se crea un diccionario de configuración:

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

El diccionario se usa con una llamada a `AddInMemoryCollection` para proporcionar la configuración:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a>GetValue

[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrae un único valor de la configuración con una clave especificada y lo convierte al tipo especificado que no es de colección. Una sobrecarga acepta un valor predeterminado.

En el ejemplo siguiente:

* Se extrae el valor de cadena de la configuración con la clave `NumberKey`. Si `NumberKey` no se encuentra en las claves de configuración, se usa el valor predeterminado de `99`.
* Se escribe el valor como `int`.
* Se almacena el valor en la propiedad `NumberConfig` para usarlo en la página.

```csharp
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

[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrae una subsección de la configuración con la clave de subsección especificada.

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

Los valores de configuración se devuelven como cadenas, pero llamar a <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> permite la construcción de objetos [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object). El enlazador enlaza los valores con todas las propiedades de lectura o escritura públicas del tipo proporcionado. Los campos **no** se enlazan.

La aplicación de ejemplo contiene un modelo `Starship` (*Models/Starship.cs*):

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

La sección `starship` del archivo *starship.json* crea la configuración cuando la aplicación de ejemplo usa el proveedor de configuración JSON para cargar la configuración:

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

Se crean los siguientes pares clave-valor de configuración:

| Key                   | Valor                                             |
| --------------------- | ------------------------------------------------- |
| starship:name         | USS Enterprise                                    |
| starship:registry     | NCC-1701                                          |
| starship:class        | Constitution                                      |
| starship:length       | 304.8                                             |
| starship:commissioned | False                                             |
| trademark             | Paramount Pictures Corp. https://www.paramount.com |

La aplicación de ejemplo llama a `GetSection` con la clave `starship`. Los pares clave-valor `starship` están aislados. El método `Bind` se llama en la subsección que pasa una instancia de la clase `Starship`. Después de enlazar los valores de instancia, la instancia está asignada a una propiedad para la representación:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

## <a name="bind-to-an-object-graph"></a>Enlazar a un gráfico de objetos

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> es capaz de enlazar todo un gráfico de objetos POCO. Al igual que ocurre con el enlace de un objeto simple, solo se enlazan las propiedades públicas de lectura o escritura.

El ejemplo contiene un modelo `TvShow` cuyo gráfico de objetos incluye las clases `Metadata` y `Actors` (*Models/TvShow.cs*):

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

La aplicación de ejemplo tiene un archivo *tvshow.xml* que contiene los datos de configuración:

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

Configuración está enlazada a todo el gráfico de objetos `TvShow`con el método `Bind`. La instancia enlazada se asigna a una propiedad para la representación:

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) enlaza y devuelve el tipo especificado. `Get<T>` es más conveniente que usar `Bind`. El código siguiente muestra cómo usar `Get<T>` con el ejemplo anterior, lo que permite que la instancia enlazada se asigne directamente a la propiedad que se usa para la representación:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a>Enlace de una matriz a una clase

*La aplicación de ejemplo muestra los conceptos que se explican en esta sección.*

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> admite enlazar matrices a objetos con los índices de matriz en las claves de configuración. Cualquier formato de matriz que expone un segmento de clave numérica (`:0:`, `:1:`, &hellip; `:{n}:`) es capaz de enlazar una matriz a una matriz de clase POCO.

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

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

La matriz omite un valor de índice &num;3. El enlazador de configuración no es capaz de enlazar los valores NULL ni de crear entradas NULL en los objetos enlazados, lo que queda claro cuando se muestra el resultado de enlazar esta matriz a un objeto.

En la aplicación de ejemplo, hay disponible una clase POCO para almacenar los datos de configuración enlazados:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

Los datos de configuración están enlazados al objeto:

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

También se puede usar la sintaxis de [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*), lo que genera un código más compacto:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

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

En `ConfigureAppConfiguration`:

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
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

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

El proveedor de configuración JSON lee los datos de configuración en los siguientes pares clave-valor:

| Key                     | Valor  |
| ----------------------- | :----: |
| json_array:key          | valueA |
| json_array:subsection:0 | valueB |
| json_array:subsection:1 | valueC |
| json_array:subsection:2 | valueD |

En la aplicación de ejemplo, la clase POCO siguiente está disponible para enlazar los pares clave-valor de configuración:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

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

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

Agregue un `EFConfigurationContext` para almacenar y tener acceso a los valores configurados.

*EFConfigurationProvider/EFConfigurationContext.cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

Cree una clase que implemente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.

*EFConfigurationProvider/EFConfigurationSource.cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

Cree el proveedor de configuración personalizado heredando de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>. El proveedor de configuración inicializa la base de datos cuando está vacía. Puesto que las [claves de configuración no distinguen entre mayúsculas y minúsculas](#keys), el diccionario empleado para iniciar la base de datos se crea con el comparador que tampoco hace tal distinción ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).

*EFConfigurationProvider/EFConfigurationProvider.cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

Un método de extensión `AddEFConfiguration` permite agregar el origen de configuración a un `ConfigurationBuilder`.

*Extensions/EntityFrameworkExtensions.cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

En el código siguiente se muestra cómo puede usar el `EFConfigurationProvider` personalizado en *Program.cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

La configuración de la aplicación en ASP.NET Core se basa en pares clave-valor establecidos por *proveedores de configuración*. Los proveedores de configuración leen los datos de configuración en los pares clave-valor de distintos orígenes de configuración:

* Azure Key Vault
* Configuración de aplicaciones de Azure
* Argumentos de la línea de comandos
* Proveedores personalizados (instalados o creados)
* Archivos de directorio
* Variables de entorno
* Objetos de .NET en memoria
* Archivos de configuración

Los paquetes de configuración para escenarios de proveedor de configuración común ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) se incluyen en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Los ejemplos de código que siguen y en la aplicación de ejemplo utilizan el espacio de nombres <xref:Microsoft.Extensions.Configuration>:

```csharp
using Microsoft.Extensions.Configuration;
```

El *patrón de opciones* es una extensión de los conceptos de configuración que se describen en este tema. Las opciones usan clases para representar grupos de configuraciones relacionadas. Para obtener más información, vea <xref:fundamentals/configuration/options>.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="host-versus-app-configuration"></a>Configuración de host y de aplicación

Antes de configurar e iniciar la aplicación, se configura e inicia un *host*. El host es responsable de la administración del inicio y la duración de la aplicación. Tanto la aplicación como el host se configuran mediante los proveedores de configuración que se describen en este tema. Los pares clave-valor de configuración del host también se incluyen en la configuración de la aplicación. Para más información sobre cómo se usan los proveedores de configuración cuando se compila el host y cómo afectan los orígenes de configuración a la configuración del host, consulte <xref:fundamentals/index#host>.

## <a name="other-configuration"></a>Otra configuración

Este tema solo concierne a la *configuración de aplicaciones*. Otros aspectos de la ejecución y el hospedaje de aplicaciones ASP.NET Core se configuran mediante archivos de configuración que no se describen en este tema:

* *launch.json*/*launchSettings.json* son archivos de configuración de herramientas para el entorno de desarrollo, que se describe a continuación:
  * En <xref:fundamentals/environments#development>.
  * En el conjunto de documentación en el que se usan los archivos para configurar aplicaciones ASP.NET Core para escenarios de desarrollo.
* *web.config* es un archivo de configuración del servidor, que se describe en los temas siguientes:
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

Para más información sobre cómo migrar la configuración de aplicaciones de versiones anteriores de ASP.NET, consulte <xref:migration/proper-to-2x/index#store-configurations>.

## <a name="default-configuration"></a>Configuración predeterminada

Las aplicaciones web basadas en las plantillas de [dotnet new](/dotnet/core/tools/dotnet-new) de ASP.NET Core llaman a <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> al crear un host. `CreateDefaultBuilder` proporciona la configuración predeterminada para la aplicación en el orden siguiente:

El contenido siguiente es válido para las aplicaciones que usen el [host de web](xref:fundamentals/host/web-host). Para obtener más información sobre la configuración predeterminada al usar el [host genérico](xref:fundamentals/host/generic-host), vea la [versión más reciente de ese tema](xref:fundamentals/configuration/index).

* La configuración del host la proporcionan los siguientes elementos:
  * Variables de entorno prefijadas con `ASPNETCORE_` (por ejemplo, `ASPNETCORE_ENVIRONMENT`) con el [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider). El prefijo (`ASPNETCORE_`) se quita cuando se cargan los pares clave-valor de configuración.
  * Argumentos de la línea de comandos con el [Proveedor de configuración de línea de comandos](#command-line-configuration-provider).
* La configuración de la aplicación la proporcionan los siguientes elementos:
  * *appsettings.json* con el [Proveedor de configuración de archivo](#file-configuration-provider).
  * *appsettings.{Entorno}.json* con el [Proveedor de configuración de archivo](#file-configuration-provider).
  * [Administrador de secretos](xref:security/app-secrets) cuando la aplicación se ejecuta en el entorno `Development` por medio del ensamblado de entrada.
  * Variables de entorno con el [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider).
  * Argumentos de la línea de comandos con el [Proveedor de configuración de línea de comandos](#command-line-configuration-provider).

## <a name="security"></a>Seguridad

Adopte los procedimientos siguientes para proteger los datos de configuración confidenciales:

* Nunca almacene contraseñas u otros datos confidenciales en el código del proveedor de configuración o en archivos de configuración de texto sin formato.
* No use secretos de producción en los entornos de desarrollo o pruebas.
* Especifique los secretos fuera del proyecto para que no se confirmen en un repositorio de código fuente de manera accidental.

Para obtener más información, vea los temas siguientes:

* <xref:fundamentals/environments>
* <xref:security/app-secrets> &ndash; Se incluyen recomendaciones sobre el uso de variables de entorno para almacenar información confidencial. El Administrador de secretos usa el proveedor de configuración de archivo para almacenar secretos de usuario en un archivo JSON del sistema local. El proveedor de configuración de archivo se describe más adelante en este tema.

[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) almacena de forma segura secretos de aplicación para aplicaciones de ASP.NET Core. Para obtener más información, vea <xref:security/key-vault-configuration>.

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

Los métodos <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> y <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> están disponibles para aislar las secciones y los elementos secundarios de una sección en los datos de configuración. Estos métodos se describen más adelante en la sección [GetSection, GetChildren y Exists](#getsection-getchildren-and-exists).

## <a name="conventions"></a>Convenciones

### <a name="sources-and-providers"></a>Orígenes y proveedores

Al iniciar la aplicación, los orígenes de configuración se leen en el orden en que se especifican sus proveedores de configuración.

Los proveedores de configuración que implementan la detección de cambios tienen la capacidad de volver a cargar la configuración cuando se cambia una configuración subyacente. Por ejemplo, el proveedor de configuración de archivos (descrito más adelante en este tema) y el [proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration) implementan la detección de cambios.

<xref:Microsoft.Extensions.Configuration.IConfiguration> está disponible en el contenedor de [inserción de dependencias (DI)](xref:fundamentals/dependency-injection) de la aplicación. <xref:Microsoft.Extensions.Configuration.IConfiguration> se puede insertar en <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> de Razor Pages o <xref:Microsoft.AspNetCore.Mvc.Controller> de MVC para obtener la configuración de la clase.

En los ejemplos siguientes, se usa el campo `_config` para tener acceso a los valores de configuración:

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }
}
```

```csharp
public class HomeController : Controller
{
    private readonly IConfiguration _config;

    public HomeController(IConfiguration config)
    {
        _config = config;
    }
}
```

Los proveedores de configuración no pueden usar la inserción de dependencias, porque no está disponible cuando el host los configura.

### <a name="keys"></a>Teclas

Las claves de configuración adoptan las convenciones siguientes:

* Las claves no distinguen mayúsculas de minúsculas. Por ejemplo, `ConnectionString` y `connectionstring` se consideran claves equivalente.
* Si los mismos o distintos proveedores de configuración establecen un valor para la misma clave, el último valor establecido en la clave es el valor que se usa.
* Claves jerárquicas
  * Dentro de la API de configuración, un separador de dos puntos (`:`) funciona en todas las plataformas.
  * En las variables de entorno, puede que un separador de dos puntos no funcione en todas las plataformas. Todas las plataformas admiten un carácter de subrayado doble (`__`), que se convierte automáticamente en un signo de dos puntos.
  * En Azure Key Vault, las claves jerárquicas usan `--` (dos guiones) como separador. Escriba código para reemplazar los guiones por dos puntos cuando los secretos se carguen en la configuración de la aplicación.
* <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> admite enlazar matrices a objetos con los índices de matriz en las claves de configuración. El enlace de matriz se describe en la sección [Enlace de una matriz a una clase](#bind-an-array-to-a-class).

### <a name="values"></a>Valores

Los valores de configuración adoptan las convenciones siguientes:

* Los valores son cadenas.
* Los valores NULL no se pueden almacenar en la configuración ni enlazar a los objetos.

## <a name="providers"></a>Proveedores

La siguiente tabla muestra los proveedores de configuración disponibles para las aplicaciones de ASP.NET Core.

| Proveedor | Proporciona la configuración de&hellip; |
| -------- | ----------------------------------- |
| [Proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration) (temas de *Seguridad*) | Azure Key Vault |
| [Proveedor de Azure App Configuration](/azure/azure-app-configuration/quickstart-aspnet-core-app) (documentación de Azure) | Configuración de aplicaciones de Azure |
| [Proveedor de configuración de línea de comandos](#command-line-configuration-provider) | Parámetros de la línea de comandos |
| [Proveedor de configuración personalizada](#custom-configuration-provider) | Origen personalizado |
| [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider) | Variables de entorno |
| [Proveedor de configuración de archivo](#file-configuration-provider) | Archivos (INI, JSON, XML) |
| [Proveedor de configuración de clave por archivo](#key-per-file-configuration-provider) | Archivos de directorio |
| [Proveedor de configuración de memoria](#memory-configuration-provider) | Colecciones en memoria |
| [Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (temas de *Seguridad*) | Archivo en el directorio del perfil de usuario |

Los orígenes de configuración se leen en el orden en que se especifican sus proveedores de configuración en el inicio. En este tema, los proveedores de configuración se describen en orden alfabético y no en el orden en el que el código los organiza. Ordene los proveedores de configuración en el código para cumplir con las prioridades relacionadas con los orígenes de configuración subyacentes que la aplicación necesita.

Esta es una secuencia típica de proveedores de configuración:

1. Archivos (*appsettings.json*, *appsettings.{Environment}.json*, donde `{Environment}` es el entorno de hospedaje actual de la aplicación)
1. [Azure Key Vault](xref:security/key-vault-configuration)
1. [Secretos de usuario (administrador de secretos)](xref:security/app-secrets) (solo para entornos de desarrollo)
1. Variables de entorno
1. Argumentos de la línea de comandos

Una práctica común es colocar el proveedor de configuración de línea de comandos al final en una serie de proveedores para permitir que los argumentos de la línea de comandos reemplacen la configuración establecida por el resto de los proveedores.

La secuencia de proveedores anterior se usa cuando se inicializa un nuevo generador de host con `CreateDefaultBuilder`. Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).

## <a name="configure-the-host-builder-with-useconfiguration"></a>Configurar el generador de host con UseConfiguration

Para configurar el generador de host, realice una llamada a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> en el generador de host con la configuración.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

    var config = new ConfigurationBuilder()
        .AddInMemoryCollection(dict)
        .Build();

    return WebHost.CreateDefaultBuilder(args)
        .UseConfiguration(config)
        .UseStartup<Startup>();
}
```

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Llame a `ConfigureAppConfiguration` cuando cree el host para especificar los proveedores de configuración de la aplicación además de aquellos que `CreateDefaultBuilder` agrega automáticamente:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a>Reemplazar la configuración anterior con argumentos de la línea de comandos

Para proporcionar configuración de aplicaciones que se pueda reemplazar por argumentos de la línea de comandos, llame a `AddCommandLine` en último lugar:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a>Quitar proveedores agregados por CreateDefaultBuilder

Para quitar los proveedores agregados por `CreateDefaultBuilder`, llame primero a [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) en [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources):

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a>Usar la configuración durante el inicio de la aplicación

La configuración proporcionada a la aplicación en `ConfigureAppConfiguration` está disponible durante el inicio de la aplicación, incluido `Startup.ConfigureServices`. Para obtener más información, vea la sección [Acceso a la configuración durante el inicio](#access-configuration-during-startup).

## <a name="command-line-configuration-provider"></a>Proveedor de configuración de línea de comandos

<xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> carga la configuración de pares clave-valor de argumento de la línea de comandos en tiempo de ejecución.

Para activar la configuración de línea de comandos, se llama al método de extensión <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

`AddCommandLine` se llama automáticamente al llamar a `CreateDefaultBuilder(string [])`. Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).

`CreateDefaultBuilder` también carga:

* Configuración opcional de los archivos *appsettings.json* y *appsettings.{Environment}.json*.
* [Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) en el entorno de desarrollo.
* Variables de entorno.

`CreateDefaultBuilder` agrega el proveedor de configuración de línea de comandos al final. Los argumentos de la línea de comandos que se pasan en tiempo de ejecución invalidan la configuración establecida por los otros proveedores.

`CreateDefaultBuilder` actúa cuando se construye el host. Por tanto, la configuración de línea de comandos activada por `CreateDefaultBuilder` puede afectar la manera en que se configura el host.

Para las aplicaciones basadas en plantillas de ASP.NET Core, `CreateDefaultBuilder` ya realiza una llamada a `AddCommandLine`. Para agregar proveedores de configuración adicionales y mantener la capacidad de reemplazar la configuración de sus proveedores con argumentos de la línea de comandos, llame a los proveedores adicionales de la aplicación en `ConfigureAppConfiguration` y llame a `AddCommandLine` en último lugar.

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

**Ejemplo**

La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.

1. Abra un símbolo del sistema en el directorio del proyecto.
1. Proporcione un argumento de la línea de comandos para el comando `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.
1. Una vez que se ejecuta la aplicación, abra un explorador a la aplicación en `http://localhost:5000`.
1. Observe que la salida contiene el par clave-valor para el argumento de línea de comandos de configuración proporcionado a `dotnet run`.

### <a name="arguments"></a>Argumentos

El valor debe seguir a un signo igual (`=`) o la clave debe tener un prefijo (`--` o `/`) cuando el valor siga a un espacio. El valor no es obligatorio si se usa un signo igual (por ejemplo, `CommandLineKey=`).

| Prefijo de la clave               | Ejemplo                                                |
| ------------------------ | ------------------------------------------------------ |
| Sin prefijo                | `CommandLineKey1=value1`                               |
| Dos guiones (`--`)        | `--CommandLineKey2=value2`, `--CommandLineKey2 value2` |
| Barra diagonal (`/`)      | `/CommandLineKey3=value3`, `/CommandLineKey3 value3`   |

Dentro del mismo comando, no mezcle pares clave-valor de argumento de la línea de comandos que usan un signo igual con pares de clave-valor que usan un espacio.

Comandos de ejemplo:

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a>Asignaciones de modificador

Las asignaciones de modificador admiten la lógica de sustitución de nombres de clave. Al crear manualmente la configuración con <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, proporcione un diccionario de reemplazos de modificador al método <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.

Cuando se usa el diccionario de asignaciones de modificador, se comprueba en el diccionario si una clave coincide con la clave proporcionada por un argumento de línea de comandos. Si la clave de la línea de comandos se encuentra en el diccionario, se devuelve el valor del diccionario (el reemplazo de la clave) para establecer el par clave-valor en la configuración de la aplicación. Se requiere una asignación de conmutador para cualquier clave de línea de comandos precedida por un solo guion (`-`).

Reglas de clave del diccionario de asignaciones de modificador:

* Los modificadores deben empezar por un guion (`-`) o guion doble (`--`).
* El diccionario de asignaciones de modificador no debe contener claves duplicadas.

Cree un diccionario de asignaciones de modificador. En el ejemplo siguiente, se crean dos asignaciones de modificador:

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

Al compilar el host, llame a `AddCommandLine` con el diccionario de asignaciones de modificador:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

En el caso de las aplicaciones que usen las asignaciones de modificador, la llamada a `CreateDefaultBuilder` no tiene que pasar argumentos. En la llamada de `AddCommandLine` del método `CreateDefaultBuilder`, no se incluyen modificadores asignados y no hay forma de pasar el diccionario de asignación de modificador a `CreateDefaultBuilder`. La solución no es pasar los argumentos de `CreateDefaultBuilder`, sino que permitir que el método `AddCommandLine` del método `ConfigurationBuilder` procese tanto los argumentos como el diccionario de asignación de modificador.

Después de crear el diccionario de asignaciones de modificador, contiene los datos que se muestran en la tabla siguiente.

| Key       | Valor             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

Si las claves de asignación de conmutador se usan al iniciar la aplicación, la configuración recibe el valor de configuración en la clave que el diccionario suministra:

```dotnetcli
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

[Azure App Service](https://azure.microsoft.com/services/app-service/) permite establecer variables de entorno en Azure Portal que pueden invalidar la configuración de la aplicación mediante el proveedor de configuración de variables de entorno. Para más información, consulte [Aplicaciones de Azure: Invalidación de la configuración de la aplicación mediante Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).

`AddEnvironmentVariables` se usa para cargar variables de entorno con el prefijo `ASPNETCORE_` para la [configuración de host](#host-versus-app-configuration) al inicializar un nuevo generador de host con el [host de web](xref:fundamentals/host/web-host) y al llamar a `CreateDefaultBuilder`. Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).

`CreateDefaultBuilder` también carga:

* Configuración de la aplicación desde variables de entorno sin prefijo mediante la llamada a `AddEnvironmentVariables` sin prefijo.
* Configuración opcional de los archivos *appsettings.json* y *appsettings.{Environment}.json*.
* [Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) en el entorno de desarrollo.
* Argumentos de la línea de comandos.

El proveedor de configuración de variables de entorno se llama una vez establecida la configuración desde los secretos de usuario y los archivos *appsettings*. Llamar al proveedor en esta posición permite que la lectura de las variables de entorno en tiempo de ejecución invaliden la configuración establecida por los secretos de usuario y los archivos *appsettings*.

Para proporcionar la configuración de la aplicación desde variables de entorno adicionales, llame a los proveedores adicionales de la aplicación en `ConfigureAppConfiguration` y llame a `AddEnvironmentVariables` con el prefijo:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

Llame a `AddEnvironmentVariables` en último lugar para permitir que las variables de entorno con el prefijo especificado invaliden los valores de otros proveedores.

**Ejemplo**

La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a `AddEnvironmentVariables`.

1. Ejecute la aplicación de ejemplo. Abra un explorador a la aplicación en `http://localhost:5000`.
1. Observe que el resultado contiene el par clave y valor para la variable de entorno `ENVIRONMENT`. El valor refleja el entorno en el que se ejecuta la aplicación, habitualmente `Development` cuando se ejecuta de manera local.

Para mantener un número adecuado de variables de entorno en la lista representada por la aplicación, la aplicación filtrará las variables de entorno. Vea el archivo *Pages/Index.cshtml.cs* de la aplicación de ejemplo.

Para exponer todas las variables de entorno disponibles para la aplicación, cambie `FilteredConfiguration` en *Pages/Index.cshtml.cs* por lo siguiente:

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

Al crear el generador de host, las variables de entorno proporcionan la configuración del host. Para obtener más información sobre el prefijo usado para estas variables de entorno, vea la sección [Configuración predeterminada](#default-configuration).

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
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | Entrada de configuración no creada.                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | Clave: `ConnectionStrings:{KEY}_ProviderName`:<br>Valor: `MySql.Data.MySqlClient` |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | Clave: `ConnectionStrings:{KEY}_ProviderName`:<br>Valor: `System.Data.SqlClient`  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | Clave: `ConnectionStrings:{KEY}_ProviderName`:<br>Valor: `System.Data.SqlClient`  |

**Ejemplo**

Se crea una variable de entorno de una cadena de conexión personalizada en el servidor:

* Nombre: `CUSTOMCONNSTR_ReleaseDB`
* Valor: `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`

Si `IConfiguration` se inserta y se asigna a un campo denominado `_config`, se lee el valor siguiente:

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

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

Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

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

Se llama automáticamente a `AddJsonFile` dos veces cuando un nuevo generador de host se inicializa con `CreateDefaultBuilder`. Se llama al método para cargar la configuración desde:

* *appsettings.json*: este archivo se lee primero. La versión del entorno del archivo puede invalidar los valores que proporciona el archivo *appsettings.json*.
* *appsettings.{Environment}.json*: la versión del entorno del archivo se carga en función de [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).

Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).

`CreateDefaultBuilder` también carga:

* Variables de entorno.
* [Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) en el entorno de desarrollo.
* Argumentos de la línea de comandos.

El proveedor de configuración de JSON se establece en primer lugar. Por tanto, los secretos de usuario, las variables de entorno y los argumentos de la línea de comandos invalidan la configuración establecida por los archivos *appsettings*.

Llame a `ConfigureAppConfiguration` al crear el host para especificar la configuración de la aplicación para archivos distintos de *appsettings.json* y *appsettings.{Environment}.json*:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

**Ejemplo**

La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a `AddJsonFile`:

* La primera llamada a `AddJsonFile` carga la configuración desde *appsettings.json*:

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* La segunda llamada a `AddJsonFile` carga la configuración desde *appsettings.{Environment}.json*. Para *appsettings.Development.json* en la aplicación de ejemplo, se carga el siguiente archivo:

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. Ejecute la aplicación de ejemplo. Abra un explorador a la aplicación en `http://localhost:5000`.
1. La salida contiene pares clave-valor para la configuración basada en el entorno de la aplicación. El nivel de registro de la clave `Logging:LogLevel:Default` es `Debug` al ejecutar la aplicación en el entorno de desarrollo.
1. Vuelva a ejecutar la aplicación de ejemplo en el entorno de producción:
   1. Abra el archivo *Properties/launchSettings.json*.
   1. En el perfil de `ConfigurationSample`, cambie el valor de la variable de entorno de `ASPNETCORE_ENVIRONMENT` por `Production`.
   1. Guarde el archivo y ejecute la aplicación con `dotnet run` en un shell de comandos.
1. La configuración de *appsettings.Development.json* ya no invalida la configuración de *appsettings.json*. El nivel de registro de la clave `Logging:LogLevel:Default` es `Warning`.

### <a name="xml-configuration-provider"></a>Proveedor de configuración XML

<xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> carga la configuración desde pares clave-valor de archivo XML en tiempo de ejecución.

Para activar la configuración de archivo XML, llame al método de extensión <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

Las sobrecargas permiten especificar:

* Si el archivo es opcional.
* Si la configuración se recarga si el archivo cambia.
* <xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.

El nodo raíz del archivo de configuración se omite cuando se crean los pares clave-valor de configuración. No especifique una definición de tipo de documento (DTD) ni el espacio de nombres en el archivo.

Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

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

Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a>Proveedor de configuración de memoria

<xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> usa una colección en memoria como pares clave-valor de configuración.

Para activar la configuración de colección en memoria, llame al método de extensión <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.

El proveedor de configuración se puede inicializar con un `IEnumerable<KeyValuePair<String,String>>`.

Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación.

En el ejemplo siguiente, se crea un diccionario de configuración:

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

El diccionario se usa con una llamada a `AddInMemoryCollection` para proporcionar la configuración:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a>GetValue

[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrae un único valor de la configuración con una clave especificada y lo convierte al tipo especificado que no es de colección. Una sobrecarga acepta un valor predeterminado.

En el ejemplo siguiente:

* Se extrae el valor de cadena de la configuración con la clave `NumberKey`. Si `NumberKey` no se encuentra en las claves de configuración, se usa el valor predeterminado de `99`.
* Se escribe el valor como `int`.
* Se almacena el valor en la propiedad `NumberConfig` para usarlo en la página.

```csharp
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

[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrae una subsección de la configuración con la clave de subsección especificada.

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

Los valores de configuración se devuelven como cadenas, pero llamar a <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> permite la construcción de objetos [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object). El enlazador enlaza los valores con todas las propiedades de lectura o escritura públicas del tipo proporcionado. Los campos **no** se enlazan.

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
| trademark             | Paramount Pictures Corp. https://www.paramount.com |

La aplicación de ejemplo llama a `GetSection` con la clave `starship`. Los pares clave-valor `starship` están aislados. El método `Bind` se llama en la subsección que pasa una instancia de la clase `Starship`. Después de enlazar los valores de instancia, la instancia está asignada a una propiedad para la representación:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

## <a name="bind-to-an-object-graph"></a>Enlazar a un gráfico de objetos

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> es capaz de enlazar todo un gráfico de objetos POCO. Al igual que ocurre con el enlace de un objeto simple, solo se enlazan las propiedades públicas de lectura o escritura.

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

[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) enlaza y devuelve el tipo especificado. `Get<T>` es más conveniente que usar `Bind`. El código siguiente muestra cómo usar `Get<T>` con el ejemplo anterior, lo que permite que la instancia enlazada se asigne directamente a la propiedad que se usa para la representación:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a>Enlace de una matriz a una clase

*La aplicación de ejemplo muestra los conceptos que se explican en esta sección.*

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> admite enlazar matrices a objetos con los índices de matriz en las claves de configuración. Cualquier formato de matriz que expone un segmento de clave numérica (`:0:`, `:1:`, &hellip; `:{n}:`) es capaz de enlazar una matriz a una matriz de clase POCO.

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

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

La matriz omite un valor de índice &num;3. El enlazador de configuración no es capaz de enlazar los valores NULL ni de crear entradas NULL en los objetos enlazados, lo que queda claro cuando se muestra el resultado de enlazar esta matriz a un objeto.

En la aplicación de ejemplo, hay disponible una clase POCO para almacenar los datos de configuración enlazados:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

Los datos de configuración están enlazados al objeto:

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

También se puede usar la sintaxis de [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*), lo que genera un código más compacto:

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

En `ConfigureAppConfiguration`:

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
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

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

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

::: moniker-end
