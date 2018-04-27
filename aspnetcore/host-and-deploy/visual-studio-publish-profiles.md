---
title: Perfiles para la implementación de aplicaciones de ASP.NET Core de publicación de Visual Studio
author: rick-anderson
description: Obtenga información acerca de cómo crear perfiles de publicación en Visual Studio y usarlas para administrar implementaciones de aplicaciones de ASP.NET Core para diferentes destinos.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3dc858793cd4ddb2630d05a5084f4b7caeaa30eb
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Perfiles para la implementación de aplicaciones de ASP.NET Core de publicación de Visual Studio

Por [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Este documento se centra en el uso de 2017 de Visual Studio para crear y usar perfiles de publicación. Los perfiles de publicación creados con Visual Studio se pueden ejecutar en MSBuild y en Visual Studio 2017. Vea [Publicar una aplicación web de ASP.NET Core en Azure App Service con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) para obtener instrucciones sobre la publicación en Azure.

El siguiente archivo de proyecto se creó con el comando `dotnet new mvc`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.5" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.6" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

---

El `<Project>` del elemento `Sdk` atributo lleva a cabo las siguientes tareas:

* Importa el archivo de propiedades de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* al principio.
* Importa el archivo targets de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* al final.

La ubicación predeterminada de `MSBuildSDKsPath` (con Visual Studio 2017 Enterprise) es la carpeta *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.

El `Microsoft.NET.Sdk.Web` depende de SDK:

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

Lo que hace que las propiedades y destinos que se importarán los siguientes:

* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*

Publicar la importación de destinos el derecho de conjunto de destinos en función del método de publicación que se utiliza.

Cuando Visual Studio o MSBuild carga un proyecto, se producen las siguientes acciones de alto nivel:

* Compilación del proyecto
* Cálculo de los archivos para la publicación
* Publicación de los archivos en el destino

## <a name="compute-project-items"></a>Cálculo de los elementos del proyecto

Cuando se carga el proyecto, se calculan los elementos del proyecto (archivos). El atributo `item type` determina cómo se procesa el archivo. De forma predeterminada, los archivos *.cs* se incluyen en la lista de elementos `Compile`. Después se compilan los archivos de la lista de elementos `Compile`.

El `Content` lista de elementos contiene archivos que se publican además de los resultados de compilación. De forma predeterminada, los archivos que coincidan con el patrón `wwwroot/**` se incluyen en el `Content` elemento. El `wwwroot/\*\*` [patrón de uso de comodines en](https://gruntjs.com/configuring-tasks#globbing-patterns) coincide con todos los archivos de la *wwwroot* carpeta **y** las subcarpetas. Para agregar un archivo de forma explícita a la lista de publicación, agregue el archivo directamente en el *.csproj* archivo tal como se muestra en [archivos de inclusión](#include-files).

Al seleccionar la **publicar** botón en Visual Studio o al publicar desde la línea de comandos:

* Se calculan los elementos/propiedades (los archivos que se deben compilar).
* **Visual Studio solo**: paquetes de NuGet se restauran. (la restauración debe ser explícita por parte del usuario en la CLI).
* Se compila el proyecto.
* Se calculan los elementos de publicación (los archivos que se deben publicar).
* Se publica el proyecto (los archivos calculados se copian en el destino de publicación).

Cuando hace referencia a un proyecto de ASP.NET Core `Microsoft.NET.Sdk.Web` en el archivo de proyecto, un *app_offline.htm* archivo se coloca en la raíz del directorio de aplicación web. Cuando el archivo está presente, el módulo de ASP.NET Core cierra correctamente la aplicación y proporciona el archivo *app_offline.htm* durante la implementación. Para más información, vea [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm) (Referencia de configuración del módulo de ASP.NET Core).

## <a name="basic-command-line-publishing"></a>Publicación de línea de comandos básica

Publicación de línea de comandos funciona en todas las plataformas compatibles con .NET Core y no requiere Visual Studio. En los ejemplos siguientes, el [publicar dotnet](/dotnet/core/tools/dotnet-publish) comando se ejecuta desde el directorio del proyecto (que contiene el *.csproj* archivo). Si no es así en la carpeta del proyecto, pasar de manera explícita en la ruta de acceso del archivo de proyecto. Por ejemplo:

```console
dotnet publish C:\Webs\Web1
```

Ejecute los comandos siguientes para crear y publicar una aplicación web:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

El [publicar dotnet](/dotnet/core/tools/dotnet-publish) comando produce un resultado similar al siguiente:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

La carpeta de publicación predeterminada es `bin\$(Configuration)\netcoreapp<version>\publish`, El valor predeterminado de `$(Configuration)` es *depurar*. En el ejemplo anterior, el `<TargetFramework>` es `netcoreapp2.0`.

`dotnet publish -h` muestra información de ayuda de la publicación.

El siguiente comando especifica una compilación `Release` y el directorio de publicación:

```console
dotnet publish -c Release -o C:\MyWebs\test
```

El [publicar dotnet](/dotnet/core/tools/dotnet-publish) comando llamadas MSBuild, que se invoca el `Publish` destino. Los parámetros pasados a `dotnet publish` se pasan a MSBuild. El parámetro `-c` se asigna a la propiedad de MSBuild `Configuration`. El parámetro `-o` se asigna a `OutputPath`.

Propiedades de MSBuild se pueden pasar mediante cualquiera de los siguientes formatos:

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

El comando siguiente publica una compilación `Release` en un recurso compartido de red:

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

El recurso compartido de red se especifica con barras diagonales (*//r8/*) y funciona en todas las plataformas compatibles de .NET Core.

Confirme que la aplicación publicada para la implementación no se está ejecutando. Los archivos de la carpeta *publish* quedan bloqueados mientras se ejecuta la aplicación. No se puede llevar a cabo la implementación porque no se pueden copiar los archivos bloqueados.

## <a name="publish-profiles"></a>Publicar los perfiles

Esta sección utiliza 2017 de Visual Studio para crear un perfil de publicación. Una vez creado, publicación de Visual Studio o la línea de comandos está disponible.

Publicar perfiles pueden simplificar el proceso de publicación, y puede existir cualquier número de perfiles. Crear un perfil de publicación en Visual Studio eligiendo una de las rutas de acceso siguientes:

* Haga clic en el proyecto en el Explorador de soluciones y seleccione **publicar**.
* Seleccione **publicar &lt;Nombre_proyecto&gt;**  desde el **generar** menú.

El **publicar** se muestra la pestaña de la página de las capacidades de la aplicación. Si el proyecto no tiene un perfil de publicación, se muestra la página siguiente:

![La pestaña de la publicación de la página de las capacidades de la aplicación](visual-studio-publish-profiles/_static/az.png)

Cuando **carpeta** está seleccionada, especifique una ruta de acceso de carpeta para almacenar los recursos publicados. La carpeta predeterminada es *bin\Release\PublishOutput*. Haga clic en el **Crear perfil** para terminar.

Una vez que se crea un perfil de publicación, la **publicar** pestaña cambios. El perfil recién creado aparece en una lista desplegable. Haga clic en **crear nuevo perfil** para crear otro nuevo perfil.

![La pestaña de la publicación de la página de las capacidades de la aplicación que muestra FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

El Asistente para publicación admite los siguientes destinos de publicación:

* Azure App Service
* Azure Virtual Machines
* IIS, FTP, etc. (para cualquier servidor web)
* Carpeta
* Perfil de importación

Para obtener más información, consulte [qué opciones de publicación son adecuadas para mí](/visualstudio/ide/not-in-toc/web-publish-options).

Al crear un perfil de publicación con Visual Studio, un *propiedades/PublishProfiles/&lt;profile_name&gt;.pubxml* se crea el archivo de MSBuild. El *.pubxml* archivo es un archivo de MSBuild y contiene opciones de configuración de publicación. Este archivo se puede cambiar para personalizar la compilación y el proceso de publicación. Este archivo se lee durante el proceso de publicación. `<LastUsedBuildConfiguration>` es especial porque es una propiedad global y no debe estar en cualquier archivo que se importa en la compilación. Vea [MSBuild: cómo establecer la propiedad de configuración](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) para obtener más información.

Al publicar en un destino de Azure, el *.pubxml* archivo contiene el identificador de suscripción de Azure. Con ese tipo de destino, agregar este archivo al control de código fuente no se recomienda. Al publicar en un destino no sea de Azure, es seguro proteger el *.pubxml* archivo.

Información confidencial (por ejemplo, la contraseña de publicación) se cifra en una por cada nivel de usuario/equipo. Se almacena en la *propiedades/PublishProfiles/&lt;profile_name&gt;. pubxml.user* archivo. Dado que este archivo puede almacenar información confidencial, se no debe proteger en el control de código fuente.

Para obtener información general sobre cómo publicar una aplicación web de ASP.NET Core, vea [Host e implementar](xref:host-and-deploy/index). Las tareas de MSBuild y los destinos necesarios para publicar una aplicación de ASP.NET Core son código abierto en https://github.com/aspnet/websdk.

`dotnet publish` Puede usar la carpeta, MSDeploy, y [Kudu](https://github.com/projectkudu/kudu/wiki) perfiles de publicación:

Carpeta (funcione entre plataformas):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

MSDeploy (actualmente este sólo funciona en Windows desde MSDeploy no multiplataforma):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

Paquete de MSDeploy (actualmente este sólo funciona en Windows desde MSDeploy no multiplataforma):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

En los ejemplos anteriores, **no** pasar `deployonbuild` a `dotnet publish`.

Para obtener más información, consulte [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).

`dotnet publish` admite las API de Kudu para publicar en Azure desde cualquier plataforma. Visual Studio publicar admite las API Kudu, pero es compatible con WebSDK para multiplataforma publicar en Azure.

Agregar un perfil de publicación para la *propiedades/PublishProfiles* carpeta con el siguiente contenido:

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

Ejecute el siguiente comando para comprimir el contenido de la publicación y publicarlo en Azure mediante las API de Kudu:

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

Establezca las siguientes propiedades de MSBuild cuando use un perfil de publicación:

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

Cuando se publica con un perfil denominado *FolderProfile*, se puede ejecutar cualquiera de los comandos siguientes:

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Al invocar [dotnet compilación](/dotnet/core/tools/dotnet-build), llama a `msbuild` para ejecutar la compilación y proceso de publicación. Una llamada a `dotnet build` o `msbuild` es equivalente al pasar de un perfil de la carpeta. Al llamar a MSBuild directamente en Windows, se utiliza la versión de .NET Framework de MSBuild. Actualmente, MSDeploy está limitado a los equipos con Windows para efectuar publicaciones. Al llamar a `dotnet build` en un perfil que no es de carpeta se invoca a MSBuild, que usa MSDeploy en los perfiles que no son de carpeta. Al llamar a `dotnet build` en un perfil que no es de carpeta, se invoca a MSBuild (mediante MSDeploy), lo cual genera un error (incluso si se ejecuta en una plataforma de Windows). Para publicar con un perfil que no es de carpeta, llame directamente a MSBuild.

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

Tenga en cuenta que `<LastUsedBuildConfiguration>` está establecido en `Release`. Al efectuar una publicación en Visual Studio, el valor de la propiedad de configuración `<LastUsedBuildConfiguration>` se establece con el valor cuando se inicia el proceso de publicación. El `<LastUsedBuildConfiguration>` propiedad de configuración es especial y no debe reemplazarse en un archivo de MSBuild importado. Esta propiedad se puede invalidar desde la línea de comandos.

Mediante el uso de la CLI de .NET Core:

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

Con MSBuild:

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Publicar en un punto de conexión de MSDeploy desde la línea de comandos

La publicación puede realizarse mediante la CLI de núcleo de .NET o MSBuild. `dotnet publish` se ejecuta en el contexto de .NET Core. El `msbuild` comando requiere .NET Framework, lo que limita a los entornos de Windows.

La forma más fácil de publicar con MSDeploy consiste en crear primero un perfil de publicación en Visual Studio 2017 y, luego, usar el perfil en la línea de comandos.

En el ejemplo siguiente, se crea una aplicación web de ASP.NET Core (mediante `dotnet new mvc`), y se agrega un perfil de publicación de Azure con Visual Studio.

Ejecutar `msbuild` desde una **símbolo del sistema para desarrolladores de VS 2017**. El símbolo del sistema para desarrolladores tiene el valor correcto *msbuild.exe* en su ruta de acceso con un conjunto de variables de MSBuild.

MSBuild usa la sintaxis siguiente:

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

Obtener la `Password` desde el *\<nombre de publicación >. Configuración de publicación* archivo. Descargue el *. Configuración de publicación* archivo desde:

* El Explorador de soluciones: Haga doble clic en la aplicación Web y seleccione **descargar un perfil de publicación**.
* Portal de Azure: haga clic en **perfil de publicación Get** en la aplicación Web **Introducción** panel.

`Username` está en el perfil de publicación.

El siguiente ejemplo se utiliza la *Web11112 - Web Deploy* perfil de publicación:

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a>Excluir archivos

Al publicar aplicaciones web de ASP.NET Core, se incluyen los artefactos de compilación y el contenido de la carpeta *wwwroot*. `msbuild` admite los [patrones globales](https://gruntjs.com/configuring-tasks#globbing-patterns). Por ejemplo, la siguiente `<Content>` elemento excluye todo el texto (*.txt*) archivos desde el *wwwroot/content* carpeta y todas sus subcarpetas.

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

El marcado anterior puede agregarse a un perfil de publicación o la *.csproj* archivo. Si se agrega al archivo *.csproj*, la regla se agrega a todos los perfiles de publicación del proyecto.

El siguiente `<MsDeploySkipRules>` elemento excluye todos los archivos de la *wwwroot/content* carpeta:

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` No eliminar el *omitir* destinos desde el sitio de implementación. `<Content>` las carpetas y archivos de destino se eliminan desde el sitio de implementación. Por ejemplo, suponga que una aplicación web implementadas tenía los siguientes archivos:

* *Views/Home/About1.cshtml*
* *Views/Home/About2.cshtml*
* *Views/Home/About3.cshtml*

Si el siguiente `<MsDeploySkipRules>` se agregan elementos, no podrá eliminar esos archivos en el sitio de implementación.

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

Los anteriores `<MsDeploySkipRules>` elementos evitar el *omitido* archivos desde que se está implementando. Una vez que se implementen no elimina esos archivos.

El siguiente `<Content>` elemento elimina los archivos de destino en el sitio de implementación:

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Mediante la implementación de línea de comandos a la anterior `<Content>` elemento produce el siguiente resultado:

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

El siguiente marcado incluye un *imágenes* carpeta fuera del directorio de proyecto para el *wwwroot/imágenes* carpeta del sitio de publicación:

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

El marcado se puede agregar al archivo *.csproj* o al perfil de publicación. Si se agrega a la *.csproj* archivo, se incluye en cada perfil de publicación en el proyecto.

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

Integrado `BeforePublish` y `AfterPublish` destinos ejecutan un destino antes o después del destino de publicación. Agregue los siguientes elementos para el perfil de publicación para registrar mensajes de la consola antes y después de la publicación:

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a>Publicar en un servidor mediante un certificado de confianza

Agregar el `<AllowUntrustedCertificate>` propiedad con un valor de `True` para el perfil de publicación:

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a>Servicio Kudu

Para ver los archivos en una implementación de aplicaciones web de servicio de aplicaciones de Azure, use la [servicio Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Anexar la `scm` token utilizado para el nombre de la aplicación web. Por ejemplo:

| Resolución                                    | Resultado       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | Aplicación web      |
| `http://mysite.scm.azurewebsites.net/` | Servicio de kudu |

Seleccione el [consola de depuración](https://github.com/projectkudu/kudu/wiki/Kudu-console) elemento de menú para ver, editar, eliminar o agregar archivos.

## <a name="additional-resources"></a>Recursos adicionales

* [Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifica la implementación de aplicaciones web y sitios Web a los servidores IIS.
* [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Problemas de archivos y solicitar características para la implementación.
