---
title: Configuración del enlazador para Blazor
author: guardrex
description: Aprenda a controlar al enlazador de lenguaje intermedio (IL) al crear una aplicación Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: host-and-deploy/razor-components/configure-linker
ms.openlocfilehash: c3c38ec2509344cc02f3895d5d0c2d35059d1d8e
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668068"
---
# <a name="configure-the-linker-for-blazor"></a><span data-ttu-id="79219-103">Configuración del enlazador para Blazor</span><span class="sxs-lookup"><span data-stu-id="79219-103">Configure the Linker for Blazor</span></span>

<span data-ttu-id="79219-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="79219-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="79219-105">Blazor realiza la vinculación de [lenguaje intermedio (IL)](/dotnet/standard/managed-code#intermediate-language--execution) durante cada compilación del modo de versión para quitar el IL innecesario de los ensamblados de salida.</span><span class="sxs-lookup"><span data-stu-id="79219-105">Blazor performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during each Release mode build to remove unnecessary IL from the output assemblies.</span></span>

<span data-ttu-id="79219-106">Puede controlar la vinculación del ensamblado con cualquiera de los enfoques siguientes:</span><span class="sxs-lookup"><span data-stu-id="79219-106">You can control assembly linking with either of the following approaches:</span></span>

* <span data-ttu-id="79219-107">Deshabilitación de la vinculación global con una propiedad de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="79219-107">Disable linking globally with an MSBuild property.</span></span>
* <span data-ttu-id="79219-108">Control de la vinculación por cada ensamblado con un archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="79219-108">Control linking on a per-assembly basis with a configuration file.</span></span>

## <a name="disable-linking-with-an-msbuild-property"></a><span data-ttu-id="79219-109">Deshabilitación de la vinculación con una propiedad de MSBuild</span><span class="sxs-lookup"><span data-stu-id="79219-109">Disable linking with an MSBuild property</span></span>

<span data-ttu-id="79219-110">La vinculación se habilita de forma predeterminada en el modo de versión cuando se crea una aplicación, lo que incluye la publicación.</span><span class="sxs-lookup"><span data-stu-id="79219-110">Linking is enabled by default in Release mode when an app is built, which includes publishing.</span></span> <span data-ttu-id="79219-111">Para deshabilitar la vinculación para todos los ensamblados, establezca la propiedad `<BlazorLinkOnBuild>` de MSBuild en `false` en el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="79219-111">To disable linking for all assemblies, set the `<BlazorLinkOnBuild>` MSBuild property to `false` in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="79219-112">Control de la vinculación con un archivo de configuración</span><span class="sxs-lookup"><span data-stu-id="79219-112">Control linking with a configuration file</span></span>

<span data-ttu-id="79219-113">La vinculación se puede controlar por cada ensamblado al proporcionar un archivo de configuración XML y especificar el archivo como un elemento MSBuild en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="79219-113">Linking can be controlled on a per-assembly basis by providing an XML configuration file and specifying the file as an MSBuild item in the project file.</span></span>

<span data-ttu-id="79219-114">A continuación, se muestra un archivo de configuración de ejemplo (*Linker.xml*):</span><span class="sxs-lookup"><span data-stu-id="79219-114">The following is an example configuration file (*Linker.xml*):</span></span>

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

<span data-ttu-id="79219-115">Para más información sobre el formato de archivo del archivo de configuración, consulte [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/linker/README.md#syntax-of-xml-descriptor) (Vinculador de IL: sintaxis del descriptor xml).</span><span class="sxs-lookup"><span data-stu-id="79219-115">To learn more about the file format for the configuration file, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/linker/README.md#syntax-of-xml-descriptor).</span></span>

<span data-ttu-id="79219-116">Especifique el archivo de configuración en el archivo de proyecto con el elemento `BlazorLinkerDescriptor`:</span><span class="sxs-lookup"><span data-stu-id="79219-116">Specify the configuration file in the project file with the `BlazorLinkerDescriptor` item:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```
