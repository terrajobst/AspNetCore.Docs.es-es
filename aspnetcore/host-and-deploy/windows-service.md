---
title: Hospedaje de ASP.NET Core en un servicio de Windows
author: guardrex
description: Aprenda a hospedar una aplicación ASP.NET Core en un servicio de Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/21/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: ab36bc1b2827c80bb1e7b9e8cee558b346a991f8
ms.sourcegitcommit: b8ed594ab9f47fa32510574f3e1b210cff000967
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66251427"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Hospedaje de ASP.NET Core en un servicio de Windows

Por [Luke Latham](https://github.com/guardrex) y [Tom Dykstra](https://github.com/tdykstra)

Una aplicación de ASP.NET Core se puede hospedar en Windows sin usar IIS como [servicio de Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Cuando se hospeda como un servicio de Windows, la aplicación se inicia automáticamente después de reiniciar el servidor.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Requisitos previos

* [ASP.NET Core SDK 2.1 o posterior](https://dotnet.microsoft.com/download)
* [PowerShell 6.2 o posterior](https://github.com/PowerShell/PowerShell)

## <a name="app-configuration"></a>Configuración de aplicaciones

::: moniker range=">= aspnetcore-3.0"

Se llama a `IHostBuilder.UseWindowsService`, proporcionado por el paquete [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices), cuando se crea el host. Si la aplicación se ejecuta como un servicio de Windows, el método:

* Establece la vigencia del host en `WindowsServiceLifetime`.
* Establece la raíz de contenido.
* Habilita el registro en el registro de eventos con el nombre de la aplicación como nombre de origen predeterminado.
  * El nivel de registro puede configurarse con la clave `Logging:LogLevel:Default` en el archivo *appsettings.Production.json*.
  * Los administradores son los únicos que pueden crear nuevos orígenes de eventos. Cuando no se puede crear un origen de eventos con el nombre de la aplicación, se registra una advertencia para el origen *Aplicación* y los registros de eventos se deshabilitan.

[!code-csharp[](windows-service/samples/3.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

La aplicación requiere referencias de paquetes para [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) y [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).

Para probar y depurar cuando se ejecuta fuera de un servicio, agregue código para determinar si la aplicación se ejecuta como un servicio o una aplicación de consola. Compruebe si el depurador está asociado o hay presente un conmutador `--console`. Si alguna de estas condiciones se cumple (la aplicación no se ejecuta como un servicio), llame a <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>. Si las condiciones no se cumplen (la aplicación se ejecuta como servicio):

* Llame a <xref:System.IO.Directory.SetCurrentDirectory*> y use una ruta de acceso a la ubicación de publicación de la aplicación. No llame a <xref:System.IO.Directory.GetCurrentDirectory*> para obtener la ruta de acceso porque una aplicación de servicio de Windows devuelve una carpeta *C:\\WINDOWS\\system32* cuando se llama a <xref:System.IO.Directory.GetCurrentDirectory*>. Para obtener más información, consulte la sección [Directorio actual y raíz del contenido](#current-directory-and-content-root). Este paso se realiza antes de que la aplicación se configure en `CreateWebHostBuilder`.
* Llame a <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> para ejecutar la aplicación como un servicio.

Dado que el [Proveedor de configuración de línea de comandos](xref:fundamentals/configuration/index#command-line-configuration-provider) requiere pares nombre-valor en los argumentos de línea de comandos, el conmutador `--console` se quita de los argumentos antes de que <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> los reciba.

Para escribir en el registro de eventos de Windows, agregue el proveedor EventLog a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>. Establezca el nivel de registro con la clave `Logging:LogLevel:Default` en el archivo *appsettings.Production.json*.

En el ejemplo siguiente de la aplicación de ejemplo, se llama a `RunAsCustomService` en lugar de a <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> con el fin de controlar los eventos de duración dentro de la aplicación. Para obtener más información, consulte la sección [Control de los eventos de inicio y detención](#handle-starting-and-stopping-events).

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a>Tipo de implementación

Para obtener información y consejos sobre los escenarios de implementación, consulte [Implementación de aplicaciones .NET Core](/dotnet/core/deploying/).

### <a name="framework-dependent-deployment-fdd"></a>Implementación dependiente de marco (FDD)

La implementación dependiente de marco de trabajo (FDD) se basa en la presencia de una versión compartida de .NET Core en todo el sistema en el sistema de destino. Cuando se adopta el escenario FDD siguiendo las instrucciones de este artículo, el SDK genera un archivo ejecutable ( *.exe*), denominado *ejecutable dependiente del marco*.

::: moniker range=">= aspnetcore-3.0"

Agregue los siguientes elementos de propiedad al archivo del proyecto:

* `<OutputType>`&ndash; Tipo de salida de la aplicación (`Exe` para ejecutable).
* `<LangVersion>`&ndash; Versión del lenguaje C# (`latest` o `preview`).

No se requiere un archivo *web.config*, que normalmente se produce cuando se publica una aplicación ASP.NET Core, para una aplicación de Windows Services. Para deshabilitar la creación de un archivo *web.config* agregue la propiedad `<IsTransformWebConfigDisabled>` establecida en `true`.

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <OutputType>Exe</OutputType>
  <LangVersion>preview</LangVersion>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

El [identificador en tiempo de ejecución (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contiene la plataforma de destino. En el ejemplo siguiente, el RID se establece en `win7-x64`. La propiedad `<SelfContained>` se establece en `false`. Estas propiedades indican al SDK que genere un archivo ejecutable ( *.exe*) para Windows y una aplicación que depende del marco .NET Core compartido.

No se requiere un archivo *web.config*, que normalmente se produce cuando se publica una aplicación ASP.NET Core, para una aplicación de Windows Services. Para deshabilitar la creación de un archivo *web.config* agregue la propiedad `<IsTransformWebConfigDisabled>` establecida en `true`.

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

El [identificador en tiempo de ejecución (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contiene la plataforma de destino. En el ejemplo siguiente, el RID se establece en `win7-x64`. La propiedad `<SelfContained>` se establece en `false`. Estas propiedades indican al SDK que genere un archivo ejecutable ( *.exe*) para Windows y una aplicación que depende del marco .NET Core compartido.

La propiedad `<UseAppHost>` se establece en `true`. Esta propiedad proporciona el servicio con una ruta de acceso de activación (un archivo ejecutable, *.exe*) para una FDD.

No se requiere un archivo *web.config*, que normalmente se produce cuando se publica una aplicación ASP.NET Core, para una aplicación de Windows Services. Para deshabilitar la creación de un archivo *web.config* agregue la propiedad `<IsTransformWebConfigDisabled>` establecida en `true`.

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

### <a name="self-contained-deployment-scd"></a>Implementación autocontenida (SCD)

Una implementación autocontenida (SCD) no depende de la presencia de un marco compartido en el sistema host. El tiempo de ejecución y las dependencias de la aplicación se implementan con la aplicación.

Un [identificador en tiempo de ejecución (RID)](/dotnet/core/rid-catalog) se incluye en el `<PropertyGroup>` que contiene la plataforma de destino:

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

Para publicar para varios RID:

* Proporcione los RID en una lista delimitada por punto y coma.
* Utilice el nombre de propiedad [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (en plural).

Para más información, vea el [Catálogo de identificadores de entorno de ejecución (RID) de .NET Core](/dotnet/core/rid-catalog).

::: moniker range="< aspnetcore-3.0"

Una propiedad `<SelfContained>` se establece en `true`:

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a>Cuenta de usuario de servicio

Para crear una cuenta de usuario para un servicio, use el cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) de un shell de comandos administrativos de PowerShell 6.

En la actualización de octubre de 2018 de Windows 10 (versión 1809/compilación 10.0.17763) o posterior:

```PowerShell
New-LocalUser -Name {NAME}
```

En el sistema operativo Windows anterior a la actualización de octubre de 2018 de Windows 10 (versión 1809/compilación 10.0.17763):

```console
powershell -Command "New-LocalUser -Name {NAME}"
```

Proporcione una [contraseña segura](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) cuando se le solicite.

A menos que el parámetro `-AccountExpires` se proporcione para el cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) con una fecha de expiración <xref:System.DateTime>, la cuenta no expirará.

Para más información, vea [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) y [Service User Accounts](/windows/desktop/services/service-user-accounts) (Cuentas de usuario del servicio).

Un enfoque alternativo de administración de usuarios al usar Active Directory consiste en usar cuentas de servicio administradas. Para obtener más información, consulte [grupo Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview) (Información general sobre cuentas de servicio administradas de grupo).

## <a name="log-on-as-a-service-rights"></a>Derechos para iniciar sesión como servicio

Para establecer los derechos de *Iniciar sesión como servicio* para una cuenta de usuario del servicio:

1. Abra el editor de la Directiva de seguridad local mediante la ejecución de *secpool.msc*.
1. Expanda el nodo **Directivas locales** y, después, seleccione **Asignación de derechos de usuario**.
1. Abra la directiva **Iniciar sesión como servicio**.
1. Seleccione **Agregar usuario o grupo**.
1. Proporcione el nombre de objeto (cuenta de usuario) mediante cualquiera de los métodos siguientes:
   1. Escriba la cuenta de usuario (`{DOMAIN OR COMPUTER NAME\USER}`) en el campo del nombre de objeto y seleccione **Aceptar** para agregar el usuario a la directiva.
   1. Seleccione **Opciones avanzadas**. Seleccione **Buscar ahora**. Seleccione la cuenta de usuario de la lista. Seleccione **Aceptar**. Seleccione **Aceptar** nuevo para agregar el usuario a la directiva.
1. Seleccione **Aceptar** o **Aplicar** para aceptar los cambios.

## <a name="create-and-manage-the-windows-service"></a>Creación y administración del servicio de Windows

### <a name="create-a-service"></a>Creación de un servicio

Use los comandos de PowerShell para registrar un servicio. Desde un shell de comandos administrativos de PowerShell 6, ejecute los comandos siguientes:

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* `{EXE PATH}`&ndash; Ruta de acceso a la carpeta de la aplicación en el host (por ejemplo, `d:\myservice`). No incluya el archivo ejecutable de la aplicación en la ruta de acceso. No se requiere una barra diagonal al final.
* `{DOMAIN OR COMPUTER NAME\USER}` &ndash;Cuenta de usuario de servicio (por ejemplo, `Contoso\ServiceUser`).
* `{NAME}` &ndash;Nombre de servicio (por ejemplo, `MyService`).
* `{EXE FILE PATH}` &ndash; Ruta de acceso del ejecutable de la aplicación (por ejemplo, `d:\myservice\myservice.exe`). Incluya el nombre de archivo del ejecutable con la extensión.
* `{DESCRIPTION}` &ndash; Descripción del servicio (por ejemplo, `My sample service`).
* `{DISPLAY NAME}` &ndash; Nombre para mostrar del servicio (por ejemplo, `My Service`).

### <a name="start-a-service"></a>Inicio de un servicio

Inicie el servicio con el siguiente comando de PowerShell 6:

```powershell
Start-Service -Name {NAME}
```

Este comando tarda unos segundos en iniciar el servicio.

### <a name="determine-a-services-status"></a>Determinación del estado de un servicio

Para comprobar el estado de un servicio, use el siguiente comando de PowerShell 6:

```powershell
Get-Service -Name {NAME}
```

El estado se notifica como uno de los siguientes valores:

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a>Detención de un servicio

Detenga un servicio con el siguiente comando de PowerShell 6:

```powershell
Stop-Service -Name {NAME}
```

### <a name="remove-a-service"></a>Eliminación de un servicio

Tras un breve intervalo para detener un servicio, elimine el servicio con el siguiente comando de PowerShell 6:

```powershell
Remove-Service -Name {NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a>Control de los eventos de inicio y detención

Para controlar los eventos <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> y <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>:

1. Cree una clase que se derive de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> con los métodos `OnStarting`, `OnStarted` y `OnStopping`:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. Cree un método de extensión para <xref:Microsoft.AspNetCore.Hosting.IWebHost> que pase `CustomWebHostService` a <xref:System.ServiceProcess.ServiceBase.Run*>:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. En `Program.Main`, llame al método de extensión `RunAsCustomService` en lugar de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:

   ```csharp
   host.RunAsCustomService();
   ```

   Para ver la ubicación de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> en `Program.Main`, consulte el ejemplo de código que se muestra en la sección [Tipo de implementación](#deployment-type).

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a>Escenarios de servidor proxy y equilibrador de carga

Los servicios que interactúan con las solicitudes de Internet o de una red corporativa y están detrás de un proxy o de un equilibrador de carga podrían requerir configuración adicional. Para obtener más información, vea <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-https"></a>Configuración de HTTPS

Para configurar el servicio con un punto de conexión seguro:

1. Cree un certificado X.509 para el sistema de hospedaje mediante la adquisición del certificado de la plataforma y con mecanismos de implementación.

1. Especifique una [configuración de punto de conexión HTTPS de servidor Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) para usar el certificado.

No se admite el uso del certificado de desarrollo HTTPS de ASP.NET Core para proteger un punto de conexión de servicio.

## <a name="current-directory-and-content-root"></a>Directorio actual y raíz del contenido

El directorio de trabajo actual devuelto por una llamada a <xref:System.IO.Directory.GetCurrentDirectory*> para un servicio de Windows es la carpeta *C:\\WINDOWS\\system32*. La carpeta *system32* no es una ubicación adecuada para almacenar los archivos de un servicio (por ejemplo los archivos de configuración). Utilice uno de los siguientes enfoques para mantener los archivos de configuración y los activos de un servicio, así como para acceder a ellos.

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a>Uso de ContentRootPath o ContentRootFileProvider

Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) o <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> para buscar los recursos de una aplicación.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a>Configuración de la ruta de acceso raíz del contenido en la carpeta de la aplicación

<xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> es la misma ruta de acceso proporcionada al argumento `binPath` cuando se crea un servicio. En lugar de llamar `GetCurrentDirectory` para crear rutas de acceso a los archivos de configuración, llame a <xref:System.IO.Directory.SetCurrentDirectory*> con la ruta de acceso a la raíz del contenido de la aplicación.

En `Program.Main`, determine la ruta de acceso a la carpeta del archivo ejecutable del servicio y use la ruta de acceso para establecer la raíz del contenido de la aplicación:

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a>Almacenamiento de los archivos de un servicio en una ubicación adecuada en el disco

Especifique una ruta de acceso absoluta con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> al utilizar un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> a la carpeta que contiene los archivos.

## <a name="additional-resources"></a>Recursos adicionales

::: moniker range=">= aspnetcore-3.0"

* [Kestrel: configuración de punto de conexión](xref:fundamentals/servers/kestrel#endpoint-configuration) (configuración de HTTPS y compatibilidad de SNI incluidas)
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* [Kestrel: configuración de punto de conexión](xref:fundamentals/servers/kestrel#endpoint-configuration) (configuración de HTTPS y compatibilidad de SNI incluidas)
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
