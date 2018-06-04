---
title: Mejorar una aplicación desde un ensamblado externo en ASP.NET Core con IHostingStartup
author: guardrex
description: Sepa cómo mejorar una aplicación ASP.NET Core desde un ensamblado externo con una implementación de IHostingStartup.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 618cb4349dcff696db37012af3aee844b82974f2
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729056"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a>Mejorar una aplicación desde un ensamblado externo en ASP.NET Core con IHostingStartup

Por [Luke Latham](https://github.com/guardrex)

Una implementación de [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) permite agregar mejoras a una aplicación al iniciarla a partir de un ensamblado externo fuera de la clase `Startup` de esta. Por ejemplo, una biblioteca de herramientas externas puede usar una implementación de `IHostingStartup` para suministrar más servicios o proveedores de configuración a una aplicación. `IHostingStartup`  *está disponible en ASP.NET 2.0 Core y versiones posteriores.*

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="discover-loaded-hosting-startup-assemblies"></a>Detectar los ensamblados de inicio de hospedaje cargados

Para detectar los ensamblados de inicio de hospedaje que la aplicación o las bibliotecas han cargado, habilite el registro y consulte los registros de la aplicación. En ellos se registran los errores que se producen al cargar ensamblados. Los ensamblados de inicio de hospedaje se registran en el nivel de depuración y se registran todos los errores.

La aplicación de ejemplo lee el valor de [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) en una matriz `string` y muestra el resultado en la página de índice de la aplicación:

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Deshabilitar la carga automática de ensamblados de inicio de hospedaje

Existen dos formas de deshabilitar la carga automática de ensamblados de inicio de hospedaje:

* Establecer la opción de configuración de host para [impedir el inicio de hospedaje](xref:fundamentals/host/web-host#prevent-hosting-startup).
* Establecer la variable de entorno `ASPNETCORE_PREVENTHOSTINGSTARTUP`.

Cuando la configuración del host o la variable de entorno está establecida en `true` o `1`, los ensamblados de inicio de hospedaje no se cargan automáticamente. Si ambas se han definido, será la configuración del host la que controle el comportamiento.

Deshabilitar los ensamblados de inicio de hospedaje a través de la variable de entorno o de la configuración de host hace que estos se deshabiliten globalmente y, asimismo, puede deshabilitar también otras características de una aplicación. Actualmente no se puede deshabilitar de manera selectiva un ensamblado de inicio de hospedaje que esté agregado a una biblioteca, a menos que esa biblioteca disponga de su propia opción de configuración. En una próxima versión existirá la posibilidad de deshabilitar ensamblados de inicio de hospedaje de forma selectiva (vea el [problema de GitHub aspnet/Hosting n.º 1243](https://github.com/aspnet/Hosting/pull/1243)).

## <a name="implement-ihostingstartup"></a>Implementar IHostingStartup

### <a name="create-the-assembly"></a>Crear el ensamblado

Una mejora de `IHostingStartup` se implementa como un ensamblado basado en una aplicación de consola sin un punto de entrada. El ensamblado hace referencia al paquete [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/):

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupEnhancement.csproj)]

Un atributo [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) identifica una clase como una implementación de `IHostingStartup` para la carga y ejecución al compilar [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost). En el siguiente ejemplo, el espacio de nombres es `StartupEnhancement` y la clase, `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet1)]

Una clase implementa `IHostingStartup`. El método [Configurar](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) de la clase usa un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) para agregar mejoras a una aplicación:

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

Al crear un proyecto de `IHostingStartup`, el archivo de dependencias (*\*.deps.json*) establece la ubicación de `runtime` del ensamblado en la carpeta *bin*:

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

Solo se muestra parte del archivo. El nombre del ensamblado en el ejemplo es `StartupEnhancement`.

### <a name="update-the-dependencies-file"></a>Actualizar el archivo de dependencias

La ubicación del tiempo de ejecución se especifica en el archivo *\*.deps.json*. Para activar la mejora, el elemento `runtime` debe especificar la ubicación del ensamblado de tiempo de ejecución de la mejora. Anteponga `lib/<TARGET_FRAMEWORK_MONIKER>/` a la ubicación de `runtime`:

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

En la aplicación de ejemplo, el archivo *\*.deps.json* se modifica por medio de un script de [PowerShell](/powershell/scripting/powershell-scripting) que se activa automáticamente a través de un destino de compilación en el archivo de proyecto.

### <a name="enhancement-activation"></a>Activación de la mejora

**Colocar el archivo de ensamblado**

El archivo de ensamblado de la implementación `IHostingStartup` debe estar implementado en *bin* en la aplicación o colocarse en el [almacenamiento en tiempo de ejecución](/dotnet/core/deploying/runtime-store):

Para usarlo individualmente con cada usuario, coloque el ensamblado en el almacenamiento en tiempo de ejecución del perfil del usuario en cuestión, en:

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

Para usarlo de manera global, colóquelo en el almacenamiento en tiempo de ejecución de la instalación de .NET Core:

```
<DRIVE>\Program Files\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

Si el ensamblado se implementa en el almacenamiento en tiempo de ejecución, puede que también se implemente el archivo de símbolos, si bien esto no es imprescindible para que la mejora funcione.

**Colocar el archivo de dependencias**

El archivo *\*.deps.json* de la implementación debe estar en una ubicación accesible.

Para usarlo individualmente con cada usuario, coloque el archivo en la carpeta `additonalDeps` de la configuración `.dotnet` del perfil de usuario: 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.1.0\
```

Para usarlo de manera global, colóquelo en la carpeta `additonalDeps` de la instalación de .NET Core:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.1.0\
```

La versión `2.1.0` es la versión del tiempo de ejecución compartido que la aplicación de destino usa. El tiempo de ejecución compartido se muestra en el archivo *\*.runtimeconfig.json*. En la aplicación de ejemplo, el tiempo de ejecución compartido se especifica en el archivo *HostingStartupSample.runtimeconfig.json*.

**Establecer las variables de entorno**

Establezca las siguientes variables de entorno en el contexto de la aplicación que usa la mejora.

ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES

Solo se examinan los ensamblados de inicio de hospedaje en busca de `HostingStartupAttribute`. El nombre del ensamblado de la implementación se proporciona en esta variable de entorno. En la aplicación de ejemplo, este valor se establece en `StartupDiagnostics`.

Dicho valor también se puede establecer usando la configuración de host de [ensamblados de inicio de hospedaje](xref:fundamentals/host/web-host#hosting-startup-assemblies).

DOTNET\_ADDITIONAL\_DEPS

Esta es la ubicación del archivo *\*.deps.json* de la implementación.

Si el archivo está en la carpeta *.dotnet* del perfil de usuario para usarlo individualmente con cada usuario:

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

Si el archivo está en la instalación de .NET Core para usarlo de manera global, indique la ruta de acceso completa al archivo:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.1.0\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

En la aplicación de ejemplo, este valor se establece en:

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

Para obtener ejemplos de cómo establecer variables de entorno en distintos sistemas operativos, vea [Uso de varios entornos](xref:fundamentals/environments).

## <a name="sample-app"></a>Aplicación de ejemplo

La [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample)) usa `IHostingStartup` para crear una herramienta de diagnóstico. Esta herramienta agrega dos middlewares a la aplicación (mientras se inicia) que proporcionan información de diagnóstico:

* Servicios registrados
* Dirección: esquema, host, ruta de acceso base, ruta de acceso, cadena de consulta
* Conexión: dirección IP remota, puerto remoto, dirección IP local, puerto local, certificado de cliente
* Encabezados de solicitud
* Variables de entorno

Para ejecutar el ejemplo:

1. El proyecto de diagnóstico de inicio usa [PowerShell](/powershell/scripting/powershell-scripting) para modificar el archivo *StartupDiagnostics.deps.json*. PowerShell se instala de forma predeterminada en cualquier sistema operativo Windows a partir de Windows 7 SP1 y Windows Server 2008 R2 SP1. Para obtener PowerShell en otras plataformas, vea [Instalación de Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).
2. Compile el proyecto de diagnóstico de inicio. Un destino de compilación en el archivo de proyecto hace lo siguiente:
   * Mueve el ensamblado y los archivos de símbolos al almacenamiento en tiempo de ejecución del perfil de usuario.
   * Activa el script de PowerShell para modificar el archivo *StartupDiagnostics.deps.json*.
   * Mueve el archivo *StartupDiagnostics.deps.json* a la carpeta `additionalDeps` del perfil de usuario.
3. Establezca las variables de entorno:
    * `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`
    * `DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`
4. Ejecute la aplicación de ejemplo.
5. Solicite al punto de conexión `/services` ver los servicios registrados de la aplicación. Solicite al punto de conexión `/diag` ver la información de diagnóstico.
