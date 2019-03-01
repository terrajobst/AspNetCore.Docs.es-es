---
title: Compilación de archivos de Razor en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo se compilan los archivos de Razor en una aplicación ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: 0b6173a7860f5f1d9d11219fbf3f57f76d703031
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899273"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>Compilación de archivos de Razor en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-1.1"

Un archivo de Razor se compila en tiempo de ejecución, cuando se invoca la vista MVC asociada. No se admite la publicación de archivos de Razor en tiempo de compilación. Opcionalmente, los archivos de Razor se pueden compilar en el momento de la publicación e implementar con la aplicación, mediante la herramienta de precompilación.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Un archivo de Razor se compila en tiempo de ejecución, cuando se invoca la vista MVC o la página de Razor asociada. No se admite la publicación de archivos de Razor en tiempo de compilación. Opcionalmente, los archivos de Razor se pueden compilar en el momento de la publicación e implementar con la aplicación, mediante la herramienta de precompilación.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Un archivo de Razor se compila en tiempo de ejecución, cuando se invoca la vista MVC o la página de Razor asociada. Los archivos de Razor se compilan en tiempo de compilación y publicación mediante el [SDK de Razor](xref:razor-pages/sdk).

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Los archivos de Razor se compilan en tiempo de compilación y publicación mediante el [SDK de Razor](xref:razor-pages/sdk). La compilación en tiempo de ejecución se puede habilitar opcionalmente mediante la configuración de la aplicación

::: moniker-end

## <a name="razor-compilation"></a>Compilación de Razor

::: moniker range=">= aspnetcore-3.0"
La compilación de los archivos de Razor en tiempo de publicación y compilación está habilitada de manera predeterminada en el SDK de Razor. Cuando se habilita la compilación en tiempo de ejecución, complementará a la de tiempo de compilación y permitirá que se actualicen los archivos de Razor si se modifican.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

La compilación de los archivos de Razor en tiempo de publicación y compilación está habilitada de manera predeterminada en el SDK de Razor. La edición de los archivos de Razor después de que se actualicen se admite en tiempo de compilación. De forma predeterminada, solo se implementan con la aplicación los archivos *Views.dll* y los que no son *.cshtml*, o las referencias de los ensamblados necesarios para compilar los archivos de Razor.

> [!IMPORTANT]
> La herramienta de precompilación está en desuso y se eliminará en ASP.NET Core 3.0. Se recomienda migrar al [SDK de Razor](xref:razor-pages/sdk).
>
> El SDK de Razor solo es efectivo cuando no hay propiedades específicas de la precompilación establecidas en el archivo de proyecto. Por ejemplo, establecer la propiedad `MvcRazorCompileOnPublish` del archivo *.csproj* en `true` deshabilita el SDK de Razor.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Si el proyecto tiene como destino .NET Framework, instale el paquete NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

Si el proyecto tiene como destino .NET Core, no es necesario hacer cambios.

Las plantillas de proyecto de ASP.NET Core 2.x establecen implícitamente la propiedad `MvcRazorCompileOnPublish` en `true` de forma predeterminada. Por tanto, este elemento se puede quitar de manera segura del archivo *.csproj*.

> [!IMPORTANT]
> La herramienta de precompilación está en desuso y se eliminará en ASP.NET Core 3.0. Se recomienda migrar al [SDK de Razor](xref:razor-pages/sdk).
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

## <a name="runtime-compilation"></a>Compilación en tiempo de ejecución

::: moniker range="= aspnetcore-2.1"

La compilación en tiempo de compilación se complementa con la compilación en tiempo de ejecución de archivos de Razor. ASP.NET Core MVC volverá a compilar los archivos de Razor cuando cambie el contenido de un archivo *.cshtml*.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

La compilación en tiempo de compilación se complementa con la compilación en tiempo de ejecución de archivos de Razor. <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> obtiene o establece un valor que determina si los archivos de Razor (vistas y Razor Pages) se vuelven a compilar y actualizar si cambian los archivos en el disco.

El valor predeterminado es `true` para:

* Si la versión de compatibilidad de la aplicación se establece en <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> o una versión anterior
* Si la versión de compatibilidad de la aplicación se establece en <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> o una versión posterior, y la aplicación está en el entorno de desarrollo <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*>. En otras palabras, los archivos de Razor no se vuelven a compilar en el entorno que no es de desarrollo a menos que <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> se establezca de forma explícita.

Para obtener instrucciones y ejemplos de configuración de la versión de compatibilidad de la aplicación, consulte <xref:mvc/compatibility-version>.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

La compilación en tiempo de ejecución se habilita mediante el paquete `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation`. Para habilitar la compilación en tiempo de ejecución, las aplicaciones deben

* instalar el paquete NuGet [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/).
* En la aplicación, actualice `ConfigureServices` para incluir una llamada a `AddMvcRazorRuntimeCompilation`:

```csharp
services
    .AddMvc()
    .AddMvcRazorRuntimeCompilation()
```

Para que la compilación en tiempo de ejecución funcione cuando se implemente, las aplicaciones también deben modificar sus archivos de proyecto para establecer `PreserveCompilationReferences` en `true`.
[!code-xml[](view-compilation/sample/RuntimeCompilation.csproj?highlight=3)]

::: moniker-end

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
