---
title: Compilación y precompilación de archivos de Razor en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre las ventajas de precompilar archivos Razor y cómo lograr la precompilación de estos archivos en una aplicación ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: 2720708f8e58fdc55b82bfb56665005170e79934
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889761"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>Compilación de archivos de Razor en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-1.1"

Un archivo de Razor se compila en tiempo de ejecución, cuando se invoca la vista MVC asociada. No se admite la publicación de archivos de Razor en tiempo de compilación. Opcionalmente, los archivos de Razor se pueden compilar en el momento de la publicación e implementar con la aplicación, mediante la herramienta de precompilación.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Un archivo de Razor se compila en tiempo de ejecución, cuando se invoca la vista MVC o la página de Razor asociada. No se admite la publicación de archivos de Razor en tiempo de compilación. Opcionalmente, los archivos de Razor se pueden compilar en el momento de la publicación e implementar con la aplicación, mediante la herramienta de precompilación.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Un archivo de Razor se compila en tiempo de ejecución, cuando se invoca la vista MVC o la página de Razor asociada. Los archivos de Razor se compilan en tiempo de compilación y publicación mediante el [SDK de Razor](xref:razor-pages/sdk).

::: moniker-end

## <a name="precompilation-considerations"></a>Consideraciones de precompilación

Los siguientes son los efectos secundarios de la precompilación de los archivos de Razor:

* Un paquete publicado más pequeño.
* Un menor tiempo de inicio.
* Los archivos de Razor no se pueden editar; el contenido asociado no está presente en el paquete publicado.

## <a name="deploy-precompiled-files"></a>Implementar archivos precompilados

::: moniker range=">= aspnetcore-2.1"

La compilación de los archivos de Razor en tiempo de publicación y compilación está habilitada de manera predeterminada en el SDK de Razor. La edición de los archivos de Razor después de que se actualicen se admite en tiempo de compilación. De forma predeterminada, solo se implementa con la aplicación el archivo *Views.dll* compilado y ningún archivo *.cshtml*.

> [!IMPORTANT]
> La herramienta de precompilación se eliminará en ASP.NET Core 3.0. Se recomienda migrar al [SDK de Razor](xref:razor-pages/sdk).
>
> El SDK de Razor solo es efectivo cuando no hay propiedades específicas de la precompilación establecidas en el archivo de proyecto. Por ejemplo, establecer la propiedad `MvcRazorCompileOnPublish` del archivo *.csproj* en `true` deshabilita el SDK de Razor.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Si el proyecto tiene como destino .NET Framework, instale el paquete NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

Si el proyecto tiene como destino .NET Core, no es necesario hacer cambios.

Las plantillas de proyecto de ASP.NET Core 2.x establecen implícitamente la propiedad `MvcRazorCompileOnPublish` en `true` de forma predeterminada. Por tanto, este elemento se puede quitar de manera segura del archivo *.csproj*.

> [!IMPORTANT]
> La herramienta de precompilación se eliminará en ASP.NET Core 3.0. Se recomienda migrar al [SDK de Razor](xref:razor-pages/sdk).
>
> La precompilación de los archivos de Razor no está disponible cuando se realiza una [implementación independiente (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) en ASP.NET Core 2.0.

::: moniker-end

::: moniker range="= aspnetcore-1.1"

Establezca la propiedad `MvcRazorCompileOnPublish` en `true` e instale el paquete NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/). El siguiente ejemplo de *.csproj* resalta estas opciones:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

Prepare la aplicación para una [implementación independiente de la plataforma](/dotnet/core/deploying/#framework-dependent-deployments-fdd) con el [comando de publicación de la CLI de .NET Core](/dotnet/core/tools/dotnet-publish). Por ejemplo, ejecute el siguiente comando en la raíz del proyecto:

```console
dotnet publish -c Release
```

Se crea un archivo *<Nombre_proyecto>.PrecompiledViews.dll*, que contiene los archivos de Razor compilados, cuando la precompilación se realiza correctamente. Por ejemplo, en la captura de pantalla siguiente se muestra el contenido de *Index.cshtml* en *WebApplication1.PrecompiledViews.dll*:

![Vistas de Razor dentro de DLL](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="recompile-razor-files-on-change"></a>Recompilación de archivos de Razor al cambiar

<xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> `AllowRecompilingViewsOnFileChange` obtiene o establece un valor que determina si los archivos de Razor (vistas y Razor Pages) se vuelven a compilar y actualizar si cambian los archivos en el disco.

Cuando se establece en `true`, [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) observa los cambios realizados en los archivos de Razor en las instancias de <xref:Microsoft.Extensions.FileProviders.IFileProvider> configuradas.

El valor predeterminado es `true` para:

* Aplicaciones ASP.NET Core 2.1 o versiones anteriores.
* Aplicaciones ASP.NET Core 2.2 o versiones posteriores en el entorno de desarrollo.

`AllowRecompilingViewsOnFileChange` se asocia con un modificador de compatibilidad y puede proporcionar un comportamiento diferente según la versión de compatibilidad configurada para la aplicación. La configuración de la aplicación estableciendo `AllowRecompilingViewsOnFileChange` tiene prioridad sobre el valor implícito en la versión de compatibilidad de la aplicación.

Si se establece la versión de compatibilidad de la aplicación en <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> o versiones anteriores, `AllowRecompilingViewsOnFileChange` se establece en `true`, a menos que se haya configurado explícitamente.

Si se establece la versión de compatibilidad de la aplicación en `CompatibilityVersion.Version_2_2` o versiones posteriores, `AllowRecompilingViewsOnFileChange` se establece en `false`, a menos que el entorno sea el de desarrollo o el valor se configure explícitamente.

Para obtener instrucciones y ejemplos de configuración de la versión de compatibilidad de la aplicación, consulte <xref:mvc/compatibility-version>.

## <a name="additional-resources"></a>Recursos adicionales

::: moniker range="= aspnetcore-1.1"

* <xref:mvc/views/overview>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end
