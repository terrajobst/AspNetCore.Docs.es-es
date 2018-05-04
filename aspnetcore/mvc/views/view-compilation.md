---
title: Precompilación y compilación de vistas de Razor en ASP.NET Core
author: rick-anderson
description: Aprenda a habilitar la compilación y precompilación de vistas de MVC Razor en aplicaciones de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 5d971645106a79497a9902063c7774dc6d546395
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
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

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
Si el proyecto tiene como destino .NET Framework, incluya una referencia de paquete a [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Si el proyecto tiene como destino .NET Core, no es necesario hacer cambios.

Las plantillas de proyecto de ASP.NET Core 2.x establecen implícitamente `MvcRazorCompileOnPublish` en `true` de forma predeterminada, lo que significa que este nodo se puede quitar de forma segura del archivo *.csproj*. Si prefiere ser explícito, no hay peligro en configurar la propiedad `MvcRazorCompileOnPublish` en `true`. El siguiente ejemplo de *.csproj* resalta esta configuración:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=5)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
Establezca `MvcRazorCompileOnPublish` en `true` e incluya una referencia de paquete a `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. El siguiente ejemplo de *.csproj* resalta estas opciones:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

Prepare la aplicación para una [implementación independiente de la plataforma](/dotnet/core/deploying/#framework-dependent-deployments-fdd) con el [comando de publicación de la CLI de .NET Core](/dotnet/core/tools/dotnet-publish). Por ejemplo, ejecute el siguiente comando en la raíz del proyecto:

```console
dotnet publish -c Release
```

Cuando se realiza correctamente la precompilación, se genera un archivo *<nombre_de_proyecto>.PrecompiledViews.dll* que contiene las vistas de Razor compiladas. Por ejemplo, la captura de pantalla de abajo muestra el contenido de *Index.cshtml* dentro de *WebApplication1.PrecompiledViews.dll*:

![Vistas de Razor dentro de DLL](view-compilation/_static/razor-views-in-dll.png)
