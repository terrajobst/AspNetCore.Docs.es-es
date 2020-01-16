---
title: Configuración del enlazador de ASP.NET Core Blazor
author: guardrex
description: Aprenda a controlar al enlazador de lenguaje intermedio (IL) al crear una aplicación Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- Blazor
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: cdf506f0c0fa720df64e59342d352ef41271d24b
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2020
ms.locfileid: "75866051"
---
# <a name="configure-the-linker-for-aspnet-core-opno-locblazor"></a><span data-ttu-id="35a41-103">Configuración del enlazador de ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="35a41-103">Configure the Linker for ASP.NET Core Blazor</span></span>

<span data-ttu-id="35a41-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="35a41-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="35a41-105"> realiza la vinculación de [lenguaje intermedio (IL)](/dotnet/standard/managed-code#intermediate-language--execution) durante una compilación para quitar el IL innecesario de los ensamblados de salida de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="35a41-105"> performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during a build to remove unnecessary IL from the app's output assemblies.</span></span>

<span data-ttu-id="35a41-106">Controle la vinculación del ensamblado con cualquiera de los enfoques siguientes:</span><span class="sxs-lookup"><span data-stu-id="35a41-106">Control assembly linking using either of the following approaches:</span></span>

* <span data-ttu-id="35a41-107">Deshabilitación de la vinculación global con una [propiedad de MSBuild](#disable-linking-with-a-msbuild-property).</span><span class="sxs-lookup"><span data-stu-id="35a41-107">Disable linking globally with a [MSBuild property](#disable-linking-with-a-msbuild-property).</span></span>
* <span data-ttu-id="35a41-108">Control de la vinculación por cada ensamblado con un [archivo de configuración](#control-linking-with-a-configuration-file).</span><span class="sxs-lookup"><span data-stu-id="35a41-108">Control linking on a per-assembly basis with a [configuration file](#control-linking-with-a-configuration-file).</span></span>

## <a name="disable-linking-with-a-msbuild-property"></a><span data-ttu-id="35a41-109">Deshabilitación de la vinculación con una propiedad de MSBuild</span><span class="sxs-lookup"><span data-stu-id="35a41-109">Disable linking with a MSBuild property</span></span>

<span data-ttu-id="35a41-110">La vinculación se habilita de forma predeterminada al compilar una aplicación, lo que incluye la publicación.</span><span class="sxs-lookup"><span data-stu-id="35a41-110">Linking is enabled by default when an app is built, which includes publishing.</span></span> <span data-ttu-id="35a41-111">Para deshabilitar la vinculación para todos los ensamblados, establezca la propiedad `BlazorLinkOnBuild` de MSBuild en `false` en el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="35a41-111">To disable linking for all assemblies, set the `BlazorLinkOnBuild` MSBuild property to `false` in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="35a41-112">Control de la vinculación con un archivo de configuración</span><span class="sxs-lookup"><span data-stu-id="35a41-112">Control linking with a configuration file</span></span>

<span data-ttu-id="35a41-113">Control de la vinculación por cada ensamblado al proporcionar un archivo de configuración XML y especificar el archivo como un elemento MSBuild en el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="35a41-113">Control linking on a per-assembly basis by providing an XML configuration file and specifying the file as a MSBuild item in the project file:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

<span data-ttu-id="35a41-114">*Linker.xml*:</span><span class="sxs-lookup"><span data-stu-id="35a41-114">*Linker.xml*:</span></span>

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
      Fixes: https://github.com/dotnet/blazor/issues/239
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

<span data-ttu-id="35a41-115">Para obtener más información, consulte [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor) (Vinculador de IL: sintaxis del descriptor xml).</span><span class="sxs-lookup"><span data-stu-id="35a41-115">For more information, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span></span>

### <a name="configure-the-linker-for-internationalization"></a><span data-ttu-id="35a41-116">Configuración del enlazador para la internacionalización</span><span class="sxs-lookup"><span data-stu-id="35a41-116">Configure the linker for internationalization</span></span>

<span data-ttu-id="35a41-117">De forma predeterminada, la configuración del enlazador de Blazor para aplicaciones WebAssembly de Blazor quita información de internacionalización, excepto para las configuraciones regionales solicitadas de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="35a41-117">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="35a41-118">Al quitar estos ensamblados se minimiza el tamaño de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="35a41-118">Removing these assemblies minimizes the app's size.</span></span>

<span data-ttu-id="35a41-119">Para controlar qué ensamblados de I18N se conservan, establezca la propiedad `<MonoLinkerI18NAssemblies>` de MSBuild en el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="35a41-119">To control which I18N assemblies are retained, set the `<MonoLinkerI18NAssemblies>` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <MonoLinkerI18NAssemblies>{all|none|REGION1,REGION2,...}</MonoLinkerI18NAssemblies>
</PropertyGroup>
```

| <span data-ttu-id="35a41-120">Valor de región</span><span class="sxs-lookup"><span data-stu-id="35a41-120">Region Value</span></span>     | <span data-ttu-id="35a41-121">Ensamblado de región de Mono</span><span class="sxs-lookup"><span data-stu-id="35a41-121">Mono region assembly</span></span>    |
| ---------------- | ----------------------- |
| `all`            | <span data-ttu-id="35a41-122">Todos los ensamblados incluidos</span><span class="sxs-lookup"><span data-stu-id="35a41-122">All assemblies included</span></span> |
| `cjk`            | <span data-ttu-id="35a41-123">*I18N.CJK.dll*</span><span class="sxs-lookup"><span data-stu-id="35a41-123">*I18N.CJK.dll*</span></span>          |
| `mideast`        | <span data-ttu-id="35a41-124">*I18N.MidEast.dll*</span><span class="sxs-lookup"><span data-stu-id="35a41-124">*I18N.MidEast.dll*</span></span>      |
| <span data-ttu-id="35a41-125">`none` (valor predeterminado)</span><span class="sxs-lookup"><span data-stu-id="35a41-125">`none` (default)</span></span> | <span data-ttu-id="35a41-126">None</span><span class="sxs-lookup"><span data-stu-id="35a41-126">None</span></span>                    |
| `other`          | <span data-ttu-id="35a41-127">*I18N.Other.dll*</span><span class="sxs-lookup"><span data-stu-id="35a41-127">*I18N.Other.dll*</span></span>        |
| `rare`           | <span data-ttu-id="35a41-128">*I18N.Rare.dll*</span><span class="sxs-lookup"><span data-stu-id="35a41-128">*I18N.Rare.dll*</span></span>         |
| `west`           | <span data-ttu-id="35a41-129">*I18N.West.dll*</span><span class="sxs-lookup"><span data-stu-id="35a41-129">*I18N.West.dll*</span></span>         |

<span data-ttu-id="35a41-130">Use una coma para separar varios valores (por ejemplo, `mideast,west`).</span><span class="sxs-lookup"><span data-stu-id="35a41-130">Use a comma to separate multiple values (for example, `mideast,west`).</span></span>

<span data-ttu-id="35a41-131">Para más información, vea [I18N: biblioteca del marco de internacionalización Pnetlib (repositorio de GitHub mono/mono)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span><span class="sxs-lookup"><span data-stu-id="35a41-131">For more information, see [I18N: Pnetlib Internationalization Framework Libary (mono/mono GitHub repository)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span></span>
