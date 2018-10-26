---
title: Hospedaje de ASP.NET Core en un servicio de Windows
author: guardrex
description: Aprenda a hospedar una aplicación ASP.NET Core en un servicio de Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/25/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 7f19db0a1d12b904daff989bc969daf8d2302bfa
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325788"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Hospedaje de ASP.NET Core en un servicio de Windows

Por [Luke Latham](https://github.com/guardrex) y [Tom Dykstra](https://github.com/tdykstra)

Una aplicación de ASP.NET Core se puede hospedar en Windows sin usar IIS como [servicio de Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Cuando se aloja como un servicio de Windows, la aplicación se inicia automáticamente después de reiniciar el equipo.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="convert-a-project-into-a-windows-service"></a>Convertir un proyecto en un servicio de Windows

Estos son los cambios mínimos necesarios para configurar un proyecto de ASP.NET Core existente para que se ejecute como un servicio:

1. El archivo del proyecto:

   * Confirme la presencia del [identificador en tiempo de ejecución](/dotnet/core/rid-catalog) de Windows o agréguelo al `<PropertyGroup>` que contiene la plataforma de destino:

      ::: moniker range=">= aspnetcore-2.1"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="= aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="< aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      Para publicar para varios RID:

      * Proporcione los RID en una lista delimitada por punto y coma.
      * Use el nombre de la propiedad `<RuntimeIdentifiers>` (plural).

      Para más información, vea el [Catálogo de identificadores de entorno de ejecución (RID) de .NET Core](/dotnet/core/rid-catalog).

   * Agregue una referencia de paquete de [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).

1. Realice los siguientes cambios en `Program.Main`.

   * Llame a [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) en lugar de a `host.Run`.

   * Llame a [UseContentRoot](xref:fundamentals/host/web-host#content-root) y use una ruta de acceso a la ubicación de publicación de la aplicación en lugar de `Directory.GetCurrentDirectory()`.

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. Publique la aplicación. Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) o un [perfil de publicación de Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles). Al utilizar Visual Studio, seleccione **FolderProfile**.

   Para publicar la aplicación de ejemplo mediante herramientas de la interfaz de la línea de comandos (CLI), ejecute el comando [dotnet publish](/dotnet/core/tools/dotnet-publish) en un símbolo del sistema desde la carpeta del proyecto. El identificador relativo debe especificarse en la propiedad `<RuntimeIdenfifier>` o `<RuntimeIdentifiers>` del archivo del proyecto. En el ejemplo siguiente, la aplicación se publica en la configuración de lanzamiento para `win7-x64` en tiempo de ejecución:

   ```console
   dotnet publish --configuration Release --runtime win7-x64
   ```

1. Use la herramienta de línea de comandos [sc.exe](https://technet.microsoft.com/library/bb490995) para crear el servicio. El valor `binPath` es la ruta de acceso al archivo ejecutable de la aplicación, que incluye el nombre del archivo ejecutable. **El espacio entre el signo igual y las comillas al inicio de la cadena de ruta de acceso es obligatorio.**

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   En el caso de un servicio publicado en la carpeta del proyecto, use la ruta de acceso a la carpeta *publish* para crear el servicio. En el ejemplo siguiente:

   * El proyecto reside en la carpeta *c:\\my_services\\AspNetCoreService*.
   * El proyecto se publica en la configuración `Release`.
   * El moniker de la plataforma de destino (TFM) es `netcoreapp2.1`.
   * El identificador del entorno de ejecución (RID) es `win7-x64`.
   * El ejecutable de la aplicación se denomina *AspNetCoreService.exe*.
   * El servicio se denomina **MyService**.

   Ejemplo:

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```

   > [!IMPORTANT]
   > No olvide incluir el espacio entre el argumento `binPath=` y su valor.

   Para publicar e iniciar el servicio desde otra carpeta:

      * Use la opción [--output &lt;DIRECTORIO_DE_SALIDA&gt;](/dotnet/core/tools/dotnet-publish#options) en el comando `dotnet publish`. Si utiliza Visual Studio, configure el valor **Ubicación de destino** de la página de la propiedad de publicación **FolderProfile** antes de hacer clic en el botón **Publicar**.
      * Cree el servicio con el comando `sc.exe` utilizando la ruta de acceso de la carpeta de salida. Incluya el nombre del archivo ejecutable del servicio en la ruta de acceso proporcionada a `binPath`.

1. Inicie el servicio con el comando `sc start <SERVICE_NAME>`.

   Use el siguiente comando para iniciar el servicio de la aplicación de ejemplo:

   ```console
   sc start MyService
   ```

   Este comando tarda unos segundos en iniciar el servicio.

1. Para comprobar el estado del servicio, use el comando `sc query <SERVICE_NAME>`. El estado se notifica como uno de los siguientes valores:

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

## <a name="run-the-app-outside-of-a-service"></a>Ejecutar la aplicación fuera de un servicio

Probar y depurar una aplicación resulta más sencillo cuando se ejecuta fuera de un servicio, por lo que es habitual agregar código que llame a `RunAsService` solo bajo determinadas condiciones. Por ejemplo, la aplicación se puede ejecutar como una aplicación de consola con un argumento de línea de comandos `--console` o si el depurador está asociado:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

Dado que la configuración de ASP.NET Core requiere pares nombre-valor en los argumentos de línea de comandos, el conmutador `--console` se quita antes de que los argumentos se pasen a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

> [!NOTE]
> `isService` no se pasa de `Main` a `CreateWebHostBuilder` porque la firma de `CreateWebHostBuilder` debe ser `CreateWebHostBuilder(string[])` para que las [pruebas de integración](xref:test/integration-tests) funcionen correctamente.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a>Controlar los eventos de inicio y detención

Para controlar los eventos [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) y [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), haga los siguientes cambios adicionales:

1. Cree una clase que se derive de la clase [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. Cree un método de extensión para [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) que pase el elemento `WebHostService` personalizado a [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. En `Program.Main`, llame al nuevo método de extensión, `RunAsCustomService`, en lugar de a [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > `isService` no se pasa de `Main` a `CreateWebHostBuilder` porque la firma de `CreateWebHostBuilder` debe ser `CreateWebHostBuilder(string[])` para que las [pruebas de integración](xref:test/integration-tests) funcionen correctamente.

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

Si el código `WebHostService` personalizado requiere un servicio de inserción de dependencias (como un registrador), obténgalo de la propiedad [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Escenarios de servidor proxy y equilibrador de carga

Los servicios que interactúan con las solicitudes de Internet o de una red corporativa y están detrás de un proxy o de un equilibrador de carga podrían requerir configuración adicional. Para obtener más información, vea <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-https"></a>Configuración de HTTPS

Especifique una [configuración de punto de conexión HTTPS de servidor Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).

## <a name="current-directory-and-content-root"></a>Directorio actual y raíz del contenido

El directorio de trabajo actual devuelto por una llamada a `Directory.GetCurrentDirectory()` para un servicio de Windows es la carpeta *C:\\WINDOWS\\system32*. La carpeta *system32* no es una ubicación adecuada para almacenar los archivos de un servicio (por ejemplo los archivos de configuración). Use uno de los enfoques siguientes para mantener y acceder a los recursos y los archivos de configuración de un servicio con [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) cuando se usa una interfaz [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):

* Use la ruta de acceso raíz del contenido. `IHostingEnvironment.ContentRootPath` es la misma ruta de acceso proporcionada al argumento `binPath` cuando se crea el servicio. En lugar de usar `Directory.GetCurrentDirectory()` para crear rutas de acceso a los archivos de configuración, use la ruta de acceso raíz del contenido y mantenga los archivos en la raíz de contenido de la aplicación.
* Almacene los archivos en una ubicación adecuada en el disco. Especifique una ruta de acceso absoluta con `SetBasePath` a la carpeta que contiene los archivos.

## <a name="additional-resources"></a>Recursos adicionales

* [Kestrel: configuración de punto de conexión](xref:fundamentals/servers/kestrel#endpoint-configuration) (configuración de HTTPS y compatibilidad de SNI incluidas)
* <xref:fundamentals/host/web-host>
