---
title: SDK de Razor de ASP.NET Core
author: Rick-Anderson
description: Obtenga información sobre cómo las páginas de Razor de ASP.NET Core facilitan la programación de escenarios centrados en páginas y hacen que resulte más productiva que con MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/23/2019
no-loc:
- Blazor
uid: razor-pages/sdk
ms.openlocfilehash: 872d90662494735dc0e4caa01c46fcdcc2606bc6
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2020
ms.locfileid: "76809138"
---
# <a name="aspnet-core-razor-sdk"></a>SDK de Razor de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="overview"></a>Información general del

El [!INCLUDE[](~/includes/2.1-SDK.md)] incluye la `Microsoft.NET.Sdk.Razor` MSBuild SDK (SDK de Razor). El SDK de Razor:

::: moniker range=">= aspnetcore-3.0"

* Se requiere para compilar, empaquetar y publicar proyectos que contengan archivos [Razor](xref:mvc/views/razor) para proyectos de ASP.net Core basado en MVC o [increíbles](xref:blazor/index) .
* Incluye un conjunto de destinos, propiedades y elementos predefinidos que permiten personalizar la compilación de archivos Razor ( *. cshtml* o *. Razor*).

El SDK de Razor incluye `Content` elementos con `Include` atributos establecidos en los patrones `**\*.cshtml` y `**\*.razor` comodines. Se publican los archivos coincidentes.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* Normaliza la experiencia de creación, empaquetado y publicación de proyectos que contienen archivos de [Razor](xref:mvc/views/razor) relativos a proyectos basados en ASP.NET Core MVC.
* Incluye un conjunto de destinos, propiedades y elementos predefinidos que permiten personalizar la compilación de archivos de Razor.

El SDK de Razor incluye un elemento `Content` con un atributo `Include` establecido en el patrón `**\*.cshtml` comodines. Se publican los archivos coincidentes.

::: moniker-end

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a>Usar el SDK de Razor

La mayoría de las aplicaciones web no están necesario hacer referencia explícitamente el SDK de Razor.

::: moniker range=">= aspnetcore-3.0"

Para usar el SDK de Razor para compilar bibliotecas de clases que contengan vistas de Razor o Razor Pages, se recomienda comenzar con la plantilla de proyecto de la biblioteca de clases de Razor (RCL). Un RCL que se usa para compilar los archivos ( *. Razor*) mínimos requiere una referencia al paquete [Microsoft. AspNetCore. Components](https://www.nuget.org/packages/Microsoft.AspNetCore.Components) . Un RCL que se usa para compilar vistas o páginas de Razor (archivos *. cshtml* ) requiere mínimamente el destino `netcoreapp3.0` o posterior y tiene un `FrameworkReference` al [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) en su archivo de proyecto.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Para usar el SDK de Razor para crear bibliotecas de clases que contengan vistas de Razor o páginas de Razor:

* Use `Microsoft.NET.Sdk.Razor` en lugar de `Microsoft.NET.Sdk`:

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* Normalmente, una referencia de paquete para `Microsoft.AspNetCore.Mvc` se requiere para recibir las dependencias adicionales que son necesarios para generar y compilar las páginas de Razor y las vistas de Razor. Como mínimo, el proyecto debe agregar referencias de paquete para:

  * `Microsoft.AspNetCore.Razor.Design`
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`

  El `Microsoft.AspNetCore.Razor.Design` paquete proporciona las tareas de compilación de Razor y destinos para el proyecto.

  Los paquetes anteriores están incluidos en `Microsoft.AspNetCore.Mvc`. El marcado siguiente muestra un archivo de proyecto que usa el SDK de Razor para generar archivos de Razor para una aplicación de páginas de Razor de ASP.NET Core:

  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> El `Microsoft.AspNetCore.Razor.Design` y `Microsoft.AspNetCore.Mvc.Razor.Extensions` paquetes se incluyen en el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app). Sin embargo, la versión de menor `Microsoft.AspNetCore.App` referencia de paquete proporciona un metapaquete a la aplicación que no incluye la versión más reciente de `Microsoft.AspNetCore.Razor.Design`. Deben hacer referencia a una versión coherente de los proyectos `Microsoft.AspNetCore.Razor.Design` (o `Microsoft.AspNetCore.Mvc`) para que se incluyen las correcciones más recientes de tiempo de compilación para Razor. Para más información, consulte [este problema de GitHub](https://github.com/aspnet/Razor/issues/2553).

::: moniker-end

### <a name="properties"></a>Propiedades

Las siguientes propiedades controlan el comportamiento del SDK de Razor como parte de la generación de un proyecto:

* `RazorCompileOnBuild` &ndash; Cuando `true`, compila y se emite el ensamblado de Razor como parte de la creación del proyecto. Tiene como valor predeterminado `true`.
* `RazorCompileOnPublish` &ndash; Cuando `true`, compila y se emite el ensamblado de Razor como parte de la publicación del proyecto. Tiene como valor predeterminado `true`.

Las propiedades y elementos en la tabla siguiente se utilizan para configurar las entradas y salida para el SDK de Razor.

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> A partir de ASP.NET Core 3,0, las vistas o Razor Pages de MVC no se atienden de forma predeterminada si las propiedades de MSBuild `RazorCompileOnBuild` o `RazorCompileOnPublish` del archivo de proyecto están deshabilitadas. Las aplicaciones deben agregar una referencia explícita al paquete [Microsoft. AspNetCore. Mvc. Razor. RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation) si la aplicación se basa en la compilación en tiempo de ejecución para procesar archivos *. cshtml* .

::: moniker-end

| Items | Descripción |
| ----- | ----------- |
| `RazorGenerate` | Elementos Item (archivos *. cshtml* ) que son entradas para la generación de código. |
| `RazorComponent` | Elementos Item (archivos *. Razor* ) que son entradas para la generación de código de componentes de Razor. |
| `RazorCompile` | Elementos de elemento (archivos *. CS* ) que son entradas para los destinos de compilación de Razor. Use este `ItemGroup` para especificar archivos adicionales que se van a compilar en el ensamblado de Razor. |
| `RazorTargetAssemblyAttribute` | Elementos que se usan para generar atributos de código para el ensamblado de Razor. Por ejemplo:  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | Elementos que se agregan como recursos incrustados en el ensamblado generado de Razor. |

| La propiedad | Descripción |
| -------- | ----------- |
| `RazorTargetName` | Nombre de archivo (sin extensión) del ensamblado generado por Razor. |
| `RazorOutputPath` | Directorio de salida de Razor. |
| `RazorCompileToolset` | Sirve para determinar el conjunto de herramientas que se va a usar para compilar el ensamblado de Razor. Valores válidos son `Implicit`, `RazorSDK` y `PrecompilationTool`. |
| [EnableDefaultContentItems](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | El valor predeterminado es `true`. Cuando `true`, incluye los archivos *Web. config*, *. JSON*y *. cshtml* como contenido en el proyecto. Cuando se hace referencia a través de `Microsoft.NET.Sdk.Web`, los archivos en *wwwroot* y también se incluyen los archivos de configuración. |
| `EnableDefaultRazorGenerateItems` | Si es `true`, incluye los archivos *.cshtml* de los elementos `Content` en elementos `RazorGenerate`. |
| `GenerateRazorTargetAssemblyInfo` | Cuando `true`, genera un *.cs* archivo que contiene los atributos especificados por `RazorAssemblyAttribute` e incluye el archivo en la salida de compilación. |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | Si es `true`, agrega a `RazorAssemblyAttribute` un conjunto predeterminado de atributos de ensamblado. |
| `CopyRazorGenerateFilesToPublishDirectory` | Cuando `true`, copias `RazorGenerate` elementos ( *.cshtml*) archivos en el directorio de publicación. Normalmente, los archivos de Razor no son necesarios para una aplicación publicada si participa en la compilación en tiempo de compilación o tiempo de publicación. Tiene como valor predeterminado `false`. |
| `CopyRefAssembliesToPublishDirectory` | Si es `true`, copia los elementos de referencia del ensamblado en el directorio de publicación. Normalmente, los ensamblados de referencia no son necesarios para una aplicación publicada si la compilación de Razor se produce en tiempo de compilación o tiempo de publicación. Establecido en `true` si la aplicación publicada requiere compilación en tiempo de ejecución. Por ejemplo, establezca el valor en `true` si modifica la aplicación *.cshtml* archivos en tiempo de ejecución o usa vistas incrustadas. Tiene como valor predeterminado `false`. |
| `IncludeRazorContentInPack` | Cuando `true`, todos los elementos de contenido de Razor ( *.cshtml* archivos) se marcan para su inclusión en el paquete NuGet generado. Tiene como valor predeterminado `false`. |
| `EmbedRazorGenerateSources` | Si es `true`, agrega los elementos de RazorGenerate ( *.cshtml*) como archivos incrustados al ensamblado de Razor generado. Tiene como valor predeterminado `false`. |
| `UseRazorBuildServer` | Si es `true`, usa un proceso de servidor de compilación persistente para descargar el trabajo de generación de código. El valor predeterminado es `UseSharedCompilation`. |
| `GenerateMvcApplicationPartsAssemblyAttributes` | Cuando `true`, el SDK genera atributos adicionales usados por MVC en tiempo de ejecución para realizar la detección de la parte de la aplicación. |
| `DefaultWebContentItemExcludes` | Un patrón comodines para elementos de elemento que se van a excluir del grupo de elementos `Content` en proyectos que tienen como destino el SDK web o Razor. |
| `ExcludeConfigFilesFromBuildOutput` | Cuando `true`, los archivos *. config* y *. JSON* no se copian en el directorio de salida de la compilación. |
| `AddRazorSupportForMvc` | Cuando `true`, configura el SDK de Razor para agregar compatibilidad con la configuración de MVC necesaria para crear aplicaciones que contengan vistas de MVC o Razor Pages. Esta propiedad se establece implícitamente para los proyectos de .NET Core 3,0 o posteriores que tienen como destino el SDK Web |
| `RazorLangVersion` | Versión del lenguaje de Razor que se va a usar como destino. |

Para más información sobre las propiedades, consulte [Propiedades de MSBuild](/visualstudio/msbuild/msbuild-properties).

### <a name="targets"></a>Destinos

El SDK de Razor establece dos objetivos principales:

* `RazorGenerate` &ndash; Genera código *.cs* archivos desde `RazorGenerate` elementos de elemento. Use la propiedad `RazorGenerateDependsOn` para especificar destinos adicionales que se pueden ejecutar antes o después de este destino.
* `RazorCompile` &ndash; Compila genera *.cs* archivos en un ensamblado de Razor. Use el `RazorCompileDependsOn` para especificar destinos adicionales que se pueden ejecutar antes o después de este destino.
* `RazorComponentGenerate` código de &ndash; genera archivos *. CS* para elementos de `RazorComponent` elemento. Use la propiedad `RazorComponentGenerateDependsOn` para especificar destinos adicionales que se pueden ejecutar antes o después de este destino.

### <a name="runtime-compilation-of-razor-views"></a>Compilación en tiempo de ejecución de vistas de Razor

* El SDK de Razor no publica de forma predeterminada los ensamblados de referencia necesarios para realizar una compilación en tiempo de ejecución. Como consecuencia, se producen errores de compilación cuando el modelo de aplicación se basa en la compilación en tiempo de ejecución; por ejemplo, la aplicación usa vistas incrustadas o cambia vistas tras haber sido publicada. Establezca `CopyRefAssembliesToPublishDirectory` en `true` para seguir publicando ensamblados de referencia.

* Para una aplicación web, asegúrese de que su aplicación tiene como destino el `Microsoft.NET.Sdk.Web` SDK.

## <a name="razor-language-version"></a>Versión de lenguaje Razor

Cuando el destino es el SDK de `Microsoft.NET.Sdk.Web`, la versión de lenguaje Razor se deduce de la versión de .NET Framework de destino de la aplicación. Para los proyectos que tienen como destino el SDK de `Microsoft.NET.Sdk.Razor` o en el caso excepcional de que la aplicación requiera una versión de lenguaje Razor diferente que el valor deducido, se puede configurar una versión estableciendo la propiedad `<RazorLangVersion>` en el archivo de proyecto de la aplicación:

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

La versión de lenguaje de Razor se integra estrechamente con la versión del Runtime para el que se compiló. El destino de una versión de idioma que no está diseñada para el tiempo de ejecución no se admite y es probable que genere errores de compilación.

## <a name="additional-resources"></a>Recursos adicionales

* [Adiciones al formato csproj para .NET Core](/dotnet/core/tools/csproj)
* [Elementos comunes de proyectos de MSBuild](/visualstudio/msbuild/common-msbuild-project-items)
