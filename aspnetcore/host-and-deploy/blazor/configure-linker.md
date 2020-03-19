---
title: Configuración del enlazador de ASP.NET Core Blazor
author: guardrex
description: Aprenda a controlar al enlazador de lenguaje intermedio (IL) al crear una aplicación Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/10/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: b08ec26fb8d139223c57774600bc3cb19a56ac49
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083288"
---
# <a name="configure-the-linker-for-aspnet-core-blazor"></a><span data-ttu-id="ef1b0-103">Configuración del enlazador para ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="ef1b0-103">Configure the Linker for ASP.NET Core Blazor</span></span>

<span data-ttu-id="ef1b0-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ef1b0-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="ef1b0-105">WebAssembly de Blazor realiza la vinculación de [lenguaje intermedio (IL)](/dotnet/standard/managed-code#intermediate-language--execution) durante una compilación para reducir el IL innecesario de los ensamblados de salida de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ef1b0-105">Blazor WebAssembly performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during a build to trim unnecessary IL from the app's output assemblies.</span></span> <span data-ttu-id="ef1b0-106">El enlazador está deshabilitado durante la compilación en la configuración Debug.</span><span class="sxs-lookup"><span data-stu-id="ef1b0-106">The linker is disabled when building in Debug configuration.</span></span> <span data-ttu-id="ef1b0-107">Las aplicaciones deben compilarse en la configuración Release para habilitar el enlazador.</span><span class="sxs-lookup"><span data-stu-id="ef1b0-107">Apps must build in Release configuration to enable the linker.</span></span> <span data-ttu-id="ef1b0-108">Se recomienda compilar en Release cuando se implementen las aplicaciones WebAssembly de Blazor.</span><span class="sxs-lookup"><span data-stu-id="ef1b0-108">We recommend building in Release when deploying your Blazor WebAssembly apps.</span></span> 

<span data-ttu-id="ef1b0-109">La vinculación de una aplicación optimiza el tamaño, pero puede tener efectos perjudiciales.</span><span class="sxs-lookup"><span data-stu-id="ef1b0-109">Linking an app optimizes for size but may have detrimental effects.</span></span> <span data-ttu-id="ef1b0-110">Las aplicaciones que usan la reflexión o características dinámicas relacionadas pueden verse interrumpidas cuando se reduzcan porque el enlazador no conoce este comportamiento dinámico y no puede determinar, en general, qué tipos son necesarios para la reflexión en el entorno de ejecución.</span><span class="sxs-lookup"><span data-stu-id="ef1b0-110">Apps that use reflection or related dynamic features may break when trimmed because the linker doesn't know about this dynamic behavior and can't determine in general which types are required for reflection at runtime.</span></span> <span data-ttu-id="ef1b0-111">Para reducir estas aplicaciones, se debe informar al vinculador de los tipos necesarios para la reflexión en el código y en los paquetes o marcos de trabajo de los que depende la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ef1b0-111">To trim such apps, the linker must be informed about any types required by reflection in the code and in packages or frameworks that the app depends on.</span></span> 

<span data-ttu-id="ef1b0-112">Para asegurarse de que la aplicación reducida funciona correctamente una vez implementada, es importante probar las compilaciones de Release de la aplicación con frecuencia durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="ef1b0-112">To ensure the trimmed app works correctly once deployed, it's important to test Release builds of the app frequently while developing.</span></span>

<span data-ttu-id="ef1b0-113">La vinculación de las aplicaciones de Blazor se puede configurar con estas características de MSBuild:</span><span class="sxs-lookup"><span data-stu-id="ef1b0-113">Linking for Blazor apps can be configured using these MSBuild features:</span></span>

* <span data-ttu-id="ef1b0-114">Configuración de la vinculación global con una [propiedad de MSBuild](#control-linking-with-an-msbuild-property).</span><span class="sxs-lookup"><span data-stu-id="ef1b0-114">Configure linking globally with a [MSBuild property](#control-linking-with-an-msbuild-property).</span></span>
* <span data-ttu-id="ef1b0-115">Control de la vinculación por cada ensamblado con un [archivo de configuración](#control-linking-with-a-configuration-file).</span><span class="sxs-lookup"><span data-stu-id="ef1b0-115">Control linking on a per-assembly basis with a [configuration file](#control-linking-with-a-configuration-file).</span></span>

## <a name="control-linking-with-an-msbuild-property"></a><span data-ttu-id="ef1b0-116">Control de la vinculación con una propiedad de MSBuild</span><span class="sxs-lookup"><span data-stu-id="ef1b0-116">Control linking with an MSBuild property</span></span>

<span data-ttu-id="ef1b0-117">La vinculación se habilita cuando se crea una aplicación en la configuración `Release`.</span><span class="sxs-lookup"><span data-stu-id="ef1b0-117">Linking is enabled when an app is built in `Release` configuation.</span></span> <span data-ttu-id="ef1b0-118">Para cambiar esto, configure la propiedad `BlazorWebAssemblyEnableLinking` de MSBuild en el archivo del proyecto:</span><span class="sxs-lookup"><span data-stu-id="ef1b0-118">To change this, configure the `BlazorWebAssemblyEnableLinking` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorWebAssemblyEnableLinking>false</BlazorWebAssemblyEnableLinking>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="ef1b0-119">Control de la vinculación con un archivo de configuración</span><span class="sxs-lookup"><span data-stu-id="ef1b0-119">Control linking with a configuration file</span></span>

<span data-ttu-id="ef1b0-120">Control de la vinculación por cada ensamblado al proporcionar un archivo de configuración XML y especificar el archivo como un elemento MSBuild en el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="ef1b0-120">Control linking on a per-assembly basis by providing an XML configuration file and specifying the file as a MSBuild item in the project file:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

<span data-ttu-id="ef1b0-121">*Linker.xml*:</span><span class="sxs-lookup"><span data-stu-id="ef1b0-121">*Linker.xml*:</span></span>

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

<span data-ttu-id="ef1b0-122">Para obtener más información, consulte [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor) (Vinculador de IL: sintaxis del descriptor xml).</span><span class="sxs-lookup"><span data-stu-id="ef1b0-122">For more information, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span></span>

### <a name="configure-the-linker-for-internationalization"></a><span data-ttu-id="ef1b0-123">Configuración del enlazador para la internacionalización</span><span class="sxs-lookup"><span data-stu-id="ef1b0-123">Configure the linker for internationalization</span></span>

<span data-ttu-id="ef1b0-124">De forma predeterminada, la configuración del enlazador de Blazor para aplicaciones Blazor WebAssembly quita información de internacionalización, excepto para las configuraciones regionales solicitadas de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="ef1b0-124">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="ef1b0-125">Al quitar estos ensamblados se minimiza el tamaño de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ef1b0-125">Removing these assemblies minimizes the app's size.</span></span>

<span data-ttu-id="ef1b0-126">Para controlar qué ensamblados de I18N se conservan, establezca la propiedad `<MonoLinkerI18NAssemblies>` de MSBuild en el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="ef1b0-126">To control which I18N assemblies are retained, set the `<MonoLinkerI18NAssemblies>` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <MonoLinkerI18NAssemblies>{all|none|REGION1,REGION2,...}</MonoLinkerI18NAssemblies>
</PropertyGroup>
```

| <span data-ttu-id="ef1b0-127">Valor de región</span><span class="sxs-lookup"><span data-stu-id="ef1b0-127">Region Value</span></span>     | <span data-ttu-id="ef1b0-128">Ensamblado de región de Mono</span><span class="sxs-lookup"><span data-stu-id="ef1b0-128">Mono region assembly</span></span>    |
| ---------------- | ----------------------- |
| `all`            | <span data-ttu-id="ef1b0-129">Todos los ensamblados incluidos</span><span class="sxs-lookup"><span data-stu-id="ef1b0-129">All assemblies included</span></span> |
| `cjk`            | <span data-ttu-id="ef1b0-130">*I18N.CJK.dll*</span><span class="sxs-lookup"><span data-stu-id="ef1b0-130">*I18N.CJK.dll*</span></span>          |
| `mideast`        | <span data-ttu-id="ef1b0-131">*I18N.MidEast.dll*</span><span class="sxs-lookup"><span data-stu-id="ef1b0-131">*I18N.MidEast.dll*</span></span>      |
| <span data-ttu-id="ef1b0-132">`none` (valor predeterminado)</span><span class="sxs-lookup"><span data-stu-id="ef1b0-132">`none` (default)</span></span> | <span data-ttu-id="ef1b0-133">None</span><span class="sxs-lookup"><span data-stu-id="ef1b0-133">None</span></span>                    |
| `other`          | <span data-ttu-id="ef1b0-134">*I18N.Other.dll*</span><span class="sxs-lookup"><span data-stu-id="ef1b0-134">*I18N.Other.dll*</span></span>        |
| `rare`           | <span data-ttu-id="ef1b0-135">*I18N.Rare.dll*</span><span class="sxs-lookup"><span data-stu-id="ef1b0-135">*I18N.Rare.dll*</span></span>         |
| `west`           | <span data-ttu-id="ef1b0-136">*I18N.West.dll*</span><span class="sxs-lookup"><span data-stu-id="ef1b0-136">*I18N.West.dll*</span></span>         |

<span data-ttu-id="ef1b0-137">Use una coma para separar varios valores (por ejemplo, `mideast,west`).</span><span class="sxs-lookup"><span data-stu-id="ef1b0-137">Use a comma to separate multiple values (for example, `mideast,west`).</span></span>

<span data-ttu-id="ef1b0-138">Para más información, vea [I18N: biblioteca del marco de internacionalización Pnetlib (repositorio de GitHub mono/mono)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span><span class="sxs-lookup"><span data-stu-id="ef1b0-138">For more information, see [I18N: Pnetlib Internationalization Framework Library (mono/mono GitHub repository)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span></span>
