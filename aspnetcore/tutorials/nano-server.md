---
title: ASP.NET Core en Nano Server
author: shirhatti
description: "Aprenda a implementar una aplicación existente de ASP.NET Core en una instancia de Nano Server que ejecuta IIS."
keywords: ASP.NET Core,nano server
ms.author: riande
manager: wpickett
ms.date: 11/04/2016
ms.topic: article
ms.assetid: 50922cf1-ca58-4006-9236-99b7ff2dd0cf
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/nano-server
ms.openlocfilehash: dd1f2c8de58ea8d3a57e64ecc519184400cb52c8
ms.sourcegitcommit: ad01283f299d346cf757c4f4744c48634dc27e73
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2017
---
# <a name="aspnet-core-with-iis-on-nano-server"></a>ASP.NET Core con IIS en Nano Server

Por [Sourabh Shirhatti](https://twitter.com/sshirhatti)

En este tutorial tomaremos una aplicación existente de ASP.NET Core y la implementaremos en una instancia de Nano Server que ejecuta IIS.

## <a name="introduction"></a>Introducción

Nano Server es una opción de instalación de Windows Server 2016 que ofrece una ocupación mínima, mayor seguridad y un mejor servicio que Server Core o el servidor completo. Eche un vistazo a la [documentación oficial de Nano Server](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) para obtener más detalles y vínculos de descarga de las versiones de evaluación de 180 días. 

Existen tres formas sencillas de probar Nano Server. Cuando inicie sesión con su cuenta de MS:

1. Puede descargar el archivo ISO de Windows Server 2016 y crear una imagen de Nano Server.

2. Descargue el disco duro virtual de Nano Server.

3. Cree una máquina virtual en Azure con la imagen de Nano Server de la Galería de Azure. Si no tiene una cuenta de Azure, puede obtener una versión de prueba gratuita de 30 días.

En este tutorial, vamos a usar la segunda opción, el disco duro virtual de Nano Server compilado previamente desde Windows Server 2016.

Antes de continuar con este tutorial, necesitará la [salida publicada](xref:hosting/directory-structure) de una aplicación existente de ASP.NET Core. Asegúrese de que la aplicación está compilada para ejecutarse en un proceso de **64 bits**.

## <a name="setting-up-the-nano-server-instance"></a>Configuración de la instancia de Nano Server

[Cree una nueva máquina virtual mediante Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) en su máquina de desarrollo usando el disco duro virtual descargado anteriormente. La máquina le pedirá que establezca una contraseña de administrador antes de iniciar sesión. En la consola de VM, presione F11 para establecer la contraseña antes del primer inicio de sesión. También necesita comprobar la dirección IP de la nueva máquina virtual. Para ello compruebe la dirección IP fija del servidor DHCP proporcionada al aprovisionar la máquina virtual o compruebe la configuración de red de la consola de recuperación de Nano Server.

> [!NOTE]
> Supongamos que la nueva máquina virtual se ejecuta con la dirección IP V4 local 192.168.1.10.

Ahora puede administrarla mediante la comunicación remota de PowerShell, que es la única manera de administrar por completo Nano Server.

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a>Conectarse a la instancia de Nano Server usando la comunicación remota de PowerShell

Abra una ventana de PowerShell con privilegios elevados para agregar la instancia remota de Nano Server a la lista `TrustedHosts`.

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> Reemplace la variable `$nanoServerIpAddress` con la dirección IP correcta.

Una vez que haya agregado la instancia de Nano Server a `TrustedHosts`, puede conectarse a ella mediante la comunicación remota de PowerShell.

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

Cuando se realiza una conexión correcta, se muestra un símbolo del sistema parecido a este:`[192.168.1.10]: PS C:\Users\Administrator\Documents>`

## <a name="creating-a-file-share"></a>Crear un recurso compartido de archivos

Cree un recurso compartido de archivos en Nano Server para que la aplicación publicada pueda copiarse en él. Ejecute los siguientes comandos en la sesión remota:

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

Después de ejecutar los comandos anteriores, debería tener acceso a este recurso compartido si visita `\\192.168.1.10\AspNetCoreSampleForNano` en el Explorador de Windows de la máquina host.

## <a name="open-port-in-the-firewall"></a>Abrir el puerto en el firewall

Ejecute los siguientes comandos en la sesión remota para abrir un puerto en el firewall a fin de permitir que IIS escuche el tráfico TCP en el puerto TCP/8000.

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a>Instalar IIS

Agregue el proveedor `NanoServerPackage` desde la Galería de PowerShell. Cuando se haya instalado e importado el proveedor, podrá instalar los paquetes de Windows.

En la sesión de PowerShell que creó anteriormente, ejecute los siguientes comandos:

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

Para comprobar rápidamente si IIS está configurado correctamente, puede visitar la dirección URL `http://192.168.1.10/`, en la que debería ver una página de bienvenida. Cuando se instala IIS, se crea de manera predeterminada un sitio web denominado `Default Web Site` que escucha en el puerto 80.

## <a name="installing-the-aspnet-core-module-ancm"></a>Instalar el módulo de ASP.NET Core (ANCM)

El módulo de ASP.NET Core es un módulo de IIS 7.5+ que se encarga de la administración de procesos de las escuchas HTTP de ASP.NET Core y de enviar mediante proxy las solicitudes a los procesos que administra. En este momento, el proceso para instalar el módulo de ASP.NET Core para IIS es manual. Debe instalar el [conjunto de productos de hospedaje de Windows Server de .NET Core](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) en una máquina normal (que no sea Nano). Después de instalar el conjunto de productos en una máquina normal, debe copiar los siguientes archivos en el recurso compartido de archivos que hemos creado con anterioridad.

En un servidor normal (que no sea Nano) con IIS, ejecute los siguientes comandos de copia:

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

Reemplace `C:\windows\system32\inetsrv` con `C:\Program Files\IIS Express` en una máquina con Windows 10.

En el lado Nano, debe copiar los siguientes archivos desde el recurso compartido de archivos que hemos creado anteriormente a las ubicaciones válidas. Ejecute los siguientes comandos de copia:

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

Ejecute el siguiente script en la sesión remota:

```PowerShell
# Backup existing applicationHost.config
Copy-Item -Path C:\Windows\System32\inetsrv\config\applicationHost.config -Destination  C:\Windows\System32\inetsrv\config\applicationHost_BeforeInstallingANCM.config

Import-Module IISAdministration

# Initialize variables
$aspNetCoreHandlerFilePath="C:\windows\system32\inetsrv\aspnetcore.dll"
Reset-IISServerManager -confirm:$false
$sm = Get-IISServerManager

# Add AppSettings section 
$sm.GetApplicationHostConfiguration().RootSectionGroup.Sections.Add("appSettings")

# Set Allow for handlers section
$appHostconfig = $sm.GetApplicationHostConfiguration()
$section = $appHostconfig.GetSection("system.webServer/handlers")
$section.OverrideMode="Allow"

# Add aspNetCore section to system.webServer
$sectionaspNetCore = $appHostConfig.RootSectionGroup.SectionGroups["system.webServer"].Sections.Add("aspNetCore")
$sectionaspNetCore.OverrideModeDefault = "Allow"
$sm.CommitChanges()

# Configure globalModule
Reset-IISServerManager -confirm:$false
$globalModules = Get-IISConfigSection "system.webServer/globalModules" | Get-IISConfigCollection
New-IISConfigCollectionElement $globalModules -ConfigAttribute @{"name"="AspNetCoreModule";"image"=$aspNetCoreHandlerFilePath}

# Configure module
$modules = Get-IISConfigSection "system.webServer/modules" | Get-IISConfigCollection
New-IISConfigCollectionElement $modules -ConfigAttribute @{"name"="AspNetCoreModule"}
```

> [!NOTE]
> Elimine los archivos *aspnetcore.dll* y *aspnetcore_schema.xml* del recurso compartido después del paso anterior.

## <a name="installing-net-core-framework"></a>Instalar .NET Framework

Si su aplicación se publica como una [implementación dependiente de Framework (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), debe instalarse .NET Core en el servidor. Use el [script de PowerShell dotnet-install.ps1](https://dot.net/v1/dotnet-install.ps1) en una sesión remota de PowerShell para instalar .NET Core en su servidor de Nano Server. Pase la versión de la CLI con el modificador `-Version`:

```console
dotnet-install.ps1 -Version 2.0.0
```

## <a name="publishing-the-application"></a>Publicar la aplicación

Copie el resultado publicado de la aplicación existente en la raíz del recurso compartido de archivos.

Puede que necesite realizar cambios en *web.config* para que apunte a la ubicación donde extrajo *dotnet.exe*. Como alternativa, puede agregar *dotnet.exe* a la ruta de acceso.

Ejemplo del aspecto que tendría un archivo *web.config* si *dotnet.exe* **no** estuviera en la ruta de acceso:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="C:\dotnet\dotnet.exe" arguments=".\AspNetCoreSampleForNano.dll" stdoutLogEnabled="false" stdoutLogFile=".\logs\aspnetcore-stdout" forwardWindowsAuthToken="true" />
  </system.webServer>
</configuration>
```

Ejecute los siguientes comandos en la sesión remota para crear un nuevo sitio en IIS para la aplicación publicada en un puerto diferente del sitio web predeterminado. También debe abrir ese puerto para tener acceso a la web. Este script usa `DefaultAppPool` para simplificar el trabajo. Para más información sobre la ejecución en un grupo de aplicaciones, vea [Grupos de aplicaciones](xref:publishing/iis#application-pools).

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a>Problema conocido al ejecutar la interfaz de la línea de comandos de .NET Core en Nano Server y solución
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a>Ejecutar la aplicación

Se puede acceder a la aplicación web publicada en un explorador en `http://192.168.1.10:8000`. Si ha configurado el registro como se describe en [Creación y redirección de registros](xref:hosting/aspnet-core-module#log-creation-and-redirection), puede ver los registros en *C:\PublishedApps\AspNetCoreSampleForNano\logs*.
