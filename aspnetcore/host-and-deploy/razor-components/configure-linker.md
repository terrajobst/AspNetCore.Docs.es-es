---
title: Configuración del enlazador para Blazor
author: guardrex
description: Aprenda a controlar al enlazador de lenguaje intermedio (IL) al crear una aplicación Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/11/2019
uid: host-and-deploy/razor-components/configure-linker
ms.openlocfilehash: c73c972e22a51842c5d8dd209b7e1ed987f9090d
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58207936"
---
# <a name="configure-the-linker-for-blazor"></a><span data-ttu-id="65332-103">Configuración del enlazador para Blazor</span><span class="sxs-lookup"><span data-stu-id="65332-103">Configure the Linker for Blazor</span></span>

<span data-ttu-id="65332-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="65332-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="65332-105">Blazor realiza la vinculación de [lenguaje intermedio (IL)](/dotnet/standard/managed-code#intermediate-language--execution) durante cada compilación del modo de versión para quitar el IL innecesario de los ensamblados de salida de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="65332-105">Blazor performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during each Release mode build to remove unnecessary IL from the app's output assemblies.</span></span>

<span data-ttu-id="65332-106">Controle la vinculación del ensamblado con cualquiera de los enfoques siguientes:</span><span class="sxs-lookup"><span data-stu-id="65332-106">Control assembly linking using either of the following approaches:</span></span>

* <span data-ttu-id="65332-107">Deshabilitación de la vinculación global con una [propiedad de MSBuild](#disable-linking-with-a-msbuild-property).</span><span class="sxs-lookup"><span data-stu-id="65332-107">Disable linking globally with a [MSBuild property](#disable-linking-with-a-msbuild-property).</span></span>
* <span data-ttu-id="65332-108">Control de la vinculación por cada ensamblado con un [archivo de configuración](#control-linking-with-a-configuration-file).</span><span class="sxs-lookup"><span data-stu-id="65332-108">Control linking on a per-assembly basis with a [configuration file](#control-linking-with-a-configuration-file).</span></span>

## <a name="disable-linking-with-a-msbuild-property"></a><span data-ttu-id="65332-109">Deshabilitación de la vinculación con una propiedad de MSBuild</span><span class="sxs-lookup"><span data-stu-id="65332-109">Disable linking with a MSBuild property</span></span>

<span data-ttu-id="65332-110">La vinculación se habilita de forma predeterminada en el modo de versión cuando se crea una aplicación, lo que incluye la publicación.</span><span class="sxs-lookup"><span data-stu-id="65332-110">Linking is enabled by default in Release mode when an app is built, which includes publishing.</span></span> <span data-ttu-id="65332-111">Para deshabilitar la vinculación para todos los ensamblados, establezca la propiedad `<BlazorLinkOnBuild>` de MSBuild en `false` en el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="65332-111">To disable linking for all assemblies, set the `<BlazorLinkOnBuild>` MSBuild property to `false` in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="65332-112">Control de la vinculación con un archivo de configuración</span><span class="sxs-lookup"><span data-stu-id="65332-112">Control linking with a configuration file</span></span>

<span data-ttu-id="65332-113">Control de la vinculación por cada ensamblado al proporcionar un archivo de configuración XML y especificar el archivo como un elemento MSBuild en el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="65332-113">Control linking on a per-assembly basis by providing an XML configuration file and specifying the file as a MSBuild item in the project file:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

<span data-ttu-id="65332-114">*Linker.xml*:</span><span class="sxs-lookup"><span data-stu-id="65332-114">*Linker.xml*:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--
  This file specifies which parts of the BCL or Blazor packages must not be
  stripped by the IL Linker even if they aren't referenced by user code.
-->
<linker>
  <assembly fullname="mscorlib">
    <!--
      Preserve the methods in WasmRuntime because its methods are called by 
      JavaScript client-side code to implement timers.
      Fixes: https://github.com/aspnet/Blazor/issues/239
    -->
    <type fullname="System.Threading.WasmRuntime" />
  </assembly>
  <assembly fullname="System.Core">
    <!--
      System.Linq.Expressions* is required by Json.NET and any 
      expression.Compile caller. The assembly isn't stripped.
    -->
    <type fullname="System.Linq.Expressions*" />
  </assembly>
  <!--
    In this example, the app's entry point assembly is listed. The assembly
    isn't stripped by the IL Linker.
  -->
  <assembly fullname="MyCoolBlazorApp" />
</linker>
```

<span data-ttu-id="65332-115">Para obtener más información, consulte [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor) (Vinculador de IL: sintaxis del descriptor xml).</span><span class="sxs-lookup"><span data-stu-id="65332-115">For more information, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span></span>
