---
title: Hospedaje de ASP.NET Core en un servicio de Windows
author: guardrex
description: Aprenda a hospedar una aplicación ASP.NET Core en un servicio de Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/30/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 11913019bfe5d06c259b806fce9cc580a8280ad5
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758198"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Hospedaje de ASP.NET Core en un servicio de Windows

Por [Luke Latham](https://github.com/guardrex) y [Tom Dykstra](https://github.com/tdykstra)

Una aplicación de ASP.NET Core se puede hospedar en Windows sin usar IIS como [servicio de Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Cuando se aloja como un servicio de Windows, la aplicación se inicia automáticamente después de reiniciar el equipo.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="convert-a-project-into-a-windows-service"></a>Convertir un proyecto en un servicio de Windows

Estos son los cambios mínimos necesarios para configurar un proyecto de ASP.NET Core existente para que se ejecute como un servicio:

1. El archivo del proyecto:

   * Confirme la presencia del [identificador en tiempo de ejecución](/dotnet/core/rid-catalog) de Windows o agréguelo al `<PropertyGroup>` que contiene la plataforma de destino:

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.2</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      Para publicar para varios RID:

      * Proporcione los RID en una lista delimitada por punto y coma.
      * Use el nombre de la propiedad `<RuntimeIdentifiers>` (plural).

      Para más información, vea el [Catálogo de identificadores de entorno de ejecución (RID) de .NET Core](/dotnet/core/rid-catalog).

   * Agregue una referencia de paquete de [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).

1. Realice los siguientes cambios en `Program.Main`.

   * Llame a [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) en lugar de a `host.Run`.

   * Llame a [UseContentRoot](xref:fundamentals/host/web-host#content-root) y use una ruta de acceso a la ubicación de publicación de la aplicación en lugar de `Directory.GetCurrentDirectory()`.

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

1. Publique la aplicación con [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), un [perfil de publicación de Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) o Visual Studio Code. Al usar Visual Studio, seleccione **FolderProfile** y configure la **Ubicación de destino** antes de seleccionar el botón **Publicar**.

   Para publicar la aplicación de ejemplo mediante herramientas de la interfaz de la línea de comandos (CLI), ejecute el comando [dotnet publish](/dotnet/core/tools/dotnet-publish) en un símbolo del sistema desde la carpeta del proyecto. El identificador relativo debe especificarse en la propiedad `<RuntimeIdenfifier>` o `<RuntimeIdentifiers>` del archivo del proyecto. En el ejemplo siguiente, la aplicación se publica en la configuración de lanzamiento para `win7-x64` en tiempo de ejecución en una carpeta creada en *c:\\svc*:

   ```console
   dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
   ```

1. Cree una cuenta de usuario para el servicio con el comando `net user`:

   ```console
   net user {USER ACCOUNT} {PASSWORD} /add
   ```

   Para la aplicación de ejemplo, cree una cuenta de usuario con el nombre `ServiceUser` y una contraseña. En el comando siguiente, reemplace `{PASSWORD}` por una [contraseña segura](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).

   ```console
   net user ServiceUser {PASSWORD} /add
   ```

   Si necesita agregar el usuario a un grupo, use el comando `net localgroup`, donde `{GROUP}` es el nombre del grupo:

   ```console
   net localgroup {GROUP} {USER ACCOUNT} /add
   ```

   Para más información, vea [Service User Accounts](/windows/desktop/services/service-user-accounts) (Cuentas de usuario de servicio).

1. Conceda acceso de escritura, lectura y ejecución a la carpeta de la aplicación con el comando [icacls](/windows-server/administration/windows-commands/icacls):

   ```console
   icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
   ```

   * `{PATH}`: ruta de acceso a la carpeta de la aplicación.
   * `{USER ACCOUNT}`: cuenta del usuario (SID).
   * `(OI)`: la marca Object Inherit (Heredar objetos) propaga los permisos a los archivos subordinados.
   * `(CI)`: la marca Container Inherit (Heredar contenedores) propaga los permisos a las carpetas subordinadas.
   * `{PERMISSION FLAGS}`: define los permisos de acceso a la aplicación.
     * Escritura (`W`)
     * Lectura (`R`)
     * Ejecución (`X`)
     * Total (`F`)
     * Modificación (`M`)
   * `/t`: se aplica de forma recursiva a las carpetas y los archivos subordinados existentes.

   Para la aplicación de ejemplo publicada en la carpeta *c:\\svc* y en la cuenta `ServiceUser` con permisos de escritura, lectura y ejecución, use el siguiente comando:

   ```console
   icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
   ```

   Para más información, vea [icacls](/windows-server/administration/windows-commands/icacls).

1. Use la herramienta de línea de comandos [sc.exe](https://technet.microsoft.com/library/bb490995) para crear el servicio. El valor `binPath` es la ruta de acceso al archivo ejecutable de la aplicación, que incluye el nombre del archivo ejecutable. **El espacio entre el signo igual y las comillas de cada parámetro y valor es obligatorio.**

   ```console
   sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
   ```

   * `{SERVICE NAME}`: nombre para asignar al servicio en el [Administrador de control de servicios](/windows/desktop/services/service-control-manager).
   * `{PATH}`: ruta de acceso al archivo ejecutable del servicio.
   * `{DOMAIN}`, o si la máquina no está unida a un dominio, el nombre de la máquina local, y `{USER ACCOUNT}`: el dominio o el nombre de la máquina local y la cuenta de usuario en los que el servicio se ejecuta. No **omita** el parámetro `obj`. El valor predeterminado de `obj` es la cuenta [LocalSystem](/windows/desktop/services/localsystem-account). Ejecutar un servicio en la cuenta `LocalSystem` plantea un riesgo de seguridad importante. Ejecute siempre un servicio en una cuenta de usuario con privilegios restringidos en el servidor.
   * `{PASSWORD}`: contraseña de la cuenta de usuario.

   En el ejemplo siguiente:

   * El servicio se denomina **MyService**.
   * El servicio publicado reside en la carpeta *c:\\svc*. El ejecutable de la aplicación se denomina *AspNetCoreService.exe*. El valor `binPath` se encuentra entre comillas rectas (").
   * El servicio se ejecuta en la cuenta `ServiceUser`. Reemplace `{DOMAIN}` por el dominio de la cuenta de usuario o el nombre de la máquina local. Encierre el valor `obj` entre comillas rectas ("). Ejemplo: si el sistema de hospedaje es una máquina local denominada `MairaPC`, establezca `obj` en `"MairaPC\ServiceUser"`.
   * Reemplace `{PASSWORD}` con la contraseña de la cuenta de usuario. El valor `password` se encuentra entre comillas rectas (").

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
   ```

   > [!IMPORTANT]
   > Asegúrese de que existen los espacios entre los signos igual de los parámetros y los valores de los parámetros.

1. Inicie el servicio con el comando `sc start {SERVICE NAME}`.

   Use el siguiente comando para iniciar el servicio de la aplicación de ejemplo:

   ```console
   sc start MyService
   ```

   Este comando tarda unos segundos en iniciar el servicio.

1. Para comprobar el estado del servicio, use el comando `sc query {SERVICE NAME}`. El estado se notifica como uno de los siguientes valores:

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

1. Detenga el servicio con el comando `sc stop {SERVICE NAME}`.

   Con el siguiente comando se detiene el servicio de la aplicación de ejemplo:

   ```console
   sc stop MyService
   ```

1. Tras un breve intervalo para detener un servicio, desinstale el servicio con el comando `sc delete {SERVICE NAME}`.

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

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

Dado que la configuración de ASP.NET Core requiere pares nombre-valor en los argumentos de línea de comandos, el conmutador `--console` se quita antes de que los argumentos se pasen a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

> [!NOTE]
> `isService` no se pasa de `Main` a `CreateWebHostBuilder` porque la firma de `CreateWebHostBuilder` debe ser `CreateWebHostBuilder(string[])` para que las [pruebas de integración](xref:test/integration-tests) funcionen correctamente.

## <a name="handle-stopping-and-starting-events"></a>Controlar los eventos de inicio y detención

Para controlar los eventos [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) y [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), haga los siguientes cambios adicionales:

1. Cree una clase que se derive de la clase [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. Cree un método de extensión para [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) que pase el elemento `WebHostService` personalizado a [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. En `Program.Main`, llame al nuevo método de extensión, `RunAsCustomService`, en lugar de a [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > `isService` no se pasa de `Main` a `CreateWebHostBuilder` porque la firma de `CreateWebHostBuilder` debe ser `CreateWebHostBuilder(string[])` para que las [pruebas de integración](xref:test/integration-tests) funcionen correctamente.

Si el código `WebHostService` personalizado requiere un servicio de inserción de dependencias (como un registrador), obténgalo de la propiedad [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Escenarios de servidor proxy y equilibrador de carga

Los servicios que interactúan con las solicitudes de Internet o de una red corporativa y están detrás de un proxy o de un equilibrador de carga podrían requerir configuración adicional. Para obtener más información, vea <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-https"></a>Configuración de HTTPS

Para configurar el servicio con un punto de conexión seguro:

1. Cree un certificado X.509 para el sistema de hospedaje mediante la adquisición del certificado de la plataforma y con mecanismos de implementación.
1. Especifique una [configuración de punto de conexión HTTPS de servidor Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) para usar el certificado.

No se admite el uso del certificado de desarrollo HTTPS de ASP.NET Core para proteger un punto de conexión de servicio.

## <a name="current-directory-and-content-root"></a>Directorio actual y raíz del contenido

El directorio de trabajo actual devuelto por una llamada a `Directory.GetCurrentDirectory()` para un servicio de Windows es la carpeta *C:\\WINDOWS\\system32*. La carpeta *system32* no es una ubicación adecuada para almacenar los archivos de un servicio (por ejemplo los archivos de configuración). Use uno de los enfoques siguientes para mantener y acceder a los recursos y los archivos de configuración de un servicio con [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) cuando se usa una interfaz [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):

* Use la ruta de acceso raíz del contenido. `IHostingEnvironment.ContentRootPath` es la misma ruta de acceso proporcionada al argumento `binPath` cuando se crea el servicio. En lugar de usar `Directory.GetCurrentDirectory()` para crear rutas de acceso a los archivos de configuración, use la ruta de acceso raíz del contenido y mantenga los archivos en la raíz de contenido de la aplicación.
* Almacene los archivos en una ubicación adecuada en el disco. Especifique una ruta de acceso absoluta con `SetBasePath` a la carpeta que contiene los archivos.

## <a name="additional-resources"></a>Recursos adicionales

* [Kestrel: configuración de punto de conexión](xref:fundamentals/servers/kestrel#endpoint-configuration) (configuración de HTTPS y compatibilidad de SNI incluidas)
* <xref:fundamentals/host/web-host>
