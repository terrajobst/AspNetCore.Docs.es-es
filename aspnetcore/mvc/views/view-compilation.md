---
title: "Precompilación y compilación de vista razor"
author: rick-anderson
description: "Un documento de referencia que se explica cómo habilitar la compilación de la vista de MVC Razor y precompilación en aplicaciones de ASP.NET Core."
keywords: "Núcleo de ASP.NET, la compilación de vista Razor, Razor anterior a la compilación, precompilación de Razor"
ms.author: riande
manager: wpickett
ms.date: 12/13/2017
ms.topic: article
ms.assetid: ab4705b7-1638-1638-bc97-ea7f292fe92a
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 6839892c104673af0fd0fd074d368f3f42259d76
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2017
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a>Compilación de vista Razor y precompilación en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Vistas de Razor se compilan en tiempo de ejecución cuando se invoca la vista. ASP.NET principales 1.1.0 y versiones posteriores puede opcionalmente compilar vistas Razor e implementarlas con la aplicación&mdash;un proceso conocido como precompilación. Las plantillas de proyecto de ASP.NET Core 2.x habilitan precompilación de forma predeterminada.

> [!IMPORTANT]
> Precompilación de vista Razor no está disponible actualmente al realizar una [implementación independiente (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) de núcleo de ASP.NET 2.0. La característica estará disponible para DVL cuando vaya a versiones 2.1. Para obtener más información, consulte [se produce un error en la compilación de la vista cuando se compila entre para Linux en Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).

Consideraciones para la precompilación:

* Precompilar vistas da como resultado un menor publicado agrupación y el tiempo de inicio.
* No se puede editar archivos de Razor después de precompilar vistas. Las vistas editadas no estará presentes en el paquete publicado. 

Para implementar vistas precompiladas:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Si el proyecto tiene como destino .NET Framework, incluir una referencia de paquete a [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Si el proyecto tiene como destino .NET Core, no son necesarios cambios.

Las plantillas de proyecto de ASP.NET Core 2.x establece implícitamente `MvcRazorCompileOnPublish` a `true` de forma predeterminada, lo que significa que este nodo se puede quitar de forma segura desde el *.csproj* archivo. Si prefiere sea explícito, no hay peligro en configuración de la `MvcRazorCompileOnPublish` propiedad `true`. El siguiente *.csproj* ejemplo resalta esta configuración:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Establecer `MvcRazorCompileOnPublish` a `true`e incluir una referencia de paquete para `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. El siguiente *.csproj* ejemplo resalta estas opciones:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

Preparar la aplicación para una [implementación dependiente de framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) al ejecutar un comando como el siguiente en la raíz del proyecto:

```console
dotnet publish -c Release
```

Un *< Nombre_proyecto >. PrecompiledViews.dll* archivo, que contiene las vistas de Razor compiladas, se produce cuando se realiza correctamente de precompilación. Por ejemplo, la captura de pantalla siguiente muestra el contenido de *Index.cshtml* dentro de *WebApplication1.PrecompiledViews.dll*:

![Vistas de Razor dentro de DLL](view-compilation/_static/razor-views-in-dll.png)
