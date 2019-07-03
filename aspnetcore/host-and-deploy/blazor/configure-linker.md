---
title: Configuración del enlazador para ASP.NET Core Blazor
author: guardrex
description: Aprenda a controlar al enlazador de lenguaje intermedio (IL) al crear una aplicación Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: bdddae16885f45df2c10e4d98b1c33eb11dfdf24
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "67153213"
---
# <a name="configure-the-linker-for-aspnet-core-blazor"></a>Configuración del enlazador para ASP.NET Core Blazor

Por [Luke Latham](https://github.com/guardrex)

Blazor realiza la vinculación de [lenguaje intermedio (IL)](/dotnet/standard/managed-code#intermediate-language--execution) durante una compilación de versión para quitar el IL innecesario de los ensamblados de salida de la aplicación.

Controle la vinculación del ensamblado con cualquiera de los enfoques siguientes:

* Deshabilitación de la vinculación global con una [propiedad de MSBuild](#disable-linking-with-a-msbuild-property).
* Control de la vinculación por cada ensamblado con un [archivo de configuración](#control-linking-with-a-configuration-file).

## <a name="disable-linking-with-a-msbuild-property"></a>Deshabilitación de la vinculación con una propiedad de MSBuild

La vinculación se habilita de forma predeterminada en el modo de versión cuando se crea una aplicación, lo que incluye la publicación. Para deshabilitar la vinculación para todos los ensamblados, establezca la propiedad `<BlazorLinkOnBuild>` de MSBuild en `false` en el archivo de proyecto:

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a>Control de la vinculación con un archivo de configuración

Control de la vinculación por cada ensamblado al proporcionar un archivo de configuración XML y especificar el archivo como un elemento MSBuild en el archivo de proyecto:

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

*Linker.xml*:

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

Para obtener más información, consulte [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor) (Vinculador de IL: sintaxis del descriptor xml).
