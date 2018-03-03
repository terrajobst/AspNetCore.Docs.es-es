---
title: "Agregar características de la aplicación con una configuración específica de la plataforma de ASP.NET Core"
author: guardrex
description: "Descubra cómo agregar características a una aplicación de ASP.NET Core desde un ensamblado externo con una implementación de IHostingStartup."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/platform-specific-configuration
ms.openlocfilehash: c36b8acd6f7fcb4e4d11e43013ccaf5ca6d1b0ab
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2018
---
# <a name="add-app-features-using-a-platform-specific-configuration-in-aspnet-core"></a>Agregar características de la aplicación con una configuración específica de la plataforma de ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Un [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementación permite agregar características a una aplicación en el inicio de un ensamblado externo fuera de la aplicación `Startup` clase. Por ejemplo, puede utilizar una biblioteca de herramientas externas un `IHostingStartup` implementación para proporcionar servicios a una aplicación o proveedores de configuración adicionales. `IHostingStartup` *está disponible en el núcleo de ASP.NET 2.0 y versiones posteriores.*

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/platform-specific-configuration/sample/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="discover-loaded-hosting-startup-assemblies"></a>Detectar los ensamblados cargados de hospedaje inicio

Para detectar hospedaje ensamblados inicio cargados por la aplicación o bibliotecas, habilitar el registro y compruebe los registros de aplicación. Se registran los errores que se producen al cargar los ensamblados. Cargar ensamblados de inicio de hospedaje se registran en el nivel de depuración y se registran todos los errores.

Las lecturas de la aplicación de ejemplo del [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) en un `string` de matriz y se muestra el resultado en la página de índice de la aplicación:

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Deshabilitar la carga automática de hospedaje de ensamblados de inicio

Hay dos formas de deshabilitar la carga automática de hospedaje de ensamblados de inicio:

* Establecer el [evitar el inicio de hospedaje](xref:fundamentals/hosting#prevent-hosting-startup) de configuración de host.
* Establecer el `ASPNETCORE_preventHostingStartup` variable de entorno.

Cuando la configuración del host o la variable de entorno se establece en `true` o `1`, hospedaje ensamblados de inicio no se carguen automáticamente. Si ambas están establecidas, la configuración del host controla el comportamiento.

Deshabilitar hospedaje ensamblados de inicio mediante la variable de entorno o configuración de host deshabilita globalmente y puede deshabilitar varias características de una aplicación. No es posible actualmente deshabilitar de forma selectiva un ensamblado de inicio hospedado agregado una biblioteca a menos que la biblioteca ofrece su propia opción de configuración. Una versión futura le ofrece la capacidad de deshabilitar de forma selectiva hospedaje ensamblados de inicio (consulte [GitHub emitir aspnet/hospedaje #1243](https://github.com/aspnet/Hosting/pull/1243)).

## <a name="implement-ihostingstartup-features"></a>Implementar características IHostingStartup

### <a name="create-the-assembly"></a>Crear el ensamblado

Un `IHostingStartup` característica se implementa como un ensamblado basado en una aplicación de consola sin un punto de entrada. Las referencias de ensamblado del [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) paquete:

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupFeature.csproj)]

A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) atributo identifica una clase como una implementación de `IHostingStartup` de carga y ejecución al compilar el [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost). En el ejemplo siguiente, el espacio de nombres es `StartupFeature`, y es la clase `StartupFeatureHostingStartup`:

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupFeature.cs?name=snippet1)]

Una clase que implementa `IHostingStartup`. La clase [configurar](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) método usa un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) para agregar características a una aplicación:

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupFeature.cs?name=snippet2&highlight=3,5)]

Cuando se crea un `IHostingStartup` del proyecto, el archivo de dependencias (*\*. deps.json*) establece el `runtime` ubicación del ensamblado en el *bin* carpeta:

[!code-json[](platform-specific-configuration/snapshot_sample/StartupFeature1.deps.json?range=2-13&highlight=8)]

Se muestra solo una parte del archivo. Es el nombre del ensamblado en el ejemplo `StartupFeature`.

### <a name="update-the-dependencies-file"></a>Actualizar el archivo de dependencias

La ubicación en tiempo de ejecución se especifica en el  *\*. deps.json* archivo. Activa la característica, la `runtime` elemento debe especificar la ubicación del ensamblado en tiempo de ejecución de la característica. Prefijo del `runtime` ubicación con `lib/netcoreapp2.0/`:

[!code-json[](platform-specific-configuration/snapshot_sample/StartupFeature2.deps.json?range=2-13&highlight=8)]

En la aplicación de ejemplo, la modificación de la  *\*. deps.json* archivo se realiza mediante un [PowerShell](/powershell/scripting/powershell-scripting) secuencia de comandos. El script de PowerShell se activa automáticamente mediante un destino de compilación en el archivo de proyecto.

### <a name="feature-activation"></a>Activación de características

**Coloque el archivo de ensamblado**

El `IHostingStartup` debe ser el archivo de ensamblado de la implementación *bin*-implementados en la aplicación o se colocan en el [en tiempo de ejecución almacén](/dotnet/core/deploying/runtime-store):

Para su uso por usuario, coloque el ensamblado en el almacén de tiempo de ejecución del perfil de usuario en:

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

Para su uso global, coloque el ensamblado en el almacén de tiempo de ejecución de la instalación de .NET Core:

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

Al implementar el ensamblado en el almacén de tiempo de ejecución, el archivo de símbolos también se puede implementar, pero no es necesario para que funcione la característica.

**Coloque el archivo de dependencias**

La implementación  *\*. deps.json* archivo debe estar en una ubicación accesible.

Para su uso por usuario, coloque el archivo en el `additonalDeps` carpeta del perfil de usuario `.dotnet` configuración: 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

Para su uso global, coloque el archivo en el `additonalDeps` carpeta de la instalación de .NET Core:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

Tenga en cuenta la versión `2.0.0`, refleja la versión del runtime compartida que utiliza la aplicación de destino. El tiempo de ejecución compartido se muestra en el  *\*. runtimeconfig.json* archivo. En la aplicación de ejemplo, el tiempo de ejecución compartido se especifica en el *HostingStartupSample.runtimeconfig.json* archivo.

**Establecer variables de entorno**

Establecer las siguientes variables de entorno en el contexto de la aplicación que utiliza la característica.

ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES

Ensamblados de inicio de hospedaje solo se examinan la `HostingStartupAttribute`. El nombre del ensamblado de la implementación se proporciona en esta variable de entorno. La aplicación de ejemplo establece este valor en `StartupDiagnostics`.

El valor también puede establecerse utilizando la [hospedaje inicio ensamblados](xref:fundamentals/hosting#hosting-startup-assemblies) de configuración de host.

DOTNET\_ADICIONALES\_DEPÓS

La ubicación de la implementación  *\*. deps.json* archivo.

Si el archivo se encuentra en el perfil de usuario *.dotnet* carpeta para su uso por usuario:

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

Si el archivo se encuentra en la instalación de .NET Core para su uso global, proporcione la ruta de acceso completa al archivo:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<FEATURE_ASSEMBLY_NAME>.deps.json
```

La aplicación de ejemplo, establece este valor en:

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

Para obtener ejemplos de cómo establecer variables de entorno para varios sistemas operativos, consulte [trabajar con varios entornos](xref:fundamentals/environments).

## <a name="sample-app"></a>Aplicación de ejemplo

El [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/platform-specific-configuration/sample/) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample)) usa `IHostingStartup` para crear una herramienta de diagnóstico. La herramienta agrega dos middlewares a la aplicación en el inicio que proporcionan información de diagnóstico:

* Servicios registrados
* Dirección: esquema, host, ruta de acceso base, ruta de acceso, cadena de consulta
* Conexión: IP remotas, puerto remoto, IP local, puerto local, el certificado de cliente
* Encabezados de solicitud
* Variables de entorno

Para ejecutar el ejemplo:

1. El proyecto de inicio diagnóstico utiliza [PowerShell](/powershell/scripting/powershell-scripting) para modificar su *StartupDiagnostics.deps.json* archivo. PowerShell se instala de forma predeterminada en el sistema operativo de Windows a partir de Windows 7 SP1 y Windows Server 2008 R2 SP1. Para obtener PowerShell en otras plataformas, vea [instalar Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).
2. Compile el proyecto de inicio diagnóstico. Un destino de compilación en el archivo de proyecto:
   * Mueve el ensamblado y los archivos al almacén de tiempo de ejecución del perfil de usuario de símbolos.
   * Desencadena la secuencia de comandos de PowerShell para modificar el *StartupDiagnostics.deps.json* archivo.
   * Mueve el *StartupDiagnostics.deps.json* archivo para el perfil de usuario `additionalDeps` carpeta.
3. Establecer las variables de entorno:
    * `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`
    * `DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`
4. Ejecute la aplicación de ejemplo.
5. Solicitar la `/services` extremo para ver la aplicación registra los servicios. Solicitar la `/diag` punto de conexión para ver la información de diagnóstico.
