---
title: Hospedaje de ASP.NET Core en un servicio de Windows
author: guardrex
description: Aprenda a hospedar una aplicación ASP.NET Core en un servicio de Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/01/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: f53c303dc63e092f08e933fea79eb805523cde9b
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861399"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Hospedaje de ASP.NET Core en un servicio de Windows

Por [Luke Latham](https://github.com/guardrex) y [Tom Dykstra](https://github.com/tdykstra)

Una aplicación de ASP.NET Core se puede hospedar en Windows sin usar IIS como [servicio de Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Cuando se aloja como un servicio de Windows, la aplicación se inicia automáticamente después de reiniciar el equipo.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="deployment-type"></a>Tipo de implementación

Puede crear una implementación del servicio de Windows dependiente del marco o autocontenida. Para obtener información y consejos sobre los escenarios de implementación, consulte [Implementación de aplicaciones .NET Core](/dotnet/core/deploying/).

### <a name="framework-dependent-deployment"></a>Implementación dependiente de marco de trabajo

La implementación dependiente de marco de trabajo (FDD) se basa en la presencia de una versión compartida de .NET Core en todo el sistema en el sistema de destino. Cuando se utiliza el escenario FDD con una aplicación de servicio de Windows de ASP.NET Core, el SDK genera un archivo ejecutable (*\*.exe*), denominado *ejecutable dependiente del marco*.

### <a name="self-contained-deployment"></a>Implementación autocontenida

Una implementación autocontenida (SCD) no depende de la presencia de los componentes compartidos en el sistema de destino. El tiempo de ejecución y las dependencias de la aplicación se implementan con la aplicación en el sistema host.

## <a name="convert-a-project-into-a-windows-service"></a>Convertir un proyecto en un servicio de Windows

Realice los cambios siguientes en un proyecto de ASP.NET Core existente para ejecutar la aplicación como un servicio:

### <a name="project-file-updates"></a>Actualizaciones del archivo del proyecto

En función de su elección de [tipo de implementación](#deployment-type), actualice el archivo de proyecto:

#### <a name="framework-dependent-deployment-fdd"></a>Implementación dependiente de marco (FDD)

Agregue un [identificador en tiempo de ejecución (RID)](/dotnet/core/rid-catalog) de Windows al `<PropertyGroup>` que contiene la plataforma de destino. Agregue la propiedad `<SelfContained>` establecida en `false`. Deshabilite la creación de un archivo *web.config* agregando la propiedad `<IsTransformWebConfigDisabled>` establecida en `true`.

::: moniker range=">= aspnetcore-2.2"

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.1</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

#### <a name="self-contained-deployment-scd"></a>Implementación autocontenida (SCD)

Confirme la presencia del [identificador en tiempo de ejecución (RID)](/dotnet/core/rid-catalog) de Windows o agregue un RID al `<PropertyGroup>` que contiene la plataforma de destino. Deshabilite la creación de un archivo *web.config* agregando la propiedad `<IsTransformWebConfigDisabled>` establecida en `true`.

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Para publicar para varios RID:

* Proporcione los RID en una lista delimitada por punto y coma.
* Use el nombre de la propiedad `<RuntimeIdentifiers>` (plural).

  Para más información, vea el [Catálogo de identificadores de entorno de ejecución (RID) de .NET Core](/dotnet/core/rid-catalog).

Agregue una referencia de paquete de [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).

Para habilitar el registro de eventos de Windows, agregue una referencia de paquete para [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).

Para obtener más información, consulte la sección [Control de los eventos de inicio y detención](#handle-starting-and-stopping-events).

### <a name="programmain-updates"></a>Actualizaciones de Program.Main

Realice los siguientes cambios en `Program.Main`.

* Para probar y depurar cuando se ejecuta fuera de un servicio, agregue código para determinar si la aplicación se ejecuta como un servicio o una aplicación de consola. Compruebe si el depurador está asociado o hay presente un argumento de línea de comandos `--console`.

  Si alguna de estas condiciones se cumple (la aplicación no se ejecuta como un servicio), llame a <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> en el host web.

  Si las condiciones no se cumplen (la aplicación se ejecuta como servicio):

  * Llame a <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> y use una ruta de acceso a la ubicación de publicación de la aplicación. No llame a <xref:System.IO.Directory.GetCurrentDirectory*> para obtener la ruta de acceso porque una aplicación de servicio de Windows devuelve una carpeta *C:\\WINDOWS\\system32* cuando se llama a `GetCurrentDirectory`. Para obtener más información, consulte la sección [Directorio actual y raíz del contenido](#current-directory-and-content-root).
  * Llame a <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> para ejecutar la aplicación como un servicio.

  Dado que el [Proveedor de configuración de línea de comandos](xref:fundamentals/configuration/index#command-line-configuration-provider) requiere pares nombre-valor en los argumentos de línea de comandos, el conmutador `--console` se quita de los argumentos antes de que <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> los reciba.

* Para escribir en el registro de eventos de Windows, agregue el proveedor EventLog a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>. Establezca el nivel de registro con la clave `Logging:LogLevel:Default` en el archivo *appsettings.Production.json*. Para fines de prueba y demostración, el archivo de configuración de producción de la aplicación de ejemplo establece el nivel de registro en `Information`. En producción, el valor se establece normalmente en `Error`. Para obtener más información, vea <xref:fundamentals/logging/index#windows-eventlog-provider>.

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

### <a name="publish-the-app"></a>Publicar la aplicación

Publique la aplicación con [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), un [perfil de publicación de Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) o Visual Studio Code. Al usar Visual Studio, seleccione **FolderProfile** y configure la **Ubicación de destino** antes de seleccionar el botón **Publicar**.

Para publicar la aplicación de ejemplo mediante herramientas de la interfaz de la línea de comandos (CLI), ejecute el comando [dotnet publish](/dotnet/core/tools/dotnet-publish) en un símbolo del sistema desde la carpeta del proyecto con una configuración de versión pasada a la opción [-c|--configuration](/dotnet/core/tools/dotnet-publish#options). Use la opción [-o|--output](/dotnet/core/tools/dotnet-publish#options) con una ruta de acceso para publicar en una carpeta fuera de la aplicación.

#### <a name="publish-a-framework-dependent-deployment-fdd"></a>Publicación de una implementación dependiente de marco (FDD)

En el ejemplo siguiente, la aplicación está publicada para la carpeta *c:\\svc*:

```console
dotnet publish --configuration Release --output c:\svc
```

#### <a name="publish-a-self-contained-deployment-scd"></a>Publicación de una implementación autocontenida (SCD)

El identificador relativo debe especificarse en la propiedad `<RuntimeIdenfifier>` o `<RuntimeIdentifiers>` del archivo del proyecto. Proporcione el entorno de ejecución a la opción [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) del comando `dotnet publish`.

En el ejemplo siguiente, la aplicación está publicada para el entorno de ejecución `win7-x64` en la carpeta *c:\\svc*:

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

### <a name="create-a-user-account"></a>Creación de una cuenta de usuario

Cree una cuenta de usuario para el servicio con el comando `net user`:

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

### <a name="set-permissions"></a>Establecer permisos

Conceda acceso de escritura, lectura y ejecución a la carpeta de la aplicación con el comando [icacls](/windows-server/administration/windows-commands/icacls):

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

## <a name="manage-the-service"></a>Administración del servicio

### <a name="create-the-service"></a>Crear el servicio

Use la herramienta de línea de comandos [sc.exe](https://technet.microsoft.com/library/bb490995) para crear el servicio. El valor `binPath` es la ruta de acceso al archivo ejecutable de la aplicación, que incluye el nombre del archivo ejecutable. **El espacio entre el signo igual y las comillas de cada parámetro y valor es obligatorio.**

```console
sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
```

* `{SERVICE NAME}`: nombre para asignar al servicio en el [Administrador de control de servicios](/windows/desktop/services/service-control-manager).
* `{PATH}`: ruta de acceso al archivo ejecutable del servicio.
* `{DOMAIN}` &ndash; El dominio de una máquina unida al dominio. Si la máquina no está unida al dominio, el nombre del equipo local.
* `{USER ACCOUNT}` &ndash; La cuenta de usuario en la que se ejecuta el servicio.
* `{PASSWORD}`: contraseña de la cuenta de usuario.

> [!WARNING]
> No **omita** el parámetro `obj`. El valor predeterminado de `obj` es la cuenta [LocalSystem](/windows/desktop/services/localsystem-account). Ejecutar un servicio en la cuenta `LocalSystem` plantea un riesgo de seguridad importante. Ejecute siempre un servicio con una cuenta de usuario que tenga privilegios restringidos.

En el ejemplo siguiente de la aplicación de ejemplo:

* El servicio se denomina **MyService**.
* El servicio publicado reside en la carpeta *c:\\svc*. El ejecutable de la aplicación se denomina *SampleApp.exe*. Ponga el valor `binPath` entre comillas dobles (" ").
* El servicio se ejecuta en la cuenta `ServiceUser`. Reemplace `{DOMAIN}` por el dominio de la cuenta de usuario o el nombre de la máquina local. Ponga el valor `obj` entre comillas dobles (" "). Ejemplo: si el sistema de hospedaje es una máquina local denominada `MairaPC`, establezca `obj` en `"MairaPC\ServiceUser"`.
* Reemplace `{PASSWORD}` con la contraseña de la cuenta de usuario. Ponga el valor `password` entre comillas dobles (" ").

```console
sc create MyService binPath= "c:\svc\sampleapp.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
```

> [!IMPORTANT]
> Asegúrese de que existen los espacios entre los signos igual de los parámetros y los valores de los parámetros.

### <a name="start-the-service"></a>Inicio del servicio

Inicie el servicio con el comando `sc start {SERVICE NAME}`.

Use el siguiente comando para iniciar el servicio de la aplicación de ejemplo:

```console
sc start MyService
```

Este comando tarda unos segundos en iniciar el servicio.

### <a name="determine-the-service-status"></a>Determinación del estado del servicio

Para comprobar el estado del servicio, use el comando `sc query {SERVICE NAME}`. El estado se notifica como uno de los siguientes valores:

* `START_PENDING`
* `RUNNING`
* `STOP_PENDING`
* `STOPPED`

Use el siguiente comando para comprobar el estado del servicio de la aplicación de ejemplo:

```console
sc query MyService
```

### <a name="browse-a-web-app-service"></a>Examinar un servicio de aplicación web

Si el servicio está en estado `RUNNING` y dicho servicio es una aplicación web, vaya a la aplicación en su ruta de acceso correspondiente (de forma predeterminada, `http://localhost:5000`, que redirige a `https://localhost:5001` cuando se usa el [Middleware de redirección de HTTPS](xref:security/enforcing-ssl)).

Si es el servicio de la aplicación de ejemplo, vaya a la aplicación en `http://localhost:5000`.

### <a name="stop-the-service"></a>Detener el servicio

Detenga el servicio con el comando `sc stop {SERVICE NAME}`.

Con el siguiente comando se detiene el servicio de la aplicación de ejemplo:

```console
sc stop MyService
```

### <a name="delete-the-service"></a>Eliminación del servicio

Tras un breve intervalo para detener un servicio, desinstale el servicio con el comando `sc delete {SERVICE NAME}`.

Compruebe el estado del servicio de la aplicación de ejemplo:

```console
sc query MyService
```

Si el servicio de la aplicación de ejemplo está en estado `STOPPED`, use el siguiente comando para desinstalar el servicio de la aplicación de ejemplo:

```console
sc delete MyService
```

## <a name="handle-starting-and-stopping-events"></a>Control de los eventos de inicio y detención

Para controlar los eventos <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> y <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>, realice los siguientes cambios adicionales:

1. Cree una clase que se derive de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> con los métodos `OnStarting`, `OnStarted` y `OnStopping`:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. Cree un método de extensión para <xref:Microsoft.AspNetCore.Hosting.IWebHost> que pase `CustomWebHostService` a <xref:System.ServiceProcess.ServiceBase.Run*>:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. En `Program.Main`, llame al método de extensión `RunAsCustomService` en lugar de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:

   ```csharp
   host.RunAsCustomService();
   ```

   Para ver la ubicación de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> en `Program.Main`, consulte el ejemplo de código que se muestra en la sección [Convertir un proyecto en un servicio de Windows](#convert-a-project-into-a-windows-service).

## <a name="proxy-server-and-load-balancer-scenarios"></a>Escenarios de servidor proxy y equilibrador de carga

Los servicios que interactúan con las solicitudes de Internet o de una red corporativa y están detrás de un proxy o de un equilibrador de carga podrían requerir configuración adicional. Para obtener más información, vea <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-https"></a>Configuración de HTTPS

Para configurar el servicio con un punto de conexión seguro:

1. Cree un certificado X.509 para el sistema de hospedaje mediante la adquisición del certificado de la plataforma y con mecanismos de implementación.

1. Especifique una [configuración de punto de conexión HTTPS de servidor Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) para usar el certificado.

No se admite el uso del certificado de desarrollo HTTPS de ASP.NET Core para proteger un punto de conexión de servicio.

## <a name="current-directory-and-content-root"></a>Directorio actual y raíz del contenido

El directorio de trabajo actual devuelto por una llamada a <xref:System.IO.Directory.GetCurrentDirectory*> para un servicio de Windows es la carpeta *C:\\WINDOWS\\system32*. La carpeta *system32* no es una ubicación adecuada para almacenar los archivos de un servicio (por ejemplo los archivos de configuración). Utilice uno de los siguientes enfoques para mantener los archivos de configuración y los activos de un servicio, así como para acceder a ellos.

### <a name="set-the-content-root-path-to-the-apps-folder"></a>Configuración de la ruta de acceso raíz del contenido en la carpeta de la aplicación

<xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> es la misma ruta de acceso proporcionada al argumento `binPath` cuando se crea el servicio. En lugar de llamar `GetCurrentDirectory` para crear rutas de acceso a los archivos de configuración, llame a <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> con la ruta de acceso a la raíz del contenido de la aplicación.

En `Program.Main`, determine la ruta de acceso a la carpeta del archivo ejecutable del servicio y use la ruta de acceso para establecer la raíz del contenido de la aplicación:

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);

CreateWebHostBuilder(args)
    .UseContentRoot(pathToContentRoot)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a>Almacene los archivos del servicio en una ubicación adecuada en el disco.

Especifique una ruta de acceso absoluta con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> al utilizar un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> a la carpeta que contiene los archivos.

## <a name="additional-resources"></a>Recursos adicionales

* [Kestrel: configuración de punto de conexión](xref:fundamentals/servers/kestrel#endpoint-configuration) (configuración de HTTPS y compatibilidad de SNI incluidas)
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
