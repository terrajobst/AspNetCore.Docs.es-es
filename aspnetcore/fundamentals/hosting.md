---
title: Hospedar en ASP.NET Core | Documentos de Microsoft
author: ardalis
description: "Introducción a los hosts de web en ASP.NET Core."
keywords: "Núcleo de ASP.NET, host de web, IWebHost"
ms.author: riande
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 4e45311d-8d56-46e2-b99d-6f65b648a277
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0725f3d2c130068094792e5ba9e17481155e4294
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2017
---
# <a name="introduction-to-hosting-in-aspnet-core"></a>Introducción a hospedar en ASP.NET Core

Por [Steve Smith](http://ardalis.com)

Para ejecutar una aplicación de ASP.NET Core, debe configurar e iniciar un host con `WebHostBuilder`.

## <a name="what-is-a-host"></a>¿Qué es un Host?

Las aplicaciones de ASP.NET Core requieren un *host* en el que se va a ejecutar. Un host debe implementar la `IWebHost` interfaz, que expone las colecciones de características y servicios, y un `Start` método. El host se crea normalmente con una instancia de un `WebHostBuilder`, que genera y devuelve un `WebHost` instancia. El `WebHost` hace referencia al servidor que atenderá las solicitudes. Obtenga más información sobre [servidores](servers/index.md).

### <a name="what-is-the-difference-between-a-host-and-a-server"></a>¿Cuál es la diferencia entre un host y un servidor?

El host es responsable de la administración de inicio y duración de la aplicación. El servidor es responsable de aceptar solicitudes HTTP. Parte de la responsabilidad del host incluye garantizar que Servicios de la aplicación y el servidor están disponible y configurado correctamente. Se puede considerar el host como un contenedor alrededor del servidor. El host está configurado para usar un servidor determinado; el servidor no es consciente de su host.

## <a name="setting-up-a-host"></a>Configurar un Host

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Crear un host con una instancia de `WebHostBuilder`. Esto se hace normalmente en el punto de entrada de la aplicación: `public static void Main` (que en las plantillas de proyecto se encuentra en un *Program.cs* archivo). Una típica *Program.cs*, como se muestra a continuación, se muestra cómo utilizar un `WebHostBuilder` para crear un host.

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=14,15,16,17,18,19,20,21)]

El `WebHostBuilder` es responsable de crear el host que se arranca el servidor de la aplicación. `WebHostBuilder`requiere que proporcione un servidor que implementa `IServer` (`UseKestrel` en el código anterior). `UseKestrel`Especifica que el servidor Kestrel usará la aplicación.

El servidor *contenido raíz* determina donde busca los archivos de contenido, como los archivos de vista de MVC. La raíz de contenido de forma predeterminada es la carpeta desde la que se ejecuta la aplicación.

> [!NOTE]
> Especificar `Directory.GetCurrentDirectory` como la raíz de contenido utilizarán carpeta raíz del proyecto web como raíz del contenido de la aplicación cuando la aplicación se inicia desde esta carpeta (por ejemplo, al llamar a `dotnet run` desde la carpeta del proyecto web). Se trata de los valores predeterminados utilizados en Visual Studio y `dotnet new` plantillas.

Para utilizar IIS como un proxy inverso, llame a `UseIISIntegration` como parte de la creación del host. 

Tenga en cuenta que `UseIISIntegration` no configura un *server*, como `UseKestrel` does. Para usar IIS con ASP.NET Core, debe especificar tanto `UseKestrel` y `UseIISIntegration`. `UseKestrel`el servidor web se crea y hospeda la aplicación. `UseIISIntegration`examina las variables de entorno utilizadas por IIS/IISExpress y configura opciones como el puerto para escuchar en y los encabezados a la utilice.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Crear un host con una instancia de `WebHostBuilder`. Esto se hace normalmente en el punto de entrada de la aplicación: `public static void Main` (que en las plantillas de proyecto se encuentra en un *Program.cs* archivo). Una típica *Program.cs*, como se muestra a continuación, llama `CreateDefaultbuilder` para crear un host:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

`CreateDefaultbuilder`crea una instancia de `WebHostBuilder` para crear el host que ejecuta un bootstrap el servidor de la aplicación. El host requiere una [server que implementa IServer](servers/index.md). Los servidores integrados son [Kestrel](servers/kestrel.md) y [HTTP.sys](servers/httpsys.md); `CreateDefaultbuilder` usar Kestrel de forma predeterminada.

`CreateDefaultbuilder`realiza tareas de instalación además de configurar Kestrel como el servidor web:

* Establece la raíz del contenido en `Directory.GetCurrentDirectory`.
* Configuración de las cargas de:
  * *appSettings.JSON que se*
  * *appSettings. \<EnvironmentName > .json*.
  * secretos del usuario cuando la aplicación se ejecuta en el entorno de desarrollo
  * variables de entorno
  * argumentos de línea de comandos proporcionados
* Configura el registro para la salida de consola y de depuración, con el filtrado de las reglas especificadas en una sección de configuración de registro.
* Habilita la integración de IIS.
* Agrega la página de la excepción de desarrollador cuando la aplicación se ejecuta en el entorno de desarrollo.

El servidor *contenido raíz* determina donde busca los archivos de contenido, como los archivos de vista de MVC. La raíz de contenido de forma predeterminada es la carpeta desde la que se ejecuta la aplicación.

> [!NOTE]
> Especificar `Directory.GetCurrentDirectory` como la raíz de contenido utilizarán carpeta raíz del proyecto web como raíz del contenido de la aplicación cuando la aplicación se inicia desde esta carpeta (por ejemplo, al llamar a `dotnet run` desde la carpeta del proyecto web). Se trata de los valores predeterminados utilizados en Visual Studio y `dotnet new` plantillas.

Cuando se usa IIS como un proxy inverso, ASP.NET Core llama automáticamente a `UseIISIntegration` como parte de la creación del host. Para obtener más información, consulte [módulo principal de ASP.NET](xref:fundamentals/servers/aspnet-core-module).

Tenga en cuenta que `UseIISIntegration` no configura un *server*, como `UseKestrel` does. `UseKestrel`el servidor web se crea y hospeda la aplicación. `UseIISIntegration`examina las variables de entorno utilizadas por IIS/IISExpress y configura opciones como el puerto para escuchar en y los encabezados a la utilice.

---

Una implementación mínima de configuración de un host (y una aplicación de ASP.NET Core) incluye sólo un servidor y la configuración de canalización de solicitud de la aplicación:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
    })
    .Build();

host.Run();
```

> [!NOTE]
> Al configurar un host, puede proporcionar `Configure` y `ConfigureServices` métodos, en lugar de o además de especificar un `Startup` clase (que también debe definir estos métodos: vea [inicio de la aplicación](startup.md)). Varias llamadas a `ConfigureServices` anexará entre sí; llama a `Configure` o `UseStartup` , reemplazará la configuración anterior.

## <a name="configuring-a-host"></a>Configuración de un Host

El `WebHostBuilder` proporciona métodos para establecer la mayoría de los valores de configuración disponibles para el host, que también se puede establecer directamente mediante `UseSetting` y la clave asociada.

### <a name="host-configuration-values"></a>Valores de configuración de host

**Capturar errores de inicio**`bool`

Clave: `captureStartupErrors`. Tiene como valor predeterminado `false`. Cuando `false`, errores durante el resultado de inicio en el host de salir. Cuando `true`, el host capturará las excepciones de la `Startup` clase e intentar iniciar el servidor. Se mostrará una página de error (genérico o detallada, según la configuración de errores detallados a continuación) para cada solicitud. Establecer mediante el `CaptureStartupErrors` método.

Nota: Cuando la aplicación se ejecuta con Kestrel e IIS, el comportamiento predeterminado es capturar los errores de inicio. 

```csharp
new WebHostBuilder()
    .CaptureStartupErrors(true)
   ```

**Contenido raíz**`string`

Clave: `contentRoot`. El valor predeterminado es la carpeta donde reside el ensamblado de aplicación (para Kestrel; IIS utilizará la raíz del proyecto web de forma predeterminada). Esta configuración determina donde se iniciará la búsqueda de archivos de contenido, como las vistas de MVC ASP.NET Core. También se utiliza como la ruta de acceso base para la [configuración de la raíz de Web](#web-root-setting). Establecer mediante el `UseContentRoot` método. Ruta de acceso debe existir o se producirá un error de host iniciar.

```csharp
new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
   ```

**Errores detallados**`bool`

Clave: `detailedErrors`. Tiene como valor predeterminado `false`. Cuando `true` (o cuando el entorno está configurado para "Desarrollo"), la aplicación mostrará detalles de excepciones de inicio, en lugar de una página de error genérica. Establecer mediante `UseSetting`.

```csharp
new WebHostBuilder()
    .UseSetting("detailedErrors", "true")
```

Cuando los errores detallados se establece en `false` y capturar errores de inicio es `true`, se muestra una página de error genérico en respuesta a todas las solicitudes al servidor.

![Página de error genérico](hosting/_static/generic-error-page.png)

Cuando los errores detallados se establece en `true` y capturar errores de inicio es `true`, se muestra una página de error detallado en respuesta a todas las solicitudes al servidor.

![Página de error detallados](hosting/_static/detailed-error-page.png)

**Entorno**`string`

Clave: `environment`. El valor predeterminado es "Producción". Puede establecerse en cualquier valor. Los valores definidos por el marco de trabajo incluyen "Desarrollo", "Ensayo" y "Production". Valores no distinguen entre mayúsculas y minúsculas. Vea [trabajar con varios entornos](environments.md). Establecer mediante el `UseEnvironment` método.

```csharp
new WebHostBuilder()
    .UseEnvironment("Development")
```

> [!NOTE]
> De forma predeterminada, el entorno se lee desde el `ASPNETCORE_ENVIRONMENT` variable de entorno. Cuando se utiliza Visual Studio, las variables de entorno pueden establecerse el *launchSettings.json* archivo.

<a id="server-urls"></a>

**Direcciones URL del servidor**`string`

Clave: `urls`. Establecido en un punto y coma (;) separados de los prefijos de lista de direcciones URL que debe responder el servidor. Por ejemplo: `http://localhost:123`. El nombre de dominio/host puede reemplazarse por "\*" para indicar el servidor debe escuchar las solicitudes en cualquier dirección IP o host que usa el puerto especificado y el protocolo (por ejemplo, `http://*:5000` o `https://*:5001`). El protocolo (`http://` o `https://`) debe incluirse en cada dirección URL. Los prefijos son interpretados por el servidor configurado; los formatos admitidos varían entre los servidores.

```csharp
new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

En ASP.NET 2.0 de núcleo, Kestrel tiene su propia API de configuración de punto de conexión y no admite `https://` en el `urls` cadena. Para obtener más información, consulte [Introducción a Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).

**Ensamblado de inicio**`string`

Clave: `startupAssembly`. Determina el ensamblado que se va a buscar la `Startup` clase. Establecer mediante el `UseStartup` método. En su lugar puede hacer referencia a un tipo específico mediante `WebHostBuilder.UseStartup<StartupType>`. Si hay varios `UseStartup` se llaman a métodos, la última de ellas tiene prioridad.

```csharp
new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

<a name=web-root-setting></a>

**Web raíz**`string`

Clave: `webroot`. Si no se especifica el valor predeterminado es `(Content Root Path)\wwwroot`, si existe. Si esta ruta de acceso no existe, se utiliza un proveedor de archivos de la operación inefectiva. Establecer mediante `UseWebRoot`.

```csharp
new WebHostBuilder()
    .UseWebRoot("public")
```

### <a name="overriding-configuration"></a>Reemplazar configuración

Use [configuración](configuration.md) para establecer los valores de configuración que va a usar el host. Estos valores pueden ser reemplazados posteriormente. Esto se especifica con `UseConfiguration`.

```csharp
public static void Main(string[] args)
{
    var config = new ConfigurationBuilder()
        .AddJsonFile("hosting.json", optional: true)
        .AddCommandLine(args)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .Configure(app =>
        {
            app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
        })
        .Build();

    host.Run();
}
```

En el ejemplo anterior, se pueden pasar argumentos de línea de comandos de para configurar el host y, opcionalmente, se pueden especificar valores de configuración en un *hosting.json* archivo. Para especificar el host que se ejecutan en una dirección URL determinada, podría pasar el valor deseado desde un símbolo del sistema:

```console
dotnet run --urls "http://*:5000"
```

El `Run` método inicia la aplicación web y bloquea el subproceso que realiza la llamada hasta que el host está apagado.

```csharp
host.Run();
```

Puede ejecutar el host de una manera sin bloqueo mediante una llamada a su `Start` método:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Pase una lista de direcciones URL para el `Start` método y escuche en las direcciones URL especificadas:

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

Dependen de los formatos de dirección URL aquí son válidos en el servidor que está usando. Para obtener más información, consulte [direcciones URL del servidor](#server-urls) anteriormente en este artículo.

> [!NOTE]
> El `UseConfiguration` no está actualmente capaz de analizar una sección de configuración devuelta por el método de extensión `GetSection` (por ejemplo, `.UseConfiguration(Configuration.GetSection("section"))`. El `GetSection` método filtra las claves de configuración a la sección solicitado, pero deja el nombre de sección de las claves (por ejemplo, `section:urls`, `section:environment`). El `UseConfiguration` método espera las claves para que coincida con el `WebHostBuilder` claves (por ejemplo, `urls`, `environment`). La presencia del nombre de sección de las claves evita que los valores de la sección de configuración del host. Este problema se corregirá en una versión futura. Para obtener más información y soluciones alternativas, consulte [pasar la sección de configuración a WebHostBuilder.UseConfiguration utiliza claves completas](https://github.com/aspnet/Hosting/issues/839).

### <a name="ordering-importance"></a>Orden de importancia

`WebHostBuilder`configuración primero se lee desde determinadas variables de entorno, si establece. Estas variables de entorno deben tener el formato `ASPNETCORE_{configurationKey}`, por lo que para que el ejemplo para establecer las direcciones URL que el servidor escuchará en de forma predeterminada, se establecería `ASPNETCORE_URLS`.

Puede invalidar cualquiera de estos valores de variable de entorno mediante la especificación de configuración (mediante `UseConfiguration`) o estableciendo el valor explícitamente (mediante `UseUrls` para la instancia). El host usará independientemente de la opción establece el valor de la última. Por esta razón, `UseIISIntegration` debe aparecer después de `UseUrls`, dado que reemplaza la dirección URL con una dinámicamente proporcionada por IIS. Si desea mediante programación establece la dirección URL predeterminada en un valor, pero permite que se reemplazará por la configuración, puede configurar el host como se indica a continuación:

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseUrls("http://*:1000") // default URL
    .UseConfiguration(config) // override from command line
    .UseKestrel()
    .Build();
```

## <a name="additional-resources"></a>Recursos adicionales

* [Publicar en Windows que usan IIS](../publishing/iis.md)
* [Publicar en Linux con Nginx](../publishing/linuxproduction.md)
* [Publicar en Linux con Apache](../publishing/apache-proxy.md)
* [Host en un servicio de Windows](xref:hosting/windows-service)

