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
ms.openlocfilehash: 013ca0d149c6415b5e6825aa5a48e93ae48f6728
ms.sourcegitcommit: 0063338c2e130409081bb60fcffa0c3f190cd46a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2018
---
# <a name="razor-file-cshtml-compilation-in-aspnet-core"></a>Compilación del archivo de Razor (.cshtml) en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Las vistas de Razor se compilan en tiempo de ejecución cuando se invoca la vista. En ASP.NET Core 2.1.0 y versiones posteriores, las vistas se compilan en tiempo de compilación y de publicación por medio del [SDK de Razor](/aspnetcore/mvc/razor-pages/sdk). En ASP.NET Core 1.1 y ASP.NET Core 2.0, las vistas se pueden compilar (si se quiere) en el tiempo de publicación e implementar con la aplicación usando la herramienta de precompilación. 



Consideraciones para la precompilación:

* Al precompilar vistas se obtiene como resultado un conjunto publicado más pequeño y un tiempo de inicio más rápido.
* No se pueden editar archivos de Razor después de precompilar vistas. Las vistas editadas no estarán presentes en el conjunto publicado. 

Para implementar vistas precompiladas:

# <a name="aspnet-core-21tabaspnetcore21"></a>[ASP.NET Core 2.1](#tab/aspnetcore21/)
En el SDK de Razor, la compilación en tiempo de compilación y de publicación de archivos de Razor está habilitada de forma predeterminada. Estos archivos de Razor se pueden editar en el tiempo de compilación, después de que se hayan actualizado. De forma predeterminada, con la aplicación solo se implementa el archivo *Views.dll* compilado, y no ningún archivo cshtml. 
    
> [!IMPORTANT]
> El SDK de Razor solo es efectivo cuando no se han establecido propiedades específicas de precompilación en el archivo de proyecto. Por ejemplo, si define `MvcRazorCompileOnPublish` en el archivo *.csproj*, el SDK de Razor se deshabilitará.

# <a name="aspnet-core-20tabaspnetcore20"></a>[ASP.NET Core 2.0](#tab/aspnetcore20/)

Si el proyecto tiene como destino .NET Framework, incluya una referencia de paquete a [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Si el proyecto tiene como destino .NET Core, no es necesario hacer cambios.

Las plantillas de proyecto de ASP.NET Core 2.x establecen `MvcRazorCompileOnPublish` en `true` implícitamente y de forma predeterminada, lo que significa que este nodo se puede quitar de forma segura del archivo *.csproj*.
    
> [!IMPORTANT]
> La precompilación de vistas de Razor no está disponible al realizar una [implementación independiente (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) en ASP.NET Core 2.0. 

Prepare la aplicación para una [implementación independiente de la plataforma](/dotnet/core/deploying/#framework-dependent-deployments-fdd) con el [comando de publicación de la CLI de .NET Core](/dotnet/core/tools/dotnet-publish). Por ejemplo, ejecute el siguiente comando en la raíz del proyecto:

```console
dotnet publish -c Release
```

Cuando se realiza correctamente la precompilación, se genera un archivo *<nombre_de_proyecto>.PrecompiledViews.dll* que contiene las vistas de Razor compiladas. Por ejemplo, la captura de pantalla de abajo muestra el contenido de *Index.cshtml* dentro de *WebApplication1.PrecompiledViews.dll*:

![Vistas de Razor dentro de DLL](view-compilation/_static/razor-views-in-dll.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Establezca `MvcRazorCompileOnPublish` en `true` e incluya una referencia de paquete a `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. El siguiente ejemplo de *.csproj* resalta estas opciones:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

