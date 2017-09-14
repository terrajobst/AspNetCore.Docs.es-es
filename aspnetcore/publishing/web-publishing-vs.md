---
title: "Crear perfiles de publicación para Visual Studio y MSBuild"
author: rick-anderson
description: "Explica la publicación web en Visual Studio."
keywords: "ASP.NET Core, publicación web, publicación, msbuild, implementación web, dotnet publish, Visual Studio 2017"
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.assetid: 0377a02d-8fda-47a5-929a-24a16e1d2c93
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/web-publishing-vs
ms.openlocfilehash: 872e8c99734a0913770281d9d87b1e30c250ab0a
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2017
---
# <a name="create-publish-profiles-for-visual-studio-and-msbuild-to-deploy-aspnet-core-apps"></a>Crear perfiles de publicación para Visual Studio y MSBuild para implementar aplicaciones ASP.NET Core

Por [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Este artículo se centra en el uso de Visual Studio 2017 para crear perfiles de publicación. Los perfiles de publicación creados con Visual Studio se pueden ejecutar en MSBuild y en Visual Studio 2017.

El siguiente archivo *.csproj* se creó con el comando `dotnet new mvc`:

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
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.1" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.2" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.1" />
  </ItemGroup>

</Project>
```

---

El atributo `Sdk` del elemento `<Project>` (en la primera línea) del marcado anterior hace lo siguiente:

* Importa el archivo `props` de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* al principio.
* Importa el archivo targets de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* al final.

La ubicación predeterminada de `MSBuildSDKsPath` (con Visual Studio 2017 Enterprise) es la carpeta *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.

`Microsoft.NET.Sdk.Web` depende de:

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

Esto hace que se importen los siguientes archivos `props` y `targets`:

* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets

La publicación de los archivos targets volverá a importar el conjunto correcto de archivos targets en función del método de publicación empleado.

Cuando Visual Studio o MSBuild carga un proyecto, se llevan a cabo las siguientes acciones de alto nivel:

* Compilación del proyecto
* Cálculo de los archivos para la publicación
* Publicación de los archivos en el destino

### <a name="compute-project-items"></a>Cálculo de los elementos del proyecto

Cuando se carga el proyecto, se calculan los elementos del proyecto (archivos). El atributo `item type` determina cómo se procesa el archivo. De forma predeterminada, los archivos *.cs* se incluyen en la lista de elementos `Compile`. Después se compilan los archivos de la lista de elementos `Compile`.

La lista de elementos `Content` contiene archivos que se van a publicar, además de los resultados de compilación. De forma predeterminada, los archivos que coincidan con el patrón wwwroot/** se incluirán en el elemento `Content`. [wwwroot/** es un patrón global](https://gruntjs.com/configuring-tasks#globbing-patterns) que especifica todos los archivos de la carpeta *wwwroot* **y** de las subcarpetas. Si necesita agregar explícitamente un archivo a la lista de publicación, puede agregar el archivo directamente en el archivo *.csproj*, como se muestra en [Incluir archivos](#including-files).

Al seleccionar el botón **Publicar** en Visual Studio o al efectuar una publicación desde la línea de comandos:

- Se calculan los elementos/propiedades (los archivos que se deben compilar).
- Solo para Visual Studio: se restauran los paquetes NuGet  (la restauración debe ser explícita por parte del usuario en la CLI).
- Se compila el proyecto.
- Se calculan los elementos de publicación (los archivos que se deben publicar).
- Se publica el proyecto (los archivos calculados se copian en el destino de publicación).

## <a name="simple-command-line-publishing"></a>Publicación simple en la línea de comandos

Esta sección es válida para todas las plataformas compatibles de .NET Core y no es necesario disponer de Visual Studio. En los ejemplos siguientes, el comando `dotnet publish` se ejecuta desde el directorio del proyecto (que contiene el archivo *.csproj*). Si no se encuentra en la carpeta del proyecto, puede pasar de forma explícita la ruta de acceso del archivo del proyecto. Por ejemplo:

```console
dotnet publish  c:/webs/web1
```

Ejecute los comandos siguientes para crear y publicar una aplicación web:

```console
dotnet new mvc
dotnet restore
dotnet publish
```

El comando `dotnet publish` genera un resultado similar al siguiente:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.1.548.43366
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp1.1\Web1.dll
```

La carpeta de publicación predeterminada es `bin\$(Configuration)\netcoreapp<version>\publish`, mientras que la de `$(Configuration)` es Debug. En el ejemplo anterior, el `<TargetFramework>` es `netcoreapp1.1`. La ruta de acceso real del ejemplo anterior es *bin\Debug\netcoreapp1.1\publish*.

`dotnet publish -h` muestra información de ayuda de la publicación.

El siguiente comando especifica una compilación `Release` y el directorio de publicación:

```console
dotnet publish -c Release -o C:/MyWebs/test
```

El comando `dotnet publish` llama a `MSBuild`, que invoca el destino `Publish`. Todos los parámetros pasados a `dotnet publish` se pasan a `MSBuild`. El parámetro `-c` se asigna a la propiedad de MSBuild `Configuration`. El parámetro `-o` se asigna a `OutputPath`.

Puede pasar propiedades de MSBuild mediante cualquiera de los siguientes formatos:

- ` p:<NAME>=<VALUE>`
- `/p:<NAME>=<VALUE>`

El comando siguiente publica una compilación `Release` en un recurso compartido de red:

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

El recurso compartido de red se especifica con barras diagonales (*//r8/*) y funciona en todas las plataformas compatibles de .NET Core.

Confirme que la aplicación publicada para la implementación no se está ejecutando. Los archivos de la carpeta *publish* quedan bloqueados mientras se ejecuta la aplicación. No se puede llevar a cabo la implementación porque no se pueden copiar los archivos bloqueados.

## <a name="publish-profiles"></a>Publicar los perfiles

En esta sección se usa Visual Studio 2017 y versiones posteriores para crear perfiles de publicación. Una vez creados, puede publicarlos en Visual Studio o en la línea de comandos.

Los perfiles de publicación pueden simplificar el proceso de publicación. Puede tener varios perfiles de publicación. Para crear un perfil de publicación en Visual Studio, haga clic con el botón derecho en el proyecto en el Explorador de soluciones y seleccione **Publicar**. Como alternativa, puede seleccionar **Publicar \<Nombre del proyecto >** en el menú Compilar. Aparecerá la pestaña **Publicar** de la página de las capacidades de la aplicación. Si el proyecto no contiene ningún perfil de publicación, se mostrará la siguiente página:

![Pestaña **Publicar** de la página de las capacidades de la aplicación, donde se muestran las opciones Azure, IIS, FTP y Carpeta, con la opción Azure seleccionada. También se muestran los botones de opción Crear nuevo y Seleccionar existente](web-publishing-vs/_static/az.png)

Al seleccionar **Carpeta**, el botón **Publicar** crea un perfil de publicación para la carpeta y lo publica.

![Pestaña **Publicar** de la página de las capacidades de la aplicación, donde se muestran las opciones Azure, IIS, FTP y Carpeta](web-publishing-vs/_static/pub1.png)

Una vez creado el perfil de publicación, la pestaña **Publicar** cambia y puede seleccionar **Crear nuevo perfil** para crear un perfil.

![Pestaña **Publicar** de la página de las capacidades de la aplicación, donde se muestra FolderProfile (creado en el último paso) y el vínculo Crear nuevo perfil](web-publishing-vs/_static/create_new.png)

El Asistente para publicación admite los siguientes destinos de publicación:

- Microsoft Azure App Service
- IIS, FTP, etc. (para cualquier servidor web)
- Carpeta
- Perfil de importación (le permite importar un perfil).
- Microsoft Azure Virtual Machines

Vea [¿Qué opciones de publicación son las adecuadas para mí?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) para más información.

Al crear un perfil de publicación con Visual Studio, se crea un archivo de MSBuild *Properties/PublishProfiles/\<nombre_publicación>.pubxml*. Este archivo *.pubxml* es un archivo de MSBuild que contiene opciones de configuración de publicación. Puede modificar este archivo para personalizar la compilación y el proceso de publicación. Este archivo se lee durante el proceso de publicación. `<LastUsedBuildConfiguration>` es especial porque es una propiedad global y no debe estar en ningún archivo importado en la compilación. Vea [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (MSBuild: Cómo establecer la propiedad de configuración) para más información.
El archivo *.pubxml* no debe estar protegido bajo control de código fuente porque depende del archivo *.user*. El archivo *.user* nunca debe estar protegido bajo control de código fuente porque puede contener información confidencial y solo es válido para un usuario y un equipo.

La información confidencial (como la contraseña de publicación) se cifra a nivel de usuario/equipo y se almacenan en el archivo *Properties/PublishProfiles/\<nombre_publicación>.pubxml.user*. Dado que este archivo puede contener información confidencial, **no** debe estar protegido bajo control de código fuente.

Para obtener información general sobre cómo publicar una aplicación web en ASP.NET Core, vea [Publishing and Deployment](index.md) (Publicación e implementación), [Publishing and Deployment](index.md) (Publicación e implementación) que es un proyecto de código abierto que encontrará en https://github.com/aspnet/websdk.

Actualmente, `dotnet publish` no tiene la capacidad de usar perfiles de publicación. Para usar perfiles de publicación, use `dotnet build`. `dotnet build` invoca a MSBuild en el proyecto. Como alternativa, puede llamar a `msbuild` directamente.

Establezca las siguientes propiedades de MSBuild cuando use un perfil de publicación:

- `DeployOnBuild=true`
- `PublishProfile=<Publish profile name>`

Por ejemplo, al efectuar una publicación con un perfil denominado *FolderProfile*, puede ejecutar cualquiera de los comandos siguientes.

- `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
- `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Al invocar a `dotnet build`, llamará a `msbuild` para ejecutar la compilación y publicar el proceso. Llamar a `dotnet build` o a `msbuild` es prácticamente lo mismo que pasar un perfil de la carpeta. Al llamar a MSBuild directamente en Windows, obtiene la versión .NET Framework de MSBuild.  Actualmente, MSDeploy está limitado a los equipos con Windows para efectuar publicaciones. Al llamar a `dotnet build` en un perfil que no es de carpeta se invoca a MSBuild, que usa MSDeploy en los perfiles que no son de carpeta. Al llamar a `dotnet build` en un perfil que no es de carpeta, se invoca a MSBuild (mediante MSDeploy), lo cual genera un error (incluso si se ejecuta en una plataforma de Windows). Para publicar con un perfil que no es de carpeta, llame directamente a MSBuild.

El siguiente perfil de publicación de carpeta se creó con Visual Studio y se publica en un recurso compartido de red:

[!code-xml[Main](web-publishing-vs/sample/FolderProfile.pubxml?highlight=5,9,11,18)]

Tenga en cuenta que `<LastUsedBuildConfiguration>` está establecido en `Release`.  Al efectuar una publicación en Visual Studio, el valor de la propiedad de configuración `<LastUsedBuildConfiguration>` se establece con el valor cuando se inicia el proceso de publicación. La propiedad de configuración `<LastUsedBuildConfiguration>` es especial y no se debe reemplazar en un archivo importado de MSBuild. Puede reemplazar esta propiedad desde la línea de comandos. Por ejemplo:

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Con MSBuild:

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Publicar en un punto de conexión de MSDeploy desde la línea de comandos

Como se ha mencionado anteriormente, puede efectuar una publicación mediante `dotnet publish` o con el comando `msbuild`. `dotnet publish` se ejecuta en el contexto de .NET Core. `msbuild` requiere .NET Framework; por lo tanto, está limitado a los entornos de Windows.

La forma más fácil de publicar con MSDeploy consiste en crear primero un perfil de publicación en Visual Studio 2017 y, luego, usar el perfil en la línea de comandos.

En el ejemplo siguiente he creado una aplicación web de ASP.NET Core (con `dotnet new mvc`) y he agregado un perfil de publicación de Azure con Visual Studio.

Debe ejecutar `msbuild` desde un **Símbolo del sistema para desarrolladores de VS 2017**. El Símbolo del sistema para desarrolladores tendrá el archivo *msbuild.exe* correcto en su ruta de acceso y establecerá algunas variables de MSBuild.

MSBuild usa la sintaxis siguiente:

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile>  /p:Username=<USERNAME> /p:Password=<PASSWORD>`

Puede obtener la `Password` del archivo *\<nombre_publicación>.PublishSettings*. Puede descargar el archivo *.PublishSettings* en:

- Explorador de soluciones: Haga clic con el botón derecho en la aplicación web y seleccione **Descargar perfil de publicación**.
- Portal de administración de Azure: Seleccione **Obtener perfil de publicación** en la hoja Aplicación web.

`Username` está en el perfil de publicación.

En el siguiente ejemplo se usa el perfil de publicación "Web11112 - Web Deploy":

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a>Excluir archivos

Al publicar aplicaciones web de ASP.NET Core, se incluyen los artefactos de compilación y el contenido de la carpeta *wwwroot*. `msbuild` admite los [patrones globales](https://gruntjs.com/configuring-tasks#globbing-patterns). Por ejemplo, el siguiente marcado del elemento `<Content>` excluirá todo los archivos de texto (*.txt*) de la carpeta *wwwroot/content* y de todas sus subcarpetas.

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

El marcado anterior se puede agregar a un perfil de publicación o al archivo *.csproj*. Si se agrega al archivo *.csproj*, la regla se agrega a todos los perfiles de publicación del proyecto.

El siguiente marcado del elemento `<MsDeploySkipRules>` excluye todos los archivos de la carpeta *wwwroot/content*:

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` no eliminará del sitio de implementación los destinos *skip*. Los archivos y carpetas destinados a `<Content>` se eliminarán del sitio de implementación. Por ejemplo, imagínese que ha implementado una aplicación web con los siguientes archivos:

- *Views/Home/About1.cshtml*
- *Views/Home/About2.cshtml*
- *Views/Home/About3.cshtml*

Si hubiera agregado el siguiente marcado `<MsDeploySkipRules>`, no se habrían eliminado los archivos del sitio de implementación.

``` xml
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

El marcado `<MsDeploySkipRules>` anterior impide que se implementen los archivos *omitidos*, pero no eliminará los archivos una vez que se hayan implementado.

El siguiente marcado `<Content>` eliminaría del sitio de implementación los archivos objetivo:

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

El uso de la implementación de la línea de comandos con el marcado `<Content>` anterior habría generado un resultado similar al siguiente:

``` console
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

## <a name="including-files"></a>Incluir archivos

Se puede usar el siguiente marcado para incluir una carpeta *images* fuera del directorio del proyecto en la carpeta *wwwroot/images* del sitio de publicación.

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

El marcado se puede agregar al archivo *.csproj* o al perfil de publicación. Si se agrega al archivo *.csproj*, se incluirá en todos los perfiles de publicación del proyecto.

En el siguiente marcado resaltado se muestra cómo:

* Copiar un archivo de fuera del proyecto a la carpeta *wwwroot*.
* Excluir la carpeta *wwwroot\Content*.
* Excluir *Views\Home\About2.cshtml*.

[!code-xml[Main](web-publishing-vs/sample/FolderProfile2.pubxml?highlight=21-29)]

Vea [WebSDK Readme](https://github.com/aspnet/websdk) (Archivo Léame de WebSDK) para ver más ejemplos de implementación.

### <a name="run-a-target-before-or-after-publishing"></a>Ejecutar un destino antes o después de la publicación

Los destinos `BeforePublish` y `AfterPublish` integrados se pueden usar para ejecutar un destino antes o después del destino de publicación. Se puede agregar el siguiente marcado al perfil de publicación para registrar mensajes en la salida de la consola antes y después de la publicación:

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a>Servicio Kudu

Para ver los archivos en la aplicación web de Azure, use el [servicio Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Anexe el token `scm` al nombre o a la aplicación web. Por ejemplo:

| Dirección URL               | Resultado|
| ----------------- | ------------ |
| `http://mysite.azurewebsites.net/` | Aplicación web |
| `http://mysite.scm.azurewebsites.net/` | Servicio Kudu |

Seleccione el elemento de menú [Consola de depuración](https://github.com/projectkudu/kudu/wiki/Kudu-console) para ver/editar/eliminar/agregar archivos.

## <a name="additional-resources"></a>Recursos adicionales

- [Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (msdeploy) simplifica la implementación de aplicaciones web y de sitios web en los servidores de IIS.

- [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): problemas de archivos y solicitud de características para la implementación.
