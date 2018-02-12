---
title: "Precompilación y compilación de vistas de Razor"
author: rick-anderson
description: "Documento de referencia que explica cómo habilitar la precompilación y la compilación de vistas de MVC Razor en aplicaciones ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: bd3f4470035b0375fc79aa7caa73b60ba6fc4f53
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a>Precompilación y compilación de vistas de Razor en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Las vistas de Razor se compilan en tiempo de ejecución cuando se invoca la vista. ASP.NET Core 1.1.0 y versiones posteriores pueden, opcionalmente, compilar las vistas de Razor e implementarlas con la aplicación (un proceso conocido como precompilación). Las plantillas de proyecto de ASP.NET Core 2.x habilitan la precompilación de forma predeterminada.

> [!IMPORTANT]
> La precompilación de vistas de Razor no está disponible actualmente al realizar una [implementación independiente (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) en ASP.NET Core 2.0. La característica estará disponible para las implementaciones independientes en la versión 2.1. Para más información, vea [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102) (Error de compilación de vistas al hacer varias compilaciones para Linux en Windows).

Consideraciones para la precompilación:

* Al precompilar vistas se obtiene como resultado un conjunto publicado más pequeño y un tiempo de inicio más rápido.
* No se pueden editar archivos de Razor después de precompilar vistas. Las vistas editadas no estarán presentes en el conjunto publicado. 

Para implementar vistas precompiladas:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Si el proyecto tiene como destino .NET Framework, incluya una referencia de paquete a [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Si el proyecto tiene como destino .NET Core, no es necesario hacer cambios.

Las plantillas de proyecto de ASP.NET Core 2.x establecen implícitamente `MvcRazorCompileOnPublish` en `true` de forma predeterminada, lo que significa que este nodo se puede quitar de forma segura del archivo *.csproj*. Si prefiere ser explícito, no hay peligro en configurar la propiedad `MvcRazorCompileOnPublish` en `true`. El siguiente ejemplo de *.csproj* resalta esta configuración:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Establezca `MvcRazorCompileOnPublish` en `true` e incluya una referencia de paquete a `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. El siguiente ejemplo de *.csproj* resalta estas opciones:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

Prepare la aplicación para una [implementación dependiente de marco](/dotnet/core/deploying/#framework-dependent-deployments-fdd) mediante la ejecución de un comando como el siguiente en la raíz del proyecto:

```console
dotnet publish -c Release
```

Cuando se realiza correctamente la precompilación, se genera un archivo *<nombre_de_proyecto>.PrecompiledViews.dll* que contiene las vistas de Razor compiladas. Por ejemplo, la captura de pantalla de abajo muestra el contenido de *Index.cshtml* dentro de *WebApplication1.PrecompiledViews.dll*:

![Vistas de Razor dentro de DLL](view-compilation/_static/razor-views-in-dll.png)
