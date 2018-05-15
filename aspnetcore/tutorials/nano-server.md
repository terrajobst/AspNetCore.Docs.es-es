---
title: ASP.NET Core en Nano Server
author: shirhatti
description: Aprenda a implementar una aplicación existente de ASP.NET Core en una instancia de Nano Server que ejecuta IIS.
manager: wpickett
ms.author: riande
ms.date: 11/04/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/nano-server
ms.openlocfilehash: 3cfe85e4591b99e3d5413a5532c5101d4a4810e2
ms.sourcegitcommit: c4a31aaf902f2e84aaf4a9d882ca980fdf6488c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2018
---
# <a name="aspnet-core-on-nano-server"></a><span data-ttu-id="c5476-103">ASP.NET Core en Nano Server</span><span class="sxs-lookup"><span data-stu-id="c5476-103">ASP.NET Core on Nano Server</span></span>

<span data-ttu-id="c5476-104">Por [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="c5476-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="c5476-105">En este tutorial tomaremos una aplicación existente de ASP.NET Core y la implementaremos en una instancia de Nano Server que ejecuta IIS.</span><span class="sxs-lookup"><span data-stu-id="c5476-105">In this tutorial, you'll take an existing ASP.NET Core app and deploy it to a Nano Server instance running IIS.</span></span>

## <a name="introduction"></a><span data-ttu-id="c5476-106">Introducción</span><span class="sxs-lookup"><span data-stu-id="c5476-106">Introduction</span></span>

<span data-ttu-id="c5476-107">Nano Server es una opción de instalación de Windows Server 2016 que ofrece una ocupación mínima, mayor seguridad y un mejor servicio que Server Core o el servidor completo.</span><span class="sxs-lookup"><span data-stu-id="c5476-107">Nano Server is an installation option in Windows Server 2016, offering a tiny footprint, better security, and better servicing than Server Core or full Server.</span></span> <span data-ttu-id="c5476-108">Eche un vistazo a la [documentación oficial de Nano Server](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) para obtener más detalles y vínculos de descarga de las versiones de evaluación de 180 días.</span><span class="sxs-lookup"><span data-stu-id="c5476-108">Please consult the official [Nano Server documentation](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) for more details and download links for 180 Days evaluation versions.</span></span> 

<span data-ttu-id="c5476-109">Existen tres formas sencillas de probar Nano Server.</span><span class="sxs-lookup"><span data-stu-id="c5476-109">There are three easy ways for you to try out Nano Server.</span></span> <span data-ttu-id="c5476-110">Cuando inicie sesión con su cuenta de MS:</span><span class="sxs-lookup"><span data-stu-id="c5476-110">When you sign in with your MS account:</span></span>

1. <span data-ttu-id="c5476-111">Puede descargar el archivo ISO de Windows Server 2016 y crear una imagen de Nano Server.</span><span class="sxs-lookup"><span data-stu-id="c5476-111">You can download the Windows Server 2016 ISO file and build a Nano Server image.</span></span>

2. <span data-ttu-id="c5476-112">Descargue el disco duro virtual de Nano Server.</span><span class="sxs-lookup"><span data-stu-id="c5476-112">Download the Nano Server VHD.</span></span>

3. <span data-ttu-id="c5476-113">Cree una máquina virtual en Azure con la imagen de Nano Server de la Galería de Azure.</span><span class="sxs-lookup"><span data-stu-id="c5476-113">Create a VM in Azure using the Nano Server image in the Azure Gallery.</span></span> <span data-ttu-id="c5476-114">Hay disponible una prueba gratuita de Azure.</span><span class="sxs-lookup"><span data-stu-id="c5476-114">A free trial of Azure is avaiable.</span></span>

<span data-ttu-id="c5476-115">En este tutorial, vamos a usar la segunda opción, el disco duro virtual de Nano Server compilado previamente desde Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="c5476-115">In this tutorial, we will be using the 2nd option, the pre-built Nano Server VHD from Windows Server 2016.</span></span>

<span data-ttu-id="c5476-116">Antes de continuar con este tutorial, necesitará la [salida publicada](xref:host-and-deploy/directory-structure) de una aplicación existente de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c5476-116">Before proceeding with this tutorial, you will need the [published output](xref:host-and-deploy/directory-structure) of an existing ASP.NET Core application.</span></span> <span data-ttu-id="c5476-117">Asegúrese de que la aplicación está compilada para ejecutarse en un proceso de **64 bits**.</span><span class="sxs-lookup"><span data-stu-id="c5476-117">Ensure your application is built to run in a **64-bit** process.</span></span>

## <a name="setting-up-the-nano-server-instance"></a><span data-ttu-id="c5476-118">Configuración de la instancia de Nano Server</span><span class="sxs-lookup"><span data-stu-id="c5476-118">Setting up the Nano Server instance</span></span>

<span data-ttu-id="c5476-119">[Cree una nueva máquina virtual mediante Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) en su máquina de desarrollo usando el disco duro virtual descargado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="c5476-119">[Create a new Virtual Machine using Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) on your development machine using the previously downloaded VHD.</span></span> <span data-ttu-id="c5476-120">La máquina le pedirá que establezca una contraseña de administrador antes de iniciar sesión.</span><span class="sxs-lookup"><span data-stu-id="c5476-120">The machine will require you to set an administrator password before logging on.</span></span> <span data-ttu-id="c5476-121">En la consola de VM, presione F11 para establecer la contraseña antes del primer inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="c5476-121">At the VM console, press F11 to set the password before the first log in.</span></span> <span data-ttu-id="c5476-122">También necesita comprobar la dirección IP de la nueva máquina virtual. Para ello compruebe la dirección IP fija del servidor DHCP proporcionada al aprovisionar la máquina virtual o compruebe la configuración de red de la consola de recuperación de Nano Server.</span><span class="sxs-lookup"><span data-stu-id="c5476-122">You also need to check your new VM's IP address either my checking your DHCP server's fixed IP supplied while provisioning your VM or in Nano Server recovery console's networking settings.</span></span>

> [!NOTE]
> <span data-ttu-id="c5476-123">Supongamos que la nueva máquina virtual se ejecuta con la dirección IP V4 local 192.168.1.10.</span><span class="sxs-lookup"><span data-stu-id="c5476-123">Let's assume your new VM runs with the local V4 IP address 192.168.1.10.</span></span>

<span data-ttu-id="c5476-124">Ahora puede administrarla mediante la comunicación remota de PowerShell, que es la única manera de administrar por completo Nano Server.</span><span class="sxs-lookup"><span data-stu-id="c5476-124">Now you're able to manage it using PowerShell remoting, which is the only way to fully administer your Nano Server.</span></span>

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a><span data-ttu-id="c5476-125">Conectarse a la instancia de Nano Server usando la comunicación remota de PowerShell</span><span class="sxs-lookup"><span data-stu-id="c5476-125">Connecting to your Nano Server instance using PowerShell Remoting</span></span>

<span data-ttu-id="c5476-126">Abra una ventana de PowerShell con privilegios elevados para agregar la instancia remota de Nano Server a la lista `TrustedHosts`.</span><span class="sxs-lookup"><span data-stu-id="c5476-126">Open an elevated PowerShell window to add your remote Nano Server instance to your `TrustedHosts` list.</span></span>

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> <span data-ttu-id="c5476-127">Reemplace la variable `$nanoServerIpAddress` con la dirección IP correcta.</span><span class="sxs-lookup"><span data-stu-id="c5476-127">Replace the variable `$nanoServerIpAddress` with the correct IP address.</span></span>

<span data-ttu-id="c5476-128">Una vez que haya agregado la instancia de Nano Server a `TrustedHosts`, puede conectarse a ella mediante la comunicación remota de PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c5476-128">Once you have added your Nano Server instance to your `TrustedHosts`, you can connect to it using PowerShell remoting.</span></span>

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

<span data-ttu-id="c5476-129">Cuando se realiza una conexión correcta, se muestra un símbolo del sistema parecido a este:`[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span><span class="sxs-lookup"><span data-stu-id="c5476-129">A successful connection results in a prompt with a format looking like: `[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span></span>

## <a name="creating-a-file-share"></a><span data-ttu-id="c5476-130">Crear un recurso compartido de archivos</span><span class="sxs-lookup"><span data-stu-id="c5476-130">Creating a file share</span></span>

<span data-ttu-id="c5476-131">Cree un recurso compartido de archivos en Nano Server para que la aplicación publicada pueda copiarse en él.</span><span class="sxs-lookup"><span data-stu-id="c5476-131">Create a file share on the Nano server so that the published application can be copied to it.</span></span> <span data-ttu-id="c5476-132">Ejecute los siguientes comandos en la sesión remota:</span><span class="sxs-lookup"><span data-stu-id="c5476-132">Run the following commands in the remote session:</span></span>

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

<span data-ttu-id="c5476-133">Después de ejecutar los comandos anteriores, debería tener acceso a este recurso compartido si visita `\\192.168.1.10\AspNetCoreSampleForNano` en el Explorador de Windows de la máquina host.</span><span class="sxs-lookup"><span data-stu-id="c5476-133">After running the above commands, you should be able to access this share by visiting `\\192.168.1.10\AspNetCoreSampleForNano` in the host machine's Windows Explorer.</span></span>

## <a name="open-port-in-the-firewall"></a><span data-ttu-id="c5476-134">Abrir el puerto en el firewall</span><span class="sxs-lookup"><span data-stu-id="c5476-134">Open port in the firewall</span></span>

<span data-ttu-id="c5476-135">Ejecute los siguientes comandos en la sesión remota para abrir un puerto en el firewall a fin de permitir que IIS escuche el tráfico TCP en el puerto TCP/8000.</span><span class="sxs-lookup"><span data-stu-id="c5476-135">Run the following commands in the remote session to open up a port in the firewall to let IIS listen for TCP traffic on port TCP/8000.</span></span>

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a><span data-ttu-id="c5476-136">Instalar IIS</span><span class="sxs-lookup"><span data-stu-id="c5476-136">Installing IIS</span></span>

<span data-ttu-id="c5476-137">Agregue el proveedor `NanoServerPackage` desde la Galería de PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c5476-137">Add the `NanoServerPackage` provider from the PowerShell Gallery.</span></span> <span data-ttu-id="c5476-138">Cuando se haya instalado e importado el proveedor, podrá instalar los paquetes de Windows.</span><span class="sxs-lookup"><span data-stu-id="c5476-138">Once the provider is installed and imported, you can install Windows packages.</span></span>

<span data-ttu-id="c5476-139">En la sesión de PowerShell que creó anteriormente, ejecute los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="c5476-139">Run the following commands in the PowerShell session that was created earlier:</span></span>

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

<span data-ttu-id="c5476-140">Para comprobar rápidamente si IIS está configurado correctamente, puede visitar la dirección URL `http://192.168.1.10/`, en la que debería ver una página de bienvenida.</span><span class="sxs-lookup"><span data-stu-id="c5476-140">To quickly verify if IIS is setup correctly, you can visit the URL `http://192.168.1.10/` and should see a welcome page.</span></span> <span data-ttu-id="c5476-141">Cuando se instala IIS, se crea de manera predeterminada un sitio web denominado `Default Web Site` que escucha en el puerto 80.</span><span class="sxs-lookup"><span data-stu-id="c5476-141">When IIS is installed, a website called `Default Web Site` listening on port 80 is created by default.</span></span>

## <a name="install-the-aspnet-core-module"></a><span data-ttu-id="c5476-142">Instalar el módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c5476-142">Install the ASP.NET Core Module</span></span>

<span data-ttu-id="c5476-143">El módulo de ASP.NET Core es un módulo de IIS 7.5+ que se encarga de la administración de procesos de las escuchas HTTP de ASP.NET Core y de enviar mediante proxy las solicitudes a los procesos que administra.</span><span class="sxs-lookup"><span data-stu-id="c5476-143">The ASP.NET Core Module is an IIS 7.5+ module which is responsible for process management of ASP.NET Core HTTP listeners and to proxy requests to processes that it manages.</span></span> <span data-ttu-id="c5476-144">En este momento, el proceso para instalar el módulo de ASP.NET Core para IIS es manual.</span><span class="sxs-lookup"><span data-stu-id="c5476-144">At the moment, the process to install the ASP.NET Core Module for IIS is manual.</span></span> <span data-ttu-id="c5476-145">Instale el [conjunto de productos de hospedaje de .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) en una máquina normal (que no sea Nano).</span><span class="sxs-lookup"><span data-stu-id="c5476-145">Install the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) on a regular (not Nano) machine.</span></span> <span data-ttu-id="c5476-146">Después de instalar el conjunto de productos en una máquina normal, copie los siguientes archivos en el recurso compartido de archivos que hemos creado con anterioridad.</span><span class="sxs-lookup"><span data-stu-id="c5476-146">After installing the bundle on a regular machine, copy the following files to the file share that we created earlier.</span></span>

<span data-ttu-id="c5476-147">En un servidor normal (que no sea Nano) con IIS, ejecute los siguientes comandos de copia:</span><span class="sxs-lookup"><span data-stu-id="c5476-147">On a regular (not Nano) server with IIS, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

<span data-ttu-id="c5476-148">Reemplace `C:\windows\system32\inetsrv` con `C:\Program Files\IIS Express` en una máquina con Windows 10.</span><span class="sxs-lookup"><span data-stu-id="c5476-148">Replace `C:\windows\system32\inetsrv` with `C:\Program Files\IIS Express` on a Windows 10 machine.</span></span>

<span data-ttu-id="c5476-149">En el lado Nano, debe copiar los siguientes archivos desde el recurso compartido de archivos que hemos creado anteriormente a las ubicaciones válidas.</span><span class="sxs-lookup"><span data-stu-id="c5476-149">On the Nano side, you will need to copy the following files from the file share that we created earlier to the valid locations.</span></span> <span data-ttu-id="c5476-150">Ejecute los siguientes comandos de copia:</span><span class="sxs-lookup"><span data-stu-id="c5476-150">So, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

<span data-ttu-id="c5476-151">Ejecute el siguiente script en la sesión remota:</span><span class="sxs-lookup"><span data-stu-id="c5476-151">Run the following script in the remote session:</span></span>

[!code-powershell[](nano-server/enable-aspnetcoremodule.ps1)]

> [!NOTE]
> <span data-ttu-id="c5476-152">Elimine los archivos *aspnetcore.dll* y *aspnetcore_schema.xml* del recurso compartido después del paso anterior.</span><span class="sxs-lookup"><span data-stu-id="c5476-152">Delete the files *aspnetcore.dll* and *aspnetcore_schema.xml* from the share after the above step.</span></span>

## <a name="installing-net-core-framework"></a><span data-ttu-id="c5476-153">Instalar .NET Framework</span><span class="sxs-lookup"><span data-stu-id="c5476-153">Installing .NET Core Framework</span></span>

<span data-ttu-id="c5476-154">Si su aplicación se publica como una [implementación dependiente de Framework (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), debe instalarse .NET Core en el servidor.</span><span class="sxs-lookup"><span data-stu-id="c5476-154">If your app is published as a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core must be installed on the server.</span></span> <span data-ttu-id="c5476-155">Use el [script de PowerShell dotnet-install.ps1](https://dot.net/v1/dotnet-install.ps1) en una sesión remota de PowerShell para instalar .NET Core en su servidor de Nano Server.</span><span class="sxs-lookup"><span data-stu-id="c5476-155">Use the [dotnet-install.ps1 PowerShell script](https://dot.net/v1/dotnet-install.ps1) in a remote PowerShell session to install .NET Core on your Nano Server.</span></span> <span data-ttu-id="c5476-156">Pase la versión de la CLI con el modificador `-Version`:</span><span class="sxs-lookup"><span data-stu-id="c5476-156">Pass the CLI version with the `-Version` switch:</span></span>

```console
dotnet-install.ps1 -Version 2.0.0
```

## <a name="publishing-the-application"></a><span data-ttu-id="c5476-157">Publicar la aplicación</span><span class="sxs-lookup"><span data-stu-id="c5476-157">Publishing the application</span></span>

<span data-ttu-id="c5476-158">Copie el resultado publicado de la aplicación existente en la raíz del recurso compartido de archivos.</span><span class="sxs-lookup"><span data-stu-id="c5476-158">Copy the published output of your existing application to the file share's root.</span></span>

<span data-ttu-id="c5476-159">Puede que necesite realizar cambios en *web.config* para que apunte a la ubicación donde extrajo *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="c5476-159">You may need to make changes to your *web.config* to point to where you extracted *dotnet.exe*.</span></span> <span data-ttu-id="c5476-160">Como alternativa, puede agregar *dotnet.exe* a la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="c5476-160">Alternatively, you can add *dotnet.exe* to your PATH.</span></span>

<span data-ttu-id="c5476-161">Ejemplo del aspecto que tendría un archivo *web.config* si *dotnet.exe* **no** estuviera en la ruta de acceso:</span><span class="sxs-lookup"><span data-stu-id="c5476-161">Example of how a *web.config* might look if *dotnet.exe* is **not** on the PATH:</span></span>

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

<span data-ttu-id="c5476-162">Ejecute los siguientes comandos en la sesión remota para crear un nuevo sitio en IIS para la aplicación publicada en un puerto diferente del sitio web predeterminado.</span><span class="sxs-lookup"><span data-stu-id="c5476-162">Run the following commands in the remote session to create a new site in IIS for the published app on a different port than the default website.</span></span> <span data-ttu-id="c5476-163">También debe abrir ese puerto para tener acceso a la web.</span><span class="sxs-lookup"><span data-stu-id="c5476-163">You also need to open that port to access the web.</span></span> <span data-ttu-id="c5476-164">Este script usa `DefaultAppPool` para simplificar el trabajo.</span><span class="sxs-lookup"><span data-stu-id="c5476-164">This script uses the `DefaultAppPool` for simplicity.</span></span> <span data-ttu-id="c5476-165">Para más información sobre la ejecución en un grupo de aplicaciones, vea [Grupos de aplicaciones](xref:host-and-deploy/iis/index#application-pools).</span><span class="sxs-lookup"><span data-stu-id="c5476-165">For more considerations on running under an application pool, see [Application Pools](xref:host-and-deploy/iis/index#application-pools).</span></span>

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a><span data-ttu-id="c5476-166">Problema conocido al ejecutar la interfaz de la línea de comandos de .NET Core en Nano Server y solución</span><span class="sxs-lookup"><span data-stu-id="c5476-166">Known issue running .NET Core CLI on Nano Server and workaround</span></span>
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a><span data-ttu-id="c5476-167">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="c5476-167">Running the application</span></span>

<span data-ttu-id="c5476-168">Se puede acceder a la aplicación web publicada en un explorador en `http://192.168.1.10:8000`.</span><span class="sxs-lookup"><span data-stu-id="c5476-168">The published web app is accessible in a browser at `http://192.168.1.10:8000`.</span></span> <span data-ttu-id="c5476-169">Si ha configurado el registro como se describe en [Creación y redirección de registros](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection), puede ver los registros en *C:\PublishedApps\AspNetCoreSampleForNano\logs*.</span><span class="sxs-lookup"><span data-stu-id="c5476-169">If you've set up logging as described in [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection), you can view your logs at *C:\PublishedApps\AspNetCoreSampleForNano\logs*.</span></span>
