---
title: Hospedaje de ASP.NET Core en un servicio de Windows
author: guardrex
description: Aprenda a hospedar una aplicación ASP.NET Core en un servicio de Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 718cc83bb29c0cff323853d22c107e00616b1dd1
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126240"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Hospedaje de ASP.NET Core en un servicio de Windows

Por [Luke Latham](https://github.com/guardrex) y [Tom Dykstra](https://github.com/tdykstra)

Una aplicación de ASP.NET Core se puede hospedar en Windows sin usar IIS como [servicio de Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Cuando se hospeda como un servicio de Windows, la aplicación puede iniciarse automáticamente después de reinicios y bloqueos sin necesidad de intervención humana.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="get-started"></a>Primeros pasos

Estos son los cambios mínimos necesarios para configurar un proyecto de ASP.NET Core existente para que se ejecute en un servicio:

1. El archivo del proyecto:

   1. Confirme la presencia del identificador en tiempo de ejecución o agregarlo al **\<PropertyGroup>** que contiene el marco de destino:
      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```
   1. Agregue una referencia de paquete de [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

1. Realice los siguientes cambios en `Program.Main`.

   * Llame a [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) en lugar de a `host.Run`.

   * Si el código llama a `UseContentRoot`, use una ruta de acceso a la ubicación de publicación de la aplicación en lugar de `Directory.GetCurrentDirectory()`.

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. Publique la aplicación. Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) o un [perfil de publicación de Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles).

   Para publicar la aplicación de ejemplo desde la línea de comandos, ejecute el siguiente comando en una ventana de la consola desde la carpeta de proyecto:

   ```console
   dotnet publish --configuration Release
   ```

1. Use la herramienta de línea de comandos [sc.exe](https://technet.microsoft.com/library/bb490995) para crear el servicio. El valor `binPath` es la ruta de acceso al archivo ejecutable de la aplicación, que incluye el nombre del archivo ejecutable. **El espacio entre el signo igual y las comillas al inicio de la cadena de ruta de acceso es obligatorio.**

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   En el caso de un servicio publicado en la carpeta del proyecto, use la ruta de acceso a la carpeta *publish* para crear el servicio. En el ejemplo siguiente, el servicio es:

   * Se denomina **MyService**.
   * Se ha publicado en la carpeta *c:\\my_services\\AspNetCoreService\\bin\\Release\\&lt;PLATAFORMA_DESTINO&gt;\\publish*.
   * Representado por un archivo ejecutable de la aplicación denominado *AspNetCoreService.exe*.

   Abra un shell de comandos con privilegios de administrador y escriba el siguiente comando:

   ```console
   sc create MyService binPath= "c:\my_services\aspnetcoreservice\bin\release\<TARGET_FRAMEWORK>\publish\aspnetcoreservice.exe"
   ```
   
   > [!IMPORTANT]
   > No olvide incluir el espacio entre el argumento `binPath=` y su valor.
   
   Para publicar e iniciar el servicio desde otra carpeta:
   
   1. Use la opción [--output &lt;DIRECTORIO_DE_SALIDA&gt;](/dotnet/core/tools/dotnet-publish#options) en el comando `dotnet publish`.
   1. Cree el servicio con el comando `sc.exe` utilizando la ruta de acceso de la carpeta de salida. Incluya el nombre del archivo ejecutable del servicio en la ruta de acceso proporcionada a `binPath`.

1. Inicie el servicio con el comando `sc start <SERVICE_NAME>`.

   Use el siguiente comando para iniciar el servicio de la aplicación de ejemplo:

   ```console
   sc start MyService
   ```

   Este comando tarda unos segundos en iniciar el servicio.

1. El comando `sc query <SERVICE_NAME>` se puede usar para comprobar el estado del servicio con objeto de conocer su estado:

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   Use el siguiente comando para comprobar el estado del servicio de la aplicación de ejemplo:

   ```console
   sc query MyService
   ```

1. Si el servicio está en estado `RUNNING` y dicho servicio es una aplicación web, vaya a la aplicación en su ruta de acceso correspondiente (de forma predeterminada, `http://localhost:5000`, que redirige a `https://localhost:5001` cuando se usa el [Middleware de redirección de HTTPS](xref:security/enforcing-ssl)).

   Si es el servicio de la aplicación de ejemplo, vaya a la aplicación en `http://localhost:5000`.

1. Detenga el servicio con el comando `sc stop <SERVICE_NAME>`.

   Con el siguiente comando se detiene el servicio de la aplicación de ejemplo:

   ```console
   sc stop MyService
   ```

1. Tras un breve intervalo para detener un servicio, desinstale el servicio con el comando `sc delete <SERVICE_NAME>`.

   Compruebe el estado del servicio de la aplicación de ejemplo:

   ```console
   sc query MyService
   ```

   Si el servicio de la aplicación de ejemplo está en estado `STOPPED`, use el siguiente comando para desinstalar el servicio de la aplicación de ejemplo:

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a>Proporcionar una forma de ejecutar la aplicación fuera de un servicio

Probar y depurar una aplicación resulta más sencillo cuando se ejecuta fuera de un servicio, por lo que es habitual agregar código que llame a `RunAsService` solo bajo determinadas condiciones. Por ejemplo, la aplicación se puede ejecutar como una aplicación de consola con un argumento de línea de comandos `--console` o si el depurador está asociado:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

Dado que la configuración de ASP.NET Core requiere pares nombre-valor en los argumentos de línea de comandos, el conmutador `--console` se quita antes de que los argumentos se pasen a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a>Controlar los eventos de inicio y detención

Para controlar los eventos [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) y [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), haga los siguientes cambios adicionales:

1. Cree una clase que se derive de la clase [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. Cree un método de extensión para [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) que pase el elemento `WebHostService` personalizado a [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. En `Program.Main`, llame al nuevo método de extensión, `RunAsCustomService`, en lugar de a [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

Si el código `WebHostService` personalizado requiere un servicio de inserción de dependencias (como un registrador), obténgalo de la propiedad [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Escenarios de servidor proxy y equilibrador de carga

Los servicios que interactúan con las solicitudes de Internet o de una red corporativa y están detrás de un proxy o de un equilibrador de carga podrían requerir configuración adicional. Para más información, vea [Configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga](xref:host-and-deploy/proxy-load-balancer).

## <a name="kestrel-endpoint-configuration"></a>Configuración del punto de conexión de Kestrel

Para más información sobre la configuración del punto de conexión de Kestrel, así como la configuración de HTTPS y la compatibilidad con SNI, vea [Configuración de punto de conexión de Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).
