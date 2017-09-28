---
title: Desarrollar aplicaciones de ASP.NET Core con dotnet watch
author: rick-anderson
description: "Se muestra cómo usar dotnet watch."
keywords: ASP.NET Core, usar dotnet watch
ms.author: riande
manager: wpickett
ms.date: 03/09/2017
ms.topic: article
ms.assetid: 563ffb3f-d369-4aa5-bf0a-7300b4e7832c
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: 6a8943619e6174dbcd3d901b7bb736ba5d3af95d
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a>Desarrollar aplicaciones de ASP.NET Core con dotnet watch


Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Victor Hurdugaci](https://twitter.com/victorhurdugaci)

`dotnet watch` es una herramienta que ejecuta un comando `dotnet` cuando se modifican los archivos de código fuente. Por ejemplo, un cambio en un archivo puede desencadenar una compilación, pruebas o una implementación.

En este tutorial usaremos una aplicación de API web existente con dos puntos de conexión: uno que devuelva una suma y otro que devuelva un producto. El método del producto contiene un error que corregiremos en este mismo tutorial.

Descargue la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample). Contiene dos proyectos, `WebApp` (una aplicación web), y `WebAppTests` (pruebas unitarias para la aplicación web).

En una consola, vaya a la carpeta WebApp y ejecute los siguientes comandos:

- `dotnet restore`
- `dotnet run`

La salida de la consola mostrará mensajes similares al siguiente (indicando que la aplicación se ejecuta y espera solicitudes):

```console
$ dotnet run
Hosting environment: Production
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

En un explorador web, vaya a `http://localhost:5000/api/math/sum?a=4&b=5`. Ahí debería ver el resultado `9`.

Vaya a la API del producto (`http://localhost:5000/api/math/product?a=4&b=5`). Devolverá `9` y no `20`, tal como se esperaría. Lo corregiremos más adelante en el tutorial.

## <a name="add-dotnet-watch-to-a-project"></a>Agregar `dotnet watch` a un proyecto

- Agregue `Microsoft.DotNet.Watcher.Tools` al archivo *.csproj*:
 ```xml
 <ItemGroup>
   <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
 </ItemGroup> 
 ```

- Ejecute `dotnet restore`.

## <a name="running-dotnet-commands-using-dotnet-watch"></a>Ejecutar comandos `dotnet` mediante `dotnet watch`

Se puede ejecutar cualquier comando `dotnet` con `dotnet watch`. Por ejemplo:

| Comando | Comando con watch |
| ---- | ----- |
| dotnet run | dotnet watch run |
| dotnet run -f net451 | dotnet watch run -f net451 |
| dotnet run -f net451 -- --arg1 | dotnet watch run -f net451 -- --arg1 |
| dotnet test | dotnet watch test |

Ejecute `dotnet watch run` en la carpeta `WebApp`. La salida de la consola indicará que se ha iniciado `watch`.

## <a name="making-changes-with-dotnet-watch"></a>Efectuar cambios con `dotnet watch`

Asegúrese de que `dotnet watch` se está ejecutando.

Corrija el error en el método `Product` del `MathController` para que devuelva el producto y no la suma.

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

Guarde el archivo. La salida de la consola mostrará mensajes que indicarán que `dotnet watch` ha detectado un cambio de archivo y ha reiniciado la aplicación.

Compruebe que `http://localhost:5000/api/math/product?a=4&b=5` devuelve el resultado correcto.

## <a name="running-tests-using-dotnet-watch"></a>Ejecutar pruebas con `dotnet watch`

- Vuelva a cambiar el método `Product` del `MathController` para devolver la suma y guarde el archivo.
- En una ventana de comandos, vaya a la carpeta `WebAppTests`.
- Ejecute `dotnet restore`.
- Ejecute `dotnet watch test`. Verá la salida que indicará que se ha producido un error en una prueba y que watcher espera cambios de archivos:

 ```console
 Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
 Test Run Failed.
  ```
- Corrija el código del método `Product` para que devuelva el producto. Guarde el archivo.

`dotnet watch` detecta el cambio de archivo y vuelve a ejecutar las pruebas. La salida de la consola mostrará que se han superado las pruebas.

## <a name="dotnet-watch-in-github"></a>dotnet-watch en GitHub

dotnet-watch forma parte del [repositorio de DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools) de GitHub.

En la [sección MSBuild](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) del [archivo Léame de dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) se describe cómo se puede configurar dotnet-watch desde el archivo de proyecto de MSBuild que se está inspeccionando. El [archivo Léame de dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) contiene información de dotnet-watch que no se trata en este tutorial.
