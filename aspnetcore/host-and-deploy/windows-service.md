---
title: Hospedaje de ASP.NET Core en un servicio de Windows
author: rick-anderson
description: Aprenda a hospedar una aplicación ASP.NET Core en un servicio de Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 29f83ee585c73aeb57a09f70ea8e28650c05ce69
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153534"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Hospedaje de ASP.NET Core en un servicio de Windows

Por [Tom Dykstra](https://github.com/tdykstra)

La manera recomendada de hospedar una aplicación ASP.NET Core en Windows sin usar IIS es ejecutarla en un [servicio de Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Cuando se hospeda como un servicio de Windows, la aplicación puede iniciarse automáticamente después de reinicios y bloqueos sin necesidad de intervención humana.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample)). Para obtener instrucciones sobre cómo ejecutar la aplicación de ejemplo, vea el archivo *README.md* del ejemplo.

## <a name="prerequisites"></a>Requisitos previos

* La aplicación debe ejecutarse en el entorno de tiempo de ejecución de .NET Framework. En el archivo *.csproj*, especifique los valores adecuados para [TargetFramework](/nuget/schema/target-frameworks) y [RuntimeIdentifier](/dotnet/articles/core/rid-catalog). Por ejemplo:

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  Al crear un proyecto en Visual Studio, use la plantilla **Aplicación ASP.NET Core (.NET Framework)**.

* Si la aplicación recibe las solicitudes de Internet (no solo desde una red interna), debe usar el servidor web [HTTP.sys](xref:fundamentals/servers/httpsys) (antes conocidos como [WebListener](xref:fundamentals/servers/weblistener) para las aplicaciones ASP.NET Core 1.x) en lugar de [Kestrel](xref:fundamentals/servers/kestrel). Se recomienda el uso de IIS como un servidor proxy inverso con Kestrel en implementaciones perimetrales. Para más información, vea [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Cuándo se debe usar Kestrel con un proxy inverso).

## <a name="get-started"></a>Primeros pasos

En esta sección se explican los cambios mínimos necesarios para configurar un proyecto de ASP.NET Core existente para que se ejecute en un servicio.

1. Instale el paquete NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

2. Realice los siguientes cambios en `Program.Main`.

   * Llame a `host.RunAsService` en lugar de a `host.Run`.

   * Si el código llama a `UseContentRoot`, use una ruta de acceso a la ubicación de publicación en lugar de `Directory.GetCurrentDirectory()`.

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

3. Publique la aplicación en una carpeta. Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) o un [perfil de publicación de Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) que publique en una carpeta.

4. Para probar esto, cree e inicie el servicio.

   Abra un shell de comandos con privilegios administrativos para usar la herramienta de la línea de comandos [sc.exe](https://technet.microsoft.com/library/bb490995) para crear e iniciar un servicio. Si el servicio se llamó MyService, se publicó en `c:\svc` y se llamó AspNetCoreService, los comandos son:

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   El valor `binPath` es la ruta de acceso al archivo ejecutable de la aplicación, que incluye el nombre del archivo ejecutable.

   ![Ventana de la consola: ejemplo de creación e inicio](windows-service/_static/create-start.png)

   Cuando finalicen estos comandos, vaya a la misma ruta de acceso que cuando se ejecutó como una aplicación de consola (de forma predeterminada, `http://localhost:5000`):

   ![Ejecución en un servicio](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a>Proporcionar una forma de ejecutar la aplicación fuera de un servicio

Probar y depurar una aplicación resulta más sencillo cuando se ejecuta fuera de un servicio, por lo que es habitual agregar código que llame a `RunAsService` solo bajo determinadas condiciones. Por ejemplo, la aplicación se puede ejecutar como una aplicación de consola con un argumento de línea de comandos `--console` o si el depurador está asociado:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a>Controlar los eventos de inicio y detención

Para controlar los eventos `OnStarting`, `OnStarted` y `OnStopping`, realice los siguientes cambios adicionales:

1. Cree una clase que derive de `WebHostService`:

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. Crear un método de extensión para `IWebHost` que pase el elemento `WebHostService` personalizado a `ServiceBase.Run`:

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. En `Program.Main`, llame al nuevo método de extensión, `RunAsCustomService`, en lugar de a `RunAsService`:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

Si el código `WebHostService` personalizado requiere un servicio de inserción de dependencias (como un registrador), obténgalo de la propiedad `Services` de `IWebHost`:

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Escenarios de servidor proxy y equilibrador de carga

Los servicios que interactúan con las solicitudes de Internet o de una red corporativa y están detrás de un proxy o de un equilibrador de carga podrían requerir configuración adicional. Para más información, vea [Configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga](xref:host-and-deploy/proxy-load-balancer).

## <a name="acknowledgments"></a>Agradecimientos

El artículo se escribió con ayuda de fuentes publicadas:

* [Hosting ASP.NET Core as Windows service](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074) (Hospedaje de ASP.NET Core como servicio de Windows)
* [How to host your ASP.NET Core in a Windows Service](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/) (Cómo hospedar ASP.NET Core en un servicio de Windows)
