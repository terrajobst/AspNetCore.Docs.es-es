---
title: Perfiles de publicación de Visual Studio para la implementación de aplicaciones ASP.NET Core
author: rick-anderson
description: Aprenda a crear perfiles de publicación en Visual Studio y usarlos para administrar implementaciones de aplicaciones ASP.NET Core en diversos destinos.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/12/2019
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: be5d1a79b7f4437d04586ae4ce24df94547d8a3c
ms.sourcegitcommit: b4ef2b00f3e1eb287138f8b43c811cb35a100d3e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969973"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Perfiles de publicación de Visual Studio para la implementación de aplicaciones ASP.NET Core

Por [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Este documento se centra en el uso de Visual Studio 2017 o versiones posteriores para crear y usar perfiles de publicación. Los perfiles de publicación creados con Visual Studio se pueden ejecutar en MSBuild y en Visual Studio. Vea [Publicar una aplicación web de ASP.NET Core en Azure App Service con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) para obtener instrucciones sobre la publicación en Azure.

El siguiente archivo de proyecto se creó con el comando `dotnet new mvc`:

::: moniker range=">= aspnetcore-2.2"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.2</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

::: moniker-end

El atributo `<Project>` del elemento `Sdk` lleva a cabo las siguientes tareas:

* Importa el archivo de propiedades de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* al comienzo.
* Importa el archivo targets de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* al final.

La ubicación predeterminada de `MSBuildSDKsPath` (con Visual Studio 2017 Enterprise) es la carpeta *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.

El SDK de `Microsoft.NET.Sdk.Web` depende de:

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

Lo que hace que se importen las propiedades y los destinos siguientes:

* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*

Los destinos de publicación importan el conjunto adecuado de destinos en función del método de publicación usado.

Cuando se carga un proyecto en Visual Studio o MSBuild, se llevan a cabo las siguientes acciones generales:

* Compilación del proyecto
* Cálculo de los archivos para la publicación
* Publicación de los archivos en el destino

## <a name="compute-project-items"></a>Cálculo de los elementos del proyecto

Cuando se carga el proyecto, se calculan los elementos del proyecto (archivos). El atributo `item type` determina cómo se procesa el archivo. De forma predeterminada, los archivos *.cs* se incluyen en la lista de elementos `Compile`. Después se compilan los archivos de la lista de elementos `Compile`.

La lista de elementos `Content` contiene archivos que se van a publicar, además de los resultados de compilación. De forma predeterminada, los archivos que coinciden con el patrón `wwwroot/**` se incluyen en el elemento `Content`. El `wwwroot/\*\*` [patrón global](https://gruntjs.com/configuring-tasks#globbing-patterns) coincide con los archivos de la carpeta *wwwroot* y de las subcarpetas **y**. Para agregar explícitamente un archivo a la lista de publicación, agregue el archivo directamente en el archivo *.csproj*, como se muestra en [Archivos de inclusión](#include-files).

Al seleccionar el botón **Publicar** en Visual Studio o al publicar desde la línea de comandos:

* Se calculan los elementos/propiedades (los archivos que se deben compilar).
* **Solo Visual Studio**: Se han restaurado los paquetes NuGet. (la restauración debe ser explícita por parte del usuario en la CLI).
* Se compila el proyecto.
* Se calculan los elementos de publicación (los archivos que se deben publicar).
* Se publica el proyecto (los archivos calculados se copian en el destino de publicación).

Cuando hace referencia a un proyecto de ASP.NET Core `Microsoft.NET.Sdk.Web` en el archivo de proyecto, se coloca un archivo *app_offline.htm* en la raíz del directorio de la aplicación web. Cuando el archivo está presente, el módulo de ASP.NET Core cierra correctamente la aplicación y proporciona el archivo *app_offline.htm* durante la implementación. Para más información, vea [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm) (Referencia de configuración del módulo de ASP.NET Core).

## <a name="basic-command-line-publishing"></a>Publicación básica de línea de comandos

La publicación de línea de comandos funciona en todas las plataformas admitidas de .NET Core y no se requiere Visual Studio. En los ejemplos siguientes, se ejecuta el comando [dotnet publish](/dotnet/core/tools/dotnet-publish) desde el directorio del proyecto (que contiene el archivo *.csproj*). Si no se encuentra en la carpeta del proyecto, pase de forma explícita la ruta de acceso del archivo del proyecto. Por ejemplo:

```console
dotnet publish C:\Webs\Web1
```

Ejecute los comandos siguientes para crear y publicar una aplicación web:

```console
dotnet new mvc
dotnet publish
```

El comando [dotnet publish](/dotnet/core/tools/dotnet-publish) produce una salida similar a la siguiente:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp{X.Y}\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp{X.Y}\publish\
```

La carpeta de publicación predeterminada es `bin\$(Configuration)\netcoreapp<version>\publish`, El valor predeterminado de `$(Configuration)` es *Debug*. En el ejemplo anterior, el elemento `<TargetFramework>` es `netcoreapp{X.Y}`.

`dotnet publish -h` muestra información de ayuda de la publicación.

El siguiente comando especifica una compilación `Release` y el directorio de publicación:

```console
dotnet publish -c Release -o C:\MyWebs\test
```

El comando [dotnet publish](/dotnet/core/tools/dotnet-publish) llama a MSBuild, que invoca el destino `Publish`. Todos los parámetros pasados a `dotnet publish` se pasan a MSBuild. El parámetro `-c` se asigna a la propiedad de MSBuild `Configuration`. El parámetro `-o` se asigna a `OutputPath`.

Se pueden pasar propiedades de MSBuild mediante cualquiera de los siguientes formatos:

* `p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

El comando siguiente publica una compilación `Release` en un recurso compartido de red:

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

El recurso compartido de red se especifica con barras diagonales (*//r8/*) y funciona en todas las plataformas compatibles de .NET Core.

Confirme que la aplicación publicada para la implementación no se está ejecutando. Los archivos de la carpeta *publish* quedan bloqueados mientras se ejecuta la aplicación. No se puede llevar a cabo la implementación porque no se pueden copiar los archivos bloqueados.

## <a name="publish-profiles"></a>Publicar los perfiles

En esta sección se usa Visual Studio 2017 o versiones posteriores para crear un perfil de publicación. Una vez creado el perfil, es posible realizar la publicación desde Visual Studio o la línea de comandos.

Los perfiles de publicación pueden simplificar el proceso de publicación, y puede existir cualquier número de perfiles. Cree un perfil de publicación en Visual Studio mediante la selección de una de las rutas de acceso siguientes:

* Haga clic con el botón derecho en el Explorador de soluciones y seleccione **Publicar**.
* Seleccione **Publicar {NOMBRE DEL PROYECTO}** en el menú **Compilar**.

Aparece la pestaña **Publicar** de la página de funcionalidades de la aplicación. Si el proyecto no contiene ningún perfil de publicación, se muestra la página siguiente:

![La pestaña Publicar de la página de funcionalidades de la aplicación](visual-studio-publish-profiles/_static/az.png)

Cuando se seleccione **Carpeta**, especifique una ruta de acceso a la carpeta para almacenar los recursos publicados. La carpeta predeterminada es *bin\Release\PublishOutput*. Haga clic en el botón **Crear perfil** para terminar.

Una vez que se crea un perfil de publicación, la pestaña **Publicar** cambia. El perfil recién creado aparece en una lista desplegable. Haga clic en **Crear nuevo perfil** para crear otro nuevo perfil.

![La pestaña Publicar de la página de funcionalidades de la aplicación que muestra FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

El Asistente para publicación admite los siguientes destinos de publicación:

* Azure App Service
* Azure Virtual Machines
* IIS, FTP, etc. (para cualquier servidor web)
* Carpeta
* Perfil de importación

Para más información, consulte [¿Qué opciones de publicación son las adecuadas para mí?](/visualstudio/ide/not-in-toc/web-publish-options)

Al crear un perfil de publicación con Visual Studio, se crea un archivo MSBuild denominado *Properties/PublishProfiles/{NOMBRE DEL PERFIL}.pubxml*. Este archivo *.pubxml* es un archivo de MSBuild que contiene la configuración de publicación. Este archivo se puede modificar para personalizar el proceso de compilación y publicación. Este archivo se lee durante el proceso de publicación. `<LastUsedBuildConfiguration>` es especial porque es una propiedad global y no debe estar en ningún archivo importado en la compilación. Consulte [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (MSBuild: Cómo establecer la propiedad de configuración) para más información.

Al publicar en un destino de Azure, el archivo *.pubxml* contiene el identificador de suscripción de Azure. Con ese tipo de destino, no se recomienda agregar este archivo al control de código fuente. Al publicar en un destino que no sea Azure, es seguro insertar el archivo *.pubxml* en el repositorio.

La información confidencial (por ejemplo, la contraseña de publicación) se cifra en cada usuario y máquina. Este se almacena en el archivo *Properties/PublishProfiles/{NOMBRE DEL PERFIL}.pubxml.user*. Como este archivo puede contener información confidencial, no se debe insertar en el repositorio de control del código fuente.

Para información general sobre cómo publicar una aplicación web en ASP.NET Core, consulte [Hospedaje e implementación](xref:host-and-deploy/index), Las tareas de MSBuild y los destinos necesarios para publicar una aplicación ASP.NET Core son de código abierto en https://github.com/aspnet/websdk.

`dotnet publish` puede usar perfiles de publicación, de carpeta, Msdeploy y [Kudu](https://github.com/projectkudu/kudu/wiki):

Carpeta (funciona entre plataformas)

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

MSDeploy (actualmente solo funciona en Windows dado que MSDeploy no es multiplataforma):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

El paquete de MSDeploy (actualmente solo funciona en Windows dado que MSDeploy no es multiplataforma):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

En los ejemplos anteriores, **no** pase `deployonbuild` a `dotnet publish`.

Para más información, consulte [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).

`dotnet publish` admite las API de Kudu para publicar en Azure desde cualquier plataforma. La publicación de Visual Studio admite API de Kudu, pero es compatible con WebSDK para la publicación multiplataforma en Azure.

Agregue un perfil de publicación a la carpeta *Properties/PublishProfiles* con el contenido siguiente:

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

Ejecute el comando siguiente para descomprimir el contenido de publicación y publicarlo en Azure mediante las API de Kudu:

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

Establezca las siguientes propiedades de MSBuild cuando use un perfil de publicación:

* `DeployOnBuild=true`
* `PublishProfile={PUBLISH PROFILE}`

Por ejemplo, al efectuar una publicación con un perfil denominado *FolderProfile*, se puede ejecutar cualquiera de los comandos siguientes:

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Al invocar [dotnet build](/dotnet/core/tools/dotnet-build), se llama a `msbuild` para ejecutar el proceso de compilación y publicación. Llamar a `dotnet build` o `msbuild` es equivalente cuando se pasa un perfil de carpeta. Al llamar a MSBuild directamente en Windows, se usa la versión .NET Framework de MSBuild. Actualmente, MSDeploy está limitado a los equipos con Windows para efectuar publicaciones. Al llamar a `dotnet build` en un perfil que no es de carpeta se invoca a MSBuild, que usa MSDeploy en los perfiles que no son de carpeta. Al llamar a `dotnet build` en un perfil que no es de carpeta, se invoca a MSBuild (mediante MSDeploy), lo cual genera un error (incluso si se ejecuta en una plataforma de Windows). Para publicar con un perfil que no es de carpeta, llame directamente a MSBuild.

El siguiente perfil de publicación de carpeta se creó con Visual Studio y se publica en un recurso compartido de red:

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

Tenga en cuenta que `<LastUsedBuildConfiguration>` está establecido en `Release`. Al efectuar una publicación en Visual Studio, el valor de la propiedad de configuración `<LastUsedBuildConfiguration>` se establece con el valor cuando se inicia el proceso de publicación. La propiedad de configuración `<LastUsedBuildConfiguration>` es especial y no se debe reemplazar en un archivo importado de MSBuild. Esta propiedad se puede invalidar desde la línea de comandos.

Con la CLI de .NET Core:

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

Con MSBuild:

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Publicar en un punto de conexión de MSDeploy desde la línea de comandos

En el ejemplo siguiente se utiliza una aplicación web ASP.NET Core de ejemplo creada mediante Visual Studio y denominada *AzureWebApp*. Se agrega un perfil de publicación de aplicaciones de Azure con Visual Studio. Para obtener más información sobre cómo crear un perfil, consulte la sección [Perfiles de publicación](#publish-profiles).

Para implementar la aplicación con un perfil de publicación, ejecute el comando `msbuild` desde un **símbolo del sistema para desarrolladores** de Visual Studio. El símbolo del sistema se encuentra disponible en la carpeta *Visual Studio* del menú **Inicio**, en la barra de tareas de Windows. Para facilitar el acceso, puede agregar el símbolo del sistema al menú **Herramientas** de Visual Studio. Para más información, consulte [Símbolo del sistema para desarrolladores de Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).

MSBuild usa la sintaxis de comando siguiente:

```console
msbuild {PATH} 
    /p:DeployOnBuild=true 
    /p:PublishProfile={PROFILE} 
    /p:Username={USERNAME} 
    /p:Password={PASSWORD}
```

* {RUTA DE ACCESO}: ruta de acceso al archivo de proyecto de la aplicación.
* {PERFIL}: nombre del perfil de publicación.
* {NOMBRE DE USUARIO}: nombre de usuario de MSDeploy. El valor {NOMBRE DE USUARIO} se encuentra en el perfil de publicación.
* {CONTRASEÑA}: contraseña de MSDeploy. Puede obtener el valor {CONTRASEÑA} en el archivo *{PERFIL}.PublishSettings*. Descargue el archivo *.PublishSettings* desde:
  * Explorador de soluciones: Seleccione **Ver** > **Cloud Explorer**. Conéctese con su suscripción de Azure. Abra **App Services**. Haga clic con el botón derecho en la aplicación. Seleccione **Descargar perfil de publicación**.
  * Azure Portal: Haga clic en **Obtener perfil de publicación**, en el panel **Información general** de la aplicación web.

En el ejemplo siguiente se usa un perfil de publicación denominado *AzureWebApp - Web Deploy*:

```console
msbuild "AzureWebApp.csproj" 
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

También se puede usar un perfil de publicación ejecutando el comando [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) de la CLI de .NET Core en un símbolo del sistema de Windows:

```console
dotnet msbuild "AzureWebApp.csproj"
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

> [!NOTE]
> El comando [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) es multiplataforma y permite compilar aplicaciones ASP.NET Core en macOS y Linux. Sin embargo, MSBuild en macOS y Linux no permite implementar una aplicación en Azure ni en otro punto de conexión de MSDeploy. MSDeploy solo está disponible en Windows.

## <a name="set-the-environment"></a>Establecimiento del entorno

Incluya la propiedad `<EnvironmentName>` en el perfil de publicación (*.pubxml*) o el archivo del proyecto para configurar el [entorno](xref:fundamentals/environments) de la aplicación:

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

Si necesita transformaciones de *web.config* (por ejemplo, establecer variables de entorno basadas en la configuración, el perfil o el entorno), consulte <xref:host-and-deploy/iis/transform-webconfig>.

## <a name="exclude-files"></a>Archivos de exclusión

Al publicar aplicaciones web de ASP.NET Core, se incluyen los artefactos de compilación y el contenido de la carpeta *wwwroot*. `msbuild` admite los [patrones globales](https://gruntjs.com/configuring-tasks#globbing-patterns). Por ejemplo, el siguiente elemento `<Content>` excluye todo los archivos de texto (*.txt*) de la carpeta *wwwroot/content* y de todas sus subcarpetas.

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

El marcado anterior se puede agregar a un perfil de publicación o al archivo *.csproj*. Si se agrega al archivo *.csproj*, la regla se agrega a todos los perfiles de publicación del proyecto.

El siguiente elemento `<MsDeploySkipRules>` excluye todos los archivos de la carpeta *wwwroot/content*:

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` no eliminará del sitio de implementación los destinos *skip*. Los archivos y carpetas destinados a `<Content>` se eliminan del sitio de implementación. Por ejemplo, suponga que una aplicación web implementada tenía los siguientes archivos:

* *Views/Home/About1.cshtml*
* *Views/Home/About2.cshtml*
* *Views/Home/About3.cshtml*

Si se agregan los siguientes elementos `<MsDeploySkipRules>`, esos archivos no se eliminarán en el sitio de implementación.

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

Los elementos `<MsDeploySkipRules>` anteriores impiden que se implementen los archivos *omitidos*. Una vez que se implementan esos archivos, no se eliminarán.

El siguiente elemento `<Content>` elimina los archivos de destino en el sitio de implementación:

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

El uso de la implementación de línea de comandos con el elemento `<Content>` anterior, produce la siguiente salida:

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="include-files"></a>Archivos de inclusión

El siguiente marcado incluye una carpeta *images* fuera del directorio del proyecto a la carpeta *wwwroot/images* del sitio de publicación:

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

El marcado se puede agregar al archivo *.csproj* o al perfil de publicación. Si se agrega al archivo *.csproj*, se incluye en todos los perfiles de publicación del proyecto.

En el siguiente marcado resaltado se muestra cómo:

* Copiar un archivo de fuera del proyecto a la carpeta *wwwroot*.
* Excluir la carpeta *wwwroot\Content*.
* Excluir *Views\Home\About2.cshtml*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

Vea [WebSDK Readme](https://github.com/aspnet/websdk) (Archivo Léame de WebSDK) para ver más ejemplos de implementación.

## <a name="run-a-target-before-or-after-publishing"></a>Ejecutar un destino antes o después de la publicación

Los destinos `BeforePublish` y `AfterPublish` ejecutan un destino antes o después del destino de publicación. Agregue los siguientes elementos al perfil de publicación para registrar mensajes de la consola antes y después de la publicación:

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a>Publicación en un servidor mediante un certificado que no es de confianza

Agregue la propiedad `<AllowUntrustedCertificate>` con un valor de `True` al perfil de publicación:

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a>Servicio Kudu

Para ver los archivos de la implementación de una aplicación web de Azure App Service, use el [servicio Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Anexe el token `scm` al nombre de la aplicación web. Por ejemplo:

| Resolución                                    | Resultado       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | Aplicación web      |
| `http://mysite.scm.azurewebsites.net/` | Servicio Kudu |

Seleccione el elemento de menú [Consola de depuración](https://github.com/projectkudu/kudu/wiki/Kudu-console) para ver, editar, eliminar o agregar archivos.

## <a name="additional-resources"></a>Recursos adicionales

* [Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifica la implementación de aplicaciones web y sitios web en servidores de IIS.
* [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): problemas de archivos y características de solicitud para la implementación.
* [Publicación de una aplicación web ASP.NET en una máquina virtual de Azure desde Visual Studio](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
* <xref:host-and-deploy/iis/transform-webconfig>
