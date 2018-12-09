---
title: SDK de Razor de ASP.NET Core
author: Rick-Anderson
description: Obtenga información sobre cómo las páginas de Razor de ASP.NET Core facilitan la programación de escenarios centrados en páginas y hacen que resulte más productiva que con MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/25/2018
uid: razor-pages/sdk
ms.openlocfilehash: eb4db69f71ec1e481e022192ca05b9d9d61573b9
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121445"
---
# <a name="aspnet-core-razor-sdk"></a>SDK de Razor de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

El [!INCLUDE[](~/includes/2.1-SDK.md)] incluye la `Microsoft.NET.Sdk.Razor` MSBuild SDK (SDK de Razor). El SDK de Razor:

* Normaliza la experiencia de creación, empaquetado y publicación de proyectos que contienen archivos de [Razor](xref:mvc/views/razor) relativos a proyectos basados en ASP.NET Core MVC.
* Incluye un conjunto de destinos, propiedades y elementos predefinidos que permiten personalizar la compilación de archivos de Razor.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a>Uso del SDK de Razor

La mayoría de las aplicaciones web no están necesario hacer referencia explícitamente el SDK de Razor.

Para usar el SDK de Razor para crear bibliotecas de clases que contengan vistas de Razor o páginas de Razor:

* Use `Microsoft.NET.Sdk.Razor` en lugar de `Microsoft.NET.Sdk`:

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    ...
  </Project>
  ```

* Normalmente, una referencia de paquete para `Microsoft.AspNetCore.Mvc` se requiere para recibir las dependencias adicionales que son necesarios para generar y compilar las páginas de Razor y las vistas de Razor. Como mínimo, el proyecto debe agregar referencias de paquete para:

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
    
  El `Microsoft.AspNetCore.Razor.Design` paquete proporciona las tareas de compilación de Razor y destinos para el proyecto.

  Los paquetes anteriores están incluidos en `Microsoft.AspNetCore.Mvc`. El marcado siguiente muestra un archivo de proyecto que usa el SDK de Razor para generar archivos de Razor para una aplicación de páginas de Razor de ASP.NET Core:
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> El `Microsoft.AspNetCore.Razor.Design` y `Microsoft.AspNetCore.Mvc.Razor.Extensions` paquetes se incluyen en el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app). Sin embargo, la versión de menor `Microsoft.AspNetCore.App` referencia de paquete proporciona un metapaquete a la aplicación que no incluye la versión más reciente de `Microsoft.AspNetCore.Razor.Design`. Deben hacer referencia a una versión coherente de los proyectos `Microsoft.AspNetCore.Razor.Design` (o `Microsoft.AspNetCore.Mvc`) para que se incluyen las correcciones más recientes de tiempo de compilación para Razor. Para obtener más información, consulte [este problema de GitHub](https://github.com/aspnet/Razor/issues/2553).

::: moniker-end

### <a name="properties"></a>Propiedades

Las siguientes propiedades controlan el comportamiento del SDK de Razor como parte de la generación de un proyecto:

* `RazorCompileOnBuild` &ndash; Cuando `true`, compila y se emite el ensamblado de Razor como parte de la creación del proyecto. Tiene como valor predeterminado `true`.
* `RazorCompileOnPublish` &ndash; Cuando `true`, compila y se emite el ensamblado de Razor como parte de la publicación del proyecto. Tiene como valor predeterminado `true`.

Las propiedades y elementos en la tabla siguiente se utilizan para configurar las entradas y salida para el SDK de Razor.

| Elementos | Descripción |
| ----- | ----------- |
| `RazorGenerate` | Elementos (archivos *.cshtml*) que son entradas para los destinos de generación de código. |
| `RazorCompile` | Elementos de elemento (*.cs* archivos) que son entradas para los destinos de compilación de Razor. Use este ItemGroup para especificar otros archivos que se compilen en el ensamblado de Razor. |
| `RazorTargetAssemblyAttribute` | Elementos que se usan para generar atributos de código para el ensamblado de Razor. Por ejemplo:  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | Elementos que se agregan como recursos incrustados en el ensamblado generado de Razor. |

| Property | Descripción |
| -------- | ----------- |
| `RazorTargetName` | Nombre de archivo (sin extensión) del ensamblado generado por Razor. | 
| `RazorOutputPath` | Directorio de salida de Razor. |
| `RazorCompileToolset` | Sirve para determinar el conjunto de herramientas que se va a usar para compilar el ensamblado de Razor. Valores válidos son `Implicit`, `RazorSDK` y `PrecompilationTool`. |
| `EnableDefaultContentItems` | Si es `true`, incluye ciertos tipos de archivo (como archivos *.cshtml*) como contenido en el proyecto. Cuando se hace referencia a través de `Microsoft.NET.Sdk.Web`, los archivos en *wwwroot* y también se incluyen los archivos de configuración. |
| `EnableDefaultRazorGenerateItems` | Si es `true`, incluye los archivos *.cshtml* de los elementos `Content` en elementos `RazorGenerate`. |
| `GenerateRazorTargetAssemblyInfo` | Cuando `true`, genera un *.cs* archivo que contiene los atributos especificados por `RazorAssemblyAttribute` e incluye el archivo en la salida de compilación. |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | Si es `true`, agrega a `RazorAssemblyAttribute` un conjunto predeterminado de atributos de ensamblado. |
| `CopyRazorGenerateFilesToPublishDirectory` | Cuando `true`, copias `RazorGenerate` elementos (*.cshtml*) archivos en el directorio de publicación. Normalmente, los archivos de Razor no son necesarios para una aplicación publicada si participa en la compilación en tiempo de compilación o tiempo de publicación. Tiene como valor predeterminado `false`. |
| `CopyRefAssembliesToPublishDirectory` | Si es `true`, copia los elementos de referencia del ensamblado en el directorio de publicación. Normalmente, los ensamblados de referencia no son necesarios para una aplicación publicada si la compilación de Razor se produce en tiempo de compilación o tiempo de publicación. Establecido en `true` si la aplicación publicada requiere compilación en tiempo de ejecución. Por ejemplo, establezca el valor en `true` si modifica la aplicación *.cshtml* archivos en tiempo de ejecución o usa vistas incrustadas. Tiene como valor predeterminado `false`. |
| `IncludeRazorContentInPack` | Cuando `true`, todos los elementos de contenido de Razor (*.cshtml* archivos) se marcan para su inclusión en el paquete NuGet generado. Tiene como valor predeterminado `false`. |
| `EmbedRazorGenerateSources` | Si es `true`, agrega los elementos de RazorGenerate (*.cshtml*) como archivos incrustados al ensamblado de Razor generado. Tiene como valor predeterminado `false`. |
| `UseRazorBuildServer` | Si es `true`, usa un proceso de servidor de compilación persistente para descargar el trabajo de generación de código. El valor predeterminado es `UseSharedCompilation`. |

### <a name="targets"></a>Destinos

El SDK de Razor establece dos objetivos principales:

* `RazorGenerate` &ndash; Genera código *.cs* archivos desde `RazorGenerate` elementos de elemento. Use la propiedad `RazorGenerateDependsOn` para especificar más destinos que puedan ejecutarse antes o después de este destino.
* `RazorCompile` &ndash; Compila genera *.cs* archivos en un ensamblado de Razor. Use `RazorCompileDependsOn` para especificar más destinos que puedan ejecutarse antes o después de este destino.

### <a name="runtime-compilation-of-razor-views"></a>Compilación en tiempo de ejecución de vistas de Razor

* El SDK de Razor no publica de forma predeterminada los ensamblados de referencia necesarios para realizar una compilación en tiempo de ejecución. Como consecuencia, se producen errores de compilación cuando el modelo de aplicación se basa en la compilación en tiempo de ejecución; por ejemplo, la aplicación usa vistas incrustadas o cambia vistas tras haber sido publicada. Establezca `CopyRefAssembliesToPublishDirectory` en `true` para seguir publicando ensamblados de referencia.

* Para una aplicación web, asegúrese de que su aplicación tiene como destino el `Microsoft.NET.Sdk.Web` SDK.
