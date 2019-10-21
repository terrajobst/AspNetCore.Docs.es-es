---
title: SDK de Razor de ASP.NET Core
author: Rick-Anderson
description: Obtenga información sobre cómo las páginas de Razor de ASP.NET Core facilitan la programación de escenarios centrados en páginas y hacen que resulte más productiva que con MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/23/2019
uid: razor-pages/sdk
ms.openlocfilehash: 606d2bdca3fa4fb1c81df73ac697d2175c3ab633
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334033"
---
# <a name="aspnet-core-razor-sdk"></a>SDK de Razor de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="overview"></a>Información general

El [!INCLUDE[](~/includes/2.1-SDK.md)] incluye el SDK de MSBuild `Microsoft.NET.Sdk.Razor` (SDK de Razor). El SDK de Razor:

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

No es necesario que la mayoría de las aplicaciones web hagan referencia explícitamente al SDK de Razor.

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

* Normalmente, se requiere una referencia de paquete a `Microsoft.AspNetCore.Mvc` para recibir dependencias adicionales necesarias para compilar y compilar vistas de Razor Pages y de Razor. Como mínimo, el proyecto debe agregar referencias de paquete a:

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  El paquete de `Microsoft.AspNetCore.Razor.Design` proporciona las tareas y destinos de compilación de Razor para el proyecto.

  Los paquetes anteriores están incluidos en `Microsoft.AspNetCore.Mvc`. En el marcado siguiente se muestra un archivo de proyecto que utiliza el SDK de Razor para compilar archivos Razor para una aplicación ASP.NET Core Razor Pages:
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]
  
::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> Los paquetes de `Microsoft.AspNetCore.Razor.Design` y `Microsoft.AspNetCore.Mvc.Razor.Extensions` se incluyen en el [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app). No obstante, la referencia de paquete de `Microsoft.AspNetCore.App` sin versión proporciona un metapaquete a la aplicación que no incluye la versión más reciente de `Microsoft.AspNetCore.Razor.Design`. Los proyectos deben hacer referencia a una versión coherente de `Microsoft.AspNetCore.Razor.Design` (o `Microsoft.AspNetCore.Mvc`), de modo que se incluyan las últimas correcciones de tiempo de compilación de Razor. Para más información, consulte [este problema de GitHub](https://github.com/aspnet/Razor/issues/2553).

::: moniker-end

### <a name="properties"></a>Propiedades

Las siguientes propiedades controlan el comportamiento del SDK de Razor como parte de la generación de un proyecto:

* `RazorCompileOnBuild` &ndash; cuando `true`, compila y emite el ensamblado Razor como parte de la compilación del proyecto. Tiene como valor predeterminado `true`.
* `RazorCompileOnPublish` &ndash; cuando `true`, compila y emite el ensamblado Razor como parte de la publicación del proyecto. Tiene como valor predeterminado `true`.

Las propiedades y los elementos de la tabla siguiente se usan para configurar entradas y salidas en el SDK de Razor.

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> A partir de ASP.NET Core 3,0, las vistas o Razor Pages de MVC no se atienden de forma predeterminada si las propiedades de MSBuild `RazorCompileOnBuild` o `RazorCompileOnPublish` del archivo de proyecto están deshabilitadas. Las aplicaciones deben agregar una referencia explícita al paquete [Microsoft. AspNetCore. Mvc. Razor. RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation) si la aplicación se basa en la compilación en tiempo de ejecución para procesar archivos *. cshtml* .

::: moniker-end

| Elementos | Descripción |
| ----- | ----------- |
| `RazorGenerate` | Elementos Item (archivos *. cshtml* ) que son entradas para la generación de código. |
| `RazorComponent` | Elementos Item (archivos *. Razor* ) que son entradas para la generación de código de componentes de Razor. |
| `RazorCompile` | Elementos de elemento (archivos *. CS* ) que son entradas para los destinos de compilación de Razor. Use este `ItemGroup` para especificar archivos adicionales que se van a compilar en el ensamblado de Razor. |
| `RazorTargetAssemblyAttribute` | Elementos que se usan para generar atributos de código para el ensamblado de Razor. Por ejemplo:  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | Elementos de elemento que se agregan como recursos incrustados al ensamblado de Razor generado. |

| Propiedad. | Descripción |
| -------- | ----------- |
| `RazorTargetName` | Nombre de archivo (sin extensión) del ensamblado generado por Razor. | 
| `RazorOutputPath` | Directorio de salida de Razor. |
| `RazorCompileToolset` | Sirve para determinar el conjunto de herramientas que se va a usar para compilar el ensamblado de Razor. Valores válidos son `Implicit`, `RazorSDK` y `PrecompilationTool`. |
| [EnableDefaultContentItems](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | El valor predeterminado es `true`. Cuando `true`, incluye los archivos *Web. config*, *. JSON*y *. cshtml* como contenido en el proyecto. Cuando se hace referencia a través de `Microsoft.NET.Sdk.Web`, también se incluyen los archivos de los archivos *wwwroot* y config. |
| `EnableDefaultRazorGenerateItems` | Si es `true`, incluye los archivos *.cshtml* de los elementos `Content` en elementos `RazorGenerate`. |
| `GenerateRazorTargetAssemblyInfo` | Cuando `true`, genera un archivo *. CS* que contiene los atributos especificados por `RazorAssemblyAttribute` e incluye el archivo en la salida de compilación. |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | Si es `true`, agrega a `RazorAssemblyAttribute` un conjunto predeterminado de atributos de ensamblado. |
| `CopyRazorGenerateFilesToPublishDirectory` | Cuando `true`, copia archivos de `RazorGenerate` elementos ( *. cshtml*) en el directorio de publicación. Normalmente, los archivos de Razor no son necesarios para una aplicación publicada si participan en la compilación en tiempo de compilación o en tiempo de publicación. Tiene como valor predeterminado `false`. |
| `CopyRefAssembliesToPublishDirectory` | Si es `true`, copia los elementos de referencia del ensamblado en el directorio de publicación. Normalmente, los ensamblados de referencia no son necesarios para una aplicación publicada si la compilación de Razor se produce en tiempo de compilación o en tiempo de publicación. Establezca en `true` si la aplicación publicada requiere compilación en tiempo de ejecución. Por ejemplo, establezca el valor en `true` si la aplicación modifica los archivos *. cshtml* en tiempo de ejecución o utiliza vistas incrustadas. Tiene como valor predeterminado `false`. |
| `IncludeRazorContentInPack` | Cuando se `true`, todos los elementos de contenido de Razor (archivos *. cshtml* ) se marcan para su inclusión en el paquete de NuGet generado. Tiene como valor predeterminado `false`. |
| `EmbedRazorGenerateSources` | Si es `true`, agrega los elementos de RazorGenerate ( *.cshtml*) como archivos incrustados al ensamblado de Razor generado. Tiene como valor predeterminado `false`. |
| `UseRazorBuildServer` | Si es `true`, usa un proceso de servidor de compilación persistente para descargar el trabajo de generación de código. El valor predeterminado es `UseSharedCompilation`. |
| `GenerateMvcApplicationPartsAssemblyAttributes` | Cuando `true`, el SDK genera atributos adicionales usados por MVC en tiempo de ejecución para realizar la detección de la parte de la aplicación. |

Para más información sobre las propiedades, consulte [Propiedades de MSBuild](/visualstudio/msbuild/msbuild-properties).

### <a name="targets"></a>Destinos

El SDK de Razor establece dos objetivos principales:

* `RazorGenerate` código de &ndash; genera archivos *. CS* a partir de elementos de `RazorGenerate` elemento. Use la propiedad `RazorGenerateDependsOn` para especificar destinos adicionales que se pueden ejecutar antes o después de este destino.
* `RazorCompile` &ndash; compila los archivos *. CS* generados en un ensamblado de Razor. Use el `RazorCompileDependsOn` para especificar destinos adicionales que se pueden ejecutar antes o después de este destino.
* `RazorComponentGenerate` código de &ndash; genera archivos *. CS* para elementos de `RazorComponent` elemento. Use la propiedad `RazorComponentGenerateDependsOn` para especificar destinos adicionales que se pueden ejecutar antes o después de este destino.

### <a name="runtime-compilation-of-razor-views"></a>Compilación en tiempo de ejecución de vistas de Razor

* El SDK de Razor no publica de forma predeterminada los ensamblados de referencia necesarios para realizar una compilación en tiempo de ejecución. Como consecuencia, se producen errores de compilación cuando el modelo de aplicación se basa en la compilación en tiempo de ejecución; por ejemplo, la aplicación usa vistas incrustadas o cambia vistas tras haber sido publicada. Establezca `CopyRefAssembliesToPublishDirectory` en `true` para seguir publicando ensamblados de referencia.

* En el caso de una aplicación Web, asegúrese de que su aplicación tiene como destino el SDK de `Microsoft.NET.Sdk.Web`.

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
