---
title: SDK de Razor de ASP.NET Core
author: Rick-Anderson
description: Obtenga información sobre cómo las páginas de Razor de ASP.NET Core facilitan la programación de escenarios centrados en páginas y hacen que resulte más productiva que con MVC.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/12/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: mvc/razor-pages/sdk
ms.openlocfilehash: 2cbebb12ccd1098e1950aa7eeb22fab4ffc689e6
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2018
---
# <a name="aspnet-core-razor-sdk"></a>SDK de Razor de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[](~/includes/2.1.md)]

El [!INCLUDE [](~/includes/2.1-SDK.md)] incluye el SDK de MSBuild `Microsoft.NET.Sdk.Razor` (SDK de Razor). El SDK de Razor:

* Normaliza la experiencia de creación, empaquetado y publicación de proyectos que contienen archivos de [Razor](xref:mvc/views/razor) relativos a proyectos basados en ASP.NET Core MVC.
* Incluye un conjunto de destinos, propiedades y elementos predefinidos que permiten personalizar la compilación de archivos de Razor.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a>Uso del SDK de Razor

En la mayoría de las aplicaciones web no es necesario hacer una referencia expresa al SDK de Razor. 

Para usar el SDK de Razor para crear bibliotecas de clases que contengan vistas de Razor o páginas de Razor:

* Use `Microsoft.NET.Sdk.Razor` en lugar de `Microsoft.NET.Sdk`:
```xml
<Project SDK="Microsoft.NET.Sdk.Razor">
  ...
</Project>
```

* Las referencias de paquete a `Microsoft.AspNetCore.Mvc` suelen ser necesarias para incluir más dependencias que son necesarias para generar y compilar páginas de Razor y vistas de Razor. Como mínimo, el proyecto debe tener agregadas referencias de paquete a:

    * `Microsoft.AspNetCore.Razor.Design` 
    * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
    
 Los paquetes anteriores están incluidos en `Microsoft.AspNetCore.Mvc`. En el siguiente marcado se muestra un archivo *.csproj* básico que usa el SDK de Razor para crear archivos de Razor para una aplicación de páginas de Razor de ASP.NET Core:
    
 [!code-xml[Main](sdk/sample/RazorSDK.csproj)]

### <a name="properties"></a>Propiedades

Las siguientes propiedades controlan el comportamiento del SDK de Razor como parte de la generación de un proyecto:

* `RazorCompileOnBuild`: si es `true`, compila y emite el ensamblado de Razor como parte de la generación del proyecto. Tiene como valor predeterminado `true`.
* `RazorCompileOnPublish`: si es `true`, compila y emite el ensamblado de Razor como parte de la publicación del proyecto. Tiene como valor predeterminado `true`.

Para configurar las entradas y la salida del SDK de Razor se usan las siguientes propiedades y elementos:

| Elementos                                         | Description                                                                   |
| ------------                                  | -------------                                                                 |
| RazorGenerate                                 | Elementos (archivos *.cshtml*) que son entradas para los destinos de generación de código. |
| RazorCompile                                  | Elementos (archivos .cs) que son entradas para los destinos de compilación de Razor. Use este ItemGroup para especificar más archivos para que se compilen en el ensamblado de Razor. |
| RazorAssemblyAttribute                        | Elementos que se usan para generar atributos de código para el ensamblado de Razor. Por ejemplo:  <br />`<RazorAssemblyAttribute ` <br />  `Include="System.Reflection.AssemblyMetadataAttribute"`<br />`  _Parameter1="BuildSource" _Parameter2="https://docs.asp.net/">` |
| RazorEmbeddedResource                         | Elementos que se agregan como recursos insertados en el ensamblado de Razor generado. |

| Property                                      | Description                                                                   |
| ------------                                  | -------------                                                                 |
| RazorTargetName                               | Nombre de archivo (sin extensión) del ensamblado generado por Razor. | 
| RazorOutputPath                               | Directorio de salida de Razor.                                      |
| RazorCompileToolset                           | Sirve para determinar el conjunto de herramientas que se va a usar para compilar el ensamblado de Razor. Los valores válidos son `Implicit` y `PrecompilationTool`. |
| EnableDefaultContentItems                     | Si es `true`, incluye ciertos tipos de archivo (como archivos *.cshtml*) como contenido en el proyecto. Si la referencia se hace a través de Microsoft.NET.Sdk.Web, también se incluyen todos los archivos de *wwwroot* y los archivos de configuración.         |
| EnableDefaultRazorGenerateItems               | Si es `true`, incluye los archivos *.cshtml* de los elementos `Content` en elementos `RazorGenerate`. |
| GenerateRazorTargetAssemblyInfo               | Si es `true`, genera un archivo *.cs* que contiene los atributos especificados por `RazorAssemblyAttribute` y este se incluye en la salida de compilación. |
| EnableDefaultRazorTargetAssemblyInfoAttributes | Si es `true`, agrega a `RazorAssemblyAttribute` un conjunto predeterminado de atributos de ensamblado. |
| CopyRazorGenerateFilesToPublishDirectory       | Si es `true`, copia los elementos de RazorGenerate (archivos *.cshtml*) en el directorio de publicación. Los archivos de Razor no suelen ser necesarios en una aplicación publicada si forman parte de la compilación en tiempo de compilación o de publicación. Tiene como valor predeterminado `false`. |
| CopyRefAssembliesToPublishDirectory            | Si es `true`, copia los elementos de referencia del ensamblado en el directorio de publicación. Los ensamblados de referencia no suelen ser necesarios en una aplicación publicada si la compilación de Razor tiene lugar en tiempo de compilación o de publicación. Establézcala en `true` si la aplicación publicada requiere compilación en tiempo de ejecución, por ejemplo, si modifica archivos cshtml en tiempo de ejecución o si usa vistas incrustadas. Tiene como valor predeterminado `false`. |
| IncludeRazorContentInPack                      | Si es `true`, todos los elementos de contenido de Razor (archivos *.cshtml*) se marcarán para incluirse en el paquete NuGet generado. Tiene como valor predeterminado `false`. |
| EmbedRazorGenerateSources | Si es `true`, agrega los elementos de RazorGenerate (*.cshtml*) como archivos incrustados al ensamblado de Razor generado. Tiene como valor predeterminado `false`. |
| UseRazorBuildServer                           | Si es `true`, usa un proceso de servidor de compilación persistente para descargar el trabajo de generación de código. El valor predeterminado es `UseSharedCompilation`. |

### <a name="targets"></a>Destinos
El SDK de Razor establece dos objetivos principales:

* `RazorGenerate`: el código genera archivos *.cs* a partir de elementos de RazorGenerate. Use la propiedad `RazorGenerateDependsOn` para especificar más destinos que puedan ejecutarse antes o después de este destino.
* `RazorCompile`: compila los archivos *.cs* generados en un ensamblado de Razor. Use `RazorCompileDependsOn` para especificar más destinos que puedan ejecutarse antes o después de este destino.